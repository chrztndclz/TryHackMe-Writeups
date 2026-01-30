## The Crown Jewel

You are on a shift, looking at the new alert coming from Imperium Labs - a company under MSSP monitoring long before you joined the team. It's hard to say what the company's primary focus is, but it has a global presence and undoubtedly has secrets to protect, especially those on heavily secured GitLab and Jira servers which store proprietary source code and project data.

## The Alert

The alert you are looking at is called Reverse Shell Outbound Connection Detected, not something you see every day. Fortunately, you were able to obtain the raw PCAPs and Splunk logs for this event. Can you analyze the network traffic and logs to reconstruct and stop a sophisticated attack aimed at stealing the "Crown Jewel" data?

---

From which internal IP did the suspicious connection originate?

Search to Splunk
```
index=network_logs log_type=ids
```

<img width="1396" height="561" alt="image" src="https://github.com/user-attachments/assets/d13c7391-b827-4fc2-a03d-b50726c9557b" />

<img width="1394" height="554" alt="image" src="https://github.com/user-attachments/assets/4ee09f4b-087c-4b0b-bfff-31e686b98629" />

`Answer: 10.10.10.100`

---

What outbound connection was detected as a C2 channel? (Answer example: 1.2.3.4:9996)

<img width="1399" height="665" alt="image" src="https://github.com/user-attachments/assets/f1b2a1f6-435c-4055-a3db-447cb08acc75" />

`Answer: 1.1.1.1:8080 `

---

Which MAC address is impersonating the gateway 10.10.10.1?

```

Open challenge.pcap file and apply this display filter

arp && arp.opcode == 2 && arp.src.proto_ipv4 == 10.10.10.1

```

<img width="1385" height="661" alt="image" src="https://github.com/user-attachments/assets/3c1f620a-0ee7-4d42-9e5b-4add505143c0" />

`Answer: 00:0c:29:11:22:33`

---

What is the non-standard User-Agent hitting the Jira instance?

Open network_logs file 

<img width="1393" height="655" alt="image" src="https://github.com/user-attachments/assets/be230fef-68ca-425d-b7cf-4f2ffd206b27" />

`Answer: CVE-202X-EXPLOIT`

---

How many ARP spoofing attacks were observed in the PCAP?

```
Go to chalenge.pcap file

Apply display filter: arp.opcode == 2

Statistics -> Capture File Properties -> Packets (Displayed)
```

<img width="1389" height="642" alt="image" src="https://github.com/user-attachments/assets/953eafe9-660a-4a1f-abdd-e18b34f98ed8" />

<img width="1270" height="652" alt="image" src="https://github.com/user-attachments/assets/06cfb842-2d3a-4b03-b323-c4add36728e7" />

`Answer: 90`

---

What's the payload containing the plaintext creds found in the POST request?

```
Go to wireshark and open challenge.pcap

Display Filter: http.request.method == "POST"

Right Click -> Follow -> HTTP Stream 

```

<img width="1394" height="262" alt="image" src="https://github.com/user-attachments/assets/8410dea5-5499-4052-b362-0bd1ab1a6723" />

<img width="1386" height="690" alt="image" src="https://github.com/user-attachments/assets/ddd0440e-6fb7-4b6b-ad90-f4cc2eca11f4" />

`Answer: username=dev_user&password=SecretPassword!`

---

What domain, owned by the attacker, was used for data exfiltration?

```

Go Splunk and search: index=* log_type=dns

and go for the event.domain field 

```

<img width="1397" height="556" alt="image" src="https://github.com/user-attachments/assets/6badcbf3-4f46-4efc-9014-c304ead1ceab" />

`Answer: exfil-domain.xyz`

---

After examining the logs, which protocol was used for data exfiltration?

<img width="1391" height="621" alt="image" src="https://github.com/user-attachments/assets/b27dc886-3e2b-4e82-94ee-879dc37cfdca" />


`Answer: dns`

---










