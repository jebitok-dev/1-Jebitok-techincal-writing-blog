---
title: "The Advent of Cyber: Day 3: Log Analysis - Even if I wanted to go, their vulnerabilities wouldn't allow it (TryHackMe)"
datePublished: Sat Jan 11 2025 11:02:00 GMT+0000 (Coordinated Universal Time)
cuid: cm5s2tq7d000i09jp5rqxczpb
slug: the-advent-of-cyber-day-3-log-analysis-even-if-i-wanted-to-go-their-vulnerabilities-wouldnt-allow-it-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736593235801/cc2e161b-1472-4bc7-b30f-a47f7fc9a3d7.png
tags: tryhackme, write-up, loganalysis, adventofcyber2024

---

In this article, we’ll cover the Log Analysis. Even if I wanted to go, their vulnerabilities wouldn't allow me to write up the log analysis as the Day 3 challenge of the Advent of Cyber event challenge. It was interesting to navigate the [Elastic Search](https://www.elastic.co/elasticsearch), [Logstash](https://www.elastic.co/logstash), and [Kibana](https://www.elastic.co/kibana) (ELK) platform and filter different events and logs based on timestamps, IP addresses, and the events, i.e., authentication, process, etc, to investigate a web attack with ELK. We’re still at Wareville for SOC-mas!

Remember, to access the Frosty Pines Resorts website ([http://frostypines.thm](http://frostypines.thm)), you must reference it in your host’s file. On the AttackBox, this can be done by executing the following command in a terminal: `echo "MACHINE_IP frostypines.thm" >> /etc/hosts`

**BLUE**: Where was the web shell uploaded to?

1. **Answer format:** /directory/directory/directory/filename.php `/media/images/rooms/shell.php`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736592624276/dd9bcf89-f730-46bb-9d16-4f3ed8823892.png align="center")
    
2. **BLUE**: What IP address accessed the web shell? `10.11.83.34`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736592650382/c9cb8997-65b2-4b2f-b32d-12e853af75bf.png align="center")
    
3. **RED**: What is the contents of the flag.txt? `THM{Gl1tch_Was_H3r3}`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736592705760/21093554-86e7-4153-802b-7101466e864f.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736593040530/25ad0170-cda6-438f-b38b-cc9817449a38.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736593089933/93c7c611-84b0-4a63-b310-9bc1ae0450d1.png align="center")
    
4. If you liked today's task, you can learn how to harness the power of [advanced ELK queries](https://tryhackme.com/jr/advancedelkqueries).
    

This [article](https://medium.com/@rahulhoysala07/tryhackme-advent-of-cyber-2024-day-3-writeup-f2c8dd7f49a4) was of help while going through this challenge.

Thank you for reading this article. Please leave a comment with your thoughts, areas for improvement, other suggestions, and questions. Stay secure until the next one!