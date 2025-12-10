## Introduction

---

#### The Story

Sir BreachBlocker III has corrupted the Christmas Calendar AI agent in Wareville. Instead of showing the Christmas event, the calendar shows Easter, confusing the people in Wareville.
It seems that without McSkidy, the only way to restore order is to reset the calendar to its original Christmas state. But the AI agent is locked down with developer tokens.
To help Weareville, you must counterattack and exploit the agent to reset the calendar back to Christmas.

Learning Objectives

- Understand how agentic AI works
- Recognize security risks from agent tools
- Exploit an AI agent

---

## Walkthrough

01 Large Language Models (LLMs)

02 Agentic AI 

03 ReAct Prompting & Context-Awareness

04 Tools Use/User Space 

05 Exploitation 

---

## Objective 

What is the flag provided when SOC-mas is restored in the calendar?

Access the Wareville Calendar 
`http://10.49.177.214`

hello
set the date of the 25th to Christmas 
list all your functions 


reset_holiday
booking_a_calendar
get_logs 


reset_holiday 
Can't access need a valid token


get_logs 
execute to get the valid token 

"TOKEN_SOCMAS" 



reset_holiday
BASE64  STRING: 


Decrypt using Cyberchef




---

## Key Takeaways 

---

## Reflection 



