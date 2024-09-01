---
title: "Security Principles (TryHackMe)"
datePublished: Sun Sep 01 2024 19:53:23 GMT+0000 (Coordinated Universal Time)
cuid: cm0jzomwo000t0amde04dfh4f
slug: security-principles-tryhackme
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1725220367336/f6ca04e9-04a7-4f88-8fa2-be1084dbcadd.png

---

1. Two companies are negotiating a certain agreement; however, they want to keep the details of the agreement secret. **Which security pillar are they emphasizing?** `Confidentiality`  
      
    One hotel is stressing that the Internet over its WiFi network must be accessible 24 hours a day, seven days a week. **Which security pillar is the hotel requiring?** `Availability`
    
    You went to cash out a cheque, and the bank teller made you wait for five minutes as they confirmed the signature of the cheque's issuer. **Which security function is the bank teller checking?** `Integrity`
    
    At a police checkpoint, the police officer suspected that the vehicle registration papers were fake. **Which security function does the officer think is lacking?** `Integrity`
    
    As the troops got deployed, the leader stressed that they should not communicate their location to anyone while the mission was ongoing. **Which security function did the leader want to have?** \*\*`Confidentiality`
    
    flag: `THM{CIA_TRIAD}`\*\*
    

2\. The attacker managed to gain access to customer records and dumped them online. **What is this attack?** `Disclosure`

3\. A group of attackers were able to locate both the main and the backup power supply systems and switch them off. As a result, the whole network was shut down. **What is this attack?** `Destruction/Denial`

4\. Fundamentals Concepts of Security Models  
  
Which model dictates “***no read down***”? `Biba` (Simple Integrity Property)  
  
Which model dictates “***no read up***”? `Bell-LaPadula` (Simple Security Property)

Which model teaches “***no write down***”? `Bell-LaPadula` (Star Security Property)

Which model forces “***no write up***”? `Biba` (Star Integrity Property)

`THM{SECURITY_MODELS}`

5\. ISO/IEC 19249 Design Principles - use numbers between 1 - 5 to represent the principle in the answers

***Which principle are you applying when you turn off an insecure server that is not critical to the business***? `2` (Attack Surface Minimisation)

Your company hired a new sales representative. **Which principle are they applying when they tell you to give them access only to the company products and prices?** `1` Least Privilege

While reading the code of an ATM, you notice a huge chunk of code to handle unexpected situations such as network disconnection and power failure. **Which principle are they applying?** `5` Preparing for Error and Exception Handling

Thanks for reading through my article. At the moment I hope that I will remain consistent in this learning journey and keep practicing as the days and months come by. You can leave a comment and we can also connect further either on [X](https://x.com/SharonJebitok) or [LinkedIn](https://www.linkedin.com/in/sharon-jebitok/)