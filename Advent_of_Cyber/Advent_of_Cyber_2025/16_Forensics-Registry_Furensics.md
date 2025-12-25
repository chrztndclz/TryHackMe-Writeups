## Introduction

---
#### The Story

TBFC is under attack. Systems are exhibiting weird behavior, and the company is now feeling the absence of its lead defender, McSkidy. However, McSkidy made sure the legacy continues.

McSkidy’s team, determined and well-trained, is fully confident in securing all the systems and regaining control before the big event, SOCMAS.

They have now decided to conduct a detailed forensic analysis on one of the most critical systems of TBFC, dispatch-srv01. The dispatch-srv01 coordinates the drone-based gifts delivery during SOCMAS. However, recently it was compromised by King Malhare’s bandits of bunnies.

TBFC’s defenders have decided to split into specialized teams to uncover the attack on this system through detailed forensics. While some of the other team members investigate logs, memory dumps, file systems, and other artefacts, you will work to investigate the registry of this compromised system.

#### Learning Objectives

- Understand what the Windows Registry is and what it contains.
- Dive deep into Registry Hives and Root Keys.
- Analyze Registry Hives through the built-in Registry Editor tool.
- Learn Registry Forensics and investigate through the Registry Explorer tool.
  
---

## Walkthrough

**01 Windows Registry**

The Windows Registry is the configuration database of the Windows operating system. It stores system settings, hardware information, installed programs, and user preferences required for normal operation.

Registry data is stored in multiple binary files called hives, such as SYSTEM, SOFTWARE, SECURITY, SAM, NTUSER.DAT, and USRCLASS.DAT. System-wide hives are located in C:\Windows\System32\config, while user-specific hives are stored in each user’s profile directory.

These hives cannot be opened directly. Instead, they are accessed through the Registry Editor, which presents them as structured root keys like HKEY_LOCAL_MACHINE and HKEY_CURRENT_USER. By navigating specific registry paths, useful information can be retrieved, such as connected USB devices or commands executed via the Run dialog.


**02 Registry Forensics**

Registry forensics involves analyzing registry data to uncover evidence of system and user activity during an incident. Since the registry records application usage, startup programs, searches, recently accessed files, and system details, it is essential for building an investigation timeline.

Live analysis is not forensically safe, so registry hives are collected and analyzed offline using tools like Registry Explorer. This tool can load offline hives, replay transaction logs for consistency, and decode binary values into readable data.

In the investigation of dispatch-srv01, Registry Explorer is used to load the collected hives and examine key artifacts. For example, the system hostname can be identified from the SYSTEM hive at ControlSet001\Control\ComputerName\ComputerName, confirming the machine name as DISPATCH-SRV01.


---

## Objective

What application was installed on the dispatch-srv01 before the abnormal activity started?

Open the in the Registry Explorer and load hive the "SOFTWARE" hive file and Navigate this folders: 
Root
Microsoft
Windows
CurrentVersion
Uninstall

<img width="1356" height="848" alt="image" src="https://github.com/user-attachments/assets/545f7914-b7d2-437f-9d2c-30cd90acd78f" />

In the timestamp filter October 21, 2025

<img width="1882" height="856" alt="image" src="https://github.com/user-attachments/assets/5259dd9a-a783-4879-a14e-c97d9ff02af1" />

`DroneManager Updater`


What is the full path where the user launched the application (found in question 1) from?

Open the in the Registry Explorer and load hive the "NTUSER.DAT" hive file and Navigate this folders: 
Root
Microsoft
Windows NT
CurrentVersion
AppCompatFlags
Compatibility Assistant
Store

<img width="1867" height="858" alt="image" src="https://github.com/user-attachments/assets/564d77fe-cc89-4ddf-971e-1887249ecf2e" />

`C:\Users\dispatch.admin\Downloads\DroneManager_Setup.exe`

Which value was added by the application to maintain persistence on startup?

Open the in the Registry Explorer and load hive the "SOFTWARE" hive file and Navigate this folders: 
Root
Microsoft
Windows
CurrentVersion
Run

<img width="1879" height="854" alt="image" src="https://github.com/user-attachments/assets/8f5cfd14-4093-4f98-ae1d-8702b028a6da" />

`"C:\Program Files\DroneManager\dronehelper.exe" --background`

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

