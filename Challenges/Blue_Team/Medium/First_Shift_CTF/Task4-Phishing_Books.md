
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
Email Analyzer 

 Data
Key 	Value
delivered-to	isabella@kingford.ac.uk
received	by 2002:a05:7022:e2a:b0:119:5273:6922 with SMTP id dx42csp217860dlb; Tue, 4 Nov 2025 06:46:55 -0800 (PST) from mail-sor-f41.kinglord.ac.uk (mail-sor-f41.kinglord.ac.uk. [207.84.120.31]) by mx.kinglord.ac.uk with SMTPS id a640c23a62f3a-b715aee65casor170440966b.13.2025.11.04.06.46.54 for <isabella@kingford.ac.uk> (Google Transport Security); Tue, 04 Nov 2025 06:46:55 -0800 (PST)
x-received	by 2002:a17:907:7241:b0:b70:b13c:3622 with SMTP id a640c23a62f3a-b70b13c60c3mr1024470066b.4.1762267614032; Tue, 04 Nov 2025 06:46:54 -0800 (PST)
arc-seal	i=1; a=rsa-sha256; t=1762267615; cv=none; d=kinglord.ac.uk; s=arc-20240605; b=BHopKeGbF2EaIKddox+LMO08kRWGczIGa1tLrrYAhKwk7WbsnsGS0cQKRJYMqssw6D du5B4U0n5zRiT25iQhXjRASjK6+yjSUdl4Y3yCAsUZ+xJzH+xBP9yYuDxIJlYwcGjfyi 1tC672IOx3RYNBcln5lU9GCm2pIa7aMBlNFdijpg8wPFifTYIDjq9L342ChHkeZ98IwP FL1LyPIQww9TQhr/4u+WgZC4DA932DYHXloX60UM00jPoXxyElOCO5HO4ARS1XqMJf/B ErkI9HI1tMICYSId9mVvLiVh71ScInBVNTgpyNEhWxffPCTUTmvU7jUoHy/1KIsf7gjJ z87Q==
arc-message-signature	i=1; a=rsa-sha256; c=relaxed/relaxed; d=kinglord.ac.uk; s=arc-20240605; h=to:subject:message-id:date:from:mime-version:dkim-signature; bh=eHJyIYXSdwWABXLpJYYVpVmG3UIqRqj1JS3MwUoLrF0=; fh=X/71MsKVOFuVb6a2euNxQ7KQPFXo6IZb4PWyPej0WKA=; b=IsQ89IJN3R2RAnjxPHdl5ykuyq0O6H3lZEgLRGSuNarNCOuDBrbqxdrLq25GLzl8nA I+lXCuUgnv+fskGzCvAZSwtMEw2VAv+OjxMZOjv08hxIC/DJrJFsAP68lpcnKYWKg0Lo 2c1T4ytOuMplRhaLl3fLsWp+BQ3QFENm2N8tEBGdTutPYNsEp/9Nh86KrCoTHrNG5ri/ PfP2zzIKpqkpjm6nIZJ9mt4V00DmI4qIGV8OPxfvXQfem3o3CAPC55Xe7nM/l9e8kvNm /lLMrvpZl7Doht8g/y48T35gDysb02NyIiBS4wfOIVPXzu4+Q26laKJradBnyEW0NLZ+ uCiw==; dara=kinglord.ac.uk
arc-authentication-results	i=1; mx.kinglord.ac.uk; dkim=none (no DKIM signature) header.i=@kinglord.ac.uk header.s=20230601 header.b=mMtxjIwD; spf=none (kinglord.ac.uk: no SPF record) smtp.mailfrom=library@kinglord.ac.uk; dmarc=none (kinglord.ac.uk: no DMARC record) header.from=kinglord.ac.uk; dara=neutral header.i=@kingford.ac.uk
return-path	<library@kinglord.ac.uk>
received-spf	none (kinglord.ac.uk: no SPF record) client-ip=207.84.120.31;
authentication-results	mx.kinglord.ac.uk; dkim=none (no DKIM signature) header.i=@kinglord.ac.uk header.s=20230601 header.b=mMtxjIwD; spf=none (kinglord.ac.uk: no SPF record) smtp.mailfrom=library@kinglord.ac.uk; dmarc=none (kinglord.ac.uk: no DMARC record) header.from=kinglord.ac.uk; dara=neutral header.i=@kingford.ac.uk
dkim-signature	v=1; a=rsa-sha256; c=relaxed/relaxed; d=kinglord.ac.uk; s=20230601; t=1762267614; x=1762872414; darn=kingford.ac.uk; h=to:subject:message-id:date:from:mime-version:from:to:cc:subject :date:message-id:reply-to; bh=eHJyIYXSdwWABXLpJYYVpVmG3UIqRqj1JS3MwUoLrF0=; b=mMtxjIwDKk+Z9m25HVB4O72a+FNfAKg1PS3k1i/mAjUP3W37kIA82wwUUizdedsWoI 10rgbE6PZwMR6ZUPJwCCM/NFS+QXjLRzi7o8FOFMbw0DaKLqy1UC6V6wPkADyMAwGedx 76lt3mK27pLtlpZPEVj56z5Ngwy/ZCAmwv98HCVdj+/MlNH1aQGHZvL1fY0DKyMn6HKv 1TagqsjVcCRuVmmOgOr56F40B34HsYV9Fw4avMwYJ37WekxRK9o+EDRjCGmVdaumStVW oRPXkcypPfwNbHK/ISmM4Rxe8p0i9sR69I6P01akIISgHHbLi4XSY1RwBlCRkLgatMfe ZR0Q==
x-google-dkim-signature	v=1; a=rsa-sha256; c=relaxed/relaxed; d=1e100.net; s=20230601; t=1762267614; x=1762872414; h=to:subject:message-id:date:from:mime-version:x-gm-message-state :from:to:cc:subject:date:message-id:reply-to; bh=eHJyIYXSdwWABXLpJYYVpVmG3UIqRqj1JS3MwUoLrF0=; b=IX0+7hylUTt9mJ8fGaUG0kzRQnKXXBFbzE5Z+pjeBTuuvs/6UqhHl4qB5MLC0OGAEn nCxR50TU5IeJDiM/tX+VUSEb0F+ROcG6Bk4g3t4BfojBXI+RnwEknx+Gw35prrTrb+KK 0QXgfMndl8maJxU4f4JKNFpNcHHvmOmnPXu1JYUodiCn0Mce4CHIyXFDs9HEuHLDGYQJ J1ZSfaq2cRzJI9BTr75n3ahcdO9uvyinnpCqJCcfegI4p5PZbD508qg792mLD83r4FbX JcZ//rm/zzce4Sc2h/jEqIU++h3/Fy3qvF1Wn5GPVoPATHpXtQmCOPXMoT7a8vutM3e9 qMvg==
x-gm-message-state	AOJu0YwZXzDT/HzWnMzpYhlkaJtxBC+TP3t0j4S1z2DsKVWFFa6P7WpUeRS3vXYjjhStj8FVhXop38nBQN76o7P7s2B+4dvKQXGIsAXwecHQiehAEluV2/AuMFR34QGU+rTPO92igOr0CFm+o+HhIcmFp/8mt9j6Qi65FdmojjQ=
x-gm-gg	ASbGncvz2weBUwhhS5BJgKXbuBIfz9AHyEu3yCcq07VTAfZVrLB7vsKqsK0ghn2+Xy1TpOGKNL3bvAI5JcoekRLAU31EdeKU70CPnF254XZreEm3svrshjp3p4fgfxehLwyCz0pd57mPBBOnIdxYLD7gu8YuXRFXl4H2ui4ngK1R7fzIVkEVpYgmUztKwg7IZVvHQqO3vxe1BKp3m91eYvVMmiP/vfnD1k0tnPWLxFxz230JhobTRIWPtXcsHFyLnEnRIJwkvfHSQ2deRLoZJQSNV+pXqtOBjjtiq5P+ucn1o3bBiw==
x-google-smtp-source	AGHT+IEXx8HVx22e7cYRPck0vvw6FrJatiKvUGFmCjoE7F7+6/0VpG7cPtjzDV852FyL8AO7lyeFFJ1uqvfS9YkVmpk=
mime-version	1.0
from	The KingFord University Library <library@kinglord.ac.uk>
date	Tue, 4 Nov 2025 11:46:41 -0300
x-gm-features	AWmQ_blnAvD2WRz3WEvDD-3s6R21vUSmy7KPdkqapMZCwC-ICdT2kKE6c4oEJSU
message-id	<CAMBVi3X8tyPz9brTQcRO5Kvdpv_RGuUOgdAooRgSR+_GNPA+9A@mail.kinglord.ac.uk>
subject	Library Services - Pending Invoice
to	"isabella@kingford.ac.uk" <isabella@kingford.ac.uk>
content-type	multipart/mixed; boundary="000000000000227f970642c5e71e"

Investigation

Links

Data

Key 	Value
1	    http://library.kingford.ac.uk:8001

Investigation
Key 	Value
1	    Virustotal Scan  Urlscan Scan  


Attachments

Data
Key 	Value
1	library-invoice.pdf.html


Investigation
Key 	Value
library-invoice.pdf.html	Name Search Scan(Virustotal)
MD5 Scan(Virustotal)
SHA1 Scan(Virustotal)
SHA256 Scan(Virustotal)


Digests

```
Data
Key 	Value
File MD5	d36d20b736f70ed0b912970fc597c65b
File SHA1	6d2b9264638423a731aa1bec4d8da1ffa3b63aa1
File SHA256	72a6bf2d8170199d4a36484cbc4287bf5fe6362dbf0507aead03b96942dc8c1e
Content MD5	0c0f5e13a82462401b94412b96d1e186
Content SHA1	5e3608e5d122b91289074210e8c190f664786f49
Content SHA256	284dcadc2de4b3dca2544af64af40609e7bfaa8faf7967ce18c10b4f3e6854f6
```

```
Investigation
Key 	Value
File MD5	Virustotal scan
File SHA1	Virustotal scan
File SHA256	Virustotal scan
Content MD5	Virustotal scan
Content SHA1	Virustotal scan
Content SHA256	Virustotal scan
```

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

---


