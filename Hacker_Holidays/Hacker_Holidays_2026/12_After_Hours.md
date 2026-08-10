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
The challenge points toward WMI persistence and forensic analysis. The description suggests that something unauthorized is running after normal business hours, which made me suspect a persistence mechanism rather than a simple one-time execution.

The provided artifacts contain a WMI repository, with OBJECTS.DATA being the main file of interest. By extracting readable strings from the artifact and searching for indicators such as PowerShell, WMI event consumers, and encoded data, I was able to identify a suspicious WMI class and recover an embedded payload.

The payload was Base64-encoded and Deflate-compressed. After decoding and decompressing it, I recovered a Windows PE executable and analyzed it with ILSpy, where I found the final flag.

---

### Methodology

#### Step 1 – Locate the Challenge Files

I started by navigating to the challenge directory.

```cd /root/Rooms/hacker-holidays-2026/after-hours```

The challenge files were provided as a .7z archive, so I extracted them using:

```7z x after-hours.7z```

The archive requires the provided password:
> Aft3rH0ursAtt4chm3ntP4ss

After extraction, I navigated into the extracted directory.

<img width="768" height="611" alt="image" src="https://github.com/user-attachments/assets/ca9bc9af-7f43-42ae-abe3-1f9868987aae" />


#### Step 2 – Review the Provided Tools

I checked the tools directory.

```
cd tools

cat instruction.txt
```

The instructions provided ILSpy, which I would use later to analyze the recovered executable.

<img width="941" height="157" alt="image" src="https://github.com/user-attachments/assets/e19cba0f-4899-473d-88c2-46e53522501c" />

I extracted ILSpy:

``` unzip ILSpy-linux-x64-Release.zip```

Then navigated to the Linux executable:

```cd artificats/linux-64```

I made the binary executable:

``` chmod +x ILSpy```

Then launched it:

``` ./ILSpy```

ILSpy was now ready for the executable I would recover later.

<img width="600" height="466" alt="image" src="https://github.com/user-attachments/assets/1a7127e2-ba58-4106-abb7-82ba737f68f1" />


#### Step 3 – Understand the WMI Artifacts

I then navigated to the challenge artifacts.

<img width="947" height="113" alt="image" src="https://github.com/user-attachments/assets/36f1515e-c3c8-40a8-8aa5-98464abb4791" />

The important files were:
> INDEX.BTR 
> MAPPING1.MAP
> MAPPING2.MAP
> MAPPING3.MAP
> OBJECTS.DATA

The important one for this investigation is:
> OBJECTS.DATA

It contains WMI class definitions and instances, making it the primary source of evidence for the suspected WMI persistence.

The other files help the WMI repository locate and map its stored records.

WMI Persistence

WMI persistence can involve three important components:

- Event Filter – Defines when something should happen.
- Event Consumer – Defines what should happen.
- FilterToConsumerBinding – Connects the trigger to the action.

Because the challenge hints at something running persistently, these were important indicators to search for.


#### Step 4 – Extract Readable Strings

Since OBJECTS.DATA is a binary file, I first extracted readable strings from it.

For ASCII strings:

```strings -a OBJECTS.DATA > strings-ascii.txt```

For UTF-16 strings:

```strings -a -el OBJECTS.DATA > strings-utf16.txt```

I could then search the extracted files for suspicious terms.

```grep - i powershell *.txt```

This gave me several interesting results, including a Base64-encoded string.

Decode the Base64-encoded string.
```echo <base64_string> | base64 -d ```

<img width="1398" height="346" alt="image" src="https://github.com/user-attachments/assets/d77fbb51-ffd9-4961-802e-b33c5c46dc14" />


#### Step 5 – Investigate the Suspicious WMI Class

While reviewing the results, I found a reference to:

> Win32_HardwareTelemetry

I also found this interesting line:

``` ([WmiClass]'ROOT\cimv2\:Win32_HardwareTelemetry').Properties['ConfigData'].Value; ```

This tells me that the suspicious class contains a property called:

> ConfigData

The value stored in ConfigData appeared to contain the encoded payload.

I searched for more information around the class:

```grep -C 3 'Win32_HardwareTelemetry' *.txt```

<img width="1398" height="214" alt="image" src="https://github.com/user-attachments/assets/95a407ee-8f76-4d9d-adb7-ef3eb7a18f51" />


#### Step 6 – Decode the Base64 Data

I copied the Base64 value into a file:

```echo <base64_string> > output.txt```

At this point, I knew the data was not simply Base64. It was also Deflate-compressed.

So I decoded the Base64 and decompressed the resulting data:

```base64 -d output.txt | python3 -c 'import sys,zlib; sys.stdout.buffer.write(zlib.decompress(sys.stdin.buffer.read(), -15))' > output.bin```

This produced:

> output.bin

#### Step 7 – Identify the Recovered Payload

Before opening the file, I checked its first few bytes:

```xxd -l 32 output.bin```

The output started with:

> MZ

The MZ header indicates that the recovered data is a Windows executable/PE file.

So I renamed it:

```mv output.bin > output.exe```

I now had a PE executable that I could analyze with ILSpy.


#### Step 8 – Analyze the Payload with ILSpy

I opened output.exe in ILSpy.

I navigated through:

```
> Output
  > AfterHours
    > Program
```

<img width="1399" height="608" alt="image" src="https://github.com/user-attachments/assets/592c8114-01c7-495b-a8d8-48e1405f7d7f" />

Inside the program, I found another Base64-encoded string containing the flag.

The encoded value was:
> VEhNe1A0dGNoX29wM25lZF90aDNfQmFjS2QwMHJ9

I decoded it: 

<img width="504" height="66" alt="image" src="https://github.com/user-attachments/assets/374b04fc-edbf-4ed2-acd7-53c4d26361f6" />

```echo 'VEhNe1A0dGNoX29wM25lZF90aDNfQmFjS2QwMHJ9' | base64 -d```

This returned the flag.

---

### Today's Itinerary

1. Parse the provided system artifacts for hidden custom configuration data
> I extracted strings from OBJECTS.DATA and searched for suspicious WMI and PowerShell-related entries.

2. Locate the malicious class and extract its embedded payload
> I identified the Win32_HardwareTelemetry class and extracted its ConfigData property containing the encoded payload.

3. Decode the payload and submit the recovered flag
> I Base64-decoded and Deflate-decompressed the payload, analyzed the recovered executable with ILSpy, and decoded the final flag.

---

### Flag
> THM{P4tch---d00r}

This challenge demonstrated how WMI can be used as a persistence mechanism and how forensic analysis can uncover malicious data hidden inside the WMI repository.

I started by analyzing OBJECTS.DATA and searching its extracted strings for suspicious WMI persistence indicators. This led me to the Win32_HardwareTelemetry class and its ConfigData property. The stored data was Base64-encoded and Deflate-compressed, so I decoded and decompressed it to recover a Windows PE executable.

Finally, I loaded the executable into ILSpy, found the encoded flag inside the Program class, decoded it, and recovered the final flag.
