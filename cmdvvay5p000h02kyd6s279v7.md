---
title: "Cipher's Secret Message"
datePublished: Sun Aug 03 2025 16:00:10 GMT+0000 (Coordinated Universal Time)
cuid: cmdvvay5p000h02kyd6s279v7
slug: ciphers-secret-message
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754236579722/f06dbc73-5974-4874-887c-4df9810646fa.png
tags: python, decrypt, tryhackme, ctf-writeup

---

In this challenge, we were provided with an encrypted message and the encryption algorithm used to secure it. Our task was to reverse-engineer the given algorithm, decrypt the message, and retrieve the hidden flag. The encryption applied an index-based shift to each character, modifying alphabetical characters differently based on their position, while leaving non-alphabetic characters unchanged. By analyzing the algorithm and writing a decryption script, we could successfully reveal the original plaintext.

One of the Ciphers' secret messages was recovered from an old system alongside the encryption algorithm, but we are unable to decode it.

**Order:** Can you help void to decode the message?

**Message** : a\_up4qr\_kaiaf0\_bujktaz\_qm\_su4ux\_cpbq\_ETZ\_rhrudm

**Encryption algorithm** :

```python
from secret import FLAG

def enc(plaintext):
    return "".join(
        chr((ord(c) - (base := ord('A') if c.isupper() else ord('a')) + i) % 26 + base) 
        if c.isalpha() else c
        for i, c in enumerate(plaintext)
    )

with open("message.txt", "w") as f:
    f.write(enc(FLAG))

```

Note: Wrap the decoded message within the flag format **THM{}**

create a `message.txt` file with this  
  
`touch message.txt`

`nano message.txt`

```python
a_up4qr_kaiaf0_bujktaz_qm_su4ux_cpbq_ETZ_rhrudm
```

improved decrypt file: `decrypt.py`

```python
def dec(ciphertext):
    return "".join(
        chr((ord(c) - (base := ord('A') if c.isupper() else ord('a')) - i) % 26 + base) 
        if c.isalpha() else c
        for i, c in enumerate(ciphertext)
    )

# Read the encrypted message
with open("message.txt", "r") as f:
    encrypted_message = f.read().strip()

# Decrypt the message
decrypted_message = dec(encrypted_message)
print("Decrypted message:", decrypted_message) 
```

`python3 decrypt.py`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754236433579/a88334f7-3ef0-4fcc-a997-320fb035b679.png align="center")

By carefully examining the encryption method and implementing its inverse logic, we successfully decrypted the provided message. The decrypted output revealed a meaningful string that, when wrapped in the required `THM{}` format, provided the correct flag for submission. This challenge demonstrated how analyzing even a simple index-based shift cipher can reveal hidden information when the original encryption logic is available.