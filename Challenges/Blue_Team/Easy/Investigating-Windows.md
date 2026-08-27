# Title: # Title: Investigating Windows 

#### Category: 

#### Difficulty: Easy

#### Description: 

A windows machine has been hacked, its your job to go investigate this windows machine and find clues to what the hacker might have done.

---

## Task 1:  Investigating Windows 

This is a challenge that is exactly what is says on the tin, there are a few challenges around investigating a windows machine that has been previously compromised.

Connect to the machine using RDP. The credentials the machine are as follows:

Username: Administrator
Password: letmein123!

---

### Questions: 

---

#### Whats the version and year of the windows machine?

First, press Win + R to open the Run dialog and type "winever

<img width="451" height="247" alt="image" src="https://github.com/user-attachments/assets/0ee52f1e-8d66-4dcf-b8c3-774060426377" />

This will give you the "About Windows" message box 

<img width="500" height="435" alt="image" src="https://github.com/user-attachments/assets/8cff7408-6650-41b1-a5fa-899b2fb429d1" />

Answer: 
> Windows server 2016

---

#### Which user logged in last? 

Run: 
```
quser
```

This shows users who are currently logged in, but it may not tell you the previous login. The user currently logged is the one who logged last. 

<img width="731" height="109" alt="image" src="https://github.com/user-attachments/assets/f38ca70b-517b-4e21-9400-a89a50232fa8" />

Answer:
> administrator

---

#### When did John log onto the system last?

Answer format: MM/DD/YYYY H:MM:SS AM/PM

Go to command prompt and type 

```
net user
```

net user is a built-in Windows command used to view and manage local user accounts.

if you see John, run this command: 

```
net user John
```

This will give all of the information about John 

<img width="458" height="485" alt="image" src="https://github.com/user-attachments/assets/95328e1f-7fa6-4778-95f1-ec250db0097d" />


Check the last logon of the account and that's the answer 

Answer:
> 03/02/2019 5:48:32 PM

---

#### What IP does the system connect to when it first starts?

Run to identify programs configured to run automatically when Windows starts.

Run:

```
wmic startup get Caption,Command,Location
```

Among the startup entries, UpdateSvc executes:
 
> C:\TMP\p.exe -s \\10.34.2.3 'net user' > C:\TMP\o2.txt

<img width="1053" height="179" alt="image" src="https://github.com/user-attachments/assets/4dda225a-2274-42a9-bb2b-2fb3ece2d0cd" />

Answer:
> 10.34.2.3

---

#### What two accounts had administrative privileges (other than the Administrator user)?

Answer format: List them in alphabetical order.

In Command Prompt, run:

```
net localgroup Administrators
```

The accounts listed under Members are the accounts with administrative privileges.

<img width="798" height="229" alt="image" src="https://github.com/user-attachments/assets/0b0baa32-1f66-42b2-8757-9ba941a0a1fa" />

Here we can see the accounts that have administrative privileges

Answer:
> Guest, Jenny

---


#### Whats the name of the scheduled task that is malicous.

Run: 
```
schtasks /query /fo LIST /v
```
This gives you detailed information for all scheduled tasks

Upon looking we stumbled upon 

> Clean file system

<img width="771" height="519" alt="image" src="https://github.com/user-attachments/assets/29432b07-6a6b-4d01-9f44-f1560ff4c6eb" />


The task appears malicious because it runs C:\TMP\nc.ps1 -l 1348, a PowerShell Netcat-style script listening on port 1348, from the unusual C:\TMP directory and executes daily with Administrator privileges, which is not normal behavior for a Windows cleanup task.

Answer:
> Clean file system

---

#### What file was the task trying to run daily?

Along side the malicious scheduled task you can notice that it executes daily 

The file responsible for that is the one in the "Task To Run" 

<img width="786" height="513" alt="image" src="https://github.com/user-attachments/assets/42f7cb52-0906-4e13-82b3-66ba3736490e" />


Answer: 
> nc.ps1

---

#### What port did this file listen locally for?

In the same malicious scheduled task 

We can see the file the it listens to port 1348

<img width="640" height="62" alt="image" src="https://github.com/user-attachments/assets/6ce441c4-4df1-40d3-a7a3-e62a85606bac" />

Answer: 
> 1348

---

#### When did Jenny last logon?

Just like John to see Jenny's info let's run 

```
net user Jenny
```

And lets look at the Last logon part, as we notice Jenny never logon 

<img width="545" height="547" alt="image" src="https://github.com/user-attachments/assets/31e8aa73-3f41-4fed-a323-901d0e112257" />

Answer: 
> Jenny 


---

At what date did the compromise take place?

Answer format: MM/DD/YYYY

The date did the compromise take place is the start date of the file nc.ps1 in the malicious scheduled task 

<img width="786" height="513" alt="image" src="https://github.com/user-attachments/assets/42f7cb52-0906-4e13-82b3-66ba3736490e" />


Answer:
> 03/02/2019

---

#### During the compromise, at what time did Windows first assign special privileges to a new logon?

Answer format: MM/DD/YYYY HH:MM:SS AM/PM

During the compromise, at what time did Windows first assign special privileges to a new logon?

To find this, I opened Event Viewer by pressing Win + R and running:

> eventvwr

I then navigated to:

Windows Logs → Security

The question mentions that Windows assigned special privileges to a new logon, which corresponds to Event ID 4672.

There were a lot of events with Event ID 4672, around 325 entries, so checking every event manually would take a while. I used the hint provided by the challenge, which stated that the correct time ends with :49 seconds and occurs in the PM.

I went through the Event ID 4672 entries and looked at their timestamps until I found the event matching the hint. After opening the event, I confirmed that it was the "Special privileges assigned to new logon" event.

The timestamp shown for this event gives the answer.

<img width="1552" height="281" alt="image" src="https://github.com/user-attachments/assets/ee43af1b-7d4b-4213-82ba-76caf8483362" />


---

#### What tool was used to get Windows passwords?

To find the tool used to obtain Windows passwords, I first checked the suspicious C:\TMP directory that we had already identified during the investigation.

I ran:

dir C:\TMP

Looking through the files, I noticed two files that stood out:

```
mim.exe
mim-out.txt
```

<img width="612" height="633" alt="image" src="https://github.com/user-attachments/assets/ea7b8927-be62-4186-afee-54d5c01960ac" />


The file mim.exe is suspicious because its name is commonly used as a shortened name for Mimikatz. The accompanying mim-out.txt also suggests that the tool's output was saved to a text file.

This gives us a strong indication that the attacker used Mimikatz to obtain Windows credentials.

What is Mimikatz?

Mimikatz is a Windows post-exploitation and credential-dumping tool. Attackers can use it to extract credentials and authentication information from a compromised Windows system, including passwords, password hashes, and Kerberos-related credentials.

It is commonly used during penetration testing and security research, but it is also frequently abused by attackers after gaining access to a Windows machine.

In this investigation, the presence of mim.exe and mim-out.txt in C:\TMP provides evidence that Mimikatz was used during the compromise.

Answer:
> Mimikatz

---

#### What was the attackers external control and command servers IP?

Since this is a Windows machine, I first checked common network configuration locations. One important location is:

C:\Windows\System32\drivers\etc\

This directory contains the hosts file, which Windows uses to map domain names to IP addresses. Attackers can modify this file to redirect traffic or block security services.

I opened the hosts file and looked for unusual entries. I found:

```
127.0.0.1    www.virustotal.com
127.0.0.1    dci.sophosupd.com

76.32.97.132    google.com
76.32.97.132    www.google.com
```

The last two entries stood out because they redirect google.com and www.google.com to the external IP 76.32.97.132.

This makes 76.32.97.132 the suspected attacker C2 IP.

<img width="835" height="681" alt="image" src="https://github.com/user-attachments/assets/b8d08956-17be-415f-9279-5e3dbc5c136d" />

Answer: 
> 76.32.97.132


---

#### What was the extension name of the shell uploaded via the servers website?

To answer this question, I looked at the website's files. Since the server is using IIS, a common web directory to check is:

C:\inetpub\wwwroot\

I listed the files with:

dir C:\inetpub\wwwroot\

The output showed:

```
74,853    b.jsp
12,572    shell.gif
657       tests.jsp
```

The important part is the file extensions.

I found two files ending in .jsp:

```
b.jsp
tests.jsp
```

<img width="543" height="313" alt="image" src="https://github.com/user-attachments/assets/58932b06-5f16-4683-b286-0b724c1e8e32" />


The question asks for the extension name of the shell uploaded via the website. Since the server contains JSP files and the suspicious file is b.jsp, the shell is using the .jsp extension.

JSP stands for JavaServer Pages and is a server-side technology that can execute Java code through a Java web server. This makes .jsp a possible extension for a web shell.

Answer
> .jsp


---

#### What was the last port the attacker opened?

To investigate the Windows firewall rules, I used the following command:

netsh advfirewall firewall show rule name=all

This command displays all firewall rules, so there were a lot of entries to go through.

I manually checked the rules and looked for the indicators that would make a rule relevant to the question.

I found this rule:

```
Rule Name:   Allow outside connections for development
Enabled:     Yes
Direction:   In
Profiles:    Public
Protocol:    TCP
LocalPort:   1337
Action:      Allow
```

The important indicators are:

Direction: In → allows incoming connections.
Protocol: TCP → specifies the network protocol.
LocalPort: 1337 → identifies the port being opened.
Action: Allow → the firewall permits connections to that port.
Enabled: Yes → the rule is active.

Because this rule allows incoming TCP connections on port 1337, this is the port I was looking for.

<img width="802" height="337" alt="image" src="https://github.com/user-attachments/assets/69d603e1-5a55-4f83-8ddf-6126b776daa3" />

Answer:
> 1337

---

#### Check for DNS poisoning, what site was targeted?

DNS poisoning is when an attacker manipulates DNS information so a domain name points to the wrong IP address.

To check for DNS poisoning, I went back to the Windows hosts file:

> C:\Windows\System32\drivers\etc\hosts

The hosts file is important because Windows can use it to manually map a domain name to an IP address, bypassing normal DNS resolution.

I looked for suspicious domain-to-IP mappings and found:

```
76.32.97.132    google.com
76.32.97.132    www.google.com
```

This is suspicious because both Google domains are being redirected to the same external IP address:

> google.com → 76.32.97.132

This indicates that the attacker modified the hostname resolution so that requests intended for Google could instead be sent to the suspicious IP.

Answer:
> google.com 


