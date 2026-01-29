Zero Tolerance

It was supposed to be a regular morning at ProbablyFine Ltd. L2 had just returned from paternity leave. L3 was hosting a live webinar on "Proactive Threat Hunting." The morning standup was the usual mix of coffee, ticket updates, and small talk about last night's football match. Then the Slack notification came through from Sales:

"NEW CLIENT ONBOARDED - VaultSecure Banking - Tier 1 Priority - Live monitoring starts NOW"

VaultSecure Banking wasn't just any client. They're a regional bank with two million customers. They had just fired their previous MSSP after a compliance audit revealed endpoints that had gone unmonitored for six months. The contract ProbablyFine signed was massive, enough to fund the company for the next two years. However, there’s a catch: a 90-day probation period with a "zero tolerance" clause - miss one critical alert, and the contract is terminated.
The Alert

You were barely skimming the onboarding docs before the SIEM lights up with a critical alert:

"CRITICAL: Suspicious Persistence Mechanism Detected - VaultSecure Banking"

You just stare at it for a second. It's been less than 4 hours since monitoring went live, and you're already staring at a critical alert. Your L2 is in back-to-back meetings with the new client. Your L3 is live on a webinar with 500 attendees. The company's future literally depends on how you handle this. For now, it's just you and this alert. It's time to show VaultSecure Banking why they chose ProbablyFine!


---

What is the hostname where the Initial Access occurred?

<img width="1065" height="694" alt="image" src="https://github.com/user-attachments/assets/b0f460f2-860b-435a-9853-321b71abbf53" />

Sysmon and Windows Event logs show the first malicious execution events originating from host JP-BROWN-WS, identifying it as the initial compromised endpoint.

`Answer: JP-BROWN-WS`

---

What MITRE subtechnique ID describes the initial code execution on the beachhead?

<img width="1914" height="698" alt="image" src="https://github.com/user-attachments/assets/d22a8386-9adb-43c1-a203-e57c6b50673d" />

The attacker relied on the user manually opening a malicious LNK file, which directly aligns with user-triggered execution of a malicious file.

`Answer: T1204.002`

---

What is the full path of the malicious file that led to Initial Access?

<img width="1913" height="562" alt="image" src="https://github.com/user-attachments/assets/13886303-f6b7-4260-95a8-58c72448c3f0" />

The file path shows a masqueraded LNK file placed in the user’s Downloads folder, which initiated malicious execution when opened.

`Answer: C:\Users\jp.brown\Downloads\TravisClart_Resume.pdf.lnk`

---

<img width="1916" height="540" alt="image" src="https://github.com/user-attachments/assets/a2e33812-e97e-40b6-b57f-3d27ddb7c4ee" />

```
Search:
index=* source="WinEventLog:Microsoft-Windows-Sysmon/Operational" host="JP-BROWN-WS" Image!="*splunk*"
| stats count by CommandLine
| sort 0 count
| head 50


Configure event dates:
11/14/25 5:04:42.000 AM to 1/29/26 4:00:42.000 PM)
```

What is the full path to the LOLBin abused by the attacker for Initial Access?

<img width="1913" height="714" alt="image" src="https://github.com/user-attachments/assets/e0379bc8-fbf0-4c5a-a92d-f5eb7754d1b6" />

The attacker abused mshta.exe, a trusted Windows LOLBin, to execute attacker-controlled HTA/script content while bypassing application allowlisting.

`Answer: C:\Windows\System32\mshta.exe`

---

What is the IP address of the attacker's Command & Control server?

<img width="1900" height="696" alt="image" src="https://github.com/user-attachments/assets/a234db26-ae81-44bd-b149-d06cf2baac93" />

The command shows mshta.exe retrieving a malicious HTA payload over HTTP, and the remote host serving that payload (10.10.14.174) is therefore the attacker’s Command & Control server.

`Answer: 10.10.14.174`

---

What is the full path of the process responsible for the C2 beaconing?

<img width="1907" height="695" alt="image" src="https://github.com/user-attachments/assets/31afc779-7425-4223-9ec5-b6a24ab3401e" />

The registry Run key shows RuntimeBroker.exe was deliberately set for persistence, indicating it is the malicious process launched on startup and responsible for ongoing C2 beaconing.

`Answer: C:\Windows\Temp\RuntimeBroker.exe`

---

What is the full path, modified for Persistence on the beachhead host?

<img width="1916" height="652" alt="image" src="https://github.com/user-attachments/assets/34dd3cd9-6b7b-48a9-9551-54ba18a7755b" />

he command explicitly creates the SystemMonitor value under the HKCU Run key, establishing user-level persistence by launching the malicious executable at logon.

`Answer: HKCU\Software\Microsoft\Windows\CurrentVersion\Run\SystemMonitor`

---

What tool and parameter did the threat actor use for credential dumping?

<img width="1895" height="736" alt="image" src="https://github.com/user-attachments/assets/55b10b80-1dd9-452f-af98-c3be50fdefbb" />

The attacker used Invoke-Mimikatz with the -DumpCreds switch to perform in‑memory credential dumping via PowerShell.

`Answer: Invoke-Mimikatz -DumpCreds`

---

The threat actor executed a command to evade defenses.
What security parameter did they attempt to change?

<img width="1917" height="413" alt="image" src="https://github.com/user-attachments/assets/c05540d1-d892-4061-82dc-967016197761" />

The attacker tried to evade detection by disabling Microsoft Defender real-time monitoring using the DisableRealtimeMonitoring security parameter.

`Answer: DisableRealtimeMonitoring`

---

The threat actor used a tool to execute remote commands on other machines.
What is the process ID (PID) that executed the remote command?

<img width="1910" height="728" alt="image" src="https://github.com/user-attachments/assets/498132af-b530-4771-9a49-ffc162ba690f" />

<img width="1183" height="538" alt="image" src="https://github.com/user-attachments/assets/c697e9a3-815a-49fd-8f8f-08dfdf05714f" />

Process ID 6612 is identified as the parent process that executed the remote command, as shown in the execution and process‑tracking logs.

`Answer: 6612`

---

At what time did the threat actor pivot from the beachhead to another system?
Answer format: YYYY-MM-DD HH:MM:SS

<img width="1918" height="621" alt="image" src="https://github.com/user-attachments/assets/1c6620a3-11e2-4c6a-aa4f-620128df5093" />

This timestamp marks the first observed remote execution event against another host, indicating the attacker’s lateral movement from the beachhead system.

`Answer: 2025-11-14 05:19:42`

---

What is the full path of the PowerShell script used by the threat actor to collect data?

<img width="1911" height="206" alt="image" src="https://github.com/user-attachments/assets/0851aef7-2f70-4717-941d-7267817ba9ff" />

Logs show the attacker executed the PowerShell script Setup‑BackupServer.ps1 from the Windows Temp directory to collect data prior to exfiltration.

`Answer: C:\Windows\Temp\Setup-BackupServer.ps1`

---

What are the first 4 file extensions targeted by this script for exfiltration?
Answer format: Chronological, comma-separated

Navigate the file "Setup-BackupServer.ps1"

<img width="486" height="132" alt="image" src="https://github.com/user-attachments/assets/f13deaa7-18e9-4604-a2bc-d3d03c263ac8" />

The script explicitly enumerates these extensions first, indicating an initial focus on backup and database files for data theft.

`Answer: .bak, .backup, .sql, .mdb`

---

What is the full path to the staged file containing collected files?

This file path is where the script stages and consolidates the collected data before exfiltration.

`Answer: C:\Users\bkup-svc\AppData\Local\Temp\sysbackup_20251114.dat`

