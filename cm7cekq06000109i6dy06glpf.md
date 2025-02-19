---
title: "Web Fundamentals: Burp Suite - Intruder (TryHackMe)"
datePublished: Wed Feb 19 2025 21:06:02 GMT+0000 (Coordinated Universal Time)
cuid: cm7cekq06000109i6dy06glpf
slug: web-fundamentals-burp-suite-intruder-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1739651954487/ddc717f2-36bd-4a50-81e2-93f1f4e5d2b5.png
tags: tryhackme, burpsuite, write-up, webfundamentals, pitchfork, intruder

---

This article will cover the [Burp Suite: Intruder](https://tryhackme.com/room/burpsuiteintruder) write-up under the Web Fundamentals on THM.

## Introduction

#### **Welcome to the Burp Suite Intruder room!**

In this room, we will explore Burp Suite's Intruder module, which offers automated request manipulation and enables tasks such as fuzzing and brute-forcing. If you are not familiar with Burp Suite's **Proxy** and **Repeater** functionality, it is recommended to complete at least the [Burp Basics room bef](https://tryhackme.com/room/burpsuitebasics)ore proceeding.

Burp Suite's I[ntruder mod](https://tryhackme.com/room/burpsuitebasics)ule is a powerful tool that allows for automated and customisable attacks. It provides the ability to modify specific parts of a request and perform repetitive tests with variations of input data. Intruder is particularly useful for tasks like fuzzing and brute-forcing, where different values need to [be tested a](https://tryhackme.com/room/burpsuitebasics)gainst a target.

Deploy the target VM attached to this task by pressing the green **Start Machine** button. Also, start the AttackBox by pressing the blue **Start AttackBox** button at the top of this room if you are not using your own machine. Then start Burp and follow along with the next tasks.

## What is Intruder

Intruder is Burp Suite's built-in fuzzing tool that allows for automated request modification and repetitive testing with variations in input values. By using a captured request (often from the Proxy module), Intruder can send multiple requests with slightly altered values based on user-defined configurations. It serves various purposes, such as brute-forcing login forms by substituting username and password fields with values from a wordlist or performing fuzzing attacks using wordlists to test subdirectories, endpoints, or virtual hosts. Intruder's functionality is comparable to command-line tools like **Wfuzz** or **ffuf**.

However, it's important to note that while Intruder can be used with Burp Community Edition, it is rate-limited, significantly reducing its speed compared to Burp Professional. This limitation often leads security practitioners to rely on other tools for fuzzing and brute-forcing. Nonetheless, Intruder remains a valuable tool and is worth learning how to use it effectively.

Let's explore the Intruder interface:

![Showing the intruder interface](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/2b2a10c651bee6531f8dbeb5e32733e8.png align="left")

The initial view of Intruder presents a simple interface where we can select our target. This field will already be populated if a request has been sent from the Proxy (using `Ctrl + I` or right-clicking and selecting "Send to Intruder").

There are four sub-tabs within Intruder:

* **Positions**: This tab allows us to select an attack type (which we will cover in a future task) and configure where we want to insert our payloads in the request template.
    
* **Payloads**: Here we can select values to insert into the positions defined in the **Positions** tab. We have various payload options, such as loading items from a wordlist. The way these payloads are inserted into the template depends on the attack type chosen in the **Positions** tab. The **Payloads** tab also enables us to modify Intruder's behavior regarding payloads, such as defining pre-processing rules for each payload (e.g., adding a prefix or suffix, performing match and replace, or skipping payloads based on a defined regex).
    
* **Resource Pool**: This tab is not particularly useful in the Burp Community Edition. It allows for resource allocation among various automated tasks in Burp Professional. Without access to these automated tasks, this tab is of limited importance.
    
* **Settings**: This tab allows us to configure attack behavior. It primarily deals with how Burp handles results and the attack itself. For instance, we can flag requests containing specific text or define Burp's response to redirect (3xx) responses.
    

**Note:** The term "fuzzing" refers to the process of testing functionality or existence by applying a set of data to a parameter. For example, fuzzing for endpoints in a web application involves taking each word in a wordlist and appending it to a request URL (e.g., [http://MACHINE\_IP/WORD\_GOES\_HERE](http://MACHINE_IP/WORD_GOES_HERE)) to observe the server's response.

**Answer the questions below**

1. In which Intruder tab can we define the "Attack type" for our planned attack? `Positions`
    

## Positions

When using Burp Suite Intruder to perform an attack, the first step is to examine the positions within the request where we want to insert our payloads. These positions inform Intruder about the locations where our payloads will be introduced (as we will explore in upcoming tasks).

Let's navigate to the Positions tab:

![Showing the positions tab](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/1372bbafab835e10806ee6beb6681f36.png align="left")

Notice that Burp Suite automatically attempts to identify the most probable positions where payloads can be inserted. These positions are highlighted in green and enclosed by section marks (`§`).

On the right-hand side of the interface, we find the following buttons: `Add §`, `Clear §`, and `Auto §`:

* The `Add §` button allows us to define new positions manually by highlighting them within the request editor and then clicking the button.
    
* The `Clear §` button removes all defined positions, providing a blank canvas where we can define our own positions.
    
* The `Auto §` button automatically attempts to identify the most likely positions based on the request. This feature is helpful if we previously cleared the default positions and want them back.
    

The following GIF demonstrates the process of adding, clearing, and automatically reselecting positions:

![Process of adding, clearing, and automatically reselecting positions](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/504d75f7c90e1c985dae5e05038f50ac.gif align="left")

Take some time to familiarize yourself with adding, clearing, and auto-selecting positions using the Burp Suite Intruder interface.

**Answer the questions below**

1. What symbol defines the start and the end of a payload position? `§`
    

## Payloads

In the **Payloads** tab of Burp Suite Intruder, we can create, assign, and configure payloads for our attack. This sub-tab is divided into four sections:

![Payloads sub-tab divided into four sections](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/f049dff9e1172b76c0f81ad4771b012d.png align="left")

1. **Payload Sets**:
    
    * This section allows us to choose the position for which we want to configure a payload set and select the type of payload we want to use.
        
    * When using attack types that allow only a single payload set (Sniper or Battering Ram), the "Payload Set" dropdown will have only one option, regardless of the number of defined positions.
        
    * If we use attack types that require multiple payload sets (Pitchfork or Cluster Bomb), there will be one item in the dropdown for each position.
        
    * **Note:** When assigning numbers in the "Payload Set" dropdown for multiple positions, follow a top-to-bottom, left-to-right order. For example, with two positions (`username=§pentester§&password=§Expl01ted§`), the first item in the payload set dropdown would refer to the username field, and the second item would refer to the password field.
        
2. **Payload settings**:
    
    * This section provides options specific to the selected payload type for the current payload set.
        
    * For example, when using the "Simple list" payload type, we can manually add or remove payloads to/from the set using the **Add** text box, **Paste** lines, or **Load** payloads from a file. The **Remove** button removes the currently selected line, and the **Clear** button clears the entire list. Be cautious with loading huge lists, as it may cause Burp to crash.
        
    * Each payload type will have its own set of options and functionality. Explore the options available to understand the range of possibilities.
        
        ![Available options](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/722a734ac5dd6437211504d3bc7bb6c2.png align="left")
        
3. **Payload Processing**:
    
    * In this section, we can define rules to be applied to each payload in the set before it is sent to the target.
        
    * For example, we can capitalize every word, skip payloads that match a regex pattern, or apply other transformations or filtering.
        
    * While you may not use this section frequently, it can be highly valuable when specific payload processing is required for your attack.
        
4. **Payload Encoding**:
    
    * The section allows us to customize the encoding options for our payloads.
        
    * By default, Burp Suite applies URL encoding to ensure the safe transmission of payloads. However, there may be cases where we want to adjust the encoding behavior.
        
    * We can override the default URL encoding options by modifying the list of characters to be encoded or unchecking the "URL-encode these characters" checkbox.
        

By leveraging these sections, we can create and customise payload sets to suit the specific requirements of our attacks. This level of control allows us to finely tune our payloads for effective testing and exploitation.

**Answer the questions below**

1. Which **Payload processing** rule could we use to add characters at the end of each payload in the set? `Add suffix`
    

## Introduction to Attack Types

The **Positions** tab of Burp Suite Intruder has a dropdown menu for selecting the attack type. Intruder offers four attack types, each serving a specific purpose. Let's explore each of them:

1. **Sniper**: The Sniper attack type is the default and most commonly used option. It cycles through the payloads, inserting one payload at a time into each position defined in the request. Sniper attacks iterate through all the payloads in a linear fashion, allowing for precise and focused testing.
    
2. **Battering ram**: The Battering ram attack type differs from Sniper in that it sends all payloads simultaneously, each payload inserted into its respective position. This attack type is useful when testing for race conditions or when payloads need to be sent concurrently.
    
3. **Pitchfork**: The Pitchfork attack type enables the simultaneous testing of multiple positions with different payloads. It allows the tester to define multiple payload sets, each associated with a specific position in the request. Pitchfork attacks are effective when there are distinct parameters that need separate testing.
    
4. **Cluster bomb**: The Cluster bomb attack type combines the Sniper and Pitchfork approaches. It performs a Sniper-like attack on each position but simultaneously tests all payloads from each set. This attack type is useful when multiple positions have different payloads, and we want to test them all together.
    

Each attack type has its advantages and is suitable for different testing scenarios. Understanding their differences helps us select the appropriate attack type based on the testing objectives.

**Answer the questions below**

1. What attack type cycles through the payloads inserting one payload at a time into each position defined in the request? `Sniper`
    

## Sniper

The **Sniper** attack type is the default and most commonly used attack type in Burp Suite Intruder. It is particularly effective for single-position attacks, such as password brute-force or fuzzing for API endpoints. In a Sniper attack, we provide a set of payloads, which can be a wordlist or a range of numbers, and Intruder inserts each payload into each defined position in the request.

Let's refer to our example template from before:

Example Positions

```html
POST /support/login/ HTTP/1.1
Host: MACHINE_IP
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Origin: http://MACHINE_IP
Connection: close
Referer: http://MACHINE_IP/support/login/
Upgrade-Insecure-Requests: 1

username=§pentester§&password=§Expl01ted§
```

In this example, we have two positions defined for the `username` and `password` body parameters. In a Sniper attack, Intruder takes each payload from the payload set and substitutes it into each defined position in turn.

Assuming we have a wordlist with three words: `burp`, `suite`, and `intruder`, Intruder would generate six requests:

| Request Number | Request Body |
| --- | --- |
| 1 | `username=burp&password=Expl01ted` |
| 2 | `username=suite&password=Expl01ted` |
| 3 | `username=intruder&password=Expl01ted` |
| 4 | `username=pentester&password=burp` |
| 5 | `username=pentester&password=suite` |
| 6 | `username=pentester&password=intruder` |

Observe how Intruder starts with the first position (`username`) and substitutes each payload into it, then moves to the second position (`password`) and performs the same substitution with the payloads. The total number of requests made by Intruder Sniper can be calculated as `requests = numberOfWords * numberOfPositions`.

The Sniper attack type is beneficial when we want to perform tests with single-position attacks, utilizing different payloads for each position. It allows for precise testing and analysis of different payload variations.

**Answer the questions below**

1. If you were using Sniper to fuzz three parameters in a request with a wordlist containing 100 words, how many requests would Burp Suite need to send to complete the attack? `300`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739648359364/69f3fd47-32a5-4fb9-bb5f-e67a1728eaa1.png align="center")
    
2. How many sets of payloads will Sniper accept for conducting an attack? `1`
    

## Battering Ram

The **Battering ram** attack type in Burp Suite Intruder differs from Sniper in that it places the same payload in every position simultaneously, rather than substituting each payload into each position in turn.

Let's refer back to our previous example template:

Example Positions

```html
POST /support/login/ HTTP/1.1
    Host: MACHINE_IP
    User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 37
    Origin: http://MACHINE_IP
    Connection: close
    Referer: http://MACHINE_IP/support/login/
    Upgrade-Insecure-Requests: 1
    
    username=§pentester§&password=§Expl01ted§
```

Using the Battering Ram attack type with the same wordlist from before (`burp`, `suite`, and `intruder`), Intruder would generate three requests:

| Request Number | Request Body |
| --- | --- |
| 1 | `username=burp&password=burp` |
| 2 | `username=suite&password=suite` |
| 3 | `username=intruder&password=intruder` |

As shown in the table, each payload from the wordlist is inserted into every position for each request made. In a Battering Ram attack, the same payload is thrown at every defined position simultaneously, providing a brute-force-like approach to testing.

The Battering Ram attack type is useful when we want to test the same payload against multiple positions at once without the need for sequential substitution.

In the upcoming tasks, we will explore further configurations and settings related to Intruder's Battering Ram attack type and examine its applications in different scenarios.

**Answer the questions below**

As a hypothetical question: You need to perform a Battering ram Intruder attack on the example request above.

If you have a wordlist with two words in it (`admin` and `Guest`) and the positions in the request template look like this:  
`username=§pentester§&password=§Expl01ted§`

1. What would the body parameters of the *first* request that Burp Suite sends be? `username=admin&password=admin`
    

## Pitchfork

The **Pitchfork** attack type in Burp Suite Intruder is similar to having multiple Sniper attacks running simultaneously. While Sniper uses one payload set to test all positions simultaneously, Pitchfork utilises one payload set per position (up to a maximum of 20) and iterates through them all simultaneously.

To better understand Pitchfork, let us revisit our brute-force example, but this time with two wordlists:

1. The first wordlist contains usernames: `joel`, `harriet`, and `alex`.
    
2. The second wordlist contains passwords: `J03l`, `Emma1815`, and `Sk1ll`.
    

We can use these two lists to perform a Pitchfork attack on the login form. Each request made during the attack would look like this:

| Request Number | Request Body |
| --- | --- |
| 1 | `username=joel&password=J03l` |
| 2 | `username=harriet&password=Emma1815` |
| 3 | `username=alex&password=Sk1ll` |

As shown in the table, Pitchfork takes the first item from each list and substitutes them into the request, one per position. It then repeats this process for the next request by taking the second item from each list and substituting it into the template. Intruder continues this iteration until one or all of the lists run out of items. It's important to note that Intruder stops testing as soon as one of the lists is complete. Therefore, in Pitchfork attacks, it is ideal for the payload sets to have the same length. If the lengths of the payload sets differ, Intruder will only make requests until the shorter list is exhausted, and the remaining items in the longer list will not be tested.

The Pitchfork attack type is especially useful when conducting credential-stuffing attacks or when multiple positions require separate payload sets. It allows for simultaneous testing of multiple positions with different payloads.

In the upcoming tasks, we will explore further configurations and settings related to Intruder's Pitchfork attack type and explore its applications in different scenarios, including credential-stuffing attacks.

**Answer the questions below**

1. What is the maximum number of payload sets we can load into Intruder in Pitchfork mode? `20`
    

## Cluster Bomb

The **Cluster bomb** attack type in Burp Suite Intruder allows us to choose multiple payload sets, one per position (up to a maximum of 20). Unlike Pitchfork, where all payload sets are tested simultaneously, Cluster bomb iterates through each payload set individually, ensuring that every possible combination of payloads is tested.

To illustrate the Cluster bomb attack type, let's use the same wordlists as before:

* Usernames: `joel`, `harriet`, and `alex`.
    
* Passwords: `J03l`, `Emma1815`, and `Sk1ll`.
    

In this example, let's assume that we don't know which password belongs to which user. We have three users and three passwords, but the mappings are unknown. In this case, we can use a Cluster bomb attack to try every combination of values. The request table for our username and password positions would look like this:

| Request Number | Request Body |
| --- | --- |
| 1 | `username=joel&password=J03l` |
| 2 | `username=harriet&password=J03l` |
| 3 | `username=alex&password=J03l` |
| 4 | `username=joel&password=Emma1815` |
| 5 | `username=harriet&password=Emma1815` |
| 6 | `username=alex&password=Emma1815` |
| 7 | `username=joel&password=Sk1ll` |
| 8 | `username=harriet&password=Sk1ll` |
| 9 | `username=alex&password=Sk1ll` |

As shown in the table, the Cluster bomb attack type iterates through every combination of the provided payload sets. It tests every possibility by substituting each value from each payload set into the corresponding position in the request.

Cluster bomb attacks can generate a significant amount of traffic as it tests every combination. The number of requests made by a Cluster bomb attack can be calculated by multiplying the number of lines in each payload set together. It's important to be cautious when using this attack type, especially when dealing with large payload sets. Additionally, when using Burp Community and its Intruder rate-limiting, the execution of a Cluster bomb attack with a moderately sized payload set can take a significantly longer time.

The Cluster bomb attack type is particularly useful for credential brute-forcing scenarios where the mapping between usernames and passwords is unknown.

In the upcoming tasks, we will explore further configurations and settings related to Intruder's Cluster bomb attack type and examine its applications in different scenarios.

**Answer the questions below**

We have three payload sets. The first set contains 100 lines, the second contains 2 lines, and the third contains 30 lines.

1. How many requests will Intruder make using these payload sets in a Cluster bomb attack? `6000`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739649236431/7d397d51-5def-481b-9ce3-062b1b1d6db5.png align="center")
    

## Practical Example

To put our theoretical knowledge into practice, we will attempt to gain access to the support portal located at [`http://MACHINE_IP/support/login`](http://MACHINE_IP/support/login). This portal follows a typical login structure, and upon inspecting its source code, we find that no protective measures have been implemented:

Support Login Form Source Code

```html
---
<form method="POST">
    <div class="form-floating mb-3">
        <input class="form-control" type="text" name=username  placeholder="Username" required>
        <label for="username">Username</label>
    </div>
    <div class="form-floating mb-3">
        <input class="form-control" type="password" name=password  placeholder="Password" required>
        <label for="password">Password</label>
    </div>
    <div class="d-grid"><button class="btn btn-primary btn-lg" type="submit">Login!</button></div>
</form>
---
```

Given the absence of protective measures, we have multiple options to exploit this form, including a cluster bomb attack for brute-forcing the credentials. However, we have an easier approach at our disposal.

Approximately three months ago, Bastion Hosting fell victim to a cyber attack, compromising employee usernames, email addresses, and plaintext passwords. While the affected employees were instructed to change their passwords promptly, there is a possibility that some disregarded this advice.

As we possess a list of known usernames, each accompanied by a corresponding password, we can leverage a credential-stuffing attack instead of a straightforward brute-force. This method proves advantageous and significantly quicker, especially when utilising the rate-limited version of Intruder. To access the leaked credentials, download the file from the target machine using the following command in the AttackBox: `wget` [`http://MACHINE_IP:9999/Credentials/BastionHostingCreds.zip`](http://MACHINE_IP:9999/Credentials/BastionHostingCreds.zip)

#### **Tutorial**

To solve this example, follow these steps to conduct a credential-stuffing attack with Burp Macros:

1. Download and Prepare Wordlists:
    
    * Download and extract the [BastionHostingCreds.zip](http://BastionHostingCreds.zip) file.
        
    * Within the extracted folder, find the following wordlists:
        
        * emails.txt
            
        * usernames.txt
            
        * passwords.txt
            
        * combined.txt
            
    
    These contain lists of leaked emails, usernames, and passwords, respectively. The last list contains the combined email and password lists. We will be using the `usernames.txt` and `passwords.txt` lists.
    
2. Navigate to [`http://MACHINE_IP/support/login`](http://MACHINE_IP/support/login) in your browser. Activate the Burp Proxy and attempt to log in, capturing the request in your proxy. Note that any credentials will suffice for this step.
    
3. Send the captured request from the Proxy to Intruder by right-clicking and selecting "Send to Intruder" or using `Ctrl + I`.
    
4. In the "Positions" sub-tab, ensure that only the username and password parameters are selected. Clear any additional selections, such as session cookies.
    
5. Set the Attack type to "Pitchfork."
    
    ![Set the attack type to pitchfork](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/ebd86a3904d8cce5659194499d31db1d.png align="left")
    
6. Move to the "Payloads" sub-tab. You will find two payload sets available for the username and password fields.
    
    ![Configure the two payload sets](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/f9ca1e72d694d8d27f7318637321d2be.png align="left")
    
7. In the first payload set (for usernames), go to "Payload Options," choose "Load," and select the `usernames.txt` list.
    
    * Repeat the same process for the second payload set (for passwords) using the `passwords.txt` list.
        
    * The entire process can be seen in the GIF image below:
        
        ![Showing the entire process](https://assets.muirlandoracle.co.uk/thm/modules/burp/settingPayloads.gif align="left")
        
8. Click the **Start Attack** button to begin the credential-stuffing attack. A warning about rate-limiting may appear; click **OK** to proceed. The attack will take a few minutes to complete in Burp Community.
    
9. Once the attack starts, a new window will display the results of the requests. However, as Burp sent 100 requests, we need to identify which one(s) were successful.
    
    Since the response status codes are not differentiating successful and unsuccessful attempts (all are 302 redirects), we need to use the response length to distinguish them.
    
    ![Showing the result with the smallest response length](https://assets.muirlandoracle.co.uk/thm/modules/burp/2ed757b27276.png align="left")
    
    Click on the header for the "Length" column to sort the results by byte length. Look for the request with a shorter response length, indicating a successful login attempt.
    
10. To confirm the successful login attempt, use the credentials from the request with the shorter response length to log in.
    

**Answer the questions below**

1. What username and password combination indicates a successful login attempt? The answer format is "username:password". `m.rivera:letmein1`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739649442045/068dc774-408f-49cb-9703-047457d73350.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739649481723/ea411244-5e14-4325-a1ac-daf3731e6918.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739649558979/d393685e-6186-4df4-b239-62e719bc4f55.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739649587252/7b97db2f-0904-4ec4-9a56-084161f03172.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739649719826/7af40f14-2536-4567-a981-a0f2514ac91b.png align="center")

## Practical Challenge

Having gained access to the support system, we now have the opportunity to explore its functionalities and see what actions we can perform.

Upon accessing the home interface, we are presented with a table displaying various tickets. Clicking on any row redirects us to a page where we can view the complete ticket. By examining the URL structure, we observe that these pages are numbered in the following format:

[`http://MACHINE_IP/support/ticket/NUMBER`](http://MACHINE_IP/support/ticket/NUMBER)

The numbering system indicates that the tickets are assigned integer identifiers rather than complex and hard-to-guess IDs. This information is significant because it suggests two possible scenarios:

1. **Access Control**: The endpoint may be properly configured to restrict access only to tickets assigned to our current user. In this case, we can only view tickets associated with our account.
    
2. **IDOR Vulnerability**: Alternatively, the endpoint may lack appropriate access controls, leading to a vulnerability known as **Insecure Direct Object References** (IDOR). If this is the case, we could potentially exploit the system and read all existing tickets, regardless of the assigned user.
    

To investigate further, we will utilize the Intruder tool to fuzz the `/support/ticket/NUMBER` endpoint. This approach will help us determine whether the endpoint has been correctly configured or if an IDOR vulnerability is present. Let's proceed with the fuzzing process!

**Note:** You have to capture a request while being logged in.

**Answer the questions below**

1. Which attack type is best suited for this task? `Sniper`
    
    Configure an appropriate position and payload (the tickets are stored at values between 1 and 100), then start the attack
    
2. You should find that at least five tickets will be returned with a status code 200, indicating that they exist.
    
    Either using the **Response** tab in the **Attack Results** window or by looking at each successful (i.e. 200 code) request manually in your browser, find the ticket that contains the flag.
    
3. What is the flag? `THM{MTMxNTg5NTUzMWM0OWRlYzUzMDVjMzJl}`  
      
    switch to the support site URL, login using the credentials we found in the previous step, and navigate to the page that shows the tickets `/support/ticket/NUMBER` You will see three tickets, we’ll move to the next step:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739994902192/1211e73d-5db4-4685-883b-906ad91536d3.png align="center")
    

Create a Python script that has numbers 0 - 100. Remember our intruder attack is a Sniper and load the numbers.txt file to to payload before starting the attack.  

```python
for I in the range(101):
         print (i)
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739995480050/37cc2ec2-377d-4e1f-83c6-c00ad7afc3c0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739995827126/caac267c-d77b-4228-83d6-d439e6894c90.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739995856949/f4f9245d-f30d-45c3-86de-cc1a827a85f7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739996014116/aade3336-9c30-44d8-83cd-4c29f959122d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739996057005/f179a68c-0e44-4d1f-990b-f8b8e027109a.png align="center")

## Extra Mile Challenge

In this extra-mile exercise, we will tackle a more challenging variant of the credential-stuffing attack we previously performed. However, this time, additional measures have been implemented to make brute-forcing more difficult. If you are comfortable using Burp Macros, you can attempt this challenge without the instructions below. Otherwise, let's proceed with the step-by-step approach.

#### **Catching the Request**

Begin by capturing a request to `http://MACHINE_IP/admin/login/` and reviewing the response. Here is an example of the response:

Example Response

```html
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Fri, 20 Aug 2021 22:31:16 GMT
Content-Type: text/html; charset=utf-8
Connection: close
Set-Cookie: session=eyJ0b2tlbklEIjoiMzUyNTQ5ZjgxZDRhOTM5YjVlMTNlMjIzNmI0ZDlkOGEifQ.YSA-mQ.ZaKKsUnNsIb47sjlyux_LN8Qst0; HttpOnly; Path=/
Vary: Cookie
Front-End-Https: on
Content-Length: 3922
---
<form method="POST">
    <div class="form-floating mb-3">
        <input class="form-control" type="text" name=username  placeholder="Username" required>
        <label for="username">Username</label>
    </div>
    <div class="form-floating mb-3">
        <input class="form-control" type="password" name=password  placeholder="Password" required>
        <label for="password">Password</label>
    </div>
    <input type="hidden" name="loginToken" value="84c6358bbf1bd8000b6b63ab1bd77c5e">
    <div class="d-grid"><button class="btn btn-warning btn-lg" type="submit">Login!</button></div>
</form>
```

In this response, we notice that alongside the username and password fields, there is now a session cookie set, as well as a CSRF (**Cross-Site Request Forgery**) token in the form as a hidden field. Refreshing the page reveals that both the **session** cookie and the **loginToken** change with each request. This means that for every login attempt, we need to extract valid values for both the session cookie and the loginToken.

To accomplish this, we will use **Burp Macros** to define a repeated set of actions (macro) to be executed before each request. This macro will extract unique values for the session cookie and loginToken, replacing them in every subsequent request of our attack.

#### **Tutorial**

1. Navigate to `http://MACHINE_IP/admin/login/`. Activate **Intercept** in the Proxy module and attempt to log in. Capture the request and send it to Intruder.
    
2. Configure the positions the same way as we did for brute-forcing the support login:
    
    * Set the attack type to "Pitchfork".
        
    * Clear all predefined positions and select only the username and password form fields. Our macro will handle the other two positions.
        
    * ![Showing the predefined positions](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/cdade963a8004b0d9da03e075a60f3aa.png align="left")
        
3. Now switch over to the Payloads tab and load in the same username and password wordlists we used for the support login attack.
    
    Up until this point, we have configured Intruder in almost the same way as our previous credential stuffing attack; this is where things start to get more complicated.
    
4. With the username and password parameters handled, we now need to find a way to grab the ever-changing loginToken and session cookie. Unfortunately, "recursive grep" won't work here due to the redirect response, so we can't do this entirely within Intruder – we will need to build a macro.
    
    Macros allow us to perform the same set of actions repeatedly. In this case, we simply want to send a GET request to `/admin/login/`.
    
    Fortunately, setting this up is a straightforward process.
    
    * Switch over to the main "Settings" tab at the top-right of Burp.
        
    * Click on the "Sessions" category.
        
    * Scroll down to the bottom of the category to the "Macros" section and click the **Add** button.
        
    * The menu that appears will show us our request history. If there isn't a GET request to `http://MACHINE_IP/admin/login/` in the list already, navigate to this location in your browser, and you should see a suitable request appear in the list.
        
    * With the request selected, click **OK**.
        
    * Finally, give the macro a suitable name, then click **OK** again to finish the process.
        
    
    There are a lot of steps here, comparatively speaking, so the following GIF shows the entire process:
    
    ![Process showing the addition of the macro](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/3b370cfce8a050faf415c7d9a5a8227e.gif align="left")
    
5. Now that we have a macro defined, we need to set Session Handling rules that define how the macro should be used.
    
    * Still in the "Sessions" category of the main settings, scroll up to the "Session Handling Rules" section and choose to **Add** a new rule.
        
    * A new window will pop up with two tabs in it: "Details" and "Scope". We are in the Details tab by default.
        
    * ![Window showing the details and scope](https://assets.muirlandoracle.co.uk/thm/modules/burp/38ceffeebf99.png align="left")
        
    * Fill in an appropriate description, then switch to the Scope tab.
        
    * In the "Tools Scope" section, deselect every checkbox other than Intruder – we do not need this rule to apply anywhere else.
        
    * In the "URL Scope" section, choose "Use suite scope"; this will set the macro to only operate on sites that have been added to the global scope (as was discussed in [Burp Basics](https://tryhackme.com/room/burpsuitebasics)). If you have not set a global scope, keep the "Use custom scope" option as default and add `http://MACHINE_IP/` to the scope in this section.
        
        ![Change the URL scope](https://assets.muirlandoracle.co.uk/thm/modules/burp/4d3fc6d19a12.png align="left")
        
6. Now we need to switch back over to the Details tab and look at the "Rule Actions" section.
    
    * Click the **Add** button – this will cause a dropdown menu to appear with a list of actions we can add.
        
    * Select "Run a Macro" from this list.
        
    * In the new window that appears, select the macro we created earlier.
        
    
    As it stands, this macro will now overwrite all of the parameters in our Intruder requests before we send them; this is great, as it means that we will get the loginTokens and session cookies added straight into our requests. That said, we should restrict which parameters and cookies are being updated before we start our attack:
    
    * Select "Update only the following parameters and headers", then click the **Edit** button next to the input box below the radio button.
        
    * In the "Enter a new item" text field, type "loginToken". Press **Add**, then **Close**.
        
    * Select "Update only the following cookies", then click the relevant **Edit** button.
        
    * Enter "session" in the "Enter a new item" text field. Press **Add**, then **Close**.
        
    * Finally, press **OK** to confirm our action.
        
    
    The following GIF demonstrates this final stage of the process:
    
    ![Showing the added macro](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/250c140897e3b470093e7232c70c07cf.gif align="left")
    
7. Click **OK**, and we're done!
    
8. You should now have a macro defined that will substitute in the CSRF token and session cookie. All that's left to do is switch back to Intruder and start the attack!
    
    **Note:** You should be getting 302 status code responses for every request in this attack. If you see 403 errors, then your macro is not working properly.
    
9. As with the support login credential stuffing attack we carried out, the response codes here are all the same (302 Redirects). Once again, order your responses by length to find the valid credentials. Your results won't be quite as clear-cut as last time – you will see quite a few different response lengths: however, the response that indicates a successful login should still stand out as being significantly shorter.
    
10. Use the credentials you just found to log in (you may need to refresh the login page before entering the credentials).
    

**Answer the questions below**

1. What username and password combination indicates a successful login attempt? The answer format is "username:password". `o.bennett:bella1`  
    
    * unzip the zip file provided in the previous steps if you don’t have it locally. `wget` [`http://MACHINE_IP:9999/Credentials/BastionHostingCreds.zip`](http://MACHINE_IP:9999/Credentials/BastionHostingCreds.zip)
        
    * Open Burpsuite, as usual, change the browser settings then open the browser `http://MACHINE_IP/admin/login/`. This is quite a tricky part you need to enter any username and password e.g random names and numbers when the intercept is off then turn on intercept before logging in this will send a POST request with status code 302 to our HTTP history on the proxy
        
    * Send the request to the intruder, set Pitchfork intruder attack, and change the username and password from the random one you had used to look like the one on the screenshot on the room notes using the e.g  
        `username=§attacker§&password=§password§&loginToken=loginToken_provided`
        
    * on the payload load the username.txt and password.txt files
        
    * then switch to the Sessions part of the settings to create a Macro that we will run:
        
        * start by **Macros** part by adding a macro, select the `GET request with 200` status code click OK, update the name of the macro then click OK
            
        * move back to **Session handling rules** to add a Rule, update the rule name on the details section, and switch to scope unselect other tool scopes and remain with intruder only
            
        * switch back to **details** under the **Rule actions** add: **Run a macro**  
            
            ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739998448102/df9a9691-32be-40b2-8554-4f1ee03be90c.png align="center")
            
        * In the next page that opens up, we’ll select the macro that we had added then makea few changes:
            
            * select the **Update only the following parameters and headers** radio button then edit the button below it to enter the `loginToken` and add it then proceed to close  
                
                ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739998621888/3097880c-d32d-432b-89e2-a15f6e4a0e24.png align="center")
                
            * select **update only the following cookies** then proceed to edit and add `session` then close  
                
                ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739998791205/fc306020-bcc0-4d37-843e-2f4fe64b2543.png align="center")
                
        * from here you can go back and run the Start Attack button: make sure that it’s showing status code 302 if it’s 200 it means it’s not a POST request and if it’s 403 it means the configuration wasn’t done well either on the macro or the login.  
              
              
            They said that the answer has a unique length, that’s why I picked the one that had 427  
            

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739997163269/515d23d8-4e9e-48dc-985f-dcc488af9b4c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739997177307/d9511a65-54e4-4b45-91f1-9b10f1aa52d8.png align="center")

1. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739996333788/986db712-4278-4b20-8341-a6d67d326bfc.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739998826488/7ea62b80-0741-4726-9b37-e92fb7a4eaab.png align="center")

## Conclusion

Congratulations on completing the Burp Suite Intruder room!

By now, you should have a solid understanding of how to effectively utilize Intruder to automate requests and perform various types of attacks.

In the next room of the module, we will be looking at some of [Burp Suite's other modules](https://tryhackme.com/room/burpsuiteom)!

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the Lab THM challenges.