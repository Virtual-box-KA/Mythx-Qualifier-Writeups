# Client-Side Secret Extraction (CTF Approach)

## Overview
The application stored sensitive data (flag) inside obfuscated JavaScript, relying on client-side verification.

---

## Steps

### 1. Recon
- Visit:
  - `/`
  - `/about`
  - `/docs`
- Look for hints about client-side logic or hidden secrets.

---

### 2. Inspect JavaScript
- Open DevTools (`F12`) → **Sources**
- Locate JS files and possible `.map` files
- Search for:
  - Obfuscated code
  - Numeric arrays
  - Decoding functions

---

### 3. Identify Encoding
Common pattern:
```js
String.fromCharCode(value)
```

Indicates ASCII decoding

### 4. Extract Arrays
### 5. Decode
```bash
String.fromCharCode(...array)
```
### 6. Get Flag
Combine decoded parts flag revealed.
