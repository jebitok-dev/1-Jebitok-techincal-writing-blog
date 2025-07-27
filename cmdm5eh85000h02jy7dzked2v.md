---
title: "Challenges: DarkMatter (TryHackMe)"
datePublished: Sun Jul 27 2025 20:45:09 GMT+0000 (Coordinated Universal Time)
cuid: cmdm5eh85000h02jy7dzked2v
slug: challenges-darkmatter-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753649057564/d7819f83-3937-4ed2-a737-3ea6d330a3fa.png
tags: decryption, python, encryption, tryhackme, ransomwareattack, ctf-writeup

---

The Hackfinitiy High School became a victim of DarkInjector’s ransomware, resulting in the encryption of critical files, including academic records. Our task was to reverse-engineer the ransomware, locate its cryptographic weaknesses, and recover the encrypted data. Through system analysis, we discovered that the ransomware stored debug information in `/tmp`, including the RSA public key used to encrypt the AES session key. Using cryptographic techniques and factorization, we derived the RSA private key, decrypted the AES key, and eventually unlocked the encrypted documents  

The Hackfinitiy high school has been hit by DarkInjector's ransomware, and some of its critical files have been encrypted. We need you and Void to use your crypto skills to find the RSA private key and restls ore the files. After some research and reverse engineering, you discover they have forgotten to remove some debugging from their code. The ransomware saves this data to the tmp directory.

Can you find the RSA private key?

Note:

**You can close the window prompting for a password after the VM has booted; this will not affect the challenge.If you close the ransomware note before solving the challenge, you might need to reboot the VM.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753648516387/d43349c8-3c4d-4bce-ae69-c49682cc7462.png align="center")

`ls -la /tmp`

`cat /tmp/public_key.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753648479304/677885af-cbd7-4b70-bd4d-144f9e0df95e.png align="center")

[script.py](http://script.py)

```python
from sympy import factorint
n = 340282366920938460843936948965011886881
factors = factorint(n)
print(factors)
```

{18446744073709551533: 1, 18446744073709551557: 1}

```python
from sympy import mod_inverse

# RSA public key values
n = 340282366920938460843936948965011886881
e = 65537
p = 18446744073709551533
q = 18446744073709551557

# Compute phi(n)
phi = (p - 1) * (q - 1)

# Compute private exponent d
d = mod_inverse(e, phi)

print("Private exponent (d):", d)

```

Private exponent (d): 196442361873243903843228745541797845217

```python
from Crypto.PublicKey import RSA

# Values
n = 340282366920938460843936948965011886881
e = 65537
d = 196442361873243903843228745541797845217
p = 18446744073709551533
q = 18446744073709551557

# Construct private key
key = RSA.construct((n, e, d, p, q))
private_key_pem = key.export_key()

print(private_key_pem.decode())

```

\----BEGIN RSA PRIVATE KEY----- MGQCAQACEQD/////////cgAAAAAAABMhAgMBAAECEQCTyWw2k8lr43J8jYNyfJjh AgkA/////////60CCQD/////////xQIJAPCUD2vwlA8dAggVsepOFbHqSQIJAMqq qqqqqqpp -----END RSA PRIVATE KEY-----

it also worked on decode.fr

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753648569804/17898feb-408e-4335-a8d4-dc1658843674.png align="center")

Entering the d on the ransomware note as the decryption key

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753648598752/f199e95e-673f-409d-960e-e2cee8fefd1f.png align="center")

then checking the document that that has the results - flag revealed

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753648937626/f426edfc-09e6-4f4f-b96d-079c7940643e.png align="center")

By leveraging weaknesses in the ransomware’s key management, specifically the insecure RSA modulus that could be factored, we successfully recovered the private key and decrypted the ransomware-encrypted files. This exercise demonstrated how improper cryptographic implementation and leftover debugging information can compromise the entire encryption process. Ultimately, the decrypted document revealed the required flag, proving that careful analysis and crypto skills can effectively counter ransomware threats.