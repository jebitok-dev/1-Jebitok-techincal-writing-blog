---
title: "The Advent of Cyber: Day 20: Traffic analysis - If you utter so much as one packet! (TryHackMe)"
datePublished: Sat Jan 11 2025 20:42:41 GMT+0000 (Coordinated Universal Time)
cuid: cm5snkhbs000109l66belenwb
slug: the-advent-of-cyber-day-20-traffic-analysis-if-you-utter-so-much-as-one-packet-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736627422754/a2553c53-2cfa-44c5-a033-e64c913342f8.png
tags: windows, tryhackme, write-up, wireshark, adventofcyber2024

---

In this article, we’ll cover Traffic analysis - If you utter so much as one packet! write-up as the Day 20 challenge of the Advent of Cyber event challenge. It involved using Wireshark to conduct traffic analysis on Windows. We’re still at Wareville for SOC-mas!  
  
**Diving Deeper**

Now that we have a better idea of what C2 traffic looks like and how to use Wireshark, double-click on the file “*C2\_Traffic\_Analysis*” on the Desktop. This will automatically open the PCAP file using Wireshark.

That's traffic! Yes, and this would take us to the truth about Mayor Malware.

We already suspect that this machine is compromised. So, let’s narrow down our list so that it will only show traffic coming from the IP address of Marta May Ware’s machine. To do this, click inside the **Display Filter Bar** on the top, type `ip.src == 10.10.229.217`, and press **Enter**.

