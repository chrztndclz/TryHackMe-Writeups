## Linux Lore

[TryHackMe](https://tryhackme.com/room/adventofcyberpreptrack)

---

## Description

TBFC’s delivery drones are glitching, dropping eggs instead of presents! McSkidy’s last login came from a Linux server, and something in his account might explain why.

Linux powers most servers worldwide, and knowing how to search within it is a must for any defender.

Objective:
Locate McSkidy’s hidden message in his Linux home directory.

Steps:

Use cd /home/mcskidy/ to enter his folder.

Run ls -la to show all files.

Use cat .secret_message to reveal the flag.

---

## Analysis

This challenge reinforces basic Linux navigation and file‑discovery skills. By inspecting McSkidy’s home directory and listing all files (including hidden ones), you uncover a concealed message. These techniques mirror real-world investigations where attackers hide artifacts inside user directories.

---

## Methodology

Step 1: Navigate directory

`cd /home/mcskidy/`

<img width="749" height="364" alt="image" src="https://github.com/user-attachments/assets/3e7ff60f-22c3-4e6f-9305-df4eb972e28e" />

Step 2: List all files

`ls -la`

<img width="732" height="432" alt="image" src="https://github.com/user-attachments/assets/5283613d-4810-45be-825c-1e42109f4263" />

Step 3: Read the file

`cat .secret_message`

<img width="754" height="403" alt="image" src="https://github.com/user-attachments/assets/33212c4c-a288-4563-96ed-34b897828456" />


---

## Flag

THM{Tr----------ny}

---

## Reflection 

This exercise emphasizes how Linux hides important files behind dot prefixes and how defenders use simple commands to reveal them. Understanding directory structures and hidden file behavior is essential for effective system auditing and threat investigation.

---

## Tools

cd — Moves between directories.

ls -la — Lists all files, including hidden ones, with permissions and metadata.

cat — Displays the contents of a file.
