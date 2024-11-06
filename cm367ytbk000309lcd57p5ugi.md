---
title: "Introduction to Offensive Security (TryHackMe)"
datePublished: Wed Nov 06 2024 18:35:35 GMT+0000 (Coordinated Universal Time)
cuid: cm367ytbk000309lcd57p5ugi
slug: introduction-to-offensive-security-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730917896318/dfe202a4-b76d-4cd1-85e6-f51d7a1e4426.png
tags: tryhackme, write-up, offensive-security

---

In this article, I will write a Web Application Security write-up covering Web Application Security Risks and Practical Examples of Web Application Security.

1. What do you need to access a web application? `Browser`
    
2. You discovered that the login page allows an unlimited number of login attempts without trying to slow down the user or lock the account. What is the category of this security risk? `Identification and Authentication Failure`
    
3. You noticed that the username and password are sent in cleartext without encryption. What is the category of this security risk? `Cryptographic Failures`  
      
    
    This task will investigate a vulnerable website that uses Insecure Direct Object References (IDOR). IDOR falls under the category of Broken Access Control. Broken access control means that an attacker can access information or perform actions not intended for them. Consider the case where a web server receives user-supplied input to retrieve objects (files, data, documents) and that they are numbered sequentially. Let’s say that the user has permission to access a photo named `IMG_1003.JPG`. We might guess that there are also `IMG_1002.JPG` and `IMG_1004.JPG`; however, the web application should not provide us with that image even if we figured out its name. In general, an IDORvulnerability can occur if too much trust has been placed on that input data. In other words, the web application does not validate whether the user has permission to access the requested object.
    
    Just providing the correct URL for a user or a product does not necessarily mean the user should be able to access that URL. For instance, consider the product page `https://store.tryhackme.thm/products/product?id=52`. We can expect this URL to provide details about product number `52`. In the database, items would be assigned numbers sequentially. The attacker would try other numbers such as `51` or `53` instead of `52`; this might reveal other retired or unreleased products if the web application is vulnerable.
    
    Let’s consider a more critical example; the URL `https://store.tryhackme.thm/customers/user?id=16` would return the user with `id=16`. Again, we expect the users to have sequential ID numbers. The attacker would try other numbers and possibly access other user accounts. This vulnerability might work with sequential files; for instance, if the attacker sees `007.txt`, the attacker might try other numbers such as `001.txt`, `006.txt`, and `008.txt`. Similarly, if you were ID number 16 and ID number 17 was another user, by changing the ID to 17, you could see sensitive data that belongs to another user. Likewise, they can change the ID to 16 and see sensitive data that belongs to you. (Of course, we assume here that the system is vulnerable to IDOR.)
    
    Click on “View Site,” and let’s see this in action. You will see a page showing an Inventory Management System. If you click on the “Planned Shipments” tab, you will discover that an attacker has managed to mix things up as part of sabotage plans. Notice how they send the wrong tires to each assembly line; for instance, they assign scooter tires and motorcycle tires to bike assembly! If left unfixed, all tires will go to the wrong assembly.
    
    We will hack the system back and undo the attacker’s steps. On “Your Activity,” you can see the activity of one of the users. We have reason to believe that this website has an IDORvulnerability.  
    
4. Check the other users to discover which user account was used to make the malicious changes and revert them. After reverting the changes, what is the flag that you have received? `THM{IDOR_EXPLORED}`  
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the Lab THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**