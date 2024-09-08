---
title: "Cybersecurity Awareness: Common Attacks (TryHackMe)"
datePublished: Sun Sep 08 2024 21:01:58 GMT+0000 (Coordinated Universal Time)
cuid: cm0u27sko001c09mb87r76eir
slug: cybersecurity-awareness-common-attacks-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725829256693/2306222c-d21f-4d30-b857-3697920b4c10.png
tags: attacks, cybersecurity-1

---

1. What was the original target of Stuxnet? `The Iran Nuclear Programme`
    
2. The static site will display a series of emails and text messages. You will be asked to identify which of these messages are genuine and which are phishing attempts. Once you have successfully identified all of the messages you will be presented with a flag to enter, here. Good luck! What is the flag `THM{I_CAUGHT_ALL_THE_PHISH}`
    
3. Based on the content of the website, you have generated a list of likely passwords, which are as follows: **TryHackMe123**!  
    `TryH@ckMe TryHackMe123 THM123456 qwertyuiop123 TryHackMe2021 TryHackMe123! TryHackMe345 TryHackM3! // copy on the Password list of the view site and click go and the password will be found`
    
4. Where you have the option, which should you use as a second authentication factor between SMS-based TOTPs or Authenticator App-based TOTPs (SMS or App)? `App`
    
5. What is the minimum number of up-to-date backups you should make? `3`
    
6. Of these, how many (at minimum) should be stored in another location? `1`
    
7. What number base could you use as a shorthand for base 2 (binary)? `Base 16`
    

Hexadecimal is used extensively as a shorthand for binary. Because it is **base 16** with 16 being a power of 2, conversion from binary to hex is clean. 5. If a password hash starts with $6$, what format is it (Unix variant)? `sha512crypt` (more on [`GitHub about SHA-512`](https://github.com/frizb/Hashcat-Cheatsheet)) The format `$6$` specifically indicates **SHA-512 crypt** (SHA512Crypt), which is used for hashing passwords in Unix-like systems. This format combines the SHA-512 hashing algorithm with a salt to produce a secure password hash. 6. What is the CVE for the 2020 Cross-Site Scripting (XSS) vulnerability found in WPForms? `CVE-2020-10385`

Thank you for reading through my article. You can leave any questions or comments on how I can improve my learning journey and the THM challenges. We can also connect more on [LinkedIn](https://www.linkedin.com/in/sharon-jebitok) or [X](https://x.com/SharonJebitok)