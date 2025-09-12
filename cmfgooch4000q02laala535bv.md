---
title: "Detecting Web Attacks (TryHackMe)"
datePublished: Fri Sep 12 2025 10:17:30 GMT+0000 (Coordinated Universal Time)
cuid: cmfgooch4000q02laala535bv
slug: detecting-web-attacks-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1757672172836/8c5efc57-5ab0-41cf-ab11-693e6843f724.png
tags: web-security, tryhackme, wireshark, loganalysis, detectingwebattacks

---

The web is one of the most exposed entry points for attackers, making web applications a frequent target in modern security incidents. From client-side exploits that trick users into compromising their own devices to server-side attacks that directly abuse application logic, adversaries continuously probe for weaknesses. Detecting these threats requires not just awareness of common attack techniques, but also the ability to interpret logs, analyze network traffic, and understand the protective role of web application firewalls (WAFs).

  
This room will guide you through the fundamentals of identifying malicious activity across various data sources, equipping you with the practical skills necessary to detect and respond to web-based attacks.

## Introduction

Web attacks are among the most common ways attackers gain entry into target systems. Public-facing websites and web applications often sit in front of databases and other infrastructure, which are appealing targets for attackers. In this room, you’ll learn how to identify these threats using practical detection methods and industry-standard tools.

## **Objectives**

* Learn common client-side and server-side attack types
    
* Understand the benefits and limitations of log-based detection
    
* Explore network traffic–based detection methods
    
* Understand how and why Web Application Firewalls are used
    
* Practice identifying common web attacks using the methods covered
    

## **Prerequisites**

Web attacks encompass a wide range of techniques. In this room, you will cover a brief overview of several common attacks before learning how to detect them. To get the most out of the exercises, you should have a foundational understanding of these attack types and some familiarity with analyzing logs and packet captures.

