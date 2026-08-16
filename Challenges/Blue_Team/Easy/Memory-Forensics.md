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

1. "When was the machine last shutdown?"

For a Windows memory image, I'd investigate the system shutdown timestamp first.

A useful Volatility plugin is:

python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.registry.hivelist

This helps locate the Windows registry hives in memory.

<img width="721" height="298" alt="image" src="https://github.com/user-attachments/assets/76a2e978-dfb1-476d-9f25-8d28f54fa36b" />

The important line for the shutdown investigation is:

0xf8a000024010    \REGISTRY\MACHINE\SYSTEM

We have the SYSTEM registry hive in memory.

And:

\??\C:\Users\John\ntuser.dat

is John's user registry hive, which could become useful when investigating what John did.


For the shutdown question

Now that we know the SYSTEM hive exists, let's extract it.

```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.registry.printkey \
--offset 0xf8a000024010 \
--key "ControlSet001\Control\Windows"
```

"Take the SYSTEM registry hive we found in memory, then navigate through the registry to ControlSet001 → Control → Windows and show me the values stored there."

<img width="1338" height="297" alt="image" src="https://github.com/user-attachments/assets/74656fa4-708f-4ba4-a74c-6675e1ecc29b" />

This gives us the ShutdownTime

Answer: 
> 2020-12-27 22:50:12


Let's now proceed to answering the next question 

Since the clue says John had a command prompt open, our target is the cmd.exe process and its console history.

Step 1 — Find cmd.exe

```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.pslist
```

<img width="1057" height="799" alt="image" src="https://github.com/user-attachments/assets/78fb6833-db59-4a78-b2bd-d0192ca9cc2c" />

Great. We found the exact cmd.exe process:

PID   PPID   ImageFileName
1920  1144   cmd.exe

Let's check the command line

```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.cmdline
```

<img width="853" height="853" alt="image" src="https://github.com/user-attachments/assets/6bc65250-35dc-4ced-a4c0-1e22051b3c8a" />

Right — this output confirms what we already suspected, so we don't need to run windows.cmdline again.

That tells us how CMD was launched, not what John typed.



Let's try 
```
python3 ~/Downloads/volatility3/vol.py \
-f ~/Downloads/Snapshot19_1609159453792.vmem \
windows.consoles
```

If the console artifacts survived in the memory dump, this may expose the commands that were entered into cmd.exe.

Your memory image appears to be from Windows 7 / Windows Server 2008 R2-era Windows, and your installed Volatility 3.28.2 windows.consoles plugin doesn't support the specific conhost.exe version in this image.

---

Let's now use the strings method, but we'll make it targeted.

Create unicode strings 

```
strings -el ~/Downloads/Snapshot19_1609159453792.vmem > ~/Downloads/john_unicode.txt
```

Find every occurrence of John's prompt

```
grep -in 'C:\\Users\\John>' ~/Downloads/john_unicode.txt
```

<img width="1855" height="223" alt="image" src="https://github.com/user-attachments/assets/1ea689c9-49c5-40cb-a136-cfce57d5b1e5" />


Search specifically for commands after the prompt


```
grep -in -E 'C:\\Users\\John>.*echo|echo.*\.txt|C:\\Users\\John>.*>' ~/Downloads/john_unicode.txt
```

<img width="815" height="83" alt="image" src="https://github.com/user-attachments/assets/6373f13b-8e09-4e37-b14f-8548a8afd7d0" />

Answer:
> You_found_me
