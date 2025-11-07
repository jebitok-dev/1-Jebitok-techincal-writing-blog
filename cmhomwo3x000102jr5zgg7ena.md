---
title: "Spring4Shell: CVE-2022-22965 (TryHackMe)"
datePublished: Fri Nov 07 2025 09:09:33 GMT+0000 (Coordinated Universal Time)
cuid: cmhomwo3x000102jr5zgg7ena
slug: spring4shell-cve-2022-22965-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762506395977/b5d118df-8897-4a6f-840f-f3f4a875e14c.png
tags: tryhackme, cve, rce, exploitation, ctf-writeup

---

In late March 2022 the security community was alerted to remote code execution flaws affecting the Spring Framework. One of the issues—now commonly referred to as **Spring4Shell** (CVE-2022-22965)—impacts parts of *Spring Core* and, under a specific set of conditions, allows an attacker to inject and execute code on a vulnerable server. Although the exploitability depends on several environmental factors (JDK ≥ 9, certain Spring versions, Tomcat deployed as a WAR and use of spring-webmvc/webflux), the vulnerability drew attention because it can permit an adversary to write a JSP webshell into the application webroot and achieve full command execution.

This room walks through the vulnerability at a high level and then gives you a hands-on opportunity to exploit it in a controlled lab. You will (1) identify the vulnerable POST endpoint, (2) run a reviewed proof-of-concept to upload a webshell, and (3) use that shell to execute commands and retrieve the lab flag. The goal is not only to demonstrate exploitation but to learn how the issue arises, what makes a deployment vulnerable, and how to mitigate the risk in real applications.

## Info: Introduction and Deploy

