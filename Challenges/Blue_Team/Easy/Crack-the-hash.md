# Title: # Title: Memory Forensics

#### Category: 

#### Difficulty: Easy

#### Description: 

Cracking hashes challenges

---

## Task 1:  Level 1 

Can you complete the level 1 tasks by cracking the hashes?

---

#### 48bb6e862e54f2a795ffc4e541caed4d

Let's first identify the type of hash.

The hash is 32 hexadecimal characters long. A 32-character hexadecimal hash is most commonly MD5.

Let's now try to crack it.

First, I'll put the hash into a .txt file:

```
echo '48bb6e862e54f2a795ffc4e541caed4d' > hash.txt
```

I'll use the rockyou.txt wordlist with Hashcat:

```
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt
```

Where:
```
-m 0 → MD5
hash.txt → File containing the target hash
/usr/share/wordlists/rockyou.txt → Wordlist used for the dictionary attack
```

After Hashcat finishes, I can display the recovered password with:

```
hashcat -m 0 hash.txt --show
```

<img width="376" height="71" alt="image" src="https://github.com/user-attachments/assets/67b6b747-ba05-4be5-a51e-16e2c2fa70a6" />

Answer:
> easy

---

#### CBFDAC6008F9CAB4083784CBD1874F76618D2A97 

It is 40 hexadecimal characters long.

A 40-character hexadecimal hash is most commonly SHA-1.

Let's put it into a .txt file:

```
echo 'CBFDAC6008F9CAB4083784CBD1874F76618D2A97' > hash2.txt
```

Now I'll use Hashcat with the rockyou wordlist:

```
hashcat -m 100 hash2.txt /usr/share/wordlists/rockyou.txt
```

Where:
```
-m 100 = SHA-1
```

After Hashcat finishes, I'll display the recovered password:

```
hashcat -m 100 hash2.txt --show
```

<img width="424" height="75" alt="image" src="https://github.com/user-attachments/assets/edd14aca-a699-4427-9b93-f42a57b53704" />

Answser:
> password123

---

#### 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032

It is 64 hexadecimal characters long.

A 64-character hexadecimal hash is most commonly SHA-256.

Let's put it into a .txt file:

```
echo '1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032' > hash3.txt
```

Now I'll use Hashcat with the rockyou wordlist:

```
hashcat -m 1400 hash3.txt /usr/share/wordlists/rockyou.txt
```

Where:
```
-m 1400 = SHA-256
```

After Hashcat finishes:

```
hashcat -m 1400 hash3.txt --show
```

<img width="586" height="77" alt="image" src="https://github.com/user-attachments/assets/e6e59e44-c1e1-4094-b198-1229686669db" />

Answer:
> letmein

---

#### $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom

This hash is a little different because we can identify the algorithm directly from its prefix.

The important part is:

> $2y$

This indicates bcrypt.

The 12 is the bcrypt cost factor, which controls how computationally expensive the hashing operation is.

```
$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom
 ↑   ↑
 |   └── Cost factor
 └────── bcrypt identifier
```

So:
```
Hash type: bcrypt
Cost factor: 12
Hashcat mode: 3200
```

Unlike MD5 or SHA-1, bcrypt is intentionally designed to be much slower to crack. Since this challenge tells us the answer is only 4 characters long, I can reduce the size of the wordlist instead of testing millions of unnecessary entries.

First, I'll save the hash:

```
echo '$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom' > hash4.txt
```

Now I'll filter rockyou.txt and keep only words that are exactly four characters long:

```
LC_ALL=C awk 'length($0) == 4' /usr/share/wordlists/rockyou.txt > rockyou4.txt
```

I'll verify how many entries are left:

```
wc -l rockyou4.txt
```

<img width="368" height="78" alt="image" src="https://github.com/user-attachments/assets/cd1279d2-3753-4a43-ab62-47dacd3abfb0" />

Instead of checking the entire wordlist, Hashcat now only has to test the filtered list:

```
hashcat -m 3200 hash4.txt rockyou4.txt
```

Where:

```
-m 3200 → bcrypt
```

After Hashcat finishes:

```
hashcat -m 3200 hash4.txt --show
```

<img width="532" height="76" alt="image" src="https://github.com/user-attachments/assets/c18764b7-b914-4cba-bfa3-f3fdaf987cf9" />


Answer:
> bleh


---


#### 279412f945939ba78ce0758d3fd83daa

Let's first identify the hash.

It is 32 hexadecimal characters long, so the most likely possibilities include MD5 and MD4. Hash length alone is not always enough to identify an algorithm because several different algorithms can produce hashes with the same length.

For this challenge, I'll try an online hash lookup tool instead.

I'll use:

> Hashes.com

I can paste the hash into the search field and check whether the hash has already been identified or cracked.

an online hash decryption tool and let's paste the hash 

