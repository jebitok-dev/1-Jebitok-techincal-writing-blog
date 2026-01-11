---
title: "Next.js: CVE-2025-29927 (TryHackMe)"
datePublished: Sun Jan 11 2026 19:05:46 GMT+0000 (Coordinated Universal Time)
cuid: cmka3trps000402jp8mljf61k
slug: nextjs-cve-2025-29927-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1768156729094/2de85551-31fe-4891-8370-478731dbd08b.png
tags: nextjs, tryhackme, websecurity, burpsuite, cve-2025-29927

---

## Introduction

In January 2025, security researchers Rachid and Yasser Allam uncovered a critical vulnerability in Next.js that sent shockwaves through the web development community. **CVE-2025-29927**, a middleware authorization bypass flaw, affects one of the most popular React frameworks used by millions of developers worldwide. What makes this vulnerability particularly alarming is its simplicity—a single HTTP header is all an attacker needs to completely bypass authentication and authorization controls.

Next.js, developed by Vercel, has become the go-to framework for building modern web applications. With features like server-side rendering (SSR), static site generation (SSG), and file-based routing, it powers everything from e-commerce platforms and SaaS applications to corporate websites and documentation portals. Companies like Netflix, Uber, Twitch, Hulu, and Nike use Next.js in production. This widespread adoption means CVE-2025-29927 isn't just a theoretical concern—it's a vulnerability affecting a massive attack surface across the internet.

The vulnerability exists in the middleware layer, which in Next.js acts as a gatekeeper for incoming requests. Developers commonly use middleware to implement authentication checks, authorization rules, rate limiting, and security controls. The flaw allows attackers to bypass these middleware-based protections entirely by adding a single header: `x-middleware-subrequest: middleware`. This header tricks Next.js into treating the request as an internal subrequest, causing it to skip middleware processing and directly access protected routes.

What's particularly concerning is that this vulnerability affects all Next.js versions before the patches (14.2.25, 15.2.3, and earlier versions in the 13.x and 12.x branches). Given that many applications don't immediately update their dependencies, thousands of vulnerable applications likely remain exposed in the wild.

In this TryHackMe room, I explored how to exploit CVE-2025-29927 using both command-line tools (curl) and GUI-based approaches (Burp Suite). More importantly, I learned how to detect this vulnerability being exploited through log analysis, network traffic monitoring with Snort IDS, and behavioral detection with Zeek. Understanding both the offensive and defensive sides of this vulnerability is crucial for anyone involved in web application security, whether you're a penetration tester, security analyst, or developer building Next.js applications.

The ease of exploitation—literally adding one HTTP header to bypass all security controls—makes this vulnerability a wake-up call about the dangers of trusting framework-level security implementations without proper validation. Let's dive into how this attack works, how to exploit it, and most critically, how to detect and prevent it.

## Exploitation

