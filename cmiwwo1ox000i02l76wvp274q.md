---
title: "Malware Analysis - Egg-xecutable  (TryHackMe) 🧑‍🎄🎉"
datePublished: Sat Dec 06 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmiwwo1ox000i02l76wvp274q
slug: malware-analysis-egg-xecutable-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765183370811/11bbbaf4-d62f-4ee1-a440-e894a6db9f7c.png
tags: windows, tryhackme, malware-analysis, executable-file, adventofcyber2025

---

Before diving into this challenge, I’ve always found Windows-based malware analysis a bit intimidating and usually avoided it. However, this room completely changed my perspective. For the first time, I really enjoyed analysing an executable using tools like **PeStudio**, **RegShot**, and **ProcMon**—all without running PowerShell or accidentally executing anything unsafe.

This extra introduction summarizes my approach, thoughts, and learning experience as I investigated *HopHelper.exe* before the official TryHackMe story begins.

## Introduction

**The Story**

![Task banner for day 6](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762347310499.svg align="left")

The town of Wareville remains quiet in the middle of the night. While the residents of Wareville are nicely tucked up in bed, blissfully unaware, the SOC team at *The Best Festival Company (TBFC)* remain alert, poised and ready for whatever may face them.

Monitoring their screens, armed with a freshly poured mug of hot cocoa, the elves of the SOC watch their dashboards diligently. 

Suddenly, the elves receive an email in unison from Elf McClause, Head of Elf Affairs, in their inboxes. It reads:

> **From:** Elf McClause
> 
> **Subject:** A new schedule awaits
> 
> **Attachments:** HopHelper.exe 
> 
> To all Elves, going forward, we will be using a new program for creating and viewing team rotas. 
> 
> This new program will revolutionalise how rotas are made here at The Best Festival Company.
> 
> Please contact IT support if you have issues running the program.
> 
> Kind regards, remain the best,
> 
> Elf McClause
> 
> Head of Elf Affairs

*"Why is Elf McClause working at 3AM?"* Screams a member of the SOC team in the background. They're right, something is amiss.

Elf McBlue is immediately suspicious. Their years of experience in the SOC have given them the wisdom not to download "out of the blue" executables. Without McSkidy's wisdom, Elf McBlue takes charge, loading up their malware investigation toolkik - the investigation begins.

## **Learning Objectives**

Today's room will have you taking the place of Elf McBlue, a highly talented member of The Best Festival Company's malware investigation squad. You have been tasked with investigating a highly suspicious executable that is being shared within the company. In today's room, we will be covering the following:

* The principles of malware analysis
    
* An introduction to sandboxes
    
* Static vs. dynamic analysis
    
* Tools of the trade: PeStudio, ProcMon, Regshot
    

## Malware Analysis Using Sandboxes

## **Principles of Malware Analysis**

Malware analysis is the process of examining a malicious file to understand its functionality, operation, and methods for defence against it. By analysing a malicious file or application, we can see exactly how it operates, and therefore, know how to prevent it. For example, could the malicious file communicate with an attacker's server? We can block that server.

Could the malicious file leave traces on the machine? We can use these to determine if the malware has ever infected another device. Instead of fearing malware, we can take a proactive approach by translating technical findings into practical defensive measures and understanding how the malware fits into an attacker's techniques.

There are two main branches of malware analysis: **static** and **dynamic**. Static analysis focuses on inspecting a file without executing it, whereas dynamic analysis involves execution. We will come to these shortly.

**Sandboxes**

In cyber security, sandboxes are used to execute potentially dangerous code. Think of this as disposable digital play-pens. These sandboxes are safe, isolated environments where potentially malicious applications can perform their actions without risking sensitive data or impacting other systems.

The use of sandboxes is part of the **golden rule in malware analysis**: **never run dangerous applications on devices you care about**.

![Malware sandboxing](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/5fc2847e1bbebc03aa89fbf2-1763131163116.png align="left")

Most of the time, sandboxes present themselves as virtual machines. Virtual machines are a popular choice for sandboxing because you can control how the system operates and benefit from features such as snapshotting, which allows you to create and restore the machine to various stages of its status. 

To reiterate, it is **imperative** to understand that potentially malicious code and applications should **only be run in a safe, isolated environment**. From now on, this room will refer to malicious code and applications as samples.

