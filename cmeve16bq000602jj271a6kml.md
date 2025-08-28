---
title: "Challenges: Reversing ELF (TryHackMe)"
datePublished: Fri Aug 01 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmeve16bq000602jj271a6kml
slug: challenges-reversing-elf-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756384493997/ebad7bf0-e169-4e97-80da-c72b882516a6.png
tags: reverse-engineering, tryhackme, ctf-writeup

---

This series of CrackMe challenges serves as a progressive introduction to binary reverse engineering. Each task gradually increases in complexity, encouraging the use of common tools such as `strings`, `objdump`, `hexdump`, and scripting in Python to analyze executables and recover hidden information. Starting with simple warmups and moving into password recovery and flag extraction, these challenges provide hands-on experience in dissecting binaries and sharpening problem-solving techniques often encountered in Capture the Flag (CTF) exercises and practical reverse engineering.

## Crackme1

Let's start with a basic warmup, can you run the binary?

Answer the questions below

What is the flag? `flag{not_that_kind_of_elf}`

```bash
file crackme1

ls -la crackme1

chmod +x crackme1 && ./crackme1

strings crackme1

hexdump -C crackme1 | head -20

strings crackme1 | grep -E "[a-zA-Z]{3,}"

strings crackme1 | grep -v "^_" | grep -v "^\./" | grep -v "^[A-Z]" | grep -v "^[0-9]" | grep -v "^$"

objdump -d crackme1 2>/dev/null | head -50

objdump -d crackme1 2>/dev/null | grep -A 50 "<main>"

objdump -d crackme1 2>/dev/null | grep -A 100 "<main>" | head -100

python3 -c "
values = [0x25, 0x2b, 0x20, 0x26, 0x3a, 0x2d, 0x2e, 0x33, 0x1e, 0x33, 0x27, 0x20, 0x33, 0x1e, 0x2a, 0x28, 0x2d, 0x23, 0x1e, 0x2e, 0x25, 0x1e, 0x24, 0x2b, 0x25, 0x3c, 0xffffffbf & 0xFF]
base = 0x41  # ASCII 'A'
result = ''
for val in values:
    char_val = (base + val) & 0xFF
    result += chr(char_val)
print('Flag:', result)
"

flag{not_that_kind_of_elf}
```

## Crackme2

Find the super-secret password! and use it to obtain the flag

Answer the questions below

What is the super secret password ? `super_secret_password`

What is the flag ? `flag{if_i_submit_this_flag_then_i_will_get_points}`

```bash
chmod +x crackme2

file crackme2

strings crackme2

objdump -d crackme2 2>/dev/null | grep -A 50 "<main>"

objdump -d crackme2 2>/dev/null | grep -A 30 "<giveFlag>"

objdump -d crackme2 2>/dev/null | grep -A 50 "<giveFlag>" | tail -30

objdump -s crackme2 2>/dev/null | grep -A 5 -B 5 "super_secret_password"

objdump -s crackme2 2>/dev/null | grep -A 20 "80486"

flag{if_i_submit_this_flag_then_i_will_get_points}
```

## Crackme3

Use basic reverse engineering skills to obtain the flag

Answer the questions below

What is the flag? `f0r_y0ur_5ec0nd_le55on_unbase64_4ll_7h3_7h1ng5`

## Crackme4

Analyze and find the password for the binary?

Answer the questions below

What is the password ? `my_m0r3_secur3_pwd`

## Crackme5

What will be the input of the file to get output `Good game` ?

Answer the questions below

What is the input ? ``OfdlDSA|3tXb32~X3tX@sX`4tXtz``

## Crackme6

Analyze the binary for the easy password

Answer the questions below

What is the password ? `1337_pwd`

## Crackme7

Analyze the binary to get the flag

Answer the questions below

What is the flag ? `flag{much_reversing_very_ida_wow}`

## Crackme8

Analyze the binary and obtain the flag

Answer the questions below

What is the flag ? `flag{at_least_this_cafe_wont_leak_your_credit_card_numbers}`

By working through CrackMe1 to CrackMe8, we explored fundamental reverse engineering workflows: inspecting strings, analyzing assembly, identifying hidden logic, and even decoding obfuscated data. The exercises demonstrated how persistence, systematic analysis, and the right tools reveal secrets embedded in executables. While the early levels focused on simple static inspection, later challenges required deeper reasoning and pattern recognition. Collectively, these challenges not only build technical confidence but also prepare us for more advanced reversing scenarios. With these foundations in place, tackling harder CTF problems or real-world reverse engineering tasks becomes far more approachable.

---