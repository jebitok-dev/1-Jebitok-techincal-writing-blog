---
title: "Extending Your Network"
datePublished: Sun Oct 20 2024 08:59:54 GMT+0000 (Coordinated Universal Time)
cuid: cm2hcwzft000209kz3mge3ai1
slug: extending-your-network
tags: networking, vpn, ports

---

In this article, we’ll cover the write-up for [Extending Your Network](https://tryhackme.com/r/room/extendingyournetwork) that covers Introduction to Port Forwarding, Firewall 101, VPN Basics, and LAN Networking Devices. Here are few definition of terms before covering the walkthrough:

**Port forwarding** is an essential component in connecting applications and services to the Internet. Without port forwarding, applications and services such as web servers are only available to devices within the same direct network.

A **firewall** is a device within a network responsible for determining what traffic is allowed to enter and exit. Think of a firewall as border security for a network. An administrator can configure a firewall to **permit** or **deny** traffic from entering or exiting a network based on numerous factors such as:

* Where the traffic is coming from? (has the firewall been told to accept/deny traffic from a specific network?)
    
* Where is the traffic going to? (has the firewall been told to accept/deny traffic destined for a specific network?)
    
* What port is the traffic for? (has the firewall been told to accept/deny traffic destined for port 80 only?)
    
* What protocol is the traffic using? (has the firewall been told to accept/deny traffic that is UDP, TCP or both?)
    

A **Virtual Private Network** (or **VPN** for short) is a technology that allows devices on separate networks to communicate securely by creating a dedicated path between each other over the Internet (known as a tunnel). Devices connected within this tunnel form their own private network.

1. What is the name of the device that is used to configure port forwarding? `router`
    
2. What layers of the OSI model do firewalls operate at? `Layer3,Layer 4`
    
3. What category of firewall inspects the **entire connection**? `stateful`
    
4. What category of firewall inspects individual packets? `stateless`
    
5. What is the flag? `THM{FIREWALLS_RULE}`
    
6. What VPN technology **only** encrypts & provides the authentication of data? `PPP`
    
7. What VPN technology uses the IP framework? `IPSec`
    
8. What is the verb for the action that a router does? `routing`
    
9. What are the two different layers of switches? Separate these by a comma I.e.: LayerX,LayerY `Layer2,Layer3`
    
10. What is the flag from the network simulator? `THM{YOU’VE_GOT_DATA}`
    
11. How many HANDSHAKE entries are there in the Network Log? `5`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**