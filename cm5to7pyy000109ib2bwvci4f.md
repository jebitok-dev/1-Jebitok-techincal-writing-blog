---
title: "The Advent of Cyber: Day 23: Hash cracking - You wanna know what happens to your hashes? (TryHackMe)"
datePublished: Sun Jan 12 2025 13:48:31 GMT+0000 (Coordinated Universal Time)
cuid: cm5to7pyy000109ib2bwvci4f
slug: the-advent-of-cyber-day-23-hash-cracking-you-wanna-know-what-happens-to-your-hashes-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736689331906/ac5bd903-64c1-40ef-b7fb-b6f7970bf339.png
tags: hashing, tryhackme, write-up, john-the-ripper, adventofcyber2024

---

In this article, we’ll cover Kubernetes DFIR - It's because I'm kubed, isn't it? write-up as the Day 22 challenge of the Advent of Cyber event challenge. It involved hash functions and hash values, saving hashed passwords, cracking hashes, and finding the password of a password-protected document using tools like [JohnRipper](https://www.openwall.com/john/). We’re still at Wareville for SOC-mas!

**Data Breach and Hash Values**

Mayor Malware had an online account in a now-defunct forum that was breached, and all its user data was leaked. After checking online, we were able to retrieve the Mayor’s password in hashed format. It is listed below.

| Username | Password Hash |
| --- | --- |
| `mayor@email.thm` | `d956a72c83a895cb767bb5be8dba791395021dcece002b689cf3b5bf5aaa20ac` |

We want to discover the original password. The first step in our approach is to figure out the type of the hash function. Then, we will try to hash different passwords from a password list until we find a match.

We have saved the above hash value in the `/home/user/AOC2024/hash1.txt` file for your convenience.

* First, we will go to the `AOC2024` directory and then display the content of `hash1.txt`.
    
* Copy the displayed hash. Selecting the text in the split view will copy it for you.
    
