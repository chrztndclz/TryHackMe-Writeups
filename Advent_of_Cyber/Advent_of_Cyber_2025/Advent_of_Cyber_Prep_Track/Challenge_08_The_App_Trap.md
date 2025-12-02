## The App Trap

---

## Description

McSkidy’s social account has gone rogue, posting strange messages about “EASTMAS.” A suspicious third party app may be behind it.

Learning to review and manage app permissions helps stop data leaks before they start.

Objective:
Find and remove the malicious connected app.

Steps:

Review the list of connected apps.

Look for one with unusual permissions (like “password vault” access).

Click “Revoke Access.”

---

## Analysis

This challenge demonstrates the risks of third-party apps with excessive permissions. Malicious apps can post content, access sensitive information, or compromise accounts. Reviewing connected apps and identifying unusual permissions is essential for preventing unauthorized access.

---

## Methodology

Step 1: Review the apps 

<img width="832" height="440" alt="image" src="https://github.com/user-attachments/assets/62e83afe-da10-47b0-8ad5-2669f968ee74" />

Step 2: Look for unusual permissions

<img width="873" height="541" alt="image" src="https://github.com/user-attachments/assets/f5954d98-f6fd-4c37-a570-d1e14b8c7271" />

Step 3: Revoke Access 

Revoke the Access of "password vault" access

<img width="880" height="252" alt="image" src="https://github.com/user-attachments/assets/465af418-fa99-40d7-ad66-244c7239c362" />

Step 4: Retrieve the flag

---

## Flag

THM{App---------ped}

---

## Reflection 

This exercise highlights the importance of regularly auditing connected applications. Revoking suspicious apps prevents data leakage and protects account integrity.

---

## Tools

(none)
