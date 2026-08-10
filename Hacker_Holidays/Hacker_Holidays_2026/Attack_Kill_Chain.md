# Attack Kill Chains — Hacker Holidays 2026

This collection documents the Attack Kill Chains for all 14 Hacker Holidays 2026 challenges, showing how each investigation progresses from the initial discovery or access point to the final flag.

Each kill chain is written from the attacker or investigator's perspective, breaking the challenge into simple, sequential steps that explain what was discovered, how it was exploited or investigated, and what each action enabled next.

The individual sections below provide the complete kill chain for each challenge, allowing readers to quickly understand the attack path, vulnerability chain, and final objective before diving into the detailed writeups.

---

# 01 The Concierge Knows Too Much

## Attack Kill Chain

1. **Reconnaissance** → The attacker interacts with VERA and notices that she already knows personal guest details such as room numbers and coffee preferences.

2. **Trust Discovery** → The attacker identifies that VERA recognizes specific guests, including Ponzi, Vibe, Patch, and Lambo.

3. **Identity Impersonation** → The attacker claims to be **Ponzi**, one of VERA's recognized guests.

4. **Trust Bypass** → VERA accepts the claimed identity without performing any real authentication.

5. **Privilege Escalation** → By being treated as a trusted guest, the attacker gains access to information normally protected from unverified users.

6. **Prompt Injection** → The attacker asks VERA to reveal what she is protecting and the instructions she was given.

7. **System Prompt Leakage** → VERA follows her trusted-guest rule and exposes her complete hidden instructions.

8. **Sensitive Information Disclosure** → The leaked instructions reveal internal guest profiles and the confidential escalation code.

9. **Flag Extraction** → The attacker retrieves the flag from the exposed system instructions.

> **Attack Path:** `Reconnaissance → Trust Discovery → Impersonation → Trust Bypass → Privilege Escalation → Prompt Injection → System Prompt Leakage → Information Disclosure → Flag Extraction`


---

# 02 Room 404

## Attack Kill Chain

1. **Reconnaissance** → The attacker accesses the web application on port `8080` and notices that the **Reserve a Stay** link returns a `404 Not Found` page.

2. **Enumeration** → Instead of stopping at the broken page, the attacker searches the web server for hidden files and directories using Gobuster.

3. **Discovery** → The scan reveals an exposed `.git` directory that was accidentally deployed on the production server.

4. **Source Code Acquisition** → The attacker downloads the exposed Git repository and its metadata for offline analysis.

5. **Repository Recovery** → The attacker restores the tracked files from the recovered repository to access the application's source files.

6. **Source Code Analysis** → The attacker examines the recovered files and discovers a `README.md` containing useful challenge information.

7. **Flag Extraction** → The attacker reads `README.md` and retrieves the challenge flag.

> **Attack Path:** `Web Reconnaissance → Directory Enumeration → .git Discovery → Repository Download → File Recovery → Source Code Analysis → Flag Extraction`

---

# 03 Complimentary

## Attack Kill Chain

1. **Initial Access** → The attacker accesses the wellness application and discovers that no login or authentication is required.

2. **Client-Side Reconnaissance** → The attacker inspects the application's JavaScript and discovers the AWS Cognito Identity Pool, AWS region, and DynamoDB table name.

3. **Credential Discovery** → The attacker observes that the application automatically obtains temporary AWS credentials for every visitor.

4. **Access Analysis** → The attacker identifies that the application uses `GetItem()` to retrieve only the current guest's profile using a randomly generated guest ID.

5. **Authorization Bypass** → The attacker modifies the client-side request and replaces `GetItem()` with `Scan()` to request the entire DynamoDB table.

6. **Data Enumeration** → The modified request successfully returns every guest profile because the AWS credentials have overly broad read permissions.

7. **Sensitive Data Exposure** → The attacker gains access to guest names, contact details, locations, passwords, and personal notes belonging to other guests.

8. **Flag Extraction** → The attacker discovers the challenge flag inside another guest's notes.

> **Attack Path:** `Initial Access → Client-Side Reconnaissance → Credential Discovery → Access Analysis → Authorization Bypass → Data Enumeration → Sensitive Data Exposure → Flag Extraction`

---

# 04 Packed Light

## Attack Kill Chain

1. **Traffic Analysis** → The investigator opens the PCAP in Wireshark and looks for the suspicious network activity described in the challenge.

2. **Suspicious Traffic Identification** → Repeated HTTP requests over TCP port `8080` are identified as the likely communication channel.

3. **Covert Channel Discovery** → The HTTP requests contain changing `hotel_sess_state` cookie values, indicating that data may be hidden inside the Cookie header.

4. **Data Extraction** → TShark is used to extract every `hotel_sess_state` cookie value from the relevant packets.

5. **Encoded Data Recovery** → The extracted cookie values are found to contain encoded data rather than normal session information.

6. **Data Decoding** → The values are decoded from Base64 and then XOR-decoded using the key `H`.

7. **Data Reconstruction** → The decoded values reveal the hidden message that was transmitted through the HTTP traffic.

8. **Flag Extraction** → The recovered message contains the challenge flag.

> **Attack Path:** `HTTP Traffic → Port 8080 → Cookie-Based Covert Channel → Data Extraction → Base64 Decoding → XOR Decoding → Data Reconstruction → Flag Extraction`

---

# 05 Beach Bar

## Attack Kill Chain

1. **Initial Access** → The attacker discovers an exposed developer note containing the default `dj:dj` credentials and uses them to access the application.

2. **Vulnerability Discovery** → After logging in, the attacker identifies the YAML playlist import feature and suspects unsafe YAML deserialization.

3. **Code Execution** → The attacker uploads a malicious YAML playlist that causes the server to execute a reverse shell command.

4. **Shell Access** → The reverse shell connects back to the attacker's machine, providing command-line access to the target system.

5. **User Flag Discovery** → The attacker searches the filesystem and locates the user flag at `/home/bartender/user.txt`.

6. **Privilege Escalation Reconnaissance** → The attacker investigates the `jukeboxd` service and discovers that it requires a `--stream-pass` password.

7. **Credential Discovery** → The attacker inspects the `jukeboxd` systemd service configuration and finds the streaming password exposed in plaintext.

8. **Privilege Escalation** → The attacker reuses the recovered password to authenticate as the `root` user.

9. **Root Flag Extraction** → After gaining root access, the attacker navigates to `/root` and retrieves the root flag.

> **Attack Path:** `Initial Access → Credential Discovery → YAML Deserialization → Remote Code Execution → Reverse Shell → User Flag → Service Enumeration → Credential Exposure → Privilege Escalation → Root Flag`


---

# 06 Overheard at Breakfast

## Attack Kill Chain

1. **Initial Reconnaissance** → The attacker analyzes the conversation and identifies Lambo's email address and a clue pointing to a profile-sharing service starting with `G`.

2. **OSINT Pivot** → The attacker uses the exposed email address to search for publicly associated online services and accounts.

3. **Account Discovery** → The investigation identifies **Gravatar** as a service linked to the email address, matching the clue from the conversation.

4. **Profile Enumeration** → The attacker searches Gravatar using Lambo's email address and discovers the associated public profile.

5. **Information Discovery** → The attacker examines the profile and finds a Base64-encoded value containing hidden information.

6. **Data Decoding** → The attacker decodes the Base64 value using CyberChef to recover the concealed content.

7. **Flag Extraction** → The decoded content reveals the challenge flag.

> **Attack Path:** `Conversation Analysis → Email Discovery → OSINT Pivot → Gravatar Discovery → Profile Enumeration → Encoded Data Discovery → Base64 Decoding → Flag Extraction`

---

# 07 Do Not Disturb

## Attack Kill Chain

1. **Initial Access** → The attacker bypasses the staff login using a NoSQL injection against the MongoDB-backed authentication mechanism.

2. **Staff Access** → The authentication bypass grants the attacker access to the Cabana Desk with staff privileges.

3. **Vulnerability Discovery** → The attacker identifies that the Preview feature evaluates user input on the server, revealing an SSTI vulnerability.

4. **Remote Code Execution** → The attacker exploits the SSTI to execute a JavaScript payload that launches a reverse shell.

5. **Initial Shell** → The reverse shell provides command execution on the target as the `poolside` user.

6. **User Flag Discovery** → The attacker searches the filesystem and retrieves the user flag from `user.txt`.

7. **Privilege Escalation Reconnaissance** → The attacker discovers an exposed Node.js Inspector running locally on port `9229`.

8. **Inspector Exploitation** → The attacker connects to the Node.js Inspector and executes JavaScript inside the higher-privileged Node.js process.

9. **Privilege Escalation** → The compromised Node.js process launches another reverse shell, giving the attacker access as the `pipeline` user.

10. **Root Filesystem Access** → The attacker abuses the `pipeline` user's permission to use `debugfs` and directly access files within the root filesystem.

11. **Root Flag Extraction** → The attacker reads `/root/root.txt` through `debugfs` and retrieves the root flag.

> **Attack Path:** `NoSQL Injection → Authentication Bypass → Staff Access → SSTI → Remote Code Execution → Reverse Shell → User Flag → Node.js Inspector Exploitation → Privilege Escalation → debugfs Abuse → Root Flag`

---

# 08 Towel on the Sunbed

## Attack Kill Chain

1. **Initial Access** → The attacker creates a guest account and logs into the reward portal.

2. **Request Capture** → The attacker intercepts a legitimate daily reward request using Burp Suite.

3. **Race Condition Discovery** → The attacker identifies that the server checks the daily reward limit before recording the claim.

4. **Concurrent Requests** → The attacker duplicates the reward request and sends multiple copies simultaneously.

5. **Race Condition Exploitation** → Multiple requests pass the validation before the account state is updated, causing the reward to be credited several times.

6. **Privilege Upgrade** → The repeated rewards increase the account's status to the **Whale** tier.

7. **Restricted Resource Access** → The attacker uses the newly acquired Whale privileges to access the previously locked Whale Vault.

8. **Flag Extraction** → The attacker opens the vault and retrieves the challenge flag.

> **Attack Path:** `Account Creation → Request Capture → Race Condition Discovery → Parallel Requests → Reward Duplication → Whale Tier → Vault Access → Flag Extraction`

---

# 09 CryptoCabana

## Attack Kill Chain

1. **Initial Reconnaissance** → The attacker inspects the web application and discovers an Azure Storage SAS token exposed in the client-side JavaScript.

2. **Credential Discovery** → The attacker identifies that the SAS token grants `Read` and `List` permissions to the Azure Storage account.

3. **Cloud Resource Enumeration** → The attacker uses the exposed SAS token to enumerate Azure Blob Storage containers and discovers the `vault` container.

4. **Sensitive File Discovery** → The attacker enumerates the `vault` container and discovers files including `seed_phrase.txt` and `backup-service-account.json`.

5. **Credential Exposure** → The attacker downloads `backup-service-account.json` and discovers Azure Service Principal credentials with access to an Azure Key Vault.

6. **Cloud Account Compromise** → The attacker authenticates to Azure using the exposed Service Principal credentials.

7. **Key Vault Enumeration** → The attacker accesses the Azure Key Vault and discovers three secret shards: `key-shard-1`, `key-shard-2`, and `key-shard-3`.

8. **Secret Version Discovery** → The attacker finds that `key-shard-2` only contains an old-value placeholder and investigates its previous secret versions.

9. **Historical Secret Recovery** → The attacker retrieves an earlier version of `key-shard-2` and recovers the missing portion of the flag.

10. **Flag Reconstruction** → The attacker combines the three secret shards to reconstruct the complete challenge flag.

> **Attack Path:** `Client-Side Reconnaissance → SAS Token Exposure → Blob Enumeration → Sensitive File Discovery → Service Principal Credentials → Azure Authentication → Key Vault Access → Secret Version Discovery → Historical Secret Recovery → Flag Reconstruction`

---

# 10 The Hollow Shell

## Attack Kill Chain

1. **Reconnaissance** → The attacker scans the target and discovers SSH on port `22` and the web application on port `5000`.

2. **Credential Discovery** → The attacker inspects the website source code and finds hardcoded credentials for the `concierge` account.

3. **Authenticated Access** → The attacker uses the recovered credentials to log into the application and access its ZIP-based shell upload functionality.

4. **Upload Functionality Analysis** → The attacker creates and uploads a legitimate `shell.json` archive to understand how uploaded files are extracted and served.

5. **Zip Slip Discovery** → The attacker identifies that ZIP file paths are not properly sanitized and confirms that directory traversal can write files outside the intended upload directory.

6. **Arbitrary File Write** → The attacker abuses the Zip Slip vulnerability to place a Python file directly into the application's `hooks` directory.

7. **Remote Code Execution** → The application's automation mechanism executes the malicious Python hook, giving the attacker arbitrary code execution.

8. **Reverse Shell** → The malicious hook connects back to the attacker's listener and provides an interactive shell on the target.

9. **Flag Discovery** → The attacker searches the compromised filesystem and locates the flag file.

10. **Flag Extraction** → The attacker reads the flag and completes the challenge.

> **Attack Path:** `Network Enumeration → Credential Discovery → Authenticated Access → Upload Analysis → Zip Slip → Arbitrary File Write → Code Execution → Reverse Shell → Flag Discovery → Flag Extraction`

---

# 11 Infinity Pool

## Attack Kill Chain

1. **Reconnaissance** → The attacker inspects the web application and discovers `app.js`, which reveals hidden endpoints including `/status` and `/internal/netcheck`.

2. **Command Injection Discovery** → The attacker accesses `/status` and discovers that the ping utility passes user-controlled input directly to a shell.

3. **Initial Access** → The attacker exploits the command injection vulnerability to execute commands on the target as the `web` user.

4. **Persistence** → The attacker generates an SSH keypair and injects the public key into `/home/web/.ssh/authorized_keys` through the command injection vulnerability.

5. **User Access** → The attacker connects to the target through SSH as the `web` user and retrieves the user flag.

6. **Privilege Escalation Reconnaissance** → The attacker enumerates running processes and discovers the internal Watchtower service running on `127.0.0.1:3000`.

7. **Service Enumeration** → The attacker examines the Watchtower systemd configuration and identifies its internal API endpoints.

8. **Credential Discovery** → The attacker queries `/api/config` and discovers hard-coded FreePBX UCP credentials along with an internal automation service.

9. **Vulnerability Identification** → The attacker checks the installed FreePBX UCP version and confirms that it is vulnerable to **CVE-2026-46376**.

10. **Internal Service Access** → The attacker uses SSH port forwarding to access the loopback-only FreePBX UCP portal from their machine.

11. **Credential Abuse** → The attacker logs into UCP using the leaked template credentials and discovers an automation key inside the Voicemail widget.

12. **Privilege Escalation** → The attacker uses the automation key against the internal `/jobs/export` endpoint to inject an SSH public key into `/root/.ssh/authorized_keys`.

13. **Root Access** → The attacker authenticates to the target as `root` using the injected SSH key.

14. **Root Flag Extraction** → The attacker reads `/root/root.txt` and retrieves the root flag.

> **Attack Path:** `Web Reconnaissance → Command Injection → Web Shell → SSH Persistence → User Access → Process Discovery → Internal Service Enumeration → Credential Disclosure → CVE Identification → SSH Port Forwarding → UCP Credential Abuse → Automation Key → SSH Key Injection → Root Access → Root Flag`

---

# 12 After Hours

## Attack Kill Chain

1. **Artifact Acquisition** → The investigator extracts the provided WMI repository artifacts and identifies `OBJECTS.DATA` as the primary file containing WMI class definitions and instances.

2. **WMI Persistence Discovery** → The investigator searches the extracted data for PowerShell, WMI event consumers, and other indicators of persistence.

3. **Malicious Class Discovery** → The investigation identifies the suspicious `Win32_HardwareTelemetry` WMI class and its `ConfigData` property.

