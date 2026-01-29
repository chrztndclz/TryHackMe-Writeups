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

`Answer: JP-BROWN-WS`

---

What MITRE subtechnique ID describes the initial code execution on the beachhead?

<img width="1914" height="698" alt="image" src="https://github.com/user-attachments/assets/d22a8386-9adb-43c1-a203-e57c6b50673d" />

`Answer: T1204.002`

---

What is the full path of the malicious file that led to Initial Access?

<img width="1913" height="562" alt="image" src="https://github.com/user-attachments/assets/13886303-f6b7-4260-95a8-58c72448c3f0" />

`Answer: C:\Users\jp.brown\Downloads\TravisClart_Resume.pdf.lnk`

---

What is the full path to the LOLBin abused by the attacker for Initial Access?

---

What is the IP address of the attacker's Command & Control server?

---

What is the full path of the process responsible for the C2 beaconing?

---

What is the full path, modified for Persistence on the beachhead host?

---

What tool and parameter did the threat actor use for credential dumping?

---

The threat actor executed a command to evade defenses.
What security parameter did they attempt to change?

---

The threat actor used a tool to execute remote commands on other machines.
What is the process ID (PID) that executed the remote command?

---

At what time did the threat actor pivot from the beachhead to another system?
Answer format: YYYY-MM-DD HH:MM:SS

---

What is the full path of the PowerShell script used by the threat actor to collect data?

---

What are the first 4 file extensions targeted by this script for exfiltration?
Answer format: Chronological, comma-separated

---

What is the full path to the staged file containing collected files?


