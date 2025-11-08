---
title: "Pwnkit: CVE-2021-4034 (TryHackMe)"
datePublished: Sat Nov 08 2025 19:23:30 GMT+0000 (Coordinated Universal Time)
cuid: cmhqoa27b000202l1dywa0tpt
slug: pwnkit-cve-2021-4034-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1762629664215/e5ad51f0-1a09-47db-896b-1dae853145ea.png
tags: exploitation, ctf-writeup, tryhackme-walkthrough, pkexec

---

CVE-2021-4034 — aka **Pwnkit** — is a high-severity local privilege escalation in the Polkit package that was present in Polkit releases dating back to 2009. Because Polkit is installed by default on most mainstream Linux distributions, this vulnerability makes nearly any affected host potentially exploitable by a local, unprivileged user.

In this room we’ll cover what Polkit and `pkexec` do, why the argument-parsing bug lets an attacker overwrite critical environment data, and how that can be turned into root code execution. You’ll see a working exploit, compile and run it against a deliberately vulnerable VM, and then apply practical mitigations so you can harden systems you manage.

This is a hands-on, defensive-minded lab: by the end you should be able to (1) explain the root cause of Pwnkit, (2) demonstrate exploitation in a controlled environment, and (3) apply and verify remediations safely.

## Info: Introduction and Deploy

CVE-2021-4034 (colloquially dubbed "Pwnkit") is a terrifying **L**ocal **P**rivilege **E**scalation (LPE) vulnerability, located in the "Polkit" package installed by default on almost every major distribution of the Linux operating system (as well as many other \*nix operating systems). In other words, it affects virtually every mainstream Linux system on the planet.

This room will provide an overview of the vulnerability, as well as recommendations to patch affected systems. A vulnerable machine has also been attached to allow you to try the vulnerability for yourself!

Without further ado, let's begin.

## Tutorial: Background

**Overview**

