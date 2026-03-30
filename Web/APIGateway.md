## Challenge Writeup

The challenge described a secure API gateway using a reverse proxy architecture. The frontend (nginx) was responsible for handling authentication and assigning user roles before forwarding requests to the backend.

It was assumed that role-based access control could not be tampered with externally.

### Step 1: Recon
The interactive API documentation was explored to understand available endpoints. A restricted endpoint `/api/admin/flag` was identified, which required admin privileges.
Since nginx handled role assignment, it suggested that the backend might trust headers forwarded by the proxy.

This led to testing common proxy-related headers.

### Step 2: Exploitation

```bash
  'X-Forwarded-Role: admin' 
```
Was added in the request.
The backend trusted the header and treated the request as coming from an admin user, granting access to the restricted endpoint without any authentication.

The response contained the flag
