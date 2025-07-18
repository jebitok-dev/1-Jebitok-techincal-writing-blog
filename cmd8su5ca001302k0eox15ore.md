---
title: "Challenges: Neighbour - IDOR (TryHackMe)"
datePublished: Fri Jul 18 2025 12:32:25 GMT+0000 (Coordinated Universal Time)
cuid: cmd8su5ca001302k0eox15ore
slug: challenges-neighbour-idor-tryhackme
tags: tryhackme, idor, gobuster, ctf-writeup, webpentesting

---

In this challenge, we explore a vulnerable cloud authentication service called **Authentication Anywhere** — a fictional login platform promising secure access from anywhere. But is it truly secure? 🤔 With the mention of *IDOR* (Insecure Direct Object Reference) in the hints, we're encouraged to test whether user data is properly protected, or if we can uncover secrets hidden in other users' profiles. Using basic recon tools and logical exploitation, we aim to bypass access controls and reveal sensitive information left exposed.

## Neighbour

Check out our new cloud service, Authentication Anywhere -- log in from anywhere you would like! Users can enter their username and password, for a totally secure login process! You definitely wouldn't be able to find any secrets that other people have in their profile, right?

**Access this challenge** by deploying both the vulnerable machine by pressing the green "Start Machine" button located within this task, and the TryHackMe AttackBox by pressing the  "Start AttackBox" button located at the top-right of the page.

Navigate to the following URL using the AttackBox: [http://MACHINE\_IP](http://machine_ip/)

Check out similar content on TryHackMe:

* [IDOR](https://tryhackme.com/room/idor)
    

### Answer the questions below

Find the flag on your neighbor's logged in page!

`nmap -sV IP_Address`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752841034195/eca9d8a3-5f17-43c0-9148-5748ec42bc26.png align="center")

gobuster dir -u http://IP\_Address -w /usr/share/wordlists/dirb/common.txt

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752841088105/1591ed13-075f-4ec0-88f9-6bc217e97351.png align="center")

on visiting the site I came a login form and there’s a not for guests to access the guest credentials

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752841132345/7d8ee086-efb4-4901-af9a-743cd20a42e1.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752841172936/f4a09756-d0d9-47ab-9397-b308dbf26504.png align="center")

I was able to login into the site and signout again, tried to look around for hints of how to access the admin before going back to gobuster

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752841248239/022b793d-c4fd-4ddc-8aa2-1e953905e9bd.png align="center")

tried to check more files/paths using gobusters  
  
`gobuster dir -u http://IP_Address -w /usr/share/wordlists/dirb/common.txt -x php,html,txt`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752841285466/42e73e91-b3a3-4869-9f0b-8bce0e02de41.png align="center")

there’s a `profile.php` and considering that the description hinted that this a IDOR room we’ll try changing the `id` from guest to check if we can access the neighbor. You only access `http://IP_Address/profile.php?user=id` once you’ve logged in

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752841444616/4e191d69-9c53-4ee2-845d-cdb987f802b5.png align="center")

## 🛡️ How Developers Can Prevent IDOR Vulnerabilities

While this challenge was intentionally insecure, it reflects a real-world issue seen far too often in production systems. Here are some ways developers can defend against IDOR attacks:

1. **Always perform server-side authorization checks**  
    Never trust user-submitted IDs or parameters without verifying that the requesting user has permission to access the referenced resource.
    
2. **Use session-based identity**  
    Avoid pulling resources based on URL parameters alone. Instead, use session or token-based identity to fetch user-specific data.
    
3. **Avoid exposing sequential or predictable IDs**  
    Instead of using `/profile.php?id=2`, consider using UUIDs or internal references not easily guessable.
    
4. **Implement Role-Based Access Control (RBAC)**  
    Assign roles (e.g., guest, user, admin) and validate access to resources based on those roles.
    
5. **Log and monitor access patterns**  
    Track user activity and watch for repeated attempts to access different IDs — this can indicate malicious probing.
    
6. **Conduct regular security testing**  
    Include IDOR checks in automated testing or manual reviews, and use tools like Burp Suite or OWASP ZAP to catch these flaws early.
    

By taking these precautions, developers can prevent unauthorized access to sensitive data — and avoid incidents that could compromise user trust or violate data privacy regulations.

This challenge demonstrates how insecure access control mechanisms can leave user data vulnerable — even on a platform claiming "totally secure" logins. By exploiting an IDOR vulnerability in the `profile.php` endpoint, we were able to access another user's page and retrieve the flag. It’s a reminder that authentication isn’t just about login forms — proper authorization checks are critical too. A simple parameter tampering trick is all it took to break the illusion of security.