* Next, we start one tool that helps identify hashes by issuing the command `python` [`hash-id.py`](http://hash-id.py).
    
* Paste the copied hash. Right-clicking with your mouse will paste the copied text in split view.
    
* Finally, we quit the tool using `CTRL`+`C`.
    

The interaction is shown in the terminal output below:

VMTerminal

```bash
user@machine:~$ cd AOC2024/
user@machine:~/AOC2024$ cat hash1.txt 
d956a72c83a895cb767bb5be8dba791395021dcece002b689cf3b5bf5aaa20ac
user@machine:~/AOC2024$ python hash-id.py
   #########################################################################
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: d956a72c83a895cb767bb5be8dba791395021dcece002b689cf3b5bf5aaa20ac

Possible Hashs:
[+] SHA-256
[+] Haval-256

Least Possible Hashs:
[+] GOST R 34.11-94
[+] RipeMD-256
[+] SNEFRU-256
[+] SHA-256(HMAC)
[+] Haval-256(HMAC)
[+] RipeMD-256(HMAC)
[+] SNEFRU-256(HMAC)
[+] SHA-256(md5($pass))
[+] SHA-256(sha1($pass))
--------------------------------------------------
 HASH: ^C

  Bye!
```

Next, we will try passwords from `rockyou.txt`, a popular password wordlist from a real data breach. The command is as follows:

`john --format=raw-sha256 --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt`

* `john` starts John the Ripper; the jumbo edition is installed on the machine
    
* `--format=raw-sha256` specifies the hash format, which we have figured out earlier that it is most likely a SHA-256
    
* `--wordlist=/usr/share/wordlists/rockyou.txt` sets the wordlist that we will use
    
* `hash1.txt` is the text file containing the hash value we are trying to crack
    

In our first attempt, `john` calculated the SHA-256 hash value for every password in `rockyou.txt` and compared it with the hash value in `hash1.txt`. Unfortunately, no password was found, as shown in the terminal output below:

VMTerminal

```bash
user@machine:~/AOC2024$ john --format=raw-sha256 --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA256 [SHA256 256/256 AVX2 8x])
Warning: poor OpenMP scalability for this hash type, consider --fork=2
Will run 2 OpenMP threads
Note: Passwords longer than 18 [worst case UTF-8] to 55 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
0g 0:00:00:03 DONE (2024-11-03 09:49) 0g/s 4765Kp/s 4765Kc/s 4765KC/s (4510458faruk)..*7¡Vamos!
Session completed.
```

There is a high chance that Mayor Malware has made some transformation to his password. For example, he might have replaced `a` with `4` or added a couple of digits to his password. John can start from a long password list and attempt various common derivations from each of the passwords to increase its chances of success. This behaviour can be triggered through the use of **rules**. Various rules come bundled with John the Ripper’s configuration files; one is suited for lengthy wordlists, `--rules=wordlist`.

Adding the option `--rules=wordlist` to your `john` command line generates multiple passwords from each one. For instance, it appends and prepends single digits. It does various common substitutions; for example, `a` can be replaced with `@`, `i` can be replaced with `!`, and `s` can be replaced with `$`. Many more mutations and transformations are part of these rules. You can check all the underlying rules by checking the `[List.Rules:Wordlist]` section in `/etc/john/john.conf`, John’s configuration file. Unlike the first attempt, using John with this option should crack the hash for you:  
`john --format=raw-sha256 --rules=wordlist --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt`

We should note that `john` will not spend computing resources to crack an already-cracked password hash. Consequently, if you repeat a command that has successfully found a password earlier, you will get a message like “No password hashes left to crack (see FAQ)”. Let’s say that you executed the command listed above and you recovered the password; then, the next time you want to see that password, you would use `john` with the `--show` option, for example, `john --format=raw-sha256 --show hash1.txt`.

**Data Breach and Hash Values**

Glitch has discovered Mayor Malware’s password used on the breached online forum. Although there is a high chance that this password will be used to access other online accounts created by the Mayor, Glitch does not want to go that route as it would violate the local laws and regulations. Instead of attempting anything illegal, he focused on the data he discovered in the Mayor’s trash. There is one interesting-looking PDF file that happens to be password-protected. You can help Glitch break it.

The first thing you need to do is to convert the password-protected file into a format that `john` can attack. Luckily, John the Ripper jumbo edition comes with the necessary tools. The different tools follow the naming style “format2john”. The terminal below shows a few examples.

VMTerminal

```bash
user@machine:~/AOC2024$ ls /opt/john/*2john*
/opt/john/1password2john.py      /opt/john/ethereum2john.py          /opt/john/openssl2john.py
/opt/john/7z2john.pl             /opt/john/filezilla2john.py         /opt/john/padlock2john.py
/opt/john/DPAPImk2john.py        /opt/john/geli2john.py              /opt/john/pcap2john.py
/opt/john/adxcsouf2john.py       /opt/john/gpg2john                  /opt/john/pdf2john.pl
/opt/john/aem2john.py            /opt/john/hccap2john                /opt/john/pdf2john.py
/opt/john/aix2john.pl            /opt/john/hccapx2john.py            /opt/john/pem2john.py
/opt/john/aix2john.py            /opt/john/htdigest2john.py          /opt/john/pfx2john.py
[...]
```

You are interested in a password-protected PDF; therefore, [`pdf2john.pl`](http://pdf2john.pl) should do the job perfectly for you. In the terminal below, you can see how to create a hash challenge from a PDF file. This hash value can later be fed to `john` to crack it.

VMTerminal

```bash
user@machine:~/AOC2024$ pdf2john.pl private.pdf > pdf.hash
user@machine:~/AOC2024$ cat pdf.hash
private.pdf:$pdf$2*3*128*-1028*1*16*c1e77e30a0456552cb8a5327241559bd*32*3dc175eae491edc29b937e4fdbda766c00000000000000000000000000000000*32*6a1b5158d8d6dd9e8380f87b624da6cc936075fd41dc3c76acf2d90db62e4a27
```

The first step to consider would be trying a long wordlist such as `rockyou.txt`; moreover, you might even use a rule such as `--rules=wordlist` to test derived passwords. In this case, neither approach works; Mayor Malware has picked a password that does not exist in these public wordlists and is not derived from any word found there. Knowing Mayor Malware, we see what he holds dear, which can hint at what he would consider for his password. Therefore, you need to create your own wordlist with the following words:

* Fluffy
    
* FluffyCat
    
* Mayor
    
* Malware
    
* MayorMalware
    

And save it as `wordlist.txt`. We have saved the above words in the `/home/user/AOC2024/wordlist.txt` file for your convenience. Consequently, our command would be:

`john --rules=single --wordlist=wordlist.txt pdf.hash`

* `--rules=single` covers more modification rules and transformations on the wordlist
    
* `--wordlist=wordlist.txt` is the custom and personalized wordlist that we created
    
* `pdf.hash` is the hash generated from the password-protected document
    

Now, you have gained all the necessary knowledge to tackle the questions below and uncover what Mayor Malware has been hiding in his password-protected document.

1. Crack the hash value stored in `hash1.txt`. What was the password? `fluffycat12`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736689576165/a9a6fcad-32ca-47fd-bcaf-bcc425545090.png align="center")
    
2. What is the flag at the top of the `private.pdf` file? `THM{do_not_GET_CAUGHT}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736689597601/fa7c54a7-cf01-4e0d-8b51-d9222a2c382f.png align="center")
    
3. To learn more about cryptography, we recommend the [Cryp](https://tryhackme.com/module/cryptography-101)[tography mod](https://tryhackme.com/module/cryptography-101)ule. If you want to practice more hash cracking, pleas[e consider t](https://tryhackme.com/module/cryptography-101)he [John](https://tryhackme.com/r/room/johntheripperbasics) [the Ripper:](https://tryhackme.com/module/cryptography-101) [Th](https://tryhackme.com/r/room/johntheripperbasics)[e Basics room.](https://tryhackme.com/r/room/johntheripperbasics)
    

[Thank y](https://tryhackme.com/r/room/johntheripperbasics)ou for readin[g this artic](https://tryhackme.com/module/cryptography-101)le. Plea[se leave a c](https://tryhackme.com/module/cryptography-101)[omment with your thoughts, areas for impr](https://tryhackme.com/r/room/johntheripperbasics)ovement, other suggestions, [and questions. Stay secure until the next one!](https://tryhackme.com/r/room/johntheripperbasics)