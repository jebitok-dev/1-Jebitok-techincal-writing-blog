---
title: "Web Attack Forensics - Drone Alone
 (TryHackMe) 🎉🧑‍🎄"
datePublished: Mon Dec 15 2025 21:10:33 GMT+0000 (Coordinated Universal Time)
cuid: cmj7ne8o5000002l45bofbgdm
slug: web-attack-forensics-drone-alone-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765832960165/d97e2c93-e26b-482a-a126-77273dcb48ae.png
tags: web-security, splunk, tryhackme, blueteam, adventofcyber2025

---

Modern web applications are a prime target for attackers because they sit at the boundary between users and internal systems. A single vulnerable endpoint can act as a gateway from harmless-looking HTTP traffic into full operating system compromise. Defending against these attacks requires visibility across multiple layers — from web server requests to the processes running on the host itself.

Splunk plays a critical role in web security by acting as a centralized platform where logs from different sources can be correlated in near real time. By ingesting Apache access and error logs alongside host-based telemetry such as Sysmon, defenders can move beyond isolated alerts and instead reconstruct attacker behaviour step by step. Obfuscated payloads, unusual request patterns, and unexpected child processes become much easier to spot when viewed through this combined lens.

In this investigation, Splunk enables a Blue Team approach to incident response: detecting suspicious web requests, validating whether exploitation occurred, uncovering attacker reconnaissance, and decoding hidden payloads. Rather than relying on a single indicator, the analysis demonstrates how layered logging and correlation turn fragmented evidence into a coherent attack narrative.

## Introduction

**The Story**

![Task banner for day 15](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1763538536985.png align="left")

TBFC’s drone scheduler web UI is getting strange, long HTTP requests containing Base64 chunks. Splunk raises an alert: “Apache spawned an unusual process.” On some endpoints, these requests cause the web server to execute shell code, which is obfuscated and hidden within the Base64 payloads. For this room, your job as the Blue Teamer is to triage the incident, identify compromised hosts, extract and decode the payloads and determine the scope.

You’ll use Splunk to pivot between web (Apache) logs and host-level (Sysmon) telemetry.

Follow the investigation steps below; each corresponds to a Splunk query and investigation goal.

## **Learning Objectives**

* Detect and analyze malicious web activity through Apache access and error logs
    
* Investigate OS-level attacker actions using Sysmon data
    
* Identify and decode suspicious or obfuscated attacker payloads
    
* Reconstruct the full attack chain using Splunk for Blue Team investigation
    

## Web Attack Forensics

## **Logging into Splunk**

After you have started the AttackBox and the target machine in the previous task, allow the system around 3 minutes to fully boot, then use Firefox on the AttackBox to access the Splunk dashboard at `http://MACHINE_IP:8000` using the credentials below.

**Credentials**

To access Splunk dashboard

Username

**Blue** 

Password

**Pass1234** 

IP address

**MACHINE\_IP** 

Connection via

**HTTPhttp://MACHINE\_IP:8000** 

The Splunk login page should look similar to the screenshot shown below.

![Splunk login screen](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1761554754985.png align="left")

After logging in successfully, you will be taken to the Search Page as shown in the screenshot below.

![Splunk search dashboard](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1761554754946.png align="left")

Make sure to adjust the Splunk time range to include the time of the events (e.g., "Last 7 days" or "All time"). If the default range is too narrow, you may see "*No results found."*  
  
A Blue Teamer would explore various attack angles via Splunk. In this task, we will follow elf Log McBlue, who uses his Splunk magic to unravel the attack path.

## **Detect Suspicious Web Commands**

In the first step, let’s search for HTTP requests that might show malicious activity. The query below searches the **web access logs** for any HTTP requests that include signs of command execution attempts, such as `cmd.exe`, `PowerShell`, or `Invoke-Expression`. This query helps identify possible **Command Injection attacks**, where the evil attacker tries to execute system commands through a vulnerable CGI script (`hello.bat`).

`index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression") | table _time host clientip uri_path uri_query status`

![Results of the Splunk query.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1761554754850.png align="left")

