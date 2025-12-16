## Introduction 

---

#### The Story

TBFC’s drone scheduler web UI is getting strange, long HTTP requests containing Base64 chunks. Splunk raises an alert: “Apache spawned an unusual process.” On some endpoints, these requests cause the web server to execute shell code, which is obfuscated and hidden within the Base64 payloads. For this room, your job as the Blue Teamer is to triage the incident, identify compromised hosts, extract and decode the payloads and determine the scope.

You’ll use Splunk to pivot between web (Apache) logs and host-level (Sysmon) telemetry.

Follow the investigation steps below; each corresponds to a Splunk query and investigation goal.

#### Learning Objectives
- Detect and analyze malicious web activity through Apache access and error logs
- Investigate OS-level attacker actions using Sysmon data
- Identify and decode suspicious or obfuscated attacker payloads
- Reconstruct the full attack chain using Splunk for Blue Team investigation
- Connecting to the Machine

---

## Walkthrough

01 Logging into Splunk
02 Detect Suspicious Web Commands 
03 Apache Error Logs 
04 Trace Suspicious Process Creation From Apache
05 Confirm Attacker Enumeration Activity 
06 Identify Base64-Encoded PowerShell Payloads 

---

## Objective 

What is the reconnaissance executable file name?


What executable did the attacker attempt to run through the command injection?


---

## Key Takeaways 



---

## Reflection 



---


