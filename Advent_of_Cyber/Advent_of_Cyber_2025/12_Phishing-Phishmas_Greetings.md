## Introduction 

---

#### The Story

Task banner for day 12

Since McSkidy’s disappearance, TBFC’s defences have weakened, and now the Email Protection Platform is down.

With filters offline, the staff must triage every suspicious message manually.
The SOC Team suspects Malhare’s Eggsploit Bunnies have sent phishing messages to TBFC’s users to steal credentials and disrupt SOC-mas.

Illustration of an Elf being phished by an Eggsploit Bunny.

You’ve joined the Incident Response Task Force to help identify which emails are legit or phishing attempts.

But beware, some phishing attempts are clever, disguised as routine TBFC operations, volunteer sign-ups, or SOC-mas event logistics. Every wrong call could bring Wareville one step closer to EAST-mas becoming a reality.

#### Learning Objectives
- Spotting phishing emails
- Learn trending phishing techniques
- Understand the differences between spam and phishing

---

## Walkthrough 

**01 Phishing Overview**

Phishing remains one of the most effective cyberattack techniques because it targets human behavior rather than technical weaknesses. Even as organizations like TBFC deploy advanced email filtering and security controls, attackers adapt by crafting highly convincing and targeted messages. These emails often imitate trusted individuals, internal departments, or legitimate services to gain credibility and bypass suspicion.

The main objectives of phishing attacks include stealing credentials, delivering malware, exfiltrating sensitive data, and committing financial fraud. By exploiting trust and familiarity, attackers can gain initial access to systems even when technical defenses are strong.


**02 Understanding the Difference Between Spam and Phishing**

Not all unwanted emails are phishing attempts. Spam is generally mass-distributed and focuses on promotion, marketing, or driving traffic, with little to no direct malicious intent. While spam can be annoying, it usually does not pose an immediate security threat.

Phishing, on the other hand, is intentional and targeted. Its goal is deception—convincing the recipient to take an action that benefits the attacker, such as revealing credentials or approving fraudulent transactions. Identifying the intent behind a message is key to determining whether it is spam or phishing.


**03 Impersonation Techniques**

Impersonation is a common phishing tactic where attackers pretend to be a trusted person, department, or service. This could include managers, IT staff, or security teams. Attackers often rely on display names to appear legitimate, while the actual sender’s email address reveals the deception.

By examining the sender’s domain and comparing it with the organization’s official email structure, impersonation attempts can often be quickly identified. Free or external email domains pretending to represent internal staff are a strong indicator of phishing.


**04 Social Engineering in Phishing**

Social engineering is the foundation of phishing attacks. Rather than exploiting software vulnerabilities, attackers manipulate emotions such as urgency, fear, curiosity, or helpfulness. Emails may pressure users with urgent language, discourage verification through official channels, or create believable scenarios tied to real events.

Common social engineering signals include urgent requests, instructions to bypass standard procedures, and appeals to authority. These techniques are designed to reduce critical thinking and prompt immediate action.


**05 Typosquatting and Punycode Attacks
**
Attackers frequently register domains that closely resemble legitimate ones to deceive users who do not carefully inspect URLs. Typosquatting involves slight misspellings of trusted domains, while punycode abuses Unicode characters that visually resemble standard Latin letters.

These techniques allow malicious domains to appear almost identical to real ones. Reviewing email headers, especially the Return-Path field, can reveal encoded punycode domains and expose the true origin of the message.


**06 Email Spoofing**

Email spoofing occurs when attackers manipulate email headers so that messages appear to come from a trusted domain. While modern email systems use authentication mechanisms like SPF, DKIM, and DMARC to prevent this, failures in these checks are a strong indicator of spoofing.

By reviewing authentication results and comparing them with the Return-Path address, recipients can identify whether an email genuinely originated from the claimed sender or was forged by an attacker.


**07 Malicious Attachments**

Malicious attachments are a classic phishing technique. Attackers disguise harmful files as legitimate documents, voice messages, or invoices. Formats such as HTML or HTA files are especially dangerous because they can execute scripts directly on the system without browser sandbox protections.

Once opened, these attachments can steal credentials, install malware, or provide attackers with deeper access to the device or network.


**08 Trending Phishing Techniques**

As security tools improve at blocking malware attachments, attackers increasingly focus on credential theft rather than direct malware delivery. Modern phishing often redirects users to external platforms, fake login pages, or malicious documents hosted on legitimate services.

This approach shifts responsibility to the user, who unknowingly leaves the secure corporate environment and interacts with attacker-controlled resources.


