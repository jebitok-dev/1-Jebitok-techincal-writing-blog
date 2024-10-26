---
title: "Networking: Tcpdump: The Basics (TryHackMe)"
datePublished: Sat Oct 26 2024 05:13:56 GMT+0000 (Coordinated Universal Time)
cuid: cm2pphifv000109jwasrphimz
slug: networking-tcpdump-the-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729919530652/7ec30470-fcf0-42eb-b2b5-a63f286c98b1.png
tags: networking, write-up, tcpdump

---

In this article, I will write a write-up for Tcpdump: The Basics that covers Basic Packet Capture, Filtering Expressions, Advanced Filtering, and Displaying Packets as ways to use `tcpdump` to save, filter and display packet.

1. What is the name of the library that is associated with `tcpdump?` `libpcap`
    
2. What option can you add to your command to display addresses only in numeric format? `-n`
    
3. How many packets in `traffic.pcap` use the ICMP protocol? `26`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729919480935/ed4a3fad-49b4-4e13-8a04-4831168915d6.png align="center")
    
4. What is the IP address of the host that asked for the MAC address of 192.168.124.137? `192.168.124.148`
    
5. What hostname (subdomain) appears in the first DNS query? [`mirrors.rockylinux.org`](http://mirrors.rockylinux.org/)
    
6. How many packets have only the TCP Reset (RST) flag set? `57`
    
7. What is the IP address of the host that sent packets larger than 15000 bytes? `185.117.80.53`
    
8. What is the MAC address of the host that sent an ARP request? `52:54:00:7c:d3:5b`
    

Using this command `tcpdump -r traffic.pcap -e`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729919411259/98f5459a-8485-473b-8a97-02d3e51d7c39.png align="center")

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**