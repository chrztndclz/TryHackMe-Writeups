## Probably Just Fine

Welcome to your first shift! You are greeted by an internal alert on the SOC dashboard titled "Unusual VPN login of susan.martin@probablyfine.thm from 37.19.201.132 (Singapore)."

The SOC handover notes did indeed mention that Susan from Marketing is in Singapore, attending a security vendor conference. It is probably just fine, but the SOC procedure tells us to verify each IP in our threat intel platform TryDetectThis. Answer the first **two** questions to gather more information and determine the threat level.

---

## TryDetectThis

TryDetectThis is a threat intelligence database to check the reputation and other details of IP addresses, domains, and file hashes. To access this platform, please navigate to the following URL in your own browser:

Access - Granted
URL - https://static-labs.tryhackme.cloud/apps/trydetectthis/

---

## Is It Really Fine

That login IP looks suspicious, doesn't it? Your teammates reached out to Susan, and she confirmed she did not log in to the company VPN. She also mentioned that while using a public Wi-Fi hotspot at a cafe, she was suddenly prompted to install a "security check" tool, which she did. The host telemetry reveals a suspicious binary with the hash **b8e02f2bc0ffb42e8cf28e37a26d8d825f639079bf6d948f8debab6440ee5630**. Can you help us figure out what this binary exactly does and answer the remaining questions?

---

Paste the IP Address (37.19.201.132) to TryDetectThis website. 

What is the ASN number related to the IP?

(TryDetectThis - Website)

<img width="919" height="502" alt="image" src="https://github.com/user-attachments/assets/201b0061-6ef4-4b9d-9af2-a301f63f6a76" />

`Answer: 212238`

---

Which service is offered from this IP?

(TryDetectThis - Website)

<img width="1444" height="78" alt="image" src="https://github.com/user-attachments/assets/036bd3f6-6ae6-4921-9475-dfd7f2357ccd" />

`Answer: vpn`

---

Paste the hash (b8e02f2bc0ffb42e8cf28e37a26d8d825f639079bf6d948f8debab6440ee5630) to VirusTotal website.

What is the filename of the file related to the hash?

<img width="850" height="231" alt="image" src="https://github.com/user-attachments/assets/cba94973-8791-4589-8b41-bbba71602065" />

`Answer: zY9sqWs.exe`

---

What is the threat signature that Microsoft assigned to the file?

<img width="740" height="83" alt="image" src="https://github.com/user-attachments/assets/c86f49d2-068e-431c-af23-d615300e1639" />

`Answer: Trojan:Win32/LummaStealer.PM!MTB`

---

One of the contacted domains is part of a large malicious infrastructure cluster.
Based on its HTTPS certificate, how many domains are linked to the same campaign?

<img width="1634" height="730" alt="image" src="https://github.com/user-attachments/assets/85df7fda-69d2-48e2-9d6b-2dc14921629d" />


<img width="1492" height="647" alt="image" src="https://github.com/user-attachments/assets/76732cbf-f036-4f9c-9306-a704d59e64e3" />

.cyou domains - 50 domains

.icu domains - 45 domains

.sbs domains - 56 domains

`Answer: 151`

---

Paste the hash (b8e02f2bc0ffb42e8cf28e37a26d8d825f639079bf6d948f8debab6440ee5630) to TryDetectThis website. 

The file matches one of the YARA rules made by "kevoreilly".
What line is present in the rule's "condition" field?

<img width="891" height="546" alt="image" src="https://github.com/user-attachments/assets/4e3e394e-29bc-422c-8272-d9651c3439b1" />

<img width="1039" height="561" alt="image" src="https://github.com/user-attachments/assets/33097fdb-8ac1-49e3-8e93-6935107601dc" />

`Answer: uint16(0) == 0x5a4d and any of them`

---

The file is also mentioned in one of the TI reports.
What is the title of the report mentioning this hash?


<img width="749" height="437" alt="image" src="https://github.com/user-attachments/assets/01a36a23-7922-4c61-b9b7-3b43f3c06076" />

`Answer: Behind the Curtain: How Lumma Affiliates Operate`

---

Read the TI report titled "Behind the Curtain: How Lumma Affiliates Operate" from this link https://www.recordedfuture.com/research/behind-the-curtain-how-lumma-affiliates-operate

Which team did the author of the malware start collaborating with in early 2024?


<img width="1430" height="636" alt="image" src="https://github.com/user-attachments/assets/703661c6-5cb3-472c-9a90-bef7d0742570" />

`Answer: GhostSocks`

---

A Mexican-based affiliate related to the malware family also uses other infostealers.
Which mentioned infostealer targets Android systems?

<img width="1421" height="529" alt="image" src="https://github.com/user-attachments/assets/452f505d-239f-4c51-9b74-d7f5af319f4b" />

`Answer: CraxsRAT`

---

The report states that the affiliates behind the malware use the services of AnonRDP.
Which MITRE ATT&CK sub-technique does this align with?

<img width="931" height="476" alt="image" src="https://github.com/user-attachments/assets/8a947ba8-97df-40b0-98c8-7e3f3fbc1070" />

AnonRDP falls under T1583.003 (Acquire Infrastructure: Virtual Private Server) because it provides anonymous VPS/RDP infrastructure that threat actors can easily obtain and use to host, control, and operate malicious activities without attribution or takedown risk.

`Answer: T1583.003`
