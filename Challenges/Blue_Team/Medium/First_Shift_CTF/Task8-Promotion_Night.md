## Promotion Night

It was a glorious Friday at ProbablyFine Ltd. After weeks of sales calls and PoC demos, the team finally signed a contract with DeceptiTech - a major tech company recently hit with ransomware and in need of an MSSP. Monitoring was set to begin on Monday, but some of their clouds and on-premises systems had already been onboarded into the SIEM.

To celebrate the win, the entire SOC team headed out for a big teambuilding.
Everyone except you - the Level 1 analyst covering the night shift, just in case.

The shift was quiet. Too quiet. Then a critical alert appeared: "Potential Ransom Note on DC-01". You blinked. Then blinked again. Then called your Level 2. No answer - just the automated message saying it's probably fine. Now, it's up to you to triage the alert alone. Tonight will either earn you the quickest promotion ever or be your last day at ProbablyFine. Good luck!

---

Important Details 

"Potential Ransom Note on DC-01"

Share Path(where the ransomwere is placed: \\DC-01\SYSVOL\gaze.exe 

---




---


What was the network share path where ransomware was placed?

```
Searches:

index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" "\\\\DC-01\\SYSVOL"

Extract the share path from Message
index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" "gaze.exe"
| rex field=Message "(?<share_path>\\\\DC-01\\\\SYSVOL)"
| table _time host share_path Message

Show all files written to that share
index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" "\\\\DC-01\\SYSVOL"
| table _time Image User Message

---------------------------------------------------------------------------------------------

Search: index=* "\\\\" host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational"

Find "Message" field

```

<img width="1058" height="705" alt="image" src="https://github.com/user-attachments/assets/730328ce-65c4-42ce-8719-871c95ae2ddd" />

`Answer: \\DC-01\SYSVOL\gaze.exe`


---




What is the value ransomware created to persist on reboot?

```
Searches

index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=13

index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=13
("Run" OR "RunOnce")

---------------------------------------------------------------------------------------------

Search: index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=13
("Run" OR "RunOnce")

Find "Target Object" 

```

<img width="1595" height="640" alt="image" src="https://github.com/user-attachments/assets/ed547ddd-5e2c-401a-946a-76caefa1a85c" />

`Answer: BabyLockerKZ`


---

index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" (EventCode=11 or EventCode=13) gaze.exe 


index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11
| table _time TargetFilename


index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
| regex TargetFilename="\.([A-Za-z0-9]{8})$"
| table TargetFilename


index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
| rex field=TargetFilename "\.(?<encrypted_ext>[A-Za-z0-9]{8})$"
| stats count by encrypted_ext



index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" "gaze.exe"
| rex field=Message "(?<share_path>\\\\DC-01\\\\SYSVOL)"
| rex field=Message "\.(?<encrypted_ext>[A-Za-z0-9]{8})"
| table _time host share_path encrypted_ext Message


index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" TargetObject="*"
| rex field=TargetObject "\.(?<encrypted_ext>[A-Za-z0-9]{8})$"
| table _time TargetObject encrypted_ext Message


What was the most likely extension of the encrypted files?

Note: The target object maybe should be where we find this "BabyLockerKZ"

and probably the rulename for the next questions and the answer to this is connected


---

Which MITRE technique ID was used to deploy ransomware?


---

What ports of SRV-ITFS did the adversary successfully scan?


---

What is the full path to the malware that performed the Discovery?


---

Which artifact did the adversary create to persist on the beachhead?


---

What is the MD5 hash of the embedded initial shellcode?

---

Which C2 framework was used by the adversary in the intrusion?

---

What hostname did the adversary log in from on the beachhead?

---

What was the UNC path that likely contained AWS credentials?

---

From which IP address did the adversary access AWS?

---

Which two sensitive files did the adversary exfiltrate from AWS?

---

What file did the adversary upload to S3 in place of the wiped ones?