<img width="406" height="293" alt="image" src="https://github.com/user-attachments/assets/467622f4-ceea-4715-b1f3-ec598c3b3e10" />

Answer:
> Eternity22


---

## Task 2:  Level 2

This task increases the difficulty. All of the answers will be in the classic rock you (opens in new tab) password list.

You might have to start using hashcat here and not online tools. It might also be handy to look at some example hashes on hashcats page (opens in new tab).

---

#### F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85

Let's first identify the type of hash.

It is 64 hexadecimal characters long, which commonly indicates SHA-256.

Let's put it into a .txt file:

```
echo 'F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85' > hash6.txt
```

Now I'll use Hashcat with rockyou:

```
hashcat -m 1400 hash6.txt /usr/share/wordlists/rockyou.txt
```

Where:

```
-m 1400 → SHA-256
```

After Hashcat finishes:

```
hashcat -m 1400 hash6.txt --show
```

<img width="566" height="79" alt="image" src="https://github.com/user-attachments/assets/9af0371a-f2e1-49f4-95df-91a9f5c58ee5" />


Answer:
> paule

---

#### 1DFECA0C002AE40B8619ECF94819CC1B

It is 32 hexadecimal characters long.

MD5 is one possibility, but 32 hexadecimal characters can also represent other hash types, including NTLM.

Since this is a Level 2 challenge, I'll try NTLM using Hashcat mode 1000.

Let's save the hash:

```
echo '1DFECA0C002AE40B8619ECF94819CC1B' > hash7.txt
```

Then I'll run:

```
hashcat -m 1000 hash7.txt /usr/share/wordlists/rockyou.txt
```

Where:

```
-m 1000 → NTLM
```

NTLM is commonly encountered when dealing with Windows password hashes. Unlike MD5, NTLM is not simply identified by its 32-character length; the context of the hash is important.

After Hashcat finishes:

```
hashcat -m 1000 hash7.txt --show
```

<img width="374" height="79" alt="image" src="https://github.com/user-attachments/assets/115c8832-af6b-4cbf-b406-41463e8033ab" />

Answer: 
> n63umy8lkf4i

---

#### Hash: $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.

Salt: aReallyHardSalt

Let's first identify the hash.

This time, the format gives us a strong clue.

The important prefix is:

> $6$

This indicates sha512crypt, which uses Hashcat mode:

> 1800

So:

```
Hash type: sha512crypt
Hashcat mode: 1800
Salt: aReallyHardSalt
```

The salt is not part of the password. It is an additional value used during the hashing process to make precomputed attacks such as rainbow tables much less useful.

I'll put the complete hash into a .txt file

```
echo '$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.' > hash8.txt
```

Now I'll use Hashcat:

```
hashcat -m 1800 hash8.txt /usr/share/wordlists/rockyou.txt
```

Where:

```
-m 1800 → sha512crypt
```

After Hashcat finishes

```
hashcat -m 1800 hash8.txt --show
```

<img width="899" height="82" alt="image" src="https://github.com/user-attachments/assets/0c0145c3-048e-4366-83f0-a5792749a6e7" />

Answer:
> waka99


---

#### Hash: e5d8870e5bdd26602cab8dbe07a942c8669e56d6

Salt: tryhackme

Let's first identify the hash.

The hash is 40 hexadecimal characters, which is consistent with SHA-1. However, the provided salt changes what we're dealing with.

A normal SHA-1 hash would simply be calculated from the password:

> SHA1(password)

Here, the challenge provides a key/salt value, so we're dealing with HMAC-SHA1 rather than plain SHA-1.

The important difference is:

```
SHA-1: a cryptographic hash function that hashes the input directly.
HMAC-SHA1: uses SHA-1 together with a secret key to produce a keyed hash.
```

In this challenge, the provided value tryhackme is used as the HMAC key.

For Hashcat, this format uses mode:

> 160

I'll save the hash and key in the format Hashcat expects:

```
echo 'e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme' > hash9.txt
```

Now I'll use the rockyou wordlist:

```
hashcat -m 160 hash9.txt /usr/share/wordlists/rockyou.txt
```

Where:

```
-m 160 → HMAC-SHA1
tryhackme → HMAC key
rockyou.txt → Candidate passwords
```

After Hashcat finishes:

```
hashcat -m 160 hash9.txt --show
```

Answer: 
> 481616481616

The main lesson from this challenge was that hash length alone is not always enough to identify a hash. I used the hash format, prefixes, salts, challenge context, and Hashcat modes to determine the appropriate cracking method.

The overall attack chain was:

Identify the hash → determine the correct Hashcat mode → prepare the hash → choose/filter the wordlist when possible → crack with Hashcat → verify the result with --show.

Level 1 introduced the common hashing algorithms, while Level 2 required more attention to formats such as NTLM, sha512crypt, and HMAC-SHA1. This also reinforced the importance of understanding what the hash actually represents before blindly trying different cracking modes.
