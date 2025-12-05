## Introduction

---
#### The Story

The unthinkable has happened - McSkidy has been kidnapped. Without her, Wareville’s defenses are faltering, and Christmas itself hangs by a thread. But panic won’t save the season. A long road lies ahead to uncover what truly happened. The TBFC (The Best Festival Company) team already brainstorms what to do next, and their first lead points to the **tbfc-web01**, a Linux server processing Christmas wishlists. Somewhere within its data may lie the truth: traces of McSkidy’s final actions, or perhaps the clues to King Malhare’s twisted vision for EASTMAS.

#### Learning Objectives

- Learn the basics of the Linux command-line interface (CLI)
- Explore its use for personal objectives and IT administration
- Apply your knowledge to unveil the Christmas mysteries

---

## Walkthrough


**01 Basic Linux CLI Interactions**

I began by executing simple commands such as:

- `echo` — displaying output

- `ls` — listing directory contents

- `cat` — reading file contents

These commands served as an entry point to understanding how Linux environments are used when GUI access is unavailable—very common in server and SOC workflows.

---

**02 Navigating the Filesystem**

Using:

- `pwd` to check my current working directory

- `cd` to navigate  
    I moved into McSkidy’s "Guides" directory and discovered that it was empty. This introduced the concept of **hidden files**—common both in legitimate configuration contexts and malware concealment.


Running `ls -la` revealed `.guide.txt`, which contained McSkidy’s notes.

---

**03 Searching Logs With `grep`**

McSkidy’s guide pointed me to `/var/log/auth.log`.  
Using:

`grep "Failed password" auth.log`

I filtered the logs to isolate authentication failures.  
This highlighted repeated failed logins from “HopSec”—the attacker’s origin—demonstrating how log analysis is used to detect intrusion attempts.

---

**04 Locating Suspicious Files With `find`**

Following another hint, I searched the `socmas` home directory for files containing “egg”:

`find /home/socmas -name *egg*`

This exposed `eggstrike.sh`, a suspicious shell script. Its contents revealed an attack pipeline designed to:

- Sort and extract unique wishlist entries

- Dump them to `/tmp`

- Delete the real wishlist

- Replace it with an EASTMAS version


This illustrated how attackers automate malicious actions in a single script.

---

**05 Learning Common CLI Operators**

**The walkthrough also introduced important operators:

- `|` — piping output between commands

- `>` / `>>` — redirecting output to files

- `&&` — chaining commands conditionally


These are essential for writing automated scripts, debugging pipelines, and analyzing malware behavior.**

---

**07 Switching to the Root User**

Finally, I viewed command history using:

- `history`

- `cat .bash_history`


This revealed that attackers had indeed been active, leaving behind a final message:  
`THM{until-we-meet-again}`.

This showed how bash history can expose attacker activity—even after scripts are deleted.

---

## Objectives

Which CLI command would you use to list a directory?

`ls` 


<img width="804" height="554" alt="image" src="https://github.com/user-attachments/assets/25808d77-5580-49fd-b78b-849b5db02e75" />

`THM{learning-linux-cli}`

---

Which command helped you filter the logs for failed logins?

`grep` 


<img width="1057" height="363" alt="image" src="https://github.com/user-attachments/assets/26dbfd17-78c2-4574-85d7-eac678a3b4ec" />

`THM{sir-carrotbane-attacks}`

---

Which command would you run to switch to the root user?

`sudo su`

---

Finally, what flag did Sir Carrotbane leave in the root bash history?

<img width="1059" height="381" alt="image" src="https://github.com/user-attachments/assets/b1427421-af02-4cd8-8fc3-f1590d5c788e" />

`THM{until-we-meet-again}`


---
## Key Takeaways

- The Linux CLI is essential for system monitoring, troubleshooting, and cybersecurity investigations.

- Hidden files (`ls -la`) are commonly used—both by admins and attackers.

- Logs contain the clues to most breaches, and `grep` is one of the fastest tools for extracting patterns.

- Shell scripts (`.sh`) often reveal attacker logic or automation.

- The `find` command is invaluable for quickly locating suspicious files.

- Root access provides full system visibility but is also a prime target for attackers.

- Bash history can preserve traces left by adversaries.

---
## Reflection

This challenge reinforced the importance of being comfortable with Linux CLI fundamentals, especially in incident response and monitoring contexts. Every command taught here—`ls`, `cat`, `grep`, `find`, `sudo`, `history`—is foundational for cybersecurity analysts, SOC engineers, and penetration testers.

By following McSkidy’s breadcrumb trail, I learned how attackers interact with Linux systems, how their activities leave traces, and how defensive teams can uncover those traces through systematic investigation.

Day 1 set the stage for more advanced tasks ahead by grounding me in the investigative mindset needed for the rest of the AoC storyline.
