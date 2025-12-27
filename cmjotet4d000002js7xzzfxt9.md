---
title: "Race Conditions - Toy to The World
 (TryHackMe) 🧑‍🎄🎉"
datePublished: Sat Dec 27 2025 21:31:02 GMT+0000 (Coordinated Universal Time)
cuid: cmjotet4d000002js7xzzfxt9
slug: race-conditions-toy-to-the-world-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1766870864265/6cde6959-5669-4871-87ec-1a25ad794cca.png
tags: race-condition, tryhackme, websecurity, burpsuite, adventofcyber2025, ctfwriteup

---

High-traffic events are every attacker’s favourite playground. Flash sales, ticket drops, limited releases — all share one thing in common: **pressure on timing**. When systems are pushed to their limits, even a few milliseconds can mean the difference between a secure transaction and a costly vulnerability.

In this challenge, The Best Festival Company (TBFC) discovers this the hard way after launching a highly anticipated, limited-edition SleighToy. Despite having only ten units in stock, confirmation emails flood in for far more purchases than should have been possible. What initially looks like a backend glitch quickly turns into a classic web security flaw: a race condition.

Through this scenario, we explore how concurrent requests can be abused to manipulate application logic, allowing attackers to bypass stock controls and create inconsistent system states. By reproducing the issue using Burp Suite, we gain hands-on experience with how race conditions occur, how they are exploited, and why they are so difficult to detect in real-world applications.

## Introduction

**The Story**

![Task banner for day 20](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1763451486547.png align="left")

The Best Festival Company (TBFC) has launched its limited-edition SleighToy, with only ten pieces available at midnight. Within seconds, thousands rushed to buy one, but something strange happened. **More than ten lucky customers received confirmation emails stating that their orders were successful**. Confusion spread fast. How could everyone have bought the "**last**" toy? McSkidy was called in to investigate.  

She quickly noticed that multiple buyers purchased at the exact moment, slipping through the system’s timing flaw. Sir Carrotbane’s mischievous Bandit Bunnies had found a way to exploit this chaos, flooding the checkout with rapid clicks. By morning, TBFC faced angry shoppers, missing stock, and a mystery that revealed just how dangerous a few milliseconds could be during the holiday rush.

## **Learning Objectives**

* Understand what race conditions are and how they can affect web applications.
    
* Learn how to identify and exploit race conditions in web requests.
    
* How concurrent requests can manipulate stock or transaction values.
    
* Explore simple mitigation techniques to prevent race condition vulnerabilities.
    

## Race Condition

A race condition happens when two or more actions occur at the same time, and the system’s outcome depends on the order in which they finish. In web applications, this often happens when multiple users or automated requests simultaneously access or modify shared resources, such as inventory or account balances. If proper synchronisation isn’t in place, this can lead to unexpected results, such as duplicate transactions, oversold items, or unauthorised data changes.

## **Types of Race Conditions**

Generally, race condition attacks can be divided into three categories:

* **Time-of-Check to Time-of-Use (TOCTOU)**: A TOCTOU race condition happens when a program checks something first and uses it later, but the data changes in between. This means what was true at the time of the check might no longer be true when the action happens. It’s like checking if a toy is in stock, and by the time you click "**Buy**" someone else has already bought it. For example, two users buy the same "last item" at the same time because the stock was checked before it was updated.
    
* **Shared resource**: This occurs when multiple users or systems try to change the same data simultaneously without proper control. Since both updates happen together, the final result depends on which one finishes last, creating confusion. Think of two cashiers updating the same inventory spreadsheet at once, and one overwrites the other’s work.
    
* **Atomicity violation**: An atomic operation should happen all at once, either fully done or not at all. When parts of a process run separately, another request can sneak in between and cause inconsistent results. It’s like paying for an item, but before the system confirms it, someone else changes the price. For example, a payment is recorded, but the order confirmation fails because another request interrupts the process.
    

## **Time for Some Action**

Now that we understand the basic concepts, let's take the example of the TBFC shopping cart app and exploit the race condition.

## **Configuring the Environment**

First, we will configure Firefox to route traffic through Burp Suite. On the AttackBox, open `Firefox`, click the `FoxyProxy` icon (1) and select the `Burp` profile (2) so all browser requests are sent to Burp, as shown below:

![Firefox window to select Burp profile.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1763482270396.png align="left")

Next, click on the Burp Suite icon on the `Desktop` to launch the application. 

![Ubuntu desktop screen with Burp icon.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1761026130664.png align="left")

You will see an introductory screen; choose `Temporary project in memory` and click `Next`.

![Introductory screen in Burp Suite.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760974022370.png align="left")

On the configuration screen, click `Start Burp` to start the application.

![Startup screen in Burp Suite.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760974022091.png align="left")

Once the application is started, you will see the following screen, where we will use the `Proxy` and `Repeater` option to exploit the race condition.

![Welcome screen in Burp Suite.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1763482551368.png align="left")

Before proceeding, ensure that you turn off "`Intercept`" in Burp Suite. Open the `Proxy` tab and check the `Intercept` sub-tab; If the button says "**Intercept on**", click it so it changes to "**Intercept off**". This step ensures that Burp Suite no longer holds your browser requests and allows them to pass through normally.

![Turn off intercept option in Burp Suite.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1765978025648.png align="left")

## **Making a Legitimate Request**

Now that the environment is configured, make a normal request. Open `Firefox`, visit the webapp at `http://MACHINE_IP`, and you will see the following login page:

![TBFC login page.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760974022467.png align="left")

