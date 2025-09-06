---
title: "Hackfinity Battle 2025 CTF: Sneaky Patch (TryHackMe)"
datePublished: Sat Sep 06 2025 16:26:09 GMT+0000 (Coordinated Universal Time)
cuid: cmf8h7bjz000s02jr9xe35nz5
slug: hackfinity-battle-2025-ctf-sneaky-patch-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1757175878478/36af955d-a35e-4e4c-a425-f57b17e97b8e.png
tags: cybersecurity, forensics, tryhackme, ctf-writeup, securityanalyst

---

In this challenge, a high-value system was suspected to be compromised by a stealthy kernel-level backdoor. Traditional security tools failed to detect the attacker’s presence, signaling the need for a manual forensic investigation. The objective was to inspect the system’s kernel modules, identify malicious artifacts, and extract hidden information that could reveal the attacker’s persistence method.

## Forensics - Sneaky Patch

A high-value system has been compromised. Security analysts have detected suspicious activity within the kernel, but the attacker’s presence remains hidden. Traditional detection tools have failed, and the intruder has established deep persistence. Investigate a live system suspected of running a kernel-level backdoor.

`ls /proc | grep -E '^[0-9]+$'`

`cat /proc/modules`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757174282971/3a634448-2d45-428d-abe2-2efa58ad596d.png align="center")

`uname -r find /lib/modules/$(uname -r) -type f -name "spatch.ko" 2>/dev/null`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757174311496/28dc9721-73d1-4bba-85bd-edc2ddfd2653.png align="center")

`strings /lib/modules/6.8.0-1016-aws/kernel/drivers/misc/spatch.ko | less`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757175690402/91718a16-92e5-4045-ab94-bb4379695654.png align="center")

used [dcode.fr](https://www.dcode.fr/ascii-code) to decode the ASCII Code

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757175745448/c50ebbb9-403b-4d9d-86ed-9ead5029df42.png align="center")

Through systematic inspection of the `/proc` filesystem and loaded kernel modules, the malicious file `spatch.ko` was uncovered. By analyzing the module with `strings`, hidden ASCII-encoded data was retrieved and later decoded to reveal the attacker’s secret. This investigation highlights the importance of low-level forensic analysis when dealing with advanced threats, as attackers often bypass conventional detection by embedding themselves deep within the kernel.