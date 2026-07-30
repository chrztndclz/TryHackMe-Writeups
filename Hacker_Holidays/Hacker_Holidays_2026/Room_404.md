# Title: Room 404

#### Category: Easy

#### Description: 
He booked the quiet room. It's not on the floor plan, not in the brochure, not on any door. But port 8080 is wide open, and the rooms it never lists are the ones worth finding.

---

## Task 1: Hacker Holidays: Day 2

<img width="893" height="846" alt="image" src="https://github.com/user-attachments/assets/47660bff-4df3-415e-8fe5-80416374228d" />


<img width="912" height="382" alt="image" src="https://github.com/user-attachments/assets/bdfa6b52-989b-4ef7-b4b7-ef7ed07df814" />


#### Analysis: 

The challenge description hints that there is a hidden resource exposed on port 8080. The phrases "not on the floor plan" and "rooms it never lists" suggest looking for resources that are not publicly linked by the website.

Another clue states:

> "The night-shift developer shipped more than the website."

This implies that the developer accidentally deployed additional files, most likely the website's source code or a Git repository.


---

### Methodology: 

#### Step 1: Access the Web Application

Navigate to the target:

http://10.49.164.150:8080

The homepage loads successfully and presents a Reserve a Stay button.

<img width="1095" height="703" alt="image" src="https://github.com/user-attachments/assets/62679a3c-7a6c-43cf-8771-dc041fb50927" />


#### Step 2: Investigate the Broken Link

Clicking Reserve a Stay returns a 404 Not Found error.

This indicates that the linked page no longer exists or was never deployed. Rather than stopping here, this suggests that other hidden resources may still be accessible.

<img width="1101" height="265" alt="image" src="https://github.com/user-attachments/assets/ec962fb1-b8b0-488b-9fa7-f6c4b938f20f" />


#### Step 3: Enumerate Hidden Directories

Use Gobuster to discover hidden files and directories.

 ```gobuster dir -u http://10.49.164.150:8080 -w /usr/share/wordlists/dirb/common.txt```
 
<img width="911" height="616" alt="image" src="https://github.com/user-attachments/assets/3f253cfd-0120-4754-858d-6bab8c5cefd0" />

The scan reveals an exposed .git directory.

This is a critical finding because a publicly accessible Git repository can expose the application's complete source code.


#### Step 4: Download the Exposed Git Repository
Recursively download the contents of the exposed repository.

```wget -r -np -R "index.html*" http://10.49.164.150:8080/.git/```

This retrieves the Git metadata and repository files for offline analysis.

#### Step 5: Inspect the Repository

Navigate to the downloaded files.

```
cd http://10.49.164.150:8080

ls

```

Among the recovered files is a README.md.

Read its contents.

```

cat README.md

```

<img width="860" height="246" alt="image" src="https://github.com/user-attachments/assets/19ac3608-199e-4e82-8123-becf9f5ecb73" />

The README contains the challenge flag.

---

### Today's Itinerary 

1. Dump the exposed source code.

The website accidentally exposed its .git directory. By downloading the repository, the complete source code became accessible for local analysis.
 
3. Find the flag.

After examining the recovered repository, the README.md file contained the challenge flag.


---

### Flag: 
THM{byt3_l0tus_n3v3r_f0rg3ts}


This challenge demonstrates the risks of exposing a Git repository on a production web server.

Sensitive source code should never be publicly accessible.
Accidentally deploying the .git directory allows attackers to reconstruct the application's source code.
Exposed repositories may contain configuration files, credentials, API keys, commit history, and other sensitive information that can lead to further compromise.

The challenge emphasizes the importance of removing development artifacts before deploying applications to production environments.
