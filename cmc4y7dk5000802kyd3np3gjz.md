---
title: "Challenges: Investigating Windows (TryHackMe)"
datePublished: Fri Jun 20 2025 15:11:53 GMT+0000 (Coordinated Universal Time)
cuid: cmc4y7dk5000802kyd3np3gjz
slug: challenges-investigating-windows-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750428064494/7290e69f-9e32-4f30-922e-dad1f08b7e8e.png
tags: windows, tryhackme, ctf-writeup

---

In this write-up, I’ll be walking through the [**Investigating Windows**](https://tryhackme.com/room/investigatingwindows) room on TryHackMe — a digital forensics-style challenge that simulates analyzing a compromised Windows Server environment. The scenario revolves around connecting to a Windows machine via RDP and digging through logs, scheduled tasks, user sessions, and file activity to trace the attacker's steps.

Through this exercise, we explore core blue team concepts such as logon tracking, privilege analysis, and identifying persistence mechanisms — all skills relevant for both defenders and curious learners diving into Windows forensics.

This is a challenge that is exactly what is says on the tin, there are a few challenges around investigating a windows machine that has been previously compromised.

Connect to the machine using RDP. The credentials the machine are as follows:

Username: Administrator  
Password: letmein123!

Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up.

## Answer the questions below

1. Whats the version and year of the windows machine? `Windows Server 2016`
    
    `systeminfo`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750428420397/b158c2cd-f329-4290-8eba-e8f7b0e5dae0.png align="center")
    
2. Which user logged in last? `Administrator`
    
    `quser`
    
    `net user Administrator`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750428716216/2e5c2878-ff2a-4a6c-8e22-0bfb1878207f.png align="center")
    
3. When did John log onto the system last? `03/02/2019 5:48:32 PM`
    
    Answer format: MM/DD/YYYY H:MM:SS AM/PM
    
    `net user John`
    
4. What IP does the system connect to when it first starts? `10.34.2.3`
    
5. What two accounts had administrative privileges (other than the Administrator user)? `Guest, Jenny`
    
    Answer format: List them in alphabetical order.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750429887807/e64d02ec-3ca6-4e7f-b94e-c7b79d958130.png align="center")
    
6. Whats the name of the scheduled task that is malicious. `Clean file system`
    
    `Get-ScheduledTask | Get-ScheduledTaskinfo`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750429911254/c5049a36-63e7-4339-b35c-0f61b770aa4f.png align="center")
    
7. What file was the task trying to run daily? `nc.ps1`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431803635/2623222a-0fc7-4994-8e97-deae7d03e80b.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431830779/c64fd0a6-4f85-4be0-925a-34ae4c2915e3.png align="center")
    
8. What port did this file listen locally for? `1348`
    
9. When did Jenny last logon? `Never`  
      
    `net user Jenny`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431677502/c9d6628d-534d-4658-a520-a72332f1af25.png align="center")
    
10. At what date did the compromise take place? `03/02/2019`
    
    Answer format: MM/DD/YYYY
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431602168/7e7b7ced-796d-4c25-acda-875f3958876e.png align="center")
    
11. During the compromise, at what time did Windows first assign special privileges to a new logon?
    
    Answer format: MM/DD/YYYY HH:MM:SS AM/PM `03/02/2019 4:04:49 PM`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431187507/cd5c6de0-6d60-4842-a838-73491b3d94e6.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431392344/f9d77e3f-d0cc-4ddb-80c1-48b78486593e.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431444251/3e70dd1c-039e-477f-8ea9-dba0ea1237fb.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431488121/baa97286-59d3-41f7-9e33-69a38803a9c2.png align="center")
    
12. What tool was used to get Windows passwords? `Mimikatz`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431139782/a3fc43cd-27c2-47a6-9a98-5e647c2adbdd.png align="center")
    
13. What was the attackers external control and command servers IP? `76.32.97.132`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431008479/037e39c9-bc56-4fc1-99e0-26aacec36662.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431062214/73a51be2-8e32-4d43-9728-91b1e962965f.png align="center")
    
14. What was the extension name of the shell uploaded via the servers website? `.jsp`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750430969304/774e587e-0362-4b8c-9d31-5739ba0a715b.png align="center")
    
15. What was the last port the attacker opened? `1337`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750430942712/0e93585a-a2a3-4ab2-ad97-67fd86e9c00c.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750430900634/be01e046-a903-4aab-bc03-346c3d1d7158.png align="center")
    
16. Check for DNS poisoning, what site was targeted? `google.com`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750431088304/60ab0267-45d2-4151-ada6-a37a94beb485.png align="center")
    

The *Investigating Windows* room offers a practical glimpse into the investigative process after a system compromise. From analyzing logon events and scheduled tasks to identifying attacker activity and tools like Mimikatz, this challenge emphasizes the importance of attention to detail and critical thinking.

It serves as a strong reminder that visibility into endpoint activity is crucial for effective threat detection and response. Thanks for reading, and as always, keep exploring, stay safe, and never stop learning! 🧠🔍💻