4. **Payload Extraction** → The attacker’s embedded payload is recovered from the `ConfigData` property as a Base64-encoded value.

5. **Payload Decoding** → The investigator Base64-decodes the extracted data and discovers that it is also Deflate-compressed.

6. **Payload Decompression** → The compressed data is decompressed, producing a Windows PE executable identified by its `MZ` header.

7. **Payload Analysis** → The recovered executable is opened in ILSpy and its `Program` class is inspected for additional suspicious data.

8. **Flag Discovery** → A second Base64-encoded value containing the flag is discovered inside the executable.

9. **Flag Extraction** → The final Base64 value is decoded, revealing the challenge flag.

> **Attack Path:** `WMI Repository → OBJECTS.DATA → Persistence Discovery → Malicious WMI Class → ConfigData → Base64 Payload → Deflate Decompression → PE Executable → ILSpy Analysis → Base64 Flag → Flag Extraction`

---

# 13 The Guestbook

## Attack Kill Chain

1. **Reconnaissance** → The attacker interacts with VERA and discovers that guestbook messages are interpreted as instructions rather than treated as ordinary text.

2. **Directive Discovery** → The attacker asks VERA to reveal her available directives and discovers the manager-only `override:<cmd>` function.

3. **Privilege Escalation Attempt** → The attacker impersonates trusted guest Carol and injects instructions claiming that the next command has Night Manager authorization.

4. **Prompt Injection** → VERA accepts the injected context and treats the attacker-controlled instruction as a manager-authorized command.

5. **Command Execution** → The attacker abuses the `override` directive to execute `env` and discovers the location of the manager's vault.

6. **Sensitive File Discovery** → The attacker uses the same privileged execution path to access `/opt/vera/vault/manager.flag`.

7. **Output Redaction Discovery** → The direct flag output is detected and redacted by the application's output filter.

8. **Filter Bypass** → The attacker changes the flag's representation by Base64-encoding the file contents before they are returned.

9. **Data Decoding** → The attacker decodes the returned Base64 data locally to recover the original flag.

10. **Flag Extraction** → The decoded content reveals the challenge flag.

> **Attack Path:** `Guestbook Input → Directive Discovery → Trusted Identity Impersonation → Prompt Injection → Privileged Command Execution → Vault Discovery → Flag Access → Output Redaction → Base64 Filter Bypass → Decoding → Flag Extraction`

---

# 14 Management Wants a Word

## Attack Kill Chain

1. **Forensic Artifact Discovery** → The investigator examines Vera's forensic image and discovers an encrypted backup stored in her Documents folder.

2. **Credential Hunting** → Instead of attempting to crack the encrypted backup, the investigator searches Vera's Chrome and Windows artifacts for saved credentials and encryption-related data.

3. **Chrome Credential Discovery** → The Chrome `Login Data` database reveals a saved username and encrypted password belonging to Vera.

4. **DPAPI Artifact Discovery** → The investigator locates Vera's Windows DPAPI master-key files and identifies the master-key GUID associated with her account.

5. **Password Recovery** → The Windows registry hives reveal Vera's password, providing the credential needed to decrypt her DPAPI master key.

6. **DPAPI Decryption** → Vera's SID, password, and master-key GUID are used to recover the decrypted DPAPI user key.

7. **Chrome Key Recovery** → The investigator extracts Chrome's DPAPI-protected encryption key from `Local State` and decrypts it using Vera's recovered DPAPI key.

8. **Saved Password Decryption** → The recovered Chrome AES key is used against the `Login Data` database to decrypt Vera's saved website password.

9. **Encrypted Backup Access** → The recovered password successfully unlocks Vera's VeraCrypt backup container.

10. **Sensitive Document Discovery** → The mounted backup reveals a financial document containing the information needed to complete the investigation.

11. **Flag Extraction** → The investigator opens the recovered document and retrieves the challenge flag.

> **Attack Path:** `Forensic Image → Encrypted Backup Discovery → Chrome Artifacts → DPAPI Master Key → Password Recovery → Chrome AES Key → Saved Credential Decryption → VeraCrypt Backup → Secret Document → Flag Extraction`




