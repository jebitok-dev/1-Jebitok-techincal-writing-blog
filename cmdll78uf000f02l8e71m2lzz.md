---
title: "Challenges: Stolen Mount (TryHackMe)"
datePublished: Sun Jul 27 2025 11:19:40 GMT+0000 (Coordinated Universal Time)
cuid: cmdll78uf000f02l8e71m2lzz
slug: challenges-stolen-mount-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753615137755/5da8fdfe-99ba-44b2-915f-fd3224d02c87.png
tags: backup, tryhackme, pcap, wireshark, ctf-writeup

---

In this challenge, we investigate a data breach involving a compromised NFS server. An intruder infiltrated the network and accessed backup files containing classified data. The only artifact available for analysis is a network packet capture (`challenge.pcapng`) recorded during the incident. Using Wireshark and basic forensic techniques, our objective is to trace the attacker’s actions, recover the stolen data, and extract the hidden flag.

## Forensics **Stolen Mount**

### **Set up your virtual environment**

To successfully complete this room, you'll need to set up your virtual environment. This involves starting the Target Machine, ensuring you're equipped with the necessary tools and access to tackle the challenges ahead.

![](https://tryhackme.com/static/media/target-machine.f0eafcef90514da27a537e33578f7c86.svg align="left")

![Character Image](https://tryhackme-images.s3.amazonaws.com/user-uploads/62ff64c3c859dc0042b2b9f6/room-content/62ff64c3c859dc0042b2b9f6-1742203659614.png align="left")

An intruder has infiltrated our network and targeted the NFS server where the backup files are stored. A classified secret was accessed and stolen. The only trace left behind is a packet capture (PCAP) file recorded during the incident. Your mission, should you accept it, is to discover the contents of the stolen data.

Note: Click the **Start Machine** button to spawn the Virtual Machine.

The packet capture (**challenge.pcapng**) is stored in the **~/Desktop** directory.

### Answer the questions below

What is the flag?

Start the machine the open the pcap file using Wireshark. The description had mentioned that an intruder infiltrated our network and targeted the NFS server where the backup files are stored.

So we’ll highlight one NFS and click on Analyze inorder to follow the TCP stream:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753614636438/e100e268-a303-47aa-9752-04f2a64c58de.png align="center")

  
this reveals a MD5 hash which is the password of a secrets. file and also

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753614484618/ca8a2fa2-2085-4723-a3ac-b2545dd7de2d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753614510021/76ce36b7-33e8-4eb2-889b-3e83c5062b34.png align="center")

There’s a secrets.png and creds.txt files revealed to be on the backup. Also it’s revealed that they’re stored on a zip file, hidden\_stash.zip and we need to access them locally. We need to find a way to export them or save locally.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753614620733/0e024721-a231-4ef4-a32b-cdc1eec02f14.png align="center")

Change the show data as field from ASCII to Raw then click on save as and save it as a .zip file locally.  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753614711984/e2e6e113-7fbc-4896-a169-939adfb3e529.png align="center")

On you terminal unzip the zip file in the Desktop folder and it will ask for the password which will be the one we got from the MD5 hash.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753614943624/147503bd-b90e-4371-aaa9-6ec815522b36.png align="center")

  
checking the secrets.png file, it’s a QR Code that needs to be scanned which reveals the flag.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753614996184/ba153eaf-d5a0-4675-b13d-76bd8b30a292.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753615009418/3a36b501-5228-420d-a2bd-5e7d2b74379f.jpeg align="center")

  
  
Through careful analysis of the PCAP file, we identified unauthorized access to the NFS server and recovered a hidden ZIP archive containing sensitive files. After extracting the archive using credentials found in the captured traffic, we uncovered a QR code embedded in `secrets.png`. Scanning the QR code revealed the final flag, confirming the intruder’s stolen data. This challenge highlights the importance of network traffic analysis and proper monitoring of file access in preventing data exfiltration.