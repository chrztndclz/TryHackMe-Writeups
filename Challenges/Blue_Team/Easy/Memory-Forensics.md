# Title: Memory Forensics

#### Category: 

#### Difficulty: Easy

#### Description: 

Perform memory forensics to find the flags

---

## Task 1:  Introduction 

Perform memory forensics to find the flags. If you are having trouble, maybe check out the volatility room first.

---

## Task 2: Login

The forensic investigator on-site has performed the initial forensic analysis of John's computer and handed you the memory dump he generated on the computer. As the secondary forensic investigator, it is up to you to find all the required information in the memory dump.


Question -  What is John's password? 

---

### Methodology:

#### 1. Download the task file

First, download the task file provided by TryHackMe.

The file we are given is a .vmem file.

> A .vmem file is a virtual machine's random-access memory (RAM) dump created by VMware. Volatility is an open-source memory forensics framework used to analyze this file. It lets security analysts inspect running processes, network connections, injected code, and passwords directly from the virtual RAM image.

Since we are dealing with a memory dump, we need a tool that can analyze it. For this, we'll use Volatility 3.

First, clone the Volatility 3 repository:

```
git clone https://github.com/volatilityfoundation/volatility3.git
```

#### 2. Identify the Windows version

Before starting the actual investigation, let's check what operating system and environment the memory dump came from.

We can use windows.info:

```
python3 ~/Downloads/volatility3/vol.py -f ~/Downloads/Snapshot6_1609157562389.vmem windows.info
```

What did we find?

<img width="1105" height="566" alt="image" src="https://github.com/user-attachments/assets/5e0381c3-a6b1-49f9-9411-70c0ec3273e6" />

### Important Findings

| Field | Value | Significance |
|---|---|---|
| **Operating System** | Windows 7 SP1 | Identifies the operating system from which the memory was captured. |
| **Architecture** | 64-bit | Indicates that the memory image belongs to a 64-bit Windows system. |
| **Kernel Base** | `0xf80002a59000` | The base address of the Windows kernel in memory. |
| **DTB** | `0x187000` | Used by Volatility for virtual-to-physical memory address translation. |
| **Kernel Symbols** | `ntkrnlmp.pdb` | Confirms that Volatility successfully identified the appropriate Windows kernel symbols. |
| **System Time** | `2020-12-27 06:20:05 UTC` | The system timestamp recorded in the memory image. |
| **System Root** | `C:\Windows` | The Windows installation directory. |
| **Processors** | `1` | The memory image indicates that the system had one logical processor. |


This confirms that the memory dump came from a 64-bit Windows 7 SP1 system.


#### 3. Check the Windows password hashes

Now, since the question is asking for John's password, let's focus on information related to Windows authentication.

One thing we can check is the Windows password hashes using Volatility's windows.hashdump plugin.

```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot6_1609157562389.vmem \
windows.hashdump
```

<img width="787" height="272" alt="image" src="https://github.com/user-attachments/assets/706f60d0-814a-4f5e-b0c6-5b8cc91e7b4d" />

The important line is:
> John	1001	aad3b435b51404eeaad3b435b51404ee	47fbd6536d7868c873d5ea455f2fc0c9

We can break it down like this:

```
Username	John
RID	1001
LM Hash	aad3b435b51404eeaad3b435b51404ee
NT Hash	47fbd6536d7868c873d5ea455f2fc0c9
```

The NT hash is the important value:
> 47fbd6536d7868c873d5ea455f2fc0c9

The LM hash is not useful because it is disabled/empty in this case, while the NT hash contains the actual NTLM credential hash associated with the Windows account. Therefore, the NT hash is the value we extract for password cracking.

However, this is not John's actual password

It's a one-way hash derived from the password. We need to determine the plaintext password from this hash.


#### 4. Alternative: Search the memory dump for plaintext credentials

Before cracking the hash, another thing we could try is searching the memory dump for John's password in plaintext.

Since this is a TryHackMe-style forensic challenge, the password could potentially still exist somewhere in memory.

We can search the memory dump using strings.

For Unicode/UTF-16 strings:
> strings -el ~/Downloads/Snapshot6_1609157562389.vmem | grep -i "John"

We can also check regular ASCII strings:
> strings ~/Downloads/Snapshot6_1609157562389.vmem | grep -i "John"

And search for likely credential-related terms:
> strings ~/Downloads/Snapshot6_1609157562389.vmem | grep -Ei "password|passwd|credential"

There can be a lot of output when searching a memory dump this way, so manually going through everything can take some time.

Because we already have John's NT hash, let's use a more direct approach and try to crack it.


5. Crack the NT hash with Hashcat

Our target is:
> 47fbd6536d7868c873d5ea455f2fc0c9

First, let's create a file containing the hash.

From ~/Downloads:

```
echo '47fbd6536d7868c873d5ea455f2fc0c9' > john_hash.txt
```

Now we can use Hashcat with the RockYou wordlist.

The -m 1000 option tells Hashcat that we are dealing with an NTLM hash, while -a 0 specifies a dictionary attack.

```
hashcat -m 1000 -a 0 john_hash.txt /usr/share/wordlists/rockyou.txt
```

Once Hashcat finishes, let's check the recovered password:

```
hashcat -m 1000 john_hash.txt --show
```

<img width="383" height="79" alt="image" src="https://github.com/user-attachments/assets/9c0cbf89-e961-430a-8f78-24a074ac80c9" />


Answer: 
> charmander999

In this investigation, we were given a .vmem memory dump from John's computer and used Volatility to analyze it. We first identified the Windows environment with windows.info, then focused on the password-related information using windows.hashdump.

