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

**01 First Lock - Outer Gate**

What is the password for the first lock?

**Second Lock - Outer Wall**

What is the password for the second lock?

**Third Lock - Guard House**

What is the password for the third lock?

**Fourth Lock - Inner Castle**

What is the password for the fourth lock?

**Fifth Lock - Prison Tower**

What is the password for the fifth lock?

What is the retrieved flag?


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

