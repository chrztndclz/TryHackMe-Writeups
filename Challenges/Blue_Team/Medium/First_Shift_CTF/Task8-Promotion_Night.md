## Promotion Night

It was a glorious Friday at ProbablyFine Ltd. After weeks of sales calls and PoC demos, the team finally signed a contract with DeceptiTech - a major tech company recently hit with ransomware and in need of an MSSP. Monitoring was set to begin on Monday, but some of their clouds and on-premises systems had already been onboarded into the SIEM.

To celebrate the win, the entire SOC team headed out for a big teambuilding.
Everyone except you - the Level 1 analyst covering the night shift, just in case.

The shift was quiet. Too quiet. Then a critical alert appeared: "Potential Ransom Note on DC-01". You blinked. Then blinked again. Then called your Level 2. No answer - just the automated message saying it's probably fine. Now, it's up to you to triage the alert alone. Tonight will either earn you the quickest promotion ever or be your last day at ProbablyFine. Good luck!

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

```
Search: index=* host="SRV-JMP" source="WinEventLog:Security" ComputerName="SRV-JMP.deceptitech.thm" Task_Name=* Account_Name="eric.portman"

```

<img width="985" height="728" alt="image" src="https://github.com/user-attachments/assets/f9c63ed0-5645-4415-aa5c-12c7caa4fb85" />

`Answer: \LanguageSync`

---

What is the MD5 hash of the embedded initial shellcode?



```

Search: index=* *FromBase64String

Look for ScriptBlock ID

Search: index=* *0cac5780-8b57-4146-82fd-0554906d4c43* "1 of 27"

Copy all of the 27 Scripts
Paste it all together in Cyberchef

Recipe: From Base64, XOR

```

<img width="1908" height="724" alt="image" src="https://github.com/user-attachments/assets/e961e4e8-ed02-4fa2-b631-c051dd3ff684" />


<img width="1724" height="338" alt="image" src="https://github.com/user-attachments/assets/029f690f-3084-435f-b329-33c8387ddbc4" />


<img width="1914" height="740" alt="image" src="https://github.com/user-attachments/assets/addf5265-123b-47af-a262-1c1d061328e3" />


<img width="1913" height="778" alt="image" src="https://github.com/user-attachments/assets/2c87d9ea-82be-4b22-8501-50e51ba91b80" />

`Answer: 27B0D51406B5360B49D968D69DF0F3E6`

---

Which C2 framework was used by the adversary in the intrusion?

The adversary used Cobalt Strike because the observed beaconing, lateral movement, and payload staging match its known C2 behaviors and TTPs.

`Answer: Cobalt Strike`

---

What hostname did the adversary log in from on the beachhead?

```

Search: index=* sourcetype="wineventlog"

```

<img width="914" height="712" alt="image" src="https://github.com/user-attachments/assets/e7e62a08-fcaa-48bd-811b-3c1043177468" />

`Answer: DESKTOP-J9PR0CO`

---

What was the UNC path that likely contained AWS credentials?


```

Search: index=* *shared

```

<img width="1240" height="609" alt="image" src="https://github.com/user-attachments/assets/80d8c9a1-bf36-4169-957d-a9d0fcc8cc9d" />

`Answer: \\SRV-ITFS\Integrations\cloud-keys.csv`

---

From which IP address did the adversary access AWS?

```

Search:
index=* src_ip="*" eventSource=*
|  table src_ip eventSource
|  stats count by src_ip


Confirmation Search: index=* src_ip="*" eventSource=*  src_ip="152.42.128.207" | table src_ip eventSource 

```

<img width="1912" height="442" alt="image" src="https://github.com/user-attachments/assets/aac6791a-c051-4e6e-9bf4-32642ab1838a" />

<img width="1914" height="393" alt="image" src="https://github.com/user-attachments/assets/68930c87-5c17-4037-b6c0-5ca1061b78d8" />

`Answer: 152.42.128.207`

---

Which two sensitive files did the adversary exfiltrate from AWS?

```

Search:
index=* src_ip="152.42.128.207" eventSource=s3.amazonaws.com

```

<img width="1912" height="613" alt="image" src="https://github.com/user-attachments/assets/0e53e554-04f0-48f0-aa80-dc29d9adb954" />

<img width="1014" height="682" alt="image" src="https://github.com/user-attachments/assets/19ed2a5b-99e3-433e-911c-8b30a2470b3c" />

`Answer: beta.tar.gz, latest.tar.gz


---

What file did the adversary upload to S3 in place of the wiped ones?

```

Search:
index=* src_ip="152.42.128.207" eventSource=s3.amazonaws.com

```

<img width="1912" height="613" alt="image" src="https://github.com/user-attachments/assets/0e53e554-04f0-48f0-aa80-dc29d9adb954" />

<img width="958" height="393" alt="image" src="https://github.com/user-attachments/assets/0ddb1e94-db24-4690-acfd-ccf24bfaf3de" />


`Answer: YOU-HAVE-BEEN-PWNED.txt`

