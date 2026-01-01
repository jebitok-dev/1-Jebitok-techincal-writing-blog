---
title: "n8n: CVE-2025-68613 (TryHackMe)"
datePublished: Thu Jan 01 2026 14:51:44 GMT+0000 (Coordinated Universal Time)
cuid: cmjvkckmj000902jlcmdcganp
slug: n8n-cve-2025-68613-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767277376810/2dd529b2-83d0-4e3b-a610-f4273f14e287.png
tags: tryhackme, n8n, ctf-writeup, cve-2025-68613

---

Low-code and automation platforms increasingly rely on dynamic expression evaluation to provide flexibility and power to their users. However, when these expressions are evaluated in unsafe execution contexts, they can introduce vulnerabilities with consequences comparable to full application compromise.

In this room, we examine **CVE-2025-68613**, a critical Remote Code Execution (RCE) vulnerability in **n8n**, disclosed in December 2025 with a **CVSS score of 9.9**. The flaw exists within n8n’s workflow expression evaluation mechanism, where user-supplied expressions wrapped in `{{ }}` are evaluated as JavaScript without proper sandboxing or isolation.

This vulnerability behaves similarly to recent **React “shell”–style CVEs**, where expression or template evaluation features can be abused to escape their intended scope and execute arbitrary system commands. In both cases, attackers leverage trusted execution contexts to pivot from controlled input to unrestricted access to the underlying runtime.

Because n8n is frequently deployed in internal networks, automation pipelines, and security tooling environments, exploitation of this flaw can allow an authenticated attacker to execute system-level commands with the privileges of the n8n process, potentially leading to data exfiltration, lateral movement, or complete infrastructure compromise.

Throughout this room, we will explore the technical root cause of the vulnerability, demonstrate browser-based exploitation, and discuss practical detection strategies that defenders can use to identify and respond to this attack in real-world environments.

## Introduction

