---
title: "Networking: Nmap: The Basics (TryHackMe)"
datePublished: Sat Oct 26 2024 21:52:21 GMT+0000 (Coordinated Universal Time)
cuid: cm2qp5hht000109lb7eq7aqu7
slug: networking-nmap-the-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729979177263/4b0db975-c9f6-411c-b547-e11671ccdf9a.png
tags: networking, nmap, tryhackme, write-up

---

In this article, I will write a write-up for Nmap: The Basics that covers Host Discovery: Who is Online, Port Scanning: Who is Listening, Version Detection: Extract More Information, Timing: How Fast is Fast, and Output: Controlling What You See.

1. What is the last IP address that will be scanned when your scan target is `192.168.0.1/27`? `192.168.0.31`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729979266644/a0c2122b-91ee-40b0-9fac-bd1fd0a3fc3a.png align="center")
    
2. How many TCP ports are open on the target system at `10.10.235.198`? 6
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729979306301/088df6de-c1d5-47f6-a3b4-204281628af6.png align="center")
    
3. Find the listening web server on `10.10.235.198` and access it with your browser. What is the flag that appears on its main page? `THM{SECRET_PAGE_38B9P6}`  
      
    to start there’s a hint on the question that you should access via `http://ip_address:port_number`. Remember the computer has 65535 ports so I tried to use common ports like 80, 8080, etc, and the browser didn’t open. I ran a command `nmap -sV -A 10.10.235.198` that gave us a comprehensive overview of our target machine IP which included open ports, 8008 showed `8008/tcp open http lighttpd/1.4.74`. On opening the browser `http://ip_address:8008` I got the flag
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729979351111/53258cbb-58a4-4d9e-9086-9ccfe406f716.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729979472074/ed45a22d-d46e-4258-99ba-14981f198cfa.png align="center")
    
4. What is the name and detected version of the web server running on `10.10.235.198`? `lighttpd 1.4.74`
    

running `nmap -A ip_address` brings it up notice that our attack machine uses `lighttpd` and not the web servers like `nginx` so that a hint too  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729979509317/32af4872-c07a-46c4-86fa-7820c4473088.png align="center")

5. What is the non-numeric equivalent of `-T4`? `-T aggressive`
    
6. What option must you add to your `nmap` command to enable debugging? `-d`
    
7. What kind of scan will Nmap use if you run `nmap MACHINE_IP` with local user privileges? `Connect Scan`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**