---
title: "Cyber Defense Frameworks: Cyber Kill Chain (TryHackMe)"
datePublished: Sun Nov 10 2024 14:04:16 GMT+0000 (Coordinated Universal Time)
cuid: cm3bo1b0z00020al2cyk3bbzz
slug: cyber-defense-frameworks-cyber-kill-chain-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731247397484/52da8309-7997-4436-9459-8e9dc9584b78.png
tags: tryhackme, write-up, cyber-kill-chain, cyber-defense

---

In this article, I will write a Cyber Kill Chain write-up: The Basics that covers Reconnaissance, Weaponization, Delivery, Exploitation, Installation, Command & Control (C&C), Action on Objectives (Exfiltration), and Practice Analysis.

1. What is the name of the Intel Gathering Tool that is a web-based interface to the common tools and resources for open-source intelligence? `OSINT Framework`
    
2. What is the definition for the email gathering process during the stage of reconnaissance? `email harvesting`
    
3. This term is referred to as a group of commands that perform a specific task. You can think of them as subroutines or functions that contain the code that most users use to automate routine tasks. But malicious actors tend to use them for malicious purposes and include them in Microsoft Office documents. Can you provide the term for it? `Macro`
    
4. What is the name of the attack when it is performed against a specific group of people, and the attacker seeks to infect the website that the mentioned group of people is constantly visiting? `Watering hole attack`
    
5. Can you provide the name for a cyberattack targeting a software vulnerability that is unknown to the antivirus or software vendors? `Zero-day`
    

A*ccording to* [*FireEye*](https://www.fireeye.com/current-threats/what-is-a-zero-day-exploit.html)*, "the zero-day exploit or a zero-day vulnerability is an unknown exploit in the wild that exposes a vulnerability in software or hardware and can create complicated problems well before anyone realizes something is wrong. A zero-day exploit leaves NO opportunity for detection at the beginning."*

```plaintext
These are examples of how an attacker carries out exploitation:

- The victim triggers the exploit by opening the email attachment or clicking on a malicious link.
- Using a zero-day exploit.
- Exploit software, hardware, or even human vulnerabilities.
- An attacker triggers the exploit for server-based vulnerabilities.
```

6. Can you provide the technique used to modify file time attributes to hide new or changes to existing files? `Timestomping`
    
7. Can you name the malicious script planted by an attacker on the webserver to maintain access to the compromised system and enables the webserver to be accessed remotely? `Web shell`
    
8. What is the C2 communication where the victim makes regular DNS requests to a DNS server and domain which belong to an attacker. `DNS Tunneling`
    
9. Can you provide a technology included in Microsoft Windows that can create backup copies or snapshots of files or volumes on the computer, even when they are in use? `Shadow Copy`  
      
    We really hope you enjoyed this room. In order to strengthen your knowledge, let's do a practice analysis.
    
    ![https://tryhackme-images.s3.amazonaws.com/user-uploads/5c549500924ec576f953d9fc/room-content/0d0675a1087fd791f04ca4a359350576.png](https://tryhackme-images.s3.amazonaws.com/user-uploads/5c549500924ec576f953d9fc/room-content/0d0675a1087fd791f04ca4a359350576.png)
    
    **Here is the real-world scenario for you to tackle:**
    
    *The infamous Target cyber-attack, which led to one of the largest data breaches in history took place on November 27, 2013.*
    
    On December 19th, 2013, Target released a [statement](https://corporate.target.com/news-features/article/2013/12/important-notice-unauthorized-access-to-payment-ca) confirming the breach, stating that approximately 40 million credit and debit card accounts were impacted between Nov. 27 and Dec. 15, 2013. Target had to pay the fine of $18.5 million under the terms of the multistate [settlement agreement](https://www.attorneygeneral.gov/taking-action/settlement-reached-with-target-following-major-consumer-data-breach/). This is considered to be the largest data-breach settlement in history.
    
    How did the data breach happen? **Deploy the static site** attached to this task and apply your skills to **build the Cyber Kill Chain of this scenario**. Here are some tips to help you complete the practical:
    
    **1.** Add each item on the list in the correct Kill Chain entry-form on the Static Site Lab:
    
    * exploit public-facing application
        
    * data from local system
        
    * powershell
        
    * dynamic linker hijacking
        
    * spearphishing attachment
        
    * fallback channels
        
    
    **2.** Use the ‘*Check answers*’ button to verify whether the answers are correct (where wrong answers will be underlined in red).
    
    Answer the questions below
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731247311612/6a092019-eeaa-4d3b-842b-f87fb6b47db3.png align="center")
    
10. What is the flag after you complete the static site? `THM{7HR347_1N73L_12_4w35om3}`
    
    * **Weaponization**: Powershell
        
    * **Delivery**: spearphishing attachment
        
    * **Exploitation**: exploit public-facing application
        
    * **Installation**: dynamic linker hijacking
        
    * **Command & Control**: fallback channels
        
    * **Actions on Objectives**: data from local system
        
    
    (make sure there are no spaces at the end of words)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731247285387/3515a660-61e8-4b9e-99a9-9f029a08088f.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**