---
title: "Web Security Essentials (TryHackMe)"
datePublished: Fri Aug 29 2025 07:09:59 GMT+0000 (Coordinated Universal Time)
cuid: cmewht9bi000402jpgug909vh
slug: web-security-essentials-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756451004615/4531b32f-6202-4e0d-bcec-d2329f99c33e.png
tags: web-server, web-security, tryhackme, web-infrastructure, webapplicationfirewall

---

The internet powers nearly every part of our daily lives—banking, shopping, entertainment, and communication. This dependence on the web makes websites and web applications a prime target for attackers. Understanding how the web works, why applications are vulnerable, and what defenses are available is essential for anyone in cybersecurity. In this room, we’ll explore the evolution of web applications, examine common threats, and learn how to protect applications, servers, and host machines from attack.

## Introduction

The internet is behind many aspects of modern life, from banking and shopping to social media and beyond. As a result, websites and web applications are among attackers' most targeted assets. Whether you're defending a company's website or investigating an incident, understanding how the web works and how to secure it is crucial.

## **Learning Objectives**

* Understand the shift from desktop applications to web applications
    
* Learn why web applications are common targets for attackers
    
* Explore web infrastructure and the tools we use to protect the web
    
* Practice applying security measures to harden a new web application
    

## **Prerequisites**

