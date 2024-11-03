---
title: "Security Solutions: Introduction to SIEM (TryHackMe)"
datePublished: Sun Nov 03 2024 14:15:21 GMT+0000 (Coordinated Universal Time)
cuid: cm31ockzi000009jsa5y2asyv
slug: security-solutions-introduction-to-siem-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730643050976/69a41d55-5903-4bc9-9993-c26c76b20640.png
tags: tryhackme, write-up, siem, securitysolutions, security-information-and-event-management-siem

---

In this article, I will write a write-up for Introduction to SIEM that covers Network Visibility through SIEM, Log Sources and Log Ingestion, Why SIEM, Analysing Logs and Alerts, and Lab Work.

1. What does SIEM stand for? `Security Information and Event Management system`
    
2. Is Registry-related activity host-centric or network-centric? `host-centric`
    
3. Is VPN related activity host-centric or network-centric? `network-centric`
    
4. In which location within a Linux environment are HTTP logs stored? `/var/log/httpd`
    
5. Which Event ID is generated when event logs are removed? `104`
    
6. What type of alert may require tuning? `False Alarm`
    

In the static lab attached, a sample dashboard and events are displayed. When a suspicious activity happens, an Alert is triggered, which means some events match the condition of some rule already configured. Complete the lab and answer the following questions.

7. Click on Start Suspicious Activity, which process caused the alert? `cudominer.exe`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730643258679/9f8315eb-28ca-441e-b65e-0d946287a360.png align="center")
    
8. Find the event that caused the alert, which user was responsible for the process execution? `chris.fort`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730643232449/8f589a22-6ea4-42a5-b33b-aca66b88516f.png align="center")
    
9. What is the hostname of the suspect user? `HR_02`
    
10. Examine the rule and the suspicious process; which term matched the rule that caused the alert? `miner`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730643207668/9f35d2e5-cd09-4eb5-9f8e-463aab29534c.png align="center")
    
11. What is the best option that represents the event? Choose from the following:
    
    * False-Positive
        
    * True-Positive
        
    
    `True-Positive`
    
12. Selecting the right ACTION will display the FLAG. What is the FLAG? `THM{000_SIEM_INTRO}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730643187835/0913a8bd-e2d4-49a3-956e-aa5305efc201.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**