---
title: "The Advent of Cyber: Day 5: XXE - SOC-mas XX-what-ee? (TryHackMe)"
datePublished: Sat Jan 11 2025 12:15:17 GMT+0000 (Coordinated Universal Time)
cuid: cm5s5fyc3001409l13cfcew2b
slug: the-advent-of-cyber-day-5-xxe-soc-mas-xx-what-ee-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736597678166/2e6de33f-2ddb-4524-92fb-a0f3f9fe3b87.png
tags: tryhackme, write-up, xxe, portswigger, adventofcyber2024

---

In this article, we’ll cover the Log Analysis. Even if I wanted to go, their vulnerabilities wouldn't allow me to write up the log analysis as the Day 5 challenge of the Advent of Cyber event challenge. It was interesting to navigate Web Security for an e-commerce gifting site using [PortSwigger’s XML external entity (XXE) injection](https://portswigger.net/web-security/xxe). We’re still at Wareville for SOC-mas!

1. What is the flag discovered after navigating through the wishes? `THM{Brut3f0rc1n6_mY_w4y}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736596726807/52403c94-e427-4602-8bf6-91a4db282a9d.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736596795732/77b15f61-c159-4ee5-8b0f-2892327f3c0c.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736596822922/cd753071-4cea-40fe-9962-141820e6f32e.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736596856898/cfacb223-8473-4c34-88c1-07f2900811bc.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736597042164/1a4952a7-f912-46ad-bfbc-6350a0c91613.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736597244362/046e03c2-ebf1-4c50-acc7-f01df80cfca1.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736597278692/10732850-d3e8-489e-8662-c1875f89b531.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736597350197/09e3699f-0fc9-42e9-b881-d96af6b9b2ee.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736597412726/dad5432f-d6f1-4ee2-b2d1-1fcdf5b97c21.png align="center")
    
2. What is the flag seen on the possible proof of sabotage? `THM{m4y0r_m4lw4r3_b4ckd00rs}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736597129714/22bb39c6-99a3-4ae1-8b5b-9cb241deac68.png align="center")
    
3. If you want to learn more about the XXE injection attack, check out [the](https://tryhackme.com/r/room/xxeinjection) [XXE](https://tryhackme.com/r/room/xxeinjection) room!
    
4. Following McSkidy's advice, Software recently hardened the server. It used to have many unneeded open ports, but not anymore. Not that this matters in any way.
    

Thank you for reading this article. Please leave a comment with your thoughts, areas for improvement, other suggestions, and questions. Stay secure until the next one!