---
title: "The Advent of Cyber: Day 14: Certificate mismanagement - Even if we're horribly mismanaged, there'll be no sad faces on SOC-mas! (TryHackMe)"
datePublished: Sat Jan 11 2025 18:07:10 GMT+0000 (Coordinated Universal Time)
cuid: cm5si0hn5000408mhc39ra2br
slug: the-advent-of-cyber-day-14-certificate-mismanagement-even-if-were-horribly-mismanaged-therell-be-no-sad-faces-on-soc-mas-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736618291435/342faae6-9486-48cb-b2ff-4466cfa0b485.png
tags: tryhackme, burpsuite, write-up, portswigger, adventofcyber2024

---

In this article, we’ll cover Certificate mismanagement - Even if we're mismanaged, there'll be no sad faces on SOC-mas! write-up as the Day 14 challenge of the Advent of Cyber event challenge. It involved using Portswagger’s Burp Suite to take advantage of the certificate mismanagement to access the admin’s password which will allow us to schedule a gift card successfully on the Gift Card site. We’re still at Wareville for SOC-mas!

1. What is the name of the CA that has signed the Gift Scheduler certificate? `THM`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736606797807/7e8c52fb-50f9-452b-b035-0a99a757b60a.png align="center")
    
2. Look inside the POST requests in the HTTP history. What is the password for the `snowballelf` account? `c4rrotn0s3`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736606842259/5a265421-6a59-452f-81d4-1ff420baec8c.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736606893725/b21da7a7-94f0-4f55-a817-a92a3702ba84.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736617378870/07880344-1583-4467-8883-6ce154d150f9.png align="center")
    
3. Use the credentials for any of the elves to authenticate to the Gift Scheduler website. What is the flag shown on the elves’ scheduling page? `THM{AoC-3lf0nth3Sh3lf}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736618221996/a0205a78-6659-4aee-b683-2ed1963f8254.png align="center")
    
      
    
4. What is the password for Marta May Ware’s account? `H0llyJ0llySOCMAS`!  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736616532721/eef6f6eb-d1a8-497a-8377-c56890cf643d.png align="center")
    
5. Mayor Malware finally succeeded in his evil intent: with Marta May Ware’s username and password, he can finally access the administrative console for the Gift Scheduler. G-Day is cancelled!  
    What is the flag shown on the admin page? `THM{AoC-h0wt0ru1nG1ftD4y}`  
    
6. If you enjoyed this task, feel free to check out the [Burp Suite](https://tryhackme.com/module/learn-burp-suite) module.
    

Thank you for reading this article. Please leave a comment with your thoughts, areas for improvement, other suggestions, and questions. Stay secure until the next one!