A proof of concept (PoC) [exploitable app](https://github.com/aydinnyunus/CVE-2025-29927) and exploit code are published by Yunus Aydin on GitHub. We have adapted the app and hosted it on the attached VM. You can view the sample web application by visiting `http://MACHINE_IP:3000` on the AttackBox. However, if you try to access the protected route at `http://MACHINE_IP:3000/protected`, you will be redirected to the home page.

## **Curl**

Exploiting the CVE-2025-29927 vulnerability is quite simple; all the attacker needs to do is add the extra HTTP header `x-middleware-subrequest: middleware` in their request. As explained in the [original post](https://zhero-web-sec.github.io/research-and-things/nextjs-and-the-corrupt-middleware) disclosing this vulnerability, the addition of the `x-middleware-subrequest` header leads to the request getting forwarded to its destination without the middleware manipulating it. Consequently, one does not need more than using `curl` with the proper header argument to access protected routes, i.e., pages.

Exploiting this vulnerability allows us to access the protected page. One simple way is to issue the command below in the terminal on the AttackBox.

`curl -H "x-middleware-subrequest: middleware" http://MACHINE_IP:3000/protected`

The command is like a usual `curl` command with one exception: It uses the `-H`, an equivalent of `--header`, to add an extra header to the HTTP GET request. Consequently, the above `curl` command allows the attacker to bypass all security controls and retrieve the protected page.

## **Burp Suite**

If you prefer a graphical user interface, it is equally easy to exploit this vulnerability using Burp Suite. One easy way to do it is by using Burp Suite’s browser and modifying the requests as you access `http://MACHINE_IP:3000/protected`.

If you are not familiar with Burp Suite, you can follow the following steps after starting Burp Suite:

1. Navigate to the **Proxy** tab and ensure that **Intercept** is on. This will ensure that Burp Suite intercepts all HTTP requests and gives you a chance to modify them before they are sent to the web server.
    
2. Click the **Open browser** button and navigate to `http://MACHINE_IP:3000/protected`.
    
3. The HTTP request is intercepted; it remains frozen till you click the **Forward** button.
    

![Locate the three buttons to turn on Intercept, open the browser, and forward the HTTP request.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1742904136259.png align="center")

Before pressing the **Forward** button, you must add `x-middleware-subrequest: middleware` to your HTTP headers. The image below shows an example of a manipulated header. You can see the results in the browser.

![Manipulating the HTTP headers to exploit the vulnerability.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1742904159886.png align="center")

### Answer the questions below

1. Using `curl` to bypass authorisation, what is the flag on the protected page?  
      
    `curl -H "x-middleware-subrequest: middleware" http://MACHINE_IP:3000/protected`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768156547388/c7bacd56-6288-4f96-8e8b-e785e7d85d82.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768156562056/c290baeb-b645-4b06-bfe0-5f50b1bfee90.png align="center")
    
      
    
2. Try using Burp Suite to exploit the vulnerability and view the protected page.  
      
    Open BurpSuite, then allow the browser on Proxy settings, make sure that `intercept is on`. Open the browser and navigate to `http://IP_Address:3000/protected`. Go back to Burp Suite on the Intercept, then add `x-middleware-subrequest: middleware` and click `Forward` the button. Switch back to the browser, and you’ll find the flag.  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768156616118/038e7057-c5a1-4a40-83d9-75b21e094596.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768156629348/58c313e0-71bd-48d2-ad72-b336c26a2638.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768156643589/d059b894-6233-42ed-b75a-9b24afea9ee2.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768156661762/e2071d33-8185-4eb9-b3c9-af96b1405fbc.png align="center")
    
      
    

## Detection

Recall from the previous tasks that CVE-2025-29927 for Next.js is a middleware authorisation bypass, resulting in the ability to access pages and routes previously requiring such authorisation.

This task will cover some techniques and rules that can be used to detect this attack taking place, both via logs and network traffic.

## **Manual**

Web server logs can potentially be used to discover evidence of this exploit taking place. However, it will depend on whether or not the web server is configured to record HTTP headers.  For instance, NodeJS allows for the logging of this specific HTTP header via `request.headers['x-middleware-subrequest']`

If the web application is proxied, the logging configuration on web servers such as Nginx or Apache2 will need to be modified to log this specific header. For example, **LogFormat** within Apache2 can be used:

`LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" \"%{x-middleware-subrequest}i\"" custom`

Once logging of this HTTP header is correctly setup, common tooling such as Grep, Yara, etc, can be used.

## **Snort (v2)**

The following Snort rule, when used as an IDS, can be used to detect CVE-2025-29927 taking place:

```bash
alert tcp any any -> any any (msg: "HTTP 'x-middleware-request' header detected, possible CVE-2025-29927 explotation"; content:"x-middleware-subrequest";  rawbytes; sid:10000001; rev:1)
```