![Display Filter Bar](https://tryhackme-images.s3.amazonaws.com/user-uploads/63588b5ef586912c7d03c4f0/room-content/63588b5ef586912c7d03c4f0-1729246743949.png align="left")

  

It’s still a lot, but at least we can now focus our analysis on outbound traffic.

If you scroll down a bit, you will find some interesting packets, specifically those highlighted with an arrow, as shown below.

![Highlighted packets](https://tryhackme-images.s3.amazonaws.com/user-uploads/63588b5ef586912c7d03c4f0/room-content/63588b5ef586912c7d03c4f0-1729246982740.png align="left")

  

Initial? Command? Exfiltrate? That is sure to be something!

Let’s dive deeper.

## **Message Received**

If you click on the `POST /initial` packet (Frame 440), more details will be shown on the bottom panes. These panes will show more detailed information about the packet frame. It shows relevant details such as frame number (440), the destination IP (10.10.123.224), and more.

You can expand each detail if you want, but the critical area to focus on is the lower-right view, the “Packet Bytes” pane.

![Packet Bytes pane](https://tryhackme-images.s3.amazonaws.com/user-uploads/63588b5ef586912c7d03c4f0/room-content/63588b5ef586912c7d03c4f0-1729248432169.png align="left")

  

This pane shows the bytes used in the communication in hexadecimal and ASCII character formats. The latter format shows readable text, which can be helpful in investigations.

The screenshot above shows something interesting: “I am in Mayor!”. This piece of text is likely relevant to us.

If we right-click on the `POST /initial` packet (Frame 440) and select `Follow` &gt; `HTTP Stream`, a new pop-up window will appear containing the back-and-forth HTTP communication relevant to the specific session. 

![The initial packet](https://tryhackme-images.s3.amazonaws.com/user-uploads/63588b5ef586912c7d03c4f0/room-content/63588b5ef586912c7d03c4f0-1730711649896.png align="left")

This feature is useful when you need to view all requests and responses between the client and the server, as it helps you understand the complete context of the communication.

The text highlighted in red is the message sent from the source to the destination, and blue is the opposite. So, based on the screenshot above, we can see that after the message “I am in Mayor!” was sent, a response that reads “Perfect!" was sent back.

*Perfect*, indeed, Mayor. We got you now!

But let’s not stop here. Other interesting HTTP packets were sent to the same destination IP. If you follow the HTTP Stream for the `GET /command` packet (Frame 457), you’ll see a request to the same IP destination. Interestingly, the reply that came back was a command commonly used in Windows and Linux systems to display the current user’s information. This communication suggests that the destination is attempting to gather information about the compromised system, a typical step during an early reconnaissance stage.

Usually, the reply from a C2 server contains the command, instructing the malicious program what to do next. However, the type of instruction depends on the malicious actor’s configuration, intention, and capabilities. These instructions often fall into several categories:

1. **Getting system information:** The attacker may want to know more about the compromised machine to tailor their next moves. This is what we are seeing above.
    
2. **Executing commands:** If the attacker needs to perform specific actions, they can also send commands directly. However, this is less stealthy and easily attracts attention.
    
3. **Downloading and executing payloads:** The attacker can also send additional payloads to the machine containing additional functionality or tools.
    
4. **Exfiltrating data:** This is one of the most common objectives. The program may be instructed to steal valuable data such as sensitive files, credentials, or personal information.
    

Exfiltrate sounds familiar, right?

## **Exfiltrating the Package**

![Picture of McSkidy](https://tryhackme-images.s3.amazonaws.com/user-uploads/63588b5ef586912c7d03c4f0/room-content/63588b5ef586912c7d03c4f0-1729268607259.png align="left")

If we follow the HTTP Stream for the `POST /exfiltrate` packet (Frame 476) sent to the same destination IP, we will see a file exfiltrated to the C2 server. We can also find some clues inside this file. 

If you check the rest of the PCAP, you’ll find that more interesting packets were captured. Let’s break these down and dive deeper into what we’ve uncovered.

## **What’s in the Beacon**

A typical C2 beacon returns regular status updates from the compromised machine to its C2 server. The beacons may be sent after regular or irregular intervals to the C2 as a heartbeat. Here’s how this exchange might look:

* **Secret agent (payload):** “I am still alive. Awaiting any instructions. Over.”
    
* **C2 server:** “Glad to hear that! Stand by for any further instructions. Over.”
    

In this scenario, Mayor Malware’s agent (payload) inside Marta May Ware’s computer has sent a message that is sent inside all the beacons. Since the content is highly confidential, the secret agent encrypts it inside all the beacons, leaving a clue for the Mayor’s C2 to decrypt it. In the current scenario, we can identify the beacons by the multiple requests sent to the C2 from the target machine after regular intervals of time.

The exfiltrated file's content hints at how these encrypted beacons can be decrypted. Using the encryption algorithm with the provided key, we now have a potential way to unlock the beacon’s message and uncover what Mayor Malware's agent is communicating to the C2 server.

But what exactly are we about to reveal?

Since the beacon is now encrypted and you have the key to decrypt it, the CyberChef tool would be our source of truth for solving this mystery. Because of its wide features, CyberChef is considered a "Swiss Army Knife". We can use this tool for encoding, decoding, encrypting, decrypting, hashing, and much more. However, considering this task's scope, we would only cover the decryption process using this tool.

This [link](https://gchq.github.io/CyberChef/) will open the CyberChef tool in your browser. *Note that you will have to open this link within your own browser, since the target VM has no internet connection.*

From the tool's dashboard, you would be utilizing the following panes for decrypting your beacon:

1. **Operations:** Search for AES Decrypt and drag it to the **Recipe** area, which is in the second pane.
    
2. **Recipe:** This is the area where you would select the mode of encryption, ECB, and enter the decryption key you have. Keep the other options as they are.
    
3. **Input:** Once the Recipe is done, it is time to enter our encrypted beacon into the **Input** area. Copy your encrypted string and paste it here.
    
4. **Output:** Once you have completed the above steps, you need to click the "Bake" button in the Recipe area. Your encrypted string will be decrypted using the AES ECB decryption with the key you provided, and the output will be displayed in the **Output** area.
    

![CyberChef](https://tryhackme-images.s3.amazonaws.com/user-uploads/6645aa8c024f7893371eb7ac/room-content/6645aa8c024f7893371eb7ac-1730447407462.png align="left")

If you want to learn more about CyberChef, check out our [CyberChef: The Basics](https://tryhackme.com/r/room/cyberchefbasics) room from the [Cyber Security 101](https://tryhackme.com/r/path/outline/cybersecurity101) path.

1. What was the first message the payload sent to Mayor Malware’s C2? `I am in Mayor!`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736627631056/5247971c-b2c8-4cb9-ac0e-91afb55a9f5c.png align="center")
    
2. What was the IP address of the C2 server? `10.10.123.224`  
    
3. What was the command sent by the C2 server to the target machine? `whoami`  
    
4. What was the filename of the critical file exfiltrated by the C2 server? `credentials.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736627835112/881f2ca7-0ea1-4d48-ae1b-712ef91baee6.png align="center")
    
5. What secret message was sent back to the C2 in an encrypted format through beacons? `THM_Secret_101`  
    
6. Learn more about WireShark in our [Wireshark: Traffic Analysis](https://tryhackme.com/r/room/wiresharktrafficanalysis) room.
    

Thank you for reading this article. Please leave a comment with your thoughts, areas for improvement, other suggestions, and questions. Stay secure until the next one!