---
title: "Session Forensics (TryHackMe)"
datePublished: Sat Sep 06 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmfgrnyf6000s02laftxsadmc
slug: session-forensics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1757331543910/5adf7852-3123-42b1-92c6-d6bd2a4bc5b7.png
tags: web-security, tryhackme, jwttoken, jwtsecurity, sessionforensic

---

Sessions and tokens are at the heart of modern web authentication and authorisation, making them prime targets for attackers. This room focuses on how sessions and JSON Web Tokens (JWTs) are created, stored, and used within applications, along with the forensic techniques required to investigate when they are compromised. Through hands-on analysis of application, web server, and identity provider logs, you’ll learn how to detect anomalies, trace malicious activity, and identify indicators of compromise in real-world scenarios. By the end, you’ll understand not only how these mechanisms work, but also how to investigate session hijacking and JWT forgery incidents effectively.

## Introduction

In this room, we'll dive into different session types and how to investigate several log types at the application level to identify compromise.

## **Learning Prerequisites**

Before diving into this room, it is recommended to complete the rooms below for better comprehension:

* [JWT Security](https://tryhackme.com/room/jwtsecurity)
    
* [Session Management](https://tryhackme.com/room/sessionmanagement)
    

## **Learning Objectives**

* Understand log types to perform application-level forensics
    
* Identify key behaviours that lead to session compromise
    
* Build alerting and detection capabilities tailored to application-level logs
    

## Recap: Session & JWT

## **Recap**

Before diving in, let's quickly recap the concepts we'll explore in this room: Sessions and tokens.

**Authentication & Authorisation**

Authentication is the process of verifying your identity (are you J. Doe?). In contrast, authorisation determines what actions an identity can perform in a given resource (what can J. Doe do?).

**Sessions**

When you authenticate in a web app, there is a high probability that a session was created. Servers use sessions to remember user information, which gets sent through multiple requests. When you log in, the server stores your User ID, role, and other useful information to keep track of your session. This is typically stored centrally in a database. They are stateful, in the sense that the server keeps track of the user information whilst the user interacts with the site. To maintain the user authenticated, the server sends a Session ID to the browser (the client), which gets stored as a cookie. This cookie gets sent back to the server with each request. This way, the server can tell who is sending the request. 

**Advantages**

* **More Control**: Since the server stores and manages the session, it is easier to control the user's data and revoke permissions when needed.
    
* **Tight Security by Design:** Since no session data (apart from the Session ID) is stored on the client side, it's harder for an attacker to modify the session, especially if implemented with HTTPS and the correct cookie flags (`Secure`, `HttpOnly`, `SameSite`).
    
* **No Exposure of Data:** Only the Session ID is exposed, so no sensitive information is stored on the client side.
    

**Disadvantages**

* **Scalability:** You need to use a centralised database to manage session data. This can become tricky if you want to scale multiple servers, as it becomes a single point of failure if not appropriately managed.
    
* **Resource Consumption & High Costs:** Sessions are stored on the server side, meaning each user takes up space. Suppose the application grows and has a large number of users. In that case, resource consumption can become expensive and potentially lead to performance issues if it is not managed and scaled to endure user growth.
    
* **Session Hijack:** If an attacker can steal the Session ID, they can impersonate the user, and they will temporarily have the keys to the kingdom. 
    

**Spotting Sessions In The Wild**

Understanding the types of apps you are dealing with can help your investigation. Certain architecture strategies tend to use sessions more than others:

* **Monoliths**: In monolithic architectures, sessions tend to be the preferred method because the architecture implements a tightly coupled frontend-backend relationship. Centralised session management fits this pattern, as the backend server will manage user and session storage to verify session IDs against the user each time a request is made. For example, imagine a user logs in to Stack Overflow and makes requests to post a thread or answer a question, whilst the back end verifies the session ID stored in the database against the user to keep their session active. 
    
* **Expendable Sessions:** Short-lived sessions are your friend in services where you make short bursts of requests, like filling out a form. The back-end can quickly end your session as you click submit. Or, if you are logging into your online banking to check your statement, your period of inactivity is likely to be short, both for security reasons and given that your actions are temporary and straightforward.
    
* **Single-Pane Management:** If your application is small to medium-sized and you don't have a distributed/microservices architecture, sessions are the go-to. This way, you don't have to hand tokens and verify them across services.
    

**JWT and Tokens**

You might have heard of JSON Web Tokens (JWT), which are commonly seen in modern web applications. JWT is an open [standard](https://www.rfc-editor.org/rfc/rfc7519) used for authentication and authorisation. JWT is stateless because session information is stored on the client side and not on the backend. The structure is as follows:

![JWT components](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/61a7523c029d1c004fac97b3-1756732950015.png align="left")

1. **Header**: Dictates the algorithm used for signing, usually RSA.
    
2. **Payload**: Contains information about the user, like role, session ID, or when the token expires.
    
3. **Signature**: Used to check if the token has been tampered with.
    

**Advantages**

* **No Back-End Dependency:** JWT contain all the necessary session information, so the server does not need to store information compared to sessions.
    
* **Security**: JWT can be encrypted and signed with algorithms like RSA.
    
* **Non-repudiability:** Tokens can be signed, making validating the token's authenticity easier.
    
* **Cross-domain Access:** Because the tokens are stored on the client side, it makes it possible to authenticate across different domains.
    
* **Scalable:** The application is no longer dependent on the server, so it can scale easily without stressing about session memory.
    

**Disadvantages**

* **Can't Revoke As Easily:** When you issue a token, you set a value for expiration. That means if a token is compromised, you technically have to wait until it expires. Revocation mechanisms need to be set when designing systems using JWT. 
    
* **Client Security Dependability:** JWT are usually stored in cookies or [local storage](https://www.freecodecamp.org/news/web-storage-localstorage-vs-sessionstorage-in-javascript/). If the client or website is not built securely, the JWT can be stolen, for example, via XSS.
    
* **Encryption:** JWT's payload is not encrypted by default, although you can implement it. HTTPS has to be present so the token is not intercepted.
    

**Spotting JWTs in the wild**

Similar to the previous section, we aim to understand where you usually see JWTs so you can nourish your investigation. Common architectural patterns are:

* **Distributed Systems:** If you have an application running on microservices, where different services are interconnected, JWTs are your best friend. Each service can validate a JWT instead of constantly talking to the back end to verify the session. You need scalability in distributed systems, and JWTs ensure you can scale autonomously by having all the session information needed on the payload. Imagine an online food delivery service. You have a service for authenticating the user, the basket, the checkout service, etc. Each of these services can verify your session by validating your JWT.
    
* **Single-page applications (SPAs):** You might have encountered [SPAs](https://www.geeksforgeeks.org/javascript/what-is-single-page-application/) multiple times (Reddit, email app, etc.). As the name suggests, SPAs are single pages that load content dynamically without refreshing the page or travelling to another subdomain. As stated in the linked article, "Imagine a Single Page Application (SPA) to be like a magical book. In a traditional book, you flip pages to move to different chapters. However, in the magical SPA book, every chapter appears instantly with a wave of your hand, without having to flip pages." As you can imagine, SPAs are extremely client-heavy, so JWTs are the preferred choice. SPAs are mainly seen on sites that use newer frameworks like [React](https://kinsta.com/blog/angular-vs-react/) or [Angular](https://kinsta.com/blog/angular-vs-react/).
    

### Answer the questions below

What security mechanism do you have to implement when introducing JWT? `revocation`

What is the attack called when an attacker steals your session ID? `session hijacking`

## Decoding and Inspecting Tokens

## **Inspection Time**

By now, you should be familiar with session tokens, JWTs, and the ecosystems they inhabit. Let's dissect them to better understand how to inspect them and where to find them.

**Session Tokens**

In session-based web applications, a unique ID (a cookie) is issued to the client, and the actual session data is stored on the server.

Let's inspect a sample session cookie. Usually, they look like this:

`sessionid=abc123def456ghi789jkl012mno345pq; Path=/; HttpOnly; Secure`

It tells the browser to include this cookie on request (so the server can verify) via HTTPS (`Secure`) and prevents JavaScript from accessing it (`HttpOnly`).

**Web Server Logs**

In web server logs, like in NGINX or Apache, they will look slightly different:

`[22/Jul/2025:14:12:03 +0000] "GET /admin HTTP/1.1" 200 342 "-" "Mozilla/5.0" "sessionid=abcmewomeow456..."`

All the types of logs seen in this room are always on the application layer (OSI layer 7) and track all HTTP requests made to the server. They might be encoded at layer 6, but don't expect logging at this level. The server can then correlate the sessionid to a specific IP, browser (user-agent), or build a request timeline. This is useful if you want to investigate a [session hijack](https://attack.mitre.org/techniques/T1185/). 

**Application Logs**

Application logs, like Django or Flask, might log the activity like this:

`[INFO] [14:12:03] User login successful. sessionid=abc123def456... assigned to user: FluffyCat`

With these logs, you can match the `sessionid` to a specific user and action in the app. In a forensic investigation, this is useful for mapping access and privilege escalation while building your timeline. 

**JWT Tokens**

![JWT components](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/61a7523c029d1c004fac97b3-1756738578731.png align="left")

Because JWTs are stateless, all information is stored on the client side. Finding localStorage in Firefox example:

In web apps, especially SPAs using React or Angular, JWTs are often stored in `localStorage` (also called `sessionStorage`). While this is not a "log", it is still a valuable forensic artefact. If an attacker gains access to the victim's browser, for example via XSS, they can attempt to extract the token:

`localStorage.getItem("auth_token");`  
`// "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."`

**Web Server Logs**

Similar to session tokens, JWT information can be logged in web server logs, but you will find different information. Not always enabled due to security and privacy reasons, some servers or proxies log HTTP requests, and you will find JWT information in the `Authorization` header:

`[22/Jul/2025:14:15:22 +0000] "GET /profile HTTP/1.1" 200 532 "-" "Mozilla/5.0" "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."`

Here, you can see which token was used to access a resource, along with the IP and user-agent. This can be useful for investigating token reuse, IP switching or unauthorised API use.

**Application Logs**

Depending on the business logic of the app, if the application decodes JWT claims, you might see application logs like: 

`[INFO] [14:15:22] Authenticated user=FluffyCat, role=admin, JWT exp=1721435800`

This helps correlate claims (role or username) inside a token to specific actions in the app. If an attacker attempts to tamper with roles or permissions, this is where you can look.

**Identity Provider Logs**

Finally, when investigating JWTs, Identity Providers (IdPs) are key. JWTs are often issued by an IdP, and these services log issuance events, token content and authentication status. For example:

`[INFO] Token issued for user=FluffyCat, client_id=webapp-prod, exp=1721435800, aud=https://app.example.com`

These logs show the origin of the token, who issued it, to which user, etc. This is your log file location for investigating token forgery, expiry, or theft.

**Decoding a JWT token**

You can use [jwt.io](https://jwt.io/) to decode tokens. Let's take a look at this sample token:

`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.KMUFsIDTnFmyG3nMiGM6H9FNFUROf3wh7SmqJp-QV30`

As discussed in the previous section, JWT consists of 3 base64 encoded sections:

&lt;Header&gt;.&lt;Payload&gt;.&lt;Signature&gt;

If you base64 decode the first two parts, you get something like:

![JWT.io example decoding of JWT token](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/61a7523c029d1c004fac97b3-1754291676353.png align="left")

The `alg` tells the server what algorithm was used to sign the token, so the server knows how to verify it. You also have information about the user requesting it and the role it can assume. You can also find out what the expiration date is and when it was issued, `iat` stands for issued at in a UNIX timestamp format. In your terminal, you can use `date -d @[timestamp]` to translate the time, or an online [converter.](https://www.unixtimestamp.com/)

### Answer the questions below

Where would you find logs useful for investigating privilege escalation? `application logs`

Where would you find logs useful for mapping user-agent and IP addresses? `web server logs`

Which logs would you check if a JWT token has been forged? `Identity Provider logs`

## Session Forensics, Log Investigation

## **Scenario**

An incident was triggered recently at TryFlufMe (TFM). It seems some weird activity was happening in TFM's internal admin portal. SecOps has reached out to you, a seasoned Application Security Engineer, asking to collaborate during the incident investigation as they need advice on where to start. After reviewing endpoint logs, they have hit a wall. You have asked them for application and server logs, browser dumps and anything else they could find. You can find the logs by downloading them from the task files. Alternatively, they can be found in the "Session Forensics Logs" directory in the attackbox. The files provided are:

* `webserver.log` Tracks incoming HTTP requests to the web server. They show a user making standard requests using a legitimate token and continuing browsing behaviour with a forged token (to blend in).
    
* `app.log` Application-level logs from the backend. It shows tokens being validated with the role user. Then, suddenly, a warning: a mismatch with known permissions. Followed by access logs showing high-privilege features being accessed.
    
* `idp.log` Logs from the Identity Provider (IDP). They show every token ever issued to the user with the role user. Upon reviewing, no admin tokens were ever legitimately issued.
    
* `browser_dump.txt` A local forensic browser dump from a suspected infected user device. It shows a token stored in localStorage. However, something doesn't seem right here.
    

You can use [this](https://jwt.io/) tool to decode the tokens. Can you help the SecOps team investigate the incident?

### Answer the questions below

What user-agent can be seen in the logs?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757331212691/7622c807-c65d-4bea-940b-9fd9585919d0.png align="center")

Based on the logs, what kind of tokens are we dealing with?

What is the IdP server that issued the tokens?

Which user has requested the tokens? `JWT`

Which role change triggered the warning?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757331234735/cb51b8f9-fac9-427b-acd2-ab5779c6b9a2.png align="center")

What was the malicious token used?  
  
copied the token on the txt file and decoded using the [base64decode](https://www.base64decode.org/) site

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757331251966/6ca4289e-c4f9-4949-aa42-318b8fe6dae5.png align="center")

What algorithm did the malicious token use?

What was the previous legitimate token?

copied the right token from the 200 request, then decoded using the [base64decode](https://www.base64decode.org/) site

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757331265292/e272cb07-8812-4eed-bb33-31ff7365d7c1.png align="center")

What algorithm did the legitimate token use?

## Investigation Findings, Containment and Remediation

## **Investigation Findings**

The investigation confirms that the user FluffyCat accessed the `/admin` endpoint using a forged JSON Web Token (JWT). Log analysis shows the Identity Provider (`auth.catportal.internal`) only issued tokens to FluffyCat with the role user.

Shortly after, a second token was used with the role admin, which was never issued by the IdP. The backend application accepted this forged token, resulting in unauthorised administrative access.

Given FluffyCat is an internal user, the most likely scenario is that they extracted their original token from browser localStorage, tampered with the payload (modifying "role": "user" to "admin"), and resigned the token, possibly using a guessed secret key, or exploiting misconfigured signature verification (e.g., accepting alg: none or not validating the signature)

In an alternative scenario, XSS (Cross-Site Scripting) could have allowed token theft from another user, but no such evidence was found in this case.

You have concluded that the admin portal was tampered with via a JWT Forgery attack. It's now time to speak with SecOps and some developers about remediation and the next steps.

**Containment**

To reduce the blast radius, it is imperative that you set up a task force collaborating with the developers and engineers owning the service to:

1. Revoke all active sessions for FluffyCat and reset their credentials. This includes invalidating refresh tokens and resetting any long-lived API keys if used.
    
2. Notify the wider security team and document the incident and post-mortem.
    
3. Conduct a full token audit across logs for other signs of forged or replayed tokens if other accounts have also shown anomalies.
    
4. Speak to the product teams and senior management to temporarily disable admin endpoints or enforce stricter authentication while investigating further.
    

Once this is done, you can now proceed to advise on longer-term solutions.

## **Remediation and Hardening**

A few days have gone by since the blast radius was reduced thanks to the containment plan you set up for the teams. Now is the time to speak with the tech leads and product teams in TFM for remediation and hardening based on the lessons learned from the post-mortem:

**Enforce Strong Signature Validation**

All JWTs must be properly validated, including checking the signature using a secure, secret key or public key. Reject all tokens using `alg: none` or mismatched algorithms.

**Implement JWT Issuer Validation**

Confirm that the issuer claim matches your Identity Provider (auth.catportal.internal). In this case it used the same IdP that TFM uses, but it's always good to check.

**Harden Client-Side Storage**

Storing tokens in `localStorage` is not recommended. To protect against XSS, it's better to use secure, `HTTP-only` cookies instead.

**Add Token Reuse Detection**

Use a [token verification (e.g Jitsi)](https://jitsi.support/how-to/authenticate-users-jitsi-meet-jwt-tokens/#:~:text=Authenticate%20users%20to%20Jitsi%20meet%20using%20JWT,setup%2C%20integration%2C%20and%20security%20tips%20for%20smooth) and store it server-side to prevent replay or reuse of tampered tokens. Token verification mechanisms are used to validate the token's signature, expiration time, issuer, and audience, along with any custom claims. This process ensures the token's integrity and authenticity, preventing unauthorised access. 

### Answer the questions below

What can you add to ensure a JWT token is not tampered with? `token verification`

## Conclusion

In this lab, you learned how to decode and inspect JWTs, identify tampered tokens, and trace malicious activity through layered logs using a real-world scenario involving FluffyCat. Forged tokens can bypass weak validation, and role escalation might slip by if proper checks aren’t in place. Thanks to your AppSec knowledge, you saw how linking activity across web, app, and IdP logs can build a timeline and uncover the root cause. Thinking like a forensic analyst during a session hijack or token forgery incident is very powerful, and the collaboration of skills between AppSec and SecOps is a killer combo.

This room highlighted the crucial role of forensic investigation in identifying and responding to session-based and token-based compromises. You practiced decoding and inspecting JWTs, mapping logs across multiple layers, and tracing role escalation through forged tokens. Most importantly, you saw how weak validation and insecure storage can be exploited by attackers, and how implementing proper signature checks, secure token storage, and monitoring can prevent such incidents. By combining forensic analysis with proactive remediation, security teams can strengthen their applications and respond more effectively when session or token abuse occurs.