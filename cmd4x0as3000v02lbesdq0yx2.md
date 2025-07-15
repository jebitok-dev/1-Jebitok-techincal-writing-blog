---
title: "Challenges: ToolsRus (TryHackMe)"
datePublished: Tue Jul 15 2025 19:18:06 GMT+0000 (Coordinated Universal Time)
cuid: cmd4x0as3000v02lbesdq0yx2
slug: challenges-toolsrus-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752607026258/7436bfee-9387-452b-a252-46e612258119.png
tags: nmap, metasploit, apache-tomcat, gobuster, ctf-writeup, webpentesting

---

Welcome to **ToysRus**, a beginner-friendly CTF room designed to introduce essential enumeration and exploitation tools commonly used in penetration testing. In this challenge, we leverage tools like **Nmap**, **Gobuster**, **Hydra**, **Nikto**, and **Metasploit** to uncover hidden directories, crack credentials, and ultimately gain shell access to the target system.

The walkthrough simulates real-world scenarios involving web directory discovery, brute-forcing HTTP Basic Auth credentials, and exploiting vulnerable services such as **Apache Tomcat**. By methodically combining enumeration and exploitation, we progress through various stages — each building on information gathered earlier.

## ToysRus

![](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/Toys_%22R%22_Us_logo.svg/1280px-Toys_%22R%22_Us_logo.svg.png align="left")

Your challenge is to use the tools listed below to enumerate a server, gathering information along the way that will eventually lead to you taking over the machine.

This room will introduce you to the following tools: 

* Dirbuster
    
* Hydra
    
* Nmap
    
* Nikto
    
* Metasploit
    

If you are stuck at any point, each tool has a respective room or module linked above.

## Answer the questions below

1. What directory can you find, that begins with a "g"?  
      
    Starting off with enumeration using nmap, gobuster and checking the site among others  
      
    `nmap -sV IP_Address`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606024128/9b9591fa-0629-4aaa-a90b-4d1386c4f380.png align="center")
    
    `gobuster dir -u http://IP_Address -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606082908/923c3d3e-1393-4b9e-8c90-9b1097fffb1d.png align="center")
    
    the directory is revealed:  
    
2. Whose name can you find from this directory?  
      
    check the site with the directory as the path:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606174591/52f2f538-693a-4355-ad02-73222e08b369.png align="center")
    
3. What directory has basic authentication?  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606249869/a393606e-6dad-4b23-97e5-718fa48b28ad.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606270321/a4b879b8-9273-41f4-b618-73a49b83a3b0.png align="center")
    
4. What is bob's password to the protected part of the website?  
      
    `hydra -l bob -P /usr/share/wordlists/rockyou.txt IP_Address http-get /protected`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606326661/116151cd-18b7-4e38-a85f-f7660281aabc.png align="center")
    
5. What other port that serves a webs service is open on the machine?  
      
    `nmap -sV -sC IP_Address`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606372028/5b01bd2e-47e2-4054-886c-7992b427091f.png align="center")
    
6. What is the name and version of the software running on the port from question 5?
    
7. Use Nikto with the credentials you have found and scan the /manager/html directory on the port found above.
    
      
    How many docume0  
      
    navigated to /manager/html the counted the ones with 0
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606453286/3df9c047-4a06-4624-803b-93d4bd80f749.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606494135/fcfaf01d-2de4-4295-b0ba-7391110118d6.png align="center")
    
8. What is the server version?
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606565821/188632a3-23b6-48d6-9c09-72ac3d918b68.png align="center")
    
9. What version of Apache-Coyote is this service using?  
      
    check the `nmap` results  
    
10. Use Metasploit to exploit the service and get a shell on the system.
    
      
    What user did you get a shell as?  
    
    ```bash
    msfconsole 
    
    search tomcat mgr upload 
    
    use 
    
    show options
    
    set HttpPassword
    
    set HttpUsername 
    
    set RHOSTS 
    
    set RPORT
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606717945/331571e6-8133-45c7-8050-ad66c55d8239.png align="center")
    
    this first time it didn’t work so I had to try again
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606763327/e995b1b9-64fd-4c58-9765-14103cb7fb01.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606788181/5514352c-2837-40d8-82ea-0a4cc0396c53.png align="center")
    
11. What flag is found in the root directory?
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606838570/dcd69fbe-959a-4c84-8e6a-3eecce4f2139.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752606878709/b903390a-82b4-47a6-8168-b7da31f7d81b.png align="center")
    
      
    The **ToysRus** CTF box was an excellent hands-on exercise in foundational offensive security techniques. Through a combination of enumeration (via **Nmap**, **Gobuster**, and **Nikto**) and exploitation (with **Hydra** and **Metasploit**), we successfully navigated from reconnaissance to post-exploitation.
    
    We uncovered sensitive directories, brute-forced credentials to protected areas of a web app, and exploited a misconfigured **Tomcat Manager** interface to gain shell access — ultimately retrieving the root flag. This challenge reinforces the value of thorough reconnaissance, patience, and chaining information to escalate privileges.
    
    Whether you’re just getting started in your ethical hacking journey or preparing for certifications, **ToysRus** is a strong reminder that even beginner tools, when used correctly, can uncover powerful attack paths.