This rule inspects the packet without factoring or considering any protocol, for example, the `http_headers` module. This is because the HTTP header "x-middleware-request" is not a [recognised header](https://docs.snort.org/rules/options/payload/http/header) within Snort at the time of writing.

First, we will add the Snort rule to our local rules. By default, on Ubuntu, this is located at `/etc/snort/rules/local.rules`. Now we will paste the code snippet above and save. Please note, you will need to change the `sid` value to another if you have existing rules.

Editing Snort's local.rules

```bash
ubuntu@tryhackme-2404:~$ sudo nano /etc/snort/rules/local.rules
# $Id: local.rules,v 1.11 2004/07/23 20:14:44 bmc Exp $
# ----------------
# LOCAL RULES
# ----------------
# This file intentionally does not come with signatures.  Put your local
# additions here.
alert tcp any any -> any any (msg: "HTTP 'x-middleware-request' header detected"; content:"x-middleware-subrequest";  rawbytes; sid:10000001; rev:1)
```

Now, we can run Snort and test for detection. The following terminal below runs Snort in console mode for demonstration of the alert triggering.

Snort detecting CVE-2025-29927

```bash
ubuntu@tryhackme-2404:/var/log/snort$ sudo snort -q -l /var/log/snort -i ens5 -A console -c /etc/snort/snort.conf
03/24-20:16:13.424299  [**] [1:10000001:1] HTTP 'x-middleware-request' header detected [**] [Priority: 0] {TCP} 10.10.142.69:49432 -> 10.10.219.251:3000
```

## **Zeek**

Zeek offers a more comprehensive opportunity for threat detection within network traffic. For CVE-2025-29927, the following Zeek rule can be used:

```bash
module CVE_2025_29927;

export {
    redef enum Log::ID += { LOG };
    global log_policy: Log::PolicyHook = Log::IGNORE;

    event http_header(c: connection, is_orig: bool, name: string, value: string) {
        if (name == "x-middleware-subrequest" && value == "middleware")
            Log::write(HTTP::LOG, [
                $timestamp=c$start_time,
                $uid=c$uid,
                $id=c$id,
                $note="CVE_2025_29927_Exploit",
                $msg="Detected HTTP header associated with CVE-2025-29927",
                $header=name,
                $value=value
           ]);
        notice_info(c, "CVE-2025-29927 Exploit", fmt("The HTTP header '%s' associated with CVE-2025-29927 was detected", value));
    }
  }
}
```

Ensure that this file is saved with the `.zeek` extension in the configured directory for Zeek scripts. You will need to modify your `local.zeek` to include this script via adding `@load ./cve_2025_29927.zeek`.

Finally, restart Zeek to apply the configuration changes via `sudo zeekctl deploy`. If successful, Zeek will now alert when CVE-2025-29927 is detected:

```bash
[Connnection_ID] The HTTP header "x-middleware-subrequest" associated with CVE-2025-29927 was detected
```

## Conclusion

In this room, we covered why this vulnerability exists and how to exploit it. Checking the original posts about the discovery of this vulnerability provides an important reminder about the many vulnerabilities that still lurk in the source code of many popular applications. For patched versions, users are required to upgrade to the following:

* Next.js 15.x should upgrade to 15.2.3
    
* Next.js 14.x should upgrade to 14.2.25
    
* Next.js 13.x should upgrade to 13.5.9
    
* Next.js 12.x should upgrade to 12.3.5
    

If patching is infeasible, the only workaround is to block HTTP requests containing the `x-middleware-subrequest` from reaching your web application.  
  
  
Completing this Next.js CVE-2025-29927 room was both enlightening and sobering. The vulnerability demonstrates a fundamental principle in application security: **security controls are only as strong as their weakest implementation layer**. When developers rely on middleware for authorization without understanding how that middleware can be bypassed, they create a false sense of security that attackers can easily exploit.

### The Exploitation Reality

The simplicity of this exploit is what makes it so dangerous. Using curl with a single additional header:

bash

```bash
curl -H "x-middleware-subrequest: middleware" http://target:3000/protected
```

Or using Burp Suite to intercept and modify a request by adding:

```bash
x-middleware-subrequest: middleware
```

That's all it takes. No complex exploitation chain, no memory corruption, no SQL injection—just one header field that tells Next.js to skip middleware processing. I was able to access supposedly "protected" routes and retrieve sensitive information without any valid credentials.

This attack technique is particularly insidious because:

1. **It's framework-specific** - Traditional web application firewalls (WAFs) might not catch this unless specifically configured
    
2. **It looks like legitimate traffic** - The HTTP request structure is valid and might not trigger anomaly detection
    
3. **It requires no authentication** - Attackers don't need stolen credentials or session tokens
    
4. **It bypasses ALL middleware** - Not just one security check, but every protection implemented in middleware
    
5. **It's easily automated** - Attackers can scan for vulnerable Next.js applications at scale
    

### Detection: The Critical Defense Layer

What struck me most about this room was the emphasis on detection mechanisms. Since many organizations can't immediately patch all their applications, detection becomes the critical compensating control.

**Manual Log Analysis:**

The challenge with detecting CVE-2025-29927 through logs is that web servers don't typically log custom HTTP headers by default. This requires proactive configuration:

For Node.js applications:

javascript

```javascript
request.headers['x-middleware-subrequest']
```

For Apache with reverse proxy:

apache

```apache
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" \"%{x-middleware-subrequest}i\"" custom
```

Once logging is configured, defenders can use grep, awk, or SIEM queries to search for this header:

bash

```bash
grep "x-middleware-subrequest" /var/log/apache2/access.log
```

**Snort IDS Detection:**

The Snort rule provided in the room demonstrates network-based detection:

snort

```bash
alert tcp any any -> any any (msg: "HTTP 'x-middleware-request' header detected, possible CVE-2025-29927 exploitation"; content:"x-middleware-subrequest"; rawbytes; sid:10000001; rev:1)
```

This rule inspects raw packet content for the malicious header without relying on HTTP protocol parsing. When I tested this in the lab environment, Snort successfully triggered alerts:

```bash
03/24-20:16:13.424299 [**] [1:10000001:1] HTTP 'x-middleware-request' header detected [**] [Priority: 0] {TCP} 10.10.142.69:49432 -> 10.10.219.251:3000
```

This provides real-time detection capability at the network perimeter, allowing security teams to identify exploitation attempts immediately.

**Zeek Behavioral Detection:**

The Zeek script takes detection a step further by parsing HTTP headers and logging exploitation attempts:

zeek

```bash
event http_header(c: connection, is_orig: bool, name: string, value: string) {
    if (name == "x-middleware-subrequest" && value == "middleware")
        Log::write(HTTP::LOG, [
            $note="CVE_2025_29927_Exploit",
            $msg="Detected HTTP header associated with CVE-2025-29927"
        ]);
}
```

Zeek provides richer context than Snort, including connection metadata, timing information, and the ability to correlate this activity with other suspicious behaviors. This makes it ideal for threat hunting and incident response investigations.

### Why This Vulnerability Matters

CVE-2025-29927 is more than just another CVE number—it represents a class of vulnerabilities that emerge when frameworks add abstraction layers that developers don't fully understand. Many Next.js developers likely assumed that implementing authorization in middleware was sufficient protection. The framework documentation might have suggested this pattern. But the underlying implementation had a flaw that allowed bypass through an internal-use header being accessible externally.

This highlights several critical lessons:

**1\. Framework Trust vs. Framework Understanding**

Developers often trust frameworks to handle security correctly without understanding the implementation details. When Next.js processes the `x-middleware-subrequest` header (intended for internal use), it assumes the request is coming from a trusted internal component. There's no validation that the header actually came from the framework itself rather than an external attacker.

**2\. Defense in Depth Still Applies**

Relying solely on middleware for authorization is a single point of failure. Applications should implement:

* Middleware-level checks (first line of defense)
    
* Route-level authorization (second line of defense)
    
* Data-level access controls (third line of defense)
    

Even if middleware is bypassed, route handlers should still validate user permissions before returning sensitive data.

**3\. Input Validation Extends to Headers**

Most developers focus on validating request bodies, query parameters, and URL paths. But HTTP headers are also user-controlled input. The `x-middleware-subrequest` header should have been:

* Stripped from external requests at the framework boundary
    
* Validated as originating from internal framework code
    
* Never trusted if present in external requests
    

**4\. Security Requires Proactive Monitoring**

The detection techniques covered in this room emphasize that security isn't just about prevention—it's about visibility. Organizations need to:

* Log security-relevant headers and events
    
* Implement network-based detection (IDS/IPS)
    
* Use behavioral analysis tools (Zeek, SIEM)
    
* Regularly review and tune detection rules
    
* Correlate multiple signals for comprehensive threat detection
    

### Remediation Strategy

For organizations running Next.js applications, the remediation strategy is clear but challenging:

**Immediate Actions:**

1. **Identify all Next.js applications** in your environment (inventory)
    
2. **Check versions** - anything before the patched versions is vulnerable:
    
    * Next.js 15.x → upgrade to 15.2.3+
        
    * Next.js 14.x → upgrade to 14.2.25+
        
    * Next.js 13.x → upgrade to 13.5.9+
        
    * Next.js 12.x → upgrade to 12.3.5+
        
3. **If immediate patching isn't possible**, implement workarounds:
    
    * Block requests containing `x-middleware-subrequest` header at reverse proxy/WAF level
        
    * Add additional authorization checks in route handlers (defense in depth)
        
    * Implement network-based detection (Snort/Zeek rules)
        
    * Enable enhanced logging to monitor for exploitation attempts
        

**Long-term Actions:** 4. **Review authorization architecture** - Don't rely solely on middleware 5. **Implement comprehensive logging** - Ensure custom headers are logged 6. **Deploy detection mechanisms** - IDS rules, log monitoring, SIEM alerts 7. **Test security controls** - Verify that authorization can't be bypassed 8. **Establish patch management process** - Ensure dependencies are regularly updated

### The Broader Implications

This vulnerability is part of a concerning trend in modern web development. As frameworks become more complex and add more abstraction layers, they introduce new classes of vulnerabilities:

* **Express.js**: Has had middleware bypass issues
    
* **Django**: URL routing bypasses
    
* **Rails**: Parameter parsing vulnerabilities
    
* **Spring Boot**: Path traversal issues
    

The pattern is similar: framework-level security controls that can be bypassed through unexpected inputs or edge cases in the framework's logic.

### Personal Takeaways

Working through this room reinforced several important concepts for my cybersecurity journey:

**For Offensive Security (Pentesting):**

* Always test authorization controls with modified headers
    
* Look for framework-specific bypass techniques in CVE databases
    
* Check for internal-use headers that might be accessible externally
    
* Don't assume that "protected" routes are actually protected
    
* Simple exploits are often the most devastating
    

**For Defensive Security (Blue Team):**

* Default logging configurations are often insufficient
    
* Network-based detection provides critical visibility
    
* Multiple detection layers (logs + IDS + behavioral) provide defense in depth
    
* Keeping frameworks updated is critical, not optional
    
* Security monitoring must evolve with new attack techniques
    

**For Secure Development:**

* Don't trust framework security controls blindly
    
* Implement authorization at multiple layers
    
* Validate ALL user inputs, including headers
    
* Strip or validate internal-use headers at application boundaries
    
* Test security controls with adversarial thinking
    

### Connection to Real-World Impact

While this was a lab environment, CVE-2025-29927 has real-world implications. Consider these scenarios:

**E-commerce Platform:**

* Bypass payment validation middleware
    
* Access admin-only discount codes
    
* View other customers' orders and personal information
    
* Modify prices or inventory levels
    

**SaaS Application:**

* Access premium features without subscription
    
* View other tenants' data in multi-tenant systems
    
* Bypass API rate limiting
    
* Access administrative dashboards
    

**Corporate Internal Apps:**

* Access HR systems with salary information
    
* View confidential business documents
    
* Bypass approval workflows
    
* Access financial reporting systems
    

Any Next.js application using middleware for authorization was potentially vulnerable until patched.

### Final Thoughts

CVE-2025-29927 serves as a powerful reminder that **security is never finished**. Even mature, widely-used frameworks maintained by reputable organizations can have critical vulnerabilities. The discovery by Rachid and Yasser Allam highlights the importance of security research and responsible disclosure.

For developers: Don't assume the framework handles security correctly. Implement defense in depth, validate inputs (including headers), and stay current with security updates.

For security professionals: Understand the frameworks your organization uses, implement comprehensive detection capabilities, and maintain an inventory of applications so you can respond quickly when vulnerabilities are disclosed.

For organizations: Establish robust patch management processes, implement security monitoring at multiple layers, and ensure your security team has visibility into your application stack.

The simplicity of this exploit—one HTTP header—demonstrates that devastating vulnerabilities don't always require sophisticated techniques. Sometimes, the most critical flaws are hiding in plain sight, waiting for someone to notice that an internal mechanism is accessible from the outside.

This room taught me not just how to exploit a specific CVE, but how to think about framework-level security, implement effective detection mechanisms, and understand the relationship between offensive and defensive security. The best defense against vulnerabilities like CVE-2025-29927 isn't just patching—it's understanding how attacks work, implementing detection capabilities, and building systems with security designed in from the start.

---

### Key Takeaways Summary:

✅ **Exploitation**: Single header `x-middleware-subrequest: middleware` bypasses all middleware  
✅ **Impact**: Complete authorization bypass on vulnerable Next.js applications  
✅ **Detection**: Requires custom logging configuration + network monitoring (Snort/Zeek)  
✅ **Remediation**: Patch to latest versions or block the header at reverse proxy  
✅ **Prevention**: Defense in depth, don't rely solely on middleware for authorization  
✅ **Lesson**: Framework-level security controls need validation and shouldn't be trusted blindly