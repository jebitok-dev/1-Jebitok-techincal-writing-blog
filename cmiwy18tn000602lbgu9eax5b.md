---
title: "Network Discovery - Scan-ta Clause (TryHackMe) 🧑‍🎄🎉"
datePublished: Sat Dec 06 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmiwy18tn000602lbgu9eax5b
slug: network-discovery-scan-ta-clause-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765185625949/e614a5b4-7783-40e9-a85a-d47555eada9f.png
tags: ftp, mysql, nmap, tryhackme, adventofcyber2025

---

As part of our SOC‑mas journey, today’s challenge brings us deep into the fundamentals of network service discovery. While TBFC’s QA environment has been compromised, this task is the perfect opportunity to sharpen our skills with real‑world techniques used by defenders and attackers alike.

This walkthrough builds on TryHackMe’s original storyline without changing it—our goal here is to expand the learning experience, reinforce the key concepts, and provide a clear path through the investigation. From simple port scans to interacting with custom services and DNS queries, this challenge highlights how exposed services can act as hidden entry points during an incident.

With each discovered key and every analyzed port, we not only fight back against HopSec’s EAST‑mas chaos but also strengthen our understanding of how critical service enumeration is during reconnaissance, pentesting, and incident response.

## Introduction

**The Story**

![Task banner for day 7](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1763378850592.png align="left")

Christmas preparations are delayed - HopSec has breached our QA environment and locked us out! Without it, the TBFC projects can't be tested, and our entire SOC-mas pipeline is frozen. To make things worse, the server is slowly transforming into a twisted EAST-mas node.

Can you uncover HopSec's trail, find a way back into **tbfc-devqa01**, and restore the server before the bunny's takeover is complete? For this task, you'll need to check every place to hide, every opened port that bunnies left unprotected. Good luck!

![Three bad bunnies with three hidden easter eggs occupy the breached server in a Christmas theme.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1763379081488.png align="left")

## **Learning Objectives**

* Learn the basics of network service discovery with Nmap
    
* Learn core network protocols and concepts along the way
    
* Apply your knowledge to find a way back into the server
    

## Discover Network Services

## **Discovering Exposed Services**