In this room, we will examine [CVE-2025-68613](https://nvd.nist.gov/vuln/detail/CVE-2025-68613)[, a critical v](https://nvd.nist.gov/vuln/detail/CVE-2025-68613)ulnerability in [n8n](https://n8n.io/) [th](https://n8n.io/)at was published on December 19, 2025, with a CVSS score of 9.9.

n8n is an open-source workflow automation pl[atform designe](https://nvd.nist.gov/vuln/detail/CVE-2025-68613)d to visually conne[ct](https://n8n.io/) applications and services for task automation. Users build workflows composed of nodes, with each node representing an action such as making an API request, processing data, or sending an email. n8n is frequently used to automate repetitive operational tasks and to integrate security tools and SaaS platforms. Below is a simple example workflow that allows us to schedule an HTTP GET request to the NVD CVE API, format the output using JavaScript, and then send the report via email and to a Slack channel.

![n8n example workflow](https://tryhackme-images.s3.amazonaws.com/user-uploads/62ff64c3c859dc0042b2b9f6/room-content/62ff64c3c859dc0042b2b9f6-1766583898233.png align="left")

The n8n platform is commonly deployed in thr[ee primary con](https://nvd.nist.gov/vuln/detail/CVE-2025-68613)figurations:

* Sel[f-h](https://n8n.io/)osted instances: Organizations deploy [n8n on-premise](https://nvd.nist.gov/vuln/detail/CVE-2025-68613)s or in private clo[ud](https://n8n.io/) environments for full control and data sovereignty
    
* Cloud-hosted ([n8n.cloud](http://n8n.cloud)): Managed service of[fering with sh](https://nvd.nist.gov/vuln/detail/CVE-2025-68613)ared infrastructure
    
* Internal automation tools: Deployed within c[orporate netwo](https://nvd.nist.gov/vuln/detail/CVE-2025-68613)rks to automate bus[ine](https://n8n.io/)ss processes between internal and external systems
    

Versions 0.211.0 through 1.120.3 contain a c[ritical Remote](https://nvd.nist.gov/vuln/detail/CVE-2025-68613) Code Execution (RC[E)](https://n8n.io/) vulnerability within the workflow expression evaluation system. If exploited, this flaw enables an authenticated attacker to execute system-level commands, potentially leading to data breaches, service disruptions, or full system compromise, all with the privileges assigned to the n8n process.

In this room, we will discuss the technical [aspects of thi](https://nvd.nist.gov/vuln/detail/CVE-2025-68613)s vulnerability, de[mon](https://n8n.io/)strate exploitation via web browser, and explore detection strategies.

This vulnerability has been addressed in ver[sions 1.120.4,](https://nvd.nist.gov/vuln/detail/CVE-2025-68613) 1.121.1, and 1.122[.0.](https://n8n.io/) To ensure system security, it is essential to update to one of these patched versions.

## Technical Background

Before exploring the exploit, let’s review n8n. It is built on Node.js, using JavaScript for platform internals and user workflow logic. Its architecture includes:

* Workflow Execution Engine: The core computational component responsible for orchestrating node-based workflow execution
    
* Expression Evaluation System: Processes dynamic expressions wrapped in double curly braces `{{ }}` that are evaluated as JavaScript code during workflow execution
    
* Code Nodes: Allow users to write custom JavaScript or Python code as workflow steps, extending platform capabilities
    
* 400+ Native Integrations: Pre-built connectors to various APIs and services that form the nodes in workflows
    

The vulnerability resides in n8n’s workflow expression evaluation system, where expressions supplied by authenticated users during workflow configuration are evaluated in an insecure execution context. The core security flaw is an expression injection vulnerability that enables authenticated attackers to execute arbitrary JavaScript code with the privileges of the n8n process. Specifically:

* n8n processes user input wrapped in double curly braces `{{ }}` as JavaScript code without adequate sandboxing or input validation.
    
* The expression evaluator lacks proper context isolation, allowing attackers to escape the intended evaluation sandbox.
    
* Authentication provides no meaningful protection against this vulnerability, as any authenticated user can exploit it.
    

Consider the following working payload from [wioui](https://github.com/wioui/n8n-CVE-2025-68613-exploit).

`{{ (function(){ return this.process.mainModule.require('child_process').execSync('id').toString() })() }}`

Inside of all these layers of curly braces, you can see `(function(){ ... })()`. This pattern creates and immediately executes an anonymous function. The attacker would try to encapsulate some complex logic while maintaining the execution context. For easier reading, the anonymous function is shown below:

```js
function () {
    return this.process.mainModule.require('child_process').execSync('id').toString()
}
```

Let’s take a closer look to better understand this exploit. When `function () { ... }` is called, it starts to execute the `return` statement. If you are not familiar with functions, the `return` statement returns a value, which requires evaluating the expression that comes after it. In this case, evaluation starts with `this`.

The exploit uses `this.process.mainModule`. Let’s break this down:

* `this` refers to the global object in the Node.js execution context
    
* `process` is a Node.js global object providing access to system processes
    
* `mainModule` references the root module of the Node.js application
    

This aims to bypass typical JavaScript sandbox restrictions by accessing Node.js internals (the root module), which should be unavailable to user expressions. It should be noted that if proper sandboxing is in place, it would isolate the expression execution context from the Node.js runtime environment.

Now that the `mainModule` object is reached, we see `.require('child_process')`. This uses `require()`, i.e., Node.js’s module loading function, in order to load `child_process`, a core Node.js module for executing system commands. It should be noted that user expressions should never have access to Node.js’s module system, especially dangerous modules like `child_process`.

Reaching this far, it is a trivial task to execute system functions. This example payload uses `.execSync('id')` to run the `id` command on the host system. Remember that the `id` command displays user identity information (UID, GID, groups).

Now that we have executed `id` on the target system, it is time to retrieve the output. This payload uses `.toString()` to convert the Buffer output from `execSync()` to a readable string, i.e., `id`’s output.

Security boundary breach: User expressions should never have access to Node.js’s module system, especially dangerous modules like child\_process

You can now see why we mentioned that the attacker will encapsulate complex logic within the anonymous function; it involves one call after another, until they are literally running commands on the vulnerable system. To summarize, the context escalation chain went as follows:

* It starts within the expression evaluator’s intended sandbox
    
* Then it escalates to the Node.js global context via `this`
    
* Furthermore, it escalates to module system access via `process.mainModule.require`
    
* Finally, it escalates to system command execution via `child_process`
    

### Answer the questions below

In this exploit, what is the name of the module that allowed us to execute system commands? `child_process`

## Exploitation

You can start the VM by clicking the green Start Machine button below. To attack the target VM, you will need to start the AttackBox by clicking the Start AttackBox button below. It should take a couple of minutes for both machines to be ready. To carry out this attack, you will need to use Firefox on the AttackBox. Alternatively, you can use your local web browser if you are connected over VPN.

### **Set up your virtual environment**

To successfully complete this room, you'll need to set up your virtual environment. This involves starting both your AttackBox (if you're not using your VPN) and Target Machines, ensuring you're equipped with the necessary tools and access to tackle the challenges ahead.

![](https://tryhackme.com/static/svg/attack-box-to-target-machine.e30b7a02.svg align="left")

**Attacker machine**Status:Off

Start AttackBox

**Target machine**Status:Off

Start Machine

On the AttackBox, use the Firefox web browser to access the vulnerable application by visiting http://MACHINE\_IP:5678. To access n8n, please use the following credentials:

* Email: `tryhackme@thm.local`
    
* Password: `Try12345!`
    

Now, it is time to exploit it. We will be using the “friendly” exploit from within your browser payload available [here](https://github.com/wioui/n8n-CVE-2025-68613-exploit). For your convenience, the exploit code is pasted below:

`{{ (function(){ return this.process.mainModule.require('child_process').execSync('id').toString() })() }}`

First, you need to start a new workflow. Depending on what you see after logging in, you may need to click “**Start from scratch**”.

![n8n showing Start from Scratch](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1766585922631.png align="left")

To carry out the outlined instructions in the original PoC, click “**Add first step**”.

![n8n showing Add first step](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1766585926569.png align="left")

And search for and add **Manual Trigger**.

![n8n Search for Manual Trigger](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1766585922680.png align="left")

Attached to the Manual Trigger that we just added, add an “**Edit Fields (Set)**” option.

![n8n Search for and add Edit Fields](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1766585922719.png align="left")

For the final step, click “**Add Field**,” which will allow you to add a name and a value. Write something like “result” or “exploit” as the name; furthermore, paste the exploit code in the value field. Once you click “**Execute step**”, you will see your command getting executed. In the screenshot below, we can see the output of the id command.

![n8n exploited](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1766586080441.png align="left")

As explained in the previous task, you can replace `id` with any command of your choice.

### Answer the questions below

What is the flag?  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767277198672/09d01122-2ffc-4e0f-a6e7-b9e116915a6f.png align="center")

At first I changed the mode to JSON thinking the manual mapping was showing an error but I could access the `ID` inorder to replace it with a command like `ls`  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767277263676/7962d9aa-51d4-481e-8f71-4c4458f79a8b.png align="center")

  
When I switched back to manual mapping I was able to edit the ID to `ls` inorder to check if we can find a file that has the flag and in the next step I found the flag after knowing the file name

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767277854610/dba591a8-02b8-4026-8eeb-c41ab2097b8c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767277539055/40d0e9bc-2408-44d6-a788-09e21976bbe1.png align="center")

##   
Detection

This portion of the room will provide details on how to detect the n8n Expression Injection Remote Code Execution (CVE-2025-68613) within your SIEM or any other detection solution.

Unfortunately, the n8n solution doesn’t provide a detailed level of logging to use its logs and detect this attack. If you want to explore their logs further, here is their official [log documentation reference](https://docs.n8n.io/hosting/logging-monitoring/logging/).

Considering that, the best way to detect this attack is by setting up a proxy solution to manage the requests coming to the n8n application. With that approach, you only need to send these proxy logs to your detection solution and then use the body content of the web requests to spot this exploitation.

Below you can see a sample configuration for nginx to log body content with `'Request-Body: "$request_body" '`. Note that this may vary depending on your chosen proxy solution:

```json
http {
    # Load Lua module
    lua_package_path "/etc/nginx/lua/?.lua;;";
    
    # Custom log format
    log_format detailed '$remote_addr - $remote_user [$time_local] '
                       '"$request" $status $body_bytes_sent '
                       '"$http_referer" "$http_user_agent" '
                       'Request-Body: "$request_body" '
                       'Content-Type: "$http_content_type" '
                       'Duration: $request_time s';
    
    # ... rest of http block ...
```

## **Sigma Rule**

To detect this vulnerability using Sigma, we can use the following rule:

```yaml
title: N8N Workflow RCE Attempt
status: experimental
description: Detects attempts to inject JavaScript expressions into n8n workflow payloads that execute OS commands via "this.process.mainModule.require('child_process').execSync(...)""
author: TryHackMe Content Engineering Team
references:
  - <https://github.com/wioui/n8n-CVE-2025-68613-exploit>
date: 2025-12-23
tags:
  - attack.execution
  - attack.t1059.007
logsource:
  category: webserver
  product: generic
detection:
  selection:
    cs-method: POST
    cs-uri-stem|endswith: /rest/workflows

  keywords:
    # Strong indicators of this n8n expression injection RCE
    - "this.process.mainModule.require('child_process')"
    - ".execSync("
    - "={{ (function(){"
    - "toString() })()"

  condition: selection and all of keywords
falsepositives:
  - Security testing / red team simulations
  - Developers storing these exact strings in logged fields
level: high
```

In summary, this Sigma rule does:

* Select only requests to the `/rest/workflows` URI path with the method `POST`.
    
* Search for keywords in the body content related to the CVE-2025-68613 exploitation.
    

## **Monitoring Suspicious Command Executions**

Beyond the previous Sigma rule, it’s critical to continue monitoring process creation events to uncover attacker activity after exploitation.

This is critical, since an attacker who only has a valid n8n credentials can fully abuse this RCE to carry out a wide range of malicious actions, such as:

* Establishing a reverse shell to gain interactive access to the system. ([example Sigma rule](https://github.com/SigmaHQ/sigma/blob/master/rules/linux/process_creation/proc_creation_lnx_netcat_reverse_shell.yml))
    
* Downloading and executing malicious payloads to maintain persistence or escalate impact. ([example Sigma rule](https://github.com/SigmaHQ/sigma/blob/master/rules/windows/process_creation/proc_creation_win_curl_download_direct_ip_exec.yml))
    
* Running reconnaissance commands to enumerate the environment hosting the n8n application. ([example Sigma rule](https://github.com/SigmaHQ/sigma/blob/master/rules/windows/process_creation/proc_creation_win_nltest_recon.yml))
    

For this reason, detection should not rely on a single signal. Instead, correlate the web log detection with other process-creation rules to reliably identify post-exploitation behavior associated with this CVE and increase overall detection confidence.

### Answer the questions below

Depending on your environment, ensure that your security solutions are detecting threats targeting your web applications and infrastructure.

## Conclusion

This payload exemplifies why expression evaluation features require extreme caution in application design. The vulnerability isn’t just about improper input validation; it’s about fundamentally flawed trust boundaries between user-provided code and the application runtime environment.

From a purple team perspective, understanding this exploitation chain helps both offensive teams test for similar vulnerabilities and defensive teams develop more effective detection strategies that focus on context escalation patterns rather than just specific payload signatures.

Finally, remember to upgrade your servers to a patched version.

CVE-2025-68613 serves as a strong reminder that **expression evaluation features are high-risk by design when not rigorously sandboxed**. Much like recent React shell-style vulnerabilities, this flaw demonstrates how a single unsafe execution boundary can allow attackers to escalate from user-controlled input to full operating system command execution.

The exploitation chain in n8n is particularly dangerous because it bridges multiple trust boundaries—starting from workflow expressions, escalating into the Node.js runtime, and ultimately reaching native system command execution via core modules such as `child_process`. Once this boundary is crossed, an attacker’s capabilities are limited only by the privileges of the running service.

From a defensive and purple-team perspective, this vulnerability highlights the importance of detecting **context-escalation behavior**, not just known payload strings. Effective detection should combine web request inspection, behavioral analysis, and process-level monitoring to uncover both exploitation attempts and post-compromise activity.

Ultimately, the most effective mitigation is timely patching. Organizations running n8n should immediately upgrade to a fixed release and reassess any systems that evaluate user-supplied expressions within privileged execution environments. Understanding vulnerabilities like this—especially their similarity across different technologies—helps teams anticipate, detect, and prevent entire classes of future attacks, not just individual CVEs.