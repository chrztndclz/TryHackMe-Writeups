# Title: Do Not Disturb

#### Category: Boot2Root

#### Difficulty: Medium

#### Description: 
Sign's on the door. Room's active. You have access you were never given, and so does he. 

---

## Task 1: Hacker Holidays Storyline: Act 2 - Drift 

<img width="827" height="669" alt="image" src="https://github.com/user-attachments/assets/4db27624-ded8-4e6f-974c-3254938b1e29" />

<img width="834" height="520" alt="image" src="https://github.com/user-attachments/assets/419e5b5c-ade1-41d5-8907-d1873256156d" />

#### Analysis: 

The story indicates that the Byte Lotus environment is progressing from individual vulnerabilities to a full-scale compromise. Instead of exploiting a single application, the attacker appears to have established persistent access, moved through the environment, hijacked authenticated sessions, and performed unauthorized actions such as cryptocurrency theft. The next set of challenges will likely focus on incident response, authentication logs, session analysis, and tracing the attacker's movement throughout the hotel's infrastructure.

---

## Task 2: Hacker Holidays: Day 7

<img width="570" height="676" alt="image" src="https://github.com/user-attachments/assets/e20bb8c9-0c89-4951-98ec-84d1c950391c" />

Analysis:

This challenge appears to focus on web exploitation and privilege escalation. The briefing suggests that an attacker has already gained unauthorized access by abusing an active session. Our goal is to investigate the web application, follow the attacker's traces, obtain the user flag, and eventually escalate privileges to recover the root flag.


---

### Methodology

Access the machine 

<img width="1909" height="701" alt="image" src="https://github.com/user-attachments/assets/2fa45c06-5b76-43cf-a5c3-8dadba96b880" />


Inspect

<img width="1904" height="729" alt="image" src="https://github.com/user-attachments/assets/5b4111fa-8224-4187-8848-9077ff363743" />

---


NoSQL Injection (Auth Bypass)
Got a cookie session 

Login using the cookie as staff 

Do 1 command that contain the cookie session login ssti and the listening for a reverse shell 
 

---


 Use Burpsuite, intercept on

User: Attendant (this gets you the role of staff)
Password: (Anything)


Return the a POST REQUEST

Go to repeater 

It accepts mongoDB parameters 

Go back to proxy and forward it 

Go back to the browser and you'll see that we signed in as a staff 


Reverse Shell is already prepared 

Paste the reverse shell in the textbox and listen using netcat 

Go to burpsuite and forward it again 

go back to the terminal and yo'll see that you are already in shell


Find user flag 

paste the payload for accessing root use a node.js 

You'll be given the shell with root access 

Find the root flag 

use a command to access to root.txt 


Finding the exploit bypass auth using mongodb 

The only account that give a permission as staff and not guest is the attendant so Its usually a trial and error becuase you need to guest it becaus so
sometimes you are using admin, guest, etc. it will just gives you a guest permission 

You need to fins the specific user for you to be a staff and not a guest



Run reverse shell to access the user level 

Got access Raw disk access 

Used to do reverse shell to access the root level and got our root.txt 
