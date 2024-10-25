---
title: "Networking: Networking Core Protocols (TryHackMe)"
datePublished: Fri Oct 25 2024 03:45:51 GMT+0000 (Coordinated Universal Time)
cuid: cm2o6wdrx00030alhdr2adtg6
slug: networking-networking-core-protocols-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729827919422/1dece411-c816-44ac-a7b9-d716a130dc2d.png
tags: networking, network-security, ctf, ctf-writeup

---

In this article, I will write a write-up for Networking Core Protocols that covers DNS: Remembering Addresses, WHOIS, HTTP(S): Accessing the Web, FTP: Transferring Files, SMTP: Sending Email, POP§: Receiving Email and IAMP: Synchronizing Email.

1. Which DNS record type refers to IPv6? `AAAA`
    
2. Which DNS record type refers to the email server? `MX`
    
3. When was the [x.com](http://x.com/) record created? Provide the answer in YYYY-MM-DD format. `1993-04-02`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729827600166/ddc80d4f-7031-4caa-a814-52cc76474d4f.png align="center")
    
4. When was the [twitter.com](http://twitter.com/) record created? Provide the answer in YYYY-MM-DD format. `2000-01-21`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729827617127/3447feab-5f14-4e2d-b02f-4d12ee5bbc98.png align="center")
    
5. Use `telnet` to access the file `flag.html` on `MACHINE_IP`. What is the hidden flag? `THM{TELNET-HTTP}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729827681069/c2cbc18a-3e46-417c-9afd-6e3e669038a7.png align="center")
    
    We used Wireshark to examine the exchanged messages more closely. The client’s messages are in **red**, while the server’s responses are in **blue**. Notice how various commands differ between the client and the server. For example, when you type `ls` on the client, the client sends `LIST` to the server. One last thing to note is that the directory listing and the file we downloaded are sent over a separate connection each.
    
    ![https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1719849609513.png](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1719849609513.png)
    
    Answer the questions below
    
6. Using the FTP client `ftp` on the AttackBox, access the FTP server at `MACHINE_IP` and retrieve `flag.txt`. What is the flag found? `THM{FAST-FTP}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729827748600/75b5c5a3-1735-405f-a417-b87e2669cf7e.png align="center")
    
7. Which SMTP command indicates that the client will start the contents of the email message? `DATA`
    
8. What does the email client send to indicate that the email message has been fully entered? `.`
    
9. Looking at the traffic exchange, what is the name of the POP3 server running on the remote server? `Dovecot`
    
10. Use `telnet` to connect to `MACHINE_IP`’s POP3 server. What is the flag contained in the fourth message? `THM{TELNET_RETR_EMAIL}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729827782263/ebf02869-01a2-45fb-83aa-ddd0933a5e79.png align="center")
    
11. What IMAP command retrieves the fourth email message? `FETCH 4 body[]`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729827817123/87296c14-8972-429a-9710-d27187ba7810.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**