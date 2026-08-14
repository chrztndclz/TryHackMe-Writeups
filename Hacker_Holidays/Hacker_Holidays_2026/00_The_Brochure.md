# Title: The Brochure

#### Category: OSINT

#### Difficulty: Easy

#### Description: 
The brochure's hero photo has an AI fingerprint. Follow the account that posted it, and the trail doesn't end at the hotel; it ends at someone the hotel never mentioned.

---

## Task 1: Hacker Holidays Storyline: Act 0 - Recon

<img width="1000" height="797" alt="image" src="https://github.com/user-attachments/assets/a72e2543-f7cf-4f9b-b0ae-e0ce3c4f91bc" />

<img width="1014" height="630" alt="image" src="https://github.com/user-attachments/assets/f04f3aa6-7654-4811-8a59-8126e8e38bc8" />

#### Analysis:

The first task provides a brochure image that contains clues pointing toward the next stage of the investigation.

The description specifically mentions that the brochure's hero photo has an AI fingerprint, suggesting that the image itself should be examined for additional information rather than simply treated as a normal hotel advertisement.

---

## Task 2: Hacker Holidays: Day 0

<img width="862" height="767" alt="image" src="https://github.com/user-attachments/assets/b7599a37-b68e-4596-be85-489004b14fa9" />

<img width="857" height="575" alt="image" src="https://github.com/user-attachments/assets/f847649a-e387-4517-a4f1-b0a9ffea838b" />

#### Analysis:

The second task continues the investigation by providing another clue that points toward the social media presence associated with the Byte Lotus Resort.

The objective is to follow the trail from the provided image and identify the account that posted or is associated with the brochure.

---

### Methodology: 

#### 1. Download the Task File

First, download the task file provided by TryHackMe.

After downloading it, extract the contents using:

```unzip <ZIP_FILE>```

The extracted files contain a PNG image.


#### 2. Open the Brochure

Open the extracted image:

```xdg-open thebrochure.png```

<img width="727" height="940" alt="image" src="https://github.com/user-attachments/assets/1db44d16-151a-4182-a183-fdf76762a988" />

Examining the brochure reveals a clue suggesting that Instagram should be investigated.

The brochure also mentions:
> Vera can assist you with further information.

This gives us two useful leads:

- The Byte Lotus Resort's Instagram presence
- An account associated with Vera


#### 3. Find the Resort's Instagram Account

The next step is to search Instagram for the resort.

Search for:

> thebytelotusresort

or other variations related to the Byte Lotus Resort.

<img width="672" height="165" alt="image" src="https://github.com/user-attachments/assets/7d322416-d04e-44a1-a1a0-fb65d5b92774" />

The search reveals the The Byte Lotus Resort Instagram account.


#### 4. Enumerate the Resort Account

Let's inspect the account.

<img width="910" height="700" alt="image" src="https://github.com/user-attachments/assets/3fafe032-2861-4032-8d47-0c60d282ff0c" />

The account only contains two posts, and neither provides any immediately useful information.

Instead of stopping here, we can investigate the account's following list

<img width="516" height="137" alt="image" src="https://github.com/user-attachments/assets/f641e7bc-be76-4d0d-9060-fb5f1c84e02f" />

The resort account follows only one account, which gives us our next OSINT pivot.

The account is Vera.


5. Investigate Vera's Account

The brochure specifically mentioned that Vera could provide further information, so let's inspect her Instagram account.

<img width="888" height="701" alt="image" src="https://github.com/user-attachments/assets/ae626f25-9f2d-4d26-9428-a0a984d49fc3" />

Vera's account contains three posts, and each post contains what appears to be a Base64-encoded string.

This is significant because the three strings may represent separate parts of the same piece of information.


6. Extract the Base64 Strings

Let's extract the encoded value from each post.

Part 1:
> VEhNe1YzckBzX2FD

Part 2: 
> QzB1bnRfaDRzX2Iz

Part 3: 
> M25fZjB1bmQhfQ==

Since the three values appear to be fragments of the same encoded message, combine them in order:
> VEhNe1YzckBzX2FDQzB1bnRfaDRzX2IzM25fZjB1bmQhfQ==


7. Decode the Base64 Data

We can decode the combined string using CyberChef.

Use the following recipe:
> FromBase64

<img width="1089" height="592" alt="image" src="https://github.com/user-attachments/assets/1148600a-90b4-474c-8423-18a023cb665f" />

The decoded output reveals the flag.

---

### Today's Itinerary 

1. Analyze the provided image for embedded clues.
> Opened thebrochure.png and examined the image for clues that pointed toward Instagram and the Vera account.

2. Apply fundamental OSINT techniques to trace the findings.
> Searched for the Byte Lotus Resort on Instagram and inspected the account's posts and following list to identify the next lead.

3. Locate the hidden social media account.
> The resort account followed only one account, Vera, which led to the three posts containing Base64-encoded strings.

4. Submit the flag.
> Extracted the three Base64 fragments, combined them in the correct order, decoded them using CyberChef's From Base64 recipe, and recovered the flag.

---

Flag: 
> THM{V3r@s_aCC0unt_h4s_b33n_f0und!}

he investigation began with the provided brochure, which contained clues pointing toward the Byte Lotus Resort's Instagram account. By applying basic OSINT techniques, we inspected the resort's following list and discovered the Vera account mentioned in the clues.

Vera's three Instagram posts contained separate Base64-encoded fragments. After collecting and combining the fragments in the correct order, we decoded the final string using CyberChef and recovered the flag.

