---
title: "Network Fundamentals: OSI Model (TryHackMe)"
datePublished: Sun Oct 20 2024 08:12:23 GMT+0000 (Coordinated Universal Time)
cuid: cm2hb7w4j000409i36ozz8ixv
slug: network-fundamentals-osi-model-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729411803848/4188e481-0f3e-4b57-97a2-5f8ba8a48e6e.png
tags: networking, osi-model

---

In this article, I will write the writeup of the [OSI Model](https://tryhackme.com/r/room/osimodelzi) overview that covers the different layers i.e Physical, Data Link, Network, Transport, Session, Presentation and Application Layers. Here is a little overview of the OSI Model before we get to the walkthrough based on THM’s documentation for the tasks.

The **OSI** model (or **O**pen **S**ystems **I**nterconnection Model) is an absolute fundamental model used in networking.  This critical model provides a framework dictating how all networked devices will send, receive and interpret data.

One of the main benefits of the OSI model is that devices can have different functions and designs on a network while communicating with other devices. Data sent across a network that follows the uniformity of the OSI model can be understood by other devices.

The OSI model consists of seven layers which are illustrated in the diagram below. Each layer has a different set of responsibilities and is arranged from Layer 7 to Layer 1.

At every individual layer that data travels through, specific processes take place, and pieces of information are added to this data, which is what we'll come to discuss in the upcoming tasks within this room. However, for now, we only need to understand that this process is called encapsulation and what the OSI model looks like in the diagram below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729411681772/8dda4361-a7c5-4c77-8783-26f478a37cc0.png align="center")

## [Physical](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)

1. [What does the "OSI" in "OSI Model" stand for? `Open Systems Interconnection`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
2. [How many layers (in digits) d](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)oes the OSI model have? `7`
    
3. What is the key term for when pieces of information get added to da[ta? `Encapsulation`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
4. [What is the name of this Layer? `Physical`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
5. [What is the name of the numbering system that is both 0's and 1'](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)s? `Binary`
    
6. What is the name of the cables that are used to connect devices? `Ethernet Cables`
    
7. What is the name of this Layer? `Dat`[`a Link`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
8. [What is the name of the piece of hardware that all networked devices come with? `Network Interface Card`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
9. [What is the n](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)ame of this Layer? `Network`
    
10. Will packets take [the most optimal route across a network? (Y/N) `Y`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
11. [What does the acronym "OSPF" stand for? `Open Shortest Path First`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
12. [What doe](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)s the acronym "RIP" stand for? `Resource Information Protocol`
    
13. What type of addresses are dealt with at this layer? `IP Addresses`
    
14. What is the name of this Layer? `Transport`
    
15. What does TCP stand for? `Transmission Control P`[`rotocol`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
16. [What does UDP stand for? `User Datagram Protocol`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
17. [What protocol guarantees the accuracy of data? `TCP`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
18. [What protocol](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg) do[esn't care if data is received or not by the other device? `UDP`](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg)
    
19. [What protocol would an application such as an email client use?](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/6d17472b87f8792dadde3bb06aa1fdaa.svg) `TCP`
    
20. What protocol would an application that downloads files use? `TCP`
    
21. What protocol would an application that streams video use? `UDP`
    
22. What is the name of this layer? `Session`
    
23. What is the technical term for when a connection is successfully established? `Session`
    
24. What is the name of this Layer? `Presentation`
    
25. What is the main purpose that this Layer acts as? `Translator`
    
26. What is the name of this Layer? `Application`
    
27. What is the technical term that is given to the name of the software that users interact with? `Graphical User Interface`
    
28. Can you escape the OSI dungeon? Climb the levels in the correct order to escape the dungeon and reveal the flag! (Can you beat our staff high score of 19 seconds?) Click the "View Site" button on the right to start.
    
    Escape the dungeon to retrieve the flag. What is the flag? `THM{OSI_DUNGEON_ESCAPED}`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**