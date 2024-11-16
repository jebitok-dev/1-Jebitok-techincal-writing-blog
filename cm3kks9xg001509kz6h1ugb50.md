---
title: "Cyber Defense: Eviction (TryHackMe)"
datePublished: Sat Nov 16 2024 19:43:12 GMT+0000 (Coordinated Universal Time)
cuid: cm3kks9xg001509kz6h1ugb50
slug: cyber-defense-eviction-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731785966801/0d7eee03-c0b3-4c4f-ad89-637cefecaf7e.png
tags: tryhackme, write-up, eviction, cyberdefense

---

In this article, I will write an Eviction: Understanding the Adversary write-up.

Sunny is a SOC analyst at E-corp, which manufactures rare earth metals for government and non-government clients. She receives a classified intelligence report that informs her that an APT group (APT28) might be trying to attack organizations similar to E-corp. To act on this intelligence, she must use the MITRE ATT&CK Navigator to identify the TTPs used by the APT group, to ensure it has not already intruded into the network, and to stop it if it has.

Please visit [this link](https://static-labs.tryhackme.cloud/sites/eviction/) to check out the MITRE ATT&CK Navigator layer for the APT group and answer the questions below.

1. What is a technique used by the APT to both perform recon and gain initial access? `Spearphishing link`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731785663304/6d155f94-d839-47a5-9896-67f2405f0c04.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731785687659/4c556d17-13a6-4950-8cfe-da867160404e.png align="center")
    
2. Sunny identified that the APT might have moved forward from the recon phase. Which accounts might the APT compromise while developing resources? `Email accounts`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731785640838/7428c306-4e4a-4b18-bfad-23dd8ef53ccb.png align="center")
    
3. E-corp has found that the APT might have gained initial access using social engineering to make the user execute code for the threat actor. Sunny wants to identify if the APT was also successful in execution. What two techniques of user execution should Sunny look out for? (Answer format: &lt;technique 1&gt; and &lt;technique 2&gt;) `Malicious file and malicious link`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731785613768/45d6f410-ee4f-4f1f-9e4a-c6b8f32abd99.png align="center")
    
4. If the above technique was successful, which scripting interpreters should Sunny search for to identify successful execution? (Answer format: &lt;technique 1&gt; and &lt;technique 2&gt;) `Powershell and Windows Command shell`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731785572276/fe095ad0-3ec4-4b4e-9e1a-549107abf586.png align="center")
    
5. While looking at the scripting interpreters identified in Q4, Sunny found some obfuscated scripts that changed the registry. Assuming these changes are for maintaining persistence, which registry keys should Sunny observe to track these changes? `Registry run keys`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731784676133/202f9c44-2b0d-40cd-9ab6-37e8cf00c26d.png align="center")
    
6. Sunny identified that the APT executes system binaries to evade defences. Which system binary's execution should Sunny scrutinize for proxy execution? `Rundll32`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731784479222/f6e84ba5-9781-4f54-b6de-26adab775df9.png align="center")
    
7. Sunny identified tcpdump on one of the compromised hosts. Assuming this was placed there by the threat actor, which technique might the APT be using here for discovery? `Network sniffing`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731784505251/18f06e86-867d-429a-bcc7-de1bf6a80811.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731784291341/fca439ba-da83-46b5-9f0f-4c091db5742e.png align="center")
    
8. It looks like the APT achieved lateral movement by exploiting remote services. Which remote services should Sunny observe to identify APT activity traces? `SMB/Windows Admin shares`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731784391497/669c3727-a8ef-4454-a68b-60c8a66d7ed6.png align="center")
    
9. It looked like the primary goal of the APT was to steal intellectual property from E-corp's information repositories. Which information repository can be the likely target of the APT? `Sharepoint`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731784262713/1894b6e1-5a45-47bc-8af5-639a73278df1.png align="center")
    
10. Although the APT had collected the data, it could not connect to the C2 for data exfiltration. To thwart any attempts to do that, what types of proxy might the APT use? (Answer format: &lt;technique 1&gt; and &lt;technique 2&gt;) `external proxy and multi-hop proxy`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731784218624/3298ed8c-3acb-473f-baab-a8f579f424e1.png align="center")
    
11. Congratulations! You have helped Sunny successfully thwart the APT's nefarious designs by stopping it from achieving its goal of stealing the IP of E-corp.
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**