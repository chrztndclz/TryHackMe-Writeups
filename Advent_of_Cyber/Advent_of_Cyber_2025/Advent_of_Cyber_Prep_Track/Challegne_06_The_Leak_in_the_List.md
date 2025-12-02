## The Leak in the List

---

## Description

Rumours swirl that TBFC’s data has been leaked. Emails are bouncing, and the staff are panicking.
McSkidy suspects his account might have been part of a breach.

Defenders often use tools like Have I Been Pwned to check for compromised accounts. Early detection can stop an attack from spreading.

Objective:
Check if McSkidy’s email has appeared in a breach.

Steps:

Enter mcskidy@tbfc.com into the breach checker.

Review results for each domain.

Identify the one marked “Compromised.”

---

## Analysis

This challenge simulates using a breach‑checking service to verify if an email address has appeared in known data leaks. By entering McSkidy’s email and reviewing the results, you identify which domain shows a compromise—an essential step in validating account exposure during incident response.

---

## Methodology

Step 1: Enter the email and check

`mcskidy@tbfc.com`

<img width="747" height="752" alt="image" src="https://github.com/user-attachments/assets/cb04fd42-cb3e-409d-b3f2-910a7ba7a10b" />

Step 2: Review results and click the "compromsed" one

<img width="762" height="716" alt="image" src="https://github.com/user-attachments/assets/eeb94cf3-c53f-4b41-bb7e-27738678c7d2" />

Step 3: Retrieve the flag

---

## Flag

THM{Lea-----------und}

---

## Reflection 

This task highlights how breach‑checking tools help defenders quickly verify potential account exposure. Detecting compromised credentials early reduces risk and guides appropriate mitigation steps such as password resets or additional investigation.

---

## Tools

(none)
