---
title: "XSS - Merry XSSMas (TryHackMe) 🎉🧑‍🎄"
datePublished: Fri Dec 12 2025 21:48:45 GMT+0000 (Coordinated Universal Time)
cuid: cmj3eftk9000202ju6cat1y7l
slug: xss-merry-xssmas-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765576039497/9694bd23-0e6d-4b21-8ac0-2062b3e5e6f1.png
tags: xss, tryhackme, portswigger, reflected-xss, stored-xss, adventofcyber2025

---

Cross-Site Scripting (XSS) remains one of the most deceptive and flexible web vulnerabilities because it blurs the line between user input and executable code. A single unchecked text field can allow an attacker to rewrite what a user sees, steal information, or even control a session — all without ever touching the backend server. One of the most overlooked behaviours of XSS is how easily it can modify the **page’s URL**, especially in reflected-XSS scenarios. A simple search term can transform the URL into a delivery vehicle for malicious scripts, tricking users into loading harmful content disguised as normal queries.

In this challenge, we step into McSkidy’s secure message portal, a system that has recently been flooded with strange logs, suspicious inputs, and payload-like search phrases. Our task is to analyse its behaviour, understand how XSS manifests within the portal, and uncover who is slipping malicious content into Santa’s systems.

Before diving into the technical details, we will revisit the core concepts of XSS, explore how Reflected and Stored attacks differ, and understand why proper sanitisation is essential for protecting any modern web platform.

## Introduction

**The Story**

![Task banner DAY 11](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f9c7574e201fe31dad228fc/room-content/5f9c7574e201fe31dad228fc-1763406635391.png align="left")

After last year's automation and tech modernisation, Santa's workshop got a new makeover. McSkidy has a secure message portal where you can contact her directly with any questions or concerns. However, lately, the logs have been lighting up with unusual activity, ranging from odd messages to suspicious search terms. Even Santa's letters appear to be scripts or random code. Your mission, should you choose to accept it: dig through the logs, uncover the mischief, and figure out who's trying to mess with McSkidy. 

## **Learning Objectives**

* Understand how XSS works
    
* Learn to prevent XSS attacks
    

## Leave the Cookies, Take the Payload

## **Equipment Check**

For today's room we will be using a the web app found under http://MACHINE\_IP. You can use the browser of your AttackBox to navigate to it. You will see a page as shown below:

![Portal page](https://tryhackme-images.s3.amazonaws.com/user-uploads/6890a24ce8a6d10e9ad22d67/room-content/6890a24ce8a6d10e9ad22d67-1761292275782.png align="left")

Let's review some key material regarding potential attacks on websites like this portal, specifically [Cross-Site Scripting (XSS)](https://tryhackme.com/room/axss). 

XSS is a web application vulnerability that lets attackers (or evil bunnies) inject malicious code (usually JavaScript) into input fields that reflect content viewed by other users (e.g., a form or a comment in a blog). When an application doesn't properly validate or escape user input, that input can be interpreted as code rather than harmless text. This results in malicious code that can steal credentials, deface pages, or impersonate users. Depending on the result, there are various types of XSS.  In today’s task, we focus on **Reflected XSS** and **Stored XSS**.

## **Reflected XSS**

You see reflected variants when the injection is immediately projected in a response. Imagine a toy search function in an online toy store, you search via:

`https://trygiftme.thm/search?term=gift`

But imagine you send this to your friend who is looking for a gift for their nephew (please don't do this):

`https://trygiftme.thm/search?term=<script>alert( atob("VEhNe0V2aWxfQnVubnl9") )</script>`

If your friend clicks on the link, it will execute code instead.

**Impact**

You could act, view information, or modify information that your friend or any user could do, view, or access. It's usually exploited via phishing to trick users into clicking a link with malicious code injected.

## **Stored XSS**

A Stored XSS attack occurs when malicious script is saved on the server and then loaded for every user who views the affected page. Unlike Reflected XSS, which targets individual victims, Stored XSS becomes a "set-and-forget" attack, anyone who loads the page runs the attacker’s script.

To understand how this works, let’s use the example of a simple blog where users can submit comments that get displayed below each post.

## **Normal Comment Submission**

```http
POST /post/comment HTTP/1.1
Host: tgm.review-your-gifts.thm

postId=3
name=Tony Baritone
email=tony@normal-person-i-swear.net
comment=This gift set my carpet on fire but my kid loved it!
```

The server stores this information and displays it whenever someone visits that blog post.

## **Malicious Comment Submission (Stored XSS Example)**

If the application does not sanitize or filter input, an attacker can submit JavaScript instead of a comment:

```http
POST /post/comment HTTP/1.1
Host: tgm.review-your-gifts.thm

postId=3
name=Tony Baritone
email=tony@normal-person-i-swear.net
comment=<script>alert(atob("VEhNe0V2aWxfU3RvcmVkX0VnZ30="))</script> + "This gift set my carpet on fire but my kid loved it!"
```

Because the comment is saved in the database, every user who opens that blog post will automatically trigger the script.

This lets the attacker run code as if they were the victim in order to perform malicious actions such as: 

* Steal session cookies
    
* Trigger fake login popups
    
* Deface the page
    

## **Protecting against XSS**

Each service is different, and requires a well-thought-out, secure design and implementation plan, but key practices you can implement are:

* **Disable dangerous rendering raths:** Instead of using the `innerHTML` property, which lets you inject any content directly into HTML, use the `textContent` property instead, it treats input as text and parses it for HTML.
    
* **Make cookies inaccessible to JS:** Set session cookies with the [HttpOnly](https://owasp.org/www-community/HttpOnly), [Secure](https://owasp.org/www-community/controls/SecureCookieAttribute), and [SameSite](https://owasp.org/www-community/SameSite) attributes to reduce the impact of XSS attacks.
    
* **Sanitise input/output and encode:**
    
    * In some situations, applications may need to accept limited HTML input—for example, to allow users to include safe links or basic formatting. However it's critical to sanitize and encode all user-supplied data to prevent security vulnerabilities. Sanitising and encoding removes or escapes any elements that could be interpreted as executable code, such as scripts, event handlers, or JavaScript URLs while preserving safe formatting.
        

To exploit XSS vulnerabilities, we need some type of input field to inject code. There is a search section, let's start there.

## **Exploiting Reflected XSS**

To exploit reflected XSS, we can use test payloads to check if the app runs the code injected. If you want to test more advanced payloads, there are [cheat sheets](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) online that you can use to craft them. For now, we'll pick the following payload:

`<script>alert('Reflected Meow Meow')</script>`

Inject the code by adding the payload to the search bar and clicking " **Search Messages**". If it shows the alert text, we have confirmed reflected XSS. So, what happened?

* * The search input is reflected directly in the results without encoding
        
    * The browser interprets your HTML/JavaScript as executable code
        
    * An alert box appeared, demonstrating successful XSS execution
        

You can track the behaviour and how the system interprets your actions by checking the " **System Logs**" tab at the bottom of the page:

![System logs](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/61a7523c029d1c004fac97b3-1760342225658.png align="left")

Now that we have confirmed reflected XSS, let's investigate if it's susceptible to stored XSS. This vector must be different, as it needs to be persisted. Looking at the website, we can see that you are able to send messages, which are stored on the server for McSkidy to view later (as opposed to searching, which is stored temporarily on the client side).

Navigate to the message form, and enter the malicious payload we used before (others work too): 

`<script>alert('Stored Meow Meow')</script>`

Click the " **Send Message**" button. Because messages are stored on the server, every time you navigate to the site or reload, the alert will display.

## **Wrapping Up**

So it's confirmed! The site is vulnerable to XSS; it's no wonder that unusual payloads have been detected in the logs. The team will now harden the site to prevent future malicious code from being injected.

### Answer the questions below

1. Which type of XSS attack requires payloads to be persisted on the backend?
    
2. What's the reflected XSS flag?  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765575897638/717d9ae5-b64b-40da-a9de-e53b8fcbf107.jpeg align="center")
    
3. What's the stored XSS flag?  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765575910873/ff4259b8-335a-4aed-a42e-67adb409e6fc.jpeg align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765575928436/bed0636e-8663-4a58-ac32-09f0c3502393.jpeg align="center")
    
4. If you enjoyed todays's room, you might want to have a look at the [Intro to Cross-site Scripting](https://tryhackme.com/room/xss) room!
    

This challenge demonstrates how subtle and dangerous Cross-Site Scripting vulnerabilities can be. From search inputs that manipulate the URL to stored messages that execute on every page load, XSS thrives anywhere user-controlled data is handled without validation or encoding. Through hands-on testing, we confirmed that both Reflected and Stored XSS were present, reinforcing why even simple features like search bars and message forms require robust protection.

By understanding how these vulnerabilities work — and seeing the impact firsthand — we gain a clearer picture of why secure development practices, proper sanitisation, safe rendering methods, and strict cookie controls are essential. With the findings uncovered, McSkidy’s team can now harden the portal and prevent attackers from injecting scripts into Santa’s systems again.