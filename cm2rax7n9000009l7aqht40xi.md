---
title: "Cryptography: Cryptography Basics (TryHackMe)"
datePublished: Sun Oct 27 2024 08:01:47 GMT+0000 (Coordinated Universal Time)
cuid: cm2rax7n9000009l7aqht40xi
slug: cryptography-cryptography-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730016039997/bcd97555-4ffa-4ea7-b9c6-81a43670fa81.png
tags: encryption, cryptography, mathematics, tryhackme, caesar-cipher, write-up

---

In this article, I will write a write-up for Cryptography Basics that covers the Importance of Cryptography, Plaintext to Ciphertext, Historical Ciphers, Types of Encryption and Basic Math in cryptography and symmetric encryption.

1. What is the standard required for handling credit card information? `PCI DSS`
    
2. What do you call the encrypted plaintext? `ciphertext`
    
3. What do you call the process that returns the plaintext? `decryption`
    
4. Knowing that `XRPCTCRGNEI` was encrypted using Caesar Cipher, what is the original plaintext? `ICANENCRYPT`
    

At first, I thought we had to decode into the PLAINTEXT manually like shifting the letters several times backwards using the alphabet A-Z with the number 26 concept. Thinking we’ll have to change from ZYXABCD… to ABCD…Z or the VWXYZABC… with no success

Eventually, I realized that the question has a hint that one can “Can use an online tool such as the ones available at [https://cryptii.com/pipes/caesar-cipher](https://cryptii.com/pipes/caesar-cipher) and [https://www.dcode.fr/caesar-cipher”](https://www.dcode.fr/caesar-cipher%E2%80%9D).

Having understood the concept of shifting the positions of the specific letters in the word we’re trying to find its Plaintext a few positions backward till you get an English word. Using [Cryptii.com](http://cryptii.com/) by default it shifts by 7 steps backward but if you move 11 steps backward you arrive to the word `ICANENCRYPT`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730015926614/341a3f23-bd46-47b2-94d8-b315bbd8c86d.png align="center")

5. Should you trust DES? (Yea/Nay) `Nay`
    
6. When was AES adopted as an encryption standard? `2001`
    
7. What’s 1001 ⊕ 1010? `0011`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730015950078/f7d5445e-b0bb-46ce-b83e-ac47d5870040.png align="center")
    
8. What’s 118613842%9091? `356`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730015977536/dc6308ec-4280-4d9d-93fe-2ed47464d254.png align="center")
    
9. What’s 60%12? `0`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**