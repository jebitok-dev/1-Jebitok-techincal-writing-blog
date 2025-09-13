---
title: "Invite Only (TryHackMe)"
datePublished: Sat Sep 13 2025 14:39:22 GMT+0000 (Coordinated Universal Time)
cuid: cmfidgyiq000202lb3bq3a8oh
slug: invite-only-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1757774244886/4361fb0d-e761-49b2-abd3-317cc383fbad.png
tags: osint, tryhackme, socanalyst, ctf-writeup, virustotal

---

As part of my role on the SOC team at Managed Server Provider TrySecureMe, I was tasked with investigating suspicious findings that had been escalated by an L1 analyst. The flagged indicators included a suspicious IP address and a SHA256 hash. Using the TryDetectThis2.0 application and supporting OSINT tools like VirusTotal, I analyzed the associated files, their execution chain, and any dropped payloads. I also traced the threat back to its malware family, reviewed related reports, and identified the attacker’s tools, phishing techniques, and delivery platform. This exercise combined hands-on analysis with threat intelligence research to connect the indicators of compromise (IOCs) into a broader attack narrative.

You are an SOC analyst on the SOC team at Managed Server Provider TrySecureMe. Today, you are supporting an L3 analyst in investigating flagged IPs, hashes, URLs, or domains as part of IR activities. One of the L1 analysts flagged two suspicious findings early in the morning and escalated them. Your task is to analyse these findings further and distil the information into usable threat intelligence.

`Flagged IP: 101[.]99[.]76[.]120`

`Flagged SHA256 hash: 5d0509f68a9b7c415a726be75a078180e3f02e59866f193b0a99eee8e39c874f`

We recently purchased a new threat intelligence search application called TryDetectThis2.0. You can use this application to gather information on the indicators above.

To solve these challenges, I used [VirusTotal](https://www.virustotal.com/) on the browser to search for the flagged SHA256 hash and the flagged IP, as well as navigating to the other mentioned hashes or files within the challenge. The challenge somehow borrows OSINT skills. It was a bit challenging at some point, but taking a break helped me think clearly and find the intended answers

### Answer the questions below

What is the name of the file identified with the flagged SHA256 hash?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757772957765/07420fa6-20e0-4aa7-b366-e98ac1cc04d3.png align="center")

What is the file type associated with the flagged SHA256 hash?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757773240994/5a0400b2-f04b-48c1-a4f6-1fedc79e644b.png align="center")

What are the execution parents of the flagged hash? List the names chronologically, using a comma as a separator. Note down the hashes for later use.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757773250946/2c57e9c6-7a68-4a5a-9dd6-3b1e25b97812.png align="center")

What is the name of the file being dropped? Note down the hash value for later use.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757773314291/cb4260f4-448e-4ed2-be22-8ceeb1332d08.png align="center")

Research the second hash in question 3 and list the four malicious dropped files in the order they appear (from up to down), separated by commas.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757773365928/cb71b948-ec8b-4574-9c5b-8314ea12d240.png align="center")

Analyse the files related to the flagged IP. What is the malware family that links these files?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757773383957/eb730002-3c0b-464d-b30a-77c55eaf87be.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757773768418/d316621f-f2bf-4641-afea-81ca67199fce.png align="center")

What is the title of the original report where these flagged indicators are mentioned? Use Google to find the report.  
This is where OSINT comes into play. I tried searching for where the malware family is mentioned alongside the common file was mentioned but found a helpful file to learn more about the [malware family’s C2 Framework](https://blog.qualys.com/vulnerabilities-threat-research/2022/08/16/asyncrat-c2-framework-overview-technical-analysis-and-detection). This wasn’t the answer. Went ahead to search for a file that would have both the flagged IP and the flagged SHA-256 hash, and the ones I found were not helpful. Searching the SHA-256 hash alone brought up the [report](https://research.checkpoint.com/2025/from-trust-to-threat-hijacked-discord-invites-used-for-multi-stage-malware-delivery/). Based on the community section of one of the hashes on VirusTotal, there was a hint of this, too.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757773744052/85f0883d-c07b-4f1f-8b8a-482b96c76ad5.png align="center")

Which tool did the attackers use to steal cookies from the Google Chrome browser?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757773868229/b739cc4c-750c-43b5-938a-9dcef36815f3.png align="center")

Which phishing technique did the attackers use? Use the report to answer the question.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757773996677/74f0cd37-f6a5-40d6-a7ff-ba5989be8901.png align="center")

What is the name of the platform that was used to redirect a user to malicious servers?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757774043907/e8738020-1dd8-4661-bea6-676ef46cdfeb.png align="center")

  
  
This investigation highlighted the importance of correlating file analysis with external threat intelligence to uncover the full scope of malicious activity. By starting with a single IP and hash, I was able to identify the dropped files, the malware family (AsyncRAT), and the adversary’s tactics, including credential and cookie theft, phishing methods, and the use of Discord as a delivery platform. The findings align with documented campaigns described in security research reports, reinforcing how OSINT complements internal detection capabilities. Overall, this case demonstrated how layered analysis supports SOC operations by transforming isolated alerts into actionable threat intelligence.