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
Username:
niels
 
Password:
TryHackMe#2025
 
IP address/Website:
http://10.48.165.16

```

Login

<img width="1281" height="843" alt="image" src="https://github.com/user-attachments/assets/c51bec65-7da6-4e70-99b3-8b7593d0a69e" />

 Right-click on the page and click Inspect, then click on the Network tab as shown below and refresh the page


<img width="1285" height="847" alt="image" src="https://github.com/user-attachments/assets/4590cc17-f0af-4500-90a3-7072aca4d661" />

Go to the response tab and you'll see that the `user_id: 10`

<img width="1292" height="771" alt="image" src="https://github.com/user-attachments/assets/af61dd8d-5dbc-4f52-81ca-bc06810546e9" />


Go to the Storage tab and expand the local storage
from the auth_user change the user_id from 10 to 11 enter and refresh the page to see the change. 


<img width="1291" height="783" alt="image" src="https://github.com/user-attachments/assets/c400e85c-2353-4ddf-8f47-7794db52e650" />





---

## Key Takeaways







---

## Reflection




