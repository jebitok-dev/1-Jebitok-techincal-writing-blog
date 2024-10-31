---
title: "Web Hacking: Web Application Basics (TryHackMe)"
datePublished: Thu Oct 31 2024 14:45:24 GMT+0000 (Coordinated Universal Time)
cuid: cm2xf3omo00020akx1lnf1hnl
slug: web-hacking-web-application-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730364094201/005e0c6a-7a4c-45dc-be2f-e2322f883206.png
tags: tryhackme, write-up, webapplication, web-hacking

---

In this article, I will write a write-up for Web Hacking: Web Application Basics that covers the Web Application Overview, Uniform Resource Locator, HTPP Messages, HTTP Request: Request Line and Methods, HTTP Request: Headers and Body, HTTP Response: Status Line and Status Codes, Security Headers, and a Practical Task: Making HTTP Requests.

1. Which component on a computer is responsible for hosting and delivering content for web applications? `web server`
    
2. Which tool is used to access and interact with web applications? `web browser`
    
3. Which component acts as a protective layer, filtering incoming traffic to block malicious attacks, and ensuring the security of the the web application? `web application firewall`
    
4. Which protocol provides encrypted communication to ensure secure data transmission between a web browser and a web server? `HTTPS`
    
5. What term describes the practice of registering domain names that are misspelt variations of popular websites to exploit user errors and potentially engage in fraudulent activities? `Typosquatting`
    
6. What part of a URL is used to pass additional information, such as search terms or form inputs, to the web server? `Query String`
    
7. Which HTTP message is returned by the web server after processing a client's request? `HTTP response`
    
8. What follows the headers in an HTTP message? `Empty Line`
    
9. Which HTTP protocol version became widely adopted and remains the most commonly used version for web communication, known for introducing features like persistent connections and chunked transfer encoding? `HTTP/1.1`
    
10. Which HTTP request method describes the communication options for the target resource, allowing clients to determine which HTTP methods are supported by the web server? `OPTIONS`
    
11. In an HTTP request, which component specifies the specific resource or endpoint on the web server that the client is requesting, typically appearing after the domain name in the URL? `URL Path`
    
12. Which HTTP request header specifies the domain name of the web server to which the request is being sent? `Host`
    
13. What is the default content type for form submissions in an HTTP request where the data is encoded as key=value pairs in a query string format? `application/x-www-form-urlencoded`
    
14. Which part of an HTTP request contains additional information like host, user agent, and content type, guiding how the web server should process the request? `Request Headers`
    
15. What part of an HTTP response provides the HTTP version, status code, and a brief explanation of the response's outcome? `Status Line`
    
16. Which category of HTTP response codes indicates that the web server encountered an internal issue or is unable to fulfil the client's request? `Server Error Responses`
    
17. Which HTTP status code indicates that the requested resource could not be found on the web server? `404`
    
18. Which HTTP response header can reveal information about the web server's software and version, potentially exposing it to security risks if not removed? `Server`
    
19. Which flag should be added to cookies in the Set-Cookie HTTP response header to ensure they are only transmitted over HTTPS, protecting them from being exposed during unencrypted transmissions? `Secure`
    
20. Which flag should be added to cookies in the Set-Cookie HTTP response header to prevent them from being accessed via JavaScript, thereby enhancing security against XSS attacks? `HttpOnly`
    
21. In a Content Security Policy (CSP) configuration, which property can be set to define where scripts can be loaded from? `script-src`
    
22. When configuring the Strict-Transport-Security (HSTS) header to ensure that all subdomains of a site also use HTTPS, which directive should be included to apply the security policy to both the main domain and its subdomains? `includeSubDomains`
    
23. Which HTTP header directive is used to prevent browsers from interpreting files as a different MIME type than what is specified by the server, thereby mitigating content type sniffing attacks? `nosniff`
    
24. Make a **GET** request to `/api/users`. What is the flag? `THM{YOU_HAVE_JUST_FOUND_THE_USER_LIST}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730363852031/68cee7b7-474a-4753-8f57-de15e1420dce.png align="center")
    
25. Make a **POST** request to `/api/user/2` and update the **country** of Bob from **UK** to **US**. What is the flag? `THM{YOU_HAVE_MODIFIED_THE_USER_DATA}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730363828438/84bc47e3-d32d-46c3-b127-c65685ac4afc.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730363814913/2a2bc2d1-df5e-4162-ba32-26fbcc8a91ac.png align="center")
    
26. Make a **DELETE** request to `/api/user/1` to delete the user. What is the flag? `THM{YOU_HAVE_JUST_DELETED_A_USER}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730363782358/0f76e9d4-90cf-4f3f-af4e-8ec1ed90da1c.png align="center")
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**