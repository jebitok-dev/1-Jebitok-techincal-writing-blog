---
title: "Offensive Security Tooling: Gobuster: The Basics (TryHackMe)"
datePublished: Sun Nov 03 2024 13:24:22 GMT+0000 (Coordinated Universal Time)
cuid: cm31mj0zu000109lb8ope5gxl
slug: offensive-security-tooling-gobuster-the-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730638950456/db355c38-2f70-4a81-8b77-76a8bc26d5cf.png
tags: tryhackme, write-up, gobuster, offensive-security

---

In this article, I will write a Gobuster: The Basics that covers Environment and Setup, Gobuster: Introduction, Use Case: Directory and File Enumeration, Use Case: Subdomain Enumeration, and Use Case: Vhost Enumeration.

1. What flag to we use to specify the target URL? `-u`
    
2. What **command** do we use for the subdomain enumeration mode? `dns`
    
3. Which flag do we have to add to our command to skip the TLS verification? Enter the long flag notation. `--no-tls-validation`
    
4. Enumerate the directories of www.offensivetools.thm. Which directory catches your attention? `secret`
    
5. Continue enumerating the directory found in question 2. You will find an interesting file there with a .js extension. What is the flag found in this file? `THM{ReconWasASuccess}`
    
6. Apart from the dns keyword and the -w flag, which **shorthand flag** is required for the command to work? `-d`
    
7. Use the commands learned in this task, how many subdomains are configured for the offensivetools.thm domain? `4`
    
8. Use the commands learned in this task to answer the following question: How many vhosts on the offensivetools.thm domain reply with a status code 200? `4`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**