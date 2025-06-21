---
title: "Challenges: Crack the hash (TryHackMe)"
datePublished: Sat Jun 21 2025 19:30:35 GMT+0000 (Coordinated Universal Time)
cuid: cmc6mvx02000l02jrclk587ps
slug: challenges-crack-the-hash-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750534169051/8f24fd86-0838-42cb-9f1d-879c33f4b630.png
tags: cryptography, tryhackme, ctf-writeup, cryptography-hash-mbedtls-python-sha256-tls, crackthehash

---

In this challenge, we explore the practical aspects of hash cracking by completing Levels 1 and 2 of the **Crack the Hash** room. These tasks involve identifying various hash types and using common tools like **Hashcat** and **John the Ripper** to uncover the original plaintext passwords. The hash algorithms range from MD5 and SHA1 to bcrypt and HMAC-SHA1, offering a great opportunity to practice recognizing hash formats and selecting appropriate cracking techniques. Throughout the room, the well-known `rockyou.txt` Wordlist is used as the main resource for dictionary attacks, together with the Rainbow table, [CrackStation](https://crackstation.net/).

## Level 1

Can you complete the level 1 tasks by cracking the hashes?

### Answer the questions below

1. 48bb6e862e54f2a795ffc4e541caed4d `easy`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750532989403/ecc009a8-6df1-4507-9fe9-7acefa709e9a.png align="center")
    
      
    
2. CBFDAC6008F9CAB4083784CBD1874F76618D2A97 `password123`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750533019404/606390a5-c537-400c-8d8b-46d5d1151cdc.png align="center")
    
3. 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032 `letmein`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750533050184/c7d6b8e3-fbda-422f-9cc0-e76995b51b8b.png align="center")
    
4. $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom `bleh`  
      
    `hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`  
    
    `grep -E '^[a-z]{4}$' /usr/share/wordlists/rockyou.txt > four_letter_words.txt`  
    
    `hashcat -m 3200 -a 0 hash.txt four_letter_words.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750533336947/56d10426-f4c9-441f-93d0-db19fc737028.png align="center")
    
5. 279412f945939ba78ce0758d3fd83daa `Eternity22`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750533090333/b64dc4f1-6957-480f-b3e2-7c301b4cd71a.png align="center")
    

## Level 2

This task increases the difficulty. All of the answers will be in the classic [rock you](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt) password list.

You might have to start using hashcat here and not online tools. It might also be handy to look at some example hashes on [hashcats page](https://hashcat.net/wiki/doku.php?id=example_hashes).

### Answer the questions below

1. Hash: F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85 `paule`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750533216096/f8c7e2ff-cca6-4dfa-ac2f-04b94a5931a6.png align="center")
    
2. Hash: 1DFECA0C002AE40B8619ECF94819CC1B `n63umy8lkf4i`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750533244854/dc26bfa4-020d-4e6e-90f7-02c330c2b645.png align="center")
    
3. Hash: $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.
    
      
    Salt: aReallyHardSalt  
      
    `waka99`  
      
    `grep -E '^[a-z]{6}$' /usr/share/wordlists/rockyou.txt > six_letter_words.txt hashcat -m 1800 -a 0 hash6.txt six_letter_words.txt`
    
    `john --format=sha512crypt --wordlist=six_letter_words.txt hash6.txt`  
    
4. Hash: e5d8870e5bdd26602cab8dbe07a942c8669e56d6
    
      
    Salt: tryhackme  
      
    `481616481616`  
    
    ```markdown
    // hash.txt
    
    e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme
    ```
    
    `hashcat -a 0 -m 160 hash.txt /usr/share/wordlists/rockyou.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750533739545/a2dfa2d9-90a3-40e1-8b54-c7ff1688c143.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750533784934/4fb7b081-8df6-4ad0-9538-75df9557098b.png align="center")

By completing both levels, we successfully cracked multiple hashes using a variety of methods and tools. This exercise reinforced key skills in hash identification, rule-based cracking, and wordlist filtering. Tools like **Hashcat**, **John the Ripper**, and Linux utilities, e.g, `grep`proved essential in tackling increasingly complex hash formats. Overall, this room provided hands-on experience with real-world hash cracking scenarios, deepening our understanding of how attackers might exploit weak password storage and how defenders can better protect systems.