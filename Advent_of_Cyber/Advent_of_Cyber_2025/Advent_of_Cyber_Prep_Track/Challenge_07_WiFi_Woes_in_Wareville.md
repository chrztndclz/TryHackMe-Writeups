## WiFi Woes in Wareville

[TryHackMe Room](https://tryhackme.com/room/adventofcyberpreptrack)

---

## Description

The TBFC drones are looping endlessly over Wareville Square. Someone logged into the company router using default credentials!

Securing WiFi is critical. Default passwords are like leaving the front gate wide open.

Objective:
Log into the router and secure it with a strong new password.

Steps:

Log in with username admin and password admin.

Go to “Security Settings.”

Set a new strong password that passes validation.

---

## Analysis

This challenge highlights the security risk of leaving default router credentials unchanged. By accessing the router interface and applying a strong password, you prevent unauthorized logins and protect the network from misuse.

---

## Methodology

Step 1: Log in 

<img width="876" height="629" alt="image" src="https://github.com/user-attachments/assets/32b39416-f77d-45e9-ab1d-addbd80d7409" />

Step 2: Create a strong password in the security settings 

e.g. `TryHackMe1234!`

<img width="879" height="642" alt="image" src="https://github.com/user-attachments/assets/5ae366dd-2b16-4be3-9b26-ca16cdded13e" />

Step 3: Save and retrieve the flag 

<img width="893" height="251" alt="image" src="https://github.com/user-attachments/assets/8f7c7abf-7ee3-47ab-a400-dba5bdabc9fd" />

---

## Flag

THM{NoM------------ult}

---

## Reflection 

This exercise reinforces a fundamental security principle: always change default credentials. Attackers frequently scan for devices using factory logins, making strong, unique passwords essential for protecting network infrastructure.

---

## Tools

(none)
