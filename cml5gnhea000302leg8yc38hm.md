---
title: "Intermediate Nmap"
datePublished: Mon Feb 02 2026 17:45:39 GMT+0000 (Coordinated Universal Time)
cuid: cml5gnhea000302leg8yc38hm
slug: intermediate-nmap
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1770053898574/5463c311-2799-4d44-9db2-3f0d6e4eb5b5.png
tags: ssh, nmap, ctf, tryhackme

---

This TryHackMe challenge demonstrates a critical security concept: information disclosure through misconfigured services. The room focuses on fundamental reconnaissance skills using nmap to discover open ports and services, but with an interesting twist—sometimes sensitive information is inadvertently exposed in service banners. This simple yet effective scenario highlights why proper security configuration is essential and shows how attackers can leverage basic enumeration tools to gain unauthorized access. Let's walk through the methodology of discovering and exploiting this misconfiguration.

You've learned some great `nmap` skills! Now can you combine that with other skills with `netcat` and protocols, to log in to this machine and find the flag? This VM MACHINE\_IP is listening on a high port, and if you connect to it it may give you some information you can use to connect to a lower port commonly used for remote access!

**Access this challenge** by deploying both the vulnerable machine by pressing the green "Start Machine" button located within this task, and the TryHackMe AttackBox by pressing the  "Start AttackBox" button located at the top-right of the page.

Use the AttackBox to scan the target: **MACHINE\_IP**

Check out similar content on TryHackMe:

* [Nmap Module](https://tryhackme.com/module/nmap)
    

### Answer the questions below

1. Find the flag!
    

```bash
nmap -p- -sV -sC 10.48.167.156
Starting Nmap 7.80 ( https://nmap.org ) at 2026-02-02 17:30 GMT
mass_dns: warning: Unable to open /etc/resolv.conf. Try using --system-dns or specify valid servers with --dns-servers
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 10.48.167.156
Host is up (0.00027s latency).
Not shown: 65532 closed ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
2222/tcp  open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
31337/tcp open  Elite?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, Kerberos, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, NULL, RPCCheck, RTSPRequest, SIPOptions, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServer, TerminalServerCookie, X11Probe: 
|     In case I forget - user:pass
|_    ubuntu:Dafdas!!/str0ng
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31337-TCP:V=7.80%I=7%D=2/2%Time=6980DF33%P=x86_64-pc-linux-gnu%r(NU
SF:LL,35,"In\x20case\x20I\x20forget\x20-\x20user:pass\nubuntu:Dafdas!!/str
SF:0ng\n\n")%r(GetRequest,35,"In\x20case\x20I\x20forget\x20-\x20user:pass\
SF:nubuntu:Dafdas!!/str0ng\n\n")%r(SIPOptions,35,"In\x20case\x20I\x20forge
SF:t\x20-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(GenericLines,35,"In
SF:\x20case\x20I\x20forget\x20-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")
SF:%r(HTTPOptions,35,"In\x20case\x20I\x20forget\x20-\x20user:pass\nubuntu:
SF:Dafdas!!/str0ng\n\n")%r(RTSPRequest,35,"In\x20case\x20I\x20forget\x20-\
SF:x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(RPCCheck,35,"In\x20case\x2
SF:0I\x20forget\x20-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(DNSVersi
SF:onBindReqTCP,35,"In\x20case\x20I\x20forget\x20-\x20user:pass\nubuntu:Da
SF:fdas!!/str0ng\n\n")%r(DNSStatusRequestTCP,35,"In\x20case\x20I\x20forget
SF:\x20-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(Help,35,"In\x20case\
SF:x20I\x20forget\x20-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(SSLSes
SF:sionReq,35,"In\x20case\x20I\x20forget\x20-\x20user:pass\nubuntu:Dafdas!
SF:!/str0ng\n\n")%r(TerminalServerCookie,35,"In\x20case\x20I\x20forget\x20
SF:-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(TLSSessionReq,35,"In\x20
SF:case\x20I\x20forget\x20-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(K
SF:erberos,35,"In\x20case\x20I\x20forget\x20-\x20user:pass\nubuntu:Dafdas!
SF:!/str0ng\n\n")%r(SMBProgNeg,35,"In\x20case\x20I\x20forget\x20-\x20user:
SF:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(X11Probe,35,"In\x20case\x20I\x20fo
SF:rget\x20-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(FourOhFourReques
SF:t,35,"In\x20case\x20I\x20forget\x20-\x20user:pass\nubuntu:Dafdas!!/str0
SF:ng\n\n")%r(LPDString,35,"In\x20case\x20I\x20forget\x20-\x20user:pass\nu
SF:buntu:Dafdas!!/str0ng\n\n")%r(LDAPSearchReq,35,"In\x20case\x20I\x20forg
SF:et\x20-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")%r(LDAPBindReq,35,"In
SF:\x20case\x20I\x20forget\x20-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n")
SF:%r(LANDesk-RC,35,"In\x20case\x20I\x20forget\x20-\x20user:pass\nubuntu:D
SF:afdas!!/str0ng\n\n")%r(TerminalServer,35,"In\x20case\x20I\x20forget\x20
SF:-\x20user:pass\nubuntu:Dafdas!!/str0ng\n\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3.51 seconds
```

nmap reveals the user and their password

```bash
$ ssh ubuntu@IP_Address
$ cat /home/user/flag.txt 
```

  

## Conclusion

This challenge effectively demonstrates how seemingly harmless information disclosure can lead to complete system compromise. The key takeaway is that **service banners should never contain sensitive information** like usernames and passwords. While this was a simple room, it illustrates an important real-world vulnerability—improper configuration and information leakage.

**Key Learning Points:**

* Comprehensive port scanning with nmap (`-p-` for all ports, `-sV` for version detection, `-sC` for default scripts) is essential for discovering all attack vectors
    
* High-numbered ports (like 31337) can host unexpected services that may expose sensitive data
    
* Service fingerprinting can reveal critical information beyond just version numbers
    
* Information disclosure vulnerabilities, while passive, can provide the keys to the kingdom
    

This room reinforces fundamental reconnaissance skills while teaching an important security principle: **never expose credentials in service banners, debug messages, or error outputs**. Even the simplest misconfiguration can turn a secure system into an easy target.

---

**Flag:** *(Retrieved from /home/user/flag.txt after SSH access)*