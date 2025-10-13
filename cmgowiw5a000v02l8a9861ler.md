---
title: "Shadow Trace"
datePublished: Mon Oct 13 2025 08:59:04 GMT+0000 (Coordinated Universal Time)
cuid: cmgowiw5a000v02l8a9861ler
slug: shadow-trace
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760345606411/e90a6d08-878d-4834-81ce-0c20c30776c2.png
tags: windows, dfir, malware-analysis, encoder-decoder, tryhackme-walkthrough

---

It’s the graveyard shift, and you’re the lone analyst in the SOC when an urgent call comes in: a user’s machine dropped a suspicious binary named `windows-update.exe`. At first glance, it masquerades as a routine updater, but alerts from the EDR and oddities in the file contents suggest something far more sinister. This exercise places you in that real-world pressure cooker: triage fast, collect reliable indicators, and decide whether to escalate or contain.

Throughout this lab, you’ll practice core SOC triage workflows—static binary inspection, IOC extraction, and alert correlation—while answering targeted questions that mirror daily incident-response tasks. By the end, you’ll be able to identify network IOCs, compute and record file hashes, read process-level triggers, and translate encoded artifacts into actionable intelligence. Think of it as hands-on experience for the moment when speed, precision, and context matter most.

##   
Scenario

It’s the middle of the night shift. You’re the only analyst in the SOC when a manager calls in urgently: a suspicious file was found on a user's machine and needs **immediate review**.

You open the file and start digging. Something doesn’t look normal for a company updater, and at the same time, the EDR throws a couple of alerts.

Your task: analyse the file, collect anything to identify it, gather any potential IOCs, correlate and analyse the alerts for potential malicious behaviour. It’s up to you to piece together what’s happening before it spreads further.

## **Learning Objectives**

* Extract IOCs from suspicious binaries
    
* Correlate alerts with malicious activity
    
* Perform basic SOC triage actions
    

## **Prerequisites**

* [Introduction to Malware Analysis](https://tryhackme.com/room/intromalwareanalysis)
    
* [MAL:Malware Introductory](https://tryhackme.com/room/malmalintroductory)
    
* [Malware Classification](https://tryhackme.com/room/malwareclassification)
    

Follow the instructions for the next tasks to start.

## File analysis

Analyse the binary located `C:\Users\DFIRUser\Desktop\windows-update.exe` in the attached machine, answer the questions below.   
  
Start the lab by clicking the Start Machine button. It will take around 2 minutes to load properly. The VM will be accessible on the right side of the split screen.

You can find several tools installed in the machine that can help you with any kind of analysis under `C:\Users\DFIRUser\DFIR Tools`

### Answer the questions below

1. What is the architecture of the binary file windows-update.exe?  
      
    `sigcheck.exe C:\Users\DFIRUser\Desktop\windows-update.exe`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760344679252/2867ea6a-7c47-445c-8f30-5ab57a50a28d.png align="center")
    
      
    
2. What is the hash (sha-256) of the file windows-update.exe?  
      
    `Get-FileHash -Algorithm SHA256 "C:\Users\DFIRUser\Desktop\windows-update.exe"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760344727080/a10b4df9-b981-4c2e-9c52-1fa950b0a522.png align="center")
    
3. Identify the URL within the file to use it as an IOC
    
    ```bash
    $bytes = Get-Content -Path "C:\Users\DFIRUser\Desktop\windows-update.exe" -Encoding Byte
    $text  = [System.Text.Encoding]::ASCII.GetString($bytes)
    [regex]::Matches($text, 'https?://[^\s"''<>\\]+') | ForEach-Object { $_.Value } | Sort-Object -Unique
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760344778838/9580f533-a309-436b-83f4-4162f8476df3.png align="center")
    
4. With the URL identified, can you spot a domain that can be used as an IOC?  
      
    `strings "C:\Users\DFIRUser\Desktop\windows-update.exe" | findstr /i ".com"`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760344846927/5f2449da-0acc-4d9c-941a-a6bf56571392.png align="center")
    
5. Input the decoded flag from the suspicious domain  
      
    decode the base64 code
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760344964971/8d8ba6ce-6972-41fc-bb0c-22c04b8537cb.png align="center")
    
6. What library related to socket communication is loaded by the binary?  
      
    searched the hash found above on Virustotal and found the decoded flag under the imports
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760345198473/89fbc98e-bea9-403c-85aa-65ecf19d1f57.png align="center")
    

## Alerts Analysis

Click on the **View Site** button attached to this task to display the static site in split view. Review the alternatives and answer the questions below.

Alternatively, if you can not see all the columns in split view, you can open the static site in full screen by clicking the link below:

![THM Key Credentials](https://tryhackme-images.s3.amazonaws.com/user-uploads/63588b5ef586912c7d03c4f0/room-content/be629720b11a294819516c1d4e738c92.png align="left")

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>ACCESS</strong></p></td><td colspan="1" rowspan="1"><p><a target="_blank" rel="noopener noreferrer nofollow" href="https://static-labs.tryhackme.cloud/apps/shadow-trace/" style="pointer-events: none">Interactive Exercise</a></p></td></tr></tbody></table>

### Answer the questions below  

For this section, data encoding and cryptography will be of help to know which encoding or decoding is used, and an easy way to decode it to plain text in order to find the expected answer. Personally, I used Cyberchef and Base64 sites to decode them.  

1. Can you identify the malicious URL from the trigger by the process powershell.exe?  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760345253824/74efffe5-3789-4a13-bb38-ee5e74535296.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760345271612/ef33ed10-8eaa-40ca-87f2-cab8710bb655.png align="center")
    
2. Can you identify the malicious URL from the alert triggered by chrome.exe?
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760345514528/a0e54e6c-b065-49d8-879d-56646743b1d6.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1760345526207/1603a6e3-35be-4930-9a46-44bff3fe66f3.png align="center")
    
      
    
3. What's the name of the file saved in the alert triggered by chrome.exe?  
      
    The answer is just there on the command for chrome.exe
    

This exercise reinforces the essentials of early-stage incident response: verify file provenance, extract trustworthy indicators from binaries, and correlate telemetry from endpoints and network sensors to form a coherent hypothesis. A malicious-looking updater can quickly turn into a lateral propagation or data-exfiltration incident if missed—so collecting SHA-256 hashes, embedded URLs, imported libraries (like Winsock), and related process activity isn’t optional, it’s critical.

Beyond the technical checks, the most valuable outcome is practice: the ability to triage under pressure, prioritize containment (isolate host, block domain/IP, collect artifacts), and document findings for escalation. With the IOCs and alerts from this lab, you’re equipped to drive a defensible response—submit artifacts to intel platforms, update detection rules, and harden controls so the next “fake update” fails to find purchase.