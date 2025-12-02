## The CMD Conundrum

---

## Description

McSkidy’s workstation shows signs of tampering, suspicious files moved, logs wiped, and a strange folder named `mystery_data`.

It’s time to use the **Windows Command Prompt** to uncover what’s hidden.  
Learning these commands helps you investigate systems and find what the GUI can’t.

**Objective:**  
Find the hidden flag file using Windows commands.

**Steps:**

1. Use `dir` to list visible files.
    
2. Try `dir /a` to reveal hidden ones.
    
3. Use `type hidden_flag.txt` to read the flag.

---

## Analysis

This challenge practices essential Windows Command Prompt skills for basic digital forensics. By using directory listing commands—including switches to reveal hidden files—you locate and read a concealed flag. These same techniques are used in real investigations to uncover tampering, hidden artifacts, and suspicious activity.

---

## Methodology

Step 1: List the files 

`dir command`

<img width="752" height="621" alt="image" src="https://github.com/user-attachments/assets/90ac23d6-98f9-4026-9c2a-57e74e1963c2" />

Step 2: Reveal hidden directory 

`dir /a command`

<img width="725" height="603" alt="image" src="https://github.com/user-attachments/assets/76a41f5e-1e1e-48d1-9850-2f92334735d5" />

Step 3: Change directory

`cd command`

<img width="705" height="606" alt="image" src="https://github.com/user-attachments/assets/b95f4571-3db3-4fbd-96c9-9d359f49d04e" />


Step 4: List the files

`dir command`

<img width="701" height="604" alt="image" src="https://github.com/user-attachments/assets/851242a3-f9fa-4e63-bba2-29f742b73cf1" />

Step 5: Read the text file 

`type command`

<img width="753" height="420" alt="image" src="https://github.com/user-attachments/assets/8fa73058-93b0-4b23-baa8-ae3f89e78b34" />

Step 6: Reveal hidden file 

`dir /a command`

<img width="735" height="606" alt="image" src="https://github.com/user-attachments/assets/d4f5df12-5d69-4521-ae7f-f4a99f33bbff" />

Step 7: Read the text file 

`type command`

<img width="739" height="507" alt="image" src="https://github.com/user-attachments/assets/a23b592e-e717-4ea5-8731-ed422c46adf3" />


---

## Flag

THM{Whe----------idy}

---

## Reflection 

This challenge demonstrates how attackers hide files in Windows systems and how analysts uncover them using simple command-line tools. Mastering basic commands like dir, dir /a, cd, and type is crucial for quickly navigating directories and analyzing suspicious activity during an investigation.

---

## Tools

dir — Lists files in a directory.

dir /a — Lists all files, including hidden and system files.

cd — Moves between directories.

type — Displays the contents of text files.
