## Introduction
 
---

#### The Story

After last year's automation and tech modernisation, Santa's workshop got a new makeover. McSkidy has a secure message portal where you can contact her directly with any questions or concerns. However, lately, the logs have been lighting up with unusual activity, ranging from odd messages to suspicious search terms. Even Santa's letters appear to be scripts or random code. Your mission, should you choose to accept it: dig through the logs, uncover the mischief, and figure out who's trying to mess with McSkidy. 

Learning Objectives
- Understand how XSS works
- Learn to prevent XSS attacks

---

## Walkthrough

**01 Equipment Check**

For this activity, the target web application is accessed through the internal address http://10.49.139.49 using the browser available in the AttackBox. Once loaded, the portal page serves as the environment for observing and testing common web vulnerabilities. Before attempting any exploitation, it is important to understand how user input is handled and rendered by the application, as this directly affects its exposure to client-side attacks such as Cross-Site Scripting (XSS).

XSS is a vulnerability that occurs when a web application accepts user input and displays it back to users without proper validation, sanitization, or encoding. When this happens, browsers may interpret injected input as executable JavaScript instead of plain text, allowing attackers to run malicious scripts in the context of the victim’s session.

**02 Reflected XSS**

Reflected XSS occurs when user-supplied input is immediately returned by the server in an HTTP response without being stored. This type of XSS is typically triggered through crafted URLs or form inputs and often relies on social engineering to trick victims into clicking malicious links.

In a reflected XSS scenario, the malicious script is embedded in a request parameter, such as a search query. When the server processes the request and reflects the input directly back into the response page, the browser executes the injected script. The impact is immediate and affects only users who interact with the malicious request, making it commonly used in phishing-style attacks.

**03 Stored XSS**

Stored XSS is more severe because the injected malicious code is saved on the server, usually in a database. Any user who later accesses the affected page will unknowingly execute the attacker’s script. Unlike reflected XSS, this attack does not require victims to click a specific link, as the payload is permanently embedded in application content.

A common example is a comment or message feature where user input is stored and displayed to others. If the application does not sanitize inputs before saving them, an attacker can submit JavaScript code instead of normal text. Each time the page is loaded, the script runs automatically, enabling actions such as session hijacking, data theft, or page defacement.

**04 Protecting against XSS**

Preventing XSS vulnerabilities requires careful design and secure coding practices. One effective approach is to avoid dangerous rendering methods such as innerHTML and instead use safer alternatives like textContent, which treats input strictly as text. Additionally, cookies should be configured with HttpOnly, Secure, and SameSite attributes to limit access from client-side scripts.

Input sanitization and output encoding are critical defenses. Applications that allow limited HTML formatting must ensure that only safe tags and attributes are permitted, while removing or escaping scripts, event handlers, and JavaScript URLs. By validating and encoding all user-controlled data before rendering it in the browser, applications significantly reduce the risk of XSS exploitation.

**05 Exploiting Reflected XSS**

To test for reflected XSS in the portal, an input field is required where user data is reflected back in the response. The search functionality provides such an entry point. By injecting a simple JavaScript payload like <script>alert('Reflected Meow Meow')</script> into the search bar and submitting the query, the application’s behavior can be observed.

If an alert box appears, it confirms that the input was not properly encoded and was executed by the browser. This demonstrates that the search parameter is vulnerable to reflected XSS. Further confirmation can be obtained by reviewing the System Logs, which show how the application processes and records the injected input. Once this behavior is identified, the same methodology can be extended to test other inputs, such as message submission forms, to determine whether stored XSS is also present.

---

## Objective 

Which type of XSS attack requires payloads to be persisted on the backend?

Answer: stored

What's the reflected XSS flag?

<img width="1340" height="815" alt="image" src="https://github.com/user-attachments/assets/5fd730e0-257f-45ff-9b52-284120d907f4" />

Type this script: 

`<script>alert('Reflected XSS')</script>`

Press "Search Messages" Button 

<img width="1317" height="768" alt="image" src="https://github.com/user-attachments/assets/c4738f95-46ec-48f5-bf62-7457cf143711" />

What's the stored XSS flag?

<img width="1346" height="839" alt="image" src="https://github.com/user-attachments/assets/2ed65fb2-bc21-47d9-874c-4cb472d8f2d4" />

Type this script: 

`<script>alert('Stored Meow Meow')</script>`

Press "Send Message" Button 

<img width="1349" height="817" alt="image" src="https://github.com/user-attachments/assets/2b38d00f-c5cf-402b-a455-ab093bbaa371" />

---

## Key Takeaways

- Cross-Site Scripting (XSS) occurs when user input is rendered by a web application without proper validation, sanitization, or encoding, allowing browsers to execute injected JavaScript as trusted code.

- Reflected XSS executes immediately and typically relies on user interaction, such as clicking a crafted link or submitting a malicious search query.

- Stored XSS is more dangerous because the malicious payload is saved on the backend and automatically executes for every user who accesses the affected content.

- Simple input fields like search bars and message forms can become attack vectors if they reflect or store user input without security controls.

- Safe rendering practices, such as using textContent instead of innerHTML, significantly reduce the risk of XSS.

- Proper input sanitization and output encoding are essential, especially for applications that allow limited HTML formatting.

- Cookie security attributes (HttpOnly, Secure, and SameSite) help minimize the impact of successful XSS attacks by protecting session data.

- System logs are critical for detecting suspicious behavior, confirming exploitation attempts, and supporting incident investigation and response.

---

## Reflection

This walkthrough provided practical insight into how XSS vulnerabilities manifest in real-world web applications and why they remain a common attack vector. Seeing JavaScript payloads execute through both the search functionality and the message submission feature reinforces how small oversights in input handling can lead to significant security issues.

The hands-on exploitation process made the theoretical concepts much clearer, especially the difference in persistence and impact between reflected and stored XSS. It emphasized that stored XSS is particularly dangerous due to its ability to affect multiple users without additional attacker interaction.

Overall, the task highlights the importance of secure-by-design development. Preventing XSS is not achieved through a single fix but through a combination of safe coding practices, proper configuration, and continuous monitoring. This exercise serves as a reminder that even internal or seemingly simple applications must be treated as potential attack surfaces and secured accordingly.

