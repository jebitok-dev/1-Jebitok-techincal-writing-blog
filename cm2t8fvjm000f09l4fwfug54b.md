---
title: "Cryptography: John the Ripper: The Basics (TryHackMe)"
datePublished: Mon Oct 28 2024 16:27:51 GMT+0000 (Coordinated Universal Time)
cuid: cm2t8fvjm000f09l4fwfug54b
slug: cryptography-john-the-ripper-the-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730132763165/58383911-45fa-4c16-97de-1d3489bcb1cc.png
tags: passwords, cracking, cryptography, tryhackme, write-up, john-the-ripper, rainbow-table

---

In this article, I will write a write-up for John the Ripper: The Basics that the Basic Terms, Setting Up John on Your System, Cracking Basic Hashes, Cracking Windows Authentication Hashes, Cracking /etc/shadow Hashes, Single Crack Mode, Custom Rules, Cracking Password Protected Zip Files, Cracking Password Protected Zip Files, Cracking Password-Protected RAR Archives, and Cracking SSH Keys with John.

1. What is the most popular extended version of John the Ripper? `Jumbo John`
    
2. Which website’s breach was the `rockyou.txt` wordlist created from? [`rockyou.com`](http://rockyou.com/)
    

the image below shows how to achieve the next questions form 3 to 10.  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730131985891/d15189fb-4d5c-432a-89d6-d6ba02bbdb64.png align="center")

3. What type of hash is hash1.txt? `md5`
    
4. What is the cracked value of hash1.txt? `biscuit`
    
5. What type of hash is hash2.txt? `sha1`
    
6. What is the cracked value of hash2.txt? `kangeroo`
    
7. What type of hash is hash3.txt? `sha256`
    
8. What is the cracked value of hash3.txt? `microphone`
    
9. What type of hash is hash4.txt? `whirlpool`
    

to identify the hash I used the rainbow table, [Crackstation](https://crackstation.net/) then continued to use Johnriper to crack the value using the command `john —-format=whirlpool —-wordlist=/usr/share/wordlists/rockyou.txt hash4.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730132032129/178a6a23-c88b-4d18-b1ff-324b655d78fb.png align="center")

10. What is the cracked value of hash4.txt? `colossal`
    
11. What do we need to set the `--format` flag to in order to crack this hash? `nt`
    
12. What is the cracked value of this password? `mushroom`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730132077158/fd87e26a-77ca-45ae-9a2b-809c35e7bd20.png align="center")
    
13. What is the root password? `1234`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730132101171/c912d67b-aab7-4c68-8684-1c2367a9ce44.png align="center")
    

**Example Usage:**

14. What is Joker’s password? `Jok3r`  
      
    after finding the hash remember to edit the file with nano then add the name before the hash you’ve found `Joker:{hash`}
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730132226025/c6c78bff-8fcc-4963-955a-99a2ec0376f1.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730132316302/bee29d5c-6e4c-4e2f-9c42-1b45160a056c.png align="center")
    
15. What do custom rules allow us to exploit? `Password complexity predictability`
    
16. What rule would we use to add all capital letters to the end of the word? `Az"[A-Z]”`
    
17. What flag would we use to call a custom rule called `THMRules`? `--rule=THMRules`
    
18. What is the password for the secure.zip file? `pass123`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730132344332/269b5694-1682-41a1-bb9d-5dfd8a21412d.png align="center")
    
19. What is the contents of the flag inside the zip file? `THM{w3ll_d0n3_h4sh_r0y4l}`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730132378032/50c0028f-d2ef-4b02-807e-bbeadd67af54.png align="center")
    
20. What is the password for the secure.rar file? `password`
    
21. What are the contents of the flag inside the zip file? `THM{r4r_4rch1ve5_th15_t1m3}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730132516007/300d1aee-a015-4f4f-87f3-fd1f231c41b9.png align="center")
    

###   
`/opt/john/ssh2john.py id_rsa > id_rsa_hash.txt`  
  
**Cracking**

For the final time, we’re feeding the file we output from ssh2john, which in our example use case is called `id_rsa_hash.txt` and, as we did with `rar2john`, we can use this seamlessly with John: \*\*\`john --wordlist=/usr/share/wordlists/rockyou.txt id\_rsa\_hash.txt\`\*\*

### Practical

Now, I’d like you to crack the hash of the \*\*\`id\_rsa\`\*\* file relevant to this task! The file is located in \*\*\`~/John-the-Ripper-The-Basics/Task11/\`\*\*.

22. What is the SSH private key password? `mango`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730132532417/c36f6ebb-91c6-4cfe-a09a-89d59d302079.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**