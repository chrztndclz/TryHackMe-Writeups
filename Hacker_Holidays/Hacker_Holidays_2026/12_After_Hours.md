# Title: After Hours

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

Download task files 

Unzip Files 

Go to challenge attachments 
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

---------------------------------------------

Extract ASCII text
```strings -a OBJECTS.DATA > strings-ascii.txt```

Extract UTF-16 text
```strings -a -el OBJECTS.DATA > strings-utf16.txt```

Search specifically for PowerShell
```grep - i powershell *.txt```
 
Search for many suspicious keywords
```
rg -i '__EventFilter|FilterToConsumerBinding|CommandLineEventConsumer|ActiveScriptEventConsumer|CommandLineTemplate|ScriptText|powershell|cmd\.exe|wscript|cscript|base64|https?://' strings-*.txt
```

```
grep -ie -i '__EventFilter|FilterToConsumerBinding|CommandLineEventConsumer|ActiveScriptEventConsumer|CommandLineTemplate|ScriptText|powershell|cmd\.exe|wscript|cscript|base64|https?://' strings-*.txt
```

---------------------------------------------


Search specifically for PowerShell
```grep - i powershell *.txt```

Save the result into a file 

Copy the result and go to cyberchef 
> From Base64
> Remove null bytes

Analyze the result


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



Open ILSpy and add the payload.exe file 

Navigate Payload in ILSpy
> AfterHours
  > Program

Load it and the flag is there in a base64 format










---

### Today's Itinerary

1. Pull apart what the kiosk hands out for free before you've even clicked anything.
> 


---

### Flag
> THM{}

