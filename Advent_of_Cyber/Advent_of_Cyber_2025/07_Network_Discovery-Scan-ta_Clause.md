## Introduction

---

#### The Story

Task banner for day 7
Christmas preparations are delayed - HopSec has breached our QA environment and locked us out! Without it, the TBFC projects can't be tested, and our entire SOC-mas pipeline is frozen. To make things worse, the server is slowly transforming into a twisted EAST-mas node.

Can you uncover HopSec's trail, find a way back into tbfc-devqa01, and restore the server before the bunny's takeover is complete? For this task, you'll need to check every place to hide, every opened port that bunnies left unprotected. Good luck!

Three bad bunnies with three hidden easter eggs occupy the breached server in a Christmas theme.

#### Learning Objectives

Learn the basics of network service discovery with Nmap
Learn core network protocols and concepts along the way
Apply your knowledge to find a way back into the server

---

## Walkthrough 

**01 The Simplest Port Scan**

Start by identifying your target: the tbfc-devqa01 server at 10.49.136.141.
Perform a basic Nmap scan to look for commonly used ports:

`nmap 10.49.136.141`

The scan reveals two open ports:

- 22/tcp – SSH

- 80/tcp – HTTP

You can attempt SSH access if credentials are known, or open the web page at http://10.49.136.141
, where you’ll see the defaced website left by the bad bunnies.


**02 Scanning Whole Range**

The first scan only checked the top 1000 ports. Hidden services might be running elsewhere, so perform a full-range scan:

`nmap -p- --script=banner 10.49.136.141`

New open ports appear:

- 21212/tcp – FTP (vsFTPd 3.0.5)

- 25251/tcp – TBFC maintd custom app

Connect to the FTP service and try anonymous login:

`ftp 10.49.136.141 21212`


List the files and download tbfc_qa_key1 — this is KEY 1.
Use get tbfc_qa_key1 - to print it on screen.
Exit the FTP client afterward.

**03 Port Scan Modes**

Next, check the custom TBFC service on port 25251 using Netcat:

`nc -v 10.49.136.141 25251`


You’ll see:

TBFC maintd v0.2
Type HELP for commands.


Run the proper command:

`GET KEY`

This returns KEY 2.
Use CTRL+C to exit Netcat.


**04 TCP and UDP Ports**

So far, only TCP ports were scanned. UDP may hide additional services.
Run a UDP scan:

`nmap -sU 10.49.136.141`


You will see 53/udp – DNS open.

Query the DNS service for the third key:

`dig @10.49.136.141 TXT key3.tbfc.local +short`


This returns KEY 3.


**05 On-Host Service Discovery**

Now you have all three keys.
Combine them inside the web panel at:

`http://10.49.136.141`


Submitting the combined key gives access to the Secret Admin Console, where you can now run commands directly on the server.


**06 Listing Listening Ports**

Inside the admin console, enumerate all open ports:

ss -tunlp

```
This shows all listening services, including:

SSH (22)

HTTP (80)

FTP (21212)

TBFC maintd (25251)

DNS (53)

Localhost-only services like:

3306 – MySQL

8000

7681
```

Since you're inside the machine, you can access MySQL without a password:

`mysql -D tbfcqa01 -e "show tables;"`
`mysql -D tbfcqa01 -e "select * from flags;"`


This reveals the final QA server flag.

---

## Objective 

IP: 10.49.162.132

What evil message do you see on top of the website?

Navigate website 

<img width="1863" height="826" alt="image" src="https://github.com/user-attachments/assets/4f43533d-f032-44bd-9e3b-0b92f8fbbc14" />

`Pwned by HosSec`

---

What is the first key part found on the FTP server?

<img width="1002" height="656" alt="image" src="https://github.com/user-attachments/assets/84bb6ece-4b78-479e-ad7b-5e16de433f0f" />

`3aster_`

---

What is the second key part found in the TBFC app?

nc -v 10.49.162.132 25251

<img width="1008" height="646" alt="image" src="https://github.com/user-attachments/assets/bd007ba7-4229-47ac-a38f-9ea99e695bba" />

`15_th3_`

---

What is the third key part found in the DNS records?

nmap -sU 10.49.162.132

dig @10.49.162.132 TXT key3.tbfc.local +short

<img width="1020" height="662" alt="image" src="https://github.com/user-attachments/assets/4da40794-a088-4460-a6e9-61ff3935adcf" />

`n3w_xm45`

---

Which port was the MySQL database running on?

Login to the website: http://10.49.136.141

using the key: 

`3aster_15_th3_n3w_xm45`

<img width="1909" height="841" alt="image" src="https://github.com/user-attachments/assets/b793e9e5-7b03-4cf3-a202-45f6a61789cb" />

List open ports using:
`ss -tunlp` 

<img width="1863" height="768" alt="image" src="https://github.com/user-attachments/assets/15900d8d-ab5d-4d96-8dfd-d06ffddac6a1" />


`3306`

---

Finally, what's the flag you found in the database?

See data base content: 

Show Table
`mysql -D tbfcqa01 -e "show tables;"`

Select the Flag
`mysql -D tbfcqa01 -e "select * from flags;"`

<img width="1861" height="774" alt="image" src="https://github.com/user-attachments/assets/7908180c-d5bf-441e-ae74-f1faa9e2afae" />

---

##Key Takeaways

- Incremental scanning matters. Starting with a basic Nmap scan revealed only two ports, but expanding to full-range and UDP scans uncovered hidden services critical to regaining access.

- Different protocols reveal different clues. FTP exposed the first key, a custom TCP service provided the second, and a DNS TXT record hidden over UDP yielded the third.

- Service enumeration is essential. Using tools like nc, dig, and Nmap scripts provided deeper insight into how the attacker left access points on the server.

- Privilege comes from chaining small discoveries. The three key parts from separate services had to be combined to unlock the compromised web panel and escalate into the admin console.

- On-host enumeration finishes the job. Once inside, commands like ss -tunlp and direct MySQL queries made it possible to locate backend services, inspect the database, and retrieve the final flag.

- Security lesson: Unmonitored services, weak configurations, and publicly accessible ports—even obscure ones—create multiple attack vectors. Always validate the entire attack surface.

---

## Reflection

This challenge emphasized the importance of thorough network enumeration and understanding how attackers may leave behind unconventional access points. Each step reflected a realistic workflow: start with surface-level reconnaissance, expand to deeper probing, identify unusual ports, and validate both TCP and UDP pathways.

Working through the exercise reinforced how critical it is not to rely solely on default Nmap scans—hidden services often sit outside common port ranges. Discovering keys from different protocols also highlighted how multi-vector analysis leads to a complete picture of a breach.

Finally, accessing the admin panel and enumerating internal services showed the value of post-exploitation techniques. Being able to trace the attacker’s footprint across the system demonstrated how a SOC analyst connects clues from different layers to regain control of a compromised environment.

Overall, this task strengthened practical skills in scanning, enumeration, protocol analysis, and on-host investigation—core capabilities for both security monitoring and incident response.