At this step, we are primarily interested in base64-encoded strings, which may reveal various types of activities. Once you spot encoded PowerShell commands, decode them using [base64decode.org](https://www.base64decode.org/) or your favourite base64 decoder to understand what the attacker was trying to do. Based on the results we received, let’s copy the encoded PowerShell string `VABoAGkAcwAgAGkAcwAgAG4AbwB3ACAATQBpAG4AZQAhACAATQBVAEEASABBAEEASABBAEEA` and paste it into [https://www.base64decode.org/](https://www.base64decode.org/) upper field, then click on decode as shown in the screenshot below.

![Using www.base64decode.org to decode a base64 string.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1761554754871.png align="left")

## **Looking for Server-Side Errors or Command Execution in Apache Error Logs**

In this stage, we will focus on inspecting web server error logs, as this would help us uncover any malicious activity. We will use the following query:

`index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")`

This query inspects the **Apache error logs** for signs of execution attempts or internal failures caused by malicious requests. As you can tell, we are searching for error messages with particular terms such as `cmd.exe` and `powershell`.

Please make sure you select `View: Raw` from the dropdown menu above the `Event` display field.

![Results of the Splunk query.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1761554754890.png align="left")

If a request like `/cgi-bin/hello.bat?cmd=powershell` triggers a 500 “Internal Server Error,” it often means the attacker’s input was processed by the server but failed during execution, a key sign of exploitation attempts.

Checking these results helps confirm whether the attack **reached the backend** or remained blocked at the web layer.

## **Trace Suspicious Process Creation From Apache**

Let’s explore Sysmon for other malicious executable files that the web server might have spawned. We will do that using the following Splunk query:

`index=windows_sysmon ParentImage="*httpd.exe"`

This query focuses on **process relationships** from Sysmon logs, specifically when the **parent process is Apache** (`httpd.exe`).

Select `View: Table` on the dropdown menu above the `Event` display field.

![Results of the Splunk query.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1761554754951.png align="left")

Typically, Apache should only spawn worker threads, not system processes like `cmd.exe` or `powershell.exe`.

If results show child processes such as:

`ParentImage = C:\Apache24\bin\httpd.exe`

`Image        = C:\Windows\System32\cmd.exe`

It indicates a successful **command injection** where Apache executed a system command.

The finding above is one of the strongest indicators that the web attack penetrated the operating system.

## **Confirm Attacker Enumeration Activity**

In this step, we aim to discover what specific programs we found from previous queries do. Let’s use the following query.

`index=windows_sysmon *cmd.exe* *whoami*`

This query looks for **command execution logs** where `cmd.exe` ran the command `whoami`.

Attackers often use the `whoami` command immediately after gaining code execution to determine which user account their malicious process is running as.

Finding these events confirms the attacker’s **post-exploitation reconnaissance**, showing that the injected command was executed on the host.

![Results of the Splunk query.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1761554755110.png align="left")

## **Identify Base64-Encoded PowerShell Payloads**

In this final step, we will work to find all successfully encoded commands. To search for encoded strings, we can use the following Splunk query:

`index=windows_sysmon Image="*powershell.exe" (CommandLine="*enc*" OR CommandLine="*-EncodedCommand*" OR CommandLine="*Base64*")`

This query detects PowerShell commands containing -EncodedCommand or Base64 text, a common technique attackers use to **hide their real commands**.

If your defences are correctly configured, this query should return **no results**, meaning the encoded payload (such as the “Muahahaha” message) never ran.

If results appear, you can decode the Base64 command to inspect the attacker’s true intent.

![Results of the Splunk query.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1761554755100.png align="left")

### Answer the questions below

* What is the reconnaissance executable file name?  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765831510689/acffe607-e6e9-476d-aa7c-d61a455c6618.png align="center")
    
    I set the time and date range to all time to help show events
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765831542335/b67b4397-7d7b-4ed5-8d06-bf25d06432c7.png align="center")
    
    `index=windows_sysmon *cmd.exe* *whoami*`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765831568683/01068531-3bdb-4505-a2f5-6f84e8d412f0.png align="center")
    
* 2. What executable did the attacker attempt to run through the command injection?  
          
        `index=windows_apache_access (cmd.exe OR powershell OR "powershell.exe" OR "Invoke-Expression") | table _time host clientip uri_path uri_query status`  
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765831660544/0ffa4a51-b113-4852-8be7-d9216fbd435c.png align="center")
    
    Decoding `VABoAGkAcwAgAGkAcwAgAG4AbwB3ACAATQBpAG4AZQAhACAATQBVAEEASABBAEEASABBAEEA` on [Base64Decode](https://www.base64decode.org/) shows us the text below  
      
    `This is now Mine! Muahaahaa`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765831676279/08daf8a3-eedb-4c9d-ad30-b05caf8a8bf8.png align="center")
    
    `index=windows_apache_error ("cmd.exe" OR "powershell" OR "Internal Server Error")`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765831686067/8231a27f-9197-4678-afb6-5cbfcebb6485.png align="center")
    
    `index=windows_sysmon ParentImage="*httpd.exe"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765831699301/fb6c48b0-cd3b-4cc8-964c-fe74ea6fd63f.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765831715318/95f4e65c-2022-4f8b-838f-204850f1007a.png align="center")
    
      
    This challenge highlights how effective web attack detection depends on correlating events across both the application and the host. Malicious HTTP requests alone do not always confirm compromise, but when paired with Sysmon data showing Apache spawning command-line or PowerShell processes, the picture becomes clear. Splunk’s ability to pivot between these data sources allows defenders to confidently distinguish probing attempts from successful exploitation.
    
* The use of Base64-encoded payloads in the attack demonstrates a common evasion technique designed to bypass simple signature-based detection. However, with proper visibility into command-line execution and process ancestry, even obfuscated activity can be uncovered and decoded. Identifying post-exploitation behaviour such as `whoami` execution further confirms attacker intent and helps define the scope of compromise.
    
* Ultimately, this investigation reinforces the importance of comprehensive logging and correlation in web security. Tools like Splunk empower defenders to trace the full attack chain — from the initial web request to OS-level execution — enabling faster triage, more accurate impact assessment, and stronger defensive improvements against future attacks.  
      
    
*