---
title: "The Advent of Cyber: Day 8: Shellcodes - Shellcodes of the world, unite! (TryHackMe)"
datePublished: Sat Jan 11 2025 13:28:23 GMT+0000 (Coordinated Universal Time)
cuid: cm5s81z0s000c08mh92drhu4t
slug: the-advent-of-cyber-day-8-shellcodes-shellcodes-of-the-world-unite-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736601041462/de6ccfa9-286a-40cc-9179-ad34062d5c96.png
tags: tryhackme, metasploit, write-up, shellcode, adventofcyber2024

---

In this article, we’ll cover the Shellcodes - Shellcodes of the world, unite! write-up as the Day 8 challenge of the Advent of Cyber event challenge. It was interesting to grasp the fundamentals of writing shellcodes for reverse shells while executing with Powershell. We’re still at Wareville for SOC-mas!

Let's dive into the story and troubleshoot the issue in this part of the task. Glitch has realized he's no longer receiving incoming connections from his home base. Mayor Malware's minion team seems to have tampered with the shellcode and updated both the IP and port, preventing Glitch from connecting. The correct IP address for Glitch is `ATTACKBOX_IP`, and the successful connection port should be `4444`.

Can you help Glitch identify and update the shellcode with the correct IP and port to restore the connection and reclaim control?

Answer the questions below

1. What is the flag value once Glitch gets reverse shell on the digital vault using port 4444? Note: The flag may take around a minute to appear in the **C:\\Users\\glitch\\Desktop** directory. You can view the content of the flag by using the command **type C:\\Users\\glitch\\Desktop\\flag.txt**.
    
    `AOC{GOT MYACCESS_B@CK007}`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736601632746/8995b07b-58d9-45e8-9d26-00f72e9ef377.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736601708197/9318bdfb-2c66-48c7-8e99-b227262fda69.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736601786217/b1fa5382-a106-4917-8d19-145d3913d6cf.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736601823723/52885542-fa3d-4052-a5ed-206ac60a59d5.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736601855188/2d3c2735-c28f-4e02-b8ac-2b4e3ac88112.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736601880459/b7d69b9b-f9c8-4b5c-af0d-b427524c24c5.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736601969130/e4686ab0-d244-4d1f-b98a-1fae5f7bbc9e.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736602018977/eb743a83-b854-4563-b3df-fb98d1992c08.png align="center")
    
2. Are you interested in learning more about evasion? Take a look at the [AV Evasion: Shellcode](https://tryhackme.com/r/room/avevasionshellcode) room. 
    

Thank you for reading this article. Please leave a comment with your thoughts, areas for improvement, other suggestions, and questions. Stay secure until the next one!