**09 Abuse of Legitimate Applications**

Attackers commonly abuse trusted services such as OneDrive, Google Drive, or Dropbox to host malicious content. These platforms lend credibility to phishing emails and help bypass security filters.

Phishing messages often include enticing offers, such as salary increases or device upgrades, to lure users into clicking shared links that lead to fake documents or login portals.


**10 Fake Login Pages**

Fake login pages are one of the most effective phishing tools. Attackers clone popular authentication portals, such as Microsoft Office 365 or Google, and host them on deceptive domains. Users who enter their credentials unknowingly hand them directly to attackers.

Carefully checking the URL and domain structure of login pages is essential to detecting these attacks.


**11 Side-Channel Communication**

Side-channel communication occurs when attackers move the conversation away from corporate email into less monitored platforms such as messaging apps, phone calls, or shared document tools. This reduces the organization’s ability to detect or block the attack.

Requests to continue conversations outside official communication channels should always raise suspicion.

---

## Objectives

Classify the 1st email, what's the flag?

<img width="1831" height="894" alt="image" src="https://github.com/user-attachments/assets/fb9009ab-c078-4b3f-9649-9d535cb5dc80" />

Spoofing 
- The SPF, DKIM and DMARC fail and it’s a strong sign the email is spoofed

<img width="1142" height="57" alt="image" src="https://github.com/user-attachments/assets/748696b5-6177-4dc9-b24d-e81f228b2491" />

Sense of Urgency 
- It gives pressure to the recipient

<img width="1209" height="505" alt="image" src="https://github.com/user-attachments/assets/a05b81dd-e2b4-477c-8f6b-49d99d88aa4b" />

Fake Invoice 
- The email trick users into making payments

<img width="1229" height="591" alt="image" src="https://github.com/user-attachments/assets/baf92c07-1f8b-438c-84b0-69143125a5e0" />

Flag:

<img width="1415" height="837" alt="image" src="https://github.com/user-attachments/assets/6a40ef14-c50a-47e5-bb26-dd84fa7b60a0" />

`THM{yougotnumber1-keep-it-going}`


Classify the 2nd email. What's the flag?

<img width="1817" height="888" alt="image" src="https://github.com/user-attachments/assets/6c3e1b60-ab34-404f-9456-4750996ac823" />

Impersonation
- It impersonate McSkidy

<img width="439" height="478" alt="image" src="https://github.com/user-attachments/assets/e3e49f07-e206-4472-a46b-19b948352f28" />


Spoofing 
- The SPF, DKIM and DMARC fail and it’s a strong sign the email is spoofed

<img width="1239" height="47" alt="image" src="https://github.com/user-attachments/assets/e6033b88-2aa5-4a61-ad4c-ae35c9bb0ad1" />

Malicious Attachment 
- It send a malicious attachment

<img width="1303" height="174" alt="image" src="https://github.com/user-attachments/assets/bc6144db-ba99-49fc-8835-307feff74be1" />


Flag: 

<img width="1324" height="784" alt="image" src="https://github.com/user-attachments/assets/76a4a801-e7be-49d4-b5d7-e9bf18987928" />

`THM{nmumber2-was-not-tha-thard!}`


Classify the 3rd email. What's the flag?

<img width="1847" height="890" alt="image" src="https://github.com/user-attachments/assets/2ec3ea8b-eab5-4c5f-846b-36363b217491" />

Impersonation
- It Impersonates McSkidy

<img width="111" height="57" alt="image" src="https://github.com/user-attachments/assets/2ebc2eeb-59ce-4e39-8205-ac602f2706fb" />

Social Engineering Text 
- The email is specific to the target

<img width="789" height="101" alt="image" src="https://github.com/user-attachments/assets/548ca97c-72c9-41ed-ab45-5f2ca2c120bc" />

Sense of Urgency
- It pressure the target

<img width="314" height="72" alt="image" src="https://github.com/user-attachments/assets/d79c27a6-33ff-4d92-bb67-464efb3867cc" />

Flag: 

<img width="1343" height="718" alt="image" src="https://github.com/user-attachments/assets/9b060e9d-322b-4815-9e1d-70388e9b8a8a" />

`THM{Impersonation-is-areal-thing-keepIt}`


Classify the 4th email. What's the flag?

<img width="1844" height="893" alt="image" src="https://github.com/user-attachments/assets/438b83bc-b42e-4dd3-8e7d-1ec53176ffd4" />

Impersonation 
- The sender impersonates the TBFC HR Department

