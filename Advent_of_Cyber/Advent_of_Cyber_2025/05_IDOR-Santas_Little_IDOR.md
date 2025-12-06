## Introduction

---

#### The Story

The elves of Wareville are on high alert since McSkidy went missing. Recently, the support team has been receiving many calls from parents who can't activate vouchers on the TryPresentMe website. They also mentioned they are receiving many targeted phishing emails containing information that is not public. The support team is wary and has enlisted the help of the TBFC staff. When looking into this peculiar case, they discovered a suspiciously named account named Sir Carrotbane, which has many vouchers assigned to it. For now, they have deleted the account and retrieved the vouchers. But something is going on. Can you help the TBFC staff investigate the TryPresentMe website and fix the vulnerabilities?

## Learning Objectives

- Understand the concept of authentication and authorization
- Learn how to spot potential opportunities for Insecure Direct Object References (IDORs)
- Exploit IDOR to perform horizontal privilege escalation
- Learn how to turn IDOR into SDOR (Secure Direct Object Reference)

---

## Walkthrough

01 Logging In

Begin by logging into the application using the provided credentials. After successfully accessing the dashboard, open your browser’s Developer Tools and switch to the Network tab so you can observe how the page retrieves data in real time.

02 Identifying the Initial user_id

Refresh the page and look for the request named view_accountinfo. When you open it, you’ll see a field such as "user_id": 10, revealing that the application directly uses your user ID from the browser to fetch account information. This behavior suggests a potential IDOR vulnerability.

03 Modifying user_id in Local Storage

Navigate to the Storage section in Developer Tools and open Local Storage. Locate the auth_user entry and change the value of user_id from 10 to 11. After refreshing the page, you will see that the account information now belongs to another user, confirming the presence of an IDOR vulnerability.

04 Resetting the Session

Once the vulnerability is confirmed, reset the session by either reverting the value back to 10 or logging out and signing back in. This ensures you return to your original user state before proceeding.

05 Observing Child Profile Access

Open one of the child profiles by clicking the eye icon. In the Network tab, you’ll notice that the child ID is sent in base64 format (e.g., Mg==). While encoded, this is simply base64 for the number 2. Changing this value to another base64 number allows you to access other children’s data, demonstrating another instance of IDOR.

06 Editing Child Profiles Using Hashed IDs

Next, click the pencil icon to edit a child profile. This time, the request uses a hash-like identifier, such as an MD5 or SHA1 hash. Despite its appearance, this hash is still derived from a predictable internal ID. If an attacker can generate or guess this hash, they could modify other users’ information—another form of IDOR disguised with more complex encoding.

07 Examining Voucher UUIDs

Proceed to the voucher section. Voucher identifiers here use a UUID format, such as b8e12a30-2acf-11ef-8c45-0242ac120002. Although this looks random, it is a UUIDv1, which contains timestamps and MAC-address-based patterns. Because UUIDv1 values can be predicted, an attacker could brute-force or guess valid voucher IDs, once again showing insufficient authorization controls.

08 Understanding the Root Cause

Across all these cases, the core problem remains unchanged: the server does not verify whether the authenticated user is permitted to access the requested resource. Regardless of the ID format—plain numeric, base64, hashed, or UUID—authorization is missing, which leaves the system wide open to IDOR attacks.

09 Recommended Fix

To resolve these issues, the application must implement proper server-side authorization checks for every sensitive resource. Encoding, hashing, or obfuscating IDs is never a replacement for real access control.
---

## Objective

What does IDOR stand for?

`Insecure Direct Object Reference`

---

What type of privilege escalation are most IDOR cases?

`Horizontal`

---

Exploiting the IDOR found in the view_accounts parameter, what is the user_id of the parent that has 10 children?


Credentials
```
Username: niels
Password: TryHackMe#2025
IP address/Website: http://10.48.165.16
```

01 Login
<img width="1281" height="843" alt="image" src="https://github.com/user-attachments/assets/c51bec65-7da6-4e70-99b3-8b7593d0a69e" />


02 Inspect the Page

Right-click anywhere on the page and select Inspect.
Open the Network tab as shown below, then refresh the page.

<img width="1285" height="847" alt="image" src="https://github.com/user-attachments/assets/4590cc17-f0af-4500-90a3-7072aca4d661" />

In the Response tab, you’ll see the value user_id: 10.

<img width="1292" height="771" alt="image" src="https://github.com/user-attachments/assets/af61dd8d-5dbc-4f52-81ca-bc06810546e9" />


03 Change the user_id Value

Go to the Storage tab and expand Local Storage.
Locate the auth_user entry and modify the user_id value from 10 to other values until you find the parent with 10 children.

In my case, I tested:

11, 12, 13, 14, 15


`At user_id = 15, I found the parent account that has 10 children.`


---

## Key Takeaways

- IDOR occurs when the server trusts user-controlled identifiers without verifying authorization.

- Simply changing a parameter (e.g., user_id) can reveal or modify other users’ data.

- Encoding or hashing the identifier (Base64, MD5, SHA1, UUIDv1) does not eliminate the vulnerability.

- Proper authorization checks must always occur server-side — never rely on obfuscation.

- Converting an IDOR to an SDOR requires enforcing strict access validation for every request.

---

## Reflection

This challenge reinforced how common and dangerous IDOR vulnerabilities are, especially in applications that rely on predictable identifiers or expose them directly in client-side logic. Even simple parameter manipulation can lead to major data leaks. Understanding how to analyze browser network traffic, inspect session storage, and observe server responses is essential for identifying—and more importantly, preventing—horizontal privilege escalation.

If you want, I can also generate a polished RRL, methodology section, or a diagram showing how the IDOR flow works.

