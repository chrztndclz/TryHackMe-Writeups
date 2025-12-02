## Welcome to the AttackBox!

[TryHackMe Room](https://tryhackme.com/room/adventofcyberpreptrack)

---

## Description

You step into TBFC’s AttackBox, a secure virtual environment built for training. The system hums quietly, waiting for your first command.

This is where defenders learn, break, and rebuild safely. Getting comfortable with the command line is your first step toward cyber mastery.

Objective:
Find and read the hidden welcome message inside your AttackBox.

Steps:

Use ls to list files.

Use cd challenges/ to change directories.

Use cat welcome.txt to read the text file.

---

## Analysis

This challenge introduces the basic Linux commands used to navigate a file system inside the AttackBox. The objective is simply to locate a specific directory and read the welcome message stored in a text file. Understanding these fundamentals is essential for navigating and working efficiently in any cybersecurity environment.

---

## Methodology

Step 1: List the files 

`ls command`

<img width="779" height="453" alt="image" src="https://github.com/user-attachments/assets/97f147c8-e1d0-4e4b-8b01-2350da6a1bf6" />

Step 2: Change directory

`cd command`

<img width="752" height="470" alt="image" src="https://github.com/user-attachments/assets/e2fc526c-9456-4720-8a32-2badaab10850" />


Step 3: List the files

`ls command`

<img width="759" height="483" alt="image" src="https://github.com/user-attachments/assets/9db957fd-a3a1-4128-8db6-af175ad5d1d5" />

Step 4: Read the text file

`cat command`

<img width="797" height="340" alt="image" src="https://github.com/user-attachments/assets/5f0c8b9f-895a-4d20-b5ef-14bbca621140" />

Step 5: Retrieve the flag

---

## Flag

THM{Rea----------ack}

---

## Reflection 

This challenge reinforces the foundational commands needed for Linux navigation. Mastering basic file listing, directory movement, and reading files is crucial, as nearly every security task—recon, exploitation, forensics—relies on efficient command-line work.

---

## Tools

ls – Lists files and directories in the current location.

cd – Changes the working directory.

cat – Displays the contents of a text file.