* [Web Application Basics](https://tryhackme.com/room/webapplicationbasics) provides an excellent overview of the essentials of web applications
    
* Complete [HTTP In Detail](https://tryhackme.com/room/httpindetail) to brush up on web requests, response codes, and all things HTTP
    

Answer the questions below

I understand the learning objectives and am ready to learn about web security!

## Why Web?

The shift from desktop to web-based applications has been ongoing for decades. In the 1990s, desktop applications were the norm because of speed and connectivity limitations. As web technology advanced, the 2000s gave way to much more widely used dynamic web applications for email, social media, and banking. In the 2010s, there was a [massive rise](https://www.techrepublic.com/article/the-most-important-cloud-advances-of-the-decade/) in cloud computing and software as a service (SaaS), and today, nearly everything can be done in a browser.

![A graphic showing the shift from desktop applications to web applications. The left side shows an old desktop where users would access their favorite apps. On the right is a modern, sleek web application emphasizing ease of use and availability.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1755698728579.svg align="left")

## **From a Security Perspective**

The shift to web apps brings some amazing advantages, including increased accessibility, faster updates, better compatibility, and reduced resource usage on the user's end. Think of it, you can browse online marketplaces and social networks, play games, edit images and video, and even run virtual machines all through your browser. However, these benefits come with tradeoffs in terms of security. The more powerful and widespread the web becomes, the more opportunities it introduces for attackers.

Web applications are among the most common entry points for attackers because they are always available and exposed. They often connect to back-end systems like databases and other infrastructure, offering attackers high-impact opportunities. A vulnerable web application is often the first stage in a larger attack sequence. Let's take a look at the risks faced by both web app owners and their users.

| **As a Web App Owner** | **As a Web App User** |
| --- | --- |
| Your web application is always online and must be secured 24/7 | Your data is stored in a web application, potentially insecurely |
| Anyone around the world can access your app at any time | Once your browser is breached, all of your accounts are at risk |
| It is challenging to stay up to date with so many emerging threats | A breach can result in identity theft or financial loss |
| You have the responsibility of securing your users' data | Your privacy can be permanently compromised |

## **Real-World Examples**

In 2017, [Equifax's](https://archive.epic.org/privacy/data-breach/equifax/) sensitive customer data of nearly 150 million Americans was compromised due to an Apache [vulnerability](https://www.cve.org/CVERecord?id=CVE-2017-5638). By abusing this vulnerability, the attackers were able to access internal databases storing valuable customer data.

[Capital One](https://www.capitalone.com/digital/facts2019/) faced a similar-scale breach in 2019, in which a misconfigured web application firewall (WAF) exposed over 100 million customers' sensitive personal and financial information. This misconfiguration allowed internal access to the company's cloud infrastructure and databases.

### Answer the questions below

Have applications shifted from desktop to web over the past couple of decades (Yea/Nay)? `Yea`

Who is ultimately responsible for ensuring the security of users' data within a web application? `Web App Owner`

## Web Infrastructure

When you visit a website, your browser sends a request to a web server. The server processes the request, verifies access, and returns a response to the user. This response can be a webpage, an image, or data like search results or your account information. This request-response cycle is the foundation of how the web functions. Attackers can abuse this request-response cycle by overwhelming servers with requests, bypassing access controls, or even tricking the server into executing harmful commands.

## **Components of a Web Service**

For example, any web service, like [tryhackme.com](https://tryhackme.com/), requires three main components to function.

* **Application**: The code, images, styles, and icons that dictate how the website looks and functions.
    
* **Web Server**: This component hosts the application. It listens for requests and returns a response to the user.
    
* **Host Machine**: The underlying operating system, Linux or Windows, that runs the web server and the application.
    

![A web request path example: Your web browser sends the request through the Internet to the Apache web server, which accepts and delivers the request for processing to a PHP web app.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1755124417169.svg align="left")

In the next task, we will investigate the security measures available to protect these three components.

## **Web Servers**

When you visit a website, your web browser sends a request to a web server, as discussed above. Web servers listen for incoming requests and return an appropriate response. Web servers are positioned in front of websites and applications, making them a crucial aspect of the internet's foundation. Because they are publicly exposed and handle all incoming web requests, web servers are a common target for attackers.

Here are some of the most common web servers that you will encounter.

* [**Apache**](https://httpd.apache.org/): The most popular web server to host simple websites and blogs, most commonly [WordPress](https://wordpress.com/).
    
* [**Nginx**](https://nginx.org/): An industry standard for high-performance web apps. Used by companies like [Netflix](https://openconnect.netflix.com/en/appliances/#software), Airbnb, and GitHub.
    
* [**Internet Information Services**](https://www.iis.net/) **(IIS)**: A Microsoft-developed web server commonly used in enterprise environments.
    

![Apache, Nginx, and Microsoft IIS logos to reference their mention in the task. These represent the three most well-known and used web servers.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1754115791334.svg align="left")

### Answer the questions below

What does your web browser send to a server to receive a web page? `Request`

What web server is most commonly used to host WordPress websites? `Apache`

What do we call the OS and environment that runs the web server and application? `Host Machine`

## Protecting the Web

## **Best Practices**

Various security measures are available when securing websites and web applications. Some solutions provide visibility, while others can actively stop or limit an attack, commonly known as mitigation. Referencing Task 3, where we discussed the three essential components of any web service: the **application**, the **web server**, and the **host machine**, let's now examine the protections available for each of these components.

**Protecting the Application**

* Secure Coding: Avoid insecure functions, ensure proper handling of errors, and remove sensitive information.
    
* Input Validation & Sanitization: Validate and sanitize user input to prevent injection attacks.
    
* Access Control: Restrict access based on user roles.
    

**Protecting the Web Server**

* Logging: Keep a detailed record of all web requests with access logs.
    
* Web Application Firewall (WAF): Filter and block harmful traffic based on defined rules.
    
* Content Delivery Network (CDN): Reduce direct exposure to your server and use integrated WAFs.
    

**Protecting the Host Machine**

* Least Privilege: Use low-privilege users for services.
    
* System Hardening: Disable unnecessary services and close unused ports.
    
* Antivirus: Add endpoint-level protection that blocks known malware.
    

**Security Tips for All Three Components**

* Strong Authentication: Don't just let anyone access your code, admin panels, or host machine.
    
* Patch Management: Ensure your app dependencies, web server, and host machine are up to date.
    

## **Logging**

Web servers can create logs for every request they receive. We call these access logs, and they are incredibly valuable from a security perspective because they track information about every interaction with the server, including the client's IP address, timestamp, requested page or data, response status from the server, and user agent. These fields can play an important role in investigations, helping analysts detect potential malicious activity and trace attacker behavior.

Let's take a look at a benign series of events that we might find in an access log to get a feel for the type of data we can observe.  
Note that `GET` requests are used to retrieve a resource from the server, like a specific web page.  
`POST` requests are used to submit data to the server, such as login credentials.

1. The user, from the client IP `10.10.10.100`, visits the website's homepage at `/index.html`.
    
2. Next, they navigate to the login page at `/login.html`.
    
3. They then enter their credentials and submit the form, signified by the `POST` request.
    
4. Finally, they access their account page at `/myaccount.html`.
    

Although this series of events is expected and not out of the ordinary, you can see how the verbosity of these logs can help analysts and incident responders reconstruct a possible attack sequence.

![A sample access log snippet in which a user accesses a home page at index.html, accesses the login portal at login.html, submits their credentials via POST request to login.html, and finally accesses their account at myaccount.html. The access log shows important information valuable to a security analyst, including IP address, timestamp, requested URL, status code, response size, referrer, and user agent.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1754970029807.svg align="left")

### Answer the questions below

What cyber security concept involves stopping or limiting damage from threats? `Mitigation`

What security control involves ensuring all software and components are up to date? `Patch Management`

## Defense Systems

## **Content Delivery Network (CDN)**

**CDNs** store and serve cached content from servers closer to the user to reduce latency. Imagine you have a main server housed in a central location. This main server provides information to edge servers worldwide so your customers can access data more quickly and safely. Aside from speed, CDNs also help in a security sense by acting as a buffer between the user and the origin server.

**Security Benefits**

* IP Masking: Hides the origin server IP address, which makes it harder for attackers to target.
    
* DDoS Protection: CDNs can absorb a large amount of traffic, making denial-of-service attacks less effective.
    
* Enforced HTTPS: Encrypted communication via TLS is enforced by default by most CDNs.
    
* Integrated WAF: Many CDNs, including [Cloudflare CDN](https://www.cloudflare.com/), [Amazon CloudFront](https://aws.amazon.com/cloudfront/) & [Azure Front Door](https://learn.microsoft.com/en-us/azure/frontdoor/front-door-overview), integrate web application firewalls.
    

In essence, CDNs allow web apps to deliver data to customers more efficiently and securely.

![A graphical representation of a Content Delivery Network (CDN) in which the central server distributes data to edge servers around the world, which then distribute data to the users.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1754461487753.svg align="left")

## **Web Application Firewall (WAF)**

**WAFs** are a powerful tool that can be integrated as another layer of protection for websites and web applications. They inspect incoming HTTP traffic and block or log potentially harmful requests based on security rules. Think of the analogy of a bouncer at a bar or club. Every person (web request) that wants to enter must be checked by the bouncer (firewall). Anyone (any request) that doesn't meet the standard requirement will be rejected.

Let's take a closer look at the types of WAFs available to us as defenders, then dive deeper into their functionality.

* Cloud-based (Reverse Proxy): Sits in front of the web server. These WAFs are easy to deploy and have great scalability.
    
* Host-based: Software deployed directly on the web server and offers control for each application.
    
* Network-based: A physical or virtual appliance situated on the network perimeter. More suited for enterprise environments.
    

**Functionality**

As stated above, WAFs inspect HTTP requests to detect anomalies, attacks, or known suspicious patterns. Below are some of the methods used, along with examples of requests that may be blocked.

| **WAF Feature** | **Detection Method** | **Example** |
| --- | --- | --- |
| Signature-Based Detection | Matches known attack patterns or payloads | A request with a User-Agent that matches a known tool, `sqlmap/1.8.1` |
| Heuristic-Based Detection | Analyzes the context and behavior of requests | A long query string with special characters `search?q=%3Cscript%20(1)` |
| Anomaly & Behavioral Analysis | Flags deviations from normal traffic behavior | A single IP address makes repeated login attempts in a short period of time |
| Location & IP Reputation Filtering | Uses location and threat intel to block IPs | A request from an IP address that is outside of your normal business area |

The above table is not exhaustive, as detection methods are constantly evolving, and custom rules can be created based on the specific needs of the web application owner.

Below is a screenshot of the Cloudflare dashboard for `tryhackme.thm`, focused on the security panel. In it, we can see all requests for the last 24 hours, including requests blocked by the integrated web application firewall.

![The Cloudflare CDN dashboard with the Security panel selected shows the total number of requests the website received in the past 24 hours and all mitigated or blocked requests.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1755125065099.svg align="left")

## **Antivirus (AV)**

**AVs** are often misunderstood as a blanket protection measure, but they are primarily made to safeguard endpoints, such as desktops, laptops, and servers, from known malicious files and programs. Most AVs rely on signature-based detection, which means they compare files with a database of known malware or patterns.

While web attacks usually target the application layer, not the host machine, AVs still play an important role in host protection, as discussed in Task 3. They can help detect malicious file uploads, such as web shells, post-exploitation tools, and other malicious software. AVs are just one layer in a broader defense-in-depth strategy and should be combined with other security measures to provide stronger protection.

### Answer the questions below

Which type of Web Application Firewall operates by running on the same system as the application itself? `Host-Based`

Which common WAF detection technique works by matching incoming requests against known malicious patterns? `Signature-Based`

## Practice Scenario

Let's take a more hands-on look at the security measures you've learned about in this room by applying them to a real-world scenario. Your site, **Secure-A-Site**, is currently being developed and will be deployed soon. Your goal is to help prepare the web application, web server, and host machine for launch by ensuring they are as secure as possible. 

You'll work through the three layers and apply the best practices that you learned about in the previous tasks:

* Web Application
    
* Web Server
    
* Host Machine
    

## **Practice**

View Site

Open **Secure-A-Site** by clicking the **View Site** button above. Once you complete each section, claim the flags and answer the task questions!

![A screenshot of the practical static site showing the three levels the user must secure.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1756294380031.svg align="left")

### Answer the questions below

What flag did you receive for securing the Web Application? `THM{web_app_secured!}`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756450866774/417c3e89-3d40-4a60-912d-b96b38eed4b8.png align="center")

What flag did you receive for securing the Web Server? `THM{server_security_expert!}`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756450931743/8a0bca2b-439d-4a01-a39a-ae684696ad89.png align="center")

What flag did you receive for securing the Host Machine? `THM{the_final_security_layer!}`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756450940330/2b112682-6138-4b45-9d4a-b4269199abf4.png align="center")

## Conclusion

In this room, you explored the essentials of web security, starting with the shift from traditional desktop applications to modern web applications. You learned why web applications are targeted by attackers, often holding sensitive data and serving as entry points into larger systems. We covered how web requests and servers work. Finally, we learned about the protections used by security professionals to prevent, detect, and mitigate common threats to web applications.

Answer the questions below

Complete the room and continue on your cyber learning journey!

In this room, we’ve walked through the fundamentals of web security. We learned about the shift from desktop to web-based applications, why they are such attractive targets, and how attackers exploit weaknesses. We also explored web infrastructure and the layered defenses—such as secure coding, WAFs, CDNs, and logging—that protect against these threats. By practicing how to secure each layer of a web service, you’ve gained hands-on experience in strengthening web applications against real-world attacks. This knowledge lays the foundation for becoming a stronger defender in the ever-changing landscape of web security.