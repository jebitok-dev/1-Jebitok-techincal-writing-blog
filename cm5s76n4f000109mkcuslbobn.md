---
title: "The Advent of Cyber: Day 7: AWS log analysis - Oh, no. I'M SPEAKING IN CLOUDTRAIL! (TryHackMe)"
datePublished: Sat Jan 11 2025 13:04:01 GMT+0000 (Coordinated Universal Time)
cuid: cm5s76n4f000109mkcuslbobn
slug: the-advent-of-cyber-day-7-aws-log-analysis-oh-no-im-speaking-in-cloudtrail-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736598937967/6972cecc-cadd-4ae8-bfc0-7b82e375b9fe.png
tags: aws, tryhackme, aws-cloudtrail, write-up, adventofcyber2024

---

In this article, we’ll cover the AWS log analysis - Oh, no. I'M SPEAKING IN CLOUDTRAIL! write-up as the Day 7 challenge of the Advent of Cyber event challenge. It was interesting to monitor an AWS Environment i.e. [AWS CloudWatch](https://aws.amazon.com/cloudwatch/), [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) ([S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/cloudtrail-logging.html) & [IAM](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/security_iam_service-with-iam.html)), and [Intro to JQ - Command line JSON](https://www.baeldung.com/linux/jq-command-json) using the command line. We’re still at Wareville for SOC-mas!

1. What is the other activity made by the user glitch aside from the ListObject action? `PutObject`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736599349402/2b6a6abd-4fc8-4d5b-b28d-e369e6fbc83f.png align="center")
    
2. What is the source IP related to the S3 bucket activities of the user glitch? `53.94.201.69`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736599397307/f46be01a-cb96-452e-a7c7-2c9834e22931.png align="center")
    
3. Based on the eventSource field, what AWS service generates the ConsoleLogin event? `signin.amazonaws.com`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736599440855/47a5b39e-26f3-4669-88a9-0121246ed474.png align="center")
    
4. When did the anomalous user trigger the ConsoleLogin event? `2024-11-28T15:21:54Z`  
      
    (based on the image above)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736599545451/a42fdbb7-8212-4b01-9d3c-78db0f33277b.png align="center")
    
5. What was the name of the user that was created by the mcskidy user? `glitch`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736599674114/72f863be-6712-493b-9a5e-2f26fb085a1f.png align="center")
    
6. What type of access was assigned to the anomalous user? `AdministratorAccess`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736599831887/c5e7ccfc-defe-4f72-932b-1a28e7dce699.png align="center")
    
7. Which IP does Mayor Malware typically use to log into AWS? `53.94.201.69`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736599781467/f0e31712-4676-4eff-8897-4d4a442ba6d7.png align="center")
    
8. What is McSkidy's actual IP address? `31.210.15.79`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736599963789/993211fe-8ddf-47cf-b68d-e327ad8053a7.png align="center")
    
9. What is the bank account number owned by Mayor Malware? `2394 6912 7723 1294`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736599996116/c27da529-2520-46c3-8c84-184e81c54666.png align="center")
    
10. Want to learn more about log analysis and how to interpret logs from different sources? Check out [the Log Universe](https://tryhackme.com/r/room/loguniverse) room!
    

Thank you for reading this article. Please leave a comment with your thoughts, areas for improvement, other suggestions, and questions. Stay secure until the next one!