CVE-2021-4034 (aka "pwnkit") was discovered by researchers at [Qualys](https://www.qualys.com/) and announced in January 2022; the technical security advisory for this vulnerability can be found [here](https://www.qualys.com/2022/01/25/cve-2021-4034/pwnkit.txt). The vulnerability has existed in every version of the "Policy Toolkit" (or, Polkit) package since it was first released in 2009 and allows any unprivileged attacker to easily obtain full administrative access over any Linux machine with the Polkit package installed. Unfortunately, Polkit is installed by default on most distributions of Linux, making this vulnerability *extremely* widespread.

The ease of exploitation and ubiquitous nature of Polkit make this an absolutely devastating vulnerability; however, fortunately it is not exploitable remotely, making Pwnkit purely a local privilege escalation (LPE) vulnerability.

---

**What is Polkit?**

Before we look at the vulnerability directly, it helps to understand what Polkit actually is.

Polkit is part of the Linux authorisation system. In effect, when you try to perform an action which requires a higher level of privileges, Polkit can be used to determine whether you have the requisite permissions. It is integrated with systemd and is much more configurable than the traditional sudo system. Indeed, it is sometimes referred to as the "sudo of systemd", providing a granular system with which to assign permissions to users.

When interacting with polkit we can use the `pkexec` utility — it is this program that contains the Pwnkit vulnerability. As an example of using the utility, attempting to run the `useradd` command through `pkexec` in a GUI session results in a pop-up asking for credentials:  
`pkexec useradd test1234`  

In a CLI session, we get a text-based prompt instead:

pkexec when executed from the command line

```bash
muiri@demo-vm:~$ pkexec useradd test1234
==== AUTHENTICATING FOR org.freedesktop.policykit.exec ===
Authentication is needed to run '/usr/sbin/useradd' as the super user
Authenticating as: muiri,,, (muiri)
Password:
```

To summarise, the policy toolkit can be thought of as a fine-grained alternative to the simpler sudo system that you may already be familiar with.

---

**The Vulnerability**

As mentioned previously, the Pwnkit vulnerability exists in the `pkexec` utility — the primary front-end to the Polkit system. We won't go into *too* much detail here in the interests of readability; however, you are encouraged to read through the [Qualys Security Advisory](https://www.qualys.com/2022/01/25/cve-2021-4034/pwnkit.txt) for a full technical explanation of the vulnerability.

The short version is this: versions of pkexec released prior to the patch don't handle command-line arguments safely, which leads to an "out-of-bounds write" vulnerability, allowing an attacker to manipulate the environment with which pkexec is run. This is all you *really* need to know, but for a slightly more technical explanation, read on!

More specifically, pkexec attempts to parse any command-line arguments that we pass it using a for-loop, starting at an index of 1 to offset the name of the program and obtain the first real argument (e.g. if we entered `pkexec bash`, then as `pkexec` is the name of the program, it would be argument 0 — the actual command-line arguments start at index 1). The name of the program is irrelevant to argument parsing, so the indexing is simply offset to ignore it.

What happens, then, if we don't provide any arguments? The index is set *permanently* to 1!

The following pseudocode may help you to visualise this:

Pseudocode: List Indexing Starting at 1

```bash
for(n=1; n < number_of_arguments; n++){
  //Do Stuff
}
```

If the number of arguments is 0 then `n` is *never* less than the number of arguments. As such, `n` stays equal to one and the loop is bypassed completely.

This becomes a problem later when pkexec attempts to write to the value of the argument at index `n`. As there are no command-line arguments, there *is* no argument at index `n` — instead the program overwrites the next thing in memory, which just so happens to be the first value in the list of environment variables when the program is called using a C function called `execve()`. In other words, by passing pkexec a null list of arguments, we can force it to overwrite an environment variable instead!

For context: certain "dangerous" environment variables are removed by the operating system when you attempt to run a program that has the [SUID bit](https://muirlandoracle.co.uk/2020/03/05/unix-file-permissions/#SUID) set (as pkexec does by necessity); this is to prevent attackers from being able to hijack the program as it runs with administrative permissions. Using the out-of-bounds write, we are able to re-introduce our choice of these dangerous environment variables by tricking pkexec into adding it for  us. There are a variety of different ways to abuse this, all leading to code execution as the root user.

### Answer the questions below

Is Pwnkit exploitable remotely (Aye/Nay)? Nay

In which Polkit utility does the Pwnkit vulnerability reside? `pkexec`

## Practical: Exploitation

Exploiting Pwnkit is, lamentably, incredibly easy.

There are many exploits available online, and writing your own version is not particularly difficult.

The version that we will be using is written in C by arthepsy, and was released soon after the Qualys security advisory was made public. The repository can be found [here](https://github.com/arthepsy/CVE-2021-4034). This variation of the exploit makes use of the dangerous `GCONV_PATH` variable to include a malicious shared object file that calls the `/bin/sh` shell with root permissions.

---

First, we must connect to the target machine. If you are using the in-browser connection, then you will already have access through your browser, using the terminal on the right-hand side of the screen.

If you are **not** using the in-browser connection, then you can connect using SSH and these credentials:

* Username: `tryhackme`
    
* Password:  `TryHackMe123!`
    

Once connected, we need to navigate to the pre-added `pwnkit/` subdirectory, then compile the exploit using the following command:

`gcc cve-2021-4034-poc.c -o exploit`

We can then run the exploit with `./exploit` to obtain instant root access over the target!

The entire process can be seen in the clip below:

### Answer the questions below

Read through the `cve-2021-4034-poc.c` file and try to understand how it works. See if you can match this up with the [Qualys security advisory](https://www.qualys.com/2022/01/25/cve-2021-4034/pwnkit.txt) and the explanation given in the previous task!

Exploit the vulnerability!

What is the flag located at `/root/flag.txt`?  
  
`gcc cve-2021-4034-poc.c -o exploit`  

`./exploit`

`find / -type f -name flag.txt 2>/dev/null   `

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1762629367341/4e691b91-64e4-4d23-8a59-db960005fa86.png align="center")

**\[Bonus Question — Optional\]** Using the [Qualys advisory](https://www.qualys.com/2022/01/25/cve-2021-4034/pwnkit.txt) and the [repository](https://github.com/arthepsy/CVE-2021-4034) linked in the task, try to write your own version of the Pwnkit exploit

## Tutorial: Remediations

Now that we've seen the devastating impact of CVE-2021-4034, how can we protect against it?

Fortunately, developers tend to be fairly fast when it comes to developing patches for critical vulnerabilities. As a prime example: at the time of writing, Canonical have already released patched versions of the Polkit package in the APT package manager for all versions of Ubuntu which are not end-of-life. The patched version can be installed with a simple apt upgrade — e.g. `sudo apt update && sudo apt upgrade`.

In distributions which have not yet released patched versions of the package, the recommended hotfix is to simply remove the SUID bit from the pkexec binary. This can be done with a command such as the following:

`sudo chmod 0755 which pkexec`

This is far from ideal, however, it works as a temporary solution until more distributions start packaging versions of polkit that are patched against Pwnkit.

It should be noted that there are many variations of the Pwnkit exploit using different environment variables and exploiting the vulnerability in different ways. Some of these leave traces and logs behind, others do not.

You can check to ensure that a system is patched by attempting to run a copy of the exploit against it. If the exploit returns the `pkexec` help menu then the system is patched:

Patched version of pkexec

```bash
tryhackme@pwnkit$ ./exploit
pkexec --version |
       --help |
       --disable-internal-agent |
       [--user username] PROGRAM [ARGUMENTS...]

See the pkexec manual page for more details.
```

### Answer the questions below

Read the remediation task

Patch the vulnerability on any Linux devices that you manage!

## Info: Conclusion

Congratulations, you have reached the end of the Pwnkit room!

During the course of this room, you have been given a high-level overview of CVE-2021-4034 and some of the available remediations to patch it. You have also been able to use one version of the exploit to attack and compromise a vulnerable target for yourself.

You should now have a working knowledge of this devastating vulnerability.

Unfortunately, Pwnkit wasn't the first Polkit vulnerability in this time period. You may also wish to check out [the TryHackMe room covering CVE-2021-3560](https://tryhackme.com/room/polkit): a timing attack that led to another local privilege escalation vulnerability!

Well done — you’ve completed the Pwnkit room. You now understand that a subtle argument-parsing bug in `pkexec` allowed an out-of-bounds write into process environment data and provided a straightforward path to root on almost any Linux system running Polkit. You also compiled and executed a public proof-of-concept, confirmed how patched systems behave, and practised two practical mitigations (installing vendor patches and the temporary SUID removal workaround).

Quick action checklist you can take away:

* **Patch**: Apply vendor Polkit updates as soon as they’re available for your distro.
    
* **Mitigate (if patch unavailable)**: Temporarily remove the SUID bit from `pkexec` (e.g. `sudo chmod 0755 $(which pkexec)`) until an official package is installed.
    
* **Verify**: Test systems after patching — a successful patch will cause exploit PoCs to print `pkexec` usage/help instead of spawning a root shell.
    
* **Audit**: Scan assets for presence of Polkit/pkexec and prioritize systems with local user access.
    
* **Hunt**: Look for evidence of local privilege escalation activity in logs and user sessions.