With these fundamentals covered, let's move on to the practical section of today's room. **The following will demonstrate a sample; you must apply these techniques to the** `HopHelper.exe` file presented to you within the "HopHelper" folder on the Desktop of the practical VM.

## **Interactive: Static Analysis**

As we alluded to previously in this room, we use static analysis to gather information about a sample without executing it and digging deep.  

Static analysis can be a quick and effective way to understand how the sample *may* operate, as well as how it can be identified. Some of the information that can be gathered from static analysis has been included in the table below:

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>Information</strong></p></td><td colspan="1" rowspan="1"><p><strong>Explanation</strong></p></td><td colspan="1" rowspan="1"><p><strong>Example</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p>Checksums</p></td><td colspan="1" rowspan="1"><p>These checksums are used within cyber security to track and catalogue files and executables. For example, you can Google the checksum to see if this has been identified before.</p></td><td colspan="1" rowspan="1"><p><code>a93f7e8c4d21b19f2e12f09a5c33e48a</code></p></td></tr><tr><td colspan="1" rowspan="1"><p>Strings</p></td><td colspan="1" rowspan="1"><p>"Strings" are sequences of readable characters within an executable. This could be, for example, IP addresses, URLs, commands, or even passwords!</p></td><td colspan="1" rowspan="1"><p><code>138.62.51.186</code></p></td></tr><tr><td colspan="1" rowspan="1"><p>Imports</p></td><td colspan="1" rowspan="1"><p>"Imports" are a list of libraries and functions that the application depends upon. For example, rather than building everything from scratch, applications will use operating system functions and libraries to interact with the OS.</p></td><td colspan="1" rowspan="1"><p><code>CreateFileW</code></p></td></tr><tr><td colspan="1" rowspan="1"><p>Resources</p></td><td colspan="1" rowspan="1"><p>"Resources" contain data such as the icon that is displayed to the user. This is useful to examine, especially since malware&nbsp;might use a Word document icon to trick the user.<br><br>Additionally, malware itself has been known to hide in this section!</p></td><td colspan="1" rowspan="1"><p>N/A</p></td></tr></tbody></table>

However, it's important to note that regardless of how a sample may appear or function, we don't truly know until it's executed. Attackers use techniques such as obfuscation to obscure how the sample appears, primarily to evade anti-viruses but also to evade a curious analyst.

**Demonstrating PeStudio**

This section of the room will demonstrate using PeStudio on an example called `downloader.exe`. **Please note** that the information you see will be from this demonstration sample. The sample you will be analysing will be different, but the techniques will still apply.

Please note, it is imperative that you do not launch the `HopHelper.exe` executable yet.

First, we will use PeStudio to gather information about the executable.  
  
**Executive Summary:**

1. **Launch PeStudio**
    
2. **Load the executable into PeStudio**
    
3. **Click on the "indicators" tab in the dropdown**
    
4. **Look for the SHA256Sum**
    

First, we will launch PeStudio and load the executable into it. The shortcut for this has been placed on the Desktop of your analyst machine. You can drag and drop the executable into the PeStudio window, or load it by selecting `File -> Open File` from the toolbar. PeStudio will display some information about the executable.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1761316694282.png align="left")

For us, at this stage, the `file > sha256` property within the table is of interest. This value is a checksum, which is a unique identifier for the executable. We can keep a note of this SHA256 as threat intelligence.

Now, proceed to do this on the sample, `HopHelper.exe`, provided to you on the analyst machine **to answer question 1.**

As a reminder, the `HopHelper.exe` sample can be found in the "HopHelper" folder on the Desktop.

Next, we will proceed with reviewing the "Strings" of the executable. You can do this by clicking on the "strings" indicator on the left pane of PeStudio.

![Viewing the "strings" within an executable](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762522282733.png align="left")

In the context of malware analysis, strings are sequences of readable characters present within an executable. This could be, for example, IP addresses, URLs, commands, or even passwords! As a malware analyst, it's great to have a look at these, as these could reveal the attacker's command infrastructure, which we can use for our defences.

Now, proceed to review the strings on the sample, `HopHelper.exe`, provided to you on the analyst machine **to answer question 2.**

Please note that it may take a few minutes for the "strings" to calculate.

Great! This concludes the static analysis portion of the practical. There's so much more to uncover using static analysis. Feel free to explore if you'd like. Let's proceed to the dynamic analysis below.

## **Interactive: Dynamic Analysis**

