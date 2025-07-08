---
title: "Challenges: Brooklyn Nine Nine (TryHackMe)"
datePublished: Mon Jul 07 2025 20:28:19 GMT+0000 (Coordinated Universal Time)
cuid: cmctjzs84000u02legcunc7l4
slug: challenges-brooklyn-nine-nine-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751917578775/be0979f8-c86b-46c6-ae7d-bd8179b80ee2.png
tags: ftp, ssh, nmap, tryhackme, sudo, ctf-writeup, privilege-escalation

---

Welcome to *Brooklyn Nine Nine*, a beginner-friendly Linux CTF challenge hosted on TryHackMe. This machine focuses on basic enumeration, initial access via commonly exposed services, and privilege escalation using a well-known `sudo` misconfiguration. Whether you're new to CTFs or brushing up on your enumeration and privilege escalation skills, this box is a solid learning experience. Let’s dive into it with curiosity, a bit of patience, and your favorite terminal.

## Deploy and get hacking

Start Machine

This room is aimed for beginner level hackers but anyone can try to hack this box. There are two main intended ways to root the box. If you find more dm me in discord at Fsociety2006.

### Answer the questions below

The first step is running an nmap scan:

`nmap -sC -sV -A <IP_Address>`

`nmap -sC -sV <IP_Address>`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751917822368/d1c42fb8-e73c-4304-93b7-39dd9b7aac04.png align="center")

21/tcp ftp vsftpd 3.0.3

22/tcp open ssh OpenSSH 7.6p1

80/tcp open http Apache httpd 2.4.29

`find / -type f -name note_to_jake.txt 2> /dev/null`

1. User flag
    
    With FTP, we can check any helpful details about the users
    
    `ftp <IP_Address>`  
    Name: Anonymous
    
    Password: (just enter - no password needed)
    
    ftp&gt; `ls`
    
    ftp&gt; `get note_to_jack.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751918001509/8c404206-0e81-48b5-9e86-a3d725a62096.png align="center")
    
    Users: Jake & Amy, queue: Holt, company: nine nine
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751918067968/1fd7da1e-2662-4f0d-a1b4-20c9ac509f2b.png align="center")
    
    Inspecting the site showed a hint of steganography, I tried using tools like steghide, strings, exiftool, binwalk, and steghide, but it wasn’t helpful. I wasn’t able to extract anything that would help us get the credentials of a user.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751918759530/01f15b36-71e2-48bb-bc0f-cf331d12aaba.png align="center")
    
    Using Hydra and SSH helped find Jake’s password, which would be used in Remote SSH authentication.
    
    `hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh://<IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751918806780/d0ab9602-d2d2-4249-b064-101a4a4d48e4.png align="center")
    
    `ssh jake@<IP_Addres>`  
    `find / -type f -name user.txt 2> /dev/null`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751918963708/62006862-1dfe-48e2-8d39-888cd38e2ea5.png align="center")
    
2. Root flag
    
    After finding the user, we now have to look for a way to escalate privileges to the root. First, we check the `Sudo -l`. With these, we’ll use online resources like [GTFOBins](https://gtfobins.github.io/gtfobins/less/#sudo) to check the commands we can use to escalate privileges. In this case, we’ll search for less and sudo on the platform.
    
    ```bash
    (ALL) NOPASSWD: /usr/bin/less
    ```
    
    We’ll use this command: `sudo less /etc/profile` which will launch with vim, use the letter v and scroll to the end or where you’ve /etc/profile and type `!/bin/sh` then exit with `:wq` then you’ll confirm Root and find the root flag
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751919482219/98bc944d-8c9e-4d0e-89e0-7f1952c5f946.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751919334409/45ed87e6-6a5d-4c2c-b57f-4260db932588.png align="center")
    

This machine walks us through several foundational skills in penetration testing: port scanning, brute-forcing services, basic file inspection, and finally, privilege escalation using `less` and `sudo`. It reinforces how crucial thorough enumeration is, and how seemingly harmless misconfigurations like giving `sudo` access to simple commands can lead to full system compromise. The hint “A recursive call” cleverly alludes to the ability of `less` to invoke a shell through nested tools like `vi`. Overall, a great exercise in sharpening your hacker instincts!

From anonymous FTPs to GTFOBinning our way to root—Brooklyn Nine Nine was more than just a precinct this time. It was a playground for hackers in training like me. Stay curious, take notes, and keep hacking. Nine Nine! 🚓💻