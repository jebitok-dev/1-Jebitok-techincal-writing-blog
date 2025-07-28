---
title: "Challenges: Corridor (TryHackMe)"
datePublished: Mon Jul 28 2025 14:30:15 GMT+0000 (Coordinated Universal Time)
cuid: cmdn7g72b000u02l4cm9q7rh8
slug: challenges-corridor-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753712961645/7c006477-8e1f-487b-9638-9c0ab3e0233d.png
tags: md5, tryhackme, idor, ctf-writeup

---

This challenge presented a web application that at first glance appeared to be static, with a single background image and minimal interactive content. Enumeration using tools like Gobuster didn’t reveal much, but manually inspecting the page source uncovered an interesting HTML `<img>` with an associated `<map>` defining clickable areas, each linked to what looked like MD5 hashes. These hashes represented potential IDs, hinting that access to resources was linked to predictable or manipulable identifiers.

Hello Hacker!

TopTierConversions LTD is proud to announce its latest and greatest product launch: MD2PDF.

This easy-to-use utility converts markdown files to PDF and is totally secure! Right...?

*Note: Please allow 3-5 minutes for the VM to boot up fully before attempting the challenge.*

### Answer the questions below

What is the flag?  

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753709919638/f4e1acef-6736-4391-8e8a-e7302a4472d8.png align="center")

`gobuster dir -u IP_Address -w /usr/share/wordlists/dirb/common.txt`  
  
Enumerating with `Gobuster` was not of much help on this case:  
  
Went ahead to check the site it had a background image and inspected the page to check the source code. It showed the `img` and an `image-map` which was interesting: it had unique identical `md5` hash for alt text and title for each area within the map

```makefile
<img src="/static/img/corridor.png" usemap="#image-map">
<map name="image-map">
    <area target="" alt="c4ca4238a0b923820dcc509a6f75849b" title="c4ca4238a0b923820dcc509a6f75849b" href="c4ca4238a0b923820dcc509a6f75849b" coords="257,893,258,332,325,351,325,860" shape="poly">
    <area target="" alt="c81e728d9d4c2f636f067f89cc14862c" title="c81e728d9d4c2f636f067f89cc14862c" href="c81e728d9d4c2f636f067f89cc14862c" coords="469,766,503,747,501,405,474,394" shape="poly">
    <area target="" alt="eccbc87e4b5ce2fe28308fd9f2a7baf3" title="eccbc87e4b5ce2fe28308fd9f2a7baf3" href="eccbc87e4b5ce2fe28308fd9f2a7baf3" coords="585,698,598,691,593,429,584,421" shape="poly">
    <area target="" alt="a87ff679a2f3e71d9181a67b7542122c" title="a87ff679a2f3e71d9181a67b7542122c" href="a87ff679a2f3e71d9181a67b7542122c" coords="650,658,644,437,658,652,655,437" shape="poly">
    <area target="" alt="e4da3b7fbbce2345d7772b0674a318d5" title="e4da3b7fbbce2345d7772b0674a318d5" href="e4da3b7fbbce2345d7772b0674a318d5" coords="692,637,690,455,695,628,695,467" shape="poly">
    <area target="" alt="1679091c5a880faf6fb5e6087eb1b2dc" title="1679091c5a880faf6fb5e6087eb1b2dc" href="1679091c5a880faf6fb5e6087eb1b2dc" coords="719,620,719,458,728,471,728,609" shape="poly">
    <area target="" alt="8f14e45fceea167a5a36dedd4bea2543" title="8f14e45fceea167a5a36dedd4bea2543" href="8f14e45fceea167a5a36dedd4bea2543" coords="857,612,933,610,936,456,852,455" shape="poly">
    <area target="" alt="c9f0f895fb98ab9159f51fd0297e236d" title="c9f0f895fb98ab9159f51fd0297e236d" href="c9f0f895fb98ab9159f51fd0297e236d" coords="1475,857,1473,354,1537,335,1541,901" shape="poly">
    <area target="" alt="45c48cce2e2d7fbdea1afc51c7c6ad26" title="45c48cce2e2d7fbdea1afc51c7c6ad26" href="45c48cce2e2d7fbdea1afc51c7c6ad26" coords="1324,766,1300,752,1303,401,1325,397" shape="poly">
    <area target="" alt="d3d9446802a44259755d38e6d163e820" title="d3d9446802a44259755d38e6d163e820" href="d3d9446802a44259755d38e6d163e820" coords="1202,695,1217,704,1222,423,1203,423" shape="poly">
    <area target="" alt="6512bd43d9caa6e02c990b0a82652dca" title="6512bd43d9caa6e02c990b0a82652dca" href="6512bd43d9caa6e02c990b0a82652dca" coords="1154,668,1146,661,1144,442,1157,442" shape="poly">
    <area target="" alt="c20ad4d76fe97759aa27a0c99bff6710" title="c20ad4d76fe97759aa27a0c99bff6710" href="c20ad4d76fe97759aa27a0c99bff6710" coords="1105,628,1116,633,1113,447,1102,447" shape="poly">
    <area target="" alt="c51ce410c124a10e0db5e4b97fc2af39" title="c51ce410c124a10e0db5e4b97fc2af39" href="c51ce410c124a10e0db5e4b97fc2af39" coords="1073,609,1081,620,1082,459,1073,463" shape="poly">
</map>

```

