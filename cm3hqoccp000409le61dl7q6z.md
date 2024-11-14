---
title: "Cyber Defence Frameworks: MITRE (TryHackMe)"
datePublished: Thu Nov 14 2024 20:04:47 GMT+0000 (Coordinated Universal Time)
cuid: cm3hqoccp000409le61dl7q6z
slug: cyber-defence-frameworks-mitre-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731576291400/56acc292-0cb8-4d1a-97b2-987726ce00ed.png
tags: defense, tryhackme, write-up, threat-hunting, threat-intelligence, mitre-attack

---

In this article, I will write an MITRE write-up: The Basics that covers Introduction to MITRE, Basic Terminology, ATT&CK Framework, CAR Knowledge Base, MITRE Engage, MITRE D3FEND, ATT&CK Emulation Plans, and ATT&CK and Threat Intelligence.

1. Besides Blue teamers, who else will use the ATT&CK Matrix? (Red Teamers, Purpe Teamers, SOC Managers?) `Red Teamers`
    
2. What is the ID for this technique? `T1566`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731578262903/48ad8e4d-f071-43f3-b6a0-bc048fea592d.png align="center")
    
3. Based on this technique, what mitigation covers identifying social engineering techniques? `User Training`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731578230297/7723ae65-7800-4a1e-b88c-ad3ebc7d518c.png align="center")
    
4. What are the data sources for Detection? \*\*(\*\*format: source1,source2,source3 with no spaces after commas) `Application Log,File,Network Traffic`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731578170270/5091a837-0f73-47c4-bf10-963732a18a93.png align="center")
    
5. What groups have used spear-phishing in their campaigns? (format: group1,group2) `Axiom,Gold SOUTHFIELD`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731578133633/78e7249b-0b2f-44dd-b74a-2b7b4ef5b900.png align="center")
    
6. Based on the information for the first group, what are their associated groups? `Group 72`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731578110051/7f229891-2830-4062-8a29-f35db3489728.png align="center")
    
7. What software is associated with this group that lists phishing as a technique? `Hikit`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731578074710/dc8d79f2-8093-44e9-955e-99f3cc2806b5.png align="center")
    
8. What is the description for this software? `Hikit is malware that has been used by Axiom for late-stage persistence and exfiltration after the initial compromise.`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731578052540/140a241e-c516-4ea6-a445-189d2c305c74.png align="center")
    
9. This group overlaps (slightly) with which other group? `Winnti Group`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731578034712/302493c4-3928-4e15-81c1-5f3199303d44.png align="center")
    
10. How many techniques are attributed to this group? `15`
    
    count the number of enterprises/techniques but ignore ID that has two parts for example T1583, you count it as one technique
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577914034/50b5a787-c98e-43a5-8401-812fd99f861c.png align="center")
    
11. What tactic has an ID of TA0003? `Persistence`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577807292/dcb20198-fb3a-4469-91de-2f4b8fe0aad4.png align="center")
    
12. What is the name of the library that is a collection of Zeek (BRO) scripts? `BZAR`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577784490/d86cbd11-51c0-41f2-9d75-b18bec3a403d.png align="center")
    
13. What is the name of the **technique** for running executables with the same hash and different names? `Masquerading`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577750440/00d360d2-6679-4aff-a994-c293cd1fd3f6.png align="center")
    
14. Examine CAR-2013-05-004, besides Implementations, what additional information is provided to analysts to ensure coverage for this technique? `Unit Tests`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577724684/8394d1df-ee69-43cc-ae89-bd83c2a4e0c1.png align="center")
    
15. Under Prepare, what is ID SAC0002? `Persona Creation`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577703665/4165e149-34a4-4803-8970-f255c6e6f952.png align="center")
    
16. What is the name of the resource to aid you with the engagement activity from the previous question? `Persona Profile Worksheet`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577580591/f270b2fe-9f73-49ea-be70-b14aabff1ce3.png align="center")
    
17. Which engagement activity baits a specific response from the adversary? `Lures`
    
18. What is the definition of Threat Model? `A risk assessment that models organizational strengths and weaknesses`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577521377/1a6ab208-6686-4de5-ba22-3bfcc5d2e9fe.png align="center")
    
19. What is the first MITRE ATT&CK technique listed in the ATT&CK Lookup dropdown? `Data Obfuscation`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577506127/deec8f9e-2ded-4cf8-9817-243b35f99c7c.png align="center")
    
20. In D3FEND Inferred Relationships, what does the ATT&CK technique from the previous question produce? `Outbound Internet Network Traffic`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577487980/4d19d3ac-b367-476f-ad2e-8ac1b5b843c5.png align="center")
    
21. In Phase 1 for the APT3 Emulation Plan, what is listed first? `C2 Setup`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577468315/5edf5e04-b55d-4a93-a937-08cb9a8b9bd5.png align="center")
    
22. Under Persistence, what binary was replaced with cmd.exe? `sethc.exe`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577448207/c3777787-e8b5-4a1f-9f29-f2220bf11bf8.png align="center")
    
23. Examining APT29, what  C2 frameworks are listed in Scenario 1 Infrastructure? (format: tool1,tool2) `Pupy,Metasploit Framework`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577429363/cf08a825-4aa5-4cca-bebe-06ac9f06a438.png align="center")
    
24. What C2 framework is listed in Scenario 2 Infrastructure? `PoshC2`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731577411762/a401352b-7ae0-411b-83ed-6fffee047ef4.png align="center")
    
25. Examine the emulation plan for Sandworm. What webshell is used for Scenario 1? Check MITRE ATT&CK for the Software ID for the webshell. What is the id? (format: webshell,id) `P.A.S.,S0598`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731614046223/a95fa74f-ede6-462c-b8ff-c739ebbe92b6.png align="center")
    
    Install the ATT&CK Powered Suit browser extension on Chrome, Brave, or any other browser. Then open the extension by clicking the & icon then search P.A.S. and the ID (`S0598`) will popup
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731576410778/6de5506c-798f-42a0-904c-f5bd769665b6.png align="center")
    
26. What is a group that targets your sector who has been in operation since at least 2013? `APT33`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731576383845/b938f3e4-d331-4c03-8578-07ae6e3e1c01.png align="center")
    
27. As your organization is migrating to the cloud, is there anything attributed to this APT group that you should focus on? If so, what is it? `Cloud Accounts`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731576357809/6b500923-96c4-4451-a709-cd580572b09b.png align="center")
    
28. What tool is associated with the technique from the previous question? `Ruler`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731576342755/fce788b0-cf2d-4ab8-b650-827dd075f72c.png align="center")
    
29. Referring to the technique from question 2, what mitigation method suggests using SMS messages as an alternative for its implementation? `Multi-factor Authentication`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731576326527/042f4a06-2ed7-4982-b208-e753ecc39b70.png align="center")
    
30. What platforms does the technique from question #2 affect? `Azure AD, Google Workspace, IaaS, Office 365, SaaS`  
      
      
    In this room, we explored tools/resources that MITRE has provided to the security community. The room's goal was to expose you to these resources and give you a foundational knowledge of their uses. Many vendors of security products and security teams across the globe consider these contributions from MITRE invaluable in the day-to-day efforts to thwart evil. The more information we have as defenders, the better we are equipped to fight back. Some of you might be looking to transition to become a SOC analyst, detection engineer, cyber threat analyst, etc. These tools/resources are a must to know.
    
      
    This is not only for defenders. As red teamers, these tools/resources are useful as well. Your objective is to mimic the adversary and attempt to bypass all the controls in place within the environment. With these resources, as the red teamer, you can effectively mimic a true adversary and communicate your findings in a common language that both sides can understand. In a nutshell, this is known as **purple teaming**.    
      
    This room made me like threat hunting and threat intelligence in general, I might consider it as a path.  
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**