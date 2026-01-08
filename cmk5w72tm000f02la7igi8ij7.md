---
title: "OWASP Top 10 2025: IAAA Failures"
datePublished: Thu Jan 08 2026 20:21:05 GMT+0000 (Coordinated Universal Time)
cuid: cmk5w72tm000f02la7igi8ij7
slug: owasp-top-10-2025-iaaa-failures
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767901227783/cc36e0be-e9da-4676-b3ce-6c37f5f54617.png
tags: tryhackme, websecurity, owasp-top-10, iaaa, owasp-top-10-2025

---

The OWASP Top 10 has long been the gold standard for understanding web application security risks, and the 2025 edition continues to highlight critical vulnerabilities that plague modern applications. In this TryHackMe room, I explored three interconnected categories that all stem from failures in how Identity, Authentication, Authorization, and Accountability (IAAA) are implemented: **A01: Broken Access Control**, **A07: Authentication Failures**, and **A09: Logging & Alerting Failures**.

What makes these vulnerabilities particularly dangerous is that they're not just theoretical concerns—they're actively exploited in the wild, leading to massive data breaches, unauthorized access, and complete system compromises. From the 2023 MOVEit breach that exposed data from hundreds of organizations to countless cases of credential stuffing and privilege escalation attacks, IAAA failures consistently rank among the most damaging security issues organizations face.

IAAA represents the four pillars of secure access management. Think of it as a security checkpoint system: first, you **identify** who's trying to access the system (username, email, or user ID). Then, you **authenticate** that they are who they claim to be (password, biometrics, or multi-factor authentication). Next, you **authorize** what they're allowed to do based on their role or permissions. Finally, you maintain **accountability** by logging and monitoring their actions—tracking login times, resources accessed, and security events. This is where SIEM (Security Information and Event Management) tools become crucial, collecting and analyzing logs to detect threats and maintain audit trails for investigations.

When any of these four pillars fail, the entire security model collapses. A broken access control mechanism might let me view your bank account just by changing a URL parameter. An authentication failure could allow an attacker to register as "aDmiN" when "admin" already exists. And logging failures? They leave defenders blind, unable to detect ongoing attacks or investigate breaches after the fact.

In this room, I got hands-on experience exploiting these vulnerabilities and, more importantly, understanding *why* they occur and *how* to prevent them. Let's dive into the breakdown of each OWASP category and the critical security lessons they teach us.

## Introduction

![OWASP logo](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/585aef878659bfbed7d6150703c6ba9d.png align="left")

  

This room breaks down 3 of the OWASP Top 10 2025 categories. In this room, you will learn about the categories that are related to failures in how Identity, Authentication, Authorisation, and Accountability (IAAA) is implemented in the application. You will put the theory into practice by completing supporting challenges. The following categories are covered in this room:

1. A01: Broken Access Control
    
2. A07: Authentication Failures
    
3. A09: Logging & Alerting Failures
    

The room has been designed for beginners and assumes no previous security knowledge.

## What is IAAA?

IAAA is a simple way to think about how users and their actions are verified on applications. Each item plays a crucial role and it isn't possible to skip a level. That means, if a previous item isn't being performed, you cannot perform the later times. The four items are:

* **Identity** - the unique account (e.g., user ID/email) that represents a person or service.
    
* **Authentication** - proving that identity (passwords, OTP, passkeys).
    
* **Authorisation** - what that identity is allowed to do.
    
* **Accountability** - recording and alerting on who did what, when, and from where.
    

The three categories of OWASP Top 10:2025 discussed in this room relates to failures in how IAAA was implemented. Weaknesses here can be incredibly detrimental, as it can allow threat actors to either access the data of other users or gain more privileges than they are suppose to have.

