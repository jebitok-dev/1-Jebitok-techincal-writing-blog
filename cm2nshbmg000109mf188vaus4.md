---
title: "Command Line: Linux Shell (TryHackMe)"
datePublished: Thu Oct 24 2024 21:02:14 GMT+0000 (Coordinated Universal Time)
cuid: cm2nshbmg000109mf188vaus4
slug: command-line-linux-shell-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729803701655/20dd683f-fa57-4ba5-86b4-4865a48188fd.png
tags: linux, shell, cybersecurity-1

---

In this article, I will write a write-up for the Linux Shell that covers Interacting With a Shell, Types of Linux Shells, Shell Scripting and Components, The Locker Script and a Practical Exercise at the end.

1. Who is the facilitator between the user and the OS? `Shell`
    
2. What is the default shell in most Linux distributions? `Bash`
    
3. Which command utility is used to list down the contents of a directory? `ls`
    
4. Which command utility can help you search for anything in a file? `grep`
    
5. Which shell comes with syntax highlighting as an out-of-the-box feature? `Fish`
    
6. Which shell does not have auto spell correction? `Bash`
    
7. Which command displays all the previously executed commands of the current session? `history`
    
8. What is the shebang used in a Bash script? `#!/bin/bash`
    
9. Which command gives executable permissions to a script? `chmod +x`
    
10. Which scripting functionality helps us configure iterative tasks? `loops`
    
11. What would be the correct PIN to authenticate in the locker script? `7385`
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729803484844/eaf15265-e5b3-44a3-9806-684724ead4e3.png align="center")
    
    **Practical Exercise**  
      
    We have placed a script on the default user directory `/home/user` of the attached Ubuntu machine. This script searches for a specific keyword in all the files (with .log extension) in a specific directory.  
      
    **Note:** Some changes are required inside the script file before you execute it. When you open the machine according to the instructions in task #2, you will be able to gain the session as a normal user. However, we recommend you to become the root user in order to search for the flag in all the files of the given directory. To become one, you only need to type the following command and enter the password of the user:
    
    Become Root User
    
    ```plaintext
    user@ubuntu:~$ sudo su[sudo] password for user:
    root@tryhackme:/home/user#
    
    ```
    
    You can make the changes in the script file by keeping in view the following details:
    
    * **Flag:** thm-flag01-script
        
    * **Directory:** /var/log
        
    
    **Hint:** Look for empty double quotes `" "` inside the script file and fill them. Make sure not to leave any space between them.
    
    Answer the questions below
    
12. Which file has the keyword? `authentication.log`
    

* The image below answers the next questions 12 & 13
    
* First you’ll get to \*\*`/home/user` then you’ll `cd ../..` then you’ll `cd** /var/log`
    
* when you run ls you’ll see a lot of files that end with .log, you can open each with `nano filename.log` but remember the hint of the flag and something about a cat sleeping
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729803394900/246277dd-eaa5-43f1-8fce-2a70d27fd250.png align="center")

13. Where is the cat sleeping? `under the table`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [\*\*X](https://x.com/SharonJebitok)\*\*