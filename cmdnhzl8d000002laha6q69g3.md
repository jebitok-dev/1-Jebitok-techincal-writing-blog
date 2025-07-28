---
title: "Challenges: Lo-Fi (TryHackMe)"
datePublished: Fri Jul 25 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmdnhzl8d000002laha6q69g3
slug: challenges-lo-fi-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753729451216/a13f7131-a000-46c5-b3e1-029a7ea1a89b.png
tags: web-security, tryhackme, lfi, ctf-writeup, file-inclusion

---

This challenge focuses on **Local File Inclusion (LFI)**, a vulnerability where an application includes files based on user input without proper validation. In practice, this allows attackers to read sensitive files or even achieve code execution. The goal here was to explore a web application, identify a vulnerable parameter, and use it to climb the filesystem and retrieve the flag.  

Want to hear some lo-fi beats, to relax or study to? We've got you covered!

**Access this challenge** by deploying both the vulnerable machine by pressing the green "Start Machine" button located within this task, and the TryHackMe AttackBox by pressing the  "Start AttackBox" button located at the top-right of the page.

Navigate to the following URL using the AttackBox: [http://MACHINE\_IP](http://machine_ip/) and find the flag in the **root of the filesystem.**

Check out similar content on TryHackMe:

* [LFI Path Traversal](https://tryhackme.com/r/room/filepathtraversal)
    
* [File Inclusion](https://tryhackme.com/room/fileinc)
    

**Note:** The web page does load some elements from external sources. However, they do not interfere with the completion of the room.

### Answer the questions below

Climb the filesystem to find the flag!

At first, I scanned with `nmap` used `gobuster` to enumerate the directories

`nmap -sV IP_Address`

`gobuster dir -u IP_Address -w /usr/share/wordlists/dirb/common.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753727181282/85c2ef4c-a2d5-4184-9562-e52f0cc6c47d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753727242598/004135fe-4c52-4542-851e-8c4eeb3ea1eb.png align="center")

Visited the site: `http://IP_Address` and it has a `?page=` parameter, which was hinted at in the file inclusion  

Next, I tested this path: `/?page=../../../../etc/passwd` And this confirmed LFI.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753727211833/70b83ec7-2fbb-4cf9-8aa7-d1feef456b51.png align="center")

At first, I tried to check the path at `http://IP_Address/?page=../../../../root/flag.txt` But there was a hint in the description:

> “Find the flag **in the root of the filesystem**.”

  
`http://IP_Address/?page=../../../../flag.txt` revealed the flag  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753727305736/5cc84769-a5da-4348-866f-dc2661203a7e.png align="center")

This room teaches that when exploiting **LFI**, understanding the directory structure and reading task hints is key. Instead of blindly guessing (`/root/flag.txt`), reading and testing paths systematically saves time. In real-world scenarios, LFI can escalate to Remote Code Execution (RCE) or credential theft if write access or log poisoning is possible, making it a high-severity vulnerability.