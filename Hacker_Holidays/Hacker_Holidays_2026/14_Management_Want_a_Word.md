# Title: Management Wants a Word

#### Category: Forensics

#### Difficulty: Medium

#### Description: 

It was always her. It was never a bug; it was the business model.

---

## Task 1: Hacker Holidays Storyline: Act 4 - Sunrise

<img width="757" height="608" alt="image" src="https://github.com/user-attachments/assets/8b65aa62-c221-4ec0-911c-deda99975adc" />

<img width="759" height="467" alt="image" src="https://github.com/user-attachments/assets/c996e80e-4b2e-4c54-987c-0b5a1a3e584a" />


---

## Task 2: Hacker Holidays: Day 14

<img width="668" height="637" alt="image" src="https://github.com/user-attachments/assets/17d0d78c-fe31-46c6-bee7-633ccd6bb8be" />

<img width="669" height="497" alt="image" src="https://github.com/user-attachments/assets/e111f2de-2402-470c-a9e5-4851fd50837d" />

#### Analysis: 
The provided clues indicate that Vera's laptop contains residual artifacts from her previous activity. The reference to the browser suggests that saved credentials or related browser artifacts may be important, while the version number 1.26.29 may provide an additional lead. Since the system underwent a full forensic triage before being wiped, the objective is to trace these remaining artifacts and identify the password Vera unintentionally left behind.

---

## Methodology

#### Step 1 – Extract the Challenge Files

I started by extracting the provided archive.

```unzip management-wants-a-word-forensics-hh-day-14-1785854680266.zip```

Then I entered the extracted directory.

```
cd management-wants-a-word-forensics-hh-day-14/KAPE
````

<img width="784" height="73" alt="image" src="https://github.com/user-attachments/assets/57250ec1-e538-431d-91df-c0cc7918313a" />

The important part here is that I needed to work from the KAPE directory because this contains the forensic artifacts from Vera's machine.

#### Step 2 – Identify the Interesting Artifacts

One of the first things I noticed was:

```
C/Users/vera/Documents/backup
```

I checked what this file actually was.

```
file C/Users/vera/Documents/backup
```

I also examined the beginning of the file:

```
xxd -l 64 C/Users/vera/Documents/backup
```

The backup appeared to be an encrypted container.

This immediately raised an important question:
> Where can I find the password for this backup?

Instead of trying to crack the container, I started looking for credentials and passwords that Vera may have saved on the machine.

#### Step 3 – Locate Vera's Chrome Artifacts

The challenge specifically hints that the browser has a "good memory."

Chrome stores several useful forensic artifacts.

The most important ones for this investigation are:


Login Data: Saved usernames and encrypted passwords
Local State: Chrome's encrypted AES key. 
Web Data: Autofill Information 
History: Browser activity and searches

I searched Vera's profile for these files.

Command:

```
find C/Users/vera -type f \
  \( -iname 'Login Data' \
  -o -iname 'Local State' \
  -o -iname 'Web Data' \
  -o -iname 'History' \) -print
```

Result:

```
C/Users/vera/AppData/Local/Google/Chrome For Testing/User Data/Default/Web Data
C/Users/vera/AppData/Local/Google/Chrome For Testing/User Data/Default/Login Data
C/Users/vera/AppData/Local/Google/Chrome For Testing/User Data/Default/History
C/Users/vera/AppData/Local/Google/Chrome For Testing/User Data/Local State
```

Now I had the Chrome artifacts I needed.


#### Step 4 – Examine Chrome's Saved Credentials

Why are we examining saved browser credentials?
> Because the challenge tells us that Vera's browser "remembered" something.

If Vera previously logged into a website and chose to save her credentials, Chrome may have a record of:

- the website
- username
- encrypted password

The password itself isn't immediately readable, but the Login Data database gives us the encrypted credential we need to work with.

I opened the database with SQLite.

```sqlite3 'C/Users/vera/AppData/Local/Google/Chrome For Testing/User Data/Default/Login Data'```

Then I enabled readable output.

```
.headers on
```
```
.mode column
```

I queried the login records.

```
SELECT
    origin_url,
    action_url,
    username_value,
    hex(password_value) AS encrypted_password