From the hashdump, we found John's NT hash. Since the hash itself does not reveal the plaintext password, we used Hashcat with the RockYou wordlist to crack it. This allowed us to recover John's password:

charmander999

Overall, the basic memory forensics process was:

Memory dump → Identify the system → Find relevant artifacts → Extract credentials/hashes → Analyze or crack the data → Recover the required information.


---

## Task 3: Analysis

On arrival a picture was taken of the suspect's machine, on it, you could see that John had a command prompt window open. The picture wasn't very clear, sadly, and you could not see what John was doing in the command prompt window.

To complete your forensic timeline, you should also have a look at what other information you can find, when was the last time John turned off his computer?

<img width="684" height="349" alt="image" src="https://github.com/user-attachments/assets/51074918-7ac3-4f9f-be4b-188a36094f30" />

Question:

When was the machine last shutdown?

What did John write?

---


### 1. When was the machine last shut down?

For a Windows memory image, one thing we can investigate is the system's shutdown timestamp.

A useful starting point is the Windows registry. Let's use Volatility's windows.registry.hivelist plugin to locate the registry hives in memory.

```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.registry.hivelist
```

This helps locate the Windows registry hives in memory.

<img width="721" height="298" alt="image" src="https://github.com/user-attachments/assets/76a2e978-dfb1-476d-9f25-8d28f54fa36b" />

What did we find?

The important entry for our shutdown investigation is:

> 0xf8a000024010    \REGISTRY\MACHINE\SYSTEM

This tells us that the SYSTEM registry hive is present in memory.

We can also see:
> \??\C:\Users\John\ntuser.dat

This is John's user registry hive, which could become useful later when investigating his activity.


Extract the shutdown information

Now that we know where the SYSTEM hive is located, we can use windows.registry.printkey to inspect a specific registry key related to Windows shutdown information.

Run:

```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.registry.printkey \
--offset 0xf8a000024010 \
--key "ControlSet001\Control\Windows"
```

In other words, we're telling Volatility:

> Take the SYSTEM registry hive we found in memory, navigate to ControlSet001 → Control → Windows, and display the values stored there.


<img width="1338" height="297" alt="image" src="https://github.com/user-attachments/assets/74656fa4-708f-4ba4-a74c-6675e1ecc29b" />

The output contains a value called:

> ShutdownTime

This gives us the last recorded shutdown time

Answer: 
> 2020-12-27 22:50:12

---

### 2. What did John write?

Let's now move on to the second question.

The clue tells us that John had a Command Prompt window open, so our target is the cmd.exe process and, if possible, the console history associated with it.

Step 1 — Find cmd.exe

```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.pslist
```

<img width="1057" height="799" alt="image" src="https://github.com/user-attachments/assets/78fb6833-db59-4a78-b2bd-d0192ca9cc2c" />

Great! We found the cmd.exe process:

PID    PPID    ImageFileName
1920   1144    cmd.exe

So we know that a Command Prompt process was present in the memory image.

Step 2 — Check the command line

Next, let's check the command line associated with the process.

```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.cmdline
```

<img width="853" height="853" alt="image" src="https://github.com/user-attachments/assets/6bc65250-35dc-4ced-a4c0-1e22051b3c8a" />

This confirms how the cmd.exe process was launched.

However, there is an important distinction here:

windows.cmdline shows the command line used to launch the process. It does not necessarily show the commands that John typed inside the Command Prompt.

So we need to look for the console history instead.

Step 3 — Try windows.consoles

```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.consoles
```

Ideally, this plugin can recover console information and commands entered into cmd.exe.

However, in this case, the plugin does not support the specific conhost.exe version found in this Windows 7-era memory image.

So we need another approach.

---

Step 4 — Search the memory dump for John's commands

Since the console plugin isn't able to recover the command history, let's search the raw memory dump for strings.

Because Windows commonly stores text as Unicode/UTF-16, we'll first extract Unicode strings from the memory image.

Run:

```
strings -el ~/Downloads/Snapshot19_1609159453792.vmem > ~/Downloads/john_unicode.txt
```

This creates a file containing the Unicode strings extracted from the memory dump.

Now let's search for John's Command Prompt:

```
grep -in 'C:\\Users\\John>' ~/Downloads/john_unicode.txt
```

<img width="1855" height="223" alt="image" src="https://github.com/user-attachments/assets/1ea689c9-49c5-40cb-a136-cfce57d5b1e5" />


Great! We can see references to John's Command Prompt.

But we still need to find what he actually typed.

Step 5 — Search for the command John entered

Since we're looking for something written from the Command Prompt, let's search for commands appearing after John's prompt.

We can specifically look for commands involving echo, .txt, or output redirection (>):

```
grep -in -E 'C:\\Users\\John>.*echo|echo.*\.txt|C:\\Users\\John>.*>' ~/Downloads/john_unicode.txt
```

<img width="815" height="83" alt="image" src="https://github.com/user-attachments/assets/6373f13b-8e09-4e37-b14f-8548a8afd7d0" />

And there it is!

We recovered the text John wrote

Answer:
> You_found_me


For this task, we used several different sources of evidence from the memory dump.

First, we located the SYSTEM registry hive and used it to recover the machine's last shutdown time. Then, we used windows.pslist to identify the cmd.exe process.

Although windows.cmdline showed how Command Prompt was launched, it didn't reveal the commands John typed. The windows.consoles plugin also couldn't recover the console history because of the Windows version in the memory image.

Instead, we extracted Unicode strings from the memory dump and searched specifically for John's Command Prompt and likely command patterns. This allowed us to recover the text:

You_found_me
