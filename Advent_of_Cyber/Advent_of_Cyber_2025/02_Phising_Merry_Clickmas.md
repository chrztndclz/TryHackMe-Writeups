## The Story


In light of several recent cyber security threats against The Best Festival Company (TBFC), the local red team has scheduled several penetration tests. The red teamers proceeded to carry out a regular penetration test against their TBFC. Part of this exercise is to ensure that the employees are diligent when clicking links and that the company is well protected against the latest phishing attacks. This type of authorised phishing is a proven way to learn whether the cyber security awareness training has fruited.

In this task, you will be part of the TBFC local red team with the elves Recon McRed, Exploit McRed, and Pivot McRed. You will help them plan and execute their phishing campaign. It is time to see if more cyber security awareness training is required.

## Learning Objectives

- Understand what social engineering is
- Learn the types of phishing
- Explore how red teams create fake login pages
- Use the Social-Engineer Toolkit to send a phishing email

----

## Walkthrough

**01 Understanding Social Engineering & Phishing**

Social engineering is the manipulation of human behavior to achieve unauthorized access. Phishing is its most widespread form, appearing through email, SMS, voice calls, QR codes, or social media. TBFC teaches employees to follow the S.T.O.P. method to identify suspicious communications—slowing down, typing URLs manually, verifying senders, and never opening unexpected content.

---

**02 Preparing the Credential-Harvesting Trap**

A fake TBFC login page was already provided in `~/Rooms/AoC2025/Day02`. Running `./server.py` hosted the phishing page on port 8000, capturing any submitted credentials. Using Firefox to visit `http://127.0.0.1:8000` confirmed the page's appearance. The server terminal remained open to log credentials once the target interacted with the link

---

**03 Crafting the Phishing Email via SET**

Launching **SET** with `setoolkit` initiated the process. The attack path followed:

1. **Social-Engineering Attacks → Mass Mailer Attack**

2. **E-Mail Attack Single Email Address**

3. **Target**: `factory@wareville.thm`

4. **Sender**: `updates@flyingdeer.thm` with name "Flying Deer"

5. **SMTP Server**: `MACHINE_IP`, port 25

6. **Subject**: “Shipping Schedule Changes”

7. **Body**: A believable shipping update referencing the phishing page URL  
    (`http://CONNECTION_IP:8000`)

SET delivered the email to the TBFC factory address.

---

## Objectives

What is the password used to access the TBFC portal?

<img width="960" height="232" alt="image" src="https://github.com/user-attachments/assets/a7706139-e948-4ef9-a3b8-fdc4c012b144" />

<img width="1332" height="791" alt="image" src="https://github.com/user-attachments/assets/fddd0e79-c1b2-4296-9bb6-c3f18e48fb60" />

<img width="934" height="164" alt="image" src="https://github.com/user-attachments/assets/f5b696d5-81bd-43f1-9efe-29c5f3598a2f" />

<img width="920" height="647" alt="image" src="https://github.com/user-attachments/assets/a2199fe2-a8dc-4e13-98f0-eddb083773c5" />

<img width="776" height="723" alt="image" src="https://github.com/user-attachments/assets/0a3e48dc-c1b3-402d-885a-c55f4a063b83" />

<img width="776" height="763" alt="image" src="https://github.com/user-attachments/assets/97bfd1e5-ee32-44a2-afac-a6a2efa5f613" />

<img width="853" height="772" alt="image" src="https://github.com/user-attachments/assets/e7403403-85cc-415b-8ffc-da2473a73bd4" />

<img width="810" height="393" alt="image" src="https://github.com/user-attachments/assets/ca955a11-0165-4d34-b51f-6df85d43e82e" />

<img width="811" height="519" alt="image" src="https://github.com/user-attachments/assets/3d406ba7-100e-42ea-addd-9fb28b0998df" />

<img width="872" height="531" alt="image" src="https://github.com/user-attachments/assets/e873af46-4f81-4efa-9990-071b72ec3f3b" />

<img width="858" height="564" alt="image" src="https://github.com/user-attachments/assets/1ac68d30-2d81-4c88-ba3d-a940326416fc" />

<img width="914" height="620" alt="image" src="https://github.com/user-attachments/assets/45e58547-9ea4-46f9-ad13-b9ed2a1f82aa" />

<img width="1038" height="636" alt="image" src="https://github.com/user-attachments/assets/2533264d-b600-426a-bf52-9ddb02344709" />

<img width="1048" height="707" alt="image" src="https://github.com/user-attachments/assets/4d067bc3-2515-4d90-af10-4569e85e1e86" />

<img width="1283" height="693" alt="image" src="https://github.com/user-attachments/assets/b4571eb5-b398-4bb9-806c-5a502c705acb" />

<img width="1276" height="384" alt="image" src="https://github.com/user-attachments/assets/734e8568-be8f-414e-b967-60532c798a01" />


username: admin
password: `unranked-wisdom-anthem`


---


Browse to `http://MACHINE_IP` from within the AttackBox and try to access the mailbox of the `factory` user to see if the previously harvested `admin` password has been reused on the email portal. What is the total number of toys expected for delivery?

<img width="1288" height="788" alt="image" src="https://github.com/user-attachments/assets/d6d3715a-9dbd-4d9f-b07d-2862af8b7309" />


<img width="1279" height="791" alt="image" src="https://github.com/user-attachments/assets/19211fa3-ad7a-410e-b9d2-fef38713b4be" />

<img width="1295" height="749" alt="image" src="https://github.com/user-attachments/assets/e412e4a2-6fab-47ca-9107-27d5642b32fa" />


<img width="1288" height="727" alt="image" src="https://github.com/user-attachments/assets/bf8aee1a-cec1-4de5-bebe-6d97fffc93ab" />

`1984000`

----

## Key Takeaways

- Social engineering remains one of the most effective attack vectors due to human vulnerability.
    
- Phishing attacks have evolved across multiple communication channels (email, SMS, calls, QR codes).
    
- Credential-harvesting login pages are simple to deploy yet highly dangerous when users aren’t vigilant.
    
- SET provides a streamlined, realistic way to send phishing emails during red-team operations.
    
- Password reuse significantly amplifies the impact of a single compromised account.
    
- Organizations must enforce multi-factor authentication, strong password policies, and continuous awareness training.


---

## Reflection

Day 2 highlighted how powerful and dangerous social engineering attacks can be, especially when combined with well-crafted phishing emails. Walking through the red-team process—from hosting a fake login page to deploying a phishing email via SET—made it clear how easily attackers can trick users with realistic communication and familiar branding. Seeing credentials appear in the terminal reinforced how quickly a single mistake can escalate into a full compromise, especially when users reuse passwords across systems. This exercise underscored the importance of user awareness, secure authentication practices, and the need for organizations to implement proactive defense strategies to counter social engineering threats.
