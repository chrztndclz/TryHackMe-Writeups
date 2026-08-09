<img width="1399" height="608" alt="image" src="https://github.com/user-attachments/assets/af8dfb38-0e37-446a-87f3-fc2b51713d83" /># Title: After Hours

#### Category: Forensics

#### Difficulty: Medium

#### Description: 

Bar closed. Guests asleep. Something on the network just clocked in for a shift off the rotation.

---

## Task 1: Hacker Holidays: Day 12

<img width="780" height="747" alt="image" src="https://github.com/user-attachments/assets/4daf81e0-b1ce-4961-8ee7-282aa5c5b54d" />

<img width="767" height="884" alt="image" src="https://github.com/user-attachments/assets/76644572-431e-41b5-8ce1-04d2bf31b301" />

Analysis: 


---

### Methodology

Navigate
> /root/Rooms/hacker-holidays-2026/after-hours

Unzip Files
```7z x after-hours.7z```
> password: Aft3rH0ursAtt4chm3ntP4ss

Go to the unzip folder

<img width="768" height="611" alt="image" src="https://github.com/user-attachments/assets/ca9bc9af-7f43-42ae-abe3-1f9868987aae" />

Navigate tools directory
```cd tools```
Open instruction.txt
```cat instruction.txt```

<img width="941" height="157" alt="image" src="https://github.com/user-attachments/assets/e19cba0f-4899-473d-88c2-46e53522501c" />

Unzip ILSpy
``` unzip ILSpy-linux-x64-Release.zip```

Navigate Artificats
```cd artificats```

Navigate linux-64
```cd linux-64```

Modify permission make it executable
``` chmod +x ILSpy```

Run the tool
``` ./ILSpy```

You'll see that the ILSpy is already running

<img width="600" height="466" alt="image" src="https://github.com/user-attachments/assets/1a7127e2-ba58-4106-abb7-82ba737f68f1" />




Navigate the challenge attachments

<img width="947" height="113" alt="image" src="https://github.com/user-attachments/assets/36f1515e-c3c8-40a8-8aa5-98464abb4791" />


Challenge attachments:
INDEX.BTR 
> B - tree index used to locate records in OBJECTS.DATA
MAPPING1.MAP
MAPPING2.MAP
MAPPING3.MAP
> Translate logical database pages into physical pages; multiple versions support repository consistency/recovery
OBJECTS.DATA
> Contains WMI class definitions and instances - main evidence


WMI Persistence - attackers can create a permanent  subscription consisting of 
Event Filter 
Event Consumer
Filter To Consumer Binding 



Search specifically for PowerShell
```grep - i powershell *.txt```

It will result to a base64 string 
Decode

```base64 -d <String>```


<img width="1398" height="346" alt="image" src="https://github.com/user-attachments/assets/d77fbb51-ffd9-4961-802e-b33c5c46dc14" />



It is base64-encoded, Deflate-compressed, and loaded directly into memory 

This is an interesting line 
> ([WmiClass]'ROOT\cimv2:Win32_HardwareTelemetry').Properties['ConfigData'].Value;
What this means? The Data is in that path

ROOT\cimv2:Win32_HardwareTelemetry.ConfigData

Let's find the Stored ConfigData 
```grep -C 3 'Win32_HardwareTelemetry' *.txt```

Copy the result and put it in the Cyberchef 
> From Base64
> Raw Inflate

We can se a "MZ" is a PE32 I think it's a kind of a payload? 
Save the result into a .exe file name payload 

<img width="1398" height="214" alt="image" src="https://github.com/user-attachments/assets/95a407ee-8f76-4d9d-adb7-ef3eb7a18f51" />

<img width="1400" height="289" alt="image" src="https://github.com/user-attachments/assets/444a3f6f-41c2-496f-8b39-19727259dbec" />



Open ILSpy and add the payload.exe file 

Navigate Payload in ILSpy
> AfterHours
  > Program

Load it and the flag is there in a base64 format



<img width="1399" height="608" alt="image" src="https://github.com/user-attachments/assets/592c8114-01c7-495b-a8d8-48e1405f7d7f" />





---

### Today's Itinerary

1. Pull apart what the kiosk hands out for free before you've even clicked anything.
> 


---

### Flag
> THM{}