Next step is converting the MD5 hashes and this look like `id` of each area

1. **c4ca4238a0b923820dcc509a6f75849b** - 1
    
2. **c81e728d9d4c2f636f067f89cc14862c** → `2`
    
3. **a87ff679a2f3e71d9181a67b7542122c** → `7`
    
4. **e4da3b7fbbce2345d7772b0674a318d5** → `5`
    
5. **1679091c5a880faf6fb5e6087eb1b2dc** → `3`
    
6. **8f14e45fceea167a5a36dedd4bea2543** → `4`
    
7. **c9f0f895fb98ab9159f51fd0297e236d** → `0`
    
8. **45c48cce2e2d7fbdea1afc51c7c6ad26** → `3`
    
9. **d3d9446802a44259755d38e6d163e820** → `6`
    
10. **6512bd43d9caa6e02c990b0a82652dca** → `9`
    
11. **c20ad4d76fe97759aa27a0c99bff6710** → `12`
    
12. **c51ce410c124a10e0db5e4b97fc2af39** → `13`
    

---

Tried adjusting the 12 hashes here `http://IP_Address/hash`:

* the 12 hashes on the image map don’t work but trying a different id which is md5 hash equavilent to 0, the flag is revealed
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753710942114/bc93b3fb-4f0a-46b5-8a8b-e104238a927a.png align="center")

At this point we know that this is an IDOR challenge and tried considering the next possible id like 0 and getting it’s `id`.

> “Security through obscurity isn’t security – always enforce proper access controls, even on hidden or hashed IDs.”

**cfcd208495d565ef66e7dff9f98764da** → `0`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753712416320/e0584d24-e0ed-41ba-aaa8-a883bf232013.png align="center")

Using this one reveals the flag

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753712478769/ecfcce66-425d-4d32-8021-ddb3c0ed2dbc.png align="center")

### **Lessons for Developers**

1. **Never Rely on Obfuscation**  
    Encoding or hashing object identifiers (like MD5 of IDs) does not provide security. If access control checks are missing, attackers can still manipulate them.
    
2. **Implement Proper Authorization**  
    Always verify that the user is authorized to access the requested resource, regardless of how the identifier is presented.
    
3. **Use Non-Predictable IDs**  
    Consider using UUIDs or securely generated random identifiers combined with proper access control to make guessing object references harder.
    
4. **Conduct Security Testing**  
    Regularly test for IDOR vulnerabilities as part of secure code review and penetration testing, especially in legacy or static-looking applications.
    

By converting these MD5 hashes back to their original numeric values, it became clear the application was using obfuscated, but not securely protected, identifiers. Changing the identifier to the MD5 of `0` (`cfcd208495d565ef66e7dff9f98764da`) directly exposed the flag without any authorization checks. This is a classic example of an **Insecure Direct Object Reference (IDOR)**, where sensitive data can be accessed simply by modifying an object reference in the request.