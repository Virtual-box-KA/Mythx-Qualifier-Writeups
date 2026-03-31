# CTF Writeup: Disallowed Path

## Challenge Overview
The web application provided the hint:  
👉 **"Disallowed path"**

This suggested that hidden or restricted endpoints might be exposed through common web files.

---

## Reconnaissance

### Step 1: Check `robots.txt`
- Tried accessing:
  ```bash
  /robots.txt
  ```
  Got 404 not found

### Step 2: Check `sitemap.xml`
- Accessed:
  ```bash
  /sitemap.xml
  ```
- Found a hidden endpoint:
  ```
  /c7f8-staging-panel
  ```

---

Navigated to this endpoint got the flag.
