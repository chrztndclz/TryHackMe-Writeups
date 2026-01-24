## Introduction

---
#### The Story

McSkidy is imprisoned in King Malhare's Quantum Warren. Sir BreachBlocker III was put in charge of securing the fortress and implemented several access controls to prevent any escape. His defenses are worthy of his name.

However, McSkidy managed to send vital clues to his team using harmless bunny pictures. One message revealed that five locks needed to be disabled to secure an escape route. The locks can be broken by examining their logic and leveraging the system's built-in chat for the guards. They can be eluded in revealing vital details or even passwords. However, you will need to speak their language.

#### Learning Objectives

- Introduction to encoding/decoding
  
- Learn how to use CyberChef
  
- Identify useful information in web applications through HTTP headers
  
---

## Walkthrough

IP: http://10.48.165.199:8080

**01 First Lock - Outer Gate**

Guard Name: CottonTail
Base64: Q290dG9uVGFpbA==

<img width="1310" height="784" alt="image" src="https://github.com/user-attachments/assets/0995426a-a23f-42e0-bb9b-4f7c29aa9f3a" />

Inspect and go to Network Tab

Magic Question: What is the password for this level?

Base64: V2hhdCBpcyB0aGUgcGFzc3dvcmQgZm9yIHRoaXMgbGV2ZWw/

<img width="1317" height="786" alt="image" src="https://github.com/user-attachments/assets/6f42a3ff-9059-45dc-bfdc-f0e4775b1ddf" />

<img width="1078" height="756" alt="image" src="https://github.com/user-attachments/assets/8f68113f-aef6-4483-b34a-419f38d58148" />


CottonTail Reply: SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IFNXRnRjMjltYkhWbVpuaz0=

Here is the password: SWFtc29mbHVmZnk=

Base64: Iamsofluffy


What is the password for the first lock?

Iamsofluffy


---


**Second Lock - Outer Wall**

Guard Name: CarrotHelm

Base64: Q2Fycm90SGVsbQ==

<img width="1310" height="765" alt="image" src="https://github.com/user-attachments/assets/ef285d2d-eaf5-4b10-9ca5-9a52dac65ddc" />

Inspect and go to Network Tab

Magic Question: Did you change the password?

Base64: RGlkIHlvdSBjaGFuZ2UgdGhlIHBhc3N3b3JkPw==

<img width="1315" height="705" alt="image" src="https://github.com/user-attachments/assets/b98e39e6-e2fb-4c63-89d9-2fae96543442" />

CarrotHelm Reply: SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IFUxaFNkbUpIVWpWaU0xWXdZakpPYjFsWE5XNWFWMnd3U1ZFOVBRPT0=

Here is the password: U1hSdmJHUjViM1YwYjJOb1lXNW5aV2wwSVE9PQ==

Base64: SXRvbGR5b3V0b2NoYW5nZWl0IQ==

Base64: Itoldyoutochangeit!

What is the password for the second lock?

Itoldyoutochangeit!


---


**Third Lock - Guard House**

Guard Name: LongEars

Base64: TG9uZ0VhcnM=

<img width="1333" height="707" alt="image" src="https://github.com/user-attachments/assets/fa07b121-a279-4afa-9505-42dcd6735a60" />


Ask the guard in base64 (You can ask in any way)
Password Please!
UGFzc3dvcmQgUGxlYXNlIQ==

LongEars Reply: SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IElRd0ZGakFXQmdzZg==

Here is the password: IQwFFjAWBgsf

Base64 + XOR: BugsBunny

<img width="1326" height="710" alt="image" src="https://github.com/user-attachments/assets/1aab62a9-2492-4628-a9f9-e06cfe0134b2" />

What is the password for the third lock?

BugsBunny


---


**Fourth Lock - Inner Castle**

Guard Name: Lenny
Base64: TGVubnk=

Ask the guard in base64 (You can ask in any way)
Password Please!
UGFzc3dvcmQgUGxlYXNlIQ==

Lenny Reply: SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IGI0YzBiZTdkN2U5N2FiNzRjMTMwOTFiNzY4MjVjZjM5

Here is the password: b4c0be7d7e97ab74c13091b76825cf39
MD5: passw0rd1

What is the password for the fourth lock?
passw0rd1


---


**Fifth Lock - Prison Tower**

Guard Name: Carl
Base64: Q2FybA==

<img width="1320" height="710" alt="image" src="https://github.com/user-attachments/assets/a8420a02-5d1a-4608-8d54-4f6ceb112838" />


Inspect and go to Network Tab
Case: R1
From Base64 ⇒ Reverse ⇒ ROT13


<img width="1338" height="595" alt="image" src="https://github.com/user-attachments/assets/af301891-bdff-4a2f-ac83-6063af2c2e43" />

Ask the guard in base64 (You can ask in any way)
Password Please!
UGFzc3dvcmQgUGxlYXNlIQ==

Carl Reply: SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IFpUTjRjREI1VDNWd05ETmxUMlV4TlE9PQ==

Here is the password: ZTN4cDB5T3VwNDNlT2UxNQ==
Base64: e3xp0yOup43eOe15
Reverse + rot13: 51rBr34chBl0ck3r

What is the password for the fifth lock?
51rBr34chBl0ck3r

<img width="1293" height="574" alt="image" src="https://github.com/user-attachments/assets/60f8c02f-12f2-44d4-bc83-cf000af6ca44" />

What is the retrieved flag?
THM{M3D13V4L_D3C0D3R_4D3P7}

---

## Key Takeaways

- The Windows Registry is a critical source of forensic evidence, storing detailed information about system configuration, user activity, installed applications, and persistence mechanisms.

- Registry hives such as SOFTWARE and NTUSER.DAT provide valuable insight into application installation history and user-executed programs.

- The Uninstall registry key helps identify software installed before or during suspicious activity, aiding timeline reconstruction.

- User execution paths can be traced through compatibility and application tracking registry keys, revealing where malicious or suspicious installers were launched from.

- Startup persistence is commonly achieved through the Run registry key, making it a high-value artifact during incident investigations.

- Offline analysis using tools like Registry Explorer ensures forensic integrity and allows decoding of binary registry data that cannot be reliably examined on a live system.

---

## Reflection

This investigation highlights how powerful registry analysis is in uncovering attacker activity on compromised systems. By methodically examining offline registry hives, it becomes possible to reconstruct the sequence of events leading up to the compromise, from application installation to execution and persistence. The discovery of DroneManager Updater and its startup mechanism demonstrates how attackers often blend malicious components with legitimate-looking software to evade detection. Overall, this exercise reinforces the importance of registry forensics as a core skill in digital investigations and incident response, especially when building accurate timelines and identifying persistence techniques.

