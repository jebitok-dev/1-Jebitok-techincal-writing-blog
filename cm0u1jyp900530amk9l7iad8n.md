---
title: "Introductory Networking: Networking Theory and Basic Networking Tools (TryHackMe)"
datePublished: Sun Sep 08 2024 20:43:26 GMT+0000 (Coordinated Universal Time)
cuid: cm0u1jyp900530amk9l7iad8n
slug: introductory-networking-networking-theory-and-basic-networking-tools-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725827185356/cdbe418e-345f-429f-81d9-67ab8bfdc1b8.png
tags: networking, tryhackme

---

I have been learning Networking Theory like the OSI Model and TCP/IP model as well as Command-Line Networking Tools like ping, traceroute, whois, and dig. I took these questions as part of the THM challenges for this networking room.

1. Which layer would choose to send data over TCP or UDP? `4` (Transport Layer)
    
2. Which layer checks received information to make sure that it hasn't been corrupted? `2` (Data Layer)
    
3. In which layer would data be formatted in preparation for transmission? `2` (Data Link Layer)
    
4. Which layer transmits and receives data? `1` (Physical Layer)
    
5. Which layer encrypts, compresses, or otherwise transforms the initial data to give it a standardized format? `6`(Presentation)
    
6. Which layer tracks communications between the host and receiving computers? `5` (Session Layer)
    
7. Which layer accepts communication requests from applications? `7` (Application Layer)
    
8. Which layer handles logical addressing? `3` (Network Layer)
    
9. When sending data over TCP, what would you call the "bite-sized" pieces of data? `7` (Application layer)
    
10. **\[Research\]** Which layer would the FTP protocol communicate with? `Segments`
    
11. Which transport layer protocol would be best suited to transmit a live video? `UDP`
    
12. How would you refer to data at layer 2 of the encapsulation process (with the OSI model)? `Frames` (Layer 2 is the Data link layer)
    
13. How would you refer to data at layer 4 of the encapsulation process (with the OSI model), if the UDP protocol has been selected? `Datagrams or segment` (Layer 4 is the transport layer)
    
14. What process would a computer perform on a received message? `De-encapsulation`
    
15. Which is the only layer of the OSI model to add a *trailer* during encapsulation? `Data Link`
    
16. Does encapsulation provide an extra layer of security **(Aye/Nay)**? `Aye`
    
17. Which model was introduced first, OSI or TCP/IP? `TCP/IP`
    
18. Which layer of the TCP/IP model covers the functionality of the Transport layer of the OSI model **(Full Name)**? `Transport`
    
19. Which layer of the TCP/IP model covers the functionality of the Session layer of the OSI model **(Full Name)**? `Application`
    
20. The Network Interface layer of the TCP/IP model covers the functionality of two layers in the OSI model. These layers are Data Link, and?.. **(Full Name)**? `Physical`
    
21. Which layer of the TCP/IP model handles the functionality of the OSI network layer? `Internet`
    
22. What kind of protocol is TCP? `Connection-based`
    
23. What is SYN short for? `Synchronise`
    
24. What is the second step of the three-way handshake? `SYN/ACK`
    
25. What is the short name for the "Acknowledgement" segment in the three-way handshake? `ACK`
    
26. What command would you use to ping the [bbc.co.uk](http://bbc.co.uk/) website? `ping bbc.co.uk`
    
27. $`Ping *muirlandoracle.co.uk*` What is the IPv4 address? `217.160.0.152`
    
28. What switch lets you change the interval of sent ping requests? `-i`  
    (using man ping to find the answers of 28 - 30)
    
29. What switch would allow you to restrict requests to IPv4? `-4`
    
30. What switch would give you a more verbose output? `-v`
    
31. Can you see the path your request has taken? `traceroute tryhackme.com`
    
32. What switch would you use to specify an interface when using Traceroute? `-i` (use man traceroute to find the answers to 32 -33)
    
33. What switch would you use if you wanted to use TCP SYN requests when tracing the route? `-T`
    
34. **\[Lateral Thinking\]** Which layer of the ***TCP/IP*** model will the traceroute run on by default (Windows)? `Internet`
    
35. **Perform a whois search on** `facebook.com` `whois Facebook.com`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725827410950/feb73fd6-9c7c-4c03-bdce-9f15329cb474.png align="center")
    
36. What is the registrant postal code for [facebook.com](http://facebook.com/)? `94025`
    
37. When was the [facebook.com](http://facebook.com/) domain first registered (Format: DD/MM/YYYY)? `29/03/1997`
    
38. **Perform a whois search on** `microsoft.com` `whois Microsoft.com`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725827397445/cef40a36-3a79-425d-8b2a-1a6108b0c3f3.png align="center")
    
39. Which city is the registrant based in? `Redmond`
    
40. **\[OSINT\]** What is the name of the golf course that is near the registrant address for [microsoft.com](http://microsoft.com/)? `Bellevue Golf Course`  
      
    
    with Microsoft address from the whois [Microsoft.com](http://microsoft.com/) search golf courses followed by the address (`One Microsoft Way, Redmond, 98052`) You’ll check golf course as a keyword and the closest miles
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725827482610/707c916e-35d4-4dc9-a73b-d958a9f20254.png align="center")
    
41. What is the registered Tech Email for [microsoft.com](http://microsoft.com/)? [`msnhst@microsoft.com`](mailto:msnhst@microsoft.com)
    
42. What is DNS short for? `Domain Name System`
    
43. What is the first type of DNS server your computer would query when you search for a domain? `Recursive`
    
44. What type of DNS server contains records specific to domain extensions (i.e. *.com,* .co.uk\*, etc)\*? Use the long version of the name. `Top-Level Domain`
    
45. Where is the very first place your computer would look to find the IP address of a domain? `Hosts File`
    
46. **\[Research\]** Google runs two public DNS servers. One of them can be queried with the IP 8.8.8.8, what is the IP address of the other one? `8.8.4.4`
    
47. If a DNS query has a TTL of 24 hours, what number would the dig query show? `86400`
    

Thank you for reading through my article. You can leave any questions or comments on how I can improve my learning journey and the THM challenges. We can also connect more on [LinkedIn](https://www.linkedin.com/in/sharon-jebitok) or [X](https://x.com/SharonJebitok).