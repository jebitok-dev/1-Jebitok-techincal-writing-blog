---
title: "Networking: Networking Concepts (TryHackMe)"
datePublished: Fri Oct 25 2024 02:00:18 GMT+0000 (Coordinated Universal Time)
cuid: cm2o34ncp000109k57fyqhg8z
slug: networking-networking-concepts-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729821587014/1c940940-3dcb-47d6-ad5e-d03946d2edcd.png
tags: networking, networking-for-beginners, telnet

---

In this article, I will write a write-up for Networking Concepts that covers OSI Model, TCP/IP Model, IP Addresses and Subnets, UDP and TCP, Encapsulation, and Telnet.

1. Which layer is responsible for connecting one application to another? `Layer 4`
    
2. Which layer is responsible for routing packets to the proper network? `Layer 3`
    
3. In the OSI model, which layer is responsible for encoding the application data? `Layer 6`
    
4. Which layer is responsible for transferring data between hosts on the same network segment? `Layer 2`
    
5. To which layer does HTTP belong in the TCP/IP model? `Application Layer`
    
6. How many layers of the OSI model does the application layer in the TCP/IP model cover? `3`
    
7. Which of the following IP addresses is not a private IP address?
    
    * 192.168.250.125
        
    * 10.20.141.132
        
    * 49.69.147.197
        
    * 172.23.182.251 `49.69.147.197`
        
8. Which of the following IP addresses is not a valid IP address?
    
    * 192.168.250.15
        
    * 192.168.254.17
        
    * 192.168.305.19
        
    * 192.168.199.13 `192.168.305.19`
        
9. Which protocol requires a three-way handshake? `TCP`
    
10. What is the approximate number of port numbers (in thousands)? `65`
    
11. On a WiFi, within what will an IP packet be encapsulated? `Frame`
    
12. What do you call the UDP data unit that encapsulates the application data? `Datagram`
    
13. What do you call the data unit that encapsulates the application data sent over TCP? `Segment`
    
    let’s request a web page using `telnet`. After connecting to port 80, you need to issue the command `GET / HTTP/1.1` and identify the host where anything goes, such as `Host: telnet.thm`. The output below shows the exchange. (The page has been redacted.)
    
    **Note**: You may have to press `Enter` after sending the information in case you don’t get a response.
    
    Terminal
    
    ```plaintext
    user@TryHackMe$ telnet MACHINE_IP 80Trying MACHINE_IP...
    Connected to MACHINE_IP.
    Escape character is '^]'.
    GET / HTTP/1.1
    Host: telnet.thm
    
    HTTP/1.1 200 OK
    Content-Type: text/html
    [...]
    
    Connection closed by foreign host.
    ```
    
    Answer the questions below
    
14. Use `telnet` to connect to the web server on `MACHINE_IP`. What is the name and version of the HTTP server? `lighttpd/1.4.63`
    

Remember Telnet replaces ssh so we won’t need the ssh this time. You run `telnet ip_address 80` 80 since it's HTTP Server . Once you’ve entered type `GET / HTTP/1.1` enter then type `Host: telnet.thm` keep entering till you get an output is the connection closes before you get an output try again

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729821395663/df04f06e-5aed-4e4f-aabf-7ae6c1313f88.png align="center")

15. What flag did you get when you viewed the page? `THM{TELNET_MASTER}`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**