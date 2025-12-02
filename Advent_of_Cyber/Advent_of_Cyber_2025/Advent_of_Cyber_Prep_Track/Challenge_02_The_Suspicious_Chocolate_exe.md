## The Suspicious Chocolate.exe

---

## Description

A shiny USB labelled “SOCMAS Party Playlist” appears on your desk. Inside is a mysterious file called chocolate.exe.
It looks festive, but who sent it?

In this challenge, you’ll scan the file using a simulated VirusTotal tool to decide whether it’s safe or malicious.
Checking suspicious files is a crucial skill for every defender.

Objective:
Determine if chocolate.exe is safe or infected.

Steps:

Click the “Scan” Button.

Review the scan report (49 clean results, 1 malicious).

Decide correctly whether the file is safe or dangerous.

---

## Analysis

This challenge simulates checking a suspicious USB file using a VirusTotal-style scanner. After scanning chocolate.exe, the report shows one malicious detection out of fifty. Even a single positive hit is enough to treat the file as unsafe, as malware can evade multiple engines. The correct conclusion is that the file is malicious.

---

## Methodology

Step 1: Scan the File 

<img width="910" height="722" alt="image" src="https://github.com/user-attachments/assets/dab13651-fd35-4f12-88a9-187504e72ab9" />

Step 2: Choose Conclusion 

Choose `Malicious`

<img width="889" height="769" alt="image" src="https://github.com/user-attachments/assets/c4bc91af-fab3-4cf1-92bd-385de55ceaff" />

Even the other's are safe even only one is malicious it could affect it all 

Step 3: Submit and Retrieve the Flag


<img width="885" height="370" alt="image" src="https://github.com/user-attachments/assets/4d9a177d-ca4c-4f9c-9722-46f2ed6b2efe" />


---

## Flag

THM{Not----------eet}

---

## Reflection 

This task highlights the importance of caution when dealing with unknown executables. A single malware detection should never be ignored, especially when files come from untrusted sources like USB drives. Relying on multi-engine scans and treating any positive result seriously helps prevent potential infections.

---

## Tools

(None)



