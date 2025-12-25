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

**01 Logging into Splunk**

After starting both the AttackBox and the target machine, allow around three minutes for all services to fully initialize. Open Firefox on the AttackBox and navigate to the Splunk web interface using http://10.48.149.86:8000
. Log in with the provided credentials (username: Blue, password: Pass1234). Once authenticated, you will be redirected to the Splunk Search dashboard. Before running any queries, adjust the time range selector to Last 7 days or All time to ensure relevant events are visible, as a narrow default range may return no results.


**02 Detect Suspicious Web Commands**

The first investigative step focuses on identifying potentially malicious HTTP requests within the Apache access logs. Run the query:
index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression") | table _time host clientip uri_path uri_query status
This query searches for indicators of command execution attempts embedded in web requests, commonly associated with command injection attacks against CGI scripts such as hello.bat. Review the results for unusual URI queries, particularly those containing Base64-encoded strings. When an encoded PowerShell command is found, copy it and decode it using a Base64 decoder to reveal the attacker’s intended command.


**03 Apache Error Logs**

Next, analyze the Apache error logs to determine whether suspicious requests reached the backend and caused execution failures. Execute the query:
index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")
Switch the event display to View: Raw to inspect full log entries. A 500 “Internal Server Error” following requests such as /cgi-bin/hello.bat?cmd=powershell suggests that the server attempted to process attacker-supplied input but failed during execution. This provides strong evidence that exploitation attempts progressed beyond the web layer.


**04 Trace Suspicious Process Creation From Apache**

To confirm whether web-based attacks led to operating system–level execution, pivot to Sysmon logs. Run the query:
index=windows_sysmon ParentImage="*httpd.exe"
Change the event display to View: Table for clarity. Normally, Apache should not spawn system utilities. If you observe child processes like cmd.exe or powershell.exe with httpd.exe as the parent, this strongly indicates successful command injection, showing that the attacker achieved code execution through the web server.


**05 Confirm Attacker Enumeration Activity**

Once execution is confirmed, investigate post-exploitation behavior. Use the query:
index=windows_sysmon *cmd.exe* *whoami*
This searches for instances where the whoami command was executed via cmd.exe. Attackers commonly run this command immediately after gaining access to identify the privilege level of the compromised process. Finding these events confirms that the attacker moved beyond initial access into reconnaissance.


**06 Identify Base64-Encoded PowerShell Payloads**

Finally, check for obfuscated PowerShell activity, a common attacker technique. Run the query:
index=windows_sysmon Image="*powershell.exe" (CommandLine="*enc*" OR CommandLine="*-EncodedCommand*" OR CommandLine="*Base64*")
This query detects PowerShell executions that include encoded payloads. If no results are returned, it suggests the encoded command did not successfully execute. If results are present, decode the Base64 command to fully understand the attacker’s intent and assess the potential impact.

---

## Objective 

What is the reconnaissance executable file name?

index=windows_sysmon *cmd.exe* *whoami*

<img width="1362" height="776" alt="image" src="https://github.com/user-attachments/assets/9f0c4b11-b80e-470a-a3a8-7eb4c884f0b1" />

Go to OrginalFileName

<img width="1339" height="767" alt="image" src="https://github.com/user-attachments/assets/91ee2199-40b0-4454-863f-5f0acf78f309" />

`whoami.exe`

What executable did the attacker attempt to run through the command injection?

index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression") | table _time host clientip uri_path uri_query status

<img width="1360" height="766" alt="image" src="https://github.com/user-attachments/assets/0b5781a7-7901-4e30-930f-b4774e90aaa2" />

`PowerShell.exe`

---

## Key Takeaways 

- Unusually long HTTP requests with Base64-encoded data are strong indicators of command injection attempts against web applications.

- Apache access logs are effective for identifying attacker attempts to execute system commands such as cmd.exe or powershell.exe through vulnerable endpoints.

- Apache error logs, especially entries showing HTTP 500 “Internal Server Error,” help confirm that malicious input reached the backend and triggered execution failures.

- Correlating web server logs with Sysmon telemetry is essential to validate whether an attack progressed from the web layer to the operating system.

- The presence of cmd.exe or powershell.exe spawned by httpd.exe is a high-confidence indicator of successful exploitation.

- Reconnaissance commands like whoami.exe are commonly used by attackers after gaining code execution to determine privilege context.

- Base64-encoded PowerShell payloads are a common obfuscation technique and must be decoded to fully understand attacker intent.

- Effective Blue Team investigations rely on pivoting across multiple log sources to reconstruct the complete attack chain.

---

## Reflection 

This exercise highlights the importance of adopting a holistic, evidence-driven approach to Blue Team investigations. Rather than relying on a single log source, the investigation shows how effective threat triage requires pivoting between different telemetry layers to reconstruct the full attack chain. Each step—from suspicious web requests to backend errors, process creation, and reconnaissance—builds a clearer picture of attacker intent and success.

The scenario also reinforces the value of understanding normal versus abnormal system behavior. Knowing that Apache should not spawn command-line utilities allows defenders to quickly identify high-confidence indicators of compromise. Moreover, the use of Base64 encoding illustrates how even simple obfuscation techniques can conceal malicious intent if defenders are not actively looking for them. Overall, this investigation strengthens the mindset of proactive threat hunting and demonstrates how Splunk can be used not only for alerting, but also for deep forensic analysis and attack-path reconstruction in real-world incidents.
