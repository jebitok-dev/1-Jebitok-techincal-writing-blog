---
title: "File Permissions Challenge: Linux Essentials: Permission Master"
datePublished: Sat Mar 23 2024 16:27:35 GMT+0000 (Coordinated Universal Time)
cuid: clu4azzd0000109gwa07w6yzz
slug: file-permissions-challenge-linux-essentials-permission-master
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1711211140368/4b087c10-efe1-4027-8ae7-f78462d8e093.png
tags: linux-for-beginners, linux-basics, file-permission

---

Acknowledgment: [https://cybertalents.com/learn/linux-essentials/units/t-linux-essentials/lessons/8-file-permissions/challenges/permission-master](https://cybertalents.com/learn/linux-essentials/units/t-linux-essentials/lessons/8-file-permissions/challenges/permission-master)

To solve this challenge you have to understand File Permissions i.e. owner, groups, and others based on the different privileges they have i.e. read, write and execute.

We also need to understand how to change file permissions using `chmod` command which stands for change mode. The `drwxrwxrwx` where d stands for directory, r for read, w for write, and x for execute. Each user i.e owner, group, and others will have an allocation of these privileges i.e

* We have this :
    
    * 7 → `rwx` → 111
        
    * 6 → `rw-` → 110
        
    * 5 → `r-w` → 101
        
    * 4 → `r--` → 100
        
    * 3 → `-wx` → 011
        
    * 2 → `-w-` → 010
        
    * 1 → `--x` → 001
        
    * 0 → `---` → 000
        
* We can run `chmod 777` when owner the owner has read, write, and execute privileges as well as the group (`rwx`) and others (`rwx`). It means each of them has 7 when we put together 7 from each group we get 777.
    
* going back to the terminal
    
    ```plaintext
    :~$ cd /tmp 
    :/tmp$ ls -la /tmp/perm_tmp
    -rw-r--r-- 1 ctf ctf 16848 Aug  6  2021 /tmp/perm_master
    :/tmp$ chmod 311 /tmp/perm_master
    :/tmp$ ./perm_master
    Your goal is to change the fiile permissions to --x-wx--x 
    :/tmp$ chmod 131 /tmp/perm_master
    :/tmp$ ./perm_master
    Your goal is to change the fiile permissions to --x-wx--x 
    
    [+] Flag= ZmxhZ3t5b3VyX25ld19uaWNrbmFtZV9pc19wZXJtQE1hc3Rlcn0=
    :/tmp$ echo "ZmxhZ3t5b3VyX25ld19uaWNrbmFtZV9pc19wZXJtQE1hc3Rlcn0=" | base64 -d
    flag{your_new_nickname_is_perm@Master}
    :/tmp$
    ```
    
    ```plaintext
    -rw-r--r-- 1 ctf ctf 16848 Aug  6  2021 /tmp/perm_master
    :/tmp$ chmod 311 /tmp/perm_master
    :/tmp$ ./perm_master
    Your goal is to change the fiile permissions to --x-wx--x
    ```
    
* here we realize that we have 3 for the owner, 1 for both groups and others, that is why we use `chmod 311` it before executing our file which shows us the goal we have to get our flag.
    
* The goal shows that we need to change owner permissions to only have execute and the group to have write and execute privileges. To achieve this, it means the owner has 1, the group has 3 and the others have 1, meaning we are going to run `chmod 131` and execute our file to get the flag
    
* ```plaintext
      :/tmp$ chmod 131 /tmp/perm_master
      :/tmp$ ./perm_master
      Your goal is to change the fiile permissions to --x-wx--x 
      
      [+] Flag= ZmxhZ3t5b3VyX25ld19uaWNrbmFtZV9pc19wZXJtQE1hc3Rlcn0=
    ```
    
* we now have a hint `"ZmxhZ3t5b3VyX25ld19uaWNrbmFtZV9pc19wZXJtQE1hc3Rlcn0="` of what our flag is but we need a flag in the format `flag{}` to solve our challenge. So we have to use some concepts we learned previously in Text Manipulation i.e base64 encoding
    
* ```plaintext
      :/tmp$ echo "ZmxhZ3t5b3VyX25ld19uaWNrbmFtZV9pc19wZXJtQE1hc3Rlcn0=" | base64 -d
      flag{your_new_nickname_is_perm@Master}
    ```
    
    Our flag is now revealed congratulations!
    

Thank you for reading through, we can connect more on [X](https://twitter.com/SharonJebitok), and [LinkedIn](https://www.linkedin.com/in/sharon-jebitok/), or can [Book a Session with Sharon via Mentorlst](https://mentorlst.com/mentor/12db03d0-8a75-4a32-9b05-b290f5381598).