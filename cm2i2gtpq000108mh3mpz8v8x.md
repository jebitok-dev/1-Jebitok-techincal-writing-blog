---
title: "Network Fundamentals: What is Networking (TryHackMe)"
datePublished: Sun Oct 20 2024 20:55:10 GMT+0000 (Coordinated Universal Time)
cuid: cm2i2gtpq000108mh3mpz8v8x
slug: network-fundamentals-what-is-networking-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729457690613/ff017586-476a-421a-8ace-f394683a2df9.png
tags: networking

---

In this article, we’re covering the Introduction to Networking. Before the walkthrough, I will share the key points about networking according to this TryHackMe room and tasks:

## What are Networks?

Networks are simply things connected. For example, your friendship circle: you are all connected because of similar interests, hobbies, skills, and sorts.

Networks can be found in all walks of life:

* A city's public transportation system
    
* Infrastructure such as the national power grid for electricity
    
* Meeting and greeting your neighbors
    
* Postal systems for sending letters and parcels
    

But more specifically, in computing, networking is the same idea, just dispersed to technological devices. Take your phone as an example; the reason that you have it is to access things. We'll cover how these devices communicate with each other and the rules that follow.

In computing, a network can be formed by anywhere from 2 devices to billions. These devices include everything from your laptop and phone to security cameras, traffic lights and even farming!

Networks are integrated into our everyday life. Be it gathering data for the weather, delivering electricity to homes or even determining who has the right of way at a road. Because networks are so embedded in the modern-day, networking is an essential concept to grasp in cybersecurity.

Take the diagram below as an example, Alice, Bob and Jim have formed their network! We'll come onto this a bit later on.

## What is Internet

The Internet is one giant network that consists of many, many small networks within itself. Using our example from the previous task, let's now imagine that Alice made some new friends named Zayn and Toby that she wants to introduce to Bob and Jim. The problem is that Alice is the only person who speaks the same language as Zayn and Toby. So Alice will have to be the messenger!

Because Alice can speak both languages, they can communicate to one another through Alice — forming a new network.

The first iteration of the Internet was within the ARPANET project in the late 1960s. This project was funded by the United States Defence Department and was the first documented network in action. However, it wasn't until 1989 when the Internet as we know it was invented by Tim Berners-Lee by the creation of the **W**orld **W**ide **W**eb (**WWW**). It wasn't until this point that the Internet started to be used as a repository for storing and sharing information, just like it is today.

Let's relate Alice's network of friends to computing devices. The Internet looks like a much larger version of this sort of diagram:

the Internet is made up of many small networks all joined together.  These small networks are called private networks, where networks connecting these small networks are called public networks -- or the Internet! So, to recap, a network can be one of two types:

* A private network
    
* A public network
    

Devices will use a set of labels to identify themselves on a network, which we will come onto in the task below.

Q/A: Tim Berners-Lee by the creation of the **W**orld **W**ide **W**eb (**WWW**)

## Identifying Devices on a Network

To communicate and maintain order, devices must be both identifying and identifiable on a network. What use is it if you don't know whom you're talking to at the end of the day?

Devices have the same thing: two means of identification, with one being permeable. These are:

* An IP Address
    
* A Media Access Control (MAC) Address -- think of this as being similar to a serial number.
    

### **IP Addresses**

Briefly, an IP address (or **I**nternet **P**rotocol) address can be used as a way of identifying a host on a network for a period of time, where that IP address can then be associated with another device without the IP address changing.

1. What is the key term for devices that are connected together? `Network`
    
2. Who invented the World Wide Web? `Tim Berners-Lee`
    
3. What does the term "IP" stand for? `Internet Protocol`
    
4. What is each section of an IP address called? `Octet`
    
5. How many sections (in digits) does an IP address have? `4`
    
6. What does the term "MAC" stand for? `Media Access Control`
    
7. Deploy the interactive lab using the "View Site" button and spoof your MAC address to access the site.  What is the flag? `THM{YOU_GOT_ON_TRYHACKME}`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**