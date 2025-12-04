Start your VM by clicking the Start Machine button below. The machine will need about 2 -3 minutes to fully boot. Once the machine is up and running, you can connect to the Splunk SIEM by visiting https://LAB_WEB_URL.p.thmlabs.com in your browser.

<img width="1869" height="555" alt="image" src="https://github.com/user-attachments/assets/98c5f118-f29d-4c40-8318-0d62d3d362b0" />


<img width="1866" height="617" alt="image" src="https://github.com/user-attachments/assets/0e86116a-137d-4478-a8b3-e7cc71921d63" />


<img width="1852" height="887" alt="image" src="https://github.com/user-attachments/assets/31e6137e-753e-4139-bd16-7fc759154939" />


The local IP assigned to the web server is 10.10.1.15.

---

## Initial Triage

index=main sourcetype=web_traffic

<img width="1844" height="888" alt="image" src="https://github.com/user-attachments/assets/fc16a7c2-6242-4d81-b5bd-33ae0510cd9e" />

---

## Visualizing the Logs Timeline

index=main sourcetype=web_traffic | timechart span=1d count

<img width="1841" height="743" alt="image" src="https://github.com/user-attachments/assets/513cf886-2dd0-4111-a1fc-2f8eea198915" />

<img width="1847" height="910" alt="image" src="https://github.com/user-attachments/assets/9d9675bb-1973-4f19-90bb-444ecd64b260" />

-reverse

index=main sourcetype=web_traffic | timechart span=1d count | sort by count | reverse

<img width="1847" height="899" alt="image" src="https://github.com/user-attachments/assets/8ee10080-47f2-4740-8aee-f0bcb9e662a2" />


---

## Anomaly Detection

user agent
<img width="1846" height="763" alt="image" src="https://github.com/user-attachments/assets/21075ae2-80bd-4c00-9b14-dd21e05369f7" />

client ip
<img width="1846" height="831" alt="image" src="https://github.com/user-attachments/assets/a0c4d317-6308-4c6b-810c-8d6b4dce75d8" />

path
<img width="1849" height="872" alt="image" src="https://github.com/user-attachments/assets/ad321c51-7587-45a5-8a3a-2f97a2dd0b22" />


---

## Filtering out Benign Values

index=main sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox*

<img width="1852" height="880" alt="image" src="https://github.com/user-attachments/assets/40498432-dceb-4c29-8dc9-907ac1f12235" />

---

## Narrowing Down Suspicious IPs

sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox* | stats count by client_ip | sort -count | head 5

<img width="1867" height="496" alt="image" src="https://github.com/user-attachments/assets/368a403c-8f2a-4f8f-81ca-bb5ab50ffafc" />

IP: 198.51.100.55

---

## Tracing the Attack Chain

#### Reconnaissance (Footprinting)

sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("/.env", "/*phpinfo*", "/.git*") | table _time, path, user_agent, status

<img width="1851" height="901" alt="image" src="https://github.com/user-attachments/assets/7732b027-6e95-4b53-99a6-c862cd48bd6c" />


#### Enumeration (Vulnerability Testing)

sourcetype=web_traffic client_ip="<REDACTED>" AND path="*..*" OR path="*redirect*"

<img width="1845" height="895" alt="image" src="https://github.com/user-attachments/assets/f835a7ea-49c0-4241-a198-e66ca18f705b" />

sourcetype=web_traffic client_ip="<REDACTED>" AND path="*..\/..\/*" OR path="*redirect*" | stats count by path

<img width="1866" height="528" alt="image" src="https://github.com/user-attachments/assets/6c4d6958-6285-42dd-b4dd-30d86c45b7cb" />

#### SQL Injection Attack

sourcetype=web_traffic client_ip="<REDACTED>" AND user_agent IN ("*sqlmap*", "*Havij*") | table _time, path, status

<img width="1845" height="886" alt="image" src="https://github.com/user-attachments/assets/affc85af-1952-497b-ac24-7c167922da07" />

---

## Exfiltration Attempts

sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("*backup.zip*", "*logs.tar.gz*") | table _time path, user_agent

---

## Ransomware Staging & RCE

sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("*bunnylock.bin*", "*shell.php?cmd=*") | table _time, path, user_agent, status

<img width="1840" height="757" alt="image" src="https://github.com/user-attachments/assets/ce8c3fa1-a65d-47f4-8cba-be18f7358510" />

---

## Correlate Outbound C2 Communication

sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="<REDACTED>" AND action="ALLOWED" | table _time, action, protocol, src_ip, dest_ip, dest_port, reason

<img width="1849" height="903" alt="image" src="https://github.com/user-attachments/assets/5faf1631-a7a4-41f3-b12c-e31b6c705584" />

---

Volume of Data Exfiltrated

sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="<REDACTED>" AND action="ALLOWED" | stats sum(bytes_transferred) by src_ip

<img width="1865" height="696" alt="image" src="https://github.com/user-attachments/assets/3669e14c-ec59-46d1-be9c-f5dab4de1d84" />


-----

Answer the questions below

---

What is the attacker IP found attacking and compromising the web server?

198.51.100.55

Narrowing IP
sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox* | stats count by client_ip | sort -count | head 5

---

Which day was the peak traffic in the logs? (Format: YYYY-MM-DD)

Oct 12, 2025
2025-10-12

Visualizing the Logs Timeline
index=main sourcetype=web_traffic | timechart span=1d count

<img width="1847" height="775" alt="image" src="https://github.com/user-attachments/assets/3e0bdf38-3b09-450a-9152-36c8e19e0d2c" />

or 

reverse
index=main sourcetype=web_traffic | timechart span=1d count | sort by count | reverse

<img width="1865" height="813" alt="image" src="https://github.com/user-attachments/assets/f2470ebb-0e60-4912-8ddc-e3769be199ff" />

---

What is the count of Havij user_agent events found in the logs?

993

Anomaly Detection
user_agent

<img width="1842" height="877" alt="image" src="https://github.com/user-attachments/assets/e620b9d8-a713-4d3c-8a1e-14a83e566c9a" />

---

How many path traversal attempts to access sensitive files on the server were observed?

658

Anomaly Detection
path

<img width="1841" height="889" alt="image" src="https://github.com/user-attachments/assets/ab67f837-5821-40f9-a5f4-429df30a8a08" />

---

Examine the firewall logs. How many bytes were transferred to the C2 server IP from the compromised web server?

126167

sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="198.51.100.55" AND action="ALLOWED" | stats sum(bytes_transferred) by src_ip

<img width="1865" height="531" alt="image" src="https://github.com/user-attachments/assets/cd92d942-5967-4986-a340-8aa7f90c649c" />
