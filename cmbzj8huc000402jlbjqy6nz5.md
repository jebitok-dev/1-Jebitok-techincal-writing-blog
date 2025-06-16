---
title: "Challenges: OhSINT (TryHackMe)"
datePublished: Mon Jun 16 2025 20:14:01 GMT+0000 (Coordinated Universal Time)
cuid: cmbzj8huc000402jlbjqy6nz5
slug: challenges-ohsint-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750104746828/d1f93b82-0b51-4000-8168-37509a7fc790.png
tags: osint, tryhackme, write-up

---

This article will cover the [OhSINT](https://tryhackme.com/room/ohsint) write-up on THM.

What information can you possibly get with just one image file?

**Note:** This challenge was updated on 2024-02-01. If you are following any older walkthroughs, expect a small change. Additionally, the file is also available on the AttackBox, under the `/Rooms/OhSINT` directory.

## Answer the questions below

1. What is this user's avatar of? `cat`  
      
    using `exiftool <filename>` we get details about the image, most importantly the copyright owner, since this is an OSINT challenge, we have to do a web search of the copyright name and some of their profiles that comes up is X or Twitter on checking the profile, it has an avatar of a cat which is the answer.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750102829919/46e7055a-da5a-4241-8f58-5f4b98a98db9.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750102966644/97ece929-994b-4b71-a20b-772c38ea5a88.png align="center")
    
2. What city is this person in? `London`  
      
    Using OSINT search skills, the other profile that comes up is their GitHub profile, and on their readme, it shows that they’re based in London. Also, using the coordinates `54°17’41.27”N 2°15’1.33”N` shown by exiftool, it shows a different town, which is not the accurate answer when searched on a site like [Microsoft Bing Maps](https://www.bing.com/maps) or [Google Maps](https://www.google.com/maps), but useful nonetheless, as it might be useful in another OSINT challenge
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750104386813/2928a0a5-863d-4bae-b4d5-2ea17737e3ea.png align="center")
    
      
    
3. What is the SSID of the WAP he connected to? `UnileverWiFi`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750104474086/8bf27dea-f130-4aaa-9d8f-7229393cf355.png align="center")
    
      
      
    Use [Wigle.net](https://wigle.net/) to search the BSSID show above  
    
4. What is his personal email address? `OWoodflint@gmail.com`
    
5. What site did you find his email address on? `Github`
    
6. Where has he gone on holiday? `New York`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750104457024/bc594cdd-117c-4477-be31-c430835daf8f.png align="center")
    
7. What is the person's password? `pennYDr0pper.!`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1750102648711/ab221749-f372-4149-a5a7-bb53eab65f94.png align="center")
    
    lll
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges.