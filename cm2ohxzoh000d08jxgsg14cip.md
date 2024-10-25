---
title: "Networking: Networking Secure Protocols (TryHackMe)"
datePublished: Fri Oct 25 2024 08:55:02 GMT+0000 (Coordinated Universal Time)
cuid: cm2ohxzoh000d08jxgsg14cip
slug: networking-networking-secure-protocols-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729846348557/c1d830df-c219-41b8-bed9-90a5f80e1273.png
tags: networking, network-security

---

In this article, I will write a write-up for Networking Core Protocols that covers how TLS, HTTPS, SMTPS, POP3S & IMAPS, SSH, SFTP & FTPS, and VPN can secure your network traffic.

1. What is the protocol name that TLS upgraded and built upon? `SSL`
    
2. Which type of certificates should not be used to confirm the authenticity of a server? `self-signed certificate`
    
3. How many packets did the TLS negotiation and establishment take in the Wireshark HTTPS screenshots above? `8`
    
4. What is the number of the packet that contain the `GET /login` when accessing the website over HTTPS? `10`
    
5. If you capture network traffic, in which of the following protocols can you extract login credentials: SMTPS, POP3S, or IMAP? `IMAP`
    
6. What is the name of the open-source implementation of the SSH protocol? `OpenSSH`
    
    SFTP stands for SSH File Transfer Protocol and allows secure file transfer. It is part of the SSH protocol suite and shares the same port number, 22. If enabled in the OpenSSH server configuration, you can connect using a command such as `sftp username@hostname`. Once logged in, you can issue commands such as `get filename` and `put filename` to download and upload files, respectively. Generally speaking, SFTP commands are Unix-like and can differ from FTP commands.
    
    SFTP should not be confused with FTPS. You are right to think that FTPS stands for File Transfer Protocol Secure. How is FTPS secured? Yes, you are correct to estimate that it is secured using TLS, just like HTTPS. While FTP uses port 21, FTPS usually uses port 990. It requires certificate setup, and it can be tricky to allow over strict firewalls as it uses separate connections for control and data transfer.
    
    Setting up an SFTP server is as easy as enabling an option within the OpenSSH server. Like HTTPS, SMTPS, POP3S, IMAPS, and other protocols that rely on TLS for security, FTPS requires a proper TLS certificate to run securely.
    
    Answer the questions below
    
7. Click on the **View Site** button to access the related site. Please follow the instructions on the site to obtain the flag. `THM{Protocols_secur3d}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729846224243/b698da70-a850-4335-b058-267cf3ac9c61.png align="center")
    
8. What would you use to connect the various company sites so that users at a remote office can access resources located within the main branch? `VPN`
    
9. One of the packets contains login credentials. What password did the user submit? `THM{B8WM6P}`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**