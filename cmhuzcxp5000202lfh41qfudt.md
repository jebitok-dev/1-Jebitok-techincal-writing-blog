---
title: "OverlayFS - CVE-2021-3493 (TryHackMe)"
datePublished: Tue Nov 11 2025 19:44:45 GMT+0000 (Coordinated Universal Time)
cuid: cmhuzcxp5000202lfh41qfudt
slug: overlayfs-cve-2021-3493-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762890225390/b515a3a5-859b-45ec-9a9e-32ca7510f5cc.png
tags: exploit, tryhackme, cve, kerne

---

OverlayFS is a lightweight Linux kernel feature that merges multiple directories into a single unified filesystem. It’s widely used for live systems, containers, and setups that want a read-only root filesystem with a writable layer on top. While that design is convenient, bugs in kernel features like OverlayFS can be catastrophic — they run at kernel privilege and, when exploitable, let a normal user escalate to root with surprising ease.

This room walks you through a real-world example: **CVE-2021-3493**, an OverlayFS local privilege escalation affecting some Ubuntu releases. You’ll learn the theory behind the vulnerability, how the proof-of-concept (PoC) works at a high level, and—within the safe confines of the TryHackMe lab—how to fetch, compile, and run the PoC against an intentionally vulnerable VM. The goal is educational: to understand why the bug is dangerous, how kernel exploits are structured, and how systems are patched and mitigated.

Follow the lab instructions exactly. The VM is intentionally vulnerable so you can practice legally and safely; do **not** run exploit code against systems you don’t own or have explicit permission to test. If a C compiler isn’t available in the lab, the task explains how to use a precompiled binary or compile elsewhere and transfer the executable to the target.

## What is OverlayFS?

OverlayFS is a Linux kernel module that allows the system to combine several mount points into one, so that you can access all the files from each within one directory structure.

It's often used by live USBs, or some other specialist applications. One use is having a read only root file system, and another partition "overlayed" with that to allow applications to write to a temporary file system.

More resources are included in the final task (Further reading) if you'd like to learn more about OverlayFS and this exploit.

Room Banner by [Maksym Kaharlytskyi](https://unsplash.com/photos/Q9y3LRuuxmg) on [Unsplash](https://unsplash.com/)

## CVE-2021-3493 - OverlayFS Exploit

### **About the vuln**

Recently, SSD-Disclosure released a proof of concept (and a great explanation) for an Ubuntu kernel exploit ([https://ssd-disclosure.com/ssd-advisory-overlayfs-pe/](https://ssd-disclosure.com/ssd-advisory-overlayfs-pe/)).

This vulnerability is particularly serious, as overlayfs is a kernel module that is installed by default on Ubuntu 1804 Server.  
If the system is vulnerable, you can very easily escalate from any user to root, as long as you can run a binary.  
If there isn't a C compiler installed on the machine, you can compile the binary statically elsewhere and copy just the binary over.

### **Credentials for SSH**

`Username: overlay`

`Password: tryhackme123`

*If you're using the web-based machine, open it in full screen (by clicking the icon) to copy/paste the exploit code.*

### Answer the questions below

Deploy the machine with the Start Machine button in this task and wait up to 2 minutes for the VM to boot.

SSH into the machine with the credentials provided in the task text.

Grab the source code for the exploit from [SSD-Disclosure here](https://ssd-disclosure.com/ssd-advisory-overlayfs-pe/) and save it as exploit.c on the target machine.

Compile the exploit with gcc. If you're finding this difficult, a command is given in the hints.

Run your compiled exploit, and get root!  
What's the flag in /root/?

Visit here [SSD-Disclosure here](https://ssd-disclosure.com/ssd-advisory-overlayfs-pe/) scroll through the page and copy the exploit script then create a exploit.c file then copy the script into it.

Run the following commands:  
  
`gcc -o exploit exploit.c`

`./exploit`

`ls -la /root/`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762890116320/c99f0e16-ef9e-48d7-ad30-a33457132ec3.png align="center")

## Further reading

### **Want to know more about OverlayFS?**

[https://yagrebu.net/unix/rpi-overlay.md](https://yagrebu.net/unix/rpi-overlay.md) - Read only root file system with overlayfs to allow applications to run normally.

[https://wiki.archlinux.org/index.php/Overlay\_filesystem](https://wiki.archlinux.org/index.php/Overlay_filesystem) - The Arch Wiki's page on OverlayFS (I don't use Arch BTW)

### **Want to know more about this specific CVE?**

[https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3493](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-3493) - Mitre's CVE entry for this vulnerability, which includes many further links.

[https://ssd-disclosure.com/ssd-advisory-overlayfs-pe/](https://ssd-disclosure.com/ssd-advisory-overlayfs-pe/) - This is where we got the PoC code, and it explains the vulnerability very well.

By completing this room you should be able to:

* Explain what OverlayFS is and why a kernel bug in it is high-impact.
    
* Describe the high-level mechanics of the CVE and the PoC without needing to memorize exploit code.
    
* Confidently perform safe lab tasks: locating files in the VM, compiling a C program with `gcc`, and inspecting `/root` to confirm privilege escalation (within the lab only).
    

For next steps, read the advisory and technical writeups linked in the “Further reading” section to deepen your understanding of kernel exploitation, mitigation techniques (patching, kernel hardening), and safe practices for vulnerability research. Always remember the ethics: practice only in controlled environments and report vulnerabilities responsibly.