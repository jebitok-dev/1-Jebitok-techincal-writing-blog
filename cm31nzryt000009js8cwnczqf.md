---
title: "Offensive Security Tooling: Shells Overview (TryHackMe)"
datePublished: Sun Nov 03 2024 14:05:23 GMT+0000 (Coordinated Universal Time)
cuid: cm31nzryt000009js8cwnczqf
slug: offensive-security-tooling-shells-overview-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730640179661/e356e9b5-80bb-4516-a165-55dcf02c4846.png
tags: shell, tryhackme, write-up, offensive-security

---

In this article, I will write a write-up for Shell Overview that covers Shell Overview, Reverse Shell, Bind Shell, Shell Listeners, Shell Payloads, Web Shell, and a Practical Task.

1. What is the command-line interface that allows users to interact with an operating system? `Shell`
    
2. What process involves using a compromised system as a launching pad to attack other machines in the network? `Pivoting`
    
3. What is a common activity attackers perform after obtaining shell access to escalate their privileges? `Privilege Escalation`
    
4. What type of shell allows an attacker to execute commands remotely after the target connects back? `Reverse Shell`
    
5. What tool is commonly used to set up a listener for a reverse shell? `Netcat`
    
6. What type of shell opens a specific port on the target for incoming connections from the attacker? `Bind Shell`
    
7. Listening below which port number requires root access or privileged permissions? `1024`
    
8. Which flexible networking tool allows you to create a socket connection between two data sources? `socat`
    
9. Which command-line utility provides readline-style editing and command history for programs that lack it, enhancing the interaction with a shell listener? `rlwrap`
    
10. What is the improved version of Netcat distributed with the Nmap project that offers additional features like SSL support for listening to encrypted shells? `ncat`
    
11. Which Python module is commonly used for managing shell commands and establishing reverse shell connections in security assessments? `subprocess`
    
12. What shell payload method in a common scripting language uses the `exec`, `shell_exec`, `system`, `passthru`, and `popen` functions to execute commands remotely through a TCP connection? `PHP`
    
13. Which scripting language can use a reverse shell by exporting environment variables and creating a socket connection? `Python`
    
14. What vulnerability type allows attackers to upload a malicious script by failing to restrict file types? `Unrestricted File Upload`
    
15. What is a malicious script uploaded to a vulnerable web application to gain unauthorized access? `Web Shell`
    
    Now that we have learned about the different types of reverse shells, let's test our knowledge with a practical exercise, and let's get the flag in the format THM{} from the vulnerable web server. Click on the `Start Machine` button to start the challenge. After that, it will be accessible on the following URLs:
    
    * MACHINE\_IP:8080 hosts the landing page
        
    * MACHINE\_IP:8081 hosts the web application that is vulnerable to command injection.
        
    * MACHINE\_IP:8082 hosts the web application that is vulnerable to an unrestricted file upload.
        
    
    You can access the above using the `AttackBox`, which will display on a split screen, or you can use your own access through the VPN.
    
    **Note:** Please allow 2 minutes for the VM to fully boot up.
    
    For the next two questions refer to this [YouTube Video](https://youtu.be/nSv589s4Fg0?si=6i_wqw1fCkkgfdV0) for more context or help navigating them.
    
16. Using a reverse or bind shell, exploit the command injection vulnerability to get a shell. What is the content of the flag saved in the / directory? `THM{0f28b3e1b00becf15d01a1151baf10fd713bc625}`
    
17. Using a web shell, exploit the unrestricted file upload vulnerability and get a shell. What is the content of the flag saved in the / directory? `THM{202bb14ed12120b31300cfbbbdd35998786b44e5}`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**