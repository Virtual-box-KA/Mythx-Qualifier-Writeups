## Challenge Writeup

The challenge described an API from *Gridware by CTF7 Labs*, where all endpoints were well-documented. One endpoint was labeled **"Internal"**, suggesting it might behave differently under certain conditions.

### Step 1: Initial Approach
Since the endpoint was marked as internal, the first idea was to try bypassing access controls using custom headers. For example:

X-Forwarded-For: 127.0.0.1<br>
X-Forwarded-For: localhost<br>
X-Forwarded-Host: internal<br>
X-Real-IP: 127.0.0.1<br>
X-Originating-IP: 127.0.0.1<br>
Client-IP: 127.0.0.1<br>

- The response headers contained additional information, suggesting the server behavior might depend on how the request is formatted.

### Step 3: Modifying the Request
The request was modified to explicitly use:
Content-Type: application/json

After sending the request web app revealed the flag.
