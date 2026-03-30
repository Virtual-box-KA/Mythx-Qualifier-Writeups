

The challenge gives two RSA public keys and says the message was encrypted with the first one:

* `n1`, `e1`
* `n2`, `e2`
* ciphertext

At first I tried checking if either modulus could be factored directly on FactorDB, but neither `n1` nor `n2` was immediately factorized.

Since there were two RSA moduli, I suspected they might share a prime. This is a common RSA mistake when key generation reuses one of the primes.

So instead of factoring them separately, I computed:

from math import gcd
p = gcd(n1, n2)  
print(hex(p))


This returned a non-trivial common factor, meaning both moduli share the same prime `p`.

Then I recovered the second prime of the first key:


**q = n1 // p**


Now the first modulus is fully factored:


**n1 = p * q**


Next I computed Euler's totient:

**phi = (p - 1) * (q - 1)**


Using `e1 = 65537`, I generated the private exponent:


**d = pow(e1, -1, phi)**


Finally I decrypted the ciphertext:


**m = pow(ciphertext, d, n1)**


Then converted the result from hex to bytes:


flag = bytes.fromhex(hex(m)[2:]).decode()
print(flag)


Output:

**ctf7{reused_prime_disaster_f6e3807d}**


The vulnerability was that both RSA keypairs reused the same prime, so taking `gcd(n1, n2)` immediately revealed one of the factors and completely broke the first key.
