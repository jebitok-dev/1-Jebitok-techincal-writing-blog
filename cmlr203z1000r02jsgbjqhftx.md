---
title: "Cryptography Concepts (TryHackMe)"
datePublished: Tue Feb 17 2026 20:26:30 GMT+0000 (Coordinated Universal Time)
cuid: cmlr203z1000r02jsgbjqhftx
slug: cryptography-concepts-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1771359587544/8cb6fd8f-f648-4102-9c22-8266a9f588fa.png
tags: cryptography, cybersecurity, tryhackme, beginners-cybersecurity

---

[Cryptography Concepts](https://tryhackme.com/room/cryptographyconcepts) is a pre-security in TryHackMe that covers the fundamentals of cryptography, which is a method of secure encoding of data to ensure confidentiality, integrity, and availability of data in all states, i.e., data at rest, data in use, and data in transit

## Introduction

**Opening question:** When you see the little padlock icon in your browser's address bar, what's actually stopping someone from reading or modifying your data as it travels across the internet?

![Side-by-side comparison of plaintext and ciphertext communication.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/5fc2847e1bbebc03aa89fbf2-1768817690734.png align="left")

## **Why Cryptography Matters**

Remember the CIA Triad from the last room? Confidentiality, integrity, and availability are the three pillars of cyber security. We saw how attackers try to break these through disclosure, alteration, and destruction.

Now here's the practical question: **how do we actually protect secrets and detect tampering in the real world?** That's where **cryptography** comes in.

## **A Real-World Scenario**

Imagine you're running a small medical clinic. You need to send patient records, including names, medical conditions, and treatment history, to specialists and insurance companies over the internet. The problem? Data doesn't travel directly from you to the recipient. It bounces through dozens of computers and routers along the way. Without protection, anyone with access to those systems could read, change, or block your data.  
Cryptography solves this by using mathematical rules and secret keys to scramble information into gibberish that only authorised people can unscramble.  
In this room, we're keeping things simple. No math background needed. We'll use everyday analogies, plain language, and a hands-on game to show you how cryptography works in practice.

## **Learning Objectives**

By the end of this room, you'll be able to:

* Explain what cryptography is and why it matters for protecting confidentiality and integrity.
    
* Describe the difference between plaintext and ciphertext with actual examples.
    
* Explain what keys and algorithms are, and why keeping keys secret is critical.
    
* Explain the difference between symmetric and asymmetric encryption using everyday objects, such as lockboxes and mailboxes.
    
* Describe how symmetric and asymmetric encryption work together to protect your web browsing.
    

## **Prerequisites**

Before jumping into this room, you should have completed all the previous modules and covered the concepts in the following rooms:

* [CIA Triad](https://tryhackme.com/room/theciatriad)
    
* [Data Encoding](https://tryhackme.com/room/dataencoding)
    

## Hiding Information - Symmetric Encryption

**Opening question:** If someone's listening to every single piece of data travelling between two people, how can those two people still share secrets?

## **Understanding the Basics**

Before we jump into symmetric encryption, let's define the core terms we'll use throughout this room:

* **Plaintext** - A message you can read normally. Like `HELLO` or `Patient name: Alice Smith`.
    
* **Ciphertext** - A scrambled version that's not supposed to make sense. Like `KHOOR` or `Sdwlhqw qdph: Dolfh Vplwk`.
    
* **Key** - The secret ingredient that controls how scrambling and unscrambling work. Think of it as a password that the algorithm uses.
    
* **Algorithm** - The public recipe—the set of steps that explain how to use the key on the message. Everyone can know the algorithm. Security comes from keeping the key secret.
    

Real-world cryptography is way more sophisticated than what we'll use here. But the basic pattern stays the same:

**Encryption process: plaintext + encryption algorithm + key  → ciphertext**

![Block diagram of encryption using a secret key](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/5fc2847e1bbebc03aa89fbf2-1771331232484.png align="left")

and then

**Decryption process: ciphertext + decryptiong algorithm + key   → plaintext**

![Block diagram of decryption using a secret key](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/5fc2847e1bbebc03aa89fbf2-1771331232460.png align="left")

## **The Lockbox Analogy**

Think about a physical lockbox:

* The **algorithm** is how the lock works. Anyone can see you insert a key and turn it, hence it's not secret.
    
* The **key** is your specific metal key. Only people with that exact key can open your box.
    
* The **plaintext** is the letter inside the box.
    
* The **ciphertext** is that locked box travelling through the postal system.
    

Nobody tries to hide how locks work to make them secure. Security comes from keeping your key private and this same principle applies to cryptography. Algorithms are usually public and tested by experts worldwide. The security comes from keeping keys secret.

To put it more practically, Alice wants to send Bob a secret letter, but it must go through the public postal system, where anyone could open it and read it.

Here's what she does:

1. She writes her message (the **plaintext**) on paper.
    
2. She puts the letter in a sturdy lockbox.
    
3. She locks it with a padlock using her key.
    
4. She sends the locked box (the **ciphertext**) through the mail.
    

When Bob gets the box, he uses his copy of the same key to unlock it and read the message. Anyone who intercepts the box along the way sees a locked metal box. Without the key, it's useless. The message stays private.

That's **symmetric encryption** in a nutshell: one key locks the box, the same key unlocks it.

![Illustration of symmetric encryption. Alice and Bob each hold a matching key. Alice sends a locked box labeled "CIPHERTEXT" through a cloud filled with masked attackers. A white arrow shows the box reaching Bob, who can open it with his copy of the same key.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/5fc2847e1bbebc03aa89fbf2-1768818141131.png align="left")

## **Plaintext versus Ciphertext**

Now, how does this look when dealing with text or data? Say Alice wants to send:

```bash
HELLO
```

That's the **plaintext**—the readable message.

Alice uses an algorithm and a secret key to scramble it. After scrambling, it becomes:

```bash
KHOOR
```

That's the **ciphertext**. To anyone without the key, `KHOOR` is meaningless. The key point: **ciphertext should look like random nonsense to anyone who doesn't have the key**.

Bob receives `KHOOR`, uses the same key and algorithm, and unscrambles it back to `HELLO`.

## **The Caesar Cipher: Algorithm Plus Key**

To make this concrete, we'll use something called the **Caesar cipher**. It's straightforward, which makes it perfect for learning (but terrible for real security—more on that in a minute).

The Caesar cipher is named after Julius Caesar, who reportedly used this technique over 2000 years ago to send military messages. Even back then, people understood the value of keeping communications secret.

**How It Works**

The Caesar cipher shifts each letter in your message by a fixed number of positions in the alphabet. That fixed number is your **key**.

Let's say the key is **3**:

* `A` shifts forward 3 spots to become `D`
    
* `B` becomes `E`
    
* `C` becomes `F`  
    and so on.
    
* `X` becomes `A` (it wraps around to the start)
    
* `Y` becomes `B`
    
* `Z` becomes `C`
    

If Alice wants to encrypt `HELLO` with a key of **3**:

* `H` → `K`
    
* `E` → `H`
    
* `L` → `O`
    
* `L` → `O`
    
* `O` → `R`
    

So `HELLO` becomes `KHOOR`.

To decrypt `KHOOR`, Bob shifts each letter **backwards** by 3:

* `K` → `H`
    
* `H` → `E`
    
* `O` → `L`
    
* `O` → `L`
    
* `R` → `O`
    

He gets `HELLO` again. Magic? Nope. Math.

With the Caesar cipher:

* The **algorithm** (shift each letter by some number) is completely public. Everyone can know how it works.
    
* The **key** (the number 3 in our example) is what's secret. Only Alice and Bob know this number.
    

If someone intercepts `KHOOR` but doesn't know the key, they'd have to try all 25 possible shifts (1 through 25) to find the right one. For a human, that's tedious; for a computer, that takes about a millisecond.

**Here's the thing:** The Caesar cipher is **not secure** and is **never used in real systems**. It's way too easy to compromise and decrypt messages. We're using it here purely because it's simple to understand and shows you how keys and algorithms work together.

Real algorithms like AES (Advanced Encryption Standard) are vastly more complex and secure. But they follow the same basic idea: algorithm + key + plaintext → ciphertext.

## **Symmetric Encryption Explained**

The Caesar cipher is an example of **symmetric encryption**. This means that:

* The same key **encrypts** (locks) and **decrypts** (unlocks) the message.
    
* Both sender and receiver need a copy of that key.
    
* The key has to stay secret from everyone else.
    

Some of the benefits of using symmetric encryption are:

* **It's fast.** Symmetric algorithms can churn through huge amounts of data really quickly.
    
* **It's efficient.** Perfect for encrypting files, hard drives, and network traffic where speed matters.
    

However, there's a catch to this efficiency:

> **How do Alice and Bob share that key safely in the first place?**

If they send the key over the internet in plain view, an eavesdropper can grab it. Then that eavesdropper can decrypt every future message.

You might think, "*Just encrypt the key!*",  but then you'd need another key to encrypt *that* key, and then another key for *that* key, and you see the problem. Infinite regress.

This is called the **key distribution problem**, and it's the Achilles' heel of symmetric encryption when used alone.

We'll solve this in the next task with **asymmetric encryption**—a clever approach that uses two different keys instead of one.

## **Try It Yourself: The "Secret Message Rescue" Game.**

Time to put this into practice.

In this game, you're helping a security team that's being monitored on their office Wi-Fi. Attackers are watching everything. The team uses a simple Caesar cipher to communicate safely, and you need to:

* Decrypt secret warnings that were intercepted.
    
* Encrypt new messages before sending them.
    

The game uses the Caesar cipher with different shift keys. You'll adjust the key, watch the message change, and submit your answers.

Remember: this is purely educational. Real systems don't use the Caesar cipher because it's laughably weak.

After you finish all the levels, you'll get a flag. Submit that flag for Question 1 below.

### Answer the questions below

1. What's the flag you received after completing all levels of the Secret Message Rescue game?
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771359644463/e04e4319-a5dd-46e1-bd74-ed1b7e34428a.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771359665998/1cba7b73-f7f8-44d6-9470-9e429db94337.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771359682577/4c74e92a-4f62-4904-bc6c-dbc091ac8aab.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771359765251/fa2b04b5-6c59-4ed0-ac59-31820c5b9451.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771359702537/e10b6e39-267d-4e06-bec0-71939aaa6637.png align="center")
    
2. Using the Caesar cipher with a key of 5, what does `CYBER` become when encoded? (Uppercase, no spaces.)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771359785718/0254d0d2-8247-42dc-8ff7-3ae5e9267905.png align="center")
    
3. Using the Caesar cipher, find the correct key and decode the following secret message: `FVZCYR PNRFNE PVCURE`.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771359860692/0529d438-db2a-4d79-9093-3eba9138be40.png align="center")
    

