
## Phishing Books 

It's another typical day at ProbablyFine Ltd. Your SOC dashboard is glowing with endless alerts, most of them false positives, as usual. Your team manages several education-sector clients, including universities, schools, and research institutes across the UK. Today, you are in charge of monitoring alerts from universities in London.

Normally, things stay quiet. These universities are very targeted by phishing attacks, but most attempts get stopped by the email filters before anyone even sees them. But today is different. You got an email from a university teacher:

```
Subject: MFA Removal Requests
From: Dr. Isabella <isabella@kingford.ac.uk>

Hey, ProbablyFine SOC Team,
I've been getting several emails asking me to approve my MFA.
Are you performing any tests? Should I approve these requests?
Dr. Isabella
```

You contact Dr. Isabella directly, and it becomes clear that she has been targeted by a phishing email designed to steal her credentials, which is why she is receiving multiple MFA requests! You advise her to reset her password immediately.

Now it's time to dig deeper: No alerts were triggered in your SIEM, so you requested the original .eml file of the phishing email to perform a manual investigation. Was this an isolated hit, or part of a larger phishing campaign targeting universities? Start the analysis machine and examine the email. Let's see what’s really going on!

---

Library Services - Pending Invoice.eml

From:	Kingford University Library <library@kinglord.ac.uk>
To:	isabella@kingford.ac.uk <isabella@kingford.ac.uk>
Subject:	Library Services - Pending Invoice
Date:	Tue, 4 Nov 2025 11:46:41 -0300 (11/04/25 14:46:41)

Dear Isabella,

Please be informed that you have a pending invoice with The Kingford University Libraries System, which will expire soon. Your library enrollment is set to expire on December 30, 2025, at 12:00 PM, so this is a reminder to complete your payment as soon as possible.

To make your payment, please open the file attached to this email and log in to The Kingford University Libraries System.

You will not be required to provide any personal data during this payment process.

Please note that the invoice renewal link is only valid for a limited time. If you fail to renew your library enrollment before the deadline, you will lose access to all online library services. For a full list of available services, please visit:
library.kingford.ac.uk

If you have any questions regarding your account status or access to library services, please contact the Library Help Desk as soon as possible.

Sincerely,
Library Services
The Kingford University Libraries System
📧 helpdesk@kingford.ac.uk
🌐 library.kingford.ac.uk

---

Which specific check within the headers explains the bypass of email filters?
Answer Example: "CHECK=value"

<img width="1168" height="451" alt="image" src="https://github.com/user-attachments/assets/ee39fb04-a13c-44ac-9c27-20d2d81db973" />

The email bypassed filters because the sender domain had DMARC=none, meaning there was no policy instructing the mail server to reject or quarantine messages that failed SPF and DKIM checks.

`Answer: DMARC=none`

---

What technique did the attacker use to make the message seem legitimate?

<img width="420" height="133" alt="image" src="https://github.com/user-attachments/assets/659b90fa-7ebb-4e0e-b96f-4ab6386a31fc" />

The attacker made the message seem legitimate by impersonating a trusted university service using a look-alike domain (domain spoofing/typosquatting).

`Answer: Typosquatting`

---

Which MITRE technique and sub-technique ID best fit this sender address trick?

<img width="1106" height="413" alt="image" src="https://github.com/user-attachments/assets/f0e13f27-2b7f-43f2-8863-3bac78c83d8d" />

Because the attacker registered and used a look-alike domain (kinglord.ac.uk) to impersonate a trusted sender, which aligns with T1583.001 – Acquire Infrastructure: Domains.

`Answer: T1583.001`

---

What is the file extension of the attached file?

<img width="743" height="289" alt="image" src="https://github.com/user-attachments/assets/c25b94d5-363e-4f05-99cb-82c345fe254f" />

`Answer: .html`

---


What is the MD5 hash of the .HTML file?

Save the  .html file
`md5sum library-invoice.pdf.html`

<img width="890" height="64" alt="image" src="https://github.com/user-attachments/assets/a89dc654-7a87-47d6-84a2-7fd583060908" />

`Answer: 442f2965cb6e9147da7908bb4eb73a72`

---

What is the landing page of the phishing attack?

<img width="1298" height="715" alt="image" src="https://github.com/user-attachments/assets/6a781596-ad4f-4076-9cef-c5ccfba5cbef" />

`Answer: http://lib-service.com:8083/`

---

Which MITRE technique ID was used inside the attached file?

`nano library-invoice.pdf.html`

<img width="1108" height="519" alt="image" src="https://github.com/user-attachments/assets/ec9db5ff-0874-4ff3-a98d-4e652f8de04c" />

The JavaScript uses Unicode string encoding to conceal a malicious URL and decode it at runtime, which matches T1027 – Obfuscated/Compressed Files and Information.

`Answer: T1027`

---

What is the hidden message the attacker left in the file?

from var egassem
<img width="1131" height="531" alt="image" src="https://github.com/user-attachments/assets/3b1ab193-0873-478f-9ad4-9d8c1c3eb273" />

<img width="1529" height="431" alt="image" src="https://github.com/user-attachments/assets/f26f87ba-3427-4145-a160-70867b0881ca" />

`Answer: I love to phish books  from libraries ^^`

---

Which line in the attached file is responsible for decoding the URL redirect?

<img width="1138" height="561" alt="image" src="https://github.com/user-attachments/assets/4fe54466-9696-4852-b1b6-c5ece7772d8a" />

`Answer: var src = reversed.split("").reverse().join("");`

---

What is the first URL in the redirect chain?

Get the target link 
<img width="1153" height="300" alt="image" src="https://github.com/user-attachments/assets/bf88cc7c-a318-4f20-bc47-76575959d05f" />

Paste and inspect the browser then go to the network tab. 

<img width="1387" height="569" alt="image" src="https://github.com/user-attachments/assets/fe27fef2-b604-432b-b20c-c41e5055b00d" />

`Answer: http://xn--librarytlu-13cwe32432-kwr.com:8082`

---

What is the Threat Actor associated with this malicious file and/or URL?

Go to "https://static-labs.tryhackme.cloud/apps/trydetectthis/report/lib-service.com"

<img width="1904" height="581" alt="image" src="https://github.com/user-attachments/assets/a7363639-5c36-40e9-bd62-dc5c2d7b1cf3" />

`Answer: Cobalt Dickens | Silent Librarian`

---

What is the main target of this Threat Actor according to MITRE?

Search "Dickens | Silent Librarian" to the mitre.org

<img width="1664" height="526" alt="image" src="https://github.com/user-attachments/assets/d96f03c7-50d0-48d3-aca4-4c77847e353e" />

`Answer: research and proprietary data`