This section of the room provides a brief introduction to dynamic analysis. As you recall, dynamic analysis involves executing the malicious sample to identify its behaviours and how it interacts with the operating system.

**Regshot**

Regshot is a widely used utility, especially when analysing malware on Windows. It works by creating two "snapshots" of the registry—one before the malware is run and another afterwards. The results are then compared to identify any changes.

Malware aims to establish persistence, meaning it seeks to run as soon as the device is switched on. A common technique for malware is to add a `Run` key into the registry, which is frequently used to specify which applications are automatically executed when the device is powered on.

**Before executing the malicious sample**, `HopHelper.exe`, let's create a snapshot of our registry as it currently is, so we can compare the difference once the sample has been executed.

Let's load up Regshot and create a capture of the registry as it currently exists. The shortcut to this has also been placed on the Desktop of the analyst machine.

First, change the output directory of the capture to the user's Desktop using the box with three dots in the "Output path" section.

Then, once set, let's create our first snapshot. Press **1st shot** and then **Shot** on the dropdown. Please note that this may take a few minutes to complete.

![Configuring the output path and then creating our first registry snapshot](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1761323311278.png align="left")

Now that we have taken a snapshot of the registry, you should proceed with **executing the** `HopHelper.exe` sample and take another snapshot. We will then compare the difference.

For you, this is the `HopHelper.exe` executable located in the "HopHelper" folder on your Desktop.

*Psst...now that you have executed the sample, you might see that some strange things have happened.*

Once we have executed our sample, let's return to Regshot and capture our second snapshot, using the same procedure as above. Click on the **2nd shot** button and press **Shot** in the dropdown. Regshot is now capturing the registry again, and outputting the differences to a file.

![Using Regshot to create the 2nd snapshot](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1761323991947.png align="left")

And now, after a few seconds, let's press the **Compare** button that appears.

![Comparing the results of the difference between the two registry snapshots. The text editor will open after a minute or so.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1761323991948.png align="left")

We can search for the executable within the log that opens up. 

**ProcMon**

Next, we will explore using ProcMon (Process Monitor) from the Sysinternals suite to investigate today's sample. Proccess Monitor is used to monitor and investigate how processes are interacting with the Windows operating system. It is a powerful tool that allows us to see exactly what a process is doing. For example, reading and writing registry keys, searching for files, or creating network connections.

Open **Process Monitor (ProcMon)**, the shortcut for this has been placed on the Desktop of the analyst machine. Process Monitor will automatically start capturing events of various processes on the system.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762516360387.png align="left")

Now, execute the sample `HopHelper.exe` again and return to Process Monitor to see how it interacts with the operating system.

You will see a lot of information here. Do not worry, we will come onto how to filter this shortly.

After allowing a minute to pass, ensuring the sample has fully executed, we will now stop capturing. To stop capturing more events, click on the **Play** button in the toolbar of Process Monitor.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762516393423.png align="left")

As you can see, there is a lot of information to scroll through here, with the most recent events at the bottom. Here we can see how various system processes are interacting with Windows. Nearly all of it, we don't care about. 

Remember, the task content is demonstrating using Process Monitor for the demonstration sample `downloader.exe`.

You will need to follow along, but doing so for the `HopHelper.exe`.

Let's apply some filters. Afterall, for this demonstration, we only care about the `downloader.exe` sample. To do so, click on the **Filter** button, and then **Filter** within the dropdown.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762869222958.png align="left")

A new window will open.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762516544414.png align="left")

Here we can create some filters to remove some of the noise that we don't care about. Because we want to only look at this `downloader.exe` sample for this demonstration, we can apply a filter like so:

1. Apply the **Process Name** filter
    
2. Set the condition to **is**
    
3. Put in the name of the process we wish to see within the text area
    
4. Press the **Add** button to apply this filter
    
5. And finally click **OK** to save.
    

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762516544233.png align="left")

Once done, returning to the main Process Monitor window, we can already see the filter has worked.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762516627726.png align="left")

Now it is much easier to investigate how the process is interacting with the operating system. Here are some **Operations** that may be of interest to us:

* RegOpenKey
    
* CreateFile
    
* TCP Connect
    
* TCP Recieve
    

However, as you can see, there is still a lot of information. We can further apply filters to look for specific things that we want to investigate, such as these aforementioned **Operations**.

