---
title: "Challenges: The Game (TryHackMe)"
datePublished: Fri Jul 25 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmdn2vz7o001p02l23lvcfp1b
slug: challenges-the-game-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753705226705/7e06dba9-5985-452a-9d7e-58b50874d0d6.png
tags: zip, tryhackme, ctf-writeup, game-hacking

---

Cipher may have vanished, but he left behind encrypted secrets buried inside one of the most unexpected places—a Tetris game executable. The mission was to reverse-engineer the game binary, dig into its codebase, and uncover whatever hidden data Cipher had concealed within. This challenge, originally featured in the Hackfinity Battle 2025 CTF Event, put our binary analysis and OSINT skills to the test.  

Cipher has gone dark, but intel reveals he’s hiding critical secrets inside Tetris, a popular video game. Hack it and uncover the encrypted data buried in its code.

*This challenge was originally a part of the Hackfinity Battle 2025 CTF Event.*

`unzip zipfile`

`binwalk -e exefile`

`cd extractions`

`strings exefile > output.txt`

`cat output.txt | grep -i thm`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753704946126/15bbf33f-de8c-4455-b028-ac976f0b3949.png align="center")

By extracting the contents of the Tetris executable with `binwalk`, analyzing the binary strings, and carefully filtering for potential flags, we successfully uncovered the encrypted data Cipher left behind. This challenge highlights how even simple, familiar software can be weaponized to hide sensitive information and demonstrates the importance of reverse-engineering and static analysis skills in cybersecurity.