---
title: "The Advent of Cyber: Day 11: WI-FI attacks: If you'd like to WPA, press the star key! (TryHackMe)"
datePublished: Wed Dec 18 2024 02:22:16 GMT+0000 (Coordinated Universal Time)
cuid: cm4t9ow50000209jsclzv8wwy
slug: the-advent-of-cyber-day-11-wi-fi-attacks-if-youd-like-to-wpa-press-the-star-key-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734488375070/e8ff0d65-19eb-4c77-9007-add64ba6eb4d.png
tags: wlan, tryhackme, write-up, adventofcyber2024, wpa

---

In this article, we’ll cover the WI-FI Attacks—If you'd like to WPA, press the star key! writeup as the Day 11 challenge of the Advent of Cyber event challenge. It was interesting to understand some concepts related to wireless networking like the WLAN commands, and fundamental concepts like BSSID(Basic Service Set Identifier), SSID(Service Set Identifier), and PSK(Pre-Shared Key). We’re still at Wareville for SOC-mas!

### 1\. **BSSID (Basic Service Set Identifier)**

* **Definition**: The BSSID is the unique identifier of a specific wireless access point (AP) in a network. It is essentially the MAC address of the wireless AP's radio interface.
    
* **Purpose**: It identifies a specific AP within a wireless network. This is particularly useful in environments with multiple APs where devices need to associate with the correct one.
    
* **Format**: A BSSID is a 48-bit address written in hexadecimal format (e.g., `00:14:22:01:23:45`).
    

---

### 2\. **SSID (Service Set Identifier)**

* **Definition**: The SSID is the name of the wireless network. It is a human-readable string used to identify a Wi-Fi network.
    
* **Purpose**: It allows users to distinguish between different networks and connect to the desired one.
    
* **Format**: SSIDs can be up to 32 characters long and can include letters, numbers, and special characters (e.g., `HomeNetwork123`).
    
* **Broadcasting**: By default, most routers broadcast the SSID, allowing devices to discover the network. However, SSID broadcasting can be disabled for security, although this is not foolproof.
    

---

### 3\. **PSK (Pre-Shared Key)**

* **Definition**: The PSK is the password or passphrase used to secure a Wi-Fi network.
    
* **Purpose**: It provides authentication and encryption, ensuring that only authorized devices can connect to the network.
    
* **Use in Security Protocols**:
    
    * WPA (Wi-Fi Protected Access) and WPA2 typically use PSKs to secure personal networks.
        
    * A PSK is shared among all users of the network and is used to derive encryption keys.
        
* **Format**: PSKs can range from 8 to 63 characters and typically include a mix of alphanumeric characters for strength.  
      
    
    Back to the write-up, the best way to approach this was by reading through the explanation/introduction to the challenge and then coding along since there are commands that have been offered, and in each step, there’s a breakdown of the output. Also, read the questions in advance since the first output could have the answer you’re looking for. After starting the machine and opening the terminal, use SSH to access the VM from the AttackBox using `ssh glitch@MACHINE_IP` and entering the credentials provided.  
      
    

1. What is the BSSID of our wireless interface? `02:00:00:00:02:00`
    
    this comes with the very first commands provided in the walkthrough
    
    `glitch@wifi:~$ sudo ip link set dev wlan2 down glitch@wifi:~$ sudo iw dev wlan2 set type monitor glitch@wifi:~$ sudo ip link set dev wlan2 up glitch@wifi:~$ sudo iw dev wlan2 info`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734487205457/ba3c9b7b-eb2e-4929-888d-9ea0d620767c.png align="center")
    
2. What is the SSID and BSSID of the access point? Format: SSID, BSSID `MalwareM_AP, 02:00:00:00:00:00`
    
    when following the walkthrough and you run those provided commands you’ll see these commands: always go back and forth between the questions if it isn’t clear what you’re looking for, the explanation is helpful to understand the different access points and concepts like SSID & BSSID.
    
    `$ iw dev $ sudo iw dev wlan2 scan`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734488454600/748bfecf-00b4-4ce8-b213-194c5739b000.png align="center")
    
3. What is the BSSID of the wireless interface that is already connected to the access point? `02:00:00:00:01:00`
    
    there’s a part where you’re told to open two terminal tabs, the first command is what is needed for this section
    
    `$ sudo airodump-ng wlan2`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734487951476/57920687-539c-483a-b38c-c168bfa8eb88.png align="center")
    
4. What is the PSK after performing the WPA cracking attack? `fluffy/champ24`
    
    In this section, you’lll open the two terminal tabs, on the first one you’ll run `sudo airodump-ng wlan2` and the second one you’ll run `sudo aireplay-ng -0 1 -a 02:00:00:00:00:00 -c 02:00:00:00:01:00 wlan2`.
    
    According to the walkthrough, “In the second terminal, we can use the captured WPA handshake to attempt to crack the WPA/WP2 passphrase. We will be performing a dictionary attack in order to match the passphrase against each entry in a specified wordlist file. A shortened version of the infamous `rockyou.txt` wordlist has already been provided for us to use. This is located in the `/home/glitch/` directory. If the passphrase is weak and appears in the wordlist, it will eventually be cracked. The command `sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output*cap` will do this for us where the `-a 2` flag indicates the WPA/WPA2 attack mode. The `-b` indicates the BSSID of the access point, and the `-w` flag indicates the dictionary list to use for the attack. Finally, we select the output files that we will be using, which contain the 4-way handshake that we will be cracking.“
    
    the final command which help us find the key will be: `sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output*cap`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734488129314/848ce557-112e-4ada-a5c0-82ac31b4e6d9.png align="center")
    
5. If you enjoyed this task, feel free to check out the [Networking](https://tryhackme.com/module/networking) module.
    

Thank you for reading through this article. You can leave a comment with your thoughts: areas to improve or other suggestions and questions if any. Till the next one, stay secure!