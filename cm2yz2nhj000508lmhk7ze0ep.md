---
title: "Web Hacking: Burp Suite: The Basics (TryHackMe)"
datePublished: Fri Nov 01 2024 16:52:15 GMT+0000 (Coordinated Universal Time)
cuid: cm2yz2nhj000508lmhk7ze0ep
slug: web-hacking-burp-suite-the-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730479813173/21cb5a44-81ef-49f4-9d4e-746e43a297db.png
tags: tryhackme, burpsuite, write-up

---

In this article, I will write a write-up for Burp Suite: The Basics that covers What is Burp Suite, Features of Burp Community, Installation, The Dashboard, Navigation, Options, Introduction to the Burp Proxy, Connecting through the Proxy (FoxyProxy extension on Firefox), Site Map and Issue Definitions, The Burp Suite Browser, Scoping and Targeting, Proxxing HTTPS, and Example Attack.

1. Which edition of Burp Suite runs on a server and provides constant scanning for target web apps? `Burp Suite Enterprise`
    
2. Burp Suite is frequently used when attacking web applications and \_\_\_\_\_\_ applications. `Mobile`
    
3. Which Burp Suite feature allows us to intercept requests between ourselves and the target? `Proxy`
    
4. Which Burp tool would we use to brute-force a login form? `Intruder`
    
5. What menu provides information about the actions performed by Burp Suite, such as starting the proxy, and details about connections made through Burp? `Event log`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730479906438/422718d9-e4ee-43c0-89f8-60f865ce941b.png align="center")
    
6. Which tab **Ctrl + Shift + P** will switch us to? `Proxy tab`
    
7. In which category can you find a reference to a "Cookie jar"? `Sessions`
    
8. In which base category can you find the "Updates" sub-category, which controls the Burp Suite update behaviour? `Suite`
    
9. What is the name of the sub-category which allows you to change the keybindings for shortcuts in Burp Suite? `Hotkeys`
    
10. If we have uploaded Client-Side TLS certificates, can we override these on a per-project basis (yea/nay)? `yea`
    
11. **Challenge**
    
    Take a look around the site on `http://MACHINE_IP/` — we will be using this a lot throughout the module. Visit every other page that is linked on the homepage, then check your sitemap — one endpoint should stand out as being very unusual!
    
    Visit this in your browser (or use the "Response" section of the site map entry for that endpoint)
    
    Answer the questions below
    
    What is the flag you receive after visiting the unusual endpoint? `THM{NmNlZTliNGE1MWU1ZTQzMzgzNmFiNWVk}`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**