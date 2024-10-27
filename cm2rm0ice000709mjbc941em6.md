---
title: "Window and AD Fundamentals: Active Directory Basics (TryHackMe)"
datePublished: Sun Oct 27 2024 13:12:16 GMT+0000 (Coordinated Universal Time)
cuid: cm2rm0ice000709mjbc941em6
slug: window-and-ad-fundamentals-active-directory-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730034538179/ddba023a-9b4d-49b0-a7d3-8022a000077e.png
tags: windows, activerecord, tryhackme, write-up

---

In this article, I will write a write-up for Windows Fundamentals 3 that covers Windows Domains, Active Directory, Managing Users in AD, Managing Computers in AD, Group Policies, and Authentication Methods, together with Trees, Forests and Trusts.

1. In a Windows domain, credentials are stored in a centralized repository called... `Active Directory`
    
2. The server in charge of running the Active Directory services is called... `Domain Controller`
    
3. Which group normally administrates all computers and resources in a domain? `Domain Admins`
    
4. What would be the name of the machine account associated with a machine named TOM-PC? `TOM-PC$`
    
5. Suppose our company creates a new department for Quality Assurance. What type of containers should we use to group all Quality Assurance users so that policies can be applied consistently to them? `Organizational Units`
    
6. What was the flag found on Sophie's desktop? `THM{thanks_for_contacting_support}`
    
7. The process of granting privileges to a user over some OU or other AD Object is called... `delegation`
    
8. After organising the available computers, how many ended up in the Workstations OU? `7`
    

when we remove the SRV (which is 3) we remain with 7  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730034695526/c647891e-6990-4730-8f80-fa1827949d93.png align="center")

9. Is it recommendable to create separate OUs for Servers and Workstations? (yay/nay) `yay`
    
10. What is the name of the network share used to distribute GPOs to domain machines? `sysvol`
    
11. Can a GPO be used to apply settings to users and computers? (yay/nay) `yay`
    
12. Will a current version of Windows use NetNTLM as the preferred authentication protocol by default? (yay/nay) `nay`
    
13. When referring to Kerberos, what type of ticket allows us to request further tickets known as TGS? `Ticket Granting Ticket`
    
14. When using NetNTLM, is a user's password transmitted over the network at any point? (yay/nay) `nay`
    
15. What is a group of Windows domains that share the same namespace called? `Tree`
    
16. What should be configured between two domains for a user in Domain A to access a resource in Domain B? `A Trust Relationship`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**