## Sharing Keys Safely: Asymmetric Encryption

**Opening question:** If Alice and Bob have never met and can't safely send a key over the internet, how can they start encrypting messages to each other?

## **The Key Distribution Problem**

In the last task, we saw how symmetric encryption works. Alice and Bob use the same key for both encryption and decryption. It's fast and efficient.

However, we also hit a wall: **how do they share that key safely in the first place?**

If they send it in plaintext, an attacker grabs it. If they encrypt the key, they need another key, which brings us right back to the same problem.

Enter **asymmetric encryption**.

## **Two Keys Instead of One**

Asymmetric encryption uses **two mathematically linked keys**:

* A **public key** that anyone can know and use.
    
* A **private key** that only one person keeps secret.
    

Here's the clever part:

* If you encrypt something with someone's **public key**, only their **private key** can decrypt it.
    
* If you encrypt something with your **private key**, anyone with your **public key** can decrypt it (this is primarily used for digital signatures, which we won't delve into here).
    

The two keys are connected by some serious maths, but it would take an ordinary computer hundreds or even thousands of years to recover the private key from the public key. This computational difficulty is what makes asymmetric encryption secure.

## **The Mailbox Analogy**

Let us use a physical mailbox on a street corner as an example:

* The **mail slot** at the top is the **public key**. Anyone walking by can drop off a letter. It's completely open and accessible.
    
* The **locked door** at the front is the **private key**. Only the mailbox owner has the key to open it and grab the letters.
    

When Alice wants to send Bob a secret:

1. Alice finds Bob's public key (the mail slot). This isn't a secret—Bob can post it on his website or email it around.
    
2. Alice writes her message, encrypts it with Bob's public key, and sends it.
    
3. Only Bob can decrypt it because he is the only one with the private key (the key to the door).
    

Even if an attacker intercepts the encrypted message, they can't decrypt it without Bob's private key.

## **Solving the Key Distribution Problem**

With asymmetric encryption, Alice and Bob don't need to share a secret key beforehand. A simple flow of events can be as follows:

1. Bob creates a **public key** and a **private key** on his computer. He keeps the private key to himself and shares the public key with the world.
    
2. Alice grabs Bob's public key (maybe from his website or a key server).
    
3. Alice encrypts her message using Bob's public key and sends it off.
    
4. Bob receives it and decrypts it using his private key.
    

At no point did they need to exchange a key over the network secretly. The only key that travelled publicly was Bob's **public key**, which isn't secret by design. That's the solution to the key distribution problem.

![Alice and Bob using a public and private key pair to exchange messages](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/5fc2847e1bbebc03aa89fbf2-1771331968602.png align="left")

## **Real-world Use: HTTPS**

The most common everyday use of asymmetric encryption is in **HTTPS**—the secure protocol you use whenever you see that padlock in your browser.

Here's what happens when you visit [`https://google.com`](https://google.com):

1. Your browser requests the website's public key.
    
2. The website sends back its public key wrapped in a **certificate** (more on this shortly).
    
3. Your browser and the website use asymmetric encryption to agree on a shared secret (a symmetric key) without anyone else being able to see it.
    
4. From there on, they switch to fast **symmetric encryption** using that shared secret for the rest of the session.
    

This combo is sometimes called a **hybrid approach**:

* **Asymmetric encryption** solves the problem of key distribution.
    
* **Symmetric encryption** handles the heavy lifting because it's way faster.
    

You might wonder: how does Alice know the public key really belongs to Bob, and not to an attacker pretending to be Bob? That's where **certificates** come in.

A certificate is a digital document that:

* Contains someone's public key.
    
* States who that key belongs to (like [`example.com`](http://example.com)).
    
* A trusted authority digitally signs it, called a **Certificate Authority** (CA).
    

Your browser and operating system come preloaded with a list of trusted CAs. When a website hands over a certificate:

* Your browser checks that a trusted CA signed it.
    
* Your browser checks that it's still valid (not expired or revoked).
    
* If everything looks good, your browser shows the padlock and trusts the public key.
    

If something's off—perhaps the certificate has expired or was signed by an untrusted authority—your browser displays a warning and may refuse to connect.

## **Viewing a Certificate In Your Browser**

You can peek at the certificate for any HTTPS site right now. These steps can guide you to view certificates:

1. Visit any HTTPS site (try [`https://www.tryhackme.com`](https://www.tryhackme.com)).
    
2. Click the **padlock icon** in the address bar.
    
3. Look for something like "Certificate", "Connection is secure", or "View certificate".
    
4. A window opens showing details like:
    
    * **Issued to:** The website's domain.
        
    * **Issued by:** The CA that signed it.
        
    * **Valid from / Valid until:** The certificate's expiration dates.
        

![Accessing TryHackMe's certificate details](https://tryhackme-images.s3.amazonaws.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/5fc2847e1bbebc03aa89fbf2-1768812516907.gif align="left")

This is how your browser knows it's talking to the real website and not an attacker's fake version.

## **Symmetric vs Asymmetric: Side by Side**

In summary, let's compare the two approaches we've covered:

| **Feature** | **Symmetric Encryption** | **Asymmetric Encryption** |
| --- | --- | --- |
| Number of keys | One key for both encrypting and decrypting | Two keys: public and private |
| Key sharing | Both people need the same secret key | Public key can be shared openly |
| Speed | Very fast | Slower (used for small amounts of data) |
| Main use | Encrypting bulk data (files, network traffic) | Sharing keys, securely and digital certificates |
| Analogy | One key locks and unlocks a box | A mailbox: anyone posts, only the owner retrieves |

In practice, **real systems use both**:

* Asymmetric encryption initiates a connection and securely shares a symmetric key.
    
* Symmetric encryption takes over for the remainder of the session to efficiently handle data.
    

This is how HTTPS, VPNs, and encrypted messaging apps all operate.

### Answer the questions below

1. In asymmetric encryption, which key stays secret? `private key`
    
2. With asymmetric encryption, Alice can encrypt a message using Bob's public key, and only Bob's private key can decrypt it. Yay or Nay? `yay`
    
3. What problem does asymmetric solve that symmetric cannot? `key distribution`
    
4. After initial asymmetric exchange in HTTPS, what encryption type handles bulk data? `symmetric`
    

## Conclusion

## **What We've Covered**

In this room, we explored the basics of cryptography and its role in protecting confidentiality—one of the three pillars of the CIA Triad. We covered the core ideas:

* Plaintext is what you can read. Ciphertext is scrambled gibberish.
    
* A key is the secret that controls scrambling and unscrambling.
    
* An algorithm is the public method for using the key.
    

We looked at two flavours of encryption:

* Symmetric encryption uses a single key for both encryption and decryption. It's fast and efficient, but you need a secure way to share that key. We used the Caesar cipher to see how this works.
    
* Asymmetric encryption uses two linked keys: a public key that anyone can use and a private key that only one person keeps. This solves the key distribution problem and powers the initial handshake for HTTPS connections.
    

We also saw how real systems combine both types:

* Asymmetric encryption sets up a shared key at the start.
    
* Symmetric encryption handles the actual data because it's faster.
    

That combo is what protects your passwords, banking details, and messages when you see that padlock in your browser.

Cryptography is one of the most critical tools in a defender's arsenal. It protects confidentiality and integrity, and it's the backbone of almost every secure system you use online. But it's not magic. It's one layer in a much bigger security picture that includes:

* Strong password practices.
    
* Secure key storage.
    
* User awareness and training.
    
* Regular software updates.
    
* Monitoring and incident response.
    

Understanding how crypto works and where it can fail helps you think more carefully about those other layers.

## **Further Learning**

In the upcoming rooms of this module, Become a Hacker and Become a Defender, you will learn about the interesting concepts of offensive and defensive security with some fun exercises.