---
title: "Challenges: CTF collection Vol.1 (TryHackMe)"
datePublished: Fri Jul 11 2025 14:31:44 GMT+0000 (Coordinated Universal Time)
cuid: cmcyx0mj7000o02le304r4brj
slug: challenges-ctf-collection-vol1-tryhackme
tags: steganography, osint, tryhackme, ctf-writeup, binwalk, decoder

---

Welcome to the walkthrough of **"TryHackMe Vol.1"**, a fun and beginner-friendly Capture The Flag (CTF) room created to test and improve your fundamental cybersecurity and problem-solving skills. This room consists of **20 short, easy challenges**, each designed to expose you to various techniques like steganography, OSINT, encoding/decoding, metadata analysis, and more.

Whether you're just starting your CTF journey or brushing up on your skills, this room offers a great hands-on opportunity. All flags follow the format: `THM{flag}` unless stated otherwise.

## Author note

![](https://i.ibb.co/m4rGdyG/holida.png align="center")

Just another random CTF room created by me. Well, the main objective of the room is to test your CTF skills. For your information, vol.1 consists of 20 tasks and all the challenges are extremely easy. Stay calm and Capture the flag. :)

Note: All the challenges flag are formatted as `THM{flag}`, unless stated otherwise

Answer the questions below

High five!

## What does the base said?

Can you decode the following?

VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ==

### Answer the questions below

1. Feed me the flag!  
      
    [base64decode](https://www.base64decode.org/)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752244080005/982e050e-272f-4cbe-bc61-bfdcbb16a647.png align="center")
    

## Meta meta

  
Meta! meta! meta! meta...................................

### Answer the questions below

1. I'm hungry, I need the flag.  
      
    `exiftool Find_me_1577975566801.jpg`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752243975525/45dc54ab-ce15-44e3-8caa-41f7d4e325da.png align="center")
    

## Mon, are we going to be okay?

  
Something is hiding. That's all you need to know.

### Answer the questions below

1. It is sad. Feed me the flag.
    
    ```bash
    docker run -it --rm -v ~/Downloads:/files ubuntu bash
    # Inside Docker:
    apt update
    apt install -y steghide
    cd /files
    steghide extract -sf yourfile.jpg
    ```
    
    It’s problematic to use Steghide on mac so I had to use docker to access ubuntu  
      
    `steghide extract -sf image_name`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752243481636/efbd9d54-35fd-4425-a0de-a914a30953f6.png align="center")
    

## Erm......Magick

Huh, where is the flag? THM{wh173\_fl46}

### Answer the questions below

1. Did you find the flag?
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752243392114/928caa2a-d76c-4e96-893c-51335d8e6951.png align="center")
    

## QRrrrr

Such technology is quite reliable.

### Answer the questions below

1. More flag please!  
      
    scan the QR Code
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752243314964/fc26fd13-470f-4cb9-8380-3164c1388ef5.jpeg align="center")

## Reverse it or read it?

  
Both works, it's all up to you.

### Answer the questions below

1. Found the flag?  
      
    `cat file_name`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752243208348/a1c17d70-5886-4193-a6c3-221ba67a77f6.png align="center")
    

## Another decoding stuff

  
Can you decode it?

3agrSy1CewF9v8ukcSkPSYm3oKUoByUpKG4L

### Answer the questions below

1. Oh, Oh, Did you get it?  
      
    using [CyberChef](https://gchq.github.io/CyberChef): Base58  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752242935974/19e63a2e-0cc5-4d53-be5a-68fc3bc01aef.png align="center")
    

## Left or right

  
Left, right, left, right... Rot 13 is too mainstream. Solve this

MAF{atbe\_max\_vtxltk}

### Answer the questions below

1. What did you get?  
      
    using [CyberChef](https://gchq.github.io/CyberChef): ROT13 Brute Force  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752243001435/493a6415-7dc3-42d3-b9ab-507703cf0df3.png align="center")
    

## Make a comment

No downloadable file, no ciphered or encoded text. Huh .......

### Answer the questions below

1. I'm hungry now... I need the flag  
      
    inspect the element of the page and check within the div of this section  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752242614693/08ac3a7a-81f1-43ac-b596-81d858c4548e.png align="center")
    

## Can you fix it?

I accidentally messed up with this PNG file. Can you help me fix it? Thanks, ^^

### Answer the questions below

1. What is the content?  
      
    `xxd spoil_1577979329740.png | head`
    
    ### 🛠️ Steps to Fix:
    
    1. **Install** `hexedit` (if not already):
        
        ```bash
        bash
        CopyEdit
        sudo apt install hexedit
        
        ```
        
    2. **Open the broken file**:
        
        ```bash
        bash
        CopyEdit
        cp spoil_1577979329740.png fixed.png
        hexedit fixed.png
        
        ```
        
    3. You'll see something like:
        
        ```bash
        r
        CopyEdit
        00000000   23 33 44 5F 0D 0A 1A 0A   ... (ASCII: #3D_....)
        
        ```
        
    4. **Navigate to the start and overwrite the first 8 bytes** with:
        
        ```bash
        mathematica
        CopyEdit
        89 50 4E 47 0D 0A 1A 0A
        
        ```
        
        (In hexedit, just move with arrow keys and type the new hex values — it will overwrite.)
        
    5. Press `Ctrl+X` → `Y` → `Enter` to save and exit.
        
    6. open the fixed.png file locally
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752242454178/7d5c92bf-f52a-49bd-b3e8-b37ecf9d87d1.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752242469904/f4262517-d111-4aee-8570-4ae08720eb7d.png align="center")

##   
Read it

Some hidden flag inside Tryhackme social account.

### Answer the questions below

