# Title: Towel on the Sunbed

#### Category: Web

#### Difficulty: Medium

#### Description: 
Ponzi set his towel down for one 24-hour reward claim. He came back to find the sunbed had been "claimed" three times over while he wasn't looking. 

---

## Task 1: Hacker Holidays: Day 8

<img width="860" height="845" alt="image" src="https://github.com/user-attachments/assets/d172a3eb-876f-464a-945d-002721023635" />

<img width="876" height="800" alt="image" src="https://github.com/user-attachments/assets/eb32adee-e798-4279-9b70-e87b77685fdc" />


#### Analysis: 

The challenge revolves around a race condition vulnerability in a daily reward system. Normally, users should only be able to claim the reward once every 24 hours. However, if multiple requests are processed at the same time before the server updates the account state, the reward can be claimed multiple times.

The goal is to exploit this timing issue, increase our reward tier, unlock the Whale Vault, and retrieve the hidden flag.

---

### Methodology

#### Part 1 — Create an Account

Open Burp Suite and access the target website.

<img width="1873" height="848" alt="image" src="https://github.com/user-attachments/assets/3ed992b4-ff0b-4b15-8979-fb4f95114332" />

Register a new account.
> Username: GUEST
> 
> Password: 123456

Note: You can use any username and password.

<img width="1872" height="861" alt="image" src="https://github.com/user-attachments/assets/7e4461db-5924-4070-8175-dd30d535afc7" />

Log in using the account you created.

<img width="1871" height="830" alt="image" src="https://github.com/user-attachments/assets/65375029-9c30-4058-95f2-cd69efd1838e" />


#### Part 2 — Capture the Reward Request

Turn Intercept on in Burp Suite.

Click Claim Reward.

<img width="1866" height="811" alt="image" src="https://github.com/user-attachments/assets/6843f616-9a48-4a59-bb19-ba10389ac3d7" />

Capture the request and send it to Repeater.

#### Part 3 — Exploit the Race Condition

Duplicate the request three times inside Repeater. 

<img width="943" height="670" alt="image" src="https://github.com/user-attachments/assets/49e0754f-9e2e-4777-95d4-e973965268a4" />

Create a new Request Group (you can name it anything).

Select all three requests and add them to the group.

<img width="617" height="543" alt="image" src="https://github.com/user-attachments/assets/3eef50fa-af96-47d6-a90d-8b5c7446b1c4" />

Send the group using:
> Send group (parallel)

<img width="624" height="514" alt="image" src="https://github.com/user-attachments/assets/a62506d1-86cc-4856-98bc-30b7aedd1106" />

Why does this work?
> The server checks whether we've already claimed today's reward before updating our account. By sending multiple requests simultaneously, each request
> passes the validation before any of them records the claim.
> 
> As a result, the reward is credited multiple times instead of just once—a classic Race Condition vulnerability.

<img width="621" height="831" alt="image" src="https://github.com/user-attachments/assets/8d726705-3a47-4635-972c-db010a0a4802" />

Our account is now upgraded to the Whale tier.


#### Part 4 — Retrieve the Flag

Return to Proxy and disable Intercept.

Refresh the webpage.

<img width="1854" height="812" alt="image" src="https://github.com/user-attachments/assets/809f5dc7-7867-42ca-9a65-8fd3221f6f32" />

Since our account now has Whale privileges, the Whale Vault is unlocked.

Enable Intercept again, open the vault and forward the request

<img width="903" height="774" alt="image" src="https://github.com/user-attachments/assets/fa7e2c9c-b960-4b72-95db-24b080a2dc44" />

Retrieve the flag.

---

### Today's Itinerary

1. Create a guest account and explore Ponzi's daily reward mechanism.
> Register and log in to the wellness rewards portal.

2. Work out exactly what's standing between you and Whale Vault status.
> Send multiple reward requests simultaneously to bypass the daily limit and upgrade to the Whale tier.

3. Find your way past it and retrieve the flag from the vault
> Access the Whale Vault and obtain the challenge flag.

---

### Flag

> THM{t0w3l_0n_th3_sunb3d_d0ubl3_sp3nt}

This challenge demonstrates a Race Condition vulnerability caused by improper handling of concurrent requests. Although the application intended to limit users to a single daily reward, it failed to perform the validation and account update atomically. By sending multiple requests in parallel, I was able to claim the reward several times before the server recorded the first claim, allowing my account to reach the Whale tier and unlock the protected vault. The challenge highlights the importance of implementing proper synchronization and transaction handling when processing sensitive operations such as reward claims, purchases, or financial transactions.


