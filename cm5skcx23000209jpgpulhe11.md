---
title: "The Advent of Cyber: Day 17: Log analysis - He analyzed and analyzed till his analyzer was sore! (TryHackMe)"
datePublished: Sat Jan 11 2025 19:12:49 GMT+0000 (Coordinated Universal Time)
cuid: cm5skcx23000209jpgpulhe11
slug: the-advent-of-cyber-day-17-log-analysis-he-analyzed-and-analyzed-till-his-analyzer-was-sore-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736620450004/28555d62-4f88-4e90-ae22-ed9ca472e39a.png
tags: splunk, tryhackme, write-up, loganalysis, adventofcyber2024

---

In this article, we’ll cover Log analysis - He analyzed and analyzed till his analyzer was sore! write-up as the Day 15 challenge of the Advent of Cyber event challenge. It involved navigating the [Splunk](https://www.splunk.com/en_us/products/splunk-enterprise.html) Enterprise and filtering the different events to search and report. We’re still at Wareville for SOC-mas!

## **Investigation Time**

It's time to fire up Splunk, where the data has been pre-ingested for us to investigate the incident. Once the lab is connected, open up the link in the browser and click on **Search & Reporting** on the left.

![Splunk Interface](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732792280322.png align="left")

On the next page, type `index=*` in the search bar to show all ingested logs. Note that we will need to select `All time` as the time frame from the drop-down on the right of the search bar.

![Splunk Search Head](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732792280326.png align="left")

After running the query, we will be presented with two separate datasets pre-ingested to Splunk. We can verify this by clicking on the `sourcetype` field in the fields list on the left of the page.

![Searching in Splunk](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732622163783.png align="left")

The two datasets are as follows:

* `web_logs`: This file contains events related to web connections to and from the CCTV web server.
    
* `cctv_logs`: This file contains information about the CCTV application access logs.
    

Let's explore the logs and investigate the attack on our CCTV servers to identify the culprit, who got unauthorized access to the server and deleted the CCTV streams.

**Examining CCTV Logs**

Let's start our investigation by examining the CCTV logs. To do so, we can either click on the corresponding value for the `sourcetype` field, or type the following query in the search bar:

`index=* sourcetype=cctv_logs`

![Examine the logs in Splunk](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732622555844.png align="left")

## **Understanding the Problem**

After examining the logs, we can figure out the following main issues:

* Logs are not parsed properly by Splunk.
    
* Splunk does not consider the actual timeline of the event; instead, it uses only the ingestion time.
    

## **Fixing the Problem**

Before analysing and investigating the logs, we must extract the relevant fields from them and adjust the timestamp.

The provided logs were generated from a custom log source, so Splunk could not parse the fields properly.

**Extract New Field**

Click on the `Extract New Fields` option, located below the fields list on the left of the page.

![Extract New fields from Logs](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732795521604.png align="left")

**Select Sample Event**

We will be presented with event logs that must be parsed properly. Though, we can select any log, but in order to follow the steps mentioned below, and avoid confusion, let's select the very first sample event and click on the green `Next` button at the top of the page.

![Select Sample Event](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732795521628.png align="left")

**Select Method**

There are two options for extracting the fields: using Regular Expressions and using Delimiters. In this exercise, we will extract fields using Regular Expressions. Select this option and then click on the green `Next` button at the top of the page.

![Select the Method](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732795522203.png align="left")

**Select Fields**

Now, to select the fields in the logs that we want to extract, we simply need to highlight them in the sample log. Splunk will autogenerate the regex (regular expression) to extract the selected field.

![Select Fields](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732624360904.gif align="left")

We'll assign an appropriate name to each of the extracted fields based on the table below:

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>Timestamp</strong></p></td><td colspan="1" rowspan="1"><p><strong>Event</strong></p></td><td colspan="1" rowspan="1"><p><strong>User_id</strong></p></td><td colspan="1" rowspan="1"><p><strong>UserName</strong></p></td><td colspan="1" rowspan="1"><p><strong>Session_id</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p>2024-12-16 17:20:01</p></td><td colspan="1" rowspan="1"><p>Logout</p></td><td colspan="1" rowspan="1"><p>5</p></td><td colspan="1" rowspan="1"><p>byte</p></td><td colspan="1" rowspan="1"><p>kla95sklml7nd14dbosc8q6vop</p></td></tr></tbody></table>

As evident from the preview section, by selecting the fields, Splunk creates a regular expression to extract that field from all the events.

All the extracted fields will be displayed in the Preview tab, as shown below:

![Select Fields](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732796660869.png align="left")

It is important to note that some of the logs may have a different format, and they may not be parsed using the parser we created above. We may have to re-extract the fields from those events. We will get back to fixing this issue later.

We can click on each extracted field to check the extracted values. When we're satisfied with the extracted values, we can click on the green `Next` button at the top of the page.

**Validate**

In the next step, we will see a green tick mark next to the sample logs to indicate the correct extraction of the fields, or a red cross sign to signal an incorrect pattern, as shown below:

![Validate the fields extracted](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732796660741.png align="left")

**Save and Analyse**

After validating that the extracted fields are correct, the next step is saving and analysing the logs.

![Save the Patterns](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732796660777.png align="left")

This tab shows us the regular expression created, the fields extracted, and the sample event that contains the fields we wanted to extract. Let's save this session by clicking on the green `Finish` button at the top of the page and move on to the search tab to search the logs. To do so, we can click on the `Explore the fields I just created in Search` link on the next page.

![Explore the extracted fields](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732798665753.png align="left")

We can verify that we successfully extracted the custom fields from the logs by clicking on any of our custom fields in the list on the left of the page. For example, if we click on the `UserName` field, we'll be presented with all the different values that have been extracted from the logs for this field.

![Search the extracted fields](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732798665743.png align="left")

It also appears that some fields have not been parsed exactly as we expected:

![Search in Splunk](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1733104572467.png align="left")

## **Improving the Field Extraction**

As previously mentioned, some of the logs are a bit different from the ones we used as a baseline for the field extraction. Some of the log formats that our parser could not pick are mentioned below:

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>Sample Log</strong></p></td><td colspan="1" rowspan="1"><p>2024-12-16 23:45:56&nbsp;<strong>Login successful</strong>&nbsp;3&nbsp;marta tktfav3m1mggj0pfjb7onm4qcv</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Sample Log</strong></p></td><td colspan="1" rowspan="1"><p>2024-12-16 22:47:12 Login failed glitch pass=ImtheB3st! rij5uu4gt204q0d3eb7jj86okt</p></td></tr></tbody></table>

It is important to note that, there can be various ways to achieving our goal of fixing the parser. We will try of of the methods, as covered in steps below:

## **Removing the Fields Extraction  
**

Let's go to `Settings` -&gt; `Fields`, as shown below:

![Navigate to Settings -> Fields](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1733105798908.png align="left")

**Field Extraction**

Click on the `Field extractions` tab; it will display all the fields extracted.

![Select Field Extractions Option](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1733105980601.png align="left")

**Delete the Regex Pattern  
**

This tab will display all the patterns/fields extracted so far in Splunk. We can look for the `cctv` related pattern in the list, or simply search `cctv` in the search bar, and it will display our recently created pattern. Once the right pattern is selected, click on the `Delete` button, as shown below.

![Select the pattern and delete.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1733106086822.png align="left")

Why we are deleting this previously created pattern? Well, this regex picks fields from some logs and leave behind other logs, which may be vital for our investigation.  
Our goal is to create one generic regular expression, that works on almost all events.  

**Open Filed Extractor**

Next, click on the `Open Field Extractor` button, and it will take us to the same tab, where we can extract the fields again.

![Open Field Extractor](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1733106631437.png align="left")

**Update the Regex**

This time, after selecting the right source type as `cctv_logs`, and time range as `All Time`, click on `I prefer to write the regular expression myself`.

![Select the source, sourcetype and Time Range](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1733230148279.png align="left")

In the next tab, enter the regex `^(?P<timestamp>\d+\-\d+\-\d+\s+\d+:\d+:\d+)\s+(?P<Event>(Login\s\w+|\w+))\s+(?P<user_id>\d+)?\s?(?P<UserName>\w+)\s+.*?(?P<Session_id>\w+)$` and select `Preview`.

![Update the Regular Expression](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1733233226011.gif align="left")

This regex will fix the field parsing pattern and extract all needed fields from the logs. Hit `Save` and on the next page, select `Finish`.

On the next page, once again, click on the `Explore the fields I just created in Search`.

Now that we can observe that all fields are being extracted as we wanted, let's start investigating the logs.

## **Investigating the CCTV Footage Logs**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/66b36e2379a5d0220fc6b99e/room-content/66b36e2379a5d0220fc6b99e-1732019992566.png align="left")

Now that we have sanitized and properly parsed the logs, it's time to examine them and find the culprit.

**Summary of the CCTV Feed**

After examining the CCTV feed logs, we can create a mental picture of the information these logs provide us. A brief summary of these logs is:

* These logs contain the successful and failed login attempts from various users.
    
* They contain a few failed login attempts, which looks suspicious.
    
* They contain information about the CCTV footage being watched and downloaded.
    

**Event Count by Each User**

Let's use the following search query to see the count of events by each user:

`index=cctv_feed | stats count(Event) by UserName`

We can easily visualise this data by first clicking on `Visualization` below the search bar, then change the visualisation type from **Bar Chart** to **Pie Chart**.

![Show results in Bar Chart](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732644352681.png align="left")

**Summary of the Event Count**

We can create a summary of the event count to see what activities were captured in the logs using the following query:

`index=cctv_feed | stats count by Event`

Splunk will automatically display the previously selected **Pie Chart** type of visualisation.

![Visualize results in PIE chart](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1733109548199.png align="left")

**Examining Rare Events**

Using the following search query, let's look at the events with fewer occurrences in the event field to see if we can find something interesting:

`index=cctv_feed | rare Event`

![Examine the rare Events captured](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1733227064365.png align="left")

It looks like we have a few attempts to delete the recording and a few failed login attempts. This means we have a clue. Let's now examine the failed login attempts first:

`index=cctv_feed *failed* | table _time UserName Event Session_id`

![Create a table of interesting fields](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732648002272.png align="left")

We found some failed login attempts against four users, but one thing remains constant: the Session\_id.

**Narrowing Down Our Investigation**

Let's narrow down our results to see what other events are associated with this `Session_id`:

`index=cctv_feed *put_Session_id_here* | table _time UserName Event Session_id`

![Examine the logs](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732647489220.gif align="left")

Let's see how many events related to the deletion of the CCTV footage were captured.

`index=cctv_feed *Delete*`

![Examine the logs related to Delete Activity](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732648512597.png align="left")

Good. We have some comprehensive information about the attacker and his notorious activities.

**Correlating With the Web Logs**

Let's use the information extracted from the earlier investigation and correlate it with the web logs.

![Narrow Down the results](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732660108101.png align="left")

**Suspicious IP Address**

During the examination, it is observed that only one IP address 10.11.105.33 is associated with the suspicious session ID.

Identify the footprint associated with the session ID.

`index=web_logs *rij5uu4gt204q0d3eb7jj86okt*`

![Find Suspicious IP](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732659354913.png align="left")

Let's narrow down the search to show results associated with the IP address found earlier. It is also important to note that, in this case, the details about the session IDs are found in the field status.

![Narrow Down the result](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732660374066.png align="left")

It looks like two more Session IDs were associated with the IP address found earlier. Let's create a search to observe what kind of activities were captured associated with the IP and these session IDs.

`index=web_logs clientip="10.11.105.33" | table _time clientip status uri ur_path file`

![Examine the Logs](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732660865560.png align="left")

Looking closely, we can see logout events when the session ID was changed. Can we correlate these session IDs in the cctv\_feeds logs and see if we can find any evidence?

**Connecting the Dots**

Let's go back to `cctv_feed` and use these session IDs associated with the IP address, as shown below:

`index=cctv_feed *lsr1743nkskt3r722momvhjcs3*`

![Examine the logs](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/5e8dd9a4a45e18443162feab-1732661207278.png align="left")

Great, we were able to locate the user name associated with the attack. Now that we have identified the user, let's summarise our investigation.

From the output, it seems the following was the timeline of the attack:

* Attacker bruteforce attempt on various accounts.
    
* There was a successful login after the failed attempts.
    
* Attacker watched some of the camera streams.
    
* Multiple camera streams were downloaded.
    
* Followed by the deletion of the CCTV footage.
    
* The web logs had an IP address associated with the attacker's session ID.
    
* We found two other session IDs associated with the IP address.
    
* We correlated back to the cctv\_feed logs to find the traces of any evidence revolving around those session IDs, and found the name of the attacker.
    

1. Extract all the events from the cctv\_feed logs. How many logs were captured associated with the successful login? `642`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736622094716/96ad42a9-2d27-4e58-b06b-5b4ea32125d6.png align="center")
    
2. What is the Session\_id associated with the attacker who deleted the recording? `rij5uu4gt204q0d3eb7jj86okt`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736622253802/91f18759-639b-47d7-8115-96abab356e20.png align="center")
    
3. What is the name of the attacker found in the logs, who deleted the CCTV footage? `mmalware`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736622329029/e36a768e-d9de-4f3e-adcc-6f018fb57595.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736622367522/56e71420-06ba-435d-b9f4-0b1cc2d080d2.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736622405320/67e4bc82-b24c-4d1c-a628-a83f3d60c638.png align="center")
    
4. Check out the [Splunk: Data Manipulation](https://tryhackme.com/jr/splunkdatamanipulation) room to learn more about parsing and manipulating data in Splunk.
    
5. Good thing we had a backup of the CCTV application from yesterday. We got it running again in no time!
    

Thank you for reading this article. Please leave a comment with your thoughts, areas for improvement, other suggestions, and questions. Stay secure until the next one!