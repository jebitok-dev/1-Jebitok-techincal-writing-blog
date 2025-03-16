---
title: "Web Fundamentals: OWASP Juice Shop (TryHackMe)"
datePublished: Sun Mar 16 2025 18:44:27 GMT+0000 (Coordinated Universal Time)
cuid: cm8bziy3a000v09jo4c6p7vzv
slug: web-fundamentals-owasp-juice-shop-tryhackme
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1742150617284/0010bf57-06d9-4b99-ba03-e68a96b8f8ee.png
tags: owasp, xss, tryhackme, burpsuite, owasp-top-10

---

This article will cover the [OWASP Juice Shop](https://tryhackme.com/room/owaspjuiceshop) write-up under the Web Fundamentals on THM.

## Open for business

Within this room, we will look at [OWASP's TOP 10 vulnerabilities](https://owasp.org/www-project-top-ten/) in web applications. You will find these in all types of web applications. But for today we will be looking at OWASP's own creation, Juice Shop!

![](https://i.imgur.com/vjfcwid.png align="left")

*The* ***FREE*** *Burpsuite rooms '*[*Burpsuite Basics*](https://tryhackme.com/room/burpsuitebasics)*'  and '*[*Burpsuite Repeater*](https://tryhackme.com/room/burpsuiterepeater)*'  are recommended before completing this room!  
~*

Juice Shop is a large application so we will not be covering every topic from the top 10.

We will, however, cover the following topics which we recommend you take a look at as you progress through this room.

&lt;-------------------------------------------------&gt;

[Injection](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection)

[Broken Authentication](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A2-Broken_Authentication)

[Sensitive Data Exposure](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A3-Sensitive_Data_Exposure)

[Broken Access Control](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A5-Broken_Access_Control)

[Cross-Site Scripting XSS](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A7-Cross-Site_Scripting_\(XSS\))

&lt;-------------------------------------------------&gt;

**PLEASE NOTE!**

**\[Task 3\] and onwards will require a flag, which will be displayed on completion of the task.**

![](https://i.imgur.com/t4KAlh2.png align="left")

**Troubleshooting**

The web app takes about 2-5 minutes to load, so please be patient!

Temporarily disable burp in your proxy settings for the current browser. Refresh the page and the flag will be shown. 

*(This is not an issue with the application but an issue with burp stopping the flag from being shown.* )

If you are doing the XSS Tasks and they are not working. Clear your cookies and site data, as this can sometimes be an issue. 

If you are sure that you have completed the task but it's still not working. Go to **\[Task 8\]**, as this will allow you to check its completion.

*Credits to* [*OWASP*](https://www.owasp.org/index.php/Main_Page) *and* [*Bjorn Kimminich*](https://twitter.com/bkimminich)

## Let's go on an adventure!

![](https://i.imgur.com/7R1O1CF.png align="center")

Before we get into the actual hacking part, it's good to have a look around. In Burp, set the Intercept mode to off and then browse around the site. This allows Burp to log different requests from the server that may be helpful later. 

This is called **walking through** the application, which is also a form of **reconnaissance**!

### Answer the questions below

**Question #1: What's the Administrator's email address?**

![](https://i.imgur.com/hPXolAn.png align="center")

The reviews show each user's email address. Which, by clicking on the Apple Juice product, shows us the Admin email! `admin@juice-sh.op` (check the screenshots attached)

![](https://i.imgur.com/YFWDP5v.png align="center")

**Question #2: What parameter is used for searching?** 

![](https://i.imgur.com/pZLtWQl.png align="center")

Click on the magnifying glass in the top right of the application will pop out a search bar.

![](https://i.imgur.com/bFgw6uS.png align="center")

We can then input some text and by pressing **Enter** will search for the text which was just inputted.

Now pay attention to the URL which will now update with the text we just entered.

![](https://i.imgur.com/AzJ3FAM.png align="center")

We can now see the search parameter after the **/#/search?** the letter **q** `q`

**Question #3: What show does Jim reference in his review?** 

Jim did a review on the Green Smoothie product. We can see that he mentions a replicator. 

![](https://i.imgur.com/RCFw2tB.png align="center")

If we google "**replicator**" we will get the results indicating that it is from a TV show called Star Trek `Star Trek`

![](https://i.imgur.com/Sp8ymGR.png align="center")

## Inject the juice

![](https://i.imgur.com/uwXqDdH.png align="left")

This task will be focusing on injection vulnerabilities. Injecti[on vulnerab](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection)ilities are quite dangerous to a company as they can potentially cause downtime and/or loss of data. Identifying injection points within a web application is usually quite simple, as most of them will return an error. There are many types of injection attacks, some of them are:

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>SQL Injection</strong></p></td><td colspan="1" rowspan="1"><p>SQL Injection is&nbsp;when an attacker enters a malici<a target="_self" rel="noopener noreferrer nofollow" href="https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection" style="pointer-events: none">ous or ma</a>lformed <a target="_self" rel="noopener noreferrer nofollow" href="https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection" style="pointer-events: none">query to </a>either retrieve or tamper data from a database. And in some cases, log into accounts.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Command Injection</strong></p></td><td colspan="1" rowspan="1"><p>Command Injection is when web applications ta<a target="_self" rel="noopener noreferrer nofollow" href="https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection" style="pointer-events: none">ke input </a>or user-cont<a target="_self" rel="noopener noreferrer nofollow" href="https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection" style="pointer-events: none">rolled da</a>ta and run them as system commands. An attacker may tamper with this data to execute their own system commands. This can be seen in applications that perform misconfigured ping tests.&nbsp;</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Email Injection</strong></p></td><td colspan="1" rowspan="1"><p>Email injection is a security vulnerability tha<a target="_self" rel="noopener noreferrer nofollow" href="https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection" style="pointer-events: none">t allows </a>malicious <a target="_self" rel="noopener noreferrer nofollow" href="https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection" style="pointer-events: none">users to </a>send email messages without prior authorization by the email server. These occur when the attacker adds extra data to fields, which are not interpreted by the server correctly.&nbsp;</p></td></tr></tbody></table>

But in our case, we will be using **SQL Injection**.

For more information: Injection

### **Answer the questions below**

**Question #1: Log into the administrator account!**

After we navigate to the login page, enter some data into the email and password fields.

![](https://i.imgur.com/4XHHSof.png align="left")

 **Before** clicking submit, make sure **Intercept mode is** **on**.

This will allow us to see the data been sent to the server!

  

![](https://i.imgur.com/6gyZ7Vr.png align="left")

We will now change the "**a**" next to the email to: **' or 1=1--** and forward it to the server.

![](https://i.imgur.com/tPFJnmC.png align="left")

**Why does this work?**

1. The character **'** will close the brackets in the SQL query
    
2. '**OR**' in a SQL statement will return true if either side of it is true. As **1=1 is always true**, the whole statement is true. Thus it will tell the server that the email is valid, and log us into **user id 0**, which happens to be the administrator account.
    
3. The **\--** character is used in SQL to comment out data, any restrictions on the login will no longer work as they are interpreted as a comment. This is like the **#** and **//** comment in python and javascript respectively.  
      
    `32a5e0f21372bcc1000a6088b93b458e41f0e02a`
    

  
  
(For this project just follow the steps provided and the flag will always show up on the landing page of the OWASP Juice Shop. For the first case, we’re given the email and password and we’ll also be using Burp Suite. As we move forward the credentials might change so you have to just be keen on the provided instructions)

![](https://i.imgur.com/Y7xYGjp.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742142146621/65c12a9a-d36b-46f7-978e-9b8054607f80.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742141821415/cd76254a-603f-4db8-8cff-020b78cae525.png align="center")

**Question #2: Log into the Bender account!**  

Similar to what we did in **Question #1**, we will now log into Bender's account! Capture the login request again, but this time we will put: **bender@juice-sh.op'--** as the email. 

![](https://i.imgur.com/1F1ufc3.png align="left")

Now, forward that to the server!

But why don't we put the **1=1**?

Well, as the email address is valid (which will return **true**), we do not need to force it to be **true**. Thus we are able to use **'--** to bypass the login system. Note the **1=1** can be used when the email or username is not known or invalid.  
  
`fb364762a3c102b2db932069c0e6b78e738d4066`

![](https://i.imgur.com/Rznz31B.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742142440527/094725cc-eecf-4602-9e99-04ed47f3f147.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742142577801/2312412a-c20a-47ee-b345-b596e01d5439.png align="center")

## Who broke my lock?!

![](https://i.imgur.com/OM71q2D.png align="left")

In this task, we will look at exploiting authentication through different flaws. When talking about flaws within authentication, we include mechanisms that are vulnerable to manipulation. These mechanisms, listed below, are what we will be exploiting. 

Weak passwords in high-privileged accounts

Forgotten password pages

 More information: [Broken Authentication](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A2-Broken_Authentication)

### **Answer the questions below**

**Question #1: Bruteforce the Administrator account's password!**

We have used SQL Injection to log into the Administrator account but we still don't know the password. Let's try a brute-force attack! We will once again capture a login request, but instead of sending it through the proxy, we will send it to Intruder.

Go to Positions and then select the **Clear §** button. In the password field place two § inside the quotes. To clarify, the § § is not two sperate inputs but rather Burp's implementation of quotations e.g. "". The request should look like the image below. 

![](https://i.imgur.com/I96sO28.png align="left")

For the payload, we will be using the **best1050.txt from Seclists**. (Which can be installed via: **apt-get install seclists**)

*You can load the list from: /usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt*

Once the file is loaded into Burp, start the attack. You will want to filter for the request by status.

A **failed** request will receive a **401 Unauthorized**   

Whereas a **successful** request will return a **200 OK**. 

Once completed, login to the account with the password. `c2110d06dc6f81c67cd8099ff0ba601241f1ac0e`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742142788990/a7ab7869-e5d2-4447-ba04-25e8771f834a.png align="center")

  
  

**Question #2: Reset Jim's password!**

Believe it or not, the reset password mechanism can also be exploited! When inputted into the email field in the Forgot Password page, Jim's security question is set to *"Your eldest siblings middle name?"*.

  

In Task 2, we found that Jim might have something to do with **Star Trek**. Googling "Jim Star Trek" gives us a wiki page for **Jame T. Kirk** from Star Trek. 

![](https://i.imgur.com/axsRMp2.png align="left")

  

  

Looking through the wiki page we find that he has a brother.

![](https://i.imgur.com/PfHXA1h.png align="left")

Looks like his brother's middle name is **Samuel**

Inputting that into the Forgot Password page allows you to successfully change his password.

You can change it to anything you want!  
  
`094fbc9b48e525150ba97d05b942bbf114987257`

![](https://i.imgur.com/uRS3kJr.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742142876312/3b0f4fd3-417a-42bc-8694-cb7ee61e367c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742142901122/c0ad1f91-d495-4be7-8f9c-d5acf144eb66.png align="center")

## AH! Don't look!

![](https://i.imgur.com/XlbJl1E.png align="left")

A web application should store and transmit sensitive data safely and securely. But in some cases, the developer may not correctly protect their sensitive data, making it vulnerable.

Most of the time, data protection is not applied consistently across the web application making certain pages accessible to the public. Other times information is leaked to the public without the knowledge of the developer, making the web application vulnerable to an attack. 

More information: [Sensitive Data Exposure](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A3-Sensitive_Data_Exposure)

### **Answer the questions below**

**Question #1: Access the Confidential Document!**

![](https://i.imgur.com/M1s8jfu.png align="left")

Navigate to the **About Us** page, and hover over the *"Check out our terms of use"*.

  

![](https://i.imgur.com/5PsmlD4.png align="left")

  

You will see that it links to  [http://MACHINE\_IP/ftp/legal.md](http://machine_ip/ftp/legal.md). Navigating to that **/ftp/** directory reveals that it is exposed to the public!

![](https://i.imgur.com/EY664PR.png align="left")

![](https://i.imgur.com/Xp2aZJW.png align="left")

We will download the **acquisitions.md** and save it. It looks like there are other files of interest here as well.

After downloading it, navigate to the **home page** to receive the flag!

  
`edf9281222395a1c5fee9b89e32175f1ccf50c5b`  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742143109375/27c0b56a-0150-4998-824d-4eafac5582ea.png align="center")

  

**Question #2: Log into MC SafeSearch's account!**

After watching the video there are certain parts of the song that stand out.

He notes that his password is "**Mr. Noodles**" but he has replaced some "**vowels into zeros**", meaning that he just replaced the o's into 0's.

We now know the password to the *mc.safesearch@juice-sh.op* account is "**Mr. N00dles**"

`66bdcffad9e698fd534003fbb3cc7e2b7b55d7f0`  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742143163811/dc774aba-5f21-4c39-8e46-3b17d9e05ad7.png align="center")

**Question #3: Download the Backup file!**

We will now go back to the  [http://MACHINE\_IP/ftp/](http://machine_ip/ftp/) folder and try to download **package.json.bak**. But it seems we are met with a 403 which says that only .md and .pdf files can be downloaded. 

![](https://i.imgur.com/LDUkDBQ.png align="left")

To get around this, we will use a character bypass called "**Poison Null Byte**". A Poison Null Byte looks like this: ***%00***. 

Note: as we can download it using the url, we will need to encode this into a url encoded format.

The Poison Null Byte will now look like this: ***%2500****.* Adding this and then a **.md** to the end will bypass the 403 error!

![](https://i.imgur.com/2qugsl5.png align="left")

**Why does this work?** 

A Poison Null Byte is actually a **NULL terminator**. By placing a NULL character in the string at a certain byte, the string will tell the server to terminate at that point, nulling the rest of the string.   
  
`bfc1e6b4a16579e85e06fee4c36ff8c02fb13795`  
  
Download the file within the virtual machine so that you can access it

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742143263568/13ec47dc-b5ca-4080-98bb-bde769d1ed99.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742143399912/ad22646b-3299-48c8-9ed6-024fc922d2a2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742143510078/d7d3a495-21c4-4c79-a580-ab5cbc2a6106.png align="center")

## Who's flying this thing?

![](https://i.imgur.com/r2qq6de.png align="left")

Modern-day systems will allow for multiple users to have access to different pages. Administrators most commonly use an administration page to edit, add and remove different elements of a website. You might use these when you are building a website with programs such as Weebly or Wix.  

When Broken Access Control exploits or bugs are found, it will be categorised into one of **two types**:

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>Horizontal </strong>Privilege Escalation</p></td><td colspan="1" rowspan="1"><p>Occurs when a user can perform an action or access data of another user with the <strong>same </strong>level of permissions.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Vertical </strong>Privilege Escalation</p></td><td colspan="1" rowspan="1"><p>Occurs when a user can perform an action or access data of another user with a&nbsp;<strong>higher </strong>level of permissions.</p></td></tr></tbody></table>

![](https://i.imgur.com/bJ9WKY4.png align="left")

*Credits: Packetlabs.net*

More information: [Broken Access Control](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A5-Broken_Access_Control)

### **Answer the questions below**

**Question #1: Access the administration page!**

First, we are going to open the **Debugger** on **Firefox**. 

(Or **Sources** on **Chrome**.)

This can be done by navigating to it in the Web Developers menu. 

![](https://i.imgur.com/rvhCm6V.png align="left")

We are then going to refresh the page and look for a javascript file for **main-es2015.js**

We will then go to that page at: [http://MACHINE\_IP](http://machine_ip/)/main-es2015.js

![](https://i.imgur.com/wF55kiO.png align="left")

To get this into a format we can read, click the { } button at the bottom  

Now search for the term "admin" 

You will come across a couple of different words containing "admin" but the one we are looking for is "path: administration"

![](https://i.imgur.com/AS1YVjU.png align="left")

This hints towards a page called "**/#/administration**" as can be seen by the **about** path a couple lines below, but going there while not logged in doesn't work. 

As this is an Administrator page, it makes sense that we need to be in the **Admin account** in order to view it.

A good way to stop users from accessing this is to only load parts of the application that need to be used by them. This stops sensitive information such as an admin page from been leaked or viewed.

  
`946a799363226a24822008503f5d1324536629a0   `  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742147800832/5fc5f6ba-8904-409e-8bbf-d1ea58bbb8e2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742147971426/5629b17f-9004-4ea8-a5dd-ea04076abd8e.png align="center")

**Question #2: View another user's shopping basket!**

Login to the Admin account and click on 'Your Basket'. Make sure Burp is running so you can capture the request!

Forward each request until you see: *GET /rest/basket/1 HTTP/1.1*

![](https://i.imgur.com/wlS88AU.png align="left")

Now, we are going to change the number **1** after /basket/ to **2**

![](https://i.imgur.com/ugsRunL.png align="left")

It will now show you the basket of UserID 2. You can do this for other UserIDs as well, provided that they have one!  
  
`41b997a36cc33fbe4f0ba018474e19ae5ce52121`

![](https://i.imgur.com/yR4xFo3.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742148344110/14326d4b-1422-4823-8bdc-67ec3d0e3b15.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742148360465/17c0baea-10cb-4d49-a2dd-a745d88f46a8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742148439860/dd353b56-5c14-4c7f-b0ec-a047a0cc5bc9.png align="center")

**Question #3: Remove all 5-star reviews!**

Navigate to the  [http://MACHINE\_IP/#/administration](http://machine_ip/#/administration) page again and click the bin icon next to the review with 5 stars!  
  
`50c97bcce0b895e446d61c83a21df371ac2266ef`

![](https://i.imgur.com/cI2sSyx.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742148537127/d080a0b4-a90e-4562-be0f-f380b575025d.png align="center")

## Where did that come from?

![](https://i.imgur.com/qBdgNKC.png align="left")

XSS or Cross-site scripting is a vulnerability that allows attackers to run javascript in web applications. These are one of the most found bugs in web applications. Their complexity ranges from easy to extremely hard, as each web application parses the queries in a different way. 

**There are three major types of XSS attacks:**

<table><tbody><tr><td colspan="1" rowspan="1"><p>DOM (Special)</p></td><td colspan="1" rowspan="1"><p><strong>DOM XSS</strong>&nbsp;<em>(Document Object Model-based Cross-site Scripting)</em>&nbsp;uses the HTML environment to execute malicious javascript. This type of attack commonly uses the&nbsp;<em>&lt;script&gt;&lt;/script&gt;</em>&nbsp;HTML tag.</p></td></tr><tr><td colspan="1" rowspan="1"><p>Persistent (Server-side)</p></td><td colspan="1" rowspan="1"><p><strong>Persistent XSS</strong>&nbsp;is javascript that is run when the server loads the page containing it. These can occur when the server does not sanitise the user data when it is <strong>uploaded </strong>to a page. These are commonly found on blog posts.&nbsp;</p></td></tr><tr><td colspan="1" rowspan="1"><p>Reflected (Client-side)</p></td><td colspan="1" rowspan="1"><p><strong>Reflected XSS</strong>&nbsp;is javascript that is run on the client-side end of the web application. These are most commonly found when the server doesn't sanitise <strong>search </strong>data.&nbsp;</p></td></tr></tbody></table>

  
More information: [Cross-Site Scripting XSS](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A7-Cross-Site_Scripting_\(XSS\))

### **Answer the questions below**

**Question #1: Perform a DOM XSS!**

![](https://i.imgur.com/AMz9jps.png align="left")

We will be using the iframe element with a javascript alert tag: 

*&lt;iframe src="javascript:alert(\`xss\`)"&gt;* 

Inputting this into the **search bar** will trigger the alert.

![](https://i.imgur.com/rKEx3aR.png align="left")

Note that we are using **iframe** which is a common HTML element found in many web applications, there are others which also produce the same result. 

This type of XSS is also called XFS (Cross-Frame Scripting), is one of the most common forms of detecting XSS within web applications.

Websites that allow the user to modify the iframe or other DOM elements will most likely be vulnerable to XSS.   

**Why does this work?**

It is common practice that the search bar will send a request to the server in which it will then send back the related information, but this is where the flaw lies. Without correct input sanitation, we are able to perform an XSS attack against the search bar. 

`9aaf4bbea5c30d00a1f5bbcfce4db6d4b0efe0bf   `  
  
Following the provided steps we’re supposed to search ``<iframe src="javascript:alert(`xss`)">`` *on the search bar. Next, we’ll click on the Javascript alert that will pop up on the page by clicking ‘****Ok****’ and we’ll get our flag*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742148967323/91f20c0b-73c3-4d90-bd7f-b003c6958928.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742149038973/a5e1944c-f6e4-406e-8b32-63d063bdddc6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742149065939/3ed31b62-0cb5-4c61-975f-e7eac5f94b4c.png align="center")

**Question #2: Perform a persistent XSS!**  

First, login to the **admin** account.

We are going to navigate to the "**Last Login IP**" page for this attack.

![](https://i.imgur.com/YTIzhh0.png align="left")

It should say the last IP Address is 0.0.0.0 or 10.x.x.x 

As it logs the 'last' login IP we will now logout so that it logs the 'new' IP.

![](https://i.imgur.com/4XHHSof.png align="left")

Make sure that Burp **intercept is on**, so it will catch the logout request.

We will then head over to the Headers tab where we will add a new header:

<table><tbody><tr><td colspan="1" rowspan="1"><p><em>True-Client-IP</em></p></td><td colspan="1" rowspan="1"><p><em>&lt;iframe src="javascript:alert(`xss`)"&gt;</em></p></td></tr></tbody></table>

![](https://i.imgur.com/VLd2Fga.png align="left")

Then forward the request to the server!  
When **signing back into the admin account** and navigating to the Last Login IP page again, we will see the XSS alert!

![](https://i.imgur.com/rKEx3aR.png align="left")

**Why do we have to send this Header?**

The *True-Client-IP*  header is similar to the *X-Forwarded-For* header, both tell the server or proxy what the IP of the client is. Due to there being no sanitation in the header we are able to perform an XSS attack.   
  
`149aa8ce13d7a4a8a931472308e269c94dc5f156`  
  
check the last login IP

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742149417695/1ed2bd8d-244d-40b0-9151-040e13392759.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742149487637/adf1f86f-f325-475b-8da6-0d4981c7baf5.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742149559366/89498eed-bd83-4f26-ac8c-8cbe33b64547.png align="center")

**Question #3: Perform a reflected XSS!**

First, we are going to need to be on the right page to perform the reflected XSS!

**Login** into the **admin account** and navigate to the '**Order History**' page. 

![](https://i.imgur.com/hBy4O1L.png align="left")

From there you will see a "**Truck**" icon, clicking on that will bring you to the track result page. You will also see that there is an id paired with the order.   

We will use the iframe XSS, *&lt;iframe src="javascript:alert(\`xss\`)"&gt;,* in the place of the *5267-f73dcd000abcc353*

After submitting the URL, refresh the page and you will then get an alert saying XSS!

![](https://i.imgur.com/rKEx3aR.png align="left")

**Why does this work?**

The server will have a lookup table or database (depending on the type of server) for each tracking ID. As the 'id' parameter is not sanitised before it is sent to the server, we are able to perform an XSS attack.    

`23cefee1527bde039295b2616eeb29e1edc660a0`  
  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742149857444/223beed8-1952-47bf-b8d0-4724dd9ab861.png align="center")

## Exploration!

![](https://i.imgur.com/DGSYlWp.png align="left")

If you wish to tackle some of the **harder** challenges that were not covered within this room, check out the **/#/score-board/** section on Juice-shop. Here you can see your completed tasks as well as other tasks in varying difficulty.

### **Answer the questions below**

**Access the /#/score-board/ page**

`7efd3174f9dd5baa03a7882027f2824d2f72d86e`  
  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742149805751/ef14ad9d-9249-439e-a172-09efbcaafd18.png align="center")

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the Lab THM challenges.