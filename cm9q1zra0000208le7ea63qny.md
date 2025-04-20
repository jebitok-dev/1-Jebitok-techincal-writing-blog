---
title: "Vulnerability Research: Vulnerability Capstone (TryHackMe)"
datePublished: Sun Apr 20 2025 19:41:59 GMT+0000 (Coordinated Universal Time)
cuid: cm9q1zra0000208le7ea63qny
slug: vulnerability-research-vulnerability-capstone-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1745177309709/bffc409d-c983-4536-abd1-41c4d47e7315.png
tags: vulnerability, tryhackme, write-up, vulnerability-research

---

This article will cover the [Vulnerability Capstone](https://tryhackme.com/room/vulnerabilitycapstone) write-up under the Jr Penetration Tester on THM.

## Introduction

![](https://assets.tryhackme.com/additional/vulnerability-module/bug-transparent.png align="left")

Summarise the skills learnt in this module by completing this capstone room for the "Vulnerability Research" module. 

Ackme Support Incorporated has recently set up a new blog. Their developer team have asked for a security audit to be performed before they create and publish articles to the public. 

It is your task to perform a security audit on the blog; looking for and abusing any vulnerabilities that you find.

### Answer the questions below

Let's get hacking

## Exploit the Machine (Flag Submission)

Deploy the vulnerable machine attached to this by pressing the green "Start Machine" button. It is **recommended** that you use the TryHackMe AttackBox to complete this room.

Allow **five minutes** to pass before attempting to attack the vulnerable machine **MACHINE\_IP**

### Answer the questions below

1. Deploy the vulnerable machine attached to this task & wait **five minutes** before visiting the vulnerable machine.
    
2. What is the name of the application running on the vulnerable machine? `Fuel CMS`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745177521505/7c24c57d-43c3-4586-92ee-80efa30d26d7.png align="center")
    
3. What is the version number of this application? `1.4`
    
4. What is the number of the CVE that allows an attacker to remotely execute code on this application?
    
    **Format:** CVE-XXXX-XXXXX `CVE-2018-16763`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745177649631/fdc75f3a-1123-4d84-a315-52dac18b6917.png align="center")
    
5. Use the resources & skills learnt throughout this module to find and use a relevant exploit to exploit this vulnerability.
    
    **Note:** There are numerous exploits out there that can be used for this vulnerability (some more useful than others!)  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745177709738/eb372265-aa58-4464-91e9-6dab13e81b4a.png align="center")
    
6. What is the value of the flag located on this vulnerable machine? This is located in /home/ubuntu on the vulnerable machine. `THM{ACKME_BLOG_HACKED}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745178013458/b7e8d8bd-75a3-411a-b6ca-43cee3737c27.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges.