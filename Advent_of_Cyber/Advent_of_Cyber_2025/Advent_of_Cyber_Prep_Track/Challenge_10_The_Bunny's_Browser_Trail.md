## The Bunny’s Browser Trail

---

## Description

SOCMAS web servers are showing heavy traffic, but one log entry stands out:

“User Agent: BunnyOS/1.0 (HopSecBot)”

Someone or something has infiltrated the system.
User Agent strings help defenders spot automated or suspicious visitors in network logs.

Objective:
Find the unusual User Agent in the HTTP log.

Steps:

Read the provided web log entries.

Compare them to common browsers (Chrome, Firefox, Edge).

Identify and select the suspicious entry.

---

## Analysis

This challenge focuses on identifying unusual user activity by examining HTTP logs. User Agent strings reveal the type of device or software accessing a server, and spotting deviations from common browsers can help detect automated bots or malicious actors.

---

## Methodology

Step 1: Read the HTTP logs

<img width="902" height="676" alt="image" src="https://github.com/user-attachments/assets/9503c6ee-b662-4c70-87de-c880a038ed62" />

Step 2: Select the unusual user agent

Compare it to the common user agent e.g.:
- Chrome
- Linux
- Firefox
- Edge
- iOS

<img width="893" height="667" alt="image" src="https://github.com/user-attachments/assets/99a2a963-99bc-47fc-bdd3-57058bd13374" />

Step 3: Submit and retrieve the flag 

<img width="926" height="253" alt="image" src="https://github.com/user-attachments/assets/acb40339-f7ef-4801-9ffb-372eeaa6821c" />


---

## Flag

THM{Eas-----------ing}

---

## Reflection 

This exercise demonstrates how monitoring HTTP logs and analyzing User Agent strings can quickly reveal unusual or automated activity. Recognizing patterns outside normal browser traffic is a key skill for network defenders.

---

## Tools

(none)
