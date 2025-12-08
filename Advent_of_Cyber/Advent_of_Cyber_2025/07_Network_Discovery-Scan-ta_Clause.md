## Introduction

---

#### The Story

Task banner for day 7
Christmas preparations are delayed - HopSec has breached our QA environment and locked us out! Without it, the TBFC projects can't be tested, and our entire SOC-mas pipeline is frozen. To make things worse, the server is slowly transforming into a twisted EAST-mas node.

Can you uncover HopSec's trail, find a way back into tbfc-devqa01, and restore the server before the bunny's takeover is complete? For this task, you'll need to check every place to hide, every opened port that bunnies left unprotected. Good luck!

Three bad bunnies with three hidden easter eggs occupy the breached server in a Christmas theme.

#### Learning Objectives

Learn the basics of network service discovery with Nmap
Learn core network protocols and concepts along the way
Apply your knowledge to find a way back into the server

---

## Walkthrough 

01 The Simplest Port Scan
02 Scanning Whole Range
03 Port Scan Modes
04 TCP and UDP Ports
O5-Host Service Discovery
06 isting Listening Ports

---

## Objective 

IP: 10.49.162.132

What evil message do you see on top of the website?

Navigate website 

<img width="1863" height="826" alt="image" src="https://github.com/user-attachments/assets/4f43533d-f032-44bd-9e3b-0b92f8fbbc14" />

`Pwned by HosSec`

---

What is the first key part found on the FTP server?

<img width="1002" height="656" alt="image" src="https://github.com/user-attachments/assets/84bb6ece-4b78-479e-ad7b-5e16de433f0f" />

`3aster_`

---

What is the second key part found in the TBFC app?

nc -v 10.49.162.132 25251

<img width="1008" height="646" alt="image" src="https://github.com/user-attachments/assets/bd007ba7-4229-47ac-a38f-9ea99e695bba" />

`15_th3_`

---

What is the third key part found in the DNS records?

nmap -sU 10.49.162.132

dig @10.49.162.132 TXT key3.tbfc.local +short

<img width="1020" height="662" alt="image" src="https://github.com/user-attachments/assets/4da40794-a088-4460-a6e9-61ff3935adcf" />

`n3w_xm45`

---

Which port was the MySQL database running on?

Login to the website: http://10.49.162.132


3306

---

Finally, what's the flag you found in the database?

---

## Key takeaways


---
## Reflection
