# Title: Packed Light

#### Category: Forensics

#### Difficulty: Easy

#### Description: 
Tiny packets. Odd hours. Suspiciously regular. Someone's smuggling out the data equivalent of a hotel towel every night, folded neatly inside
traffic that looks ordinary until you decode it.

---

## Task 1: Hacker Holidays: Day 4

<img width="875" height="703" alt="image" src="https://github.com/user-attachments/assets/1830d831-0cce-4580-b454-02b89a2e145a" />

<img width="876" height="635" alt="image" src="https://github.com/user-attachments/assets/1042634d-158d-4570-a234-ab32828887f8" />


#### Analysis: 
The challenge hints at a network forensics investigation involving covert data exfiltration. Phrases like "tiny packets," "traffic that looks ordinary," and "decode it" suggest that data is being hidden inside legitimate network traffic rather than sent openly.

0xMia's story provides additional clues, mentioning repeated connections to port 8080, suspicious HTTP request headers, and encrypted-looking data. This suggests the attacker is using HTTP traffic on TCP port 8080 as a covert communication channel, making it the logical starting point for the investigation.

---

### Methodology

#### Step 1: Open the Packet Capture

Download the provided PCAP file and open it in Wireshark.

At first glance, the capture contains many packets, making it difficult to identify suspicious traffic manually.

<img width="1666" height="635" alt="image" src="https://github.com/user-attachments/assets/2e16b6cd-7258-4ba2-9d5b-f0547129b5e6" />


#### Step 2: Focus on the Suspicious Traffic

Based on the challenge clues, filter the capture to display only HTTP requests sent over TCP port 8080 that contain cookies

```tcp.port == 8080 && http.request && http.cookie```

<img width="1666" height="837" alt="image" src="https://github.com/user-attachments/assets/916a78d9-a937-45b3-a7fb-7f238e1c5e5a" />

This filter combines three conditions:

- tcp.port == 8080
  > Displays only traffic using TCP port 8080, matching the clue provided by 0xMia.
- http.request
  > Limits the results to HTTP requests, ignoring responses from the server.
- http.cookie
  > Displays only requests that contain an HTTP Cookie header.


#### Step 3: Display the Cookie Column

Instead of opening every packet individually, add the HTTP Cookie field as a column in Wireshark.

This allows you to quickly observe that every request contains a different value for:

> hotel_sess_state=<value>

<img width="1663" height="827" alt="image" src="https://github.com/user-attachments/assets/1044d193-b948-4757-ae19-9e98e7daaac8" />




#### Step 4: Extract Every Cookie Value

Rather than copying each cookie manually, use TShark, the command-line version of Wireshark, to extract all cookie values automatically

```
tshark -r traffic.pcapng \
    -Y 'tcp.port == 8080 && http.request && http.cookie' \
    -T fields \
    -e http.cookie |
  sed 's/^hotel_sess_state=//'
```

<img width="472" height="672" alt="image" src="https://github.com/user-attachments/assets/81bfff11-f9ab-45e7-ad09-8b1c1d37a26b" />


Command Breakdown

tshark
> Command-line version of Wireshark used to analyze packet captures.

-r traffic.pcapng
> Reads packets from the specified PCAP file.

-Y 'tcp.port == 8080 && http.request && http.cookie'
> Filters for HTTP requests on TCP port 8080 that contain a Cookie header.

-T fields
> Outputs only the specified packet fields.

-e http.cookie
> Extracts the value of the HTTP Cookie header.

|
> Sends the output to the next command.

sed 's/^hotel_sess_state=//'
> Removes the hotel_sess_state= prefix, leaving only the encoded cookie values.


#### Step 5: Decode the Hidden Data

Open CyberChef and paste the extracted cookie values.

Recipe:
> From Base64
> XOR - Key: H, Encoding: UTF8

<img width="1321" height="575" alt="image" src="https://github.com/user-attachments/assets/cea36b32-42eb-4109-806a-cfbf82ebd0f2" />



---

### Today's Itinerary 

1. Analyze the provided capture for a covert communication channel.

>  By analyzing the packet capture in Wireshark, repeated HTTP requests sent over TCP port 8080 were identified. The presence of the recurring hotel_sess_state cookie indicated that the Cookie header was being used as a covert communication channel to transmit hidden data.

2. Identify where the exfiltrated data is being hidden and reassemble it.

> The exfiltrated data was hidden inside the HTTP Cookie header named hotel_sess_state. Using TShark, every cookie value was extracted from the packet capture and prepared for decoding.

3. Decode the recovered data and submit the flag. 

> The extracted cookie values were first decoded from Base64 and then XOR-decoded using the key H in CyberChef. This recovered the original hidden message containing the challenge flag.

---

### Flag: 
> thm{V3r4_1s_w4tch1ng_0veR_y0u}

This challenge demonstrates how attackers can hide sensitive information inside legitimate-looking network traffic. Instead of creating a custom protocol, the attacker embedded encoded data within standard HTTP Cookie headers, allowing the traffic to blend in with normal web requests.

It also highlights the importance of network traffic analysis during incident response. By combining packet filtering, field extraction with TShark, and data decoding using CyberChef, seemingly harmless HTTP requests were revealed to contain a covert communication channel used for data exfiltration.
