---
title: "Introduction to Web Hacking: Subdomain Enumeration (TryHackMe)"
datePublished: Wed Feb 05 2025 02:23:58 GMT+0000 (Coordinated Universal Time)
cuid: cm6rabtda00000ajx64db3bi8
slug: introduction-to-web-hacking-subdomain-enumeration-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1738722187246/8a545881-d183-44e2-bec2-8f6a38dcf85e.png
tags: osint, tryhackme, write-up, ffuf, subdomain-enumeration, sublist3r

---

This article will cover the [Subdomain Enumeration](https://tryhackme.com/room/subdomainenumeration) write-up under the Web Fundamentals on THM.

Subdomain enumeration is the process of finding valid subdomains for a domain, but why do we do this? We do this to expand our attack surface to try and discover more potential points of vulnerability.

We will explore three different subdomain enumeration methods: Brute Force, OSINT (Open-Source Intelligence), and Virtual Host.

Start the machine and then move on to the next task.

**Answer the questions below**

1. What is a subdomain enumeration method beginning with B? `Brute Force`
    
2. What is a subdomain enumeration method beginning with O? `OSINT`
    
3. What is a subdomain enumeration method beginning with V? `Virtual Host`
    

## OSINT - SSL/TLS Certificates

**SSL/TLS Certificates**

When an SSL/TLS (Secure Sockets Layer/Transport Layer Security) certificate is created for a domain by a CA (Certificate Authority), the CA takes part in what's called "Certificate Transparency (CT) logs". These are publicly accessible logs of every SSL/TLS certificate created for a domain name. The purpose of Certificate Transparency logs is to stop malicious and accidentally made certificates from being used. We can use this service to our advantage to discover subdomains belonging to a domain, sites like [https://crt.sh](http://crt.sh/) and [https://ui.ctsearch.entrust.com/ui/ctsearchui](https://ui.ctsearch.entrust.com/ui/ctsearchui) offer a searchable database of certificates that show current and historical results.

Go to [crt.sh](https://crt.sh/) and search for the domain name **tryhackme.com**, find the entry that was logged on **2020-12-26,** and enter the domain below to answer the question.

**Answer the questions below**

1. What domain was logged on crt.sh on 2020-12-26? `store.tryhackme.com`
    
    visit the provided date and check the given date, and you’ll find the domain
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738721638963/42bbe16c-e508-42ef-94c4-2eb8b6a4ed3b.png align="center")
    

## OSINT - Search Engines

**Search Engines**

Search engines contain trillions of links to more than a billion websites, which can be an excellent resource for finding new subdomains. Using advanced search methods on websites like Google, such as the site: filter, can narrow the search results. For example, `site:*.domain.com -site:www.domain.com` would only contain results leading to the domain name domain.com but exclude any links to www.domain.com; therefore, it shows us only subdomain names belonging to domain.com.

Go to [Google](https://www.google.com/) and use the search term `site:*.tryhackme.com -site:www.tryhackme.com`, which should reveal a subdomain for tryhackme.com; use that subdomain to answer the question below.

**Answer the questions below**

1. What is the TryHackMe subdomain beginning with **S** discovered using the above Google search? `store.tryhackme.com`
    

## DNS Bruteforce

Bruteforce DNS (Domain Name System) enumeration is the method of trying tens, hundreds, thousands or even millions of different possible subdomains from a pre-defined list of commonly used subdomains. Because this method requires many requests, we automate it with tools to make the process quicker. In this instance, we are using a tool called dnsrecon to perform this. Click the "View Site" button to open the static site, press the "Run DNSrecon Request" button to start the simulation, and then answer the question below.

**Answer the questions below**

1. What is the first subdomain found with the dnsrecon tool? `api.acmeitsupport.thm`
    
    using the `dnsrecon` command we’ll find the subdomains and we’ll pick the first as expected
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738721780932/98301de1-dfcf-4f61-90bb-7d7b01e81999.png align="center")
    

## OSINT - Sublist3r

**Automation Using Sublist3r**

To speed up the process of OSINT subdomain discovery, we can automate the above methods with the help of tools like [Sublist3r](https://github.com/aboul3la/Sublist3r), click the "View Site" button to open up the static site and run the sublist3r simulation to discover a new subdomain that will help answer the question below.

**Answer the questions below**

1. What is the first subdomain discovered by sublist3r? `web55.acmeitsupport.thm`
    
    using the sublist3r we’ll identify the subdomain as shown on the images below:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738721900568/6b249d18-afda-4afe-9e41-d717cc4c24f6.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738721927562/cee62f4c-e689-4525-87bf-d46d12a9ac39.png align="center")

Virtual Hosts

Some subdomains aren't always hosted in publically accessible DNS results, such as development versions of a web application or administration portals. Instead, the DNS record could be kept on a private DNS server or recorded on the developer's machines in their /etc/hosts file (or c:\\windows\\system32\\drivers\\etc\\hosts file for Windows users) which maps domain names to IP addresses.

Because web servers can host multiple websites from one server when a website is requested from a client, the server knows which website the client wants from the **Host** header. We can utilise this host header by making changes to it and monitoring the response to see if we've discovered a new website.

Like with DNS Bruteforce, we can automate this process by using a wordlist of commonly used subdomains.

Start an AttackBox and then try the following command against the Acme IT Support machine to try and discover a new subdomain.

`ffuf`

```plaintext
user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP
```

The above command uses the **\-w** switch to specify the wordlist we are going to use. The **\-H** switch adds/edits a header (in this instance, the Host header), we have the **FUZZ** keyword in the space where a subdomain would normally go, and this is where we will try all the options from the wordlist.

Because the above command will always produce a valid result, we need to filter the output. We can do this by using the page size result with the **\-fs** switch. Edit the below command replacing {size} with the most occurring size value from the previous result and try it on the AttackBox.

`ffuf`

```plaintext
user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP -fs {size}
```

This command has a similar syntax to the first apart from the **\-fs** switch, which tells ffuf to ignore any results that are of the specified size.

The above command should have revealed two positive results that we haven't come across before.

**Answer the questions below**

1. What is the first subdomain discovered? `delta`
    
    using `ffuf` command we’re able to identify the answers to question 1 & 2
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738722035307/79df6cc7-75c5-4617-8e3d-8906b2a2262f.png align="center")
    
2. What is the second subdomain discovered? `yellow`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the Lab THM challenges.