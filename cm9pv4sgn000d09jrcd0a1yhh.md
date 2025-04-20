---
title: "Network Security: Net Sec Challenge (TryHackMe)"
datePublished: Sun Apr 20 2025 16:29:57 GMT+0000 (Coordinated Universal Time)
cuid: cm9pv4sgn000d09jrcd0a1yhh
slug: network-security-net-sec-challenge-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1745166393135/db984bdc-b4be-40a8-be66-4ca6e52e80b5.png
tags: nmap, network-security, tryhackme, write-up, gobuster

---

This article will cover the [Net Sec Challenge](https://tryhackme.com/room/netsecchallenge) write-up under the **Jr Penetration Tester** on THM.

## Introduction

Use this challenge to test your mastery of the skills you have acquired in the Network Security module. All the questions in this challenge can be solved using only `nmap`, `telnet`, and `hydra`.

### Answer the questions below

Launch the AttackBox and the target VM.

## Challenge Questions

### You can answer the following questions using Nmap, Telnet, and Hydra.

1. What is the highest port number being open less than 10,000? `8080`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745164854191/92da9fc7-0600-44c3-abb6-e180acfb68b6.png align="center")
    
2. There is an open port outside the common 1000 ports; it is above 10,000. What is it? `10021`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745164889414/8f0b9b53-7c25-4507-b707-b6a064948bc1.png align="center")
    
3. How many TCP ports are open? `6`
    
    count the TCP ports showing here
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745164917424/d61ce0fe-3b85-4083-a91d-3caeffd97d05.png align="center")
    
4. What is the flag hidden in the HTTP server header? `THM{web_server_25352}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745164968038/e2ece812-a6a0-4dd9-acdd-0b0bc0275f7a.png align="center")
    
5. What is the flag hidden in the SSH server header? `THM{946219583339}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745164999372/3e1ff8d7-4307-4cca-9018-38d557574306.png align="center")
    
6. We have an FTP server listening on a nonstandard port. What is the version of the FTP server? `vsftpd 3.0.5`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745165028063/aa18e7d8-c037-4c03-9f89-64efea7db7ba.png align="center")
    
7. We learned two usernames using social engineering: `eddie` and `quinn`. What is the flag hidden in one of these two account files and accessible via FTP? `THM{321452667098}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745165084871/41e4cc75-4ecd-45e9-be7a-90fd9a3df58c.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745165115705/853bff4d-dd87-4baa-9ec8-1d8fbf0b8a1d.png align="center")
    
8. Browsing to [`http://MACHINE_IP:8080`](http://MACHINE_IP:8080) displays a small challenge that will give you a flag once you solve it. What is the flag? `THM{f7443f99}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745165871963/3979b857-8b45-46c8-87f4-c643194dfd7f.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745165952473/936dd69a-c99a-454c-afa3-ce4124409756.png align="center")

## Summary

Congratulations. In this module, we have learned about passive reconnaissance, active reconnaissance, Nmap, protocols and services, and attacking logins with Hydra.

### Answer the questions below

Time to continue your journey with a new module.

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the Lab THM challenges.