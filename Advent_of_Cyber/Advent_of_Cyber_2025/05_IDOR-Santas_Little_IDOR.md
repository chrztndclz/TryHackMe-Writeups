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

01
02
03
04

---

## Objective

What does IDOR stand for?


---

What type of privilege escalation are most IDOR cases?

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


At user_id = 15, I found the parent account that has 10 children.


---

## Key Takeaways







---

## Reflection




