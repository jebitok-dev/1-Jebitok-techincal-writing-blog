---
title: "Cyber Defense Frameworks: SOC Level 1 (TryHackMe)"
datePublished: Sat Nov 09 2024 09:05:22 GMT+0000 (Coordinated Universal Time)
cuid: cm39xx1xj000009mgguvhb1i7
slug: cyber-defense-frameworks-soc-level-1-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731140414934/04ef1ad0-d5f8-4e24-ae1f-1174d18b964c.png
tags: tryhackme, write-up, soc-analyst, cyberdefense

---

In this article, I will write a Pyramid of Pain write-up: The Basics that covers Hash Values, IP Addresses, Domain Names, Host Artifacts, Network Artifacts, Tools, TTPs, and a Practical of the Pyramid of Pain.

1. Analyse the report associated with the hash "b8ef959a9176aef07fdca8705254a163b50b49a17217a4ff0107487f59d4a35d" [here.](https://assets.tryhackme.com/additional/pyramidofpain/t3-virustotal.pdf) What is the filename of the sample? `Sales_Receipt 5606.xls`
    

I used [VirusTotal](https://www.virustotal.com/gui/file/b8ef959a9176aef07fdca8705254a163b50b49a17217a4ff0107487f59d4a35d) to search the hash inorder to find the filename

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731143060878/ef5aaee0-03a3-4d50-a62a-0abd6226a1f0.png align="center")

2\. Read the following [report](https://assets.tryhackme.com/additional/pyramidofpain/task3-anyrun.pdf) to answer this question. What is the **first IP address** the malicious process (**PID 1632**) attempts to communicate with? `50.87.136.52`

3\. Read the following [report](https://assets.tryhackme.com/additional/pyramidofpain/task3-anyrun.pdf) to answer this question. What is the **first domain name** the malicious process ((PID 1632) attempts to communicate with? [`craftingalegacy.com`](http://craftingalegacy.com/)

You will find an answer by scrolling through this page i.e a [screenshot provided](https://assets.tryhackme.com/additional/pyramidofpain/task3-anyrun.pdf)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731142914459/849ac937-8c06-464e-8bf1-4be580212fbb.png align="center")

4\. Go to [this report on app.any.run](https://app.any.run/tasks/a66178de-7596-4a05-945d-704dbf6b3b90) and provide the first **suspicious** domain request you are seeing, you will be using this report to answer the remaining questions of this task. [`craftingalegacy.com`](http://craftingalegacy.com/)

click the text report for the suspicious domain request and you will find this domain on the redirected page

5. What term refers to an address used to access websites? `Domain Name`
    
6. What type of attack uses Unicode characters in the domain name to imitate the a known domain? `Punycode attack`
    
7. Provide the redirected website for the shortened URL using a preview: [https://tinyurl.com/bw7t8p4u](https://tinyurl.com/bw7t8p4u) [`https://tryhackme.com/`](https://tryhackme.com/)
    

just run this shortened URL on the web and it will open the Tryhackme website mine opened my THM dashboard but notice that the answer ends at .com/

8. A security vendor has analysed the malicious sample for us. Review the report [here](https://assets.tryhackme.com/additional/pyramidofpain/task5-report.pdf) to answer the following questions.
    
9. A process named **regidle.exe** makes a POST request to an IP address based in the United States (US) on **port 8080**. What is the IP address? `96.126.101.6`
    

open the URL to the report then scroll down to HTTP Requests checking the one that has port 8080 and based in the US

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731142816779/9e07341e-e69e-435c-957e-419d70d3089a.png align="center")

10. The actor drops a malicious executable (EXE). What is the name of this executable? `G_jugk.exe`  
      
    after the HTTP Requests, there’s a link to a report on [Any.Run](http://any.run/) which is an interactive malware-hunting service. Under Malicious you’ll check for the ‘**Drops executable file immediately after starts**’
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731142763197/6d57537c-3a9c-4763-b50f-c1d5e0a0f071.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731141591651/a6299f3f-9b68-4aac-8ddc-11c4c93e05e2.png align="center")
    
11. Look at this [report](https://assets.tryhackme.com/additional/pyramidofpain/vtotal2.png) by Virustotal. How many vendors determine this host to be malicious? `9`
    

According to the hint we’re to count both the malicious and malware labels

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731142743105/d4aa962f-c814-4d25-8fc7-cf7dc0c42e0c.png align="center")

12. What browser uses the User-Agent string shown in the screenshot above? `Internet Explorer`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731142703956/162b245b-2331-4d2b-bf17-d836d0274468.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731142692598/69e91e67-7374-46cc-b8f8-681b896dff04.png align="center")
    
13. How many POST requests are in the screenshot from the pcap file? `6`
    
14. Provide the method used to determine similarity between the files `Fuzzy Hashing`
    
15. Provide the alternative name for fuzzy hashes without the abbreviation `context triggered piecewise hashes`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731141340611/ee8cfeb0-755f-4df9-a907-5e12c74b9179.png align="center")
    
16. Navigate to ATT&CK Matrix webpage. How many techniques fall under the Exfiltration category? `9`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731141324766/2aaf645e-b76d-47c8-95e3-535945fac159.png align="center")
    
17. Chimera is a China-based hacking group that has been active since 2018. What is the name of the commercial, remote access tool they use for C2 beacons and data exfiltration? `Cobalt Strike`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731141304331/b02ab209-0610-436f-81de-385a506004f0.png align="center")
    
      
    
    Deploy the static site attached to this task and place the prompts into the correct tiers in the pyramid of pain!
    
    Once you are sure, submit your answer on the static site to retrieve a flag!
    
    Answer the questions below
    
18. Complete the static site. What is the flag? `THM{PYRAMIDS_COMPLETE}`
    
    1. **TTP (Tactics, Techniques, and Procedures)** - *"The attackers' plans and objectives"*
        
        * TTPs describe the behavior and methods attackers use to achieve their objectives.
            
    2. **Tools** - *"The attackers have utilized these to accomplish their objective"*
        
        * Tools are the software, scripts, or utilities used by attackers to carry out their activities.
            
    3. **Network** - *"These artifacts can present themselves as C2 traffic, for example"*
        
        * Network artifacts like traffic patterns or protocols can indicate command-and-control (C2) communications.
            
    4. **IP addresses** - *"These addresses can be used to identify the infrastructure an attacker is using for their campaign"*
        
        * IP addresses are often tied to servers or hosts used in an attacker’s infrastructure.
            
    5. **Hash values** - *"These signatures can be used to attribute payloads and artifacts to an actor"*
        
        * Hash values uniquely identify files or payloads, helping attribute them to specific threat actors or campaigns.
            
    6. **Domain** - *"An attacker has purchased this and used it in a typosquatting campaign"*
        
        * Domains can be registered to resemble legitimate sites in typosquatting or phishing campaigns.
            
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731141142019/4cc441dc-306e-4dcc-ad24-311e1172a0cf.png align="center")
    
      
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**