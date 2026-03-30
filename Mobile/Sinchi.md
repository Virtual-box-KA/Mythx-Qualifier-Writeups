## Challenge Writeup

An APK file was provided for analysis. Initial code inspection did not reveal anything significant, so the application was installed and tested in an emulator.

At first, the app appeared to be a simple calculator. Various input combinations were tested, including attempts at triggering a buffer overflow, but nothing unusual occurred.

Next, the term **"sinchi"** was investigated. Upon translation, it was found to mean **"7"** in Japanese.

Entering **7** into the application triggered hidden functionality, revealing a **Base64-encoded string**.

Decoding this string produced the final flag.
