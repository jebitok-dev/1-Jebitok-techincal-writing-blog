---
title: "Defensive Security: SOC Fundamentals (TryHackMe)"
datePublished: Sat Nov 02 2024 15:37:17 GMT+0000 (Coordinated Universal Time)
cuid: cm30bu3u4000309mmbe2v6ods
slug: defensive-security-soc-fundamentals-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730561644951/2344d40e-9d12-46d9-8393-6e7f334ceacd.png
tags: soc, tryhackme, write-up, soc-analyst, defensive-security

---

In this article, I will write a write-up for SOC Fundamentals that covers Introduction to SOC, Purpose and Components, and People, Process, and Technology.

1. What does the term SOC stand for? `Security Operations Center`
    
2. The SOC team discovers an unauthorized user is trying to log in to an account. Which capability of SOC is this? `Detection`
    
3. What are the three pillars of a SOC? `People, Process, Technology`
    
4. Alert triage and reporting is the responsibility of? `SOC Analyst (Level 1)`
    
5. Which role in the SOC team allows you to work dedicatedly on establishing rules for alerting security solutions? `Detection Engineer`
    
6. At the end of the investigation, the SOC team found that John had attempted to steal the system's data. Which 'W' from the 5 Ws does this answer? `Who`
    
7. The SOC team detected a large amount of data exfiltration. Which 'W' from the 5 Ws does this answer? `What`
    
8. Which security solution monitors the incoming and outgoing traffic of the network? `Firewall`
    
9. Do SIEM solutions primarily focus on detecting and alerting about security incidents? (yea/nay) `yea`
    
    This practical exercise uses People, Processes, and Technology and gives you a practical walkthrough of the role of a Level 1 Analyst in the SOC team.
    
    Click on the **View Site** button below to display the lab on the right side of the screen.
    
    View Site
    
    ## **Scenario**
    
    You are the Level 1 Analyst of your organization’s SOC team. You receive an alert that a port scanning activity has been observed on one of the hosts in the network. You have access to the SIEM solution, where you can see all the associated logs for this alert. You are tasked to view the logs individually and answer the question to the 5 Ws given below.
    
    **Note**: The vulnerability assessment team notified the SOC team that they were running a port scan activity inside the network from the host: `10.0.0.8`
    
10. What: Activity that triggered the alert? `Port Scan`
    
11. When: Time of the activity? `June 12, 2024 17:24`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730561679881/1b1aec1f-fd4b-4b55-9a03-929a206f836a.png align="center")
    
12. Where: Destination host IP? `10.0.0.3`
    
13. Who: Source host name? `Nessus`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730561741734/05999095-a539-4ba9-a02a-fd4a1ce173de.png align="center")
    
14. Why: Reason for the activity? Intended/Malicious `Intended`
    
15. Additional Investigation Notes: Has any response been sent back to the port scanner IP? (yea/nay) `yea`
    
16. What is the flag found after closing the alert? `THM{000_INTRO_TO_SOC}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730561701035/16cdc7e2-f099-409a-bdab-7c1de6dfed06.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**