If you want a deeper dive on IAAA first, work through [this room.](https://tryhackme.com/room/iaaaidm)

### Answer the questions below

1. What does IAAA stand for? `Identity, Authentication, Authorisation, Accountability`
    

## A01: Broken Access Control

Broken Access Control happens when the server doesn’t properly enforce **who can access what** on every request. A common occurence of this is **IDOR** (Insecure Direct Object Reference): if changing an ID (like `?id=7 → ?id=6`) lets you see or edit someone else’s data, access control is broken.

In practice this shows up as horizontal privilege escalation (same role, other user’s stuff) or vertical privilege escalation (jumping to admin-only actions) because the application trusts the client too much.

Start the static site attached to this task and play with the `accountID` value in the URL. So if you can identify which user has more than $ 1 million in their account!

If you want a deeper dive or more variations on the theme (encoded IDs, hashed IDs, etc.), work through these rooms afterwards:

* [**Broken Access Control**](https://tryhackme.com/room/owaspbrokenaccesscontrol)
    
* [**Insecure Direct Object References**](https://tryhackme.com/room/idor)
    

### Answer the questions below

1. If you don't get access to more roles but can view the data of another users, what type of privilege escalation is this? `Horizontal`
    
2. What is the note you found when viewing the user's account who had more than $ 1 million? `THM{Found.the.Millionare!}`
    

## A07: Authentication Failures

Authentication Failures happen when an application can’t reliably verify or bind a user’s identity. Common issues include:

* username enumeration
    
* weak/guessable passwords (no lockout/rate limits)
    
* logic flaws in the login/registration flow
    
* insecure session or cookie handling
    

If any of these are present, an attacker can often log in as someone else or bind a session to the wrong account.

Let's try to break into the `admin` user's account. We know that their username is `admin`, so let's try to fool the application by registering a user with the name of `aDmiN`. Start the static site attached to this task. register your account and log into the admin user's account to get your next flag!

If you want more depth or broader techniques (e.g., brute force, session handling, cookies/JWT/OAuth, and MFA specifics), work through these after this room:

* [**Authentication Bypass Room**](https://tryhackme.com/room/authenticationbypass)
    
* [**Multi-Factor Authentication**](https://tryhackme.com/room/multifactorauthentications)
    
* [**Authentication Module**](https://tryhackme.com/module/authentication)
    

### Answer the questions below

1. What is the flag on the `admin` user's dashboard? `THM{Account.confusion.FTW!}`  
    
    * When you try to log in as `admin` you’ll get something like an incorrect password
        
    * When you try to register a new user account using `admin` you’ll get an error message that the user already exists
        
    * When you create a new user, like `aDmin` and log in to the account, you’ll get the flag.  
          
        It’s interesting how input validation is important during the authentication process to ensure that, in such a case, `aDmin` can also be marked as an existing user.  
        
    * Always test case variations of usernames
        
    * Check for inconsistencies between registration/login
        
    * Look for reserved name bypasses  
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767901459687/d4c05dcb-a25a-4142-a6e5-238b0c28e5ba.png align="center")
    

## A09: Logging & Alerting Failures

When applications don’t record or alert on security-relevant events, defenders can’t detect or investigate attacks. Good logging underpins **accountability** (being able to prove who did what, when, and from where). In practice, failures look like missing authentication events, vague error logs, no alerting on brute-force or privilege changes, short retention, or logs stored where attackers can tamper with them.

Let's take a look at what is required to perform an investigation of a application under attack. Start the static site attached to this task, perform your investigation, and answer the questions below. Then, think about how hard it would be to understand what happened during this attack if key pieces of this log information as missing. 

If you want a deeper dive into logging for accountability, take a look at [this room](https://tryhackme.com/room/loggingforaccountability).

### Answer the questions below

1. It looks like an attacker tried to perform a brute-force attack. What is the IP of the attacker? `203.0.113.45`  
      
    First, IAAA is an important concept in cybersecurity. When you think of it, you’ve to identify a user, based on their credentials like usernames, userID, or emails, etc, then they’ll have to go through the authentication process e.g using their password, biometrics or multifactor-authentication (MFA), then we’ll have to authorize our users based on their roles or permissions, determining what resources they can access and what actions they can perform then finally we’ve to account for user activities by logging and monitoring what they do i.e tracking login times, actions performed, resources accessed and any security events, this is where SIEM tools come in, collecting and analyzing logs to detect threats and analyze logs to detect threats and maintain audit trails  
      
    In this case, we’ll identify the attacker based on the IP addresses that had failed login attempts
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767901408794/90427767-fead-4ae6-a21e-2f1c9dd52ed0.png align="center")
    
2. Looks like they were able to gain access to an account! What is the username associated with that account? `admin`  
    
    * Checking all logs linked to the above IP, we can see the username and password.
        
    * I believe it’s not ideal for logging systems to reveal the credentials in plain text or at all, as it’s likely to be attributed to **sensitive data exposure in logs**  
        
3. What action did the attacker try to do with the account? List the endpoint the accessed. `/supersecretadminstuff`
    
    * Based on the logs, once the attacker logged into the admin account, they navigated to the path that has supersecretadminsuff
        

## Conclusion

You’ve just worked through the essentials of **Identity, Authentication, Authorisation, and Accountability** in web applications and how it can cause several of the categories of vulnerabilities discussed in OWASP Top 10:2025. The big ideas to keep:

* **A01 Broken Access Control:** Enforce server-side checks on **every** request
    
* **A07 Authentication Failures:** Enforce unique indexes on the canonical form, rate-limit/lock out brute force, and rotate sessions on password/privilege changes.
    
* **A09 Logging & Alerting Failures:** Log the full auth lifecycle (fail/success, password/2FA/role changes, admin actions), centralise logs off-host with retention, and alert on anomalies (e.g., brute-force bursts, privilege elevation).
    

Continue the journey with **Room 2 in this module:** [**Application Design Flaws**](https://tryhackme.com/jr/owasptopten2025two).

Completing this OWASP Top 10 2025 IAAA room was an eye-opening experience that reinforced just how interconnected these security failures really are. The three categories covered—Broken Access Control, Authentication Failures, and Logging & Alerting Failures—aren't isolated vulnerabilities but rather different manifestations of the same fundamental problem: **poor implementation of the IAAA framework**.

### Key Takeaways

**A01: Broken Access Control** taught me **never trust the client**. The server must enforce authorization checks on every single request, regardless of what the UI suggests users can or cannot do. Changing `?accountID=7` to `?accountID=6` shouldn't give me access to someone else's data, yet this horizontal privilege escalation is shockingly common. The solution? Server-side validation, proper access control lists (ACLs), and role-based access control (RBAC) that's enforced at the backend—not just the frontend.

**A07: Authentication Failures** revealed how subtle logic flaws can completely undermine security. The case sensitivity bypass I exploited (registering "aDmiN" when "admin" existed) demonstrates why **input validation and normalization are non-negotiable**. Developers must normalize usernames to a consistent format (typically lowercase) at registration, check for duplicates case-insensitively, and apply the same normalization at login. Additionally, implementing rate limiting, account lockouts after failed attempts, and proper session management are essential defenses against brute-force attacks and credential stuffing.

**A09: Logging & Alerting Failures** showed me the critical importance of **accountability in security**. Without comprehensive logging, defenders are flying blind. During the investigation challenge, I was able to identify the attacker's IP address, trace their brute-force attempts, discover the compromised admin account, and see exactly what endpoint they accessed—all because proper logs existed. However, I also noticed a critical flaw: **passwords were being logged in plaintext**. This is a severe security vulnerability that violates every best practice and could turn logs themselves into a goldmine for attackers.

### Critical Lessons for Development Teams

If you're building web applications, here are the non-negotiable security practices this room reinforced:

#### **For Access Control:**

* ✅ Enforce authorization checks server-side on every request
    
* ✅ Never rely on client-side controls (hidden buttons, disabled fields)
    
* ✅ Use indirect references or UUIDs instead of sequential IDs
    
* ✅ Implement proper RBAC with least privilege principle
    
* ✅ Test for both horizontal and vertical privilege escalation
    

#### **For Authentication:**

* ✅ Normalize all usernames to lowercase before storage and comparison
    
* ✅ Perform case-insensitive duplicate checks during registration
    
* ✅ Implement rate limiting and account lockouts (e.g., 5 failed attempts = 15-minute lockout)
    
* ✅ Use strong password policies and consider passwordless options
    
* ✅ Rotate session tokens after privilege changes or password resets
    
* ✅ Test for username enumeration vulnerabilities
    
* ✅ Consider implementing CAPTCHA after multiple failed attempts
    

#### **For Logging & Accountability:**

* ✅ Log all authentication events (successes AND failures)
    
* ✅ **NEVER log passwords, API keys, or sensitive credentials**—not even hashed versions
    
* ✅ Implement log sanitization to remove sensitive data before writing
    
* ✅ Centralize logs off-host (use SIEM tools like Splunk, ELK Stack, or Azure Sentinel)
    
* ✅ Set up real-time alerts for anomalies (brute-force attempts, privilege escalations, suspicious logins)
    
* ✅ Include contextual information: WHO did WHAT, WHEN, WHERE (IP address), and OUTCOME
    
* ✅ Maintain adequate log retention for compliance and forensics (typically 90+ days)
    
* ✅ Protect logs from tampering with proper access controls
    
* ✅ Test your incident response procedures using your logs
    

#### **Secure Logging Implementation Example:**

python

```python
# ❌ BAD - Never do this!
logger.info(f"Login attempt: username={username}, password={password}")

# ✅ GOOD - Log only what's needed
logger.info(f"Login attempt: username={username}, ip={request.ip}, status=failed")
logger.warning(f"Multiple failed logins: username={username}, count={failed_count}, ip={request.ip}")

# ✅ GOOD - Sanitize before logging
def sanitize_for_logs(data):
    sensitive_fields = ['password', 'token', 'api_key', 'secret']
    return {k: '[REDACTED]' if k in sensitive_fields else v for k, v in data.items()}

logger.info(f"User action: {sanitize_for_logs(request.data)}")
```

### The Bigger Picture

What struck me most about this room is how these vulnerabilities often work together in real attacks. An attacker might exploit broken access control to enumerate valid usernames, use an authentication failure (like case sensitivity bypass) to gain initial access, and then benefit from logging failures that prevent defenders from detecting or responding to the breach in time.

The IAAA framework isn't just a theoretical model—it's a practical blueprint for building secure applications. Every layer matters:

* **Identity** ensures we know who's who
    
* **Authentication** proves they are who they claim to be
    
* **Authorization** limits what they can do
    
* **Accountability** tracks what they actually did
    

Break any one of these pillars, and the entire security model crumbles.

### Connection to Security+ and Career Development

For those studying for Security+ (like I am), this room directly maps to multiple exam domains:

* **Domain 2.0 (Architecture & Design)**: Identity and access management, authentication methods
    
* **Domain 3.0 (Implementation)**: Secure coding practices, access control schemes
    
* **Domain 4.0 (Operations & Incident Response)**: Logging, monitoring, SIEM configuration
    

The hands-on experience of exploiting these vulnerabilities—and more importantly, understanding how to prevent them—is invaluable preparation not just for certification exams, but for real-world security roles.

### Moving Forward

This room is part of a larger OWASP Top 10 2025 series, and I'm excited to continue exploring other vulnerability categories. The skills learned here—thinking like an attacker while understanding defender perspectives—are foundational to any cybersecurity career.

If you're interested in diving deeper into these topics, I highly recommend:

* **Broken Access Control Room** (for IDOR variations with encoded/hashed IDs)
    
* **Authentication Bypass Room** (for advanced auth exploitation techniques)
    
* **Logging for Accountability Room** (for SIEM implementation and log analysis)
    

Remember: **security isn't a feature you add at the end—it's a foundation you build from the start**. Every line of code that handles user identity, validates credentials, enforces permissions, or writes logs is a potential security control or vulnerability depending on how it's implemented.

Stay curious, keep hacking (ethically!), and always ask: "What could go wrong here?"

---

### Final Stats from This Room:

* **Flags Captured**: 3/3 ✅
    
* **Vulnerabilities Exploited**: Horizontal privilege escalation, case sensitivity bypass, log analysis
    
* **Key Skills Gained**: Access control testing, authentication bypass techniques, security log investigation
    
* **Real-World Impact**: Understanding how millions of records get breached through these exact vulnerabilities
    

Thanks for reading! If you found this helpful, check out my other TryHackMe writeups and cybersecurity content on my blog. And if you're building web applications, please—for the love of all that is secure—normalize your usernames and stop logging passwords! 🔒

---

**Additional Resources:**

* [OWASP Top 10 2025 Official Documentation](https://owasp.org/www-project-top-ten/)
    
* [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
    
* [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
    
* [NIST Special Publication 800-63B: Digital Identity Guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html)