Although we lost access to the QA server, at least it's still active, and we know its IP address. That's good news, since now we can counterattack and hopefully find our way back. Ensure you understand basic [Networking Concepts](https://tryhackme.com/room/networkingconcepts) like network ports, and let's plan the engagement!

1. Know your target. In our case, it is the `tbfc-devqa01` server with the `MACHINE_IP` IP address.
    
2. Scan the IP for open ports, especially common ones like 22 for SSH and 80 for HTTP.
    
3. Explore what's behind the open ports, maybe it's a vulnerable web server running on port 80.
    
4. Exploit the exposed service, find a way in, and kick out the bad bunnies from the QA server.
    

Along the practical of today's task you will find three keys.  
Keep note of them since you will later need them for the web app.  
The format will be `KEYNAME:KEY`.

**The Simplest Port Scan**

There are many tools you can use to scan for open ports, from preinstalled Netcat on Linux and PowerShell on Windows, to specialized, powerful tools like Nmap and Naabu. Let's use Nmap for this task and perform a basic scan from the AttackBox or your own VPN-connected attacking machine. Open a new command line terminal and run the following command:

BasicNmapScan

```markdown
root@attackbox:~# nmap MACHINE_IP
Nmap scan report for MACHINE_IP
Host is up (0.061s latency).
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
```

The command scanned the top 1000 most commonly used ports and reported if any services were running there. The only results you received are an opened SSH port for remote machine access and a HTTP port for a website. This means you can now access the server via SSH (if you know the password), or open the website by visiting `http://MACHINE_IP` from within the AttackBox:

![A screenshot from the tbfc-qa server, where evil bunnies left the "pwned" message.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1762976351953.png align="left")

**Scanning Whole Range**

It seems like the website is defaced by bad bunnies, and we don't know the key to enter the admin panel! But worry not. We scanned just 1000 ports, but there are actually 65535 ports where other services can hide! Now let's add the `-p-` argument to scan all ports, and `--script=banner` to see what's likely behind the port:

Specifying Port Range

```markdown
root@attackbox:~# nmap -p- --script=banner MACHINE_IP
Nmap scan report for MACHINE_IP
Host is up (0.00036s latency).
Not shown: 65531 filtered ports
PORT      STATE SERVICE
22/tcp    open  ssh
|_banner: SSH-2.0-OpenSSH_9.6p1 Ubuntu-3ubuntu13.14
80/tcp    open  http
21212/tcp open  trinket-agent
|_banner: 220 (vsFTPd 3.0.5)
25251/tcp open  unknown
|_banner: TBFC maintd v0.2\x0AType HELP for commands.
```

Looks like you found a running FTP server and some custom TBFC application. Even though FTP runs on port 21 by default, it's possible to change the port to any other one, such as 21212. Let's try accessing the FTP in anonymous mode with the `ftp` command and see if we can find our way in! You can follow the commands from the terminal below:

UsingFTPClient

```markdown
root@attackbox:~# ftp MACHINE_IP 21212
Connected to 10.10.239.62.
Name (10.10.239.62:root): anonymous
ftp> ls
-rw-r--r--  1  ftp  ftp  13 Oct 22 16:27 tbfc_qa_key1
ftp> get tbfc_qa_key1 -
[GET HIDDEN KEY 1 FROM HERE]
ftp> ! # Will exit the FTP shell
```

**Port Scan Modes**

Good job finding the first flag part. There is nothing more we can see on the FTP server, so let's move on to the custom TBFC app on port 25251. Since it is not a well-known service like HTTP or FTP, your web browser or FTP client won't know how to access it. Luckily, you can always use Netcat (`nc`), a universal tool to interact with network services:

Using Netcat Client

```markdown
root@attackbox:~# nc -v MACHINE_IP 25251
Connection to 10.10.239.62 25251 port [tcp/*] succeeded!
TBFC maintd v0.2
Type HELP for commands.
HELP                                   # Your input/request
Commands: HELP, STATUS, GET KEY, QUIT  # Server response
[RUN THE CORRECT COMMAND TO GET HIDDEN KEY 2]
```

Once you received the key, press `CTRL+C` to exit the Netcat client.

**TCP and UDP Ports**

Congratulations on the second flag! But where to look for the third one? Till now, you have scanned only TCP ports, but there are also 65535 ports for UDP, another transport protocol. And there is a chance HopSec secrets are hiding there, too! You can switch to UDP scan by specifying the `-sU` flag:

ScanningUDPPorts

```markdown
root@attackbox:~# nmap -sU MACHINE_IP
PORT   STATE SERVICE
53/udp open  domain
[...]
```

After a minute you should see an open port 53 associated with DNS - a protocol that drives the modern web by connecting domains to IPs, and many more! DNS is a complex topic and many secrets can hide there, but let's just ask the DNS server if it knows the key by using `dig` - a command to perform advanced DNS queries:

QueryingDNSServer

```markdown
root@attackbox:~# dig @MACHINE_IP TXT key3.tbfc.local +short
[RUN THE CORRECT COMMAND TO GET HIDDEN KEY 3]
```

## **On-Host Service Discovery**

Now that you know all three keys to the **tbfc-devqa01** QA server, it's time to call your TBFC teammates and kick out the bad bunnies. But first, log in to the server's admin panel by visiting `http://MACHINE_IP` from within the AttackBox and access the secret admin console by submitting the combined keys:

![The terminal window you will see after combining three key parts on the tbfc-qa website.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1761256003855.png align="left")

**Listing Listening Ports**

Once you have access to the console, there is no need to scan the ports, as you can simply ask the OS to list its open ports, also called listening ports. You can do it by running `ss -tunlp` (or `netstat` on older systems) inside the Secret Admin Console of the web app. In the output, you may see exactly the same services you scanned before listening on `0.0.0.0`, but also some listening on `127.0.0.1` (available only from the host itself):

Listing Listening Ports

```markdown
tbfcapp@tbfc-devqa01:~$ ss -tunlp
Netid  State   Recv-Q  Send-Q       Local Address:Port      Peer Address:Port  Process                   
udp    UNCONN  0       0                  0.0.0.0:53             0.0.0.0:*                               
udp    UNCONN  0       0        10.10.76.132%ens5:68             0.0.0.0:*                               
tcp    LISTEN  0       50                 0.0.0.0:25251          0.0.0.0:*                               
tcp    LISTEN  0       32                 0.0.0.0:21212          0.0.0.0:*                               
tcp    LISTEN  0       4096               0.0.0.0:22             0.0.0.0:*                               
tcp    LISTEN  0       32                 0.0.0.0:53             0.0.0.0:*                               
tcp    LISTEN  0       511                0.0.0.0:80             0.0.0.0:*                               
tcp    LISTEN  0       2048             127.0.0.1:8000           0.0.0.0:*
tcp    LISTEN  0       151              127.0.0.1:3306           0.0.0.0:*                               
tcp    LISTEN  0       4096             127.0.0.1:7681           0.0.0.0:*                               
tcp    LISTEN  0       4096                  [::]:22                [::]:*                                       
```

With root permissions, you can also view the process column. However, for now, let's focus on the `3306` port, which is the default MySQL database port. Usually databases require a password for remote clients, but allow unauthenticated logins from localhost. Since you are already inside the host, let's see the database content by using the `mysql` program:

Enumerating MySQL Tables

```markdown
tbfcapp@tbfc-devqa01:~$ mysql -D tbfcqa01 -e "show tables;"            
+--------------------+                                                 
| Tables_in_tbfcqa01 |                                                 
+--------------------+                                                 
| flags              |                                                 
+--------------------+                                                 
tbfcapp@tbfc-devqa01:~$ mysql -D tbfcqa01 -e "select * from flags;"    
[GET YOUR FLAG HERE]    
```

Great job on finding the flag! You have exposed all bunnies' secrets and regained full access to the QA server. Now it's time to secure all ports and restore the SOC-mas preparation pipeline. But for now, answer the questions below and finish the task!

### Answer the questions below

1. What evil message do you see on top of the website?  
      
    To find the message, just visit the site on the browser and look around just hint the word is close to the site icon  
      
    `nmap IP_Address`  
      
    `nmap -p- --script=banner IP_Address`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765183720878/bf8dd2e4-0a56-406a-8066-a1444f89d67b.png align="center")
    
    This nmap scan shows us the open ports, like `ssh`, `http`, `trinket-agent - vsFTPd`, and an `unknown` one. Open ports give us a hint of what we can access. It’s helpful during reconnaissance, especially in penetration testing.  
    
