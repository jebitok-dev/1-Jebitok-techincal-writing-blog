---
title: "Defensive Security Tooling: CAPA: The Basics (TryHackMe)"
datePublished: Mon Nov 04 2024 14:40:27 GMT+0000 (Coordinated Universal Time)
cuid: cm334oq8v000009mh3had7xmb
slug: defensive-security-tooling-capa-the-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730710180303/6f739245-70b9-4f68-86df-68f1054cc324.png
tags: cybersecurity-1, tryhackme, write-up, capa, defensive-security

---

In this article, I will write a write-up for CAPA: The Basics that covers Tool Overview: How CAPA Works, Dissecting CAPA Results Part 1: General Information,, MITRE and MAEC, Dissecting CAPA Results Part 2: Malware Behavior Catalogue, Dissecting CAPA Results Part 3: Namespaces, Dissecting CAPA Results Part 4: Capability and More Information, More fun!.

1. What command-line option would you use if you need to check what other parameters you can use with the tool? Use the shortest format. `-h`
    
2. What command-line options are used to find detailed information on the malware's capabilities? Use the shortest format. `-v`
    
3. What command-line options do you use to find very verbose information about the malware's capabilities? Use the shortest format. `-vv`
    
4. What PowerShell command will you use to read the content of a file? `Get-Content`
    
5. What is the sha256 of cryptbot.bin? `ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730710248993/b4b84665-4086-4139-9c37-827b3e34f8cb.png align="center")
    
6. What is the **Technique** Identifier of **Obfuscated Files or Information**? `T1027`
    
7. What is the **Sub-Technique** Identifier of **Obfuscated Files or Information::Indicator Removal from Tools**? `T1027.005`
    
8. When CAPA tags a file with this MAEC value, it indicates that it demonstrates behaviour similar to, but not limited to, **Activating persistence mechanisms**? `launcher`
    
9. When CAPA tags a file with this MAEC value, it indicates that the file demonstrates behaviour similar to, but not limited to, **Fetching additional payloads or resources from the internet**? `Downloader`
    
10. What serves as a catalogue of malware objectives and behaviours? `Malware Behavior Catalogue`
    
11. Which field is based on ATT&CK tactics in the context of malware behaviour? `Objective`
    
12. What is the Identifier of **"Create Process**" micro-behavior? `C0017`
    
13. What is the behaviour with an Identifier of **B0009**? `Virtual Machine Detection`
    
14. Malware can be used to obfuscate data using base64 and XOR. What is the related **micro-behavior** for this? `Encode Data`
    
15. Which micro-behavior refers to "**Malware is capable of initiating HTTP communications**"? `HTTP Communication`
    
16. Which top-level Namespace contains a set of rules specifically designed to detect behaviours, including obfuscation, packing, and anti-debugging techniques **exhibited by malware to evade analysis**? `anti-analysis`
    
17. Which namespace contains rules to **detect virtual machine (VM) environments**? Note that this is not the TLN or Top-Level Namespace. `anti-vm/vm-detection`
    
18. Which Top-Level Namespace contains rules related to **behaviours associated with maintaining access or persistence within a compromised system**? This namespace is focused on understanding how malware can establish and maintain a presence within a compromised environment, allowing it to persist and carry out malicious activities over an extended period. `persistence`
    
19. Which namespace addresses techniques such as **String Encryption, Code Obfuscation, Packing, and Anti-Debugging Tricks**, which conceal or obscure the true purpose of the code? `obfuscation`
    
20. Which Top-Level Namespace Is a **staging ground** for rules that are not quite polished? `Nursery`
    
21. What **rule yaml file** was matched if the Capability or rule name is **check HTTP status code**? `check-http-status-code.yml`
    
22. What is the **name of the Capability** if the rule YAML file is `reference-anti-vm-strings.yml`? `reference anti-VM strings`
    
23. Which **TLN** or Top-Level Namespace includes the Capability or rule name **run PowerShell expression**? `load-code`
    
24. Check the conditions inside the `check-for-windows-sandbox-via-registry.yml` rule file from this [link](https://github.com/MBCProject/capa-rules-1/blob/master/anti-analysis/anti-vm/vm-detection/check-for-windows-sandbox-via-registry.yml). What is the **value of the API** that ends in `Ex` is it looking for? `RegOpenKeyEx`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730710229958/95ddef14-2c74-4a53-9aec-ef83f2e65588.png align="center")
    
25. Which parameter allows you to output the result of CAPA into a **.json file**? `-j`
    
26. What tool allows you to interactively explore CAPA results in your web browser? `CAPA Web Explorer`
    
27. Which feature of this CAPA Web Explorer allows you to filter options or results? `Global Search Box`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the Lab THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**