* [OWASP Top 10](https://tryhackme.com/room/owasptop102021) covers the ten most critical web security risks
    
* Complete [Intro to Log Analysis](https://tryhackme.com/room/introtologanalysis) for an overview of logs and useful indicators
    
* [Wireshark: The Basics](https://tryhackme.com/room/wiresharkthebasics) provides a great introduction to packet capture analysis
    

## Client-Side Attacks

**Client-side** attacks rely on abusing weaknesses in user behavior or on the user's device. These attacks often exploit vulnerabilities in browsers or trick the user into performing unsafe actions to gain access to accounts and steal sensitive information. Valuable data can be stored on a user's device, so a successful client-side attack can result in data loss. With the demand for dynamic and versatile web applications rising, companies are integrating more third-party plugins, broadening the attack surface in browsers, and opening more opportunities for client-side attacks.

Imagine browsing your favorite e-commerce site and clicking on an image of a product you are interested in. Unknown to you, an attacker has hidden a malicious invisible window inside the page that loads another site in the background. This hidden site runs malicious code that steals your login session cookies. Nothing seems out of the ordinary, but now the attacker is able to impersonate you and access your account!

## **SOC Limitations**

The tools available to an analyst, such as server-side logs and network traffic captures, offer little to no visibility into what occurs inside a user's browser. As discussed above, client-side attacks occur on the user's system, meaning they can execute malicious code, steal information, or manipulate the environment without generating suspicious HTTP requests or network traffic that the SOC can see. As a result, detecting these attacks from a SOC perspective is often difficult or even impossible without additional browser-side security controls or endpoint monitoring.

![A graphic showing an attacker performing a client-side attack on a victim and the SOC having no visibility into the attack surface.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1756210099234.svg align="left")

## **Common Client-Side Attacks**

* [**Cross-Site Scripting**](https://tryhackme.com/room/axss) (XSS) is the [most common](https://www.hackerone.com/blog/how-cross-site-scripting-vulnerability-led-account-takeover) client-side attack, in which malicious scripts are run in a trusted website and executed in the user's browser. If your website has a comment box that doesn't filter input, an attacker could post a comment like: `Hello <script>alert('You have been hacked');</script>`. When visitors load the page, the script runs inside their browser, and the pop-up appears. In a real attack, instead of a harmless pop-up, the attacker could steal cookies or session data.
    
* [**Cross-Site Request Forgery**](https://tryhackme.com/room/csrfV2) (CSRF): The browser is tricked into sending unauthorized requests on behalf of the trusted user.
    
* **Clickjacking**: Attackers overlay invisible elements on top of legitimate content, making users believe they are interacting with something safe.
    

### Answer the questions below

What class of attacks relies on exploiting the user's behavior or device? `Client-Side`

What is the most common client-side attack? `XSS`

## Server-Side Attacks

**Server-side** attacks rely on exploiting vulnerabilities within a web server, the application's code, or the backend that supports a website or a web application. While client-side attacks manipulate users' interaction with a site, server-side attacks focus on taking advantage of the systems themselves. By exploiting flaws and vulnerabilities, server logic, misconfigurations, or input handling, attackers can gain access, steal information, and cause damage to running services.

Your favorite website is most likely filled with forms that allow user input. These could be login forms that accept a username or password, or search forms to search for past orders or specific products. Imagine if the website mishandles the input from just one of these forms. This vulnerability could allow an attacker to access sensitive customer or financial information stored in the backend database.

## **Catching Server-Side Attacks**

One advantage for defenders when dealing with server-side attacks is that they leave a trail of evidence, if you know where to look. Every web request sent to an application is processed by the server and is recorded in logs or other monitoring systems. These requests also travel across the network, meaning network traffic can reveal suspicious behavior. In the upcoming tasks, we will identify server-side attacks in both logs and network traffic.

![An attacker sending a suspicious request to a web server. The request leaves behind a trail of evidence in the form of network traffic and a log file.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1756209970986.svg align="left")

## **Common Server-Side Attacks**

* [**Brute-force**](https://tryhackme.com/room/passwordattacks) attacks occur when an attacker repeatedly attempts different usernames or passwords in an attempt to gain unauthorized access to an account. Automated tools are often used to send these requests quickly, allowing attackers to go through large lists of credentials and common passwords. [T-Mobile](https://www.fierce-network.com/operators/t-mobile-ceo-says-hacker-used-brute-force-attacks-to-breach-it-servers) faced a breach in 2021 that stemmed from a brute-force attack, allowing attackers access to the personally identifiable information (PII) of over 50 million T-Mobile customers.
    
* [**SQL Injection**](https://tryhackme.com/room/sqlinjectionlm) (SQLi) relies on attacking the database that sits behind a website and occurs when applications build queries through string concatenation instead of using parameterized queries, allowing attackers to alter the intended SQL command and access or manipulate data. In 2023, an SQLi vulnerability in [MOVEit](https://www.akamai.com/blog/security-research/moveit-sqli-zero-day-exploit-clop-ransomware), a file-transfer software, was exploited, affecting over 2,700 organizations, including U.S. government agencies, the BBC, and British Airways.
    
* [**Command Injection**](https://tryhackme.com/room/oscommandinjection) is a [common attack](https://krishnag.ceo/blog/the-2024-cwe-top-25-understanding-and-mitigating-cwe-78-os-command-injection/) that occurs when a website takes user input and passes it to the system without checking it. Attackers can sneak in commands, making the server run them with the same permissions as the application.
    

### Answer the questions below

What class of attacks relies on exploiting vulnerabilities within web servers? `Server-Side`

Which server-side attack lets attackers abuse forms to dump database contents? `SQLi`

## Log-Based Detection

Logs can be a valuable tool for detecting web attacks. Every request sent to a web server can leave evidence in its access and error logs. Defenders can spot patterns that reveal scanning, exploitation attempts, or other attacks by reviewing log entries. In this task, you will review the basics of the access log format and look at how various attacks play out in web server access logs, then practice your skills in a real attack sequence scenario.

**Access Log Format**

Below is a sample access log entry. Depending on the context, each field can indicate either benign or malicious traffic. While not all access logs follow this exact format, they generally include the following information:

| **Log Field** | **Example Indicator** |
| --- | --- |
| 1\. Client IP Address | A known malicious or outside of the expected geo range |
| 2\. Timestamp and Requested Page | Requests made at unusual hours or repeated in a short period of time |
| 3\. Status Code | Repeated `404` responses indicating a page could not be found |
| 4\. Response Size | Significantly smaller or larger than normal response sizes |
| 5\. Referrer | Referring pages that don't fit normal site navigation |
| 6\. User-Agent | Outdated browser versions or common attack tools (e.g. `sqlmap`, `wpscan`) |

![A sample access log showing the different fields logged by a web server.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1755581520615.svg align="left")

## **Attacks in Logs**

Next, we'll examine an example of a condensed attack sequence. The sequence of log entries will only display the attacker's requests; in a real-world scenario, however, benign traffic would make up the vast majority of log entries, making it essential to develop a sharp eye for spotting malicious patterns amid normal activity. It is also important to note that in the example below, the query string in the SQLi attack is logged, so you will be able to see the full SQLi payloads.

1. The attacker tests for potential directories and forms to exploit with a directory fuzz. The `200` response codes signify valid finds for the attacker.
    
2. Next, the attacker exploits the `login.php` form found with a brute-force attack. Note the repeated `POST` requests in quick succession. The last `POST` request has a different response code, `302 Found`, which, in this case, signifies a successful login attempt. The page then redirects to the user's account page at `/account`.
    
3. Once the attacker gains access to the account, they attempt two SQLi payloads, `' OR '1'='1` and `1' OR 'a'='a`, on the `/search` form. If the application builds SQL queries dynamically instead of using parameterised queries, the attacker could dump the database.
    

![A sample access log showing an attack chain beginning with directory fuzzing, brute force, and SQL injection.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1755573006862.svg align="left")

## **Log Limitations**

While access logs are useful, they do not always capture the full contents of a request, particularly the body of `POST` or `GET` requests. For example, a login attempt may appear in access logs as:

`10.10.10.100 [12/Aug/2025:14:32:10] "POST /login HTTP/1.1" 200 532 "/home.html" "Mozilla/5.0"`

The above sample log shows the request method, the requested page, and the status code, but not the actual credentials or payload that was submitted in the request. Importantly, access logs do not record the actual `POST` body data, such as submitted credentials, so investigators can only see that the request occurred. `GET` requests may log full paths and query strings, but some log formats won't include them at all. It depends entirely on the server software being used and the logging configuration.

## **Investigation**

TryBankMe, a small online banking platform, has suffered a breach. Attackers broke in and leaked sensitive customer data on a darknet forum. Management believes the intrusion began through the company’s public website, and it’s up to you to uncover exactly how it happened.

Begin by opening up the `access.log` file on the desktop. 

Your mission is to analyze the logs and retrace the attacker’s steps to reveal how the breach unfolded. Good luck!

### Answer the questions below

What is the attacker's User-Agent while performing the directory fuzz?  
  
Checking the access.log file on the machine helps answer these questions:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757671287203/540d40dc-99d4-4369-b068-616128758637.png align="center")

What is the name of the page on which the attacker performs a brute-force attack?

What is the complete, [decoded](https://gchq.github.io/CyberChef/) SQLi payload the attacker uses on the `/changeusername.php` form?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757671345196/2a9b0c8e-fae4-41c7-85b6-53ef1411a8a3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757671369215/7ec917cc-b528-4a7b-9e61-b84e7948b8c4.png align="center")

## Network-Based Detection

**Network traffic analysis** allows analysts to examine the raw data exchanged between a client and a server. By capturing and inspecting packets, analysts can observe attack behavior on a more detailed level, including the underlying transport protocols and the application data itself. Network captures are more verbose than server logs, revealing the data behind every request and response. This can include full HTTP headers, POST bodies, cookies, and uploaded and downloaded files, for example.

Protocols that rely on encryption, like HTTPS and SSH, limit what can be seen in packet payloads without access to the decryption keys. For our examples, we will focus specifically on HTTP traffic in this task.

## **Attacks in Network Traffic**

Let's begin by reviewing the attack sequence from the previous task and comparing how it appears in Wireshark. Many different filters are available to you to highlight the fields you're interested in. Below, we have filtered for the destination IP address `10.10.20.200` and User-Agent using the `http.user_agent` filter.

Recall the sequence of events.

1. Directory fuzz to find valid directories or forms
    
2. Brute-force attempt with packet 13 being a successful login
    
3. SQL injection attempts
    

![Network traffic viewed in Wireshark showing an attack sequence beginning with a directory fuzz, followed by brute force, and SQL injection.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1757667081561.svg align="left")

Now that we have examined the sequence of events, let's examine the packet details to gather further evidence of the attack. First, looking at the brute force attempts. If you recall, the logs showed us repeated `POST` requests to the `login.php` form. Now we will be able to see the actual username and password that the attacker attempted to log in with. Checking out the last request to the form (packet 13), which we know was successful, we can see that the attacker found the valid password `password123`. Not a very secure password, especially for an admin account!

![A network packet inspected in Wireshark showing a brute force login attempt to /login.php.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1757139222895.svg align="left")

Next, let's look at the first SQLi attempt from the attack in much more detail. Inspecting the HTTP packet allows us to clearly see the payload used by the attacker and the results of the attack. In this case, the `' OR '1'='1` payload allowed the Users table to be dumped, displaying First name and Surname in clear text for the attacker! Note that MySQL protocol traffic can also be analyzed using Wireshark and show the payload and returned result.

![A network packet inspected in Wireshark showing a SQL injection attempt to /passreset.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1757139222765.svg align="left")

## **Investigation Continued**

While your log analysis uncovered some evidence of the attack, you could not see which user was breached or what data was actually stolen. Thankfully, we have network traffic captures when the attack occurred. Dive into the `traffic.pcap` file on the user's desktop to continue your investigation. As you look through the network traffic, keep the attack sequence from your previous investigation in mind. Best of luck!

**Tips:** 

Use the `http` filter in Wireshark to view only HTTP traffic.

You can also right-click on any packet → follow HTTP Stream to reconstruct the full request and response between the client and server.

### Answer the questions below

* After opening Wireshark, open the pcap file and look for HTTP traffic. Then, highlight it & click Analyze, and select Follow. Next, click on HTTP Stream.
    

* I found the password after switching streams upto the 32nd and the flag on the 44th stream out of 45 streams
    

What password does the attacker successfully identify in the brute-force attack?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757671619803/cbc7bf21-53c4-42ac-83a8-f716dfcf3296.png align="center")

What is the flag the attacker found in the database using SQLi?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757671642737/6f4a4cc1-0f57-4a0a-935a-367981814e76.png align="center")

## Web Application Firewall

![A visual representation of a web application firewall (WAF) blocking a suspicious request and allowing a legitimate request.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1755226220031.svg align="left")

**Web Application Firewalls (WAFs)** are often the first line of defense for websites and web applications. So far in this room, we have focused on detecting and analyzing malicious activity. Now, we will shift our focus to one of the most effective mitigation tools available. WAFs act as gatekeepers for your web applications, inspecting full request packets, similar to Wireshark but with the ability to decrypt TLS traffic and filter it before it reaches the server.

## **Rules**

WAFs inspect and decide whether to allow a web request or block it entirely based on predefined rules. Let's examine a few categories of firewall rules.

| **Rule Type** | **Description** | **Example Use Case** |
| --- | --- | --- |
| Block common attack patterns | Blocks known malicious payloads and indicators | Block malicious User-Agents: `sqlmap` |
| Deny known malicious sources | Uses IP reputation, threat intel, or geo-blocking to stop risky traffic | Block IPs from recent botnet campaigns |
| Custom-built rules | Tailored to your specific application’s needs | Allow only GET/POST requests to `/login` |
| Rate-limiting & abuse prevention | Limits request frequency to prevent abuse | Limit login attempts to 5 per minute per IP |

Imagine you notice repeated `GET` requests to `/changeusername` with the User-Agent string `sqlmap/1.9`. SQLMap is an automated tool for detecting and exploiting SQL injection vulnerabilities. After reviewing the network traffic, you see that the requests include SQLi payloads.

You might create a rule to block any User-Agent string matching `sqlmap`:

`If User-Agent contains "sqlmap"`  
`then BLOCK`

This is a rudimentary example and modern WAFs will detect and block known suspicious User-Agents automatically. However, rules like this can be custom-made to suit your specific application or threat scenario, allowing you to block malicious activity without impacting normal site traffic.

**Challenge-Response Mechanisms**

WAFs don't always need to block suspicious requests outright. For example, they can challenge requests with CAPTCHA to verify whether they come from a real user rather than a bot. This capability is extremely valuable considering that malicious bot traffic [makes up 37%](https://www.thalesgroup.com/en/worldwide/defence-and-security/press_release/artificial-intelligence-fuels-rise-hard-detect-bots) of global web traffic. This approach is beneficial for firewall rules with a higher chance of blocking legitimate web traffic.

![A list of custom firewall rules. The first rule blocks the IP 10.10.10.100, the second rule blocks the user agent Hydra, and the third rule challenges requests originating outside of the U.S. and EU.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1756708312890.svg align="left")

## **Integrating Known Indicators and Threat Intelligence**

Many modern WAF solutions include built-in rule sets designed to mitigate the [OWASP Top 10](https://owasp.org/www-project-top-ten/) security risks, some of which we have covered previously. WAFs also leverage threat intelligence feeds to automatically block requests from known malicious IP addresses and suspicious User-Agents. They receive regular updates to combat new and emerging threats, including those from known APT groups and recently discovered CVEs. [Check out](https://blog.cloudflare.com/new-waf-intelligence-feeds) how Cloudflare maintains curated IP lists, from sources like botnets, VPNs, anonymizers, and malware, based on global threat intelligence.

### Answer the questions below

What do WAFs inspect and filter? `Web Requests`

Create a custom firewall rule to block any `User-Agent` that matches `"BotTHM"`.  
`IF User-Agent CONTAINS "BotTHM" THEN block`

## Conclusion

In this room, you learned about detecting web attacks, starting with a refresher on common client-side and server-side attacks. You then covered log-based analysis, followed by network traffic analysis, in order to understand common indicators and how to spot them as an analyst. Finally, we explored Web Application Firewalls and the rules used to defend against malicious web requests. By applying correlation across these sources, you can move beyond isolated alerts and develop a more well-rounded approach to detecting and responding to web attacks.

Detecting and defending against web attacks requires a layered approach. In this room, you reviewed how attackers abuse both client-side and server-side vulnerabilities, and practiced identifying evidence of these intrusions in server logs and network captures. You also explored how WAFs act as a proactive barrier, filtering malicious requests before they reach critical systems. By combining log analysis, traffic inspection, and firewall rule creation, analysts can move from simply spotting isolated alerts to building a comprehensive defense strategy. With these skills, you are better prepared to investigate, understand, and mitigate real-world web application attacks.