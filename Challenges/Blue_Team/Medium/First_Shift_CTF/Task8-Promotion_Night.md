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
Search: index=* "\\\\" host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational"

Find "Message" field

```

<img width="1058" height="705" alt="image" src="https://github.com/user-attachments/assets/730328ce-65c4-42ce-8719-871c95ae2ddd" />

`Answer: \\DC-01\SYSVOL\gaze.exe`


---


What is the value ransomware created to persist on reboot?

```
Search: index=* host="DC-01" source="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=13
("Run" OR "RunOnce")

Find "Target Object" 

```

<img width="1595" height="640" alt="image" src="https://github.com/user-attachments/assets/ed547ddd-5e2c-401a-946a-76caefa1a85c" />

`Answer: BabyLockerKZ`


---


What was the most likely extension of the encrypted files?

```
Search: index=* host="SRV-JMP" CommandLine="C:\\Windows\\Temp\\gaze.exe"

Find "Hashes"

Hashes: SHA256=6D000A159FE10AF1B29DDF4E4015931A9E9D0A020AEEF0C602D8C5419B5966E6

Go to virus total (Behavior -> Files Written)

c:\msocache\all users\{90160000-0011-0000-1000-0000000ff1ce}-c\office32ww.xml.danger17

```

<img width="1911" height="628" alt="image" src="https://github.com/user-attachments/assets/bd4fb2de-67e6-4018-902a-3488857c6c75" />


<img width="1417" height="152" alt="image" src="https://github.com/user-attachments/assets/dba252f7-7780-4bf8-af06-3ef64db92b9f" />


<img width="983" height="611" alt="image" src="https://github.com/user-attachments/assets/34b4b03f-5d94-4a2c-a10b-63c4127f6f95" />


`Answer: .danger17`

---

Which MITRE technique ID was used to deploy ransomware?

```
Search:
index=* host="SRV-JMP" CommandLine=*
| table CommandLine

Search "shadowcopy" to mitre.org

```

<img width="1013" height="543" alt="image" src="https://github.com/user-attachments/assets/58d8e004-c1a6-4fba-9c29-d839299b13b9" />

<img width="1906" height="739" alt="image" src="https://github.com/user-attachments/assets/e67547b2-b710-4e2c-82e8-2b5d6ac889d7" />

`Answer: T1047`

---

What ports of SRV-ITFS did the adversary successfully scan?

```
Search: index=* *net view CommandLine=*  CommandLine="net  view SRV-ITFS"

We must find all of the ports related to this event.


Search:
index=* host="SRV-JMP" DestinationPort=*
| table DestinationPort
| sort DestinationPort


```

<img width="1911" height="731" alt="image" src="https://github.com/user-attachments/assets/7d0d349c-c9e2-4898-8088-64995a7a79f8" />

<img width="1911" height="649" alt="image" src="https://github.com/user-attachments/assets/6ba7f26f-737d-4cc8-a555-978cc0185764" />

Note: Explore the ports (You can still narrow down the search)

`Answer: 135, 139, 445, 3389, 5985`

---

What is the full path to the malware that performed the Discovery?

```
Search: index=* *gaze.exe CommandLine=* CurrentDirectory="\\\\DC-01\\SYSVOL\\"

```

<img width="1900" height="698" alt="image" src="https://github.com/user-attachments/assets/9af32607-7f8b-43c8-ad85-7edc8f2cd824" />

<img width="1576" height="184" alt="image" src="https://github.com/user-attachments/assets/0caa08d9-5077-4a39-817c-55ba00b9eff2" />

<img width="1539" height="214" alt="image" src="https://github.com/user-attachments/assets/74998f97-a9a7-4492-8227-f2402f833236" />

`Answer: C:\Windows\System32\fr-FR\ruche.dll`

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
