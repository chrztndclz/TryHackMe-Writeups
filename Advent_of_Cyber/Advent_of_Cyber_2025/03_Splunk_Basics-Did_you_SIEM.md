## Introduction

---
#### The Story

It’s almost Christmas in Wareville, and the team of The Best Festival Company (TBFC) is busy preparing for the big celebration. Everything is running smoothly until the SOC dashboard flashes red. A ransom message suddenly appears:

The message comes from King Malhare, the jealous ruler of HopSec Island, who’s tired of Easter being forgotten. He’s sent his Bandit Bunnies to attack TBFC’s systems and turn Christmas into his new holiday, EAST-mas.

With McSkidy missing and the network under attack, the TBFC SOC team will utilize Splunk to determine how the ransomware infiltrated the system and prevent King Malhare’s plan from being compromised before Christmas.

#### Learning Objectives

- Ingest and interpret custom log data in Splunk
- Create and apply custom field extractions
- Use Search Processing Language (SPL) to filter and refine search results
- Conduct an investigation within Splunk to uncover key insights

---

## Walkthrough

Start your VM by clicking the Start Machine button below. The machine will need about 2 -3 minutes to fully boot. Once the machine is up and running, you can connect to the Splunk SIEM by visiting https://LAB_WEB_URL.p.thmlabs.com in your browser.

<img width="1869" height="555" alt="image" src="https://github.com/user-attachments/assets/98c5f118-f29d-4c40-8318-0d62d3d362b0" />


<img width="1866" height="617" alt="image" src="https://github.com/user-attachments/assets/0e86116a-137d-4478-a8b3-e7cc71921d63" />


<img width="1852" height="887" alt="image" src="https://github.com/user-attachments/assets/31e6137e-753e-4139-bd16-7fc759154939" />


The local IP assigned to the web server is 10.10.1.15.

---

**01 Initial Triage**

Run a broad search to view all HTTP activity:

`index=main sourcetype=web_traffic`

<img width="1844" height="888" alt="image" src="https://github.com/user-attachments/assets/fc16a7c2-6242-4d81-b5bd-33ae0510cd9e" />

---

**02 Visualizing the Logs Timeline**

Timechart showing daily log volume:

`index=main sourcetype=web_traffic | timechart span=1d count`

<img width="1841" height="743" alt="image" src="https://github.com/user-attachments/assets/513cf886-2dd0-4111-a1fc-2f8eea198915" />

<img width="1847" height="910" alt="image" src="https://github.com/user-attachments/assets/9d9675bb-1973-4f19-90bb-444ecd64b260" />

Sorting by highest traffic:

`index=main sourcetype=web_traffic | timechart span=1d count | sort by count | reverse`

<img width="1847" height="899" alt="image" src="https://github.com/user-attachments/assets/8ee10080-47f2-4740-8aee-f0bcb9e662a2" />


---

**03 Anomaly Detection**

**Unusual User Agents**

Inspect user agents:
<img width="1846" height="763" alt="image" src="https://github.com/user-attachments/assets/21075ae2-80bd-4c00-9b14-dd21e05369f7" />

**Suspicious Client IPs**

Check frequency of IPs using non-browser user agents:
<img width="1846" height="831" alt="image" src="https://github.com/user-attachments/assets/a0c4d317-6308-4c6b-810c-8d6b4dce75d8" />
**Identified Attacker:**  
**198.51.100.55**


**Suspicious Paths**

Review abnormal access patterns:
<img width="1849" height="872" alt="image" src="https://github.com/user-attachments/assets/ad321c51-7587-45a5-8a3a-2f97a2dd0b22" />


---

**04 Filtering out Benign Values**

`index=main sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox*`

<img width="1852" height="880" alt="image" src="https://github.com/user-attachments/assets/40498432-dceb-4c29-8dc9-907ac1f12235" />

---

**05 Narrowing Down Suspicious IPs**

`sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox* | stats count by client_ip | sort -count | head 5`

<img width="1867" height="496" alt="image" src="https://github.com/user-attachments/assets/368a403c-8f2a-4f8f-81ca-bb5ab50ffafc" />

IP: 198.51.100.55

---

**06 Tracing the Attack Chain**

#### Reconnaissance (Footprinting)

Look for attempts to access sensitive system files:

`sourcetype=web_traffic client_ip="REDACTED" AND path IN ("/.env", "/*phpinfo*", "/.git*") | table _time, path, user_agent, status`

<img width="1851" height="901" alt="image" src="https://github.com/user-attachments/assets/7732b027-6e95-4b53-99a6-c862cd48bd6c" />

**Enumeration (Vulnerability Testing)**

Detect traversal and redirection tests:

`sourcetype=web_traffic client_ip="REDACTED" AND path="*..*" OR path="*redirect*"`

<img width="1845" height="895" alt="image" src="https://github.com/user-attachments/assets/f835a7ea-49c0-4241-a198-e66ca18f705b" />

Count traversal attempts:

`sourcetype=web_traffic client_ip="REDACTED" AND path="*..\/..\/*" OR path="*redirect*" | stats count by path`

<img width="1866" height="528" alt="image" src="https://github.com/user-attachments/assets/6c4d6958-6285-42dd-b4dd-30d86c45b7cb" />

**SQL Injection Attack**

Identify automated SQL injection tools:

`sourcetype=web_traffic client_ip="REDACTED" AND user_agent IN ("*sqlmap*", "*Havij*") | table _time, path, status`

<img width="1845" height="886" alt="image" src="https://github.com/user-attachments/assets/affc85af-1952-497b-ac24-7c167922da07" />

---

**07 Exfiltration Attempts**

Check for attempts to steal archives:

`sourcetype=web_traffic client_ip="REDACTED" AND path IN ("*backup.zip*", "*logs.tar.gz*") | table _time path, user_agent`

---

**08 Ransomware Staging & RCE**

Look for ransomware payloads and RCE calls:

`sourcetype=web_traffic client_ip="REDACTED" AND path IN ("*bunnylock.bin*", "*shell.php?cmd=*") | table _time, path, user_agent, status`

<img width="1840" height="757" alt="image" src="https://github.com/user-attachments/assets/ce8c3fa1-a65d-47f4-8cba-be18f7358510" />

---

**09 Correlate Outbound C2 Communication**

Correlate outbound traffic from the compromised server:

`sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="REDACTED" AND action="ALLOWED" | table _time, action, protocol, src_ip, dest_ip, dest_port, reason`

<img width="1849" height="903" alt="image" src="https://github.com/user-attachments/assets/5faf1631-a7a4-41f3-b12c-e31b6c705584" />

---

**10 Volume of Data Exfiltrated**

Sum all bytes transferred to the attacker's C2:

`sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="REDACTED" AND action="ALLOWED" | stats sum(bytes_transferred) by src_ip`

<img width="1865" height="696" alt="image" src="https://github.com/user-attachments/assets/3669e14c-ec59-46d1-be9c-f5dab4de1d84" />

---

## Objectives

What is the attacker IP found attacking and compromising the web server?

`198.51.100.55`

**05 Narrowing Down Suspicious IPs**

`sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox* | stats count by client_ip | sort -count | head 5`

---

Which day was the peak traffic in the logs? (Format: YYYY-MM-DD)

`2025-10-12`

**02 Visualizing the Logs Timeline**

`index=main sourcetype=web_traffic | timechart span=1d count`

<img width="1847" height="775" alt="image" src="https://github.com/user-attachments/assets/3e0bdf38-3b09-450a-9152-36c8e19e0d2c" />

or 

Sorting by highest traffic:

`index=main sourcetype=web_traffic | timechart span=1d count | sort by count | reverse`

<img width="1865" height="813" alt="image" src="https://github.com/user-attachments/assets/f2470ebb-0e60-4912-8ddc-e3769be199ff" />

---

What is the count of Havij user_agent events found in the logs?

`993`

Anomaly Detection

user_agent

<img width="1842" height="877" alt="image" src="https://github.com/user-attachments/assets/e620b9d8-a713-4d3c-8a1e-14a83e566c9a" />

---

How many path traversal attempts to access sensitive files on the server were observed?

`658`

**03 Anomaly Detection**

Suspicious Paths

<img width="1841" height="889" alt="image" src="https://github.com/user-attachments/assets/ab67f837-5821-40f9-a5f4-429df30a8a08" />

---

Examine the firewall logs. How many bytes were transferred to the C2 server IP from the compromised web server?

`126167`

**10 Volume of Data Exfiltrated**

`sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="198.51.100.55" AND action="ALLOWED" | stats sum(bytes_transferred) by src_ip`

<img width="1865" height="531" alt="image" src="https://github.com/user-attachments/assets/cd92d942-5967-4986-a340-8aa7f90c649c" />

---
## Key Takeaways

- **Single attacker IP** generated nearly all malicious activity

- **Reconnaissance → Enumeration → Exploitation → Exfiltration → C2** fully visible in logs

- **SQL injection**, **path traversal**, and **webshell upload** used to gain RCE

- **bunnylock.bin** ransomware executed via remote command

- **Firewall logs confirm** outbound C2 communication

- **Exfiltrated data volume** was significant

- Shows the importance of **log correlation**, not just isolated events


---

## Reflection

This challenge demonstrates how a structured, layered investigation in Splunk allows analysts to uncover not just isolated malicious events, but the full narrative of an attack. By correlating web server logs with firewall data, we reconstructed the attacker’s entire workflow — from their initial reconnaissance using scripted tools, to exploiting vulnerabilities, installing a webshell, deploying ransomware, and finally establishing a command-and-control channel for data exfiltration. The exercise highlights the importance of understanding normal traffic patterns so anomalies stand out quickly, as well as the value of field-based filtering and SPL queries to progressively narrow the investigation. Ultimately, Splunk’s visibility across log sources is what enabled the TBFC SOC team to expose King Malhare’s plot and secure their systems in time to save Christmas.