2. What is the first key part found on the FTP server?  
      
    ftp is used to upload and download files between a client and a server  
      
    `ftp IP_Address`  
      
    ftp&gt; `anonymous`  
      
    ftp&gt; `ls`  
      
    ftp&gt; `get tbfc_qa_key1`  
      
    ftp&gt; `exit`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765184108623/0b645794-df53-4855-b0d9-b0718b014133.png align="center")
    
    `cat tbfc_qa_key1`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765184643918/cbfce997-4022-45fb-a556-30c0e4d8d8f3.jpeg align="center")
    
3. What is the second key part found in the TBFC app?  
      
    `nc -v IP_Address 25251`  
      
    `HELP`  
      
    `GET KEY`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765184783240/c6a8ae4d-f0f5-4695-b7e8-8ea8e9202fa1.jpeg align="center")
    
4. What is the third key part found in the DNS records?  
      
    `nmap -sU IP_Address`  
      
    `dig @IP_Address TXT key3.tbfc.local +short`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765185126273/c34e2fd4-ee05-4124-b1a9-92f29365b783.jpeg align="center")
    
    Next step, we now visit the site and combine the three keys to access the Terninal
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765185252333/8bfe317b-dc0c-42f4-809d-17d82b39e420.jpeg align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765185282037/d71fafab-7f58-417b-acc4-311a4c108d6a.png align="center")
    
    `echo “Hello World!“`  
      
    `pwd`  
      
    `ls -la`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765185321405/a36cdaad-2797-495c-9860-8dc963aa1d42.png align="center")
    
5. Which port was the MySQL database running on?  
      
    `ss -tunlp`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765185355992/9d835170-743e-4e9a-922d-67c462155b22.jpeg align="center")
    
6. Finally, what's the flag you found in the database?  
      
    `mysql -D tbfcqa01 -e “show tables;”`  
      
    `mysql -D tbfcqa01 -e “select * from flags;”`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765185465516/75cf7461-43c3-4392-88b5-c7e4b08f206b.jpeg align="center")
    
7. If you enjoyed today's room, feel free to check out the [Nmap: The Basics](https://tryhackme.com/room/nmap) room.
    

By the end of this task, we successfully uncovered all three key parts, accessed the secret admin console, enumerated on‑host services, and retrieved the final flag from the database. More importantly, we saw firsthand how attackers abuse overlooked ports and misconfigurations—and how defenders can detect, investigate, and respond to such activity.

This challenge reinforces why port scanning, service discovery, and protocol awareness remain core skills for cybersecurity professionals. Whether interacting with FTP, probing custom TCP services, or analyzing DNS records, each step mirrored real investigative work performed during incident response.

Now that the QA server has been restored and HopSec’s mischief uncovered, you’re better equipped to approach future SOC‑mas challenges and more advanced reconnaissance tasks. On to the next mission! 🎄🛡️