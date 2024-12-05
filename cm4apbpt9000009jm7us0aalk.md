---
title: "The Advent of Cyber: Day 2: Log Analysis - One Man's False Positive is Another Man's Potpourri (TryHackMe)"
datePublished: Thu Dec 05 2024 02:32:18 GMT+0000 (Coordinated Universal Time)
cuid: cm4apbpt9000009jm7us0aalk
slug: the-advent-of-cyber-day-2-log-analysis-one-mans-false-positive-is-another-mans-potpourri-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733365847097/de5c5efe-c9c2-401b-8053-1bc5f97b8ba0.png
tags: tryhackme, write-up, siem, loganalysis, adventofcyber2024, elastic-security

---

In this article, we’ll cover the Log Analysis—One Man's False Positive is Another Man's Potpourri writeup as the Day 2 challenge of the Advent of Cyber event challenge. It was interesting to navigate the platform and filter different events and logs based on timestamps, IP addresses, and the events, i.e., authentication, process, etc. We’re still at Wareville for SOC-mas!  
  
The use-case website: [Elastic Security](https://www.elastic.co/security) just for learning purposes. **Elastic Security combines Elastic SIEM**, whose detection engine automates threat detection so you can quickly investigate and respond to threats, and Endpoint Security into a single solution that unifies prevention, detection, and response across your entire network.

Start your machine within the Day 2 challenge and follow the description and given steps. To start we’ll visit our browser within the machine and open the URL that is given based on your IP\_ADDRESS, use the given username & password. Once on the platform we’ll select Discover and start setting the date as guided. It would be advisable to expand the machine in order to have a new tab to get a proper view of the site and filter better.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364126180/8703b8a5-65f6-4ac7-9951-c0c51446c7a2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364248416/829500ea-20ae-433b-bd2a-dc2e2162b564.png align="center")

Just follow the different filter options and selected fields that are important in order to help you solve the challenge or access what’s required. In the case below the date that’s limited to Dec 1, 2024, from 9 am to 9:30 am  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364275800/e6d5db87-1ac9-40a5-8aa6-b049d688a4ac.png align="center")

  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364303812/85a6e9de-b8dd-40fe-8bdb-0ce84f082e24.png align="center")

Here the filtered field is `event.category: authentication`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364327599/b30f7fa4-77c1-470a-a193-d83dbd4b1d1e.png align="center")

Here the filtered `source.ip: 10.0.11.11` and `user.name: service_admin`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733365155695/d016eee7-ff6e-4cb1-9004-df38a625367c.png align="center")

Here we remove the previously filtered source.ip in order to remain with the `user.name: service_admin` and `event.category: authentication`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733365218148/c223062e-afe9-449a-a69d-3266e807eccb.png align="center")

  

1. What is the name of the account causing all the failed login attempts? `service_admin`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364417839/8fb50c0c-a306-4db0-9ba8-145b325345b5.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364445936/9edf9bf0-4920-462f-9427-2e93feb2fe38.png align="center")
    
2. How many failed logon attempts were observed? `6791`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364470846/903246f9-390b-4d9e-b9db-8d6c3ccf6c64.png align="center")
    
3. What is the IP address of Glitch? `10.0.255.1`
    
4. When did Glitch successfully logon to ADM-01? Format: MMM D, YYYY HH:MM:SS.SSS `Dec 1, 2024 08:54:39.000`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364518496/57c8526c-a1f7-417a-a88b-ab7116782c4a.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364538686/71368d04-8d66-45b2-91e2-afa63bd56069.png align="center")
    
5. What is the decoded command executed by Glitch to fix the systems of Wareville? `Install-WindowsUpdate -AcceptAll -AutoReboot`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364634346/19dce704-b9a2-43e7-9eac-411b68f0a9c6.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733364650504/65e66776-421a-4e56-8feb-b2bb7696795c.png align="center")
    
    use CyberChef to bake from ***Base64 and Decode Text UTF-16LE***(1200).
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733365031251/fc702eb2-5fae-4e6c-82cd-a02f8e606ce9.png align="center")
    
      
    Thank you for reading through this article. You can leave a comment with your thoughts: areas to improve or other suggestions and questions if any. Till the next one, stay secure!