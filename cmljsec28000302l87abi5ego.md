---
title: "AWS Security Logging (TryHackMe)"
datePublished: Thu Feb 12 2026 18:23:14 GMT+0000 (Coordinated Universal Time)
cuid: cmljsec28000302l87abi5ego
slug: aws-security-logging-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1770920279814/47f803e5-6ef0-40e4-9ef2-c97d73fab67e.png
tags: splunk, awssecurity, socanalyst, cloud-security, awslogs, tryhackme-walkthrough

---

This write-up covers TryHackMe’s [**AWS Security Logging**](https://tryhackme.com/room/awssecuritylogging) room, which focuses on monitoring AWS cloud environments using CloudTrail and GuardDuty. The challenge explores three key security areas: Control Plane monitoring, Managed Services, and Workload security. Through practical Splunk queries, we investigate a cryptocurrency mining incident on an EC2 instance, analyzing GuardDuty alerts, CloudTrail logs, and service-specific logs to piece together the attack timeline.

## Introduction

Amazon Web Services (AWS) is one of the most used IaaS cloud providers, and many companies worldwide, including TryHackMe, heavily use its compute, storage, databases, networking, and ML services. This room explains the tools to monitor the AWS cloud environment for threats and provides best practices on auditing AWS activities in SIEM by the SOC.

**Tip:** It is a good idea to create your own AWS account and test each service as you go through the tasks. While this room focuses on the SIEM perspective, seeing how things appear in the AWS console will help you better understand the material. AWS has a free tier that you can try by following [the link](https://aws.amazon.com/free). Just be mindful of the costs going beyond the free tier.

## **Learning Objectives**

* Explore control plane, managed services, and workload security
    
* Practice using CloudTrail and GuardDuty for threat detection
    
* Learn about cloud log sources, such as CloudFront and S3 logs
    
* Gain a broad overview of how to log and monitor AWS as a SOC
    

## **Prerequisites**

* Complete the [Cloud Security Pitfalls](https://tryhackme.com/room/cloudsecuritypitfalls) room
    
* Complete the [Splunk: The Basics](https://tryhackme.com/room/splunk101) room
    
* Preferably, complete the [Introduction to AWS](https://tryhackme.com/module/introduction-to-aws) module
    
* Preferably, complete the [SOC Level 1 Analyst](https://tryhackme.com/path/outline/soclevel1) path
    

## What to Secure in AWS

## **Cloud Security Areas**

Before explaining what can be logged in AWS, let's recall what exactly we want to protect. To make it easier, imagine the simplest infrastructure built in AWS: a website hosted on a virtual machine ([Amazon EC2](https://aws.amazon.com/ec2/)) that uses an AWS-managed SQL database ([Amazon RDS](https://aws.amazon.com/rds/)) and a managed file storage ([Amazon S3](https://aws.amazon.com/s3/)). To monitor it properly in a SOC, you need to cover at least three security areas:

1. **Control Plane**: Monitor the actions of those who access the AWS console and manage the infrastructure
    
2. **Workloads**: Monitor files and processes on the website's VM, same as you would do in on-premises
    
3. **Managed Services**: Monitor access to the RDS database and suspicious changes to the S3 file storage
    

![AWS architecture diagram showing traffic from an IT admin (1) through an Internet Gateway to an EC2 web server (2), which connects to an RDS database and an S3 bucket (3).](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765244267976.svg align="left")

## **Covering Big Networks**

AWS, and cloud environments in general, can grow large and overly complex ([example](https://aws.amazon.com/blogs/architecture/wordpress-best-practices-on-aws/)). As your company grows, you will find it increasingly challenging to distinguish malicious activity from legitimate behavior and to maintain full visibility across all three security areas. A helpful tip in advance: be prepared to keep learning and adapting as you face challenges like those below.

**Control Plane Chaos  
**Some IT teams manage AWS resources via a graphical interface while others use automation tools, such as [Terraform](https://developer.hashicorp.com/terraform). You must know how your IT operates to build strong detection rules and reliably differentiate good and bad.

**Containerized Workload  
**Instead of regular virtual machines, your IT can run its applications in managed containers ([Amazon Fargate](https://aws.amazon.com/fargate)) or use other serverless technologies. In this case, you must learn how the new technology works in order to monitor it.

**Too Many Managed Services  
**AWS has [more than 200](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/amazon-web-services-cloud-platform.html) managed services, many of which can be exploited in cyber attacks. The service selection by your company defines which logs SOC must ingest and which detection rules you must prepare.

### Answer the questions below

1. Which security area covers management actions within the AWS console? `Control Plane`
    
2. Which of the mentioned AWS services provides virtual machines in the cloud? `Amazon EC2`
    

## Covering AWS Control Plane

## **Meet CloudTrail**

CloudTrail is the first AWS service that any SOC analyst must know. This service records all control plane activities, including console logins, launch of EC2 instances, access to S3 buckets, changes in user privileges and network settings, and more. Technically, CloudTrail is a service that logs all API calls you or your browser makes to the AWS servers. You can view CloudTrail logs in the **CloudTrail &gt; Event history** menu:

![CloudTrail event history menu showing multiple ConsoleLogin events.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765397909072.png align="left")

CloudTrail events are enabled by default and are available for free [for the past 90 days](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html). However, if you want to keep logs longer (and you should), the common choice is to forward the logs to the S3 bucket, and then configure your SIEM to periodically pull the logs from that S3 bucket. The configuration process is a bit tedious, so it's better to refer to the [Splunk documentation](https://splunk.github.io/splunk-add-on-for-amazon-web-services/CloudTrail) or other SIEM-specific guides.

| **AWS Side: Send CloudTrail Logs to S3 Bucket** | **Splunk Side: Get CloudTrail Logs From S3 Bucket** |
| --- | --- |
|  |  |

## **Log Format**

The sample CloudTrail logs are ingested into the Splunk `aws` index, so you can inspect the log structure there. You will learn the details in later rooms, but for now, just focus on the example and key fields below. As a reminder, in AWS terminology, an account and a user are not the same thing: an account represents an entire cloud tenant, while a user is an individual identity within that account. For example, you might have a `john.doe` user inside the `398985017225` AWS account.

```js
// CloudTrail ConsoleLogin event about user authentication (simplified)
{
  "eventID": "b12da7f9-36a9-4ac1-9cb6-7d3b4874d2a7",
  "eventTime": "2025-12-10T17:54:44Z",
  "eventSource": "signin.amazonaws.com",
  "eventName": "ConsoleLogin",
  "awsRegion": "us-east-1",
  "sourceIPAddress": "72.44.21.87",
  "userAgent": "Mozilla/5.0 [...]",
  "requestParameters": null,
  "responseElements": {
    "ConsoleLogin": "Success"
  },
  "readOnly": false,
  "recipientAccountId": "398985017225",
   "userIdentity": {
    "type": "IAMUser",
    "arn": "arn:aws:iam::398985017225:user/john.doe",
    "accountId": "398985017225",
    "userName": "john.doe"
  },
}
```

## **Key CloudTrail Fields**

* `eventSource`: AWS service that was requested. Similar to Windows event channels (e.g. Security)
    
* `eventName`: The name of the API call, such as **ConsoleLogin**, **RunInstances**, or **CreateBucket**
    
* `recipientAccountId`: The target AWS account of the API call. That's where the activity happened
    
* `sourceIPAddress`: Source of the API call. Useful to identify actions from an unusual location or known bad IPs
    
* `requestParameters`: Event-specific details. For **CreateBucket**, it will contain the name of the created S3 bucket
    
* `userIdentity.*`: Details about the actor behind the API call. ARN subfield is a globally unique user identifier
    
* `readOnly`: Whether the API call is a read (e.g. GetObject) or non-read (e.g. PutObject, ConsoleLogin) request
    

**Practice**  
For this task, open the [Splunk console](https://lab_web_url.p.thmlabs.com/) in your web browser.  
You can start from the `index=aws eventName=ConsoleLogin` Splunk query.  
Search for **All Time** and note that field ordering may vary from the example.

### Answer the questions below

1. From which IP did **jeff.harrison** user log in to AWS?  
      
    `index=* user_name="jeff.harrison" source="s3.json"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919110307/23457fbb-b08e-48e8-8658-fb617752f277.png align="center")
    
2. To which AWS account ID did **jeff.harrison** user log in?  
      
    `index=* user_name="jeff.harrison" source="s3.json"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919144045/d6eace6d-93e3-440e-9995-f437b75a00af.png align="center")
    
3. What S3 bucket did **jeff.harrison** create after the login?  
    Note: You will need to change your Splunk query.  
      
    `index=* user_name="jeff.harrison" source="s3.json"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919158070/6cf8efdb-f0ef-479c-9e80-4dc221876341.png align="center")
    

## CloudTrail and GuardDuty

## **CloudTrail Usage**

Once CloudTrail is in your SIEM, you can start using it for control plane monitoring. This is essential because your IT team may accidentally expose internal resources, grant excessive permissions, or fall victim to a cyber attack. CloudTrail gives you the visibility needed to detect all these incidents by recording every API call. For example, imagine SSH access to your Linux VM suddenly becomes public, and you need to know who changed the firewall rules. Simply search CloudTrail for `ModifySecurityGroupRules` events and review the matching record. Here is how it appears in SIEM logs:

```js
// Insecure security group modification (simplified)
{
  "eventTime": "2025-12-12T15:19:57Z",
  "eventSource": "ec2.amazonaws.com",
  "eventName": "ModifySecurityGroupRules",                    // Change in AWS security group (VM firewall)
  "awsRegion": "us-east-1",
  "sourceIPAddress": "34.77.14.56",
  "userAgent": "Mozilla/5.0 [...]",
  "requestParameters": {
    "ModifySecurityGroupRulesRequest": {
      "SecurityGroupRule": {
        "SecurityGroupRuleId": "sgr-0134d56a1ff2c9c74",
        "SecurityGroupRule": {                                // The actual change to the security group
          "CidrIpv4": "0.0.0.0/0",                            // Alows access from anywhere on the Internet
          "FromPort": 22,                                     // To ports in range 22-22 (just one SSH port)
          "ToPort": 22,
          "IpProtocol": "tcp"
        }
      },
      "GroupId": "sg-023caa3bf78723abe"                       // The security group can be used by many VMs
    }
  },
  "responseElements": {...},
  "readOnly": false,                                          // Indicates a "write", risky API call
  "recipientAccountId": "398985017225",
  "userIdentity": {
    "type": "IAMUser",
    "arn": "arn:aws:iam::398985017225:user/jeff.harrison",
    "accountId": "398985017225",
    "userName": "jeff.harrison"                               // Now you know who exposed the SSH port
  },
}
```

## **Control Plane Threats**

There are hundreds of control plane detections you can build using CloudTrail logs. For production-ready examples, review the [Sigma rules](https://github.com/SigmaHQ/sigma/tree/master/rules/cloud/aws/cloudtrail) and search for "AWS" in the [Elastic rule repository](https://www.elastic.co/guide/en/security/current/prebuilt-rules.html) for additional inspiration. When writing custom detections or threat hunting, don't hesitate to Google or ask an LLM which event to look for - AWS has thousands of API actions, and no one memorizes them all. To get started, focus on the key areas below:

* Start from the write events (`readOnly=false`), as they often indicate a high-risk change
    
* **IAM**: Look for changes in user privileges, IAM access keys, and unusual AWS logins
    
* **EC2**: Alert on insecure changes to the EC2 security groups, such as exposed RDP
    
* **S3**: Alert whenever S3 buckets are made public, as they are often exposed by mistake
    

## **Meet GuardDuty**

GuardDuty is an AWS service that automatically analyzes control plane, network, DNS, and endpoint activities within AWS and generates alerts on suspicious behavior in real time. It is a paid service, but you can have a 30-day trial, simulate an attack, and see the alerts in **GuardDuty &gt; Findings** tab. GuardDuty is a perfect choice for companies without a SOC, because it is ready to use out of the box and has many built-in detection rules (finding types) covering the most common AWS threats, such as:

| **GuardDuty Finding Type** | **Description** |
| --- | --- |
| [Exfiltration:IAMUser/AnomalousBehavior](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html#discovery-iam-anomalousbehavior) | The user performed suspicious control plane actions, often seen during data exfiltration |
| [Policy:S3/BucketAnonymousAccessGranted](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-s3.html#policy-s3-bucketanonymousaccessgranted) | The S3 bucket was configured to allow public, anonymous access from the Internet |
| [UnauthorizedAccess:EC2/RDPBruteForce](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-ec2.html#unauthorizedaccess-ec2-rdpbruteforce) | The EC2 instance is exposed to the Internet and is a target of the RDP brute force |
| [Impact:EC2/BitcoinDomainRequest.Reputation](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-ec2.html#impact-ec2-bitcoindomainrequestreputation) | The EC2 instance queried a known domain related to cryptocurrency mining |

![GuardDuty console showing a Findings list with multiple alerts, including root credential usage, potential root account compromise, and an EC2 instance communicating with a known Bitcoin-related IP, with severities ranging from Low to Critical.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765568355252.png align="left")

```js
// The third GuardDuty alert from the screenshot (simplified)
{
  "AccountId": "398985017225",
  "CreatedAt": "2025-12-12T16:57:03.190Z",
  "Id": "c4cd89bf4deb284b5d317d22180ac495",
  "Resource": {                                 // Affected resource details, if any
      "InstanceDetails": {
      "ImageId": "ami-0ecb62995f68bb549",
      "InstanceId": "i-04fa0268276e1f763",
      "InstanceType": "t3.micro",
      "LaunchTime": "2025-12-12T15:17:18.000Z",
      "NetworkInterfaces": [...],
      "Tags": [...]
    },
    "ResourceType": "Instance"
  },
  "Service": {                                  // Finding-specific alert details
  "Action": {
      "ActionType": "NETWORK_CONNECTION",
      "NetworkConnectionAction": {
        "RemoteIpDetails": { "IpAddressV4": "45.32.185.122" },
        "RemotePortDetails": { "Port": 3333 }
      }
    },
    "FeatureName": "VpcFlowLogs",               // Log source used to detect the threat
    "ResourceRole": "TARGET",
    "ServiceName": "guardduty"
  },
  "Severity": 8,                                // Alert severity ranging from 1 to 10
  "Title": "The EC2 instance is [...]",         // The title you saw on the screenshot
  "Type": "CryptoCurrency:EC2/BitcoinTool.B",   // Detection rule (finding type) ID
  "UpdatedAt": "2025-12-12T16:57:03.190Z"
}
```

## **GuardDuty and SOC**

GuardDuty is likely the best monitoring service for companies without a strong security team, as it is very easy to use, can detect a large number of common threats, and does not require much knowledge to be effectively used by IT. However, any mature SOC team would still need to collect and monitor raw logs, such as CloudTrail, because:

* GuardDuty alerts often lack sufficient context to fully scope and document the attack
    
* GuardDuty detection rules do not adapt to your company-specific policies and risks
    
* GuardDuty is effective against common threats, but is unlikely to detect advanced attacks
    
* Finally, GuardDuty can't reliably detect when AWS logging is tampered with or disabled
    

**Practice**  
Сontinue with the **aws** index in Splunk. You might need two queries:  
    - CloudTrail logs: `index=aws sourcetype=aws:cloudtrail`  
    - GuardDuty alerts:  `index=aws sourcetype=aws:cloudwatch:guardduty`  
Search for **All Time** and note that field ordering may vary from the examples.

### Answer the questions below

1. Find the "AnomalousBehavior" GuardDuty alert in the logs.  
    From which VPN did the suspicious activity originate?  
      
    `index=aws sourcetype=aws:cloudwatch:guardduty`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919335637/49b75227-5c74-44c8-850a-bc4dcfdb5615.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919361694/86f4caf0-8d53-47d0-afe7-17e1321582e7.png align="center")
    
2. Analyze two other alerts from the **i-04fa0268276e1f763** EC2 instance.  
    What is the path to the detected malware, and which domain did it query?  
      
    `index=aws i-04fa0268276e1f763 sourcetype=aws:cloudwatch:guardduty`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919376460/45f8247f-d807-4af2-8ffe-748c808ff114.png align="center")
    
    `index=aws i-04fa0268276e1f763 sourcetype=aws:cloudwatch:guardduty xmrig`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919389424/972eec56-5d31-4656-b9f2-94e099af1f62.png align="center")
    
3. Continue to the CloudTrail logs to get more instance context.  
    Who created the infected EC2 instance? Provide the full ARN field.  
      
    `index=aws sourcetype=aws:cloudtrail i-04fa0268276e1f763`  
      
    the role = `root`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919574652/831ea470-107c-40dc-9d3d-be0cb64dfb95.png align="center")
    
4. Which two risky ports did that user expose for the EC2 instance?  
    Answer Example: 80, 443  
      
    For this step, we’ll need to add the `eventName=ModifySecurityGroupRules` to access the ports  
      
    `index=aws sourcetype=aws:cloudtrail eventName="ModifySecurityGroupRules"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919747885/e502cc0d-b6c0-4d1b-b4de-5c1a84b974a1.png align="center")
    
    `index=aws sourcetype=aws:cloudtrail eventName="ModifySecurityGroupRules"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770919758438/e36e7bf3-cec6-475d-930d-f082e1f528b9.png align="center")
    

## Covering Managed Services

CloudTrail is ideal for capturing control plane operations, but it doesn't provide visibility into service-specific actions. For example, it cannot see queries made to an Amazon RDS database, network traffic flowing through your [VPCs](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html), web requests served through Amazon CloudFront, and many other operations. To cover this blind spot, AWS provides detailed logging capabilities for each major service separately. Let's learn some examples!

## **CloudFront Logs**

CloudFront is a CDN service heavily used by AWS customers to optimize and secure web applications. You might have heard about its competitors: Cloudflare, Fastly, and Akamai. The importance of this service is that it usually routes all web traffic and thus can capture SQL injections, CVE exploits, web shell uploads, brute force, and DDoS attacks. The log format is very similar to IIS access logs and follows the W3C standard, where `c/cs` prefix refers to client fields, and `s/sc` denotes server-side fields:

CloudFront Logs in Raw Format

```plaintext
date       time     edge_srv sc_bytes c_ip         cs_method cs_host       cs_uri_stem sc_status cs_user_agent
2025-12-12 11:32:11 SFO53-C1 4955     80.42.89.4   POST      tryhackme.com /login      200       Mozilla/5.0   [...]
2025-12-12 11:32:24 SFO53-C1 6002     88.15.14.16  GET       tryhackme.com /portal     200       Mozilla/5.0   [...]
2025-12-12 11:32:35 SFO53-C1 1624     13.13.17.146 GET       tryhackme.com /search     200       Mozilla/5.0   [...]
```

![A screenshot from Splunk showing CloudTrail events. The fields in the events are delimited by tabs and resemble IIS logs.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765842784198.png align="left")

## **VPC Flow Logs**

In the on-premises world, you would collect network logs from your firewall devices. But how would you monitor traffic in the cloud? AWS offers several services for auditing network traffic, with Amazon VPC Flow being the most notable one. This service records network flows to, from, and between your cloud workloads, giving you visibility into how systems communicate across your subnets. You will find many fields there, most importantly:

* `srcaddr` and `dstaddr`: Source and destination IP address of the flow
    
* `srcport` and `dstport`: Source and destination network port of the flow
    
* `protocol`: IANA protocol number of the traffic (TCP is 6, UDP is 17, ICMP is 1)
    
* `action`: Whether the traffic was allowed by AWS security groups and ACLs
    
* `account-id`, `vpc-id`, `interface-id`: Additional context about the traffic source
    

VPC Flow Logs in Raw Format

```shell
version account-id interface-id srcaddr       dstaddr      srcport dstport protocol action [...]
2       123456789  eni-12345678 172.31.16.4   172.31.16.7  20641   80      6        ACCEPT [...]
2       123456789  eni-12345678 172.31.9.69   172.31.9.12  49761   22      6        REJECT [...]
```

Note that while VPC Flow Logs are very popular among SOC teams, they can be challenging to use and generate terabytes of events per day in busy environments. For example, VPC Flow Logs will generate a dozen events for just a single web request, because it logs every hop of the packet, both in and out, as separate events. Before you start ingesting it for network monitoring, consider alternatives with better noise-to-signal ratio: DNS logs from Amazon Route53 or endpoint events (e.g., Sysmon event codes 3 and 22)

## **S3 Data Events**

Remember that CloudTrail audits control plane activities and doesn't log operations inside managed services. However, CloudTrail can be extended to record Data events (`CloudTrail > Your Trail > Data Events`) for some of the services, including S3. With Data events, you will see operations on individual S3 objects, such as file access (GetObject) or upload to the bucket (PutObject). The log format is nearly identical to the CloudTrail control plane events:

![A screenshot from Splunk showing S3 data events. The events are in JSON format and closely resemble CloudTrail logs.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765843858839.png align="left")

## **Other Events**

CloudWatch is yet another AWS service widely used for centralized logging and monitoring. While it doesn't replace a SIEM, your DevOps team will likely use it to collect different logs for debugging purposes. Depending on your environment, CloudWatch may contain dozens of service logs, from databases and VMs to Kubernetes and AI. At some point, you may be required to connect CloudWatch with your SIEM to monitor services like RDS:

![AWS RDS console displaying a database audit log file (server_audit.log), with database logins and row SQL queries visible in logs. The last query states "SELECT email,password FROM thm_users" and may indicate data exfiltration.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765406862562.png align="left")

**Practice**  
Сontinue with the **aws** index in Splunk. You will work with CloudFront and S3 logs:  
    - CloudFront logs: `index=aws source="cloudfront.log"`  
    - S3 data events:  `index=aws source="s3.json"`  
Search for **All Time** and note that field ordering may vary from the examples.

### Answer the questions below

1. Start with the CloudFront access logs.  
    Which IP address logged in to the admin portal?  
      
    `index=aws source="cloudfront.log" cs_uri_stem="/admin/dashboard"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770920004779/d94bee19-c7ab-40df-9a77-74d0ea75561f.png align="center")
    
2. Find an answer in the same CloudFront logs.  
    How many IPs searched for the "tryhackme" keyword?  
      
    `index=aws source="cloudfront.log" tryhackme`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770920092567/e8281d87-7309-4fdd-90d2-eab103afe2b4.png align="center")
    
3. Now, move on to the S3 Data events.  
    Which interesting S3 file has been accessed?  
      
    `index=aws source="s3.json"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770920125768/042341e2-15a3-4e2c-a606-73ad0ddafb6a.png align="center")
    

## Logging of AWS Workloads  
  
**Workloads and Services**

The previous task focused on logging fully AWS-managed services - solutions like Amazon S3 that the vendor builds, maintains, and patches on your behalf. However, some companies choose not to rely on managed services due to cost concerns, government regulations, or other constraints. Instead, they run the necessary software on EC2 instances or integrate the cloud with their on-premises data centers. For example, they can:

* Install PostgreSQL on a virtual machine (EC2) instead of using managed Amazon Aurora
    
* Keep confidential document scans in an on-premises data center rather than in S3
    
* Deploy and configure Nginx web proxies instead of relying on AWS load balancers
    
* Run niche software on EC2 that is not available through any AWS-managed service
    

![Diagram comparing SOC monitoring of unmanaged versus managed AWS services. On the left, a self-managed PostgreSQL database on an EC2 VM requires installing a SIEM agent and collecting local log files. On the right, a managed service like Amazon Aurora provides logs via AWS APIs, with AWS handling the OS and infrastructure.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1769437972682.svg align="left")

*SOC approach to monitoring unmanaged versus managed services  
*

For you as a SOC analyst, this approach means reverting to traditional endpoint monitoring. Instead of relying on AWS-managed logging, you will need to install SIEM agents on the VMs, deploy tools like Auditd or Sysmon across your EC2 instances, and keep an eye on software vulnerabilities. If your company uses the cloud only to host virtual machines, your monitoring will look much the same as it would in an on-premises environment.

## **Containerized Workloads**

Small cloud-adopting companies typically run between 5 and 20 EC2 instances, along with a few managed services such as S3 for storing uploaded images. Monitoring is straightforward in such environments. However, at some point, the IT team may decide to migrate workloads into containers (for example, using Amazon ECS) or adopt serverless technologies, such as AWS Lambda. We will cover these in detail in another room, but please keep the following in mind for reference:

| **Important Information** | **Possible Solution** |
| --- | --- |
| Auditd and most EDR solutions don't work effectively in containers. | Use cloud-native tools such as [Falco](https://tryhackme.com/room/k8sruntimesecurity), or CWPP solutions like [Wiz](https://www.wiz.io/solutions/cwpp). |
| All traditional monitoring agents can't operate in serverless workloads. | Use CWPP or refer to AWS security guidelines for that specific service. |
| You can't protect cloud environments without understanding containers. | Master your Linux knowledge and learn about containers and Kubernetes! |

### Answer the questions below

1. How would you call a service that is built and maintained by the cloud vendor? `Managed Service`
    
2. Which cloud workload monitoring tool was mentioned as an alternative to Auditd? `Falco`
    

## Conclusion

CloudTrail, CloudFront, CloudWatch, and many more - feels like a lot to learn, right? Don't worry, you will explore some of these services in much greater detail in the upcoming rooms. Think of this room as a general reference you can return to whenever you need a refresher. Hopefully, you enjoyed it!

## **Note for Other Clouds**

The concepts explained throughout this room are applicable to most cloud providers. For example, AWS CloudTrail maps to Azure Activity Log in Microsoft Azure and to Cloud Audit Logs in Google Cloud. By understanding the fundamentals explained in this room, especially the three security areas, you will be able to switch between cloud platforms with ease after some practice.

  
This room provided hands-on experience with AWS security logging across multiple log sources: - CloudTrail for control plane API monitoring - GuardDuty for automated threat detection - CloudFront and S3 logs for service-specific activity - VPC Flow Logs for network visibility Key takeaway: Effective cloud security requires understanding which logs capture what activities. CloudTrail shows WHO did WHAT, GuardDuty shows WHY it's suspicious, and service logs provide the detailed context. The investigation revealed how an exposed EC2 instance was used for cryptocurrency mining, demonstrating the importance of monitoring security group changes and establishing baseline behaviors in cloud environments.