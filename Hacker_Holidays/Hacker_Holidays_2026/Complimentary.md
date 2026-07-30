# Title: Complimentary

#### Category: Easy

#### Description: 
Install the free app and it hands your phone a set of cloud keys, the same set it hands everyone. They're read-only, but read-
only of every guest's contacts, location, and passwords, not just Lambo's. She gave consent. Technically.

---

## Task 1: Hacker Holidays: Day 3

<img width="805" height="722" alt="image" src="https://github.com/user-attachments/assets/f12b1312-5a52-490d-82c6-554464194dde" />

<img width="805" height="636" alt="image" src="https://github.com/user-attachments/assets/59d8328a-180f-4f5a-b744-372264d5a28e" />


#### Analysis: 

Lambo installed Byte Lotus Wellness App 
Free tote bag for camera, mic, contacts and location access 
No account
No login

Complimentary Access - No sign up 

How the app knws anything about you at all 


---

### Methodology

Navigate the website: 

http://complimentary-wellness-app-332173347248.s3-website-us-east-1.amazonaws.com


<img width="1882" height="596" alt="image" src="https://github.com/user-attachments/assets/ce5fdae5-e093-4eb9-9c5d-d8f6a302da38" />



Check the page source

Notice there's a 

app.js

Inspect this

Note: 


Get part of the code and run it to the console 

Modify it and run 

Put name table and the region, remove the guest_id because we want the whole table 

and check the POST to see the dynamoDB table and look for the flag 

---

### Today's Itinerary 

1. Track down the AWS mechanism issuing you credentials behind the scenes

2. Use those credentials to dump more than your own record from the app's DynamoDB table

3. Retrieve the flag from another guest's data 


---

### Flag: 




---








