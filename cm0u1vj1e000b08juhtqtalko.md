---
title: "Starting Out In CyberSec: (TryHackMe)"
datePublished: Sun Sep 08 2024 20:52:26 GMT+0000 (Coordinated Universal Time)
cuid: cm0u1vj1e000b08juhtqtalko
slug: starting-out-in-cybersec-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725828698548/43a2ad18-f918-4e85-9295-28091d4c8e2b.png
tags: beginner, cybersecurity-1

---

1. What is the name of the career role that is legally employed to find vulnerabilities in applications? `Penetration Tester`
    
2. What is the name of the role whose job is to identify attacks against an organization? `security analyst`
    
3. In the Burp Suite Program that ships with Kali Linux, what mode would you use to manually send a request (often repeating a captured request numerous times)? `Repeater`
    
4. What hash format are modern Windows login passwords stored in? `NTLM`
    
    Modern Windows login passwords are stored in the **NTLM (NT LAN Manager)** hash format. Specifically, Windows uses the **NT hash** (also known as the NTLM hash or simply NTLM) to store passwords.
    
    The NT hash is derived from the user's password using the MD4 hashing algorithm. Unlike older LM (LAN Manager) hashes, which were used in earlier versions of Windows and were much less secure, NTLM hashes do not truncate or convert the password to uppercase, making them more secure, though they are still vulnerable to certain types of attacks, such as brute force and rainbow table attacks.
    
5. What are automated tasks called in Linux? `Cron Jobs`
    
    In Linux, automated tasks are typically referred to as **cron jobs**. These tasks are scheduled and managed using a tool called **cron**.
    
    Cron allows users to schedule scripts or commands to run at specified intervals, such as daily, weekly, or monthly. The schedules for these tasks are defined in a file called a **crontab** (short for "cron table"). Each user can have their own crontab file, and there is also a system-wide crontab for tasks that require root privileges.
    
6. SCP is a tool used to copy files from one computer to another. *What switch would you use to copy an entire directory?* `-r` ([resource on Geeks4Geeks](https://www.geeksforgeeks.org/scp-command-in-linux-with-examples/))
    
7. fdisk is a command used to view and alter the partitioning scheme used on your hard drive. *What switch would you use to list the current partitions?* `-l` [resource](https://brainly.com/question/31837087)
    
8. nano is an easy-to-use text editor for Linux. There are arguably better editors (Vim, being the obvious choice); however, nano is a great one to start with. *What switch would you use to make a backup when opening a file with nano?* `-B` [resource](https://www.linode.com/docs/guides/use-nano-text-editor-commands/)
    
9. Netcat is a basic tool used to manually send and receive network requests. *What* ***command*** *would you use to start netcat in listen mode, using port 12345?* `nc -l -p 12345` [How to use netcat commands - resource](https://www.varonis.com/blog/netcat-commands#:~:text=Netcat%20Command%20Syntax&text=By%20default%2C%20the%20Netcat%20tool,turn%20on%20full%20debugging%20mode)
    

Thank you for reading through my article. You can leave any questions or comments on how I can improve my learning journey and the THM challenges. We can also connect more on [LinkedIn](https://www.linkedin.com/in/sharon-jebitok) or [X](https://x.com/SharonJebitok)