<img width="414" height="110" alt="image" src="https://github.com/user-attachments/assets/18aafab5-e17b-4ed6-b705-acd50f287d56" />

External Sender Domain
- They use Dropbox as a trusted service

<img width="741" height="424" alt="image" src="https://github.com/user-attachments/assets/5601a894-f983-41db-a18e-f6c9ce831ff6" />

Social Engineering Text 
- It uses messages specific to the target

<img width="556" height="189" alt="image" src="https://github.com/user-attachments/assets/70067c8e-d183-4e45-9dbc-f35eba31bcaf" />

Flag: 

<img width="1342" height="666" alt="image" src="https://github.com/user-attachments/assets/6abab8ce-bce7-4af1-84f7-8f32ca9e7cdf" />

`THM{Get-back-SOC-mas!!}`


Classify the 5th email. What's the flag?

<img width="1834" height="889" alt="image" src="https://github.com/user-attachments/assets/82e8494e-a210-4475-8bef-85faf1aed11c" />

It's just a spam that advertise their service 

Flag: 

<img width="1323" height="667" alt="image" src="https://github.com/user-attachments/assets/ba8768f1-db70-416f-ad52-285d56e58389" />

`THM{It-was-just-a-sp4m!!}`


Classify the 6th email. What's the flag?

<img width="1847" height="891" alt="image" src="https://github.com/user-attachments/assets/c7d53697-2749-45b0-82ad-6ba0c558dd15" />


---

## Key Takeaways 

Impersonation
- The sender impersonates the TBFC-IT

<img width="420" height="113" alt="image" src="https://github.com/user-attachments/assets/187f5f0b-27ba-4c64-8da1-eae9e1ab4998" />

Typosquatting/Punycodes
- They use different font for the letter 'f'

<img width="859" height="104" alt="image" src="https://github.com/user-attachments/assets/76b90aae-b59e-4a5a-8908-cb7225d849ba" />

Social Engineering Text 
- Delivers a message specific to the target 

<img width="854" height="603" alt="image" src="https://github.com/user-attachments/assets/1940682e-adae-4806-bccf-b0d11fbd714e" />

Flag: 

<img width="1338" height="633" alt="image" src="https://github.com/user-attachments/assets/49697056-c232-4257-a629-18bfd2b8170d" />

`THM{number6-is-the-last-one!-DX!}`

---

## Key takeaways 

- Phishing is highly effective because it exploits human behavior rather than technical vulnerabilities, making users the primary target when automated defenses are unavailable.

- Not all unwanted emails are malicious; distinguishing spam from phishing depends on identifying intent, where phishing aims to deceive and cause harm, while spam is usually promotional or nuisance-based.

- Impersonation is a core phishing tactic, where attackers pretend to be trusted individuals or departments such as IT, HR, or executives to gain credibility.

- Email spoofing can be identified through failed SPF, DKIM, and DMARC checks, revealing that the sender is not authorized to send emails on behalf of the claimed domain.

- Social engineering techniques such as urgency, fear, authority, and personalization are consistently used to pressure victims into acting without verification.

- Typosquatting and punycode attacks exploit visual similarities in domain names or characters, making malicious domains appear legitimate at a glance.

- Malicious attachments, especially HTML or HTA files, remain dangerous because they can execute scripts directly and bypass browser protections.

- Modern phishing increasingly abuses legitimate platforms like Dropbox or OneDrive to bypass email filters and appear trustworthy.

- Fake login pages are a primary method for credential theft, often mimicking well-known services and relying on deceptive domain names.

- Side-channel communication attempts move conversations away from corporate email into messaging apps or shared platforms, reducing organizational visibility and control.

- Email header analysis, sender domain verification, and careful review of message content are essential skills when automated protections are unavailable.

---

## Reflection

This exercise highlighted how phishing attacks evolve when technical defenses fail and human judgment becomes the last line of defense. Manually triaging emails reinforced the importance of slowing down and critically analyzing sender details, message intent, attachments, and embedded links instead of relying on surface-level trust.

The walkthrough demonstrated that phishing is rarely dependent on a single indicator. Instead, attackers layer multiple techniques—such as impersonation combined with urgency or spoofing paired with malicious attachments—to increase their success rate. Recognizing these patterns made it easier to confidently classify emails and identify genuine threats versus harmless spam.

Overall, the task emphasized that effective defense against phishing is not just about tools, but about awareness, verification, and disciplined analysis. Developing the habit of questioning unexpected requests, inspecting technical details like headers, and understanding attacker psychology is essential for protecting organizations like TBFC from social engineering-driven attacks.



