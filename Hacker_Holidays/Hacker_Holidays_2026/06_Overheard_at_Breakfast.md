# Title: Overheard at Breakfast 

#### Category: OSINT

#### Difficulty: Easy

#### Description: 
Two strangers. One conversation. One profile they never meant to reveal.

---

## Task 1: Hacker Holidays: Day 6

<img width="750" height="661" alt="image" src="https://github.com/user-attachments/assets/cf6cf782-57d7-411b-899e-fbb20cbced34" />

<img width="805" height="578" alt="image" src="https://github.com/user-attachments/assets/f6d999bd-9b57-40b3-9b11-4c3d5df2ea40" />


#### Analysis: 

This challenge focuses on Open Source Intelligence (OSINT)—collecting publicly available information to identify hidden accounts and reveal information that people unintentionally expose online.

The only evidence provided is a screenshot of a conversation, so the first step is to carefully inspect every message for usernames, email addresses, websites, or other identifying information. Small details that seem unimportant can often be used to pivot into other publicly available sources.

---

### Methodology

#### Step 1: Download Task Files
First, I downloaded the task files and extracted them.

The archive contains a single image named:
> conversation.png

I opened it using:
> xdg-open conversation.png

<img width="1211" height="666" alt="image" src="https://github.com/user-attachments/assets/9594bfd4-f009-4d77-a95c-58e2dc3a48c0" />

#### Step 2: Conversation Analysis
Before searching anywhere, I carefully read the conversation to identify useful information.

Who's in the conversation?
Ponzi
Lambo (@0xMia)

I already know Lambo from the previous Hacker Holidays challenges.

What are they talking about?

From the conversation, I gathered several useful clues:

- Lambo is staying at the Byte Lotus Resort.
- Ponzi regularly posts content on social media.
- Lambo mentions using a free service that lets users upload a profile and connect multiple social media accounts.
- She remembers the service starts with the letter "G".
- She also accidentally reveals her email address:
  > lambobytelotushotel@gmail.com

This email becomes my primary OSINT pivot point.


#### Step 3: Investigate the Email Address

Given only an email address, I wanted to determine which online services were associated with it.

One useful OSINT tool for this is Epieos, which checks what public services are linked to an email address.

I searched the email:
> lambobytelotushotel@gmail.com

<img width="1638" height="483" alt="image" src="https://github.com/user-attachments/assets/ec17a3c8-8bf7-4cb8-ae55-9cf174a160f3" />

After downloading the report, I reviewed the results.

<img width="976" height="473" alt="image" src="https://github.com/user-attachments/assets/5765b459-e445-4878-b029-1d246a4a449f" />

We can see that there's a website included named "gravatar.com" 

<img width="957" height="575" alt="image" src="https://github.com/user-attachments/assets/7282499a-583d-4b37-9d94-4bcf314f18d8" />

<img width="967" height="588" alt="image" src="https://github.com/user-attachments/assets/f1bf6367-c8cf-42e3-8bb7-0725b424a6c1" />

<img width="961" height="234" alt="image" src="https://github.com/user-attachments/assets/a8ff4b6c-fcf6-4cbc-bbc1-2aaa1a93c25a" />

We can't see his profile in gravatar because we need to subscribe


Let's visit the gravatar.com and inspect 

Upon reviewing we got to know that we can search profile in gravatar using emails. Besides its functionality that we can uploade profile pic to every website we registered on, etc. 

Navigate to Gravatar.com/site/check paste the Email of lambo

<img width="1119" height="810" alt="image" src="https://github.com/user-attachments/assets/67e6062a-d01c-4a30-a682-10f3fe44060c" />

Navigate his profile URL 

<img width="738" height="572" alt="image" src="https://github.com/user-attachments/assets/3abe2198-0a86-4d4c-97d2-e81ee63d0f12" />

We know find the secret profile we are looking for. 

Copy the base64 string and paste in cybechef

<img width="1104" height="536" alt="image" src="https://github.com/user-attachments/assets/24c749f6-9010-4aec-9043-5cc706db75d3" />

Retrieve the flag. 

---

We need more powerful OSINT tool 

Holehe
Ghunt





---


### Today's Itinerary 

1. Analyze the provided conversation for Identifying details

2. Extract the relevant clues

3. Locate the hidden account

4. Submit the flag


---

### Flag: 
> THM{S3creT_Pr0fil3_H4s_b33n_Ident1fi3d}