FROM logins;
```

<img width="1115" height="239" alt="image" src="https://github.com/user-attachments/assets/a4f507cf-e36b-4246-b3c0-e6a594e6b753" />

This gave me a username and an encrypted password.

The important discovery here was that the password exists, but Chrome has protected it.

So I needed to understand how Windows protects Chrome credentials.

Exit SQLite:

```.quit```


#### Step 5 – Find Vera's DPAPI Master Key

Why do we need the DPAPI Master Key?
> Windows uses DPAPI — Data Protection API to protect sensitive information belonging to users.

Chrome can use DPAPI to protect the key that ultimately protects saved browser credentials.

So our chain is becoming:

```
Chrome Login Data
        ↓
Encrypted Chrome key
        ↓
Windows DPAPI
        ↓
Vera's DPAPI Master Key
```

DPAPI master keys are normally stored under the user's:
> AppData\Roaming\Microsoft\Protect

I searched that directory.

```
find C/Users/vera/AppData/Roaming/Microsoft/Protect \
  -type f -printf '%f %p\n'
```

<img width="1335" height="108" alt="image" src="https://github.com/user-attachments/assets/2877878f-0d1a-4e5b-96f7-a841dcb588af" />


This revealed Vera's SID and the master-key GUID.
```
SID:
S-1-5-21-2529683458-431225740-1723070931-1000

Master-key GUID:
c90719ef-5b98-474e-b934-136d606a702a
```
The important thing here is that we now know which DPAPI master key belongs to Vera.


#### Step 6 – Examine the Windows Registry Hives

What can we use to decrypt Vera's DPAPI master key?

The Windows registry hives are particularly interesting here.

The relevant hives are:

```
C/Windows/System32/config/SAM
C/Windows/System32/config/SYSTEM
C/Windows/System32/config/SECURITY
```
These contain sensitive Windows security information.

I used impacket-secretsdump to examine them.

What is impacket-secretsdump?
> secretsdump is an Impacket utility used to extract Windows credential-related secrets from registry hives and other sources.

In this case, we're using it against the offline SAM, SYSTEM, and SECURITY files.

```
impacket-secretsdump \
  -sam C/Windows/System32/config/SAM \
  -system C/Windows/System32/config/SYSTEM \
  -security C/Windows/System32/config/SECURITY \
  LOCAL
```

<img width="1088" height="538" alt="image" src="https://github.com/user-attachments/assets/f54c2622-0fe6-438c-9a29-3c8e6e6dacc5" />

The output revealed:

```
[*] DefaultPassword 
(Unknown User):minivera
```

This was a major discovery.

We now have a password that can potentially be used to decrypt Vera's DPAPI material.


#### Step 7 – Decrypt Vera's DPAPI Master Key

Now that I know:

```
Vera's SID
Vera's master-key GUID
Vera's password
```

I can attempt to decrypt the master key.

```
impacket-dpapi masterkey \
  -file 'C/Users/vera/AppData/Roaming/Microsoft/Protect/S-1-5-21-2529683458-431225740-1723070931-1000/c90719ef-5b98-474e-b934-136d606a702a' \
  -sid 'S-1-5-21-2529683458-431225740-1723070931-1000' \
  -password 'minivera'
```

<img width="1172" height="360" alt="image" src="https://github.com/user-attachments/assets/380573c4-e794-4de5-9c85-afc198aa1b85" />

What is happening here?

The command takes the encrypted DPAPI master-key file and uses:

- Vera's SID
- Vera's password

to derive the information needed to decrypt it.

The result gives us the decrypted User Key.

I received:
> 0x5e5715ec9b6df5a86e97902692a66d28e691f05d5bc1e04d0159cfe960e94c978c07e5004a0179d3a96df2468885a28175b0b02cc064445f116a752d2b3e9d40

I stored it in a variable so I could easily reuse it.

```MASTERKEY='5e5715ec9b6df5a86e97902692a66d28e691f05d5bc1e04d0159cfe960e94c978c07e5004a0179d3a96df2468885a28175b0b02cc064445f116a752d2b3e9d40'```


#### Step 8 – Locate Chrome's Local State

We now have Vera's decrypted DPAPI key.

But we aren't finished.

Remember the encrypted password from Chrome?

Chrome uses an encryption hierarchy, so we still need to recover the Chrome AES key.

That information is stored in Chrome's Local State file.

I located it and stored the path in a variable.

```
LOCAL_STATE="$(find "$PWD/C/Users/vera" \
  -type f -iname 'Local State' -print -quit)"
