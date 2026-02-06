---
title: "TShark Challenge I: Teamwork - (T-Shark & Virus Total) (TryHackMe)"
datePublished: Fri Feb 06 2026 19:06:00 GMT+0000 (Coordinated Universal Time)
cuid: cmlb9a7uj000j02jpbhx85fxv
slug: tshark-challenge-i-teamwork-t-shark-and-virus-total-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1770404715880/e0bb65e6-8cd5-4cc2-8bf5-7e3ec39310a6.png
tags: ctf, tryhackme, virustotal, cyberchef, tshark

---

## Introduction

This room presents you with a challenge to investigate some traffic data as a part of the SOC team. Let's start working with TShark to analyse the captured traffic. We recommend completing the [TShark: The Basics](https://tryhackme.com/room/tsharkthebasics) [and TShark: CLI W](https://tryhackme.com/room/tsharkthebasics)[ireshark Features](https://tryhackme.com/room/tsharkcliwiresharkfeatures) [rooms first, which will teach](https://tryhackme.com/room/tsharkcliwiresharkfeatures) you how to use the tool in depth. 

## Case: Teamwork!

**An alert has been triggered:** "The threat research team discovered a suspicious domain that could be a potential threat to the organisation."

The case was assigned to you. Inspect the provided **teamwork.pcap** located in `~/Desktop/exercise-files` and create artefacts for detection tooling.

**Your tools:** TShark, [VirusTotal](https://www.virustotal.com/gui/home/upload).

### Answer the questions below

Investigate the contacted domains.  
Investigate the domains by using VirusTotal.  
According to VirusTotal, there is a domain marked as malicious/suspicious.  
  
What is the full URL of the malicious/suspicious domain address?

1. Enter your answer in **defanged** format. `hxxp[://]www[.]paypal[.]com4uswebappsresetaccountrecovery[.]timeseaways[.]com/`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770370523368/7616913f-1623-4208-8728-d114e6d63c3e.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770370674268/a0c851df-85bc-47c2-858e-71738c394afb.png align="center")
    
2. When was the URL of the malicious/suspicious domain address first submitted to VirusTotal? `2017-04-17 22:52:53 UTC`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770370542033/b3ef6605-f0d5-43c2-8277-4e609ad54c54.png align="center")
    
3. Which known service was the domain trying to impersonate? `PayPal`
    
4. What is the IP address of the malicious domain?
    
    Enter your answer in **defanged** format. `184[.]154[.]127[.]226`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770370598068/c323b265-9402-4f8c-be97-9b3f7509fd45.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770370772171/a9fe2c9d-6b80-45da-9731-bf95b0e44c62.png align="center")
    
5. What is the email address that was used?
    
    Enter your answer in **defanged** format. (**format:** aaa\[at\]bbb\[.\]ccc) `johnny5alive[at]gmail[.]com`
    
    ```bash
    tshark -r ~/Desktop/exercise-files/teamwork.pcap -Y "http.request.method == POST" -T fields -e http.file_data
    xBrowser=Mozilla+FireFox+v43&xOperatingSystem=Linux&xPlatForm=Desktop+Platform
    user=johnny5alive%40gmail.com&pass=johnny5alive&xBrowser=Mozilla+FireFox+v43&xOperatingSystem=Linux&xPlatForm=Desktop+Platform&xTimeZone=Mon+Apr+17+2017+22%3A00%3A35+GMT-0400+(EDT)&xResoLution=Computer%3A+1920x1080%3B+Browser+inner%3A+1920x762%3B+Browser+outer%3A+1920x1027&xLang=en-US
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770370800076/6685b0b3-eaf0-4aa8-a2fb-14c168b2ce33.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770370814845/1b6a8541-7dbc-4ef6-80a3-68758dae026a.png align="center")

Congratulations! You have finished the first challenge room, but there is one more ticket before calling it out a day!

* [TShark Challenge II: Directory](https://tryhackme.com/r/room/tsharkchallengestwo)