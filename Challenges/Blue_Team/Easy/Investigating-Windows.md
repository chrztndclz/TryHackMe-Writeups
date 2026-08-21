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


Id=4672






