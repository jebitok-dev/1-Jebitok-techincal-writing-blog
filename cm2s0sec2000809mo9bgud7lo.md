---
title: "Cryptography: Hashing Basics (TryHackMe)"
datePublished: Sun Oct 27 2024 20:05:52 GMT+0000 (Coordinated Universal Time)
cuid: cm2s0sec2000809mo9bgud7lo
slug: cryptography-hashing-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730059088960/d11fb23f-7a6d-4b0a-a9c3-fded29ee86eb.png
tags: passwords, hashing, rainbow-table-attack

---

In this article, I will write a write-up for Hashing Basics that covers Hash Functions, Insecure Password Storage for Authentication, Using Hashing for Secure Password Storage, Recognising Password Hashes, Password Cracking, and Hashing for Integrity Checking.

1. What is the SHA256 hash of the `passport.jpg` file in `~/Hashing-Basics/Task-2`? `77148c6f605a8df855f2b764bcc3be749d7db814f5f79134d2aa539a64b61f02`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730059134145/65f90b75-088a-46bd-a922-3cb64d3162fd.png align="center")
    
2. What is the output size in bytes of the MD5 hash function? `16`
    
3. If you have an 8-bit hash output, how many possible hash values are there? `256`
    
4. What is the 20th password in `rockyou.txt`? `qwerty`
    

there’s a hint to this command `head -n 20 rockyou.txt` since using without the number only shows the first 10 passwords

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730058864829/aec20f90-adce-40e9-bd6e-35ae74d1b8ee.png align="center")

5. Manually check the hash “4c5923b6a6fac7b7355f53bfe2b8f8c1” using the rainbow table above. `inS3CyourP4$$`
    
6. Crack the hash “5b31f93c09ad1d065c0491b764d04933” using an online tool. `tryhackme`
    
7. Should you encrypt passwords in password-verification systems? Yea/Nay `Nay`
    
8. What is the hash size in yescrypt? `256`
    
9. What’s the Hash-Mode listed for Cisco-ASA MD5? `2410`
    
10. What hashing algorithm is used in Cisco-IOS if it starts with `$9$`? `scrypt`
    
11. Use `hashcat` to crack the hash, `$2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01pMSrddr1ZG`, saved in `~/Hashing-Basics/Task-6/hash1.txt`. `85208520`
    
12. Use `hashcat` to crack the SHA2-256 hash, `9eb7ee7f551d2f0ac684981bd1f1e2fa4a37590199636753efe614d4db30e8e1`, saved in saved in `~/Hashing-Basics/Task-6/hash2.txt`. `halloween`
    
13. Use `hashcat` to crack the hash, `$6$GQXVvW4EuM$ehD6jWiMsfNorxy5SINsgdlxmAEl3.yif0/c3NqzGLa0P.S7KRDYjycw5bnYkF5ZtB8wQy8KnskuWQS3Yr1wQ0`, saved in `~/Hashing-Basics/Task-6/hash3.txt`. `spaceman`
    
14. Crack the hash, `b6b0d451bbf6fed658659a9e7e5598fe`, saved in `~/Hashing-Basics/Task-6/hash4.txt`. `funforyou`
    
15. What is SHA256 hash of `libgcrypt-1.11.0.tar.bz2` found in `~/Hashing-Basics/Task-7?09120c9867ce7f2081d6aaa1775386b98c2f2f246135761aae47d81f58685b9c`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730059421505/221ff4e6-de21-49f0-9eca-94b51e0511c6.png align="center")
    
16. What’s the hashcat mode number for `HMAC-SHA512 (key = $pass)`? `1750`  
      
    you can check the Hashcat [website](https://hashcat.net/wiki/doku.php?id=example_hashes) and [GitHub](https://github.com/unstable-deadlock/brashendeavours.gitbook.io/blob/master/pentesting-cheatsheets/hashcat-hash-modes.md) that shows the hash-modes and examples
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730059259866/04ca5db9-15d8-44de-8ddb-0da877075e7f.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730059231068/3fe4c3a2-6ed0-4c02-94ea-1b465fb457f0.png align="center")
    
17. Use `base64` to decode `RU5jb2RlREVjb2RlCg==`, saved as `decode-this.txt` in `~/Hashing-Basics/Task-8`. What is the original word?
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**