---
title: "b3dr0ck - NGINX, Certificate-based Authentication & OpenSSL (TryHackMe)"
datePublished: Wed Jan 28 2026 19:58:03 GMT+0000 (Coordinated Universal Time)
cuid: cmkyg6hbm000002l1amovhkhi
slug: b3dr0ck-nginx-certificate-based-authentication-and-openssl-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769628397317/1b2998c3-9e3f-4902-a090-b2a5f70eb5af.png
tags: nginx, web-security, openssl, tryhackme, ctf-writeup

---

The b3dr0ck challenge takes us to Bedrock, where Barney Rubble is setting up the ABC (Abbadabba Broadcasting Company) webserver and struggling with TLS certificate configuration. This beginner-to-intermediate level challenge focuses on certificate-based authentication, privilege escalation, and multi-layer encoding techniques.

**Challenge Overview:** The scenario presents us with a misconfigured server where Barney and his colleague Bamm Bamm have been attempting to secure their web services with SSL/TLS certificates. Our objective is to exploit their misconfigurations to gain access to user accounts and ultimately achieve root access.

**Key Learning Objectives:**

* Understanding SSL/TLS client certificate authentication
    
* Working with OpenSSL for certificate validation and connection
    
* Certificate retrieval from custom services
    
* Privilege escalation through sudo misconfigurations
    
* Multi-layer encoding and decoding (base64, base32, hash cracking)
    

The challenge provides several hints through the initial webpage, including references to port 9009 ("OVER 9000!"), database mishaps, and certificate management issues that point us toward our attack vectors.

## Yabba-Dabba-Doo

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/6244f6c52f56e10056a5f483/room-content/689b67cf1c8c5e1eb83c54b80360123b.jpg align="left")

**Fred Flintstone   &   Barney Rubble!**

Barney is setting up the ABC webserver, and trying to use TLS certs to secure connections, but he's having trouble. Here's what we know...

* He was able to establish `nginx` on port `80`,  redirecting to a custom TLS webserver on port `4040`
    
* There is a TCP socket listening with a simple service to help retrieve TLS credential files (client key & certificate)
    
* There is another TCP (TLS) helper service listening for authorized connections using files obtained from the above service
    
* Can you find all the Easter eggs?
    

*Please allow an extra few minutes for the VM to fully startup﻿.*

### Answer the questions below

1. What is the barney.txt flag?  
      
    `nmap -p- -sV <IP_Address>`  
      
    open ports: 22, 80, 4040, and 9009
    
    ```bash
    nmap -p- -sV <IP_Address>
    Starting Nmap 7.80 ( https://nmap.org ) at 2026-01-28 17:18 GMT
    mass_dns: warning: Unable to open /etc/resolv.conf. Try using --system-dns or specify valid servers with --dns-servers
    mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
    Nmap scan report for 10.49.158.183
    Host is up (0.00011s latency).
    Not shown: 65530 closed ports
    PORT      STATE SERVICE      VERSION
    22/tcp    open  ssh          OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
    80/tcp    open  http         nginx 1.18.0 (Ubuntu)
    4040/tcp  open  ssl/yo-main?
    9009/tcp  open  pichat?
    54321/tcp open  ssl/unknown
    3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
    ==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
    SF-Port4040-TCP:V=7.80%T=SSL%I=7%D=1/28%Time=697A44EC%P=x86_64-pc-linux-gn
    SF:u%r(GetRequest,3BE,"HTTP/1\.1\x20200\x20OK\r\nContent-type:\x20text/htm
    SF:l\r\nDate:\x20Wed,\x2028\x20Jan\x202026\x2017:18:36\x20GMT\r\nConnectio
    SF:n:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html>\n\x20\x20<head>\n\x20\x20
    SF:\x20\x20<title>ABC</title>\n\x20\x20\x20\x20<style>\n\x20\x20\x20\x20\x
    SF:20\x20body\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20width:\x2035em;\n\x20\
    SF:x20\x20\x20\x20\x20\x20\x20margin:\x200\x20auto;\n\x20\x20\x20\x20\x20\
    SF:x20\x20\x20font-family:\x20Tahoma,\x20Verdana,\x20Arial,\x20sans-serif;
    SF:\n\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x20</style>\n\x20\x20</head>\
    SF:n\n\x20\x20<body>\n\x20\x20\x20\x20<h1>Welcome\x20to\x20ABC!</h1>\n\x20
    SF:\x20\x20\x20<p>Abbadabba\x20Broadcasting\x20Compandy</p>\n\n\x20\x20\x2
    SF:0\x20<p>We're\x20in\x20the\x20process\x20of\x20building\x20a\x20website
    SF:!\x20Can\x20you\x20believe\x20this\x20technology\x20exists\x20in\x20bed
    SF:rock\?!\?</p>\n\n\x20\x20\x20\x20<p>Barney\x20is\x20helping\x20to\x20se
    SF:tup\x20the\x20server,\x20and\x20he\x20said\x20this\x20info\x20was\x20im
    SF:portant\.\.\.</p>\n\n<pre>\nHey,\x20it's\x20Barney\.\x20I\x20only\x20fi
    SF:gured\x20out\x20nginx\x20so\x20far,\x20what\x20the\x20h3ll\x20is\x20a\x
    SF:20database\?!\?\nBamm\x20Bamm\x20tried\x20to\x20setup\x20a\x20sql\x20da
    SF:tabase,\x20but\x20I\x20don't\x20see\x20it\x20running\.\nLooks\x20like\x
    SF:20it\x20started\x20something\x20else,\x20but\x20I'm\x20not\x20sure\x20h
    SF:ow\x20to\x20turn\x20it\x20off\.\.\.\n\nHe\x20said\x20it\x20was\x20from\
    SF:x20the\x20toilet\x20and\x20OVER\x209000!\n\nNeed\x20to\x20try\x20and\x2
    SF:0secure\x20")%r(HTTPOptions,3BE,"HTTP/1\.1\x20200\x20OK\r\nContent-type
    SF::\x20text/html\r\nDate:\x20Wed,\x2028\x20Jan\x202026\x2017:18:36\x20GMT
    SF:\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html>\n\x20\x20<he
    SF:ad>\n\x20\x20\x20\x20<title>ABC</title>\n\x20\x20\x20\x20<style>\n\x20\
    SF:x20\x20\x20\x20\x20body\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20width:\x2
    SF:035em;\n\x20\x20\x20\x20\x20\x20\x20\x20margin:\x200\x20auto;\n\x20\x20
    SF:\x20\x20\x20\x20\x20\x20font-family:\x20Tahoma,\x20Verdana,\x20Arial,\x
    SF:20sans-serif;\n\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x20</style>\n\x2
    SF:0\x20</head>\n\n\x20\x20<body>\n\x20\x20\x20\x20<h1>Welcome\x20to\x20AB
    SF:C!</h1>\n\x20\x20\x20\x20<p>Abbadabba\x20Broadcasting\x20Compandy</p>\n
    SF:\n\x20\x20\x20\x20<p>We're\x20in\x20the\x20process\x20of\x20building\x2
    SF:0a\x20website!\x20Can\x20you\x20believe\x20this\x20technology\x20exists
    SF:\x20in\x20bedrock\?!\?</p>\n\n\x20\x20\x20\x20<p>Barney\x20is\x20helpin
    SF:g\x20to\x20setup\x20the\x20server,\x20and\x20he\x20said\x20this\x20info
    SF:\x20was\x20important\.\.\.</p>\n\n<pre>\nHey,\x20it's\x20Barney\.\x20I\
    SF:x20only\x20figured\x20out\x20nginx\x20so\x20far,\x20what\x20the\x20h3ll
    SF:\x20is\x20a\x20database\?!\?\nBamm\x20Bamm\x20tried\x20to\x20setup\x20a
    SF:\x20sql\x20database,\x20but\x20I\x20don't\x20see\x20it\x20running\.\nLo
    SF:oks\x20like\x20it\x20started\x20something\x20else,\x20but\x20I'm\x20not
    SF:\x20sure\x20how\x20to\x20turn\x20it\x20off\.\.\.\n\nHe\x20said\x20it\x2
    SF:0was\x20from\x20the\x20toilet\x20and\x20OVER\x209000!\n\nNeed\x20to\x20
    SF:try\x20and\x20secure\x20");
    ==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
    SF-Port9009-TCP:V=7.80%I=7%D=1/28%Time=697A44DC%P=x86_64-pc-linux-gnu%r(NU
    SF:LL,29E,"\n\n\x20__\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20__\x20\x20_\x
    SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
    SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20_\x20\x20\x20\x20\x20\x20\x20\x2
    SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20____\x20\x20\x20_____\x20\
    SF:n\x20\\\x20\\\x20\x20\x20\x20\x20\x20\x20\x20/\x20/\x20\|\x20\|\x20\x20
    SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
    SF:0\x20\x20\x20\x20\x20\x20\|\x20\|\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
    SF:20\x20\x20/\\\x20\x20\x20\|\x20\x20_\x20\\\x20/\x20____\|\n\x20\x20\\\x
    SF:20\\\x20\x20/\\\x20\x20/\x20/__\|\x20\|\x20___\x20___\x20\x20_\x20__\x2
    SF:0___\x20\x20\x20___\x20\x20\|\x20\|_\x20___\x20\x20\x20\x20\x20\x20/\x2
    SF:0\x20\\\x20\x20\|\x20\|_\)\x20\|\x20\|\x20\x20\x20\x20\x20\n\x20\x20\x2
    SF:0\\\x20\\/\x20\x20\\/\x20/\x20_\x20\\\x20\|/\x20__/\x20_\x20\\\|\x20'_\
    SF:x20`\x20_\x20\\\x20/\x20_\x20\\\x20\|\x20__/\x20_\x20\\\x20\x20\x20\x20
    SF:/\x20/\\\x20\\\x20\|\x20\x20_\x20<\|\x20\|\x20\x20\x20\x20\x20\n\x20\x2
    SF:0\x20\x20\\\x20\x20/\\\x20\x20/\x20\x20__/\x20\|\x20\(_\|\x20\(_\)\x20\
    SF:|\x20\|\x20\|\x20\|\x20\|\x20\|\x20\x20__/\x20\|\x20\|\|\x20\(_\)\x20\|
    SF:\x20\x20/\x20____\x20\\\|\x20\|_\)\x20\|\x20\|____\x20\n\x20\x20\x20\x2
    SF:0\x20\\/\x20\x20\\/\x20\\___\|_\|\\___\\___/\|_\|\x20\|_\|\x20\|_\|\\__
    SF:_\|\x20\x20\\__\\___/\x20\x20/_/\x20\x20\x20\x20\\_\\____/\x20\\_____\|
    SF:\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
    SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
    SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
    SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
    SF:20\x20\x20\x20\x20\x20\x20\x20\x20\n\x20\x20\x20\x20\x20\x20\x20\x20\x2
    SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
    SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
    SF:x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
    SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\n\
    SF:n\nWhat\x20are\x20you\x20looking\x20for\?\x20");
    ==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
    SF-Port54321-TCP:V=7.80%T=SSL%I=7%D=1/28%Time=697A44E1%P=x86_64-pc-linux-g
    SF:nu%r(NULL,31,"Error:\x20'undefined'\x20is\x20not\x20authorized\x20for\x
    SF:20access\.\n")%r(GenericLines,31,"Error:\x20'undefined'\x20is\x20not\x2
    SF:0authorized\x20for\x20access\.\n")%r(GetRequest,31,"Error:\x20'undefine
    SF:d'\x20is\x20not\x20authorized\x20for\x20access\.\n")%r(HTTPOptions,31,"
    SF:Error:\x20'undefined'\x20is\x20not\x20authorized\x20for\x20access\.\n")
    SF:%r(RTSPRequest,31,"Error:\x20'undefined'\x20is\x20not\x20authorized\x20
    SF:for\x20access\.\n")%r(RPCCheck,31,"Error:\x20'undefined'\x20is\x20not\x
    SF:20authorized\x20for\x20access\.\n")%r(DNSVersionBindReqTCP,31,"Error:\x
    SF:20'undefined'\x20is\x20not\x20authorized\x20for\x20access\.\n")%r(DNSSt
    SF:atusRequestTCP,31,"Error:\x20'undefined'\x20is\x20not\x20authorized\x20
    SF:for\x20access\.\n")%r(Help,31,"Error:\x20'undefined'\x20is\x20not\x20au
    SF:thorized\x20for\x20access\.\n")%r(SSLSessionReq,31,"Error:\x20'undefine
    SF:d'\x20is\x20not\x20authorized\x20for\x20access\.\n")%r(TerminalServerCo
    SF:okie,31,"Error:\x20'undefined'\x20is\x20not\x20authorized\x20for\x20acc
    SF:ess\.\n")%r(TLSSessionReq,31,"Error:\x20'undefined'\x20is\x20not\x20aut
    SF:horized\x20for\x20access\.\n")%r(Kerberos,31,"Error:\x20'undefined'\x20
    SF:is\x20not\x20authorized\x20for\x20access\.\n")%r(SMBProgNeg,31,"Error:\
    SF:x20'undefined'\x20is\x20not\x20authorized\x20for\x20access\.\n")%r(X11P
    SF:robe,31,"Error:\x20'undefined'\x20is\x20not\x20authorized\x20for\x20acc
    SF:ess\.\n")%r(FourOhFourRequest,31,"Error:\x20'undefined'\x20is\x20not\x2
    SF:0authorized\x20for\x20access\.\n")%r(LPDString,31,"Error:\x20'undefined
    SF:'\x20is\x20not\x20authorized\x20for\x20access\.\n")%r(LDAPSearchReq,31,
    SF:"Error:\x20'undefined'\x20is\x20not\x20authorized\x20for\x20access\.\n"
    SF:)%r(LDAPBindReq,31,"Error:\x20'undefined'\x20is\x20not\x20authorized\x2
    SF:0for\x20access\.\n")%r(SIPOptions,31,"Error:\x20'undefined'\x20is\x20no
    SF:t\x20authorized\x20for\x20access\.\n")%r(LANDesk-RC,31,"Error:\x20'unde
    SF:fined'\x20is\x20not\x20authorized\x20for\x20access\.\n")%r(TerminalServ
    SF:er,31,"Error:\x20'undefined'\x20is\x20not\x20authorized\x20for\x20acces
    SF:s\.\n")%r(NCP,31,"Error:\x20'undefined'\x20is\x20not\x20authorized\x20f
    SF:or\x20access\.\n");
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 160.45 seconds
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769628489246/e1a6d95a-f2ae-4688-be4a-6ed7068f9596.png align="center")
    
    Opening the browser redirects us to `http://<IP_Address>:4040` that reveals a localhost certificate  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769628604244/baf1b463-846b-460f-91c2-f3d8975f5f9a.png align="center")
    
    ```bash
    Abbadabba Broadcasting Compandy
    
    We're in the process of building a website! Can you believe this technology exists in bedrock?!?
    
    Barney is helping to setup the server, and he said this info was important...
    
    Hey, it's Barney. I only figured out nginx so far, what the h3ll is a database?!?
    Bamm Bamm tried to setup a sql database, but I don't see it running.
    Looks like it started something else, but I'm not sure how to turn it off...
    
    He said it was from the toilet and OVER 9000!
    
    Need to try and secure connections with certificates...
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769628617970/bab53738-6a0c-4144-aede-6d17b261b30d.png align="center")
    
    `nc <IP_Address> 9009`  
      
    This is my first time interacting with this so at first i didn’t know what to ask but help of Claude I was able to make progress to find the user’s full name and their private key as well as the certificate key which helped us to combine them inorder to use OpenSSL to find the user’s password and later login via ssh to get the flag and progress to answer the next questions.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769628629679/6c7b09ef-b12b-47ae-b9a0-e25c2ffe11f6.png align="center")
    
    I had downloaded localhost.pem on the browser, so I made the first attempt
    
    ```bash
    openssl x509 -in localhost.pem -text -noout | grep -i subject
            Subject: CN = localhost
            Subject Public Key Info:
    ```
    
    ```bash
    nc 10.49.158.183 9009
    
    
     __          __  _                            _                   ____   _____ 
     \ \        / / | |                          | |            /\   |  _ \ / ____|
      \ \  /\  / /__| | ___ ___  _ __ ___   ___  | |_ ___      /  \  | |_) | |     
       \ \/  \/ / _ \ |/ __/ _ \| '_ ` _ \ / _ \ | __/ _ \    / /\ \ |  _ <| |     
        \  /\  /  __/ | (_| (_) | | | | | |  __/ | || (_) |  / ____ \| |_) | |____ 
         \/  \/ \___|_|\___\___/|_| |_| |_|\___|  \__\___/  /_/    \_\____/ \_____|
                                                                                   
                                                                                   
    
    
    What are you looking for? client:barney
    Sounds like you forgot your certificate. Let's find it for you...
    
    
    -----BEGIN CERTIFICATE-----
    MIICoTCCAYkCAgTSMA0GCSqGSIb3DQEBCwUAMBQxEjAQBgNVBAMMCWxvY2FsaG9z
    dDAeFw0yNjAxMjgxNzE1MTFaFw0yNzAxMjgxNzE1MTFaMBgxFjAUBgNVBAMMDUJh
    cm5leSBSdWJibGUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC4IjiN
    8jHgooa2B6JNzPYssO9u6+INDXcbykbAMBWnqgmOKvYuN8MRFuEDXlPlxajHPr49
    oT+IcGHYLNn1zmxh43YTHD32uhNYPsni5xgjb6yKo6fqvwPH0XFs3KiR3klc7Idi
    UVT+FozeDHhK8ekjstwUIJIJ/KInZYTE6lhusfhk/QnFZysvxgazMMCDDd0dX+VH
    6kcgRCon3apW7EpWgAzCWqsXkECDdEQcCIZv8bVNWErAKKoj+B4Gi9tx2bZ4jmCu
    XziSBaJJXBet3cqEOi3eQrvCGKfvzqF5flbxd2Oc4DQiBDZzAA53ezA7sSsY0Apr
    iav9jRiKz03CYYBHAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAEc6cCHpYzbcdM1q
    kFrYKrFK9lEmyROMS1WkenBCz/3ESmyOSVerkXe+YPEEmal3on9ZFWxFqsRxLt2o
    bLn0iNqLdywAFh/FeYroLGtiGMuc+bGy2HQD9+Hm6WKg7cs6DgPSZg22eq1Ko8QR
    H8xJCX180vHTU2VsiVtWlfwlDqTXe0Yg6SMQKRd02ce/N2bywme5ARbV0UbbrXaq
    Z9M9pfiTuzeEmMcYzaLyddFJ/X3O0ssNm4mgeGqeWCFvJjM8a52lsjKOWqBDL7qS
    kIDm+mAyn7zsyR+0e7z7HVcIO46mCWyD4X7Uwg1Zyi/rCMfI3LDf08WMQ7d/tzEz
    cd6wAIQ=
    -----END CERTIFICATE-----
    ```
    
    the output was the same as the above but it counts
    
    ```bash
    What are you looking for? client:Barney Rubble
    Sounds like you forgot your certificate. Let's find it for you...
    
    -----BEGIN CERTIFICATE-----
    MIICoTCCAYkCAgTSMA0GCSqGSIb3DQEBCwUAMBQxEjAQBgNVBAMMCWxvY2FsaG9z
    dDAeFw0yNjAxMjgxNzE1MTFaFw0yNzAxMjgxNzE1MTFaMBgxFjAUBgNVBAMMDUJh
    cm5leSBSdWJibGUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC4IjiN
    8jHgooa2B6JNzPYssO9u6+INDXcbykbAMBWnqgmOKvYuN8MRFuEDXlPlxajHPr49
    oT+IcGHYLNn1zmxh43YTHD32uhNYPsni5xgjb6yKo6fqvwPH0XFs3KiR3klc7Idi
    UVT+FozeDHhK8ekjstwUIJIJ/KInZYTE6lhusfhk/QnFZysvxgazMMCDDd0dX+VH
    6kcgRCon3apW7EpWgAzCWqsXkECDdEQcCIZv8bVNWErAKKoj+B4Gi9tx2bZ4jmCu
    XziSBaJJXBet3cqEOi3eQrvCGKfvzqF5flbxd2Oc4DQiBDZzAA53ezA7sSsY0Apr
    iav9jRiKz03CYYBHAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAEc6cCHpYzbcdM1q
    kFrYKrFK9lEmyROMS1WkenBCz/3ESmyOSVerkXe+YPEEmal3on9ZFWxFqsRxLt2o
    bLn0iNqLdywAFh/FeYroLGtiGMuc+bGy2HQD9+Hm6WKg7cs6DgPSZg22eq1Ko8QR
    H8xJCX180vHTU2VsiVtWlfwlDqTXe0Yg6SMQKRd02ce/N2bywme5ARbV0UbbrXaq
    Z9M9pfiTuzeEmMcYzaLyddFJ/X3O0ssNm4mgeGqeWCFvJjM8a52lsjKOWqBDL7qS
    kIDm+mAyn7zsyR+0e7z7HVcIO46mCWyD4X7Uwg1Zyi/rCMfI3LDf08WMQ7d/tzEz
    cd6wAIQ=
    -----END CERTIFICATE-----
    ```
    
    ```bash
    What are you looking for? privatekey:barney
    Sounds like you forgot your private key. Let's find it for you...
    
    -----BEGIN RSA PRIVATE KEY-----
    MIIEowIBAAKCAQEAuCI4jfIx4KKGtgeiTcz2LLDvbuviDQ13G8pGwDAVp6oJjir2
    LjfDERbhA15T5cWoxz6+PaE/iHBh2CzZ9c5sYeN2Exw99roTWD7J4ucYI2+siqOn
    6r8Dx9FxbNyokd5JXOyHYlFU/haM3gx4SvHpI7LcFCCSCfyiJ2WExOpYbrH4ZP0J
    xWcrL8YGszDAgw3dHV/lR+pHIEQqJ92qVuxKVoAMwlqrF5BAg3REHAiGb/G1TVhK
    wCiqI/geBovbcdm2eI5grl84kgWiSVwXrd3KhDot3kK7whin786heX5W8XdjnOA0
    IgQ2cwAOd3swO7ErGNAKa4mr/Y0Yis9NwmGARwIDAQABAoIBADoiO76873hfd65y
    vJBZnINBnWZkvKqoTKmOdEXrKHsV2Qx0jeLLoh98HLBmkJBjlkTJj2B/z0IxUQLi
    LjJH6LoDCV1BNmL2HnNm53Bmy50sfcoLn1Ei23uzqV7YT7Q1/O2Ek727tdy+m9zj
    DPuEYrntl0QXgGP8yeksSP2uixrvNkfYoaSYQfBck+37P9NBNneoCTbLwSGmDLJl
    Sxi82rG7ZhpMGT1E3DzHseJ//zm1UO+y5L+cEt90U9Ha+APxKfEc/Mfni3e9y1ic
    zzZONpDooDkIkaPg7Jbo0pVbSC9eRkWeHyDCGJ5gFaR6TMm4gjRhfNNGXoat6kfi
    WP41JzECgYEA2dWkm+gl5ob9XvpbGUvqKeZ5lkYslUM8XWWUQsu+lRE47h3fxCSL
    8ng6yMVeIIO9vYRTCVktt54O2zOOWBLjskkmYrYpMw2EB3xfloimJRQ7c6ZVcHzR
    PkZDlMlElIf55LSC8D3r+QVq/mwPq3SFg6SfLNnlay+rcxzA4gjjt/kCgYEA2GUF
    JMO7TeGtvcZ6sVed61mpBEdfvRrkEE6z44QaISCHvlN8rVnBzwHVqhRc2buyLA8Z
    w3sWe44AFb+xFvOaM4TgY/Up0VB5/peu/iTERiSLg0a6IBKU67yaqFhO32SSmGim
    c6zZO//XdBT46JyXzaY0VvJExtwnZAGnH8NDij8CgYBc7mcOWEirA1XeiiIJNbjN
    bVTbxk5I02lSUv6gTHme8Lz/4ODQteK6a45wjrrsVifBTU17hH8bgaPG4kTM0Cox
    qkFlBV+oNxDnfKju+Jcg5HPEj+aNWcKOtb0pIYBPeX8jkdr2kQQ1tZkCt6krtUcs
    dTOvF7pnMuwK7O5gv/geSQKBgF1OufgPWh2L+WHCSrdbg2GvKJ0KtbKR/NM3TIQH
    O0kbgMzFrRYGhPZg4SPUYfEU6Q/sdvx+f8RJB6pZHOA7rDsYybTwStbRr7ji6Pb3
    HZbtPScfATTBLmuzyLBcW64h7MBBJ7zrRESF3f5U6LtMgnyPdTQkhRmGZonN2KVu
    ihn7AoGBAJIGOA3XcavV238UOYqfIw1C9UcXgcRrC34Oo901fynQ0dVInLiZHiil
    83yW9jtqyxslnK9TzGMDHPl7D/jSTWZgddGHmoO/hrgzYCoo2vuo9VyAVxcDGt5/
    qkZc9oum1IMlZIidSR0K2C2rzHl7ov1IyaZcE3ZWW1nRZLLm3A2P
    -----END RSA PRIVATE KEY-----
    ```
    
    ```bash
    cat barney.pem barney_priv.pem > barney_full.pem
    root@ip-10-49-117-136:~# chmod 600 barney_full.pem
    root@ip-10-49-117-136:~# openssl s_client -connect 10.49.158.183:54321 -cert barney_full.pem -key barney_full.pem
    CONNECTED(00000003)
    Can't use SSL_get_servername
    depth=0 CN = localhost
    verify error:num=18:self signed certificate
    verify return:1
    depth=0 CN = localhost
    verify return:1
    ---
    Certificate chain
     0 s:CN = localhost
       i:CN = localhost
    ---
    Server certificate
    -----BEGIN CERTIFICATE-----
    MIICrzCCAZcCFBxEkqCvJ0eC8da/5oBmtcDo2F9nMA0GCSqGSIb3DQEBCwUAMBQx
    EjAQBgNVBAMMCWxvY2FsaG9zdDAeFw0yNjAxMjgxNzE1MTFaFw0yNzAxMjgxNzE1
    MTFaMBQxEjAQBgNVBAMMCWxvY2FsaG9zdDCCASIwDQYJKoZIhvcNAQEBBQADggEP
    ADCCAQoCggEBAN32A7c63nBX50DNFjV4hpFJqoYT42jNXbWurTsCSHx29zwqwcLP
    tJf+RWSp9/sRhZpKqz94aWoszS7RAdFvvSG6hwGKuW/e9vVwI7jFdfN8I9Xsj1Ei
    00PlepgXLzXfTZCilTKkEL5non5WgS8br9im7GZtww9lny/gEsu1qFrcW2wSpDjb
    WhPwjF+gzA8pqAKHOniiM+7wPkK5qf/B6qENLlRuBkPg3C33xSA9Z8MKxHoHTjqR
    F8bRz864jMwHCgSlxbEB/ABwlZF5srKPEbxl9mxWMBVML5aHPfLHoqg4tCYcXQaY
    1be/pKj8mp7qJrRa/YkaqdIWU27+H30mvxsCAwEAATANBgkqhkiG9w0BAQsFAAOC
    AQEAg404Rkq8lzbwR+mDBhSyvS882Dm9k9hkqZAS8+DlIdUKBJfYHw8j0s8L4xgF
    xN5YsTerUDpYZP/diVeKkxSUAB97GHVq6ZAtB4qC8hEIH2cuP0u73Vrd+SrneK2t
    oNDxPOE/KGbOP6e50S5UCVsWv0aYloxq28Zuojzwetzq4YgOFQziQVLvRc6McG5b
    PUfG6Dda6VnMgvKmVgyik7ld87h6GNs8jGWW1o9karWSW5BDxbqrMiZVbiObAnQn
    b2pNE9p+P3ZbXpxIQj1XNmEKJ2SeNHp0HHq4utWac2kNrRYhgtcP6JTMMr9pv5FR
    B2KWq80gEU9NJ3TK6E6goRR6cw==
    -----END CERTIFICATE-----
    subject=CN = localhost
    
    issuer=CN = localhost
    
    ---
    Acceptable client certificate CA names
    CN = localhost
    Requested Signature Algorithms: ECDSA+SHA256:ECDSA+SHA384:ECDSA+SHA512:Ed25519:Ed448:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA+SHA256:RSA+SHA384:RSA+SHA512:ECDSA+SHA224:ECDSA+SHA1:RSA+SHA224:RSA+SHA1
    Shared Requested Signature Algorithms: ECDSA+SHA256:ECDSA+SHA384:ECDSA+SHA512:Ed25519:Ed448:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA+SHA256:RSA+SHA384:RSA+SHA512
    Peer signing digest: SHA256
    Peer signature type: RSA-PSS
    Server Temp Key: X25519, 253 bits
    ---
    SSL handshake has read 1348 bytes and written 1361 bytes
    Verification error: self signed certificate
    ---
    New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
    Server public key is 2048 bit
    Secure Renegotiation IS NOT supported
    Compression: NONE
    Expansion: NONE
    No ALPN negotiated
    Early data was not sent
    Verify return code: 18 (self signed certificate)
    ---
    
    
     __     __   _     _             _____        _     _             _____        _ 
     \ \   / /  | |   | |           |  __ \      | |   | |           |  __ \      | |
      \ \_/ /_ _| |__ | |__   __ _  | |  | | __ _| |__ | |__   __ _  | |  | | ___ | |
       \   / _` | '_ \| '_ \ / _` | | |  | |/ _` | '_ \| '_ \ / _` | | |  | |/ _ \| |
        | | (_| | |_) | |_) | (_| | | |__| | (_| | |_) | |_) | (_| | | |__| | (_) |_|
        |_|\__,_|_.__/|_.__/ \__,_| |_____/ \__,_|_.__/|_.__/ \__,_| |_____/ \___/(_)
                                                                                     
                                                                                     
    
    ---
    Post-Handshake New Session Ticket arrived:
    SSL-Session:
        Protocol  : TLSv1.3
        Cipher    : TLS_AES_256_GCM_SHA384
        Session-ID: 1A9914B5E3C930393DF81C5172BED275F2442A27E7F2D0E68EA95AA1948CBAA9
        Session-ID-ctx: 
        Resumption PSK: A0382900BF7F0511B2F3FA19ACC6E9B3FD655CCDE7F5A2DF54B1DD3AA9303EDB86A3AFF12934F95EAD206C282E484388
        PSK identity: None
        PSK identity hint: None
        SRP username: None
        TLS session ticket lifetime hint: 7200 (seconds)
        TLS session ticket:
        0000 - 30 5e 1f d5 53 96 1f 5c-20 0f d8 50 d6 e9 cd 15   0^..S..\ ..P....
        0010 - 2a b8 72 6c 20 51 94 91-f3 8b 40 83 e5 22 e7 ce   *.rl Q....@.."..
        0020 - 58 95 0a cb 5f d2 c8 63-e2 a7 09 fd e7 f4 56 22   X..._..c......V"
        0030 - c0 79 e6 22 bc 22 d3 5f-34 28 5a 69 48 3c 64 23   .y."."._4(ZiH<d#
        0040 - 71 53 ac f4 3b d0 78 4a-a7 0f ae 74 2d 19 96 56   qS..;.xJ...t-..V
        0050 - 06 5b 8d ec fb 03 e5 18-28 d2 49 9a ef 84 aa 08   .[......(.I.....
        0060 - e9 32 02 6a 63 aa 0e 3f-ea 74 48 70 84 8d 4f af   .2.jc..?.tHp..O.
        0070 - f4 86 a1 e0 04 6a aa 4f-8c 88 37 12 ed b0 48 2d   .....j.O..7...H-
        0080 - 3c c3 89 d2 bc 75 23 2a-81 65 c6 81 16 21 9e 37   <....u#*.e...!.7
        0090 - 2f f4 b6 ed 0c 10 c1 32-bb 0a 89 61 d9 8c aa eb   /......2...a....
        00a0 - 31 b8 dc 17 50 0d 10 ab-ec 23 d9 3b ac 52 d3 6b   1...P....#.;.R.k
        00b0 - a7 df 44 f0 16 0b a4 0b-95 02 a3 96 bc 40 b2 d8   ..D..........@..
        00c0 - e9 cd d0 1e 1c 35 d9 dc-6d 03 99 81 71 c9 3c 43   .....5..m...q.<C
        00d0 - 2f 94 9c 4e 57 14 75 94-cd ac d8 75 f1 64 05 20   /..NW.u....u.d. 
        00e0 - 17 26 5b 2a 61 cf 2d 6c-21 26 e6 bc 60 2e 1d 34   .&[*a.-l!&..`..4
        00f0 - cf d4 8d 4e b5 8a a7 e5-92 54 02 68 b3 d8 82 16   ...N.....T.h....
        0100 - 98 63 14 48 50 1c 42 db-d4 54 dd c3 c1 1b 4a 4e   .c.HP.B..T....JN
        0110 - 9a 39 48 64 51 80 a5 d4-a5 ec e3 ac d4 71 25 65   .9HdQ........q%e
        0120 - fe 65 59 26 90 b2 f8 2a-83 c4 24 9a 68 70 8e 66   .eY&...*..$.hp.f
        0130 - fd d9 a4 4d 99 e2 3f c4-78 d5 a4 f7 98 fc 44 df   ...M..?.x.....D.
        0140 - cd 33 f8 e2 0e 6e 0d 48-7b 37 a0 ed 01 47 76 e3   .3...n.H{7...Gv.
        0150 - e2 e1 88 b3 05 01 3b f0-36 bc a4 d2 dd b3 95 e8   ......;.6.......
        0160 - 0c 1f 4c 6e 60 95 de 5d-b4 13 22 e2 0b 9d 79 93   ..Ln`..].."...y.
        0170 - ac de fb 9c 79 6d 71 b1-04 82 fb ff 3a 57 0f 5f   ....ymq.....:W._
        0180 - 7b a4 56 44 0d 5d b6 c3-c4 22 cb 21 d2 fd b0 39   {.VD.]...".!...9
        0190 - cb 7b 1e 0e 23 29 8c e5-a0 ba 1b fc 5f ef df 90   .{..#)......_...
        01a0 - d9 55 66 0c 59 67 a6 8c-9d 0a cc a6 26 ae 06 b9   .Uf.Yg......&...
        01b0 - 5e c7 2f d1 88 1b 83 fd-46 e5 96 fe 29 25 ce 98   ^./.....F...)%..
        01c0 - 4a a4 b5 d9 ac 33 e5 5d-c9 fb 5d c5 f1 e7 1c de   J....3.]..].....
        01d0 - 56 23 bc c2 63 23 a0 6a-eb 73 df 66 e7 50 2d 8e   V#..c#.j.s.f.P-.
        01e0 - 3d 77 b3 ce 75 ab cc ae-b2 df bd 99 02 f0 c6 7c   =w..u..........|
        01f0 - 0b 94 7a e7 72 5d 50 0a-1c a3 45 0d 98 95 67 44   ..z.r]P...E...gD
        0200 - a3 9e c2 40 94 49 67 dd-c9 b0 29 a8 65 ac db d5   ...@.Ig...).e...
        0210 - 6b d9 3c 65 c8 40 c1 f0-ce 6e b5 a1 ec a3 6c 00   k.<e.@...n....l.
        0220 - 42 41 c8 94 cd 43 5b 1b-1d b1 fa f6 a8 93 3e 55   BA...C[.......>U
        0230 - 11 58 75 ec de 4e 5a 72-91 f9 87 04 2a 09 09 3b   .Xu..NZr....*..;
        0240 - 05 e8 c8 3f db 87 d0 75-6c 8a cf ec 18 41 bc af   ...?...ul....A..
        0250 - 35 35 96 ef 86 ad 59 76-e1 c8 9e c5 15 ef 42 57   55....Yv......BW
        0260 - ec 3b 4f 6c 17 26 96 5f-9c 43 05 a9 aa b9 d1 0f   .;Ol.&._.C......
        0270 - 5b ef 46 37 09 03 02 2f-7d f1 6b 8b f4 14 34 28   [.F7.../}.k...4(
        0280 - 19 6b e5 4a 75 29 45 45-04 22 37 71 88 5d ae 9e   .k.Ju)EE."7q.]..
        0290 - 8a d3 c5 0c ee b7 2b a3-23 2d 01 e9 e5 1f 1d a0   ......+.#-......
        02a0 - c1 4b 47 02 68 35 b8 f4-d3 01 c1 dd 6f c2 a1 31   .KG.h5......o..1
        02b0 - 2a f3 cc 38 3f 3f 40 de-30 0c d9 ac a0 8a 07 2c   *..8??@.0......,
        02c0 - c7 22 b2 b8 66 8a 38 a7-a6 c7 38 32 3c b0 e1 4d   ."..f.8...82<..M
        02d0 - 3a 81 6b d4 0a c9 52 b4-f3 23 8e 35 f4 7a c8 fb   :.k...R..#.5.z..
        02e0 - 80 3c 4f df a4 19 63 05-ca 43 3d e0 ea 50 5c b3   .<O...c..C=..P\.
        02f0 - c8 5c 17 83 1a cc 6c 06-60 56 52 9c 3a 63 68 73   .\....l.`VR.:chs
        0300 - 44 47 49 02 53 6f e9 ef-ee c6 45 9c 52 63 a5 d5   DGI.So....E.Rc..
        0310 - e9 e7 2d 64 86 2d ea 40-b8 cb 87 2e d5 38 84 50   ..-d.-.@.....8.P
        0320 - 00 e7 0b 93 30 83 1f c8-f1 f5 7b 7d 2f ad 9e 4b   ....0.....{}/..K
        0330 - 0a a0 94 c1 ee be db 58-2c 81 bc b5 c4 49 98 1a   .......X,....I..
        0340 - 18 36 db 89 8e 59 c5 ee-ed 65 1a a1 b9 b1 08 72   .6...Y...e.....r
        0350 - fd 6b 75 51 5e a3 79 6c-cc 15 e2 f0 78 d1 2a 8a   .kuQ^.yl....x.*.
        0360 - bf 89 e1 7b d7 0b 3b ba-b5 77 ca 30 3a aa 5c ee   ...{..;..w.0:.\.
        0370 - d2 9a 58 63 f4 5c f6 8e-b7 36 38 ac b9 f6 3a 27   ..Xc.\...68...:'
        0380 - cf 6a 2a fc 5f 30 da 9d-ee a5 f4 70 d0 44 2e 3f   .j*._0.....p.D.?
    
        Start Time: 1769623442
        Timeout   : 7200 (sec)
        Verify return code: 18 (self signed certificate)
        Extended master secret: no
        Max Early Data: 0
    ---
    read R BLOCK
    ---
    Post-Handshake New Session Ticket arrived:
    SSL-Session:
        Protocol  : TLSv1.3
        Cipher    : TLS_AES_256_GCM_SHA384
        Session-ID: 2D004F61EA6C33DD00EF9A485F174BD07CA59BE5271E5F61D2D0AE4C95660B1C
        Session-ID-ctx: 
        Resumption PSK: D7D2303973FEA13255EA2C72FC9EB8B1599EC2EBE7313A066C0D50AD9A015C0370A21B7634EF2C2F919BAB28FB285582
        PSK identity: None
        PSK identity hint: None
        SRP username: None
        TLS session ticket lifetime hint: 7200 (seconds)
        TLS session ticket:
        0000 - 30 5e 1f d5 53 96 1f 5c-20 0f d8 50 d6 e9 cd 15   0^..S..\ ..P....
        0010 - bf f9 82 f0 5b 36 bb 5a-96 96 be 50 21 8e 5d 0b   ....[6.Z...P!.].
        0020 - a0 7a 89 00 47 89 a6 2c-13 37 7e ba da 25 fa 7f   .z..G..,.7~..%..
        0030 - 5b 2d 5c 50 8f 0e 10 f3-2c 0e 71 a3 83 b6 43 5e   [-\P....,.q...C^
        0040 - 47 bc 14 ab 58 4f e7 0f-ba 7e 72 d3 31 11 8f 7d   G...XO...~r.1..}
        0050 - d4 64 5e 69 03 60 73 af-e3 e4 26 e7 91 81 d6 51   .d^i.`s...&....Q
        0060 - ad dc b3 15 64 d9 7e c6-0c 13 a7 9c e1 31 e2 f2   ....d.~......1..
        0070 - 7e 6d 4f f4 5c 38 de 0b-d9 8b a3 4d 03 33 da 63   ~mO.\8.....M.3.c
        0080 - 68 40 31 ec dc e9 b6 76-91 30 8d 73 98 01 a3 f4   h@1....v.0.s....
        0090 - 99 56 cf 12 90 b3 b0 72-fe a4 20 9e d7 af de 34   .V.....r.. ....4
        00a0 - a8 24 4a f1 8b 55 82 9a-ee 02 cc b2 7c fe f7 7b   .$J..U......|..{
        00b0 - 15 f1 7c f2 37 5e 02 79-24 07 13 7f cf 64 90 96   ..|.7^.y$....d..
        00c0 - 84 01 8d 68 0c f5 a8 7a-5e d1 a9 39 b4 8e 90 0b   ...h...z^..9....
        00d0 - 8c 81 e4 e6 57 57 d2 f2-6a 0e 64 cd 87 25 c5 5d   ....WW..j.d..%.]
        00e0 - 9b 65 d3 c4 25 cd 45 32-bc 89 08 22 c1 d6 46 77   .e..%.E2..."..Fw
        00f0 - bb df b3 9d c5 ac 26 c4-2c ca 07 b3 a8 32 fe 33   ......&.,....2.3
        0100 - 2f 48 dc 37 24 d7 34 8a-b7 6f 38 98 72 4b 2d 9e   /H.7$.4..o8.rK-.
        0110 - ad c1 55 9e 10 35 a1 09-26 2d f4 e5 aa 08 cf 97   ..U..5..&-......
        0120 - 05 f6 e1 4c ea 40 b2 0c-1e b7 70 62 79 e6 b3 e5   ...L.@....pby...
        0130 - 66 6a 82 d5 ed 5c 1d b4-49 b5 3a e5 19 df a6 0d   fj...\..I.:.....
        0140 - ee e2 02 cf 23 1b c9 71-3f 97 29 f2 26 25 28 92   ....#..q?.).&%(.
        0150 - 17 0f ee 3d 5c f2 9a c2-a5 40 28 ed c5 72 76 06   ...=\....@(..rv.
        0160 - 64 00 4a 73 85 27 ab 17-33 36 14 97 8f 51 8c 84   d.Js.'..36...Q..
        0170 - fd ad c6 31 7c 66 df ef-af 91 a2 35 9d 58 ab 92   ...1|f.....5.X..
        0180 - a2 6e 69 7e 14 e8 49 ae-3f a5 a2 78 12 66 e3 1e   .ni~..I.?..x.f..
        0190 - 19 35 b8 b1 b8 92 1e 95-47 70 9f 28 ef fc 7e 58   .5......Gp.(..~X
        01a0 - 36 f8 0f 0f f5 4a 1c 1b-a8 b3 ec a1 93 cc ef 21   6....J.........!
        01b0 - 46 88 a3 6b ee d9 94 7f-d6 25 36 92 8c 21 c2 35   F..k.....%6..!.5
        01c0 - af 5c 31 ea b7 81 9d 8b-6a 48 9e 5a 9d 04 14 1d   .\1.....jH.Z....
        01d0 - 2b 90 d6 ce fe 2e 09 de-42 69 21 c8 fd a7 71 70   +.......Bi!...qp
        01e0 - d0 2d 38 47 50 c9 f3 95-ba 17 18 6d a7 76 53 9f   .-8GP......m.vS.
        01f0 - 92 ce 0b 66 eb 9a 7f 4b-5c 09 a8 81 a4 67 75 f5   ...f...K\....gu.
        0200 - e1 41 b3 9f c0 5b c6 42-73 bd ae b7 62 cc ea 66   .A...[.Bs...b..f
        0210 - c9 76 23 26 97 e0 5f b5-a5 02 32 9f 55 1f 0a d4   .v#&.._...2.U...
        0220 - 87 e1 20 fe 06 e2 f9 a9-67 44 6b a8 ea a2 19 ab   .. .....gDk.....
        0230 - 2f 45 6e 3a 20 14 d4 8b-21 ab 6b 0f 84 5e d8 b0   /En: ...!.k..^..
        0240 - fc 12 12 f0 0e 6c 32 48-f5 44 78 94 c9 87 56 7f   .....l2H.Dx...V.
        0250 - aa 84 9c 62 80 e6 7b 5d-f0 20 b0 f4 88 c6 39 2d   ...b..{]. ....9-
        0260 - 55 b0 ec 7e e3 02 c9 7e-3e d9 a0 84 9b 87 f4 57   U..~...~>......W
        0270 - 31 7d 87 ed 89 27 19 ec-31 21 f8 57 be c1 1b 47   1}...'..1!.W...G
        0280 - 85 4b 96 be 29 8d 24 2c-dd 07 75 24 2b 42 eb 02   .K..).$,..u$+B..
        0290 - c3 3e 7e 56 53 76 6a da-32 23 31 8a 5a 4f b7 d5   .>~VSvj.2#1.ZO..
        02a0 - 0e 43 af cf 54 5d a3 6b-b5 8f 3b 00 7e b2 d2 5d   .C..T].k..;.~..]
        02b0 - dd c2 8e a8 ed 34 11 d5-8d 8d fb e3 aa 0f 0c fa   .....4..........
        02c0 - d0 32 87 da 64 2c 94 e0-a6 b0 eb a5 b8 18 6f da   .2..d,........o.
        02d0 - 7e a2 26 2b 5b 59 3b 4b-c5 78 d9 81 8b d0 e7 b8   ~.&+[Y;K.x......
        02e0 - b6 ba 51 82 4a 4e 0d 74-e5 41 62 27 6d dc ea b9   ..Q.JN.t.Ab'm...
        02f0 - 45 17 a6 d9 c6 d0 86 13-be 48 81 b7 2e b1 8d cb   E........H......
        0300 - d8 24 ac cb a4 24 d3 b5-c3 4c 76 22 f0 6e 4b 3a   .$...$...Lv".nK:
        0310 - ef cf d1 f8 34 8e 4c f8-9e 50 fd ae ed b4 9c c2   ....4.L..P......
        0320 - 60 f5 86 05 a9 db 15 cc-54 21 e0 a8 44 17 61 63   `.......T!..D.ac
        0330 - ac ff f0 e0 72 cf b6 da-eb ed 6f 7c 67 34 03 a9   ....r.....o|g4..
        0340 - 1b 7f 89 09 66 c0 d8 d8-e7 70 24 48 49 c7 1f f7   ....f....p$HI...
        0350 - 9e 65 29 5b 05 49 f9 e3-e9 24 56 27 f5 fc ae df   .e)[.I...$V'....
        0360 - 1c 47 69 17 33 03 de 5a-ce cf c5 fb d4 2f 85 00   .Gi.3..Z...../..
        0370 - 8d db 3d dc fb a7 4b 51-2e bb 72 dc a8 12 75 e5   ..=...KQ..r...u.
        0380 - 4b 19 cc f6 93 69 88 0a-47 b0 b6 dc b2 9b 4b 19   K....i..G.....K.
    
        Start Time: 1769623442
        Timeout   : 7200 (sec)
        Verify return code: 18 (self signed certificate)
        Extended master secret: no
        Max Early Data: 0
    ---
    read R BLOCK
    Welcome: 'Barney Rubble' is authorized.
    b3dr0ck> pass
    Password hint: d1ad7c0a3805955a35eb260dab4180dd (user = 'Barney Rubble')
    ```
    
      
    `ssh barney@<IP_Address>`  
    `cat barney.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629044563/e1dd7ceb-5df8-4870-8cec-44a34f90fd40.png align="center")
    
2. What is fred's password?
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629403555/4d054b3c-2de4-43d8-b131-b22c44a77747.png align="center")
    
      
      
    next we move to Fred trying to use the same process as Barney was bit of a back and forth but adopted part of it especially for getting the password
    
    ```bash
    sudo] password for barney: 
    Sorry, user barney is not allowed to execute '/usr/bin/cat fred.csr.pem' as root on ip-10-48-165-120.ap-south-1.compute.internal.
    barney@ip-10-48-165-120:~$ sudo certutil -a fred.csr.pem
    [sudo] password for barney: 
    Generating credentials for user: a (fredcsrpem)
    Generated: clientKey for a: /usr/share/abc/certs/a.clientKey.pem
    Generated: certificate for a: /usr/share/abc/certs/a.certificate.pem
    -----BEGIN RSA PRIVATE KEY-----
    MIIEpAIBAAKCAQEA8S5eoF7JwX3ujpfamcEMcwrKEzcP1ZdM+7sRQVSBaQlGaAG+
    Dzmr6xkKKLzcS05WWgFTif3RzrQMeL+Ze03v0KenczsiOZsXGZoNvlBS3AU3LeJy
    pjIhANVaa10gOL3ofaFVbclXvAykH8rgnUr9XOdi62wWdqInz78yBT1loZ8uV3Te
    85II/EMPxt4c+Nn1dL6aqpXYvwQTBrHMbuC7SjBsxzrQyEUOpOdmuDm2r0qJgEAB
    XuGB8FEb3md3rMfC5LIOPmDJFXwtj4xcwWScxqs3XrslUyEoiBd3Ot9SKFaaKu8A
    jGbAJ7GNmQO+CFfq1tnQxid5KlWYKDOd8PppeQIDAQABAoIBAQCiECyZytf74ylY
    PAYyZxx8eXWML+W9+utKBEv2AFjtQcRDw1f5VyGE2tCOy0mkeMFhvZ5rDVnccfQn
    d2glCRvqs527urKmzuZY18IAbm0fV4nalV3A+9JdgwA9QNiUCFifgrerDC6eOEhY
    7LS+PVO/B+TamUkG8rOZz7pG9xjB3q0NFJaBscUX25P9c2DX07zuP2ay8+OwUf4d
    1/RksY6tFtyeE1JZqNs5mzyn+wkEwBLBhSfCodv6vIH76SQqQchtbxsWFqUBZMSU
    JlJWVOeEOKDMoYlhPG/uEW4d0sr3Q3w+iic6IdrxMaQsCojP4eitK/vztDnLKD50
    cVQPcZUBAoGBAPnYGwaolOQMoNaWVzWWnWLyvRjb+v9ac4d0TuLTyrsbIErnzAPV
    mAzWJlA2iJX3DN3khErb6UvnWd6Id5BC8/YlLVFH8RMIRynosxTtPSBEtRixx8MM
    0zKipCW0d+2c0EZAUREypbgDm0+zOXbGLLOrWC97cAJQSdPYfXBKjxlxAoGBAPcf
    nzW+YOS/0oUgcJdaTdg4J09zjkQKNOplpdsC0Pefg/eSHj1Dn3UOgqQzfrvH/Pv+
    vlg8J+YQ0oT8fUGgIlrZfpK6mOWXZhzFWdpuz98Lu4RpBCMplq0ouV2ww9FDLuCh
    NL3JMppk3YhC7Kz5iPSOdEPOcR3GQfuOQDnyPIyJAoGAW2kP6k2/u/hK2HhFDBxB
    FTTOrsu9d7P2VPbe7eqjX/W6yO0qHbUGXpvzhKGPM58iHAVBycnP/69zmBHB/Qwi
    Ckrr27NApr/+mXzEGMH7/VRmFdTly51joEEVYZ43yr33Ddz2TB6Ft1/KcUyXl7dG
    H65vI2A8SPxrJpEsw+rjlGECgYAZkr4pE6B3S5p/iFHed8viej0nLbAlPmEneIln
    Vq1FUs6XShmzAVJ5DJ1cXK/awuFdOol8HA2M82S1umND7Uxg1m0QxhqYd8hrZcjc
    n+cHD/HBzBRPhzg+hx5XoblB6I/rUwNgOHfyHkoGldZkOBaCKFPDyJNQH3oojBn9
    ZSH68QKBgQDIzjaTWC2Wkon9F6f5XIZMto7PEwKD0bAWDpgaiEu/GlZFpDOpnuVD
    XgS9x6n4F+hRN3ktPakCoZUSww7LBoq6pH2gBYZePguYfoLvKFWEtySq2iigv2tq
    KJ2xpi+j3eqFa/1jMJBc6+E4gkBV5qn015qBK317aJYL+SXZYyf/1g==
    -----END RSA PRIVATE KEY-----
    -----BEGIN CERTIFICATE-----
    MIICnjCCAYYCAjA5MA0GCSqGSIb3DQEBCwUAMBQxEjAQBgNVBAMMCWxvY2FsaG9z
    dDAeFw0yNjAxMjgxODU3MTFaFw0yNjAxMjkxODU3MTFaMBUxEzARBgNVBAMMCmZy
    ZWRjc3JwZW0wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDxLl6gXsnB
    fe6Ol9qZwQxzCsoTNw/Vl0z7uxFBVIFpCUZoAb4POavrGQoovNxLTlZaAVOJ/dHO
    tAx4v5l7Te/Qp6dzOyI5mxcZmg2+UFLcBTct4nKmMiEA1VprXSA4veh9oVVtyVe8
    DKQfyuCdSv1c52LrbBZ2oifPvzIFPWWhny5XdN7zkgj8Qw/G3hz42fV0vpqqldi/
    BBMGscxu4LtKMGzHOtDIRQ6k52a4ObavSomAQAFe4YHwURveZ3esx8Lksg4+YMkV
    fC2PjFzBZJzGqzdeuyVTISiIF3c631IoVpoq7wCMZsAnsY2ZA74IV+rW2dDGJ3kq
    VZgoM53w+ml5AgMBAAEwDQYJKoZIhvcNAQELBQADggEBAANIZQ6aLdxGa0jrxADR
    w3pZpRFrnZgrQCXN63s4zFgAAYyPt5umIOa1vsWfOvymor1ExabI0vyiRsd0onNM
    RqDN37jyLYmKkHG6kSYzhiDIlGcJ+w6EEri2opTVx3tOnsHuFgcedm82QZP+tj/U
    kCgHo/X/VLVRMTsNvNat750rVOt4JkQ7RNW6Ff93Czmpz2ttSV7VIf4Mu0jRfLP6
    N5IPvLHjSMDLPz9d1mhNWssYc/bH7XnubNFXG4opEWFKQHwBRzwscZLL2kpTko2u
    IYbXmbzvPS58A4EJKslfyxBBGDyHs5FAeBs42zB7LHG65iDH9H3WrJWykAjIrYZw
    vI8=
    -----END CERTIFICATE-----
     
    ```
    
    ```bash
    nano fred.pem
    nano fredcert.pem
    cat fred.pem fredcert.pem > fred_full.pem
    openssl s_client -connect 10.48.165.120:54321 -cert fred_full.pem -key fred_full.pem
    CONNECTED(00000003)
    Can't use SSL_get_servername
    depth=0 CN = localhost
    verify error:num=18:self signed certificate
    verify return:1
    depth=0 CN = localhost
    verify return:1
    ---
    Certificate chain
     0 s:CN = localhost
       i:CN = localhost
    ---
    Server certificate
    -----BEGIN CERTIFICATE-----
    MIICrzCCAZcCFG2jt8IWGXM0a2kbUMY09M4s5YpBMA0GCSqGSIb3DQEBCwUAMBQx
    EjAQBgNVBAMMCWxvY2FsaG9zdDAeFw0yNjAxMjgxODUyMDhaFw0yNzAxMjgxODUy
    MDhaMBQxEjAQBgNVBAMMCWxvY2FsaG9zdDCCASIwDQYJKoZIhvcNAQEBBQADggEP
    ADCCAQoCggEBAK/MG6Pc4DCQnEZlul3uUM8O8MlnrCnuRKg+qcY0Xj27u5v42ySU
    googH6m/If0K1yzA/BlJx7hJjRHyX29T2Nl49NWCkp4zTEGV1/6eII2eQHnGuTln
    I+v6EiquLfuvxhG6JdAGH/gLtVWOsuymhIvIgSZO9FK0NBONXaWAbtZ9dWr9UnkK
    1J1Mwq/RnzQ4R0LDjo6fbndd/zvEgWhlGTITAeZb+vZ7/uDJOfoytX8HG9ll4ZxT
    IQlG2vko4l1PuHfr/CqfE2Uf0L67rGeIxm4SEFJU6qqGaoczVaeIZPgHpGynACFF
    EdJRbkULn5R/8FKmbPWdaa3KDfbTNnL267cCAwEAATANBgkqhkiG9w0BAQsFAAOC
    AQEAKJLK9wOBsW5QULhlJ1X8DZIj3fHYFu0/bqmJiT1shDsPuv5uqkvtcxNzUhN9
    L8Xv5jukRap9NpfotmirjvGj5GSYQGSFBGeFkBT9IWjZ1aMKZaqzM7oUdJ2fl/cW
    IltEGUBgqtIFrB3v03XnF+ygbXUlR5Ib2IECgLoulegQEKEuzFWsWZPp9aGoj32G
    C+pNelYOeYU8PBh7gpMBhxkvnQe0Lgkqt+OQn0BVFjBFhMtVKMy5kMPFfHfd5QAf
    5x4t1lmHysuzaYud2Bl3WQol89gqm0wc+owcWXl9PQndhVb5mXTn3e/dYQa48Die
    VkcNh8lIZqQjkuKxuwl13l+4mA==
    -----END CERTIFICATE-----
    subject=CN = localhost
    
    issuer=CN = localhost
    
    ---
    Acceptable client certificate CA names
    CN = localhost
    Requested Signature Algorithms: ECDSA+SHA256:ECDSA+SHA384:ECDSA+SHA512:Ed25519:Ed448:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA+SHA256:RSA+SHA384:RSA+SHA512:ECDSA+SHA224:ECDSA+SHA1:RSA+SHA224:RSA+SHA1
    Shared Requested Signature Algorithms: ECDSA+SHA256:ECDSA+SHA384:ECDSA+SHA512:Ed25519:Ed448:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA+SHA256:RSA+SHA384:RSA+SHA512
    Peer signing digest: SHA256
    Peer signature type: RSA-PSS
    Server Temp Key: X25519, 253 bits
    ---
    SSL handshake has read 1348 bytes and written 1358 bytes
    Verification error: self signed certificate
    ---
    New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
    Server public key is 2048 bit
    Secure Renegotiation IS NOT supported
    Compression: NONE
    Expansion: NONE
    No ALPN negotiated
    Early data was not sent
    Verify return code: 18 (self signed certificate)
    ---
    
    
     __     __   _     _             _____        _     _             _____        _ 
     \ \   / /  | |   | |           |  __ \      | |   | |           |  __ \      | |
      \ \_/ /_ _| |__ | |__   __ _  | |  | | __ _| |__ | |__   __ _  | |  | | ___ | |
       \   / _` | '_ \| '_ \ / _` | | |  | |/ _` | '_ \| '_ \ / _` | | |  | |/ _ \| |
        | | (_| | |_) | |_) | (_| | | |__| | (_| | |_) | |_) | (_| | | |__| | (_) |_|
        |_|\__,_|_.__/|_.__/ \__,_| |_____/ \__,_|_.__/|_.__/ \__,_| |_____/ \___/(_)
                                                                                     
                                                                                     
    
    ---
    Post-Handshake New Session Ticket arrived:
    SSL-Session:
        Protocol  : TLSv1.3
        Cipher    : TLS_AES_256_GCM_SHA384
        Session-ID: 6411A1034D7859EE91E58FFF82DCF15D806763DE1A4C2333A44C02E91CE97940
        Session-ID-ctx: 
        Resumption PSK: F03DDABB79EBD7614EE7CE437DD3C8D431746144F8D44FE92CDFE786E558E6430E3D54C840778087F8A4C82E0349BAFF
        PSK identity: None
        PSK identity hint: None
        SRP username: None
        TLS session ticket lifetime hint: 7200 (seconds)
        TLS session ticket:
        0000 - 61 90 d8 09 d1 3f 2f 36-89 90 ac 18 dc 96 47 ca   a....?/6......G.
        0010 - 8f e4 15 a4 94 fe c7 1d-65 55 70 e4 c4 e5 1e 80   ........eUp.....
        0020 - e9 43 e2 05 9b b4 9a b1-b6 fc 9d 7c 4f cd 96 94   .C.........|O...
        0030 - c2 0c 83 b3 2b 4c 4f 35-c9 6f 0d 4d 68 1e b1 a0   ....+LO5.o.Mh...
        0040 - df 92 43 2d 33 a1 c0 bb-fa 7b 1c 9f 44 c8 08 e9   ..C-3....{..D...
        0050 - b7 b4 1e 5e 84 a0 23 97-77 4e e9 8d 04 05 38 88   ...^..#.wN....8.
        0060 - e5 ad e2 19 c7 c4 6b b3-d5 74 b8 1e 78 5e f0 44   ......k..t..x^.D
        0070 - a6 2f d8 43 df 29 86 62-a6 d9 3a 08 39 1c 79 4d   ./.C.).b..:.9.yM
        0080 - a2 94 00 fa fb 35 c7 e9-6e 6c b5 81 02 91 76 bf   .....5..nl....v.
        0090 - 84 ac 45 a6 39 40 32 bc-c0 86 72 d2 3e 45 f6 3c   ..E.9@2...r.>E.<
        00a0 - 1e 84 30 aa b8 6a 43 f5-00 39 a4 35 b9 47 95 37   ..0..jC..9.5.G.7
        00b0 - 69 40 08 54 74 90 de 4f-a3 a0 22 76 66 ec 76 18   i@.Tt..O.."vf.v.
        00c0 - 48 5a 10 2c c4 c5 9d 76-31 56 c5 37 52 a7 e6 89   HZ.,...v1V.7R...
        00d0 - 14 d4 12 54 10 43 f5 27-14 55 07 b0 2d 64 87 8b   ...T.C.'.U..-d..
        00e0 - 84 29 f8 bc 28 e0 16 0b-dd 40 31 40 32 39 66 2c   .)..(....@1@29f,
        00f0 - 59 d7 e0 8a 68 e0 aa 05-36 a3 92 13 f7 b1 4d e7   Y...h...6.....M.
        0100 - 1a 92 35 13 36 ee f5 60-cd 72 34 5d 3e e5 a1 e1   ..5.6..`.r4]>...
        0110 - 0b e2 e6 31 45 64 25 57-45 c0 90 4d da 19 3b 16   ...1Ed%WE..M..;.
        0120 - 12 7e 74 41 a1 c6 57 9b-c7 c5 a7 9e c7 0a 25 0e   .~tA..W.......%.
        0130 - 4d 44 88 3e 1b ff 03 88-5f bd f8 20 b0 e5 b4 8b   MD.>...._.. ....
        0140 - 5e 40 3e 7e 45 cc 25 5e-5b cf 6a 7a f5 aa 99 53   ^@>~E.%^[.jz...S
        0150 - ec 23 86 66 85 44 fa 1f-6b 5a 63 e7 18 c9 da 1a   .#.f.D..kZc.....
        0160 - 8b 9d 75 d5 84 5f 5f 21-4a f5 e5 1e 5e a1 00 2e   ..u..__!J...^...
        0170 - 32 b6 a9 7a 09 36 b5 f2-2e 80 79 ec 6b bf 60 19   2..z.6....y.k.`.
        0180 - 35 f8 40 4f ac 81 14 f9-ff fc 8c 35 ca 75 dd 16   5.@O.......5.u..
        0190 - bb 82 8e f7 db 69 1d fd-dc 1d e4 14 1a 41 16 5d   .....i.......A.]
        01a0 - b1 85 66 e9 78 17 89 7e-4a d6 9e 45 70 a6 94 10   ..f.x..~J..Ep...
        01b0 - f5 0d cd 24 c9 1b 74 6a-02 ab 36 3d 1a 52 4d b9   ...$..tj..6=.RM.
        01c0 - 47 da ad 93 ee 09 a1 82-34 f6 9c 6d cd f0 84 cd   G.......4..m....
        01d0 - ed e8 9f 87 96 1a 1c 3a-1f 3b 19 3c 72 e0 80 22   .......:.;.<r.."
        01e0 - a8 5c 1e bc 5c 19 52 b0-03 1f 5b 97 3c 5a 08 bf   .\..\.R...[.<Z..
        01f0 - 38 b3 3b 26 49 bc ab 56-8e cb a3 73 4a d3 b5 4e   8.;&I..V...sJ..N
        0200 - a9 6f c3 e1 e5 2b d7 ba-fe a6 d6 03 29 9a ed 57   .o...+......)..W
        0210 - c0 fb 29 90 bf 84 de b1-f0 95 26 53 47 f4 94 3b   ..).......&SG..;
        0220 - b0 50 9e 6b c4 3d 50 75-51 73 fd bf 14 88 aa e5   .P.k.=PuQs......
        0230 - 97 08 8e 52 b2 d2 0e cf-1b 43 61 0d 70 1e 4b 31   ...R.....Ca.p.K1
        0240 - 19 9e eb cd fa e8 d9 14-d3 42 d5 08 31 ac 07 e8   .........B..1...
        0250 - 06 72 ac 14 de 44 a1 8b-aa fd 95 51 41 a0 5f 7f   .r...D.....QA._.
        0260 - d5 b2 a4 23 1c bf cf fd-b5 00 25 28 5a 91 a3 22   ...#......%(Z.."
        0270 - c6 b5 25 7b f4 10 d6 5f-ea c0 cb 89 89 3b 43 dc   ..%{..._.....;C.
        0280 - f2 2a 3f 22 8c c2 07 42-df de 2e e5 07 39 65 e6   .*?"...B.....9e.
        0290 - 55 e5 37 bf 67 a0 18 87-72 d8 af 9e 64 31 d0 fb   U.7.g...r...d1..
        02a0 - fb 9f 5d 0d d2 8a f9 b5-b9 35 f0 36 e3 5f f7 71   ..]......5.6._.q
        02b0 - e9 c4 33 eb a6 52 a9 93-f9 65 d7 d6 cc 17 b1 fe   ..3..R...e......
        02c0 - 21 c0 39 4a 81 d5 78 5a-1f fb 62 ac 76 10 fa 0b   !.9J..xZ..b.v...
        02d0 - f5 82 52 eb 4b 5d 68 e0-ef ab 53 40 91 16 70 d2   ..R.K]h...S@..p.
        02e0 - 19 09 c1 d2 73 58 36 a8-3a 6c 3e 9c c6 16 b4 7f   ....sX6.:l>.....
        02f0 - 79 3a 6b 81 df 5a cd 1f-a4 82 56 85 72 e4 b2 ea   y:k..Z....V.r...
        0300 - 0b 53 24 4e f8 4c 9c eb-fa 02 89 53 c7 1c 63 8f   .S$N.L.....S..c.
        0310 - b3 94 cd 98 be d5 2b 2d-90 6f e4 51 18 39 a2 5f   ......+-.o.Q.9._
        0320 - f2 9e 17 ff d3 a8 ca 74-bb 82 ce 6f 8c c4 4b 6b   .......t...o..Kk
        0330 - 33 dd 4d 11 d0 d9 11 09-cc b9 e6 e5 ca 8b f6 78   3.M............x
        0340 - 4f 2f fc 3d 12 f1 d4 de-4e ce 80 8e f9 50 aa 4f   O/.=....N....P.O
        0350 - 0a eb b9 77 00 90 b4 c7-ed 5a 72 cf 0b f2 ca c2   ...w.....Zr.....
        0360 - b3 86 b0 be 7d 88 1e 26-3a c4 c7 b5 95 4f 5c f7   ....}..&:....O\.
        0370 - 52 89 a9 c8 80 e1 f1 e7-2b d5 cb bc 94 08 db 11   R.......+.......
        0380 - 61 ba f6 b0 33 34 dd d1-97 b4 7d b8 5b 11 33 d4   a...34....}.[.3.
    
        Start Time: 1769627121
        Timeout   : 7200 (sec)
        Verify return code: 18 (self signed certificate)
        Extended master secret: no
        Max Early Data: 0
    ---
    read R BLOCK
    ---
    Post-Handshake New Session Ticket arrived:
    SSL-Session:
        Protocol  : TLSv1.3
        Cipher    : TLS_AES_256_GCM_SHA384
        Session-ID: C1896E95CDB662E55E1143A03212412437580CA82649179F7C5A734AA078FD51
        Session-ID-ctx: 
        Resumption PSK: E6FC8917BF340F8F4B456D06F35EAAA5A24B32DCABB31FF42FAB7049C91DDD1BAF697926398073FF999B7285E70A4220
        PSK identity: None
        PSK identity hint: None
        SRP username: None
        TLS session ticket lifetime hint: 7200 (seconds)
        TLS session ticket:
        0000 - 61 90 d8 09 d1 3f 2f 36-89 90 ac 18 dc 96 47 ca   a....?/6......G.
        0010 - cc 3d 5c 98 17 df 23 86-8d f4 f9 60 0c bf ad 63   .=\...#....`...c
        0020 - 3c ba 84 e1 e4 2d 16 6c-8f 40 32 5c d2 ee c4 dc   <....-.l.@2\....
        0030 - cb 67 f0 48 3a d3 04 10-0a 68 94 72 eb 7d 05 99   .g.H:....h.r.}..
        0040 - b7 a5 ea 9e 07 5f bc 88-c2 db 6d ac 14 a1 07 5f   ....._....m...._
        0050 - fe 22 fd 75 3b bb db 29-5b e0 ad 17 d2 07 4e 71   .".u;..)[.....Nq
        0060 - 0d da 4a 65 88 08 04 da-56 dc 80 a0 01 19 eb 36   ..Je....V......6
        0070 - d8 42 39 82 1d 13 60 7d-88 08 3b b1 49 b5 a0 93   .B9...`}..;.I...
        0080 - 9d d0 42 58 96 fe d9 0c-de 71 ef bd f0 4b 92 6c   ..BX.....q...K.l
        0090 - 45 6f d8 33 43 7f 4d 6f-a2 46 f7 85 fb 6b 2b 11   Eo.3C.Mo.F...k+.
        00a0 - ff 44 ac f0 6b 90 ff e2-b6 69 73 3d 46 f7 2e ef   .D..k....is=F...
        00b0 - d7 db 42 f1 02 37 7e 6f-a2 e6 9f 0a 62 b7 ce d8   ..B..7~o....b...
        00c0 - 0f 80 c4 bd 5d 5e 11 92-e9 18 de b1 a8 be f0 87   ....]^..........
        00d0 - b2 12 07 14 58 82 b9 a0-0c e8 38 1e 64 01 bf 25   ....X.....8.d..%
        00e0 - db 3a 62 c4 66 ff e1 f8-10 19 d0 41 81 8c 7a 45   .:b.f......A..zE
        00f0 - 57 59 6a 38 f4 d9 c8 76-1a 77 ee 66 f8 a1 b6 f0   WYj8...v.w.f....
        0100 - e1 a6 47 2c 4d b4 9d 7a-bd 4a 83 ca 89 4b 24 b6   ..G,M..z.J...K$.
        0110 - d9 c3 c5 9b f6 14 86 36-0d 17 fa 32 6b d2 cf 0e   .......6...2k...
        0120 - c0 ed 8b 32 fc 3e da d9-e8 4b 3c 0c 5c ad 65 30   ...2.>...K<.\.e0
        0130 - ee 0f 0a d4 eb 74 fd 6c-67 f5 37 10 49 e8 d4 ed   .....t.lg.7.I...
        0140 - 96 ee 8e bf a5 87 10 0a-b6 4d e8 7a da 98 79 30   .........M.z..y0
        0150 - 60 ec be 0f b5 9a 06 2f-eb 9e e3 7a 20 b2 bd 72   `....../...z ..r
        0160 - 4b 20 54 0e 1f 56 4c b7-5a a4 16 50 b0 32 13 cd   K T..VL.Z..P.2..
        0170 - fc e2 5c 39 b5 e7 9f af-87 35 3e f3 c2 ee dc 81   ..\9.....5>.....
        0180 - 64 69 15 72 d9 03 ae 6f-52 ce d0 b0 d7 69 0a e0   di.r...oR....i..
        0190 - 8a db 37 1a f7 d2 cd b9-b0 1c 30 18 9a 79 e6 cc   ..7.......0..y..
        01a0 - 53 45 93 b3 a8 96 fa 6a-0e 2b df 8b 91 8f b4 cb   SE.....j.+......
        01b0 - 63 fc fa 48 83 39 21 b9-0e a8 d2 89 b2 54 61 a6   c..H.9!......Ta.
        01c0 - b7 5d cf 22 7f 52 93 a1-14 6d 5e 8e 32 f8 58 80   .].".R...m^.2.X.
        01d0 - 13 52 8f 90 18 cc da 07-1b 56 15 6e 83 34 0c e3   .R.......V.n.4..
        01e0 - 42 cf 28 aa 7e 36 c2 81-ab ca c5 30 74 5c dd 14   B.(.~6.....0t\..
        01f0 - 9b 40 46 ac ef 2d f8 2d-8f bf c3 c2 ef 3a 21 bd   .@F..-.-.....:!.
        0200 - 4c 84 96 f0 0b 1d 37 6b-10 f7 d7 c5 e5 e9 71 5c   L.....7k......q\
        0210 - 74 67 dd 4d 29 7f 69 e5-61 fc 69 53 0c 5a ce 89   tg.M).i.a.iS.Z..
        0220 - 41 eb 35 00 f2 09 21 b7-d6 e4 f7 9c aa 93 b3 c2   A.5...!.........
        0230 - 73 86 0d 67 b2 50 d6 14-22 5d d0 35 b1 1f e3 b1   s..g.P.."].5....
        0240 - da fd b6 08 38 65 8e 07-d5 e4 d1 0b 24 59 8f 2c   ....8e......$Y.,
        0250 - 5f 48 21 21 e3 b6 62 a9-88 42 c6 93 17 e2 e5 c4   _H!!..b..B......
        0260 - 82 88 85 f8 90 bb d0 67-b6 04 1d 44 46 cc f1 88   .......g...DF...
        0270 - c5 f3 31 31 9e b3 d3 4d-cc d6 0d 65 4c 7a 6f c3   ..11...M...eLzo.
        0280 - d6 de 34 be 5f f2 58 93-cf 0f ce 0d b5 99 d1 0c   ..4._.X.........
        0290 - 25 7b d6 f0 f7 c2 ea 3d-f3 08 f6 ce 58 a0 13 2b   %{.....=....X..+
        02a0 - 5a 6c 81 a2 e1 6b 9f 2a-66 37 7c 3d 64 eb c9 18   Zl...k.*f7|=d...
        02b0 - fc 77 bd 00 6a c8 a1 35-ec a0 78 06 3d 97 0d 3e   .w..j..5..x.=..>
        02c0 - ba f7 4b de a5 ff 58 0c-e8 1b 0a b4 45 7a 3e ef   ..K...X.....Ez>.
        02d0 - 97 2b 28 cf d3 af 64 7e-7d b6 e4 98 f2 dd c7 cc   .+(...d~}.......
        02e0 - fb 35 a2 1d c3 c3 d3 ef-86 f7 63 a7 7e 0f 73 33   .5........c.~.s3
        02f0 - bf 14 72 e6 e2 aa 10 22-24 22 55 00 df d6 f6 2e   ..r...."$"U.....
        0300 - 4d 5a 90 d4 77 e9 28 d1-ee 52 9b 2b 16 b4 25 0a   MZ..w.(..R.+..%.
        0310 - 7d cb 3f f3 5d 76 73 c6-b1 90 d5 0c 54 cd 45 e4   }.?.]vs.....T.E.
        0320 - 04 ed 0d 6c 09 85 db 7d-28 31 07 ad cf d6 52 a0   ...l...}(1....R.
        0330 - 93 b5 f9 f8 e7 50 4f f5-f7 b4 a3 d7 d3 5a 36 b0   .....PO......Z6.
        0340 - ef ea 4d dc c3 28 42 a8-bd a7 14 ef f1 84 6b 34   ..M..(B.......k4
        0350 - 90 ad 61 4b 87 68 35 05-42 12 cc 26 2e 81 ff 1e   ..aK.h5.B..&....
        0360 - 83 ee 20 8e 22 ce 04 9e-27 d7 04 e1 04 df 75 3b   .. ."...'.....u;
        0370 - 36 59 bf 0c 31 28 f4 81-8c 32 89 07 a5 e7 7c 16   6Y..1(...2....|.
        0380 - ee da 27 69 e0 b0 4b 39-02 af 3a 92 d2 53 75 b8   ..'i..K9..:..Su.
    
        Start Time: 1769627121
        Timeout   : 7200 (sec)
        Verify return code: 18 (self signed certificate)
        Extended master secret: no
        Max Early Data: 0
    ---
    read R BLOCK
    Welcome: 'fredcsrpem' is authorized.
    b3dr0ck> pass
    Password hint: YabbaDabbaD0000! (user = 'fredcsrpem')
    b3dr0ck> 
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629436680/8f1a861c-5ab3-4dec-9109-a46b5e45571d.png align="center")
    
3. What is the fred.txt flag?  
      
    `ssh fred@<IP_Address>`  
    `cat fred.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629276539/1d80cad3-8d54-468b-a9eb-5add219a435a.png align="center")
    
      
    
4. What is the root.txt flag?  
      
    `sudo -l`
    
    ```bash
    sudo -l
    Matching Defaults entries for fred on ip-10-48-165-120:
        insults, env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
    User fred may run the following commands on ip-10-48-165-120:
        (ALL : ALL) NOPASSWD: /usr/bin/base32 /root/pass.txt
        (ALL : ALL) NOPASSWD: /usr/bin/base64 /root/pass.txt
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629465410/48efd46a-fe1e-41c8-a102-c12cba309771.png align="center")
    
    checked hints on [GTFOBins](https://gtfobins.org/gtfobins/base32/#file-read) for base64 and base32
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629513140/fbcfc47c-b0db-4746-8a58-8f21e0e1a137.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629535732/0585efb1-bef5-4700-9735-3cf3a6551a7c.png align="center")

Based on my output on the screenshot, it was much trial and error and trying to involve Claude, but the password didn’t work. I had to check another [b3dr0ck writeup](https://medium.com/@RosanaFS/b3dr0ck-privilege-escalation-tryhackme-walkthrough-120-points-af8effe16b2f) for hints and found it helpful that they opted to decode the base64, then move to CyberChef to decode from Base64 → From Base32 → From Base64, then used crackstation to crack the hash to find the right password

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629813017/d6e30326-d3cf-4ce6-b63e-75bb9de40cbc.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629824277/59a77af3-e6ee-4dac-83fa-bb9b97d35955.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769629849820/32462a83-39a9-4610-aa69-016c68c04eca.png align="center")

`su -`

`find / -type f -name root.txt 2> /dev/null`

## Conclusion

**Summary and Key Takeaways**

The b3dr0ck challenge provided an excellent introduction to certificate-based authentication and multi-stage privilege escalation. The challenge required a methodical approach, progressing through three distinct privilege levels: initial access as Barney, lateral movement to Fred, and finally escalation to root.

**Attack Chain Summary:**

1. **Reconnaissance**: Identified five open ports through nmap scanning, with particular focus on the certificate recovery service (port 9009) and authentication service (port 54321)
    
2. **Initial Access**: Retrieved Barney's client certificate and private key from port 9009, then authenticated to port 54321 to obtain his SSH password ("b3dr0ck")
    
3. **Lateral Movement**: Repeated the certificate retrieval process for Fred, though encountered initial difficulties with certificate generation vs. retrieval
    
4. **Privilege Escalation**: Exploited Fred's sudo permissions on base32/base64 commands to read /root/pass.txt, then decoded multiple encoding layers (base64 → base32 → base64 → MD5 hash) to obtain the root password
    

**Key Lessons Learned:**

* **Certificate Management**: Understanding how client certificates work for mutual TLS authentication and the security implications of exposing certificate recovery services
    
* **Service Enumeration**: The importance of thoroughly exploring custom services (like port 9009) to understand their functionality
    
* **Encoding Chains**: Real-world credentials are often protected by multiple layers of encoding and hashing, requiring tools like CyberChef for efficient decoding
    
* **Sudo Misconfigurations**: GTFOBins remains an invaluable resource for identifying privilege escalation vectors through misconfigured sudo permissions
    
* **Persistence and Methodology**: When stuck (like with Fred's certificate), sometimes stepping back and following the proven methodology from earlier steps (Barney's approach) leads to success
    

**Tools and Techniques Used:**

* nmap for service discovery
    
* netcat for interacting with custom services
    
* openssl for certificate validation and SSL connections
    
* CyberChef for multi-layer decoding
    
* CrackStation for hash cracking
    
* GTFOBins for privilege escalation research
    

This challenge reinforced the importance of systematic enumeration, proper tooling, and understanding the fundamentals of cryptographic authentication. The Flintstones theme made it approachable while still teaching valuable security concepts applicable to real-world penetration testing scenarios.

## Reflection: The Human Element

**The Relatable Developer**

One of the most compelling aspects of the b3dr0ck challenge is how realistic and relatable the developer character (Barney) is. His struggles mirror what many of us experience in real development scenarios:

* **"I only figured out nginx so far, what the h3ll is a database?!?"** - We've all been there, learning one technology while feeling overwhelmed by the next
    
* **Trying to implement security features (TLS certificates) but not fully understanding them** - A common scenario where good intentions meet incomplete knowledge
    
* **Leaving debug/recovery services running in production** - The certificate recovery service on port 9009 was likely meant as a helpful tool during development but was never disabled
    
* **"Security through obscurity" with multiple encoding layers** - The base64→base32→base64→hash chain shows someone trying to protect sensitive data but not using proper security practices
    
* **Sudo misconfigurations** - Giving broad permissions to make troubleshooting easier, without considering the security implications
    

**Why This Matters:**

This challenge isn't just about exploiting vulnerabilities—it's a mirror showing us mistakes we might make ourselves. Barney isn't incompetent; he's learning, struggling with complexity, and making the same shortcuts and assumptions many developers make under pressure or when learning new technologies.

The takeaway isn't "look how dumb this developer is," but rather "here's what can go wrong when security isn't part of the design from the start." It's a reminder to:

* Fully understand security tools before deploying them
    
* Remove debug/helper services before going to production
    
* Follow the principle of least privilege with sudo permissions
    
* Use proper secrets management instead of encoding layers
    
* Ask for help or security reviews when implementing authentication
    

The b3dr0ck challenge succeeds because it's not just teaching us penetration testing—it's teaching us to be better, more security-conscious developers.