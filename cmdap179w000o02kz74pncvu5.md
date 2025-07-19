---
title: "Challenges: Sakura Room - OSINT (TryHackMe)"
datePublished: Sat Jul 19 2025 20:21:28 GMT+0000 (Coordinated Universal Time)
cuid: cmdap179w000o02kz74pncvu5
slug: challenges-sakura-room-osint-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752956332920/40b93459-76e4-4902-b11b-5a26d11bf575.png
tags: wifi, metadata, osint, digitalforensic, etherscan

---

The **Sakura Room**, created by the OSINT Dojo, offers more than just a challenge — it provides a narrative-driven training ground where participants trace the steps of a fictional cybercriminal using nothing but publicly available information.

This room bridges the gap between digital forensics, investigation, and real-world cyber behavior. From a single image file, we follow a trail that leads through usernames, social profiles, deleted commits, crypto wallets, and even geographic breadcrumbs. Every clue unravels a deeper layer of the attacker's digital identity, reinforcing a key OSINT principle: **people leave traces — it’s just a matter of knowing where to look.**

For developers, this room reveals something deeper: how simple missteps in personal and application security (like username reuse or leaked metadata) can be exploited by skilled investigators. It’s a powerful reminder of how important operational security is — not just for attackers, but for anyone building or maintaining a digital presence.

## INTRODUCTION

