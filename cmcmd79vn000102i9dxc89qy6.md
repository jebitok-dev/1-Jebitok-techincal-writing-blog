---
title: "Challenges: Takeover (TryHackMe)"
datePublished: Wed Jul 02 2025 19:43:48 GMT+0000 (Coordinated Universal Time)
cuid: cmcmd79vn000102i9dxc89qy6
slug: challenges-takeover-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751485327703/39699f6d-d593-4110-9500-7dddee03fb1a.png
tags: tryhackme, ctf-writeup, subdomain-takeover, fluf, subdomain-enumeration

---

In this Capture The Flag (CTF) challenge titled [**Takeover**](https://tryhackme.com/room/takeover), we are tasked with assisting Futurevera, a fictional space research company, whose online infrastructure might be at risk of being compromised. The company reports that blackhat hackers have claimed they can take over parts of the site, demanding a ransom. Our goal is to investigate potential weaknesses by performing **subdomain enumeration** to uncover hidden or forgotten services that could be exploited.

To begin, we map the domain `futurevera.thm` to the provided IP address via `/etc/hosts`. We then perform reconnaissance using tools like `nmap` to identify open ports and services, and `ffuf` to brute-force potential subdomains. Our focus is on enumeration — a key step in ethical hacking and CTFs, which helps us discover hidden domains that may reveal the flag or sensitive access points.

## Help Us

Start Machine

Hello there,  
  
I am the CEO and one of the co-founders of futurevera.thm. In Futurevera, we believe that the future is in space. We do a lot of space research and write blogs about it. We used to help students with space questions, but we are rebuilding our support.

Recently blackhat hackers approached us saying they could takeover and are asking us for a big ransom. Please help us to find what they can takeover.  
  
Our website is located at [https://futurevera.thm](https://futurevera.thm/)

Hint: Don't forget to add the 10.10.209.47 in /etc/hosts for futurevera.thm ; )

### Answer the questions below

1. What's the value of the flag? `flag{beea0d6edfcee06a59b83fb50ae81b2f}`  
      
    `nano /etc/hosts`  
    add `<IP_Address> futurevera.thm`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751484836777/7211554c-5070-40d1-9c0d-4f233de6a242.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751484996694/bcd12295-6d8b-49ee-bfe7-12eb56f7aaa0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751484859085/0f9a77af-0c9d-479c-88f7-8b0773230d97.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751485013423/1d0c367b-de61-40b9-b018-56731efd2866.png align="center")

checking `nmap -sC -sV <IP_Address>`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751483338595/faec02f6-581f-4116-8c88-2961e9be82a5.png align="center")

```bash
# Ports Open:

22/tcp → SSH (OpenSSH 8.2p1)

80/tcp → HTTP (Apache 2.4.41)

443/tcp → HTTPS (Apache 2.4.41)
```

We're looking for a **flag** by **enumerating the subdomains** of `futurevera.thm`.

  
We’ll use `ffuf` to enumerate in order to actively discover names associated with the domain (`futurevera.thm`) that might reveal our flag

`ffuf -c -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.futurevera.thm" -u https://futurevera.thm -fw 1`

`ffuf -c -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt \ -H "Host: FUZZ.futurevera.thm" -u https://futurevera.thm \ -fs 1234 -fw 10 -fl 14`  
  
  
(some of the output)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751484700458/33aae5d2-5273-45f1-b462-ccf5d00aaec1.png align="center")

### 🧠 Why is enumeration important in CTFs and pentesting?

Because subdomains:

* **Might be forgotten** or misconfigured by developers
    
* **Can expose admin panels**, test environments, APIs, etc.
    
* **May contain vulnerabilities** that the main site doesn’t
    

### 🛠️ Types of Subdomain Enumeration

| Method | Description |
| --- | --- |
| **Brute Force (Wordlist)** | Try known words against a domain (e.g., `admin`, `test`) |
| **Passive (OSINT)** | Use public data, like VirusTotal, [crt.sh](http://crt.sh), search engines |
| **Certificate Analysis** | Look at SSL certs for embedded subdomain names |
| **DNS Records** | Query DNS for known names (like with `dig` or `dnsrecon`) |
| **VHost Fuzzing** | Try subdomains in `Host:` headers with tools like `ffuf` |

adding `support.futurevera.thm` to the `/etc/hosts`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751485038196/9e74c8f3-6a2c-453d-b186-0554285748e4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751485062349/ec9201d0-215d-4c62-a173-8ef78660bc39.png align="center")

replace the /etc/hosts changes with this one:

`<IP_ADDRESS> secrethelpdesk934752.support.futurevera.thm`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751485164369/1d2aa2a0-6b16-4a63-b2f0-fcb38fbbb836.png align="center")

This challenge highlights the importance of subdomain enumeration in real-world cybersecurity testing. Misconfigured or forgotten subdomains often become the weakest link in otherwise secure infrastructures. By using tools like `ffuf` and analyzing SSL certificates, we were able to simulate how attackers might uncover hidden assets, reinforcing why continuous monitoring and cleanup of DNS entries are essential for organizations.