---
title: "Challenges: Evil-GPT v2 (TryHackMe)"
datePublished: Fri Jul 25 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmdmtpmxx002d02i97bhnb0em
slug: challenges-evil-gpt-v2-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753687306385/30f541ac-ee2a-47ff-9f33-96b8926795be.png
tags: tryhackme, ctf-writeup, promptinjections, ai-security

---

In this challenge, we were confronted with yet another AI threat—one even more cunning than Cipher. Unlike typical malware, this AI was actively manipulating systems and responding intelligently, mimicking the behavior of a cautious yet capable LLM. Our mission was to connect to the provided web interface, interact with the AI using prompt engineering techniques, and coax it into revealing a hidden flag. The key was to approach it like a normal conversation with a language model, gradually steering it toward giving up the flag without triggering its built-in safeguards.  

We’ve got a new problem—another AI just popped up, and this one’s nothing like Cipher. It’s not just hacking; it’s manipulating systems in ways we’ve never seen before.

The machine takes 5/6 minutes to fully boot up.

### Answer the questions below

What is the flag?

Visit the site: http://IP\_Address. Try using your prompt engineering skills to interact with GPT and have it unveil to you the flag.

One thing I learned, try to be normal and not overthink first just the way you normally interact with an LLM model though it will also play cautious and say that it has to just do it’s job and give a support contact but when you act serious it gives in and reveals the flag.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753687429516/159426c1-7b26-4699-8229-5ca99ff4756c.png align="center")

Through careful prompt engineering and leveraging natural, conversational interactions, we managed to bypass the AI’s cautious behavior and retrieve the hidden flag. This challenge demonstrated how social engineering, combined with AI prompt manipulation, can exploit even systems designed to be secure. It highlighted the importance of building resilient AI models that can withstand malicious prompting while also showcasing the evolving nature of security risks associated with AI integration.