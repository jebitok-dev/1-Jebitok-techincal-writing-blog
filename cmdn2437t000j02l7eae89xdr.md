---
title: "Challenges:"
datePublished: Fri Jul 25 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmdn2437t000j02l7eae89xdr
slug: challenges
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753704033959/3c03baa2-aca6-48a1-b22f-a71d0a434f88.png
tags: file, compiled, tryhackme, ctf-writeup

---

This challenge involved analyzing a compiled binary to uncover a hidden password without executing it. The binary, located in the `/root/Rooms/Compiled/` directory, wouldn’t run in the AttackBox environment. Instead, we relied on static analysis techniques—commonly used in reverse engineering—to extract meaningful strings and clues embedded in the compiled code. Tools like `strings` and `file` were essential to understand the binary’s components and spot potential hardcoded secrets.

Download the task file and get started. The binary can also be found in the AttackBox inside the `/root/Rooms/Compiled/` directory.

**Note:** The binary will not execute if using the AttackBox. However, you can still solve the challenge.

### Answer the questions below

What is the password?

`strings Compiled-1688545393558.Compiled | less`

```makefile
/lib64/ld-linux-x86-64.so.2
jKUhR
__cxa_finalize
__libc_start_main
strcmp
stdout
__isoc99_scanf
fwrite
printf
libc.so.6
GLIBC_2.7
GLIBC_2.2.5
GLIBC_2.34
_ITM_deregisterTMCloneTable
gmon_start
_ITM_registerTMCloneTable
PTE1
u+UH
StringsIH
sForNoobH
Password:
DoYouEven%sCTF
__dso_handle
_init
Correct!
Try again!
;*3$"
GCC: (Debian 11.3.0-5) 11.3.0
Scrt1.o
__abi_tag
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.0
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
zzz.c
FRAME_END
_DYNAMIC
__GNU_EH_FRAME_HDR
GLOBAL_OFFSET_TABLE
__libc_start_main@GLIBC_2.34
_ITM_deregisterTMCloneTable
stdout@GLIBC_2.2.5
_edata
_fini
printf@GLIBC_2.2.5
__data_start
strcmp@GLIBC_2.2.5
gmon_start
__dso_handle
_IO_stdin_used
_end
__bss_start
main
__isoc99_scanf@GLIBC_2.7
fwrite@GLIBC_2.2.5
TMC_END
_ITM_registerTMCloneTable
__cxa_finalize@GLIBC_2.2.5
_init
:
```

`file Compiled-1688545393558.Compiled`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753702429196/ea70f3d3-da59-42ed-942d-c1a91e7d5794.png align="center")

  
I tried playing around with different commands and tools as recommended on other walkthrough and by ChatGPT

Password: `DoYouEven_init`

By inspecting the binary strings, we discovered suspicious text, including formatted messages and function names. Among these, the pattern revealed a password embedded directly in the binary: `DoYouEven_init`. This exercise demonstrated how binary analysis can bypass execution restrictions and still uncover critical information, highlighting the value of static analysis in reverse engineering and Capture The Flag (CTF) challenges.