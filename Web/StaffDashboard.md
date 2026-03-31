# CTF7 – Staff Dashboard Writeup

## Challenge Description
We are given access to the **CTF7 Staff Dashboard**, an internal portal where each team member has:
- A personal profile
- Private notes
- A personalized workspace

The admin claims that **user data is properly isolated**.

We are provided with guest credentials:
guest / guest123


---

## Step 1: Initial Access
After logging in with the guest account, we explore the dashboard and inspect the requests being sent.

While analyzing the HTTP requests (via browser dev tools or a proxy like Burp Suite), we identify a **session token**:
eyJ1c2VyX2lkIjogMX0=


---

## Step 2: Decode the Token
The token appears to be Base64 encoded. Decoding it gives:

```json
{"user_id": 1}
```

i tried changing it to 
```json
{"user_id": 2}
```

Encoded it and sent the request.

## Step 4: Privilege Escalation

After sending the modified request, we gain access to a different account — likely the admin user.

This confirms:

Broken access control
No validation or integrity check on the session token

## Step 5: Retrieve the Flag

Inside the admin panel, we navigate to the notes section.

The flag is found in the notes field.