```
Then I checked the variable.

```
printf '%s\n' "$LOCAL_STATE"
```

This gave me:
> C/Users/vera/AppData/Local/Google/Chrome For Testing/User Data/Local State

<img width="1265" height="144" alt="image" src="https://github.com/user-attachments/assets/d71ff342-9e70-4838-b8e2-f3b3fd488766" />


#### Step 9 – Extract Chrome's Encrypted Key

Inside Local State, Chrome stores the encrypted key under:
> os_crypt.encrypted_key

The value is Base64 encoded.

I extracted and decoded it:

```
jq -r '.os_crypt.encrypted_key' "$LOCAL_STATE" |
base64 -d |
xxd
```
<img width="785" height="449" alt="image" src="https://github.com/user-attachments/assets/18f5c68b-8218-473b-98e6-9353eb24b2ed" />

The output showed that the decoded data begins with:
> DPAPI

This is important because it tells us that Chrome's key is protected using Windows DPAPI.

Why are we doing this?

We already recovered Vera's DPAPI master key.

Now we can use it to decrypt this Chrome-specific DPAPI blob.

So the chain is now:

```
Vera password
      ↓
DPAPI Master Key
      ↓
Chrome encrypted key
      ↓
Chrome AES key
```


#### Step 10 – Remove the DPAPI Header

The Chrome encrypted key contains a five-byte DPAPI prefix.

I removed that prefix and saved the remaining data into a separate file.

```
jq -r '.os_crypt.encrypted_key' "$LOCAL_STATE" |
base64 -d |
tail -c +6 > chrome-key.dpapi
```

We now have:
> chrome-key.dpapi

This is the DPAPI-protected Chrome key.


#### Step 11 – Decrypt Chrome's AES Key

Now I used the previously recovered MASTERKEY together with chrome-key.dpapi.

```
python3 - "$MASTERKEY" <<'PY'
import sys
from impacket.dpapi import DPAPI_BLOB

masterkey = bytes.fromhex(sys.argv[1])

with open("chrome-key.dpapi", "rb") as f:
    blob = DPAPI_BLOB(f.read())

decrypted = blob.decrypt(masterkey)

if decrypted is None:
    raise SystemExit("DPAPI decryption failed")

with open("chrome-aes.key", "wb") as f:
    f.write(decrypted)

print(f"Wrote {len(decrypted)} bytes")
print(f"Chrome AES key: {decrypted.hex()}")
PY
```

What happened here?
> This is the key step that connects Windows DPAPI to Chrome.

The script:

1. Converts the master key from hexadecimal into bytes.
2. Reads chrome-key.dpapi.
3. Treats it as a DPAPI blob.
4. Uses Vera's decrypted DPAPI master key to decrypt it.
5. Writes the resulting Chrome AES key to chrome-aes.key.


<img width="782" height="522" alt="image" src="https://github.com/user-attachments/assets/e959f493-03c6-4b33-8903-e415533486ee" />

The result was a 32-byte AES key:
> 206a39a0971327ea9487e4aea9844f5d3670162456982276939a712646da0b02

Now we finally have the key Chrome uses to encrypt the saved passwords.


#### Step 12 – Locate Chrome's Login Database

Earlier, we found the Login Data database.

I stored its path in a variable:


```
LOGIN_DATA="$(find "$PWD/C/Users/vera" \
  -type f -iname 'Login Data' -print -quit)"
```

Then I verified it:

```
printf '%s\n' "$LOGIN_DATA"
````

<img width="1328" height="151" alt="image" src="https://github.com/user-attachments/assets/5a2037f6-21b6-4686-8edf-219546e5bff7" />

This gives us a convenient reference to the database containing Vera's encrypted credentials.


#### Step 13 – Decrypt Vera's Saved Chrome Password

Now we have everything required:

```
Login Data
      +
Chrome AES Key
      ↓
Decrypted credentials
```

I used the following Python script:

```
python3 - "$LOGIN_DATA" ./chrome-aes.key <<'PY'
import sqlite3
import sys
from pathlib import Path
from cryptography.hazmat.primitives.ciphers.aead import AESGCM

database = Path(sys.argv[1]).resolve()
keyfile = Path(sys.argv[2]).resolve()

if not database.is_file():
    raise SystemExit(f"Missing database: {database}")

key = keyfile.read_bytes()

if len(key) != 32:
    raise SystemExit(f"Unexpected AES key length: {len(key)}")

db = sqlite3.connect(database.as_uri() + "?mode=ro", uri=True)

for url, username, encrypted in db.execute("""
    SELECT origin_url, username_value, password_value
    FROM logins
"""):
    blob = bytes(encrypted)

    if not blob.startswith((b"v10", b"v11")):
        print(f"Unsupported format: {blob[:10]!r}")
        continue

    nonce = blob[3:15]
    ciphertext_and_tag = blob[15:]

    password = AESGCM(key).decrypt(
        nonce,
        ciphertext_and_tag,
        None
    ).decode("utf-8", errors="replace")

    print(f"URL:      {url}")
    print(f"Username: {username}")
    print(f"Password: {password}")
PY
```
What does this script do?

- Opens Chrome's Login Data database.
- Reads the saved website, username, and encrypted password.
- Loads the 32-byte Chrome AES key.
- Checks that the credential uses Chrome's v10 or v11 format.
- Extracts the encryption nonce.
- Uses AES-GCM to decrypt the password.
- Prints the recovered credentials.


<img width="786" height="846" alt="image" src="https://github.com/user-attachments/assets/b35729f5-02f2-4a24-9c6f-5cbf2998bae9" />

The result was:
```
URL:      http://bytelotus.thm:8080/
Username: VeraSecretVault
Password: Wh4t1sV3raD0inG0nTh1sH0st
```

This was exactly what I was looking for.

We now have the password needed to access Vera's encrypted backup.


#### Step 14 – Open Vera's Encrypted Backup

Earlier, we discovered:

> C/Users/vera/Documents/backup

This turned out to be a VeraCrypt container.

I didn't need the VeraCrypt GUI because Linux cryptsetup supports VeraCrypt containers.

I opened it using:

```
sudo cryptsetup tcryptOpen \
  --veracrypt \
  'C/Users/vera/Documents/backup' \
  vera_backup
```

When prompted for the password, I entered the password recovered from Chrome: 
> Wh4t1sV3raD0inG0nTh1sH0st

What is happening here?
> The encrypted backup is being opened as a mapped encrypted device.
The name:
> vera_backup
is the name I gave the mapped device.


#### Step 15 – Mount the Backup

I created a mount point:
```
sudo mkdir -p /mnt/vera
```

Then mounted the decrypted filesystem read-only:
```
sudo mount -o ro /dev/mapper/vera_backup /mnt/vera
```

Using -o ro is important in a forensic investigation because it mounts the filesystem read-only, reducing the chance of modifying evidence.


#### Step 16 – Locate the Secret Documents

I checked the directory containing the financial documents.

```
ls /mnt/vera/secret_financial_documents/
```

This revealed:

> important_invoice_byte_lotus.pdf

<img width="788" height="143" alt="image" src="https://github.com/user-attachments/assets/8a0bd78c-136e-4c0a-b136-f1d84c37a865" />

I opened the PDF:

```
xdg-open /mnt/vera/secret_financial_documents/important_invoice_byte_lotus.pdf 
```

The flag was contained inside the document.

<img width="968" height="800" alt="image" src="https://github.com/user-attachments/assets/7aeac8b8-4018-4b1d-8307-301b2ce66498" />


#### Step 17 – Clean Up

Since this is a forensic investigation, I also cleanly unmounted the evidence.

```
sudo umount /mnt/vera
```

Then I closed the encrypted mapping:
```
sudo cryptsetup close vera_backup
```
This leaves the encrypted container closed instead of leaving the mapped filesystem active.

---

### Today's Itinerary

1. Take a closer look at what she left behind
> I extracted the forensic image and searched Vera's profile for browser and Windows artifacts. The Chrome Login Data, Local State, DPAPI files, and registry hives gave me the pieces needed to reconstruct her stored credentials.

2. Some things aren't as locked away as she thought
> I followed the encryption chain from Chrome to Windows DPAPI. The registry revealed Vera's password, which allowed me to decrypt her DPAPI master key and then recover Chrome's AES key and saved website password.

3. FInd out what she was hiding and claim the flag
> The recovered Chrome password opened Vera's encrypted backup. I mounted the VeraCrypt container read-only, located the financial document, opened the PDF, and recovered the flag.


---

### Flag
> THM{1t_w4s_V3r4_A11_Al0ng?!}

This challenge demonstrated how forensic investigations often require connecting small pieces of evidence rather than finding the flag directly.

The key lesson for me was that encryption does not necessarily make evidence inaccessible. If the necessary credentials, keys, and artifacts are still present on the system, they can sometimes be reconstructed from the surrounding forensic evidence.

The browser's "good memory" clue was especially important, as it ultimately helped uncover the information needed to continue the investigation and recover the flag.
