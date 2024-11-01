---
title: "Web Hacking: SQL Fundamentals (TryHackMe)"
datePublished: Fri Nov 01 2024 10:17:24 GMT+0000 (Coordinated Universal Time)
cuid: cm2ykyvy1000d09mg9ejy6b0i
slug: web-hacking-sql-fundamentals-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730455524753/1e33cbfe-2c6d-4821-bdee-b0c936fde0c1.png
tags: databases, sql, tryhackme, write-up

---

In this article, I will write a write-up for SQL Fundamentals that covers Database 101, SQL, Database and Table Statements, CRUD Operations, Clauses, Operators, and Functions.

1. What type of database should you consider using if the data you're going to be storing will vary greatly in its format? `Non-relational database`
    
2. What type of database should you consider using if the data you're going to be storing will reliably be in the same structured format? `relational database`
    
3. In our example, once a record of a book is inserted into our "Books" table, it would be represented as a \_\_\_ in that table? `row`
    
4. Which type of key provides a link from one table to another? `foreign key`
    
5. which type of key ensures a record is unique within a table? `primary key`
    
6. What serves as an interface between a database and an end user? `DBMS`
    
7. What query language can be used to interact with a relational database? `SQL`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730456200360/3edbafb3-e859-4557-b45e-5eec1b97af3d.png align="center")
    
8. Using the statement you've learned to list all databases, it should reveal a database with a flag for a name; what is it? `THM{575a947132312f97b30ee5aeebba629b723d30f9}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730456117380/bdd11555-899e-4f06-b659-bac57d9284bc.png align="center")
    
9. In the list of available databases, you should also see the  `task_4_db` database. Set this as your active database and list all tables in this database; what is the flag present here?`THM{692aa7eaec2a2a827f4d1a8bed1f90e5e49d2410}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730456093378/3cf3fa47-ab9f-45d2-8d46-8c1183f42106.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730456059053/da8cb461-feab-4d40-ace8-3e668146790a.png align="center")
    
10. Using the `tools_db` database, what is the name of the tool in the `hacking_tools` table that can be used to perform man-in-the-middle attacks on wireless networks? `Wi-Fi Pineapple`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730456024912/6e0657c4-d617-4d3e-8087-5e29d6dceaaf.png align="center")
    
11. Using the `tools_db` database, what is the shared category for both **USB Rubber Ducky** and **Bash Bunny**? `USB attacks`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455997380/0f5e8c6d-65ed-43e8-b7b5-4ec4d03f71bb.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455981891/6c18c042-8f9a-4310-a6e1-d9efd74f65e1.png align="center")
    
12. Using the `tools_db` database, what is the total number of distinct categories in the `hacking_tools` table? `6`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455956996/d61d15d4-747f-445f-a15a-069de4bfcf0f.png align="center")
    
13. Using the `tools_db` database, what is the first tool (by name) in ascending order from the `hacking_tools` table? `Bash Bunny`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455929466/9818653b-bd93-4c03-afc3-a2d90b0feca9.png align="center")
    
14. Using the `tools_db` database, what is the first tool (by name) in descending order from the `hacking_tools` table? `Wi-Fi Pineapple`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455876520/448651ae-5a7b-4f65-8323-07f7d1b4bde3.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455860380/368131db-d5b6-463c-a278-848b0c783416.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455774059/d0734ce7-cb8b-411a-8b94-7d47a08ec4c6.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455749333/58f4acc4-f6f0-42b9-b692-76f32978ef95.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455731570/1bc7ba53-e113-484e-b33a-9fe9eb745ee0.png align="center")
    
15. Using the `tools_db` database, which tool falls under the **Multi-tool** category and is useful for **pentesters** and **geeks**? `Flipper Zero`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455697919/17fa5925-47c4-48a1-9b67-9f3673d90a8e.png align="center")
    
16. Using the `tools_db` database, what is the category of tools with an amount **greater than** or **equal** to **300**? `RFID cloning`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455679436/a76fdb49-eee7-475c-9702-acbf60b212eb.png align="center")
    
17. Using the `tools_db` database, which tool falls under the **Network intelligence** category with an amount **less than 100**? `Lan Turtle`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455644197/1b3d73f8-a1cc-4b85-90e9-7c6d237238a0.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455625101/b0a36f2c-3b8e-4918-85ae-de5151050c24.png align="center")
    
18. Using the `tools_db` database, what is the tool with the longest name based on character length? `USB Rubber Ducky`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455584589/c9fbca63-e10b-455d-9dd8-a9a0ca30a577.png align="center")
    
19. Using the `tools_db` database, what is the total sum of all tools? `1444`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455565833/c1c0eae6-9c5b-4cca-9944-e918712ad0bd.png align="center")
    
20. Using the `tools_db` database, what are the tool names where the amount does not end in **0**, and **group** the tool names **concatenated** by " & ". `Flipper Zero & iCopy-XS`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730455547328/b8f5dfae-aefd-4af2-b1d5-e21d7f558efe.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**