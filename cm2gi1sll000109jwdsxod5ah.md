---
title: "Security of the Pipeline: Introduction to Pipeline Automation"
datePublished: Sat Oct 19 2024 18:35:50 GMT+0000 (Coordinated Universal Time)
cuid: cm2gi1sll000109jwdsxod5ah
slug: security-of-the-pipeline-introduction-to-pipeline-automation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729362768044/dacb3561-9f5e-45af-9cc9-134eb7f210fd.png
tags: security, automation, devops

---

In this article, I will be writing the write-up for the walkthrough of security of the pipeline specifically the introduction to pipeline automation in DevOps.

According to TryHackMe introduction to this room, humans are always looking for simpler and more efficient ways to do things. Just as we started programming and developing software, we were looking for ways to automate some of the tasks. Today, automation is heavily ingrained in the Software Development Life Cycle (SDLC) and DevOps processes. While this is incredibly good for production, allowing for faster development and deployment, it does, however, introduce new security risks. When these processes are manual, an attacker would have to compromise the credentials or workstation of the individual that performed the relevant process. However, with automation, an attacker can now go after the pipeline itself.

1. Where in the pipeline is our end product deployed? `Environments`
    
2. Who is the largest online provider of Git? `Github`
    
3. What popular Git product is used to host your own Git server? `Gitlab`
    
4. What tool can be used to scan the commits of a repo for sensitive information? `GittyLeaks`
    
5. What do we call the type of dependency that was created by our organisation? (Internal/External) `Internal`
    
6. What type of dependency is JQuery? (Internal/External) `External`
    
7. What is the name of Python's public dependency repo? `PyPi`
    
8. What dependency 0day vulnerability set the world ablaze in 2021? `Log4j`
    
9. What type of tool scans code to look for potential vulnerabilities? `SAST`
    
10. What type of tool runs code and injects test cases to look for potential vulnerabilities? `DAST`
    
11. Can SAST and DAST be used as a replacement for penetration tests? (Yea,Nay) `Nay`
    
12. What does CI in CI/CD stand for? `Continuous Integration`
    
13. What does CD in CI/CD stand for? `Continuous Delivery`
    
14. What do we call the build infrastructure element that controls all builds? `Build Orchestrator`
    
15. What do we call the build infrastructure element that performs the build? `Build Agent`
    
16. Which environment usually has the weakest security configuration? `DEV`
    
17. Which environment is used to test the application? `UAT`
    
18. Which environment is similar to PROD but is used to verify that everything is working before it is pushed to PROD? `PrePROD`
    
19. What is a common class of vulnerabilities that is discovered in PROD due to insecure code creeping in from DEV? `Developer Bypasses`
    
20. What is the flag received after successfully building your pipeline?`THM{`[`Pipeline.Automation.Is.Fun`](http://Pipeline.Automation.Is.Fun)`}`
    

Thank you for reading through my article. You can leave any questions or comments on how I can improve my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**