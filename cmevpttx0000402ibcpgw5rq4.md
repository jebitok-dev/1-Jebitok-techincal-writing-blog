---
title: "Challenges: A Bucket of Phish (TryHackMe)"
datePublished: Fri Aug 01 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmevpttx0000402ibcpgw5rq4
slug: challenges-a-bucket-of-phish-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756389501118/be3eccbd-7464-456d-bbc8-cbdadca42037.png
tags: phishing, tryhackme, aws-s3, s3-bucket, ctf-writeup

---

Phishing remains one of the most common attack vectors used by threat actors to harvest user credentials. In this task, we investigated a phishing website hosted on Amazon S3, allegedly set up by “DarkInjector” to trick users into providing their login information. The objective was to analyze the site and extract the list of victim accounts that may have been compromised.

Void

![Character Image](https://tryhackme-images.s3.amazonaws.com/user-uploads/62ff64c3c859dc0042b2b9f6/room-content/62ff64c3c859dc0042b2b9f6-1742203659685.png align="left")

DarkInjector has been using a Cmail phishing website to try to steal our credentials. We believe some of our users may have fallen for his trap. Can you retrieve the list of victim users?

Here's the link to the website: [http://darkinjector-phish.s3-website-us-west-2.amazonaws.com](http://darkinjector-phish.s3-website-us-west-2.amazonaws.com)

`aws s3 ls s3://darkinjector-phish --region us-west-2 --no-sign-request`

`aws s3 cp s3://darkinjector-phish/captured-logins-093582390 ./victims.txt --region us-west-2 --no-sign-request`

`cat victims.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756388010536/2407a8d8-4a55-4303-bf9f-469c1cda5e22.png align="center")

`curl -I http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/captured-login-093582390`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756388424484/819c463f-88be-47b9-b8f6-13e98642978b.png align="center")

`cat victims.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756389221701/8cb06cd6-fa25-45bb-a197-07abad1a81d3.png align="center")

By carefully examining the exposed S3 bucket and retrieving the captured login file, we successfully identified the victim user credentials stored by the attacker. This exercise highlights the importance of monitoring for phishing campaigns, enforcing user awareness, and securing cloud storage to prevent abuse.