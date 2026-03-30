## Challenge Writeup

The challenge provided some encoded string

It was known that the flag format follows: 
```bash
ctf7{
```

### Step 1: Identifying the Cipher
By comparing the given ciphertext with the expected flag format (`ctf7{`), it was clear that a **Caesar cipher** was used, but with an unknown shift.

### Step 2: Brute Forcing the Shift
Since the shift value was unknown, a brute-force approach was used. An online Caesar cipher decoder was used to try all possible shifts.

### Step 3: Retrieving the Flag
After testing all shifts, the correct plaintext was revealed.