On the site’s login panel, enter the credentials, **username:** `attacker` and **password:** `attacker@123`. After logging in, you’ll arrive at the main dashboard, which shows the limited-edition SleighToy with only 10 units available.

![TBFC welcome page.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760974022385.png align="left")

To make a legitimate purchase, click `Add to Cart` for the SleighToy and then click `Checkout` to go to the checkout page.

![TBFC inventory page.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760974022318.png align="left")

On the checkout page, click `Confirm & Pay` to complete the purchase. You should see a success message confirming the order, as shown below:

![TBFC checkout page.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760970837165.png align="left")

## **Exploiting the Race Condition**

Now that we have made a legitimate request, navigate back to Burp Suite and click on  `Proxy > HTTP history` and find the **POST** request to the `/process_checkout` endpoint created by our legitimate checkout request. Right-click that entry and choose `Send to Repeater`, which will copy the exact HTTP request (headers, cookies, body) into Burp’s Repeater tool as shown below:

![Sending Burp Suite with multiple requests.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760720808915.png align="left")

Next, switch to the `Repeater` tab and confirm the request appears there, right-click on the first tab, select `Add tab to group`, and click on `Create tab group`.

![Burp Suite to create tab group.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760971316638.png align="left")

Enter a name for the tab group, such as `cart`, and click **Create**, which will create a tab group named `cart`.

![Create Tab Group screen in Burp Suite.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760971316538.png align="left")

Then, right-click the request tab and select `Duplicate tab`. When prompted, enter the number of copies you want (for example, 15). You’ll now have that many identical request tabs inside the cart group.

![Duplicate requests in Burp Suite.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760971316687.png align="left")

Next, use the Repeater toolbar `Send` dropdown menu and select `Send group in parallel (last-byte sync)`, which launches all copies at once and waits for the final byte from each response, maximising the timing overlap to trigger race conditions.

![Send Parallel requests in Burp Suite.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760971773327.png align="left")

Once this is done, click `Send group (parallel)`; this will launch all 15 requests to the server simultaneously. The server will attempt to handle them simultaneously, which will cause the timing bug to appear (due to multiple orders being processed at once).

![Response after sending successful requests.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760971773182.png align="left")

Finally, visit the web app, and you will see multiple confirmed orders and the SleighToy stock reduced (possibly going negative). 

![Negative stocks and flag value.](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/62a7685ca6e7ce005d3f3afe-1760972351248.png align="left")

## **Mitigation**

The attacker logged in and made a normal purchase of the limited SleighToy. Using Burp Suite, he captured the checkout request and sent it multiple times in parallel. Because the app didn’t **handle simultaneous checkouts correctly**, each request succeeded before the stock could update. This allowed the attacker to buy more toys than available, resulting in a race condition and pushing the stock into negative values. Here are a few mitigation measures to avoid the vulnerability:

* Use **atomic database transactions** so stock deduction and order creation execute as a single, consistent operation.
    
* Perform a **final stock validation** right before committing the transaction to prevent overselling.
    
* Implement **idempotency keys** for checkout requests to ensure duplicates aren’t processed multiple times.
    
* Apply **rate limiting** or concurrency controls to block rapid, repeated checkout attempts from the same user or session.
    

### Answer the questions below

1. What is the flag value once the stocks are negative for **SleighToy Limited Edition**?  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766870429738/c56cf3ce-752f-4b9e-82ef-4919689699d5.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766870498525/103e775d-38a8-4c95-a779-3ed24384c4d0.png align="center")
    
    Have to duplicate 15 times: if you have trouble replicating the same, you can use the Ctrl + R shortcut 15 times before selecting the. “`send group in parallel option`”  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766870513866/08069fa3-971f-4da1-b6f1-f1cdc2929619.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766870537615/7aa7987a-b6b1-47ed-a5a2-7eaae2db18ea.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766870851154/16374a7f-212c-48bd-8c76-18776dced5d5.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766870759202/b5f0760f-d858-4de2-8200-a92c43f03f55.jpeg align="center")
    
2. Repeat the same steps as were done for ordering the SleighToy Limited Edition. What is the flag value once the stocks are negative for **Bunny Plush (Blue)**?  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766870778300/804a6354-61d2-4815-9137-9d0b6af907cb.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766870811415/82eda6de-6ff5-42e6-94ce-5fb3f3e45e62.jpeg align="center")
    
3. Feel free to check out the [Race Conditions](https://tryhackme.com/r/room/raceconditionsattacks) room if you enjoyed this task.
    

This task highlights how vulnerabilities don’t always come from broken authentication or missing access controls — sometimes they come from **perfectly valid requests sent at the wrong time**. By exploiting a race condition in TBFC’s checkout process, we saw how an attacker could oversell limited inventory and push stock values into impossible states, all without triggering traditional security alarms.

Race condition vulnerabilities are especially dangerous because they often hide behind normal functionality and only appear under high load or deliberate concurrency. As demonstrated, tools like Burp Suite make it easy for attackers to replay and parallelise requests, turning minor logic flaws into major business impacts.

From a defensive standpoint, the lesson is clear: applications must treat critical operations like payments and inventory updates as atomic and idempotent. Proper locking, transaction handling, and concurrency controls are not optional — they are essential safeguards in any system that processes simultaneous user actions.

Ultimately, this challenge reinforces an important security principle: **when timing becomes a variable, security becomes a risk**. Understanding and mitigating race conditions is crucial for building resilient applications, especially during moments when systems are under the most pressure.