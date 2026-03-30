I started by looking at the ciphertext provided by the challenge:

d1ca9f3fbccad18b57b5d7dd967ea2c0db9d57acd7c7a66da5d4dd983ef581c3

The challenge description mentioned that the service exposes an encryption endpoint and that it uses XOR encryption. A very common mistake in these kinds of challenges is that the service uses a short repeating XOR key.

Since the flag format is given as:

**ctf7{...}**

we already know the first 5 characters of the plaintext:

**ctf7{**

The ASCII values of these characters are:

**c  = 0x63
t  = 0x74
f  = 0x66
7  = 0x37
{  = 0x7b**

So the beginning of the plaintext in hex is:

63 74 66 37 7b

Next, I took the first 5 bytes of the ciphertext:

d1 ca 9f 3f bc

For XOR encryption:

ciphertext = plaintext XOR key

and because XOR is reversible:

key = ciphertext XOR plaintext

So I XORed each ciphertext byte with the corresponding known plaintext byte:

d1 XOR 63 = b2
ca XOR 74 = be
9f XOR 66 = f9
3f XOR 37 = 08
bc XOR 7b = c7

This gave the key:

b2 be f9 08 c7

or:

**b2bef908c7**

At this point, it is very standard to assume that the key repeats across the whole ciphertext. So the ciphertext would be decrypted like this:

ciphertext[0] XOR key[0]
ciphertext[1] XOR key[1]
ciphertext[2] XOR key[2]
ciphertext[3] XOR key[3]
ciphertext[4] XOR key[4]
ciphertext[5] XOR key[0]
...

I repeated the 5-byte key across the full ciphertext and XORed every byte.

Using the repeating key:

b2 be f9 08 c7 b2 be f9 08 c7 ...

the ciphertext decrypted cleanly to:

**ctf7{xor_recovered_key_ebfca623}**

So the final flag is:

ctf7{xor_recovered_key_ebfca623}

The key idea behind the solve is that repeating-key XOR is weak whenever part of the plaintext is known.
