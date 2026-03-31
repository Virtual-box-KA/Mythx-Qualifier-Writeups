# 🔍 Audio Steganography Write-up: Hidden Data in a 440 Hz Tone

## 🧠 Challenge Overview
We were given a `.wav` audio file that appeared to be a simple **440 Hz sine wave**—commonly used as a calibration tone. However, based on intelligence, the file was suspected to contain **hidden data embedded within the signal**.

This is a classic case of **audio steganography**, where information is concealed inside audio samples in a way that is not perceptible to human hearing.

---

## ⚙️ Key Idea: Least Significant Bit (LSB) Encoding

Digital audio stores sound as a sequence of samples (integers). Each sample is typically 16 bits.

The trick used here:
- The **least significant bit (LSB)** of each sample is modified.
- Changing the LSB does **not significantly affect the sound**, making it ideal for hiding data.
- Each LSB represents **1 bit of hidden information**.

So:
- 8 samples → 1 byte → 1 character

---

## 🧪 Extraction Strategy

We:
1. Read raw audio samples.
2. Extract the **LSB of each sample**.
3. Group bits into chunks of 8.
4. Convert each chunk into a character.
5. Stop when we hit a null byte (`0`), signaling the end of the message.

---

## 💻 Code

```python
import wave, struct

with wave.open('challenge.wav', 'rb') as f:
    data = struct.unpack('<{}h'.format(f.getnframes()), f.readframes(f.getnframes()))

bits = [x & 1 for x in data]

msg = ''
for i in range(0, len(bits), 8):
    b = bits[i:i+8]
    if len(b) < 8: break
    n = int(''.join(map(str, b)), 2)
    if n == 0: break
    msg += chr(n)

print(msg)
```

### Final Flag 
```bash
ctf7{transmission_succesfull_tha_kya_b06f083c}
```
