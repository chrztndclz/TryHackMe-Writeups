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

<img width="957" height="575" alt="image" src="https://github.com/user-attachments/assets/7282499a-583d-4b37-9d94-4bcf314f18d8" />

<img width="967" height="588" alt="image" src="https://github.com/user-attachments/assets/f1bf6367-c8cf-42e3-8bb7-0725b424a6c1" />

<img width="961" height="234" alt="image" src="https://github.com/user-attachments/assets/a8ff4b6c-fcf6-4cbc-bbc1-2aaa1a93c25a" />

Among the linked services, one immediately stood out:
> gravatar.com

This matches the clue from the conversation:
> "Started with a G..."

Gravatar is a free service that lets users upload a profile picture and associate it with their email across many websites, making it a strong candidate for the hidden profile.


#### Step 4: locate the Hidden Profile

Instead of relying solely on Epieos, I visited Gravatar directly to verify the finding.

I navigated to:
> gravatar.com/site/check
and searched using Lambo's email address.

<img width="1119" height="810" alt="image" src="https://github.com/user-attachments/assets/67e6062a-d01c-4a30-a682-10f3fe44060c" />

The search successfully returned a Gravatar profile linked to the email.

I opened the profile.

<img width="738" height="572" alt="image" src="https://github.com/user-attachments/assets/3abe2198-0a86-4d4c-97d2-e81ee63d0f12" />

Inside the profile, I found the hidden information needed for the challenge.

One of the profile fields contained a Base64-encoded string.

I copied the encoded value and opened CyberChef.

<img width="1104" height="536" alt="image" src="https://github.com/user-attachments/assets/24c749f6-9010-4aec-9043-5cc706db75d3" />

Using the From Base64 recipe, I decoded the string and successfully recovered the flag. 

---

### Today's Itinerary 

1. Analyze the provided conversation for Identifying details
> I carefully examined the conversation and extracted valuable information, including the participants, Lambo's email address, and the clue that the hidden profile was hosted on a service starting with the letter "G".

2. Extract the relevant clues
> The most valuable clue was: lambobytelotushotel@gmail.com
This email became the starting point for the OSINT investigation.

4. Locate the hidden account
> Using the email address with OSINT tools, I discovered that it was linked to a Gravatar profile. After inspecting the profile and decoding the Base64 string it contained, I successfully recovered the flag.

5. Submit the flag
> THM{S3creT_Pr0fil3_H4s_b33n_Ident1fi3d}

---

### Flag: 
> THM{S3creT_Pr0fil3_H4s_b33n_Ident1fi3d}

This challenge demonstrates how a small amount of publicly available information can reveal much more than intended. A casual conversation exposed an email address and a subtle hint about a profile-sharing service, which was enough to pivot into an OSINT investigation. By correlating information from the conversation with publicly accessible resources like Gravatar, I was able to locate a hidden profile and recover the flag. It highlights the importance of limiting the personal information shared online, as even seemingly harmless details can be combined to build a much larger picture.
