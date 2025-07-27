---
title: "Challenges: Order (TryHackMe)"
datePublished: Sun Jul 27 2025 13:22:08 GMT+0000 (Coordinated Universal Time)
cuid: cmdlpkqm6000i02l3g5xr9c8h
slug: challenges-order-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753622492598/1d96dd8c-3372-4a26-9628-427bfc678ee9.png
tags: python, xor, tryhackme, ctf-writeup

---

In this challenge, we intercepted one of Cipher’s encrypted messages which supposedly contained details of their next target. The message was protected using a repeating-key XOR cipher. However, a critical mistake was made—every message from Cipher begins with a known header, making the encryption vulnerable to cryptanalysis. Our goal was to exploit this weakness, recover the encryption key, and decrypt the intercepted message to reveal Cipher’s next target.

We intercepted one of Cipher's messages containing their next target. They encrypted their message using a repeating-key XOR cipher. However, they made a critical error—every message always starts with the header:

Can you help void decrypt the message and determine their next target?  
Here is the message we intercepted:

`1c1c01041963730f31352a3a386e24356b3d32392b6f6b0d323c22243f6373`

`1a0d0c302d3b2b1a292a3a38282c2f222d2a112d282c31202d2d2e24352e60`

used a python script to look for the flag

```python
cipher_hex = "1c1c01041963730f31352a3a386e24356b3d32392b6f6b0d323c22243f6373" \
             "1a0d0c302d3b2b1a292a3a38282c2f222d2a112d282c31202d2d2e24352e60"

key = b"SNEAKY"
cipher = bytes.fromhex(cipher_hex)
plain = bytes([c ^ key[i % len(key)] for i, c in enumerate(cipher)])
print(plain.decode())
```

`python3 script.py`

By leveraging the known header and the repeating-key XOR weakness, we successfully determined the encryption key and decrypted the message. Using a simple Python script, the plaintext was revealed, exposing Cipher’s next target. This demonstrates how small cryptographic implementation errors can compromise sensitive data, even when strong encryption algorithms are used incorrectly.