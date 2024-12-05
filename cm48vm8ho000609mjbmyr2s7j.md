---
title: "Advent Of Cyber: Day 1 - OPSEC (TryHackMe)"
datePublished: Tue Dec 03 2024 19:52:54 GMT+0000 (Coordinated Universal Time)
cuid: cm48vm8ho000609mjbmyr2s7j
slug: advent-of-cyber-day-1-opsec-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733363056440/5890d415-6026-4751-bba3-9deb705d4aa8.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1733255556311/65747865-9c98-40c5-9359-fcfa42cc6175.png
tags: tryhackme, write-up, opsec, adventofcyber2024

---

The Advent of Cyber, 2024 by TryHackMe is finally here. AOC is a yearly event by TryHackMe that drops daily up to Christmas. This year is focused on SOC-mas where we’ll be exploring by learning and solving challenges of various topics.

In this article, I will be writing a **Day 1: Maybe SOC-mas music, he thought, doesn't come from a store?** writeup

Before answering the questions, follow the instructions by starting the machine and opening the ip\_address on the browser which will open The Glitch website. You’ll paste the provided YouTube URL in order to convert it to MP3 and download the zip file. Go to Root’s home folder open it and you’ll extract the download.zip, you can extract it to the download folder or just choose extract here. (Good OPSec, always use the virtual machines as much as possible to keep your system secure)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733220620904/35a1fbac-ee04-4bf1-8683-4d679dbf1216.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733220780393/b73acdc9-38f2-47ae-8dc8-739a77aafb5c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733220607185/a1995384-6fa7-4091-bccf-d07045afa04c.png align="center")

With the two files song.mp3 and somg.mp3. We’re good to start checking our questions but first you can try follow the commands provided before the questions this will help you navigate the questions.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733220835265/a2788286-e473-4e67-8c34-b48f93c90a07.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733220830751/b4965499-415d-4fbe-847b-5010eefc6d38.png align="center")

Above you’ll see the https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1 which will open up the script

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733220856908/12f050d4-48cd-440e-9165-0d088645162a.png align="center")

From here we’ll be starting to answer the questions: on GitHub Explorer we’ll search Created by the one and only M.M in order to find the repository and be able to answer some of the questions

Answer the questions below

1. Looks like the song.mp3 file is not what we expected! Run "exiftool song.mp3" in your terminal to find out the author of the song. Who is the author? `Tyler Ramsbey`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733220116027/28c812ad-fd0a-4e8b-b49b-9c7416e18963.png align="center")
    
2. The malicious PowerShell script sends stolen info to a C2 server. What is the URL of this C2 server? [`http://papash3ll.thm/data`](http://papash3ll.thm/data)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733220232902/34badd42-a4e3-4884-87c8-162ac9894647.png align="center")
    
3. Who is M.M? Maybe his Github profile page would provide clues? `Mayor Malware`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733221696441/bf7311b7-3ba4-424d-b4a5-b09d2cf6b25f.png align="center")
    
4. What is the number of commits on the GitHub repo where the issue was raised?`1`
    
    Based on the time of creation of the challenge, the answer is one since the other issues were created after the challenge dropped
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733255353586/e87cfacf-6333-4882-b2a7-b83ddce3fc50.png align="center")
    
    Thank you for reading, looking forward to the other days of AOC till Christmas day. You can leave a comment or we can connect more on [LinkedIn](https://linkedin.com/in/Sharon-Jebitok).