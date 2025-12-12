## Introduction
 
---

#### The Story

With time between Easter and Christmas being destabilised, the once-quiet systems of The Best Festival Company began showing traces of encrypted data buried deep within their servers. Sir Carrotbane, stumbled upon a series of locked PDF and ZIP files labelled “North Pole Asset List.” Rumours spread that these could contain fragments of Santa’s master gift registry, critical information that could help Malhare control the festive balance between both worlds.

Sir Carrotbane sets out to crack the encryption, learning how weak passwords can expose even the most guarded secrets. Can the Elves adapt fast and prevent their secrets from being discovered?

#### Learning Objectives

- How password-based encryption protects files such as PDFs and ZIP archives.
- Why weak passwords make encrypted files vulnerable.
- How attackers use dictionary and brute-force attacks to recover passwords.
- A hands-on exercise: cracking the password of an encrypted file to reveal its contents.
- The importance of using strong, complex passwords to defend against these attacks.
- A few simple points to remember:

- The strength of protection depends almost entirely on the password. Short or common passwords can be guessed; long, random passwords are far harder to break.
- Different file formats use different algorithms and key derivation methods. For example, PDF encryption and ZIP encryption differ in details (how the key is derived, salt use, number of hash iterations). That affects how easy or hard cracking is.
- Many consumer tools still support legacy or weak modes (particularly older ZIP encryption). That makes some encrypted archives much easier to attack than modern, well-implemented schemes.
- Encryption protects data confidentiality only. It does not prevent someone with access to the encrypted file from trying to guess the password offline.

To make it simple, encryption makes the contents unreadable unless the correct password is known. If the password is weak, an attacker can simply try likely passwords until one works.

---

## Walkthrough 

**01 How Attackers Recover Weak Passwords**

Attackers don’t break encryption — they guess the password.
They start with dictionary attacks, trying thousands of common or leaked passwords until one works.

If that fails, they use mask/brute-force attacks, testing structured patterns (e.g., letters + numbers).

They often use GPU-powered tools like john or hashcat to speed up cracking weak passwords.



**02 Exercise**

The task is to check what type of protected file you have and try to recover its password.

Identify the file type using the file command (PDF or ZIP).

Pick the right tool:

- PDF → pdfcrack or John (via pdf2john)
- ZIP → fcrackzip or John (via zip2john)

Run a dictionary attack using rockyou.txt — this is usually enough for weak passwords.

If successful, the tool prints the recovered password and you can open the file.



**03 Detection of Indicators and Telemetry**

Password cracking is offline but leaves clear traces on a machine.

- Processes: Tools like john, hashcat, pdfcrack, zip2john show up in process logs.
- Commands: Flags like --wordlist, --mask, -a 3, and references to rockyou.txt are strong indicators.
- Files: potfiles and repeated reads of wordlists or encrypted files stand out.
- GPU usage: Hashcat causes high, steady GPU load visible in monitoring tools.
- Network: Attackers may download tools or wordlists before cracking.

If detected, analysts isolate the host, collect evidence, check what was decrypted, and escalate if unauthorized.

---

## Objective 

What is the flag inside the encrypted PDF?

Command: `pdfcrack -f flag.pdf -w /usr/share/wordlists/rockyou.txt`

<img width="1351" height="846" alt="image" src="https://github.com/user-attachments/assets/ed171f69-116d-492b-9be9-548e9a7f781e" />

Password: `naughtylist`

Open the pdf file using the password

<img width="1355" height="821" alt="image" src="https://github.com/user-attachments/assets/0410ce57-128f-4b62-83ba-47d8232dfd00" />

<img width="1350" height="839" alt="image" src="https://github.com/user-attachments/assets/96c5befd-71b5-43f2-8ea0-439a12b3dc50" />


What is the flag inside the encrypted zip file?

zip2john flag.zip > ziphash.txt     

john --wordlist=/usr/share/wordlists/rockyou.txt ziphash.txt

<img width="1335" height="809" alt="image" src="https://github.com/user-attachments/assets/06b40ff6-9c44-4973-98e2-082d76b8239a" />

Open the zip file using the password

Password: `winter4ever`

<img width="1360" height="801" alt="image" src="https://github.com/user-attachments/assets/247e4dc1-e7c3-4e61-ad1a-e58c4c406a0b" />

<img width="1359" height="844" alt="image" src="https://github.com/user-attachments/assets/78e1ab87-6a6c-4144-83e9-6695ae9e6e0f" />

--- 

## Key Takeaways

- Encryption is only as strong as the password that protects it. Weak or common passwords make “encrypted” files easy to crack.

- Attackers rely on password-guessing techniques, not breaking encryption algorithms. Dictionary attacks and mask/brute-force patterns are the most common.

- Tools like pdfcrack, fcrackzip, john, and hashcat make password recovery fast — especially when using popular wordlists like rockyou.txt.

- Identifying the file type first ensures the right cracking tool is used (PDF vs ZIP).

- Weak passwords like naughtylist and winter4ever highlight how predictable patterns expose sensitive files.

- Cracking activity leaves detectable traces: unusual processes, high GPU usage, repeated wordlist reads, and tool command lines.

- Strong, long, random passwords remain the simplest and most effective defence against offline cracking.

---

## Reflection

This activity shows how easily encrypted files can be compromised when weak passwords are involved. Even though the files were protected, the actual security depended entirely on the strength of the password. By running basic dictionary attacks, both the PDF and ZIP were cracked quickly, proving how dangerous predictable words and seasonal themes can be.

From a defensive perspective, it reinforces the need for strict password policies and monitoring systems that can spot cracking attempts. For attackers, this exercise demonstrates how straightforward password recovery can be when poor password hygiene is in place. For defenders, it’s a reminder that encryption alone is not enough — strong passwords, MFA, and monitoring are essential to keep sensitive data protected.