1. Did you found the hidden flag?  
      
    on Reddit under r/tryhackme, search [New room](https://www.reddit.com/r/tryhackme/comments/eizxaq/new_room_coming_soon/) coming soon on reddit
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752242272848/6811ba4d-c137-4e0a-8335-5583c74b98db.png align="center")
    
      
      
    

## Spin my head

What is this?

++++++++++\[&gt;+&gt;+++&gt;+++++++&gt;++++++++++&lt;&lt;&lt;&lt;-\]&gt;&gt;&gt;++++++++++++++.------------.+++++.&gt;+++++++++++++++++++++++.&lt;&lt;++++++++++++++++++.&gt;&gt;-------------------.---------.++++++++++++++.++++++++++++.&lt;++++++++++++++++++.+++++++++.&lt;+++.+.&gt;----.&gt;++++.

### Answer the questions below

1. Can you decode it?  
      
    decode [brainfuck language](https://www.dcode.fr/brainfuck-language)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752242976113/840ec235-49d2-41d5-a89b-a6bc15356710.png align="center")
    
    [  
    ](https://www.dcode.fr/brainfuck-language)
    

## [An exclus](https://www.dcode.fr/brainfuck-language)ive!

Exclusive strings for everyone!

S1: 44585d6b2368737c65252166234f20626d  
S2: 1010101010101010101010101010101010

### Answer the questions below

1. Did you crack it? Feed me now!  
      
    using [XOR calculator](https://xor.pw/#) to get the output of S1 XOR S2 then used [SimplyCalc](https://simplycalc.com/base16-decode.php) to decode base16
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752241828154/c2ff4701-221c-40a9-a539-9a467f28697d.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752241853615/6630e508-4c58-4c2d-a699-e3312481fd87.png align="center")
    

## Binary walk

Please exfiltrate my file :)

### Answer the questions below

1. Flag! Flag! Flag!  
      
    using binwalk:  
      
    `binwalk -e --run-as=root hell_1578018688127.jpg`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752241501215/2bf897b5-4b9d-4e73-8087-93d39a21ff2a.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752241699132/be556662-0968-432e-a648-2cad0a08993e.png align="center")

## Darkness

There is something lurking in the dark.

### Answer the questions below

1. What does the flag said?  
      
    Using StegSolve, I had to first install java and stegsolve.jar and ended up using the command `java -jar Stegsolve.jar` that launched a Stegsolve where I uploaded the image and after few scroll the flag got visible
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752241265236/d87f51ee-95a7-453c-8e7d-fbe173dac87f.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752241378441/23f2d9ac-a2d9-42de-beda-49ea0988cbfc.png align="center")

##   
A sounding QR

  
How good is your listening skill?

P/S: The flag formatted as THM{Listened Flag}, the flag should be in All CAPS

### Answer the questions below

1. What does the bot said? `THM{SOUNDINGQR}`  
      
    once I scanned the QR CODE, a url for soundcloud came up, listened to the audio and found the flag
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752241162634/10ea0465-228a-45d8-af22-71c8c335c8d6.jpeg align="center")
    

## Dig up the past

Sometimes we need a 'machine' to dig the past

Targetted website: [https://www.embeddedhacker.com/  
Targetted](https://www.embeddedhacker.com/￼Targetted) time: 2 January 2020

### Answer the questions below

1. Did you found my past?  
      
    Visited [Wayback Machine](https://web.archive.org/web/20200102131252/https://www.embeddedhacker.com/) and searched the targetted website then selected the given date and found the flag on the page
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752240945522/40716388-223d-43b6-b371-5ab9788bec00.png align="center")
    

## Uncrackable!

Can you solve the following? By the way, I lost the key. Sorry &gt;.&lt;

MYKAHODTQ{RVG\_YVGGK\_FAL\_WXF}

Flag format: TRYHACKME{FLAG IN ALL CAP}

### Answer the questions below

1. The deciphered text  
      
    using [Decoder](https://www.dcode.fr/vigenere-cipher) to decode the Vigenere Cipher and found the flag
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752240834740/db9b80bc-968e-427f-a978-405655a532a3.png align="center")
    

## Small bases

Decode the following text.

581695969015253365094191591547859387620042736036246486373595515576333693

### Answer the questions below

1. What is the flag?  
      
    Using [Rapid Tables](https://www.rapidtables.com/) to convert from Decimal to Hex to ASCII
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752240701321/9208dc4b-fd36-4092-b426-96751fcc5387.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752240721259/82dc6c43-f95d-4418-9e7b-91d7c2c4e4d4.png align="center")

## Read the packet

I just hacked my neighbor's WiFi and try to capture some packet. He must be up to no good. Help me find it.

### Answer the questions below

1. Did you captured my neighbor's flag?
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752240641275/3e5d8b25-c84c-4593-875b-c930ffc25732.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752240677662/759ddfce-0aae-42d8-910e-3bc596b61026.png align="center")

**Final Thoughts**:  
*"Vol.1"* is an engaging and diverse CTF room that provides a gentle introduction to a wide range of security challenges. From **image forensics** and **base encodings** to **QR code analysis**, **metadata inspection**, **steganography**, and even **retro ciphers** like **Brainfuck** and **Vigenère**, the room packs a lot into quick, satisfying puzzles.

Some challenges required creative tooling — like using Docker to install `steghide` or solving broken image headers with hex editors — while others leaned on intuitive platforms like **CyberChef**, **Wayback Machine**, or simply your browser’s **Inspect Element**. It’s a testament to the idea that CTFs aren’t just about code; they’re about curiosity, patience, and technique.

This walkthrough provides insight into the **thought process** behind solving each challenge — not just the answers. Keep exploring and remember: **Every flag you capture adds to your arsenal** for future rooms.

Happy Hacking 🕵️‍♂️🔐!