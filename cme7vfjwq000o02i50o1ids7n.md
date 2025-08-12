---
title: "Challenges: The Greenholt Phish (TryHackMe)"
datePublished: Fri Aug 08 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cme7vfjwq000o02i50o1ids7n
slug: challenges-the-greenholt-phish-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754962529058/c6bc6c32-9253-4577-8d3b-e4f1025c2656.png
tags: phishing, tryhackme, write-up, soc-analyst, emailphishing

---

Just another day in the SOC often begins with an unexpected alert — this time, a suspicious email reported by a Sales Executive at Greenholt PLC. The email contained unusual language, an unexpected reference to a money transfer, and an unsolicited attachment. Given the potential for phishing or malware delivery, the Security Operations Center (SOC) initiated a full investigation to verify the legitimacy of the message, analyze the attachment, and assess any potential threat to the organization’s systems and data.

## Just another day as a SOC Analyst

![ ](https://tryhackme-images.s3.amazonaws.com/user-uploads/5c549500924ec576f953d9fc/room-content/c91d2c9c87ca3363e7386033ed327839.png align="left")

A Sales Executive at Greenholt PLC received an email that he didn't expect to receive from a customer. He claims that the customer never uses generic greetings such as "Good day" and didn't expect any amount of money to be transferred to his account. The email also contains an attachment that he never requested. He forwarded the email to the SOC (Security Operations Center) department for further investigation. 

Investigate the email sample to determine if it is legitimate. 

Deploy the Machine

Deploy the machine attached to this task; it will be visible in the **split-screen** view once it is ready.

If you don't see a virtual machine automatically appear, click the **Show Split View** button.

**Tip**: Open the EML file with Thunderbird. To do so, **right-click** on the `challenge.eml` file and select **Open With Other Application**. From there, scroll down to select **Thunderbird Mail** and click **Open**. It may take a few moments to open the application. You will then see the email and its contents appear in the app.

![Right-click window on the eml file](https://tryhackme-images.s3.amazonaws.com/user-uploads/6490641ea027b100564fe00a/room-content/94dfe6b382c9f66853114ec75d630df1.png align="left")

### Answer the questions below

1. What is the **Transfer Reference Number** listed in the email's **Subject**? `09674321`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754961011443/49403c66-41c3-4833-b5bb-58c2bad8be7c.png align="center")
    
2. Who is the email from? `Mr. James Jackson`
    
3. What is his email address?
    
4. What email address will receive a reply to this email? 
    
5. What is the Originating IP?  
      
    checked the source of section of the email inorder to get the x-originating-IP, return-path, among other details which were helpful in answering the next set of questions  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754961034375/c64c2b37-e8d3-472f-b917-3f166a3d2d7a.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754960926366/ec430fe6-eafc-4908-87ba-db8faed05d6c.png align="center")
    
6. Who is the owner of the Originating IP? (Do not include the "." in your answer.)  
      
    using the originating IP found in the previous question we used the whois command to find the organization name or the owner.  
      
    `whois IP_Address`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754960905998/a08e792a-7335-404a-b226-233aecd0d84f.png align="center")
    
7. What is the SPF record for the Return-Path domain?  
      
    got the domain by extracting it from the email provided. Used the dig command to find the SPF record  
      
    `dig TXT mutawamarine.com`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754960878176/6315d7fa-0e76-44c5-af24-fe3bbb0653a7.png align="center")
    
8. What is the DMARC record for the Return-Path domain?  
      
    to find the DMARC record we had prepend \_dmarc on the domain and used the dig, and nslookup commands to find the record.  
      
    `dig TXT _dmarc.mutawamarine.com`  
      
    `nslookup -type=TXT _dmarc.mutawamarine.com`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754960854780/88a95890-72c9-44c7-b059-f1fdd4bad24a.png align="center")
    
9. What is the name of the attachment?  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754960586751/6eefc240-c38a-4f0e-9014-0e4ea762e503.png align="center")
    
10. What is the SHA256 hash of the file attachment?  
      
    tried to use 7z to extract files from the file attachment but later on found that it was unnecessary as the original attachment is the one that revealed the right SHA256 hash  
      
    `sha256sum Attachment_File_Name`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754960567144/287d4aa2-b192-449a-8e76-8d190b41ec83.png align="center")
    
11. What is the attachments file size? (Don't forget to add "KB" to your answer, **NUM KB**)  
      
    the hint suggested using an open-source tool to find the details of this question. I searched the hash on Virus Total and it revealed the size in KB.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754960548492/804d921d-cca4-40da-8244-77523be482fa.png align="center")
    
12. What is the actual file extension of the attachment?  
      
    still using virus total to search the hash under the details section the basic properties of the file are shown including the file type.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754960523031/957a80b4-702f-4278-8087-080a8948f63b.png align="center")
    
      
    The investigation confirmed that the email was malicious. Analysis of the email headers exposed the originating IP, revealing its true owner, and DNS queries of the Return-Path domain uncovered both SPF and DMARC records inconsistent with legitimate communications from the alleged sender. The attachment, when hashed and reviewed in VirusTotal, showed a suspicious executable file disguised as a document, further confirming the phishing attempt. By following email header forensics, DNS record analysis, and file hashing procedures, the SOC was able to identify and contain the threat before any damage occurred — another example of proactive incident response stopping a phishing attack in its tracks.