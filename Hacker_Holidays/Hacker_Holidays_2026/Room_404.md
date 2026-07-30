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

Step 1: Access the Web Application

Navigate to the target:

http://10.49.164.150:8080

The homepage loads successfully and presents a Reserve a Stay button.

<img width="1095" height="703" alt="image" src="https://github.com/user-attachments/assets/62679a3c-7a6c-43cf-8771-dc041fb50927" />


Step 2: Investigate the Broken Link

Clicking Reserve a Stay returns a 404 Not Found error.

This indicates that the linked page no longer exists or was never deployed. Rather than stopping here, this suggests that other hidden resources may still be accessible.

<img width="1101" height="265" alt="image" src="https://github.com/user-attachments/assets/ec962fb1-b8b0-488b-9fa7-f6c4b938f20f" />






<img width="911" height="616" alt="image" src="https://github.com/user-attachments/assets/3f253cfd-0120-4754-858d-6bab8c5cefd0" />



 ```gobuster dir -u http://10.49.164.150:8080 -w /usr/share/wordlists/dirb/common.txt```



<img width="911" height="616" alt="image" src="https://github.com/user-attachments/assets/0b618616-960d-40b4-9789-a003df92c4df" />


To download all of the files 

```wget -r -np -R "index.html*" http://10.49.164.150:8080/.git/```

---

```
cd http://10.49.164.150:8080

ls

cat README.md

```

<img width="860" height="246" alt="image" src="https://github.com/user-attachments/assets/19ac3608-199e-4e82-8123-becf9f5ecb73" />



---


### Today's Itinerary 

1. Dump the exposed source code.
 
2. Find the flag.
