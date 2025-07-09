---
title: "Challenges: Anthem (TryHackMe)"
datePublished: Wed Jul 09 2025 20:50:34 GMT+0000 (Coordinated Universal Time)
cuid: cmcwfo3n5000002laa2hc3zgp
slug: challenges-anthem-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752091594048/3574dd4b-43e2-47a6-81ad-0a16c2aef491.png
tags: nmap, rdp, tryhackme, ctf-writeup, xfreerdp

---

This beginner-friendly challenge focuses on attention to detail and passive information gathering, guiding you to uncover the “keys to the castle.” You don’t need brute force — just your browser, Remote Desktop, and observation skills. After initial reconnaissance with tools like `nmap` and `gobuster`, you’ll explore a web server running Umbraco CMS, hunt for hidden clues in source code and public files, and gather credentials needed to access the target system.

## Website Analysis

This task involves you, paying attention to details and finding the 'keys to the castle'.

This room is designed for beginners, however, everyone is welcomed to try it out!

Enjoy the Anthem.

In this room, you don't need to brute force any login page. Just your preferred browser and Remote Desktop.

Please give the box up to 5 minutes to boot and configure.

### Answer the questions below

1. Let's run nmap and check what ports are open.  
      
    `nmap -sC -sV <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752092068420/d750ffa1-af45-4835-8d08-7de113582c04.png align="center")
    
2. What port is for the web server? `80`
    
3. What port is for remote desktop service? `3389`  
      
    Gobuster gives an hint of some paths we can check on the site:  
      
    `gobuster dir -u http:// -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752092145101/d0849ecc-06f2-4b87-82f6-743c6e0c632c.png align="center")
    
      
    
4. What is a possible password in one of the pages web crawlers check for? `UmbracoIsTheBest!`  
      
    Checking `https://<Target_IP>/robots.txt` reveals the password
    
    ```bash
    UmbracoIsTheBest!
    
    # Use for all search robots
    User-agent: *
    
    # Define the directories not to crawl
    Disallow: /bin/
    Disallow: /config/
    Disallow: /umbraco/
    Disallow: /umbraco_client/
    ```
    
5. What CMS is the website using? `Umbraco` - [Umbraco](https://umbraco.com/), a .NET CMS
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752092290416/cec71ed3-5ef2-43b2-a351-c5a29add8d61.png align="center")
    
6. What is the domain of the website? `anthem.com`  
      
    the site `http://<TARGET_IP>` reveals the domain  
    
7. What's the name of the Administrator `Solomon Grundy`  
    
    the “*we are hiring*” part of the site has a famous poem and the hint of the question is searching on browser.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752092468177/4bb1760a-701d-4789-af4a-06bdabacd5e2.png align="center")
    
8. Can we find find the email address of the administrator? `SG@anthem.com`  
      
    based on the hiring email format of JD@anthem.com the administrator’s email is likely to be an abbrevation of their name too.
    

## Spot the flags

Our beloved admin left some flags behind that we require to gather before we proceed to the next task..

### Answer the questions below

Anthem are hiring!

1. What is flag 1? `THM{L0L_WH0_US3S_M3T4}`  
      
    this flag was on the comments within the source code found when you inspect the page
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752092692600/62107194-a3a0-4c46-aad2-209d791f6373.png align="center")
    
2. What is flag 2? `THM{G!T_G00D}`  
      
    the flag was also within the source code as the placeholder of. the search form
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752092780674/7c907a4c-b95a-489f-bf83-ee2eff680225.png align="center")
    
3. What is flag 3? `THM{L0L_WH0_D15}`  
      
    I came across this flag while scrolling through some of the open paths (`http://<TARGET_IP>/authors`) on the site  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752092576277/65357ca1-9e2b-445c-ba34-f65855bd9022.png align="center")
    
4. What is flag 4? `THM{AN0TH3R_M3TA}`  
      
    the last flag was also within the header section of the source code on the inspect element
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752092848985/579fe2fe-3692-4b79-b2ba-2415c17c8738.png align="center")
    

## Final stage

Let's get into the box using the intel we gathered.

### Answer the questions below

1. Let's figure out the username and password to log in to the box.(The box is not on a domain)
    
2. Gain initial access to the machine, what is the contents of user.txt? `THM{N00T_NO0T}`  
      
    For this we only have two ports open, 80 - http and 3389 - ms-wbt-server, the get low user access we also need to consider that we don’t have ssh or ftp as options. In this case i tried to explore the option of using the Umbraco CVE to use metasploit to get the user access but didn’t work had to eventually research further and found an option of using Windows RDP.  
      
    `xfreerdp /f /u:SG /p:UmbracoIsTheBest! /v:<TARGET_IP>`  
      
    when promoted whether you trust the above certificate use `T`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752093222118/bbf6fb5d-6c63-401a-b009-d981a5edea99.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752093351618/932b0b46-d02b-491b-a69b-c9471fb46ced.png align="center")
    
3. Can we spot the admin password? `ChangeMeBaby1MoreTime`  
      
    To find the file that has the admin password we need to change the file settings to show hiddens files
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752093385597/738f6c24-c0db-4b22-9984-ec98c04f0ab4.png align="center")
    
    we’ll now be able to see backups folder that has a restore.txt file  
      
    Next we’ll open the properties of restore then switch to Security, click Edit,
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752093482339/0c1d2444-b9a8-4a5c-985c-eee6b85378b4.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752093761530/fac6b33c-39bc-45ff-b1e6-48eb78596019.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752093823075/19815ee5-99aa-4686-9fd1-65680fd59cae.png align="center")
    
    After applying SG, we’ll now be able to access the restore.txt file to get Admin’s password.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752093879446/ae386c0e-4fd0-4108-b916-ef6dbe8ec072.png align="center")
    
4. Escalate your privileges to root, what is the contents of root.txt? `THM{Y0U_4R3_1337}`  
      
    To escalate privileges we’ll use the password we found in previous step to access the Administrator’s files and folders. Under the Desktop we’ll find the root flag
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752094067092/bf3094cf-93a5-4ebe-9fe9-9fbcd16017b7.png align="center")
    

Using the intelligence gathered from passive reconnaissance — such as hidden passwords, usernames, and CMS info — you gain initial access via RDP using valid credentials. From there, by enabling visibility of hidden files and adjusting file permissions, you uncover the admin password. This allows privilege escalation to the Administrator account, where the final root flag is retrieved. The challenge reinforces careful enumeration and lateral thinking without needing brute-force methods.