![](https://raw.githubusercontent.com/OsintDojo/public/main/SakuraLogo.png align="left")

**Welcome to the OSINT Dojo’s Sakura Room!**

**Background**

---

This room is designed to test a wide variety of different OSINT techniques. With a bit of research, most beginner OSINT practitioners should be able to complete these challenges. This room will take you through a sample OSINT investigation in which you will be asked to identify a number of identifiers and other pieces of information in order to help catch a cybercriminal. Each section will include some pretext to help guide you in the right direction, as well as one or more questions that need to be answered in order to continue on with the investigation. Although all of the flags are staged, this room was created using working knowledge from having led and assisted in OSINT investigations both in the public and private sector. 

NOTE: All answers can be obtained via passive OSINT techniques, DO NOT attempt any active techniques such as reaching out to account owners, password resets, etc to solve these challenges.

If you have any other questions, comments, or suggestions, please reach out to us at @OSINTDojo on Twitter.

  

**Instructions**

---

Ready to get started? Type in "Let's Go!" in the answer box below to continue.

### Answer the questions below

1. Are you ready to begin?
    

## TIP-OFF

![](https://raw.githubusercontent.com/OsintDojo/public/main/SakuraLogo.png align="left")

Background

The OSINT Dojo recently found themselves the victim of a cyber attack. It seems that there is no major damage, and there does not appear to be any other significant indicators of compromise on any of our systems. However during forensic analysis our admins found an image left behind by the cybercriminals. Perhaps it contains some clues that could allow us to determine who the attackers were?

We've copied the image left by the attacker, you can view it in your browser [here](https://raw.githubusercontent.com/OsintDojo/public/3f178408909bc1aae7ea2f51126984a8813b0901/sakurapwnedletter.svg).

**Instructions**

---

Images can contain a treasure trove of information, both on the surface as well as embedded within the file itself. You might find information such as when a photo was created, what software was used, author and copyright information, as well as other metadata significant to an investigation. In order to answer the following question, you will need to thoroughly analyze the image found by the OSINT Dojo administrators in order to obtain basic information on the attacker.

### Answer the questions below

1. What username does the attacker go by?  
      
    Download image then run the following command:  
      
    `string image_name | less`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752954127569/0969c7fa-fc3c-4a27-83e5-717de59c9d92.png align="center")
    

## RECONNAISSANCE

![](https://raw.githubusercontent.com/OsintDojo/public/main/SakuraLogo.png align="left")

**Background**

---

It appears that our attacker made a fatal mistake in their operational security. They seem to have reused their username across other social media platforms as well. This should make it far easier for us to gather additional information on them by locating their other social media accounts. 

**Instructions**

---

Most digital platforms have some sort of username field. Many people become attached to their usernames, and may therefore use it across a number of platforms, making it easy to find other accounts owned by the same person when the username is unique enough. This can be especially helpful on platforms such as on job hunting sites where a user is more likely to provide real information about themselves, such as their full name or location information.

A quick search on a reputable search engine can help find matching usernames on other platforms, and there are also a large number of specialty tools that exist for that very same purpose. Keep in mind, that sometimes a platform will not show up in either the search engine results or in the specialized username searches due to false negatives. In some cases you need to manually check the site yourself to be 100% positive if the account exists or not. In order to answer the following questions, use the attacker's username found in Task 2 to expand the OSINT investigation onto other platforms in order to gather additional identifying information on the attacker. Be wary of any false positives! 

### Answer the questions below

1. What is the full email address used by the attacker?  
      
    There’s a [GitHub repository](https://github.com/sakurasnowangelaiko/PGP/blob/main/publickey) [that he forked,](https://github.com/sakurasnowangelaiko/PGP/blob/main/publickey) and has a public key, when decrypted it revealed the email  
      
    copy the Public Key to public\_key.asc file or download it
    
      
    `nano public_key.asc`  
      
    `gpg —show-keys public_key.asc`  
      
    this will reveal the email
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752954404505/7e605fcd-d06a-40fb-b063-7c572f647a2e.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752954284612/04b99dbe-c5e7-47ee-97a6-f9732340efa6.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752954201924/7a85d43c-15c5-4209-ae55-16ddfc7c7794.png align="center")
    
2. What is the attacker's full real name?  
      
    Sakura’s X account tags the another of his accounts that reveals his other name
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752954587905/64a7d93f-d24a-4b55-a63c-4afb50b48bd0.png align="center")
    

## UNVEIL

![](https://raw.githubusercontent.com/OsintDojo/public/main/SakuraLogo.png align="left")

﻿﻿**Background**

---

It seems the cybercriminal is aware that we are on to them. As we were investigating into their Github account we observed indicators that the account owner had already begun editing and deleting information in order to throw us off their trail. It is likely that they were removing this information because it contained some sort of data that would add to our investigation. Perhaps there is a way to retrieve the original information that they provided?

**Instructions**

---

On some platforms, the edited or removed content may be unrecoverable unless the page was cached or archived on another platform. However, other platforms may possess built-in functionality to view the history of edits, deletions, or insertions. When available this audit history allows investigators to locate information that was once included, possibly by mistake or oversight, and then removed by the user. Such content is often quite valuable in the course of an investigation. In order to answer the below questions, you will need to perform a deeper dive into the attacker's Github account for any additional information that may have been altered or removed. You will then utilize this information to trace some of the attacker's cryptocurrency transactions.

### Answer the questions below

1. What cryptocurrency does the attacker own a cryptocurrency wallet for?  
      
    Ethereum is revealed on his [GitHub](https://github.com/sakurasnowangelaiko/ETH/blob/main/miningscript)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752954773673/6044aff1-1b56-4e7b-b817-dd153bad426a.png align="center")
    
2. What is the attacker's cryptocurrency wallet address?  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752954924157/9180446c-b56b-4612-a7a0-0ef2f31483dc.png align="center")
    
      
    
3. What mining pool did the attacker receive payments from on January 23, 2021 UTC?  
      
    searching address on [Etherscan](https://etherscan.io/address/0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef) will help us answer the next questions
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752955010699/ba2b22e4-d0bc-489e-a55a-089575c2ec81.png align="center")
    
      
    
4. What other cryptocurrency did the attacker exchange with using their cryptocurrency wallet?  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752955046538/ab7116a5-eee0-4d7a-8978-3b934a1fa3df.png align="center")
    

## TAUNT

![](https://raw.githubusercontent.com/OsintDojo/public/main/SakuraLogo.png align="left")

**Background**

---

Just as we thought, the cybercriminal is fully aware that we are gathering information about them after their attack. They were even so brazen as to message the OSINT Dojo on Twitter and taunt us for our efforts. The Twitter account which they used appears to use a different username than what we were previously tracking, maybe there is some additional information we can locate to get an idea of where they are heading to next?

We've taken a screenshot of the message sent to us by the attacker, you can view it in your browser [here](https://raw.githubusercontent.com/OsintDojo/public/main/taunt.png).

  

**Instructions**

---

Although many users share their username across different platforms, it isn't uncommon for users to also have alternative accounts that they keep entirely separate, such as for investigations, trolling, or just as a way to separate their personal and public lives. These alternative accounts might contain information not seen in their other accounts, and should also be investigated thoroughly. In order to answer the following questions, you will need to view the screenshot of the message sent by the attacker to the OSINT Dojo on Twitter and use it to locate additional information on the attacker's Twitter account. You will then need to follow the leads from the Twitter account to the Dark Web and other platforms in order to discover additional information.

### Answer the questions below

1. What is the attacker's current Twitter handle?
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752955395738/93447c95-67ab-4e63-a91d-fd65326f771a.png align="center")
    
2. What is the BSSID for the attacker's Home WiFi?  
      
    Resources used for this challenge:
    
    * [Deep Search on Onion Live](https://onion.live/?search=deep%20search)
        
    * [DeepWebLinks - Pastebin](https://deepweblinks.net/pastebin/)
        
    * [Wigle.net - have to create an account](https://wigle.net/search#fullSearch)
        
    * [This medium article of the Sakura Room Walkthrough](https://medium.com/@leventd/tryhackme-sakura-room-ctf-write-up-47994b800e64)
        
    
      
    this was challenging to me but this details will be helpful
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752955748834/9c34186c-b1c6-41dc-9790-a65476005617.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752956052394/12939639-851b-47ff-a029-0e781c4bfb55.png align="center")

##   
[HOMEBOUND](https://deepweblinks.net/pastebin/)

![](https://raw.githubusercontent.com/OsintDojo/public/main/SakuraLogo.png align="left")

[**Background**](https://deepweblinks.net/pastebin/)

---

[B](https://deepweblinks.net/pastebin/)[as](https://medium.com/@leventd/tryhackme-sakura-room-ctf-write-up-47994b800e64)[ed on their tweets, it appears our](https://wigle.net/search#fullSearch) [cybercriminal is indeed heading home as they claimed. Their Twitter account seems to have plenty of photos which sho](https://medium.com/@leventd/tryhackme-sakura-room-ctf-write-up-47994b800e64)uld allow us to piece together their route back home. If we follow the trail of breadcrumbs they left behind, we should be able to track their movements from one location to the next back all the way to their final destination. Once we can identify their final stops, we can identify which law enforcement organization we should forward our findings to.

**Instructions**

---

In OSINT, there is oftentimes no "smoking gun" that points to a clear and definitive answer. Instead, an OSINT analyst must learn to synthesize multiple pieces of intelligence in order to make a conclusion of what is likely, unlikely, or possible. By leveraging all available data, an analyst can make more informed decisions and perhaps even minimize the size of data gaps. In order to answer the following questions, use the information collected from the attacker's Twitter account, as well as information obtained from previous parts of the investigation to track the attacker back to the place they call home.

### Answer the questions below

1. What airport is closest to the location the attacker shared a photo from prior to getting on their flight? `DCA`  
    
2. What airport did the attacker have their last layover in? `HND`
    
3. What lake can be seen in the map shared by the attacker as they were on their final flight home? `Lake Inawashiro`
    
4. What city does the attacker likely consider "home"? `Hirosaki`
    

The Sakura Room is a brilliant showcase of how much can be discovered without touching a target system. Through passive OSINT methods alone, we moved from a static image left by an attacker to uncovering their identity, tracking crypto transactions, and pinpointing their physical location. No brute force. No phishing. Just breadcrumbs and careful analysis.

For those transitioning from development to cybersecurity, this room underscores a critical truth: **security isn’t just code — it’s context.** Metadata, naming habits, image uploads, and forgotten repos all tell stories. And in the hands of an analyst, those stories become leads.

Whether you're an aspiring threat hunter or a curious developer, rooms like Sakura are more than exercises — they’re reminders that every bit of data matters, and every detail is a piece of the puzzle.