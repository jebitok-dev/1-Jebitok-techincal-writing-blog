---
title: "Offensive Security Tooling: SQLMap: The Basics (TryHackMe)"
datePublished: Sun Nov 03 2024 13:25:34 GMT+0000 (Coordinated Universal Time)
cuid: cm31mkk7l000a08mf0fhu54jh
slug: offensive-security-tooling-sqlmap-the-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730638696087/7983e1fc-5da2-4c12-bcc9-9372f683fae8.png
tags: sql, tryhackme, write-up, sqlmap

---

In this article, I will write a write-up for SQLMap: The Basics that covers SQL Injection Vulnerability, Automated SQL Injection Tool, and a Practical Exercise.

This room was a bit complicated to me so I’ll share this [LinkedIn article](https://www.linkedin.com/pulse/sqlmap-basics-cyber-security-101-tryhackme-writeup-detailed-verma--aegnf/) that has clear steps to navigate the practical exercise. I wasn’t able to capture them well since we have few to the closure of the challenge.

1. Which language builds the interaction between a website and its database? `SQL`
    
2. Which boolean operator checks if at least one side of the operator is true for the condition to be true? `or`
    
3. Is 1=1 in an SQL query always true? (YEA/NAY) `YEA`
    
4. Which flag in the SQLMap tool is used to extract all the databases available? `—dbs`
    
5. What would be the full command of SQLMap for extracting all tables from the "members" database? (Vulnerable URL: [http://sqlmaptesting.thm/search/cat=1](http://sqlmaptesting.thm/search/cat=1)) `sqlmap -u <http://sqlmaptesting.thm/search/cat=1> -D members --tables`
    
6. How many databases are available in this web application? `6`
    
7. What is the name of the table available in the "ai" database? `user`
    
8. What is the password of the email [test@chatai.com](mailto:test@chatai.com)? `12345678`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**