In late March 2022, two remote command execution vulnerabilities in the Java [Spring framework](https://spring.io/) were made public. The first of these vulnerabilities affects a component of the framework called "Spring Cloud Functions". The second, arguably more serious vulnerability, affects a component in  "Spring Core" —  the heart of the framework — thus significantly increasing the vulnerability's potential impact and earning it the name "Spring4Shell" (a play on [Log4Shell](https://tryhackme.com/room/solar), the name of a brutal vulnerability disclosed at the end of 2021).

For various reasons, there has been a lot of confusion surrounding these vulnerabilities in the wider infosec community. As such, this room may be updated as new information comes to light. On a similar note, the impact of Spring4Shell is currently unknown; only time will tell how wide-spread the vulnerability is in the wild.

This room will provide an overview of the Spring4Shell RCE vulnerability in Spring Core, as well as give you an opportunity to exploit it for yourself in the vulnerable machine attached to this task. We will start by taking a look at the vulnerability at a high-level, before exploiting the target machine for ourselves.

Let's begin!

## Tutorial: Vulnerability Background

**Overview**

Spring4Shell was originally released as an 0-day in a now-deleted thread of Tweets. It was quickly identified as a bypass of the patch for [CVE-2010-1622](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2010-1622) — a vulnerability in earlier versions of the Spring Framework which allowed attackers to obtain remote command execution by abusing the way in which Spring handles data sent in HTTP requests. In short, the vulnerability allows attackers to upload a "webshell" (a piece of code which accepts commands from the attacker that the webserver is then tricked into executing) to the vulnerable server, achieving remote command execution.

**How Does it Work?**

To understand Spring4Shell, it is important that we understand CVE-2010-1622. Spring MVC (**M**odel-**V**iew-**C**ontroller) is part of the Spring Framework which makes it easy to develop web applications following the MVC design pattern. One of the features of Spring MVC is that it automatically instantiates and populates an object of a specified class when a request is made based on the parameters sent to the endpoint. In simple terms, this could be abused to overwrite important attributes of the parent class, resulting in remote code execution.

Spring4Shell works along similar lines, bypassing the mitigations that were added to patchCVE-2010-1622. The majority of the exploits for the Spring4Shell vulnerability operate by forcing the application to write a malicious`.jsp`file (effectively plaintext Java which Tomcat can execute — much like a PHP webserver would execute files with a`.php`extension) to the webserver. This webshell can then be executed to gain remote command execution over the target.  

**Limitations**

Fortunately, despite how commonly used the Spring Framework is, the conditions in which the vulnerability can be exploited are actually fairly limited.

The Spring4Shell vulnerability affects Spring Core before version 5.2, as well as in versions 5.3.0-17 and 5.2.0-19, running on a version of the **J**ava **D**evelopment **K**it (JDK) greater than or equal to 9. The publicly available exploits currently available only work on applications deployed to Apache Tomcat as WARs; however, the Spring Framework maintainers have stated that they believe there may be other ways to exploit the vulnerability.

Current conditions for vulnerability (as stated in [Spring's announcement of the vulnerability](https://spring.io/blog/2022/03/31/spring-framework-rce-early-announcement)) can be summarised as follows:

* JDK 9+
    
* A vulnerable version of the Spring Framework (&lt;5.2 | 5.2.0-19 | 5.3.0-17)
    
* Apache Tomcat as a server for the Spring application, packaged as a WAR
    
* A dependency on the `spring-webmvc` and/or `spring-webflux` components of the Spring Framework
    

It is worth noting, however, that these may change over time as other ways to exploit the vulnerability are discovered.

**Remediations**

Fortunately, patched versions of the Spring Framework have been released. To remediate Spring4Shell, ensure that you are using a version of Spring released after patch 18 of minor release 5.3 (i.e. after 5.3.18), or after patch 20 if using minor release 5.2 (i.e. after 5.2.20). Upgrading the version of the framework is enough to remove the vulnerability from your applications.

If upgrading is not possible, it is possible to somewhat mitigate this vulnerability by setting a blocklist of vulnerable field patterns. More advice on this can be found [here](https://spring.io/blog/2022/03/31/spring-framework-rce-early-announcement).

## Practical: Exploitation

**Downloading The Exploit**

We've covered the theory, now it's time to exploit the target!

As this is a network-based exploit, you will need to use the TryHackMe AttackBox or a local VM in order to run the exploit.

Copies of the exploit code can be obtained in three places:

* Attached to this task, accessible using the blue "Download Task Files" button.
    
* On port 8080 of the target machine (`http://MACHINE_IP:8080/exploit.zip`).
    
* Directly on the AttackBox (`/root/Rooms/Spring4Shell/exploit.py`). This will already have been extracted from the archive for your convenience.
    

Use whichever method is easiest to obtain a copy of the `exploit.zip` zipfile. The password for this archive is `TryHackMe123!`.

When you unzip the archive you should find a single Python script: `exploit.py`. This is a modified version of the proof of concept that is currently circulating online — it is slightly more user friendly than the original (obtained [here](https://github.com/BobTheShoplifter/Spring4Shell-POC)); however, you may use whichever version you wish. This task will assume that you are using the code provided.

**The Exploit**

Read through the exploit code and ensure that you are happy that it does what it claims to do. You should *always* review unknown exploits before running them.

Having satisfactorily completed our code review, let's start by taking a look at the help menu for the exploit:

Exploit Help Menu

```bash
user@attacker:~$ ./exploit.py --help
usage: exploit.py [-h] [-f FILENAME] [-p PASSWORD] [-d DIRECTORY] url

Spring4Shell RCE Proof of Concept

positional arguments:
  url                   Target URL

optional arguments:
  -h, --help            show this help message and exit
  -f FILENAME, --filename FILENAME
                        Name of the file to upload (Default tomcatwar.jsp)
  -p PASSWORD, --password PASSWORD
                        Password to protect the shell with (Default: thm)
  -d DIRECTORY, --directory DIRECTORY
                        The upload path for the file (Default: ROOT)
```

The script requires one argument: `url` — this specifies the target for the POST request. The other three optional arguments can be ignored in the current context.

Let's start by taking a look at the website in a browser (`http://MACHINE_IP/`)

We can easily find our target URL by checking the source code of the website homepage (`view-source:http://MACHINE_IP/`).

Specifically we are looking for the "action" of the contact form (the only POST request available to us). This is found on line 20:

`<form id="contactForm" action="/" method="post">`

The action is "`/`", meaning that our target URL will simply be: `http://MACHINE_IP/`.

***Note:*** *the trailing slash is very important here!*

**Attack**

Time to use the exploit!

Execute the script (if you're using the AttackBox navigate to the folder /root/Rooms/spring4shell). You should receive output similar to the following:

Running the Exploit

```bash
user@attacker:~$ ./exploit.py http://MACHINE_IP/
Shell Uploaded Successfully!
Your shell can be found at: http://MACHINE_IP/tomcatwar.jsp?pwd=thm&cmd=whoami
```

A webshell should now have been uploaded!

Navigate to the link provided by the exploit to see this in action. You can change which command gets run by changing the `cmd` parameter. By default the command is `whoami`.

***Note:*** *Once the exploit has been used successfully once, it cannot be used again until the application has been restarted. If you upload the webshell with the wrong name or directory path then you will need to terminate and re-deploy the target machine.*

### Answer the questions below

Follow the steps in the task to exploit Spring4Shell and obtain a webshell.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762506383713/5c33fdea-3906-4185-a075-482f49101d9e.png align="center")

**\[Bonus Question: Optional\]** Use your webshell to obtain a reverse/bind shell on the target.

What is the flag in `/root/flag.txt?`  

`http://10.10.153.244/tomcatwar.jsp?pwd=thm&cmd=cat /root/flag.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762506390058/e77554b7-0879-4355-bffe-079ac603493d.png align="center")

## Info: Conclusion

Congratulations, you have reached the end of the Spring4Shell room!

Having‌‌‌‌‌﻿‌‌ ‌‌‌‌‍‬‍‌completed‌‌‌‌‍‬‌‍ this‌‌‌‌‍﻿‬‍ room, you should hopefully have a high‌‌‌‌‌‬‌‌-level‌‌‌‌‍﻿‍﻿‌‌‌‌‍‬‌‍‌‌‌‌‍﻿‌﻿ understanding ‌‌‌‌‌‬‌‌of the vulnerability ‌‌‌‌‍‬‬‌and be comfortable ‌‌‌‌‍‬‍‍attacking ‌‌‌‌‍﻿‌‬applications vulnerable to CVE-2022-22965.

As mentioned previously, this vulnerability has the potential to be very widespread, but only time will tell how serious it truly is. Keep an eye out, and make sure that you patch any systems you are responsible for!

If you would like to read more about Spring4Shell, you may find the following resources informative:

* [Spring RCE Announcement](https://spring.io/blog/2022/03/31/spring-framework-rce-early-announcement)
    
* [LunaSec Blog Post](https://www.lunasec.io/docs/blog/spring-rce-vulnerabilities/)
    
* [Praetorian Blog Post](https://www.praetorian.com/blog/spring-core-jdk9-rce/)
    
* [o0o Writeup for CVE-2010-1622](http://blog.o0o.nu/2010/06/cve-2010-1622.html)
    

Spring4Shell demonstrates how powerful — and dangerous — automatic data binding can be when combined with an overly permissive runtime environment. While the conditions required for exploitation are narrower than many initial headlines suggested, the impact in a vulnerable deployment is severe: an attacker who can write a JSP or other executable artifact to the webroot can obtain remote command execution and fully compromise the application. The practical exercise in this room highlights two main takeaways: (1) always treat automatic binding features with caution and prefer explicit parameter handling, and (2) operate web applications with least privilege (the app process should not be able to write to its deployed webroot).

Mitigation is straightforward where feasible: upgrade Spring to a patched release (≥ 5.3.18 or ≥ 5.2.20 depending on your branch), keep Tomcat/JDK patched, lock down file permissions, and monitor for unexpected file writes in the webapp directories. For defenders, add logging and file-integrity checks, and consider WAF rules to block known exploit payloads. Finally, if you discover similar issues in production, follow a coordinated disclosure/patching process rather than attempting destructive testing — the fixes and guidance from the Spring project and several reputable security teams are the best next steps.