To do so, return to the **Filter** heading and create the filter we want to apply. For example, we can filter by **Operations**. Let's do so below, filtering for any TCP **Operation**:

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762516627630.png align="left")

We will now see all **Operations** that include **TCP**. Remember, you can remove the filters you've previously applied by pressing the filter in the **Filter** list, and pressing **Remove:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762516654013.png align="left")

Or, alternatively, if you wish to start over, you can simply press the **Reset Filter** option when clicking on the **Filter** heading.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de96d9ca744773ea7ef8c00/room-content/5de96d9ca744773ea7ef8c00-1762516750128.png align="left")

Phew! Well done. That concludes the demonstration for today's room. Remember, you will need to apply what you have learnt here on the `**HopHelper.exe**` executable that has been placed in the **HopHelper** folder on the analyst Desktop, to answer the questions below.  
  
Launch the machine, then look around. Finding `HopHelper.exe` took my time, but checking the video helped me know which file it was since I couldn’t find it in the notes. Open the `PeStudio` Then select or drag and drop the HopHelper.exe without launching it

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765182089347/f1ba2a48-ddd0-4b92-a4cc-fde96f5f9749.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765182135113/3656c7c1-76d9-484d-8627-2beebf09b1ab.png align="center")

### Answer the questions below

1. **Static analysis:** What is the SHA256Sum of the HopHelper.exe?  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765182279109/c297aa92-243f-495c-becc-a2dbb714e1cf.jpeg align="center")
    
2. **Static analysis:** Within the strings of HopHelper.exe, a flag with the format THM{XXXXX} exists. What is that flag value?
    
    Note, this can be found towards the bottom of the strings output.  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765182369440/5d99909e-0f27-4cdd-a2e3-638220f440dc.jpeg align="center")
    
3. **Dynamic analysis:** What registry value has the HopHelper.exe modified for persistence?
    
    Note: Provide the full path of the key that has been modified  
      
    For this section, you need to use RegShot to capture the shot before launching the HopHelper.exe and after launching it, so that you can compare and find the path
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765182472211/dc1b4641-bc30-4039-b1cc-db72f496308c.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765182510613/fe972fc1-6fd3-4b3d-8216-92a95ba9a09b.png align="center")
    
    Now we can execute the HopHelper.exe, once executed we notice the regards from King Malhare and also some icons are changed
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765182542147/122e5ac0-e179-4ee0-a234-553a2c444d2c.png align="center")
    
    Now it’s time to take the next shot using Regshot, after which the compare button will be activated, which we will click to look for the path
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765182602020/6a7bf6ed-45b7-4925-bf18-430b3c03d9d4.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765182750549/6d3f5566-2266-4902-b22b-b4a899ad08ed.jpeg align="center")
    
4. **Dynamic analysis**: Filter the output of ProcMon for "TCP" operations. What network protocol is HopHelper.exe using to communicate?  
    Make sure to have executed HopHelper.exe while ProcMon was open and capturing events.  
      
    The next challenge involves using ProcMon. I restarted the machine so that we can have a version where HopHelper.exe has not been executed. After executing the `HopHelper.exe` file, when ProcMon is open and monitoring. After this, I added some filters. This was my first time using ProcMon so I made some mistake and by the end I enjoyed the experience  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765183012882/2875a403-3a5e-402a-89db-65aa608c5102.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765183116922/26476324-5179-4a08-a061-ea2d3089ee71.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765183139264/08f4c15c-460b-410b-9edd-c8962902b3c4.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765183170733/0bf87a33-2fd8-4443-b0a5-f478c237dff2.png align="center")
    
5. **Bonus:** Can you find the web panel that HopHelper.exe is communicating with?
    
6. If you enjoyed today's room, feel free to explore both the [Basic Static Analysis](https://tryhackme.com/room/staticanalysis1) and [Basic Dynamic Analysis](https://tryhackme.com/room/basicdynamicanalysis) rooms, where you can delve deeper into the techniques and tools discussed today.
    

This challenge turned out to be far more enjoyable than I expected, especially considering that I normally shy away from Windows malware tasks. Using tools like **PeStudio** for static analysis and **RegShot**, **ProcMon** for dynamic monitoring helped me understand how malware behaves without needing to run commands or scripts manually.

I appreciated how the room guided me from safe inspection to controlled execution, and by the end, I felt much more confident navigating Windows analysis techniques. This challenge definitely encouraged me to take on more Windows-focused tasks in the future.