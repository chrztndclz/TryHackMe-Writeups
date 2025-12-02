## Password Pandemonium

[TryHackMe Room](https://tryhackme.com/room/adventofcyberpreptrack)

---

## Description

As you log into your new TBFC workstation, an alert pops up:

> “_Weak passwords detected on 73 TBFC accounts!_”

Even McSkidy’s password, `P@ssw0rd123`, has been flagged. Before gaining full access, you’ll need to prove your password prowess.

Strong passwords are one of the simplest yet most effective defences against cyber attacks.

**Objective:**  
Create a password that passes all system checks and isn’t found in the leaked password list.

**Steps:**

1. Enter a password with at least 12 characters.
    
2. Include uppercase, lowercase, numbers, and symbols.
    
3. Ensure it isn’t in the breach database.


---

## Analysis 

This task focuses on strengthening weak passwords detected on several TBFC accounts. The system enforces strict password requirements to ensure security against brute-force attacks and credential stuffing. The goal is simply to craft a password that satisfies the policy and is not found in the leaked breach database.

--- 

## Methodology 

Step 1: Update Password 


<img width="918" height="631" alt="image" src="https://github.com/user-attachments/assets/b2b4d07b-82ee-48f7-8ad7-c4ce70729743" />


Step 1: Create a strong password 

**Steps:**

1. Enter a password with at least 12 characters.
    
2. Include uppercase, lowercase, numbers, and symbols.
    
3. Ensure it isn’t in the breach database.

e.g. `TryHackMe@2o25`

<img width="917" height="498" alt="image" src="https://github.com/user-attachments/assets/8b90f18b-eaca-41db-b103-35c12780dfb0" />


Step 2: Submit and Retrieve the Flag 


<img width="679" height="141" alt="image" src="https://github.com/user-attachments/assets/ef24e3db-2b3d-4c2d-98d5-a32447cb3062" />


---

## Flag 

THM{Str---------art}

---

## Reflection

This challenge reinforces the importance of creating strong, non-guessable passwords. Even slightly complex passwords like “P@ssw0rd123” fail because attackers rely on breached dictionaries and predictable patterns. By incorporating randomness, mixed character sets, and sufficient length, we significantly reduce the risk of credential compromise.

Password hygiene remains one of the easiest yet most effective cybersecurity defenses.

---

## Tools

(none)
