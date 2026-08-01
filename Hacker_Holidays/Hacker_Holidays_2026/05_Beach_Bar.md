# Title: Beach Bar

#### Category: Boot2Root

#### Difficulty: Easy

#### Description: 
At the Beach Bar even shell access is complimentary. The jukebox takes requests. Any kind. 

---

## Task 1: Hacker Holidays: Day 5

<img width="689" height="676" alt="image" src="https://github.com/user-attachments/assets/9c2e082d-0c03-45a1-9b08-883da7063ead" />

<img width="704" height="339" alt="image" src="https://github.com/user-attachments/assets/e66ead0d-7b32-4b62-a06e-1538eec7ad11" />


#### Analysis: 





---

### Methodology


Access the website 

View Page Source 

You''ll see the staff note and used it to log in 

Import Page 
- Paste a reverse shell 

We can now do a RCE and have a shell 

Go back and type a command to get the user flag 


2nd flag:

ls command and you'll see the root dir but you can't access it 

cd opt
cd beach-bar
cd jukeboxd
cat jukeboxd.py

More command to find the password: stream-pass 

su root 
paste the password 

find the root.txt

And that's the flag. 


---

### Today's Itinerary 

1. Find the user flag

2. Find the root flag

---

### Flag: 
> thm{}

