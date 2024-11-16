---
title: "Cyber Defense: Summit (TryHackMe)"
datePublished: Sat Nov 16 2024 12:24:19 GMT+0000 (Coordinated Universal Time)
cuid: cm3k53vf7000t09ibddjm0tyn
slug: cyber-defense-summit-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731758012674/44b058cb-e9de-460f-a86a-e38559412949.png
tags: summit, soc-analyst, cyberdefense

---

In this article, I will write a Summit Challenge write-up.

* For a better understanding of this room, you can refer to the [Medium article](https://medium.com/@niceselol/tryhackme-summit-walkthrough-b14cd75fb910).
    

## Objective

After participating in one too many incident response activities, PicoSecure has decided to conduct a threat simulation and detection engineering engagement to bolster its malware detection capabilities. You have been assigned to work with an external penetration tester in an iterative purple-team scenario. The tester will be attempting to execute malware samples on a simulated internal user workstation. At the same time, you will need to configure PicoSecure's security tools to detect and prevent the malware from executing.

Following the **Pyramid of Pain's** ascending priority of indicators, your objective is to increase the simulated adversaries' cost of operations and chase them away for good. Each level of the pyramid allows you to detect and prevent various indicators of attack.

# Room Prerequisites

Completing the preceding rooms in the [Cyber Defence Frameworks module](https://tryhackme.com/module/cyber-defence-frameworks) will be beneficial before venturing into this challenge. Specifically, the following:

* [The Pyramid of Pain](https://tryhackme.com/room/pyramidofpainax)
    
* [MITRE](https://tryhackme.com/room/mitre)
    

# Connection Details

Please click **Start Machine** to deploy the application, and navigate to [https://LAB\_WEB\_URL.p.thmlabs.com](https://lab_web_url.p.thmlabs.com/) once the URL has been populated.

**Note:** It may take a few minutes to deploy the machine entirely. If you receive a "Bad Gateway" response, wait a few minutes and refresh the page.

**Answer the questions below**

1. What is the first flag you receive after successfully detecting **sample1.exe**? `THM{f3cbf08151a11a6a331db9c6cf5f4fe4}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758063400/bc161812-d39c-417c-b6e0-581751b98023.png align="center")
    
2. What is the second flag you receive after successfully detecting **sample2.exe**? `THM{2ff48a3421a938b388418be273f4806d}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758115632/76cb9b2e-27eb-472c-881b-a57793538948.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758137496/9b4fc1e5-3646-4b31-9692-d8e78850dd8c.png align="center")
    
3. What is the third flag you receive after successfully detecting sample3.exe? `THM{4eca9e2f61a19ecd5df34c788e7dce16}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758162841/1c387ab0-236a-4106-85bd-6b88f317ee31.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758255066/43bc5a6b-d51a-4891-b2c5-cc385d7a3a9a.png align="center")
    
4. What is the fourth flag you receive after successfully detecting sample4.exe? `THM{c956f455fc076aea829799c0876ee399}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758302001/ad934568-1e47-44ca-8a1a-f2266b60ecce.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758347089/05a2aaf9-70f1-40ab-be8c-37732406da66.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758407757/09636c25-74b9-4b20-85c7-9acc6ed52b58.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758544018/1a00807a-0fde-4bf5-9714-5e9b962d9487.png align="center")
    
5. What is the fifth flag you receive after successfully detecting sample5.exe? `THM{46b21c4410e47dc5729ceadef0fc722e}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731759527195/61edbd47-f474-477f-a74e-8998e2842441.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731759559711/964b5d5e-8fcb-4ffb-b408-4b6f9ad388cd.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731759627372/ab9910be-b694-40f0-aeec-a1fd4a6cc02a.png align="center")
    
      
      
    
6. What is the final flag you receive from Sphinx? `THM{c8951b2ad24bbcbac60c16cf2c83d92c}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758688260/36ab1a54-4e8e-4076-9e80-9858c3090e04.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758721624/6a7249d6-fb56-408a-99f4-9d2dfeda0438.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731758748898/4531c5b0-c716-4e78-9b7c-e803415c925c.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**