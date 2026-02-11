---
title: "Monitoring AWS Logins (TryHackMe)"
datePublished: Wed Feb 11 2026 17:35:43 GMT+0000 (Coordinated Universal Time)
cuid: cmlib9dp5000g02jvc0k2el5g
slug: monitoring-aws-logins-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1770831078198/ff4705ec-e1c0-461a-8583-b9de93c5474b.png
tags: aws, splunk, iam, tryhackme, awsmonitoring

---

[Monitoring AWS Logins](https://tryhackme.com/room/monitoringawslogins) room teaches you to:

1. **Monitor AWS authentication** - who's logging in, from where, and with what credentials
    
2. **Understand IAM components** - users, roles, policies, and access keys
    
3. **Detect real-world attacks** - phishing, leaked keys, role abuse
    
4. **Use CloudTrail + Splunk** - filter logs and investigate incidents
    

**Key takeaway:** Authentication monitoring is the *first line of defense* in cloud security.

## Introduction

Monitoring the control plane is the first step in securing your cloud environment. And within the control plane, authentication is the first area to focus on: who is logging in to AWS, from where, and which credentials they are using. This room walks you through the most common attacks against AWS identities and the defenses used to protect them.

## **Learning Objectives**

* Understand the concept of IAM, access keys, roles, and policies
    
* Learn how CloudTrail logs different methods of logins in AWS
    
* Explore real-world cloud breaches and learn how to avoid them
    
* Practice the acquired knowledge in a series of mini-challenges
    

## **Prerequisites**

* [AWS Security Logging](https://tryhackme.com/room/awssecuritylogging)
    
* [Splunk: The Basics](https://tryhackme.com/room/splunk101)
    
* Preferably, [Introduction to AWS](https://tryhackme.com/module/introduction-to-aws) module
    
* Preferably, [SOC Level 1 Analyst](https://tryhackme.com/path/outline/soclevel1) path
    

## IAM and User Credentials

## **IAM and AWS Credentials**

AWS Identity and Access Management (IAM) is a service, or rather a core AWS feature, that allows you to create cloud users, define policies and permissions, manage access to AWS resources, and much more. In this task, you will explore three methods to authenticate into AWS with IAM credentials, but first, remember its key components:

* **IAM User**: A person or application that needs AWS access. Just like the Active Directory user
    
* **IAM Group**: A collection of IAM Users, such as "EU DevOps Team" or "IT Support Contractors"
    
* **IAM Role**: An AWS identity that can be assumed by users or AWS services, more on it soon
    
* **IAM Policy**: A JSON document that defines permissions attached to users, groups, or roles
    

## **AWS Management Console**

How do you securely access AWS resources via a web browser? First, you don't want to use the root user every time, so you create a new IAM user (e.g. **steven.carter**). Then, you attach an IAM policy to the user (e.g. **AdministratorAccess**). Finally, you open the AWS website, enter your account ID, username, password, and, hopefully, MFA. Once you log in, you will appear in a Management Console, from which you can launch VMs and manage your cloud account.

![AWS login screen comparison showing Root user sign-in by email on the left and IAM user sign-in by IAM credentials on the right. Center labels point to each option and highlight that all login types support MFA.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765929400663.svg align="left")

## **IAM Access Keys**

If you need to access AWS resources programmatically, such as in your Python code (SDK) or through the Bash terminal (CLI), you would need AWS access keys. Each user can have up to two access keys, and each access key grants you the same permissions as you'd have in the Management Console. The key consists of two parts: a safe-to-share access key ID and a confidential access key secret. Refer to the examples below.

![AWS IAM console screenshot showing the Create access key workflow for user michael.turner. The page displays the generated Access key ID and Secret access key.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765984016248.png align="left")

Access Key Usage inAWSCLI

```bash
admin@it-srv:~$ aws configure                           # Setting the access key
AWS Access Key ID [None]: AKIAVZZK4G6ETIOB7S7I
AWS Secret Access Key [None]: WzHnzY2hj2UI[...]
admin@it-srv:~$ aws s3 ls                               # Listing all S3 buckets
2025-12-09 20:30:30 aws-cloudtrail-logs
2024-11-24 17:41:10 thm-db-backups
admin@it-srv:~$ aws s3 ls thm-db-backups --recursive    # Viewing the S3 bucket
2024-11-25 22:26:37             0   prod/
2025-12-17 01:07:14        790984   prod/backup.meta
2025-12-17 02:08:56   12089789743   prod/backup.tar.gz
```

## **IAM Roles and Policies**

Imagine you have a few EC2 instances that resize uploaded images in the **user-avatars** S3 bucket. Would you create a new IAM service user, assign the correct permissions, generate a long-term access key, and hardcode it in EC2 in your application's source code? No, since it's tedious and insecure. Instead, you can create a **UserAvatarsProcessor** role, define its policy, and link it to the EC2 instances. No passwords or keys required! See an example in the image below:

![AWS IAM role creation flow highlighting three steps: Select trusted entities (define the trust policy and who can assume the role), Add permissions (set the permissions policy and allowed actions), and create the role, after which it can be assumed on demand.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765984015994.svg align="left")

The roles are extensively used by AWS services to perform actions on your behalf, allow SSO or cross-account access, and other [common scenarios](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios.html). With roles, you don't worry about credentials, as everything is controlled by policies and trust policies. Behind the scenes, AWS generates short-lived **STS** credentials whenever you assume a role. While STS tokens can still be stolen (for example, from a compromised workload), doing so is much harder than compromising long-lived access keys. Below are the key benefits of using IAM roles:

| **#1** | STS tokens are temporary and valid for a few hours, unlike the permanent passwords and access keys |
| --- | --- |
| **#2** | Roles and STS tokens aren't hardcoded in files by design, reducing the risk of credential leaks (e.g. [from GitHub](https://github.blog/security/application-security/next-evolution-github-advanced-security)) |
| **#3** | Roles can be restricted to specific IP addresses, resources, or identities, which allows for granular access control |

## **Other Sign-In Methods**

There are also more advanced sign-in methods, such as via Google Workspace SSO, Active Directory, Amazon Cognito, and IAM Identity Center. Each introduces its own security risks and requires specific detection. However, at a high level, all of them rely on temporary STS credentials and the same role assumption mechanism you already read about. For more details and future reference, check out [this AWS blog post](https://aws.amazon.com/blogs/security/how-to-easily-identify-your-federated-users-by-using-aws-cloudtrail/).

### Answer the questions below

1. What type of credential is used to access AWS resources via CLI/SDK? `Access Key`
    
2. Which IAM identity type allows you to gain AWS permissions temporarily? `IAM Role`
    

## Monitoring Console Logins

## **Monitoring Console Logins**

Brute-forcing in AWS is rare, and most users are protected with MFA anyway. So is it worth monitoring logins to the Management Console? Absolutely! Attackers often run phishing campaigns to lure victims into entering their AWS credentials into fake login forms, leading to data exfiltration, miner infections, or cloud ransomware attacks. Many companies were breached this way. Refer to the [Wiz blog post](https://www.wiz.io/blog/emerging-phishing-campaign-targeting-aws-accounts) for a good example:

![Screenshot from Wiz blogpost of a phishing email styled as an AWS notification claiming Service Suspended due to overdue payment, showing an amount, due date, and a Manage Account button.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765921655890.png align="left")

*A phishing email leading to a fake AWS login page (Wiz)*

## **Console Logins and CloudTrail**

Management Console logins are the easiest to monitor, as CloudTrail logs every sign-in as the **ConsoleLogin** event. You can filter CloudTrail events using `eventName=ConsoleLogin` query and apply the regular investigation approach: verify whether the source IP is trusted or malicious, assess if the login time, user agent, and geolocation are typical for the user, and look for any unusual activity following the login. Below is an example of the ConsoleLogin event and a table that clarifies some of the fields:

```js
// Most important fields of the ConsoleLogin event
{
  "eventTime": "2025-12-18T16:48:27Z",
  "eventName": "ConsoleLogin",
  "errorMessage": "Failed authentication",
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDAVZZK4G6EYIICHBXXX",
    "arn": "arn:aws:iam::398985017225:user/john.doe",
    "accountId": "398985017225",
    "userName": "john.doe"
  },
  "sourceIPAddress": "12.42.87.160",
  "userAgent": "Mozilla/5.0 [...]",
  "requestParameters": null,
  "additionalEventData": {
    "LoginTo": "https://console.aws.amazon.com/console/[...]",
    "MobileVersion": "No",
    "MFAUsed": "Yes"
  },
 "recipientAccountId": "398985017225"
}
```

| **Field Name** | **Explanation** |
| --- | --- |
| errorMessage | If this field exists, it means the authentication was unsuccessful. |
| userIdentity.arn | A unique user identifier across all AWS accounts worldwide. |
| additionalEventData.MobileVersion | Whether the user logged in from the AWS mobile application. |
| additionalEventData.MFAUsed | Whether the user used MFA. It should always be set to "Yes"! |

## **Key Monitoring Tips**

* Monitor logins on the root user. Your IT team should use root for emergencies, not the daily routine
    
* Monitor console logins from service users. They are expected to use roles or access keys instead
    
* Monitor successful logins from VPN, Tor, cloud hostings, or otherwise suspicious IP addresses
    
* A focused, aggressive brute force might indicate that adversaries target your company
    

**Practice**  
For this task, access the VM and search the **task3** Splunk index.  
You can start from the `index=task3 eventName=*` Splunk query.  
Search for **All Time** and note that field ordering may vary from the example.

### Answer the questions below

1. How many times did Thomas fail to log in to the AWS console?
    
    `index=task3 eventName=*`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770808192230/25392835-7550-4d6a-b849-64b640720d49.png align="center")
    
    `index=task3 eventName=* user="thomas.bennett"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770808067145/8b856846-98f3-41a9-9a71-bb70cf7ebc41.png align="center")
    
2. Which other user logged in to the AWS console without MFA?  
    `index=task3 eventName=*` `authentication_method=SFA action=success`
    
    selected `authentication_method` that wasn’t MFA
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770808362066/b6545878-a7a5-438c-a027-fe454681bf79.png align="center")
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770808525279/865dafde-c2b2-4fd4-938f-ca35dfd5d7b3.png align="center")

## Monitoring Access Keys

## **Risks of Access Keys**

For programmatic access, you use access keys instead of passwords: to integrate with third-party services, deploy infrastructure with Terraform, or send CloudTrail logs to a SIEM. Because access keys must be included in every API call, developers often store them in plaintext in source code, databases, or [configuration files](https://docs.aws.amazon.com/cli/latest/userguide/cli-authentication-user.html#cli-authentication-user-configure-csv.titlecli-authentication-user-configure-file:~:text=see%20aws_configure_import.-,Directly%20editing%20the%20config%20and%20credentials%20files,-To%20directly%20edit). The risk is that if any of these locations are compromised, the access keys will be stolen. In fact, leaked AWS access keys remain the most common cause of cloud breaches. Here are some real-world examples:

| **Cloud Email Abuse Campaign (Wiz)** | In May 2025, threat actors [abused](https://www.wiz.io/blog/wiz-discovers-cloud-email-abuse-campaign) AWS access keys, previously harvested by data stealers or discovered through public dorking, to start a phishing campaign. Using these keys, they configured Amazon Simple Email Service (SES) to mass-deliver phishing messages directly from the victims' AWS accounts. Save the blog post for future reading, it's very insightful! |
| --- | --- |
| **Large Scale Cloud Extortion Operation (Unit 42)** | In this global campaign, more than 70,000 AWS access keys were [obtained](https://unit42.paloaltonetworks.com/large-scale-cloud-extortion-operation/) from publicly accessible .env files. The keys were then used to exfiltrate and delete S3 objects from the target's S3 buckets. Finally, the threat actors uploaded a ransom note to the empty S3 buckets, urging the victims to pay a ransom to prevent the threat actors from selling the data. |

![Screenshot from Unit 42 website of a ransom note claiming 100% of S3 files were exfiltrated, stating the attacker has client personal information, and demanding Bitcoin payment to prevent sale on the dark web. It warns of consequences if unpaid and provides a contact method for negotiation.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1766581369471.png align="left")

*Ransom note left by the threat actor (Unit 42)*

## **Monitoring Access Keys**

The first difference you should know about is that the **ConsoleLogin** event **is not logged** when using access keys, as programmatic access does not have a login stage - your credentials are included with each API call. Also, compare the evidence of deleting the RDS database via console and via CLI. Within the console session, it's always a bunch of Get\*, List\*, and Describe\* events. But with access keys, it's only what you explicitly asked for:

![Timeline comparing database deletion via Access Key (CLI) vs Console Login. The CLI path shows a single CLI command leading directly to DeleteDBInstance. The console path shows multiple CloudTrail events—ConsoleLogin, GetCallerIdentity/DescribeAccount, repeated ListDBInstances/DescribeDB calls, and finally DeleteDBInstance—reflecting how the AWS console loads menus before the delete action.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1769450456413.svg align="left")

In other respects, CloudTrail logs are almost identical whether you use access keys or the console. The more interesting challenge is figuring out how the action was performed: from the console (GUI) or via an access key (CLI/SDK). This distinction matters for root cause analysis: was it a phished console session, or a leaked access key? Let's look at the differences using an example from the **DeleteDBInstance** event:

![Diagram comparing CloudTrail logs for Console Login vs Access Key activity. It highlights that for access key logins, accessKeyId shows the real key, while for GUI logins, it may be a temporary token; access key events lack a sessionContext section; and userAgent differs (browsers for console vs programmatic agents like aws-cli for access keys).](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1769453111147.svg align="left")

```c
// Splunk query to filter only for access key logins
index=aws userIdentity.accessKeyId=AKIA*
```

```c
// Alternative query to filter out console actions
index=aws NOT userIdentity.sessionContext.attributes.creationDate=*
```

## **SOC Pain Points**

Investigating CLI events performed by human employees is usually straightforward, since the intent behind their actions is often easy to understand. The real challenge comes when analyzing activity coming from DevOps scripts or other automations, the purpose of which is often unknown to SOC. To help you identify whether an action is likely benign or malicious, consider the three practical tips below:

* \*\*Correlate user purpose and action  
    \*\*If the svc-s3-backup user suddenly starts launching EC2 instances instead of uploading objects to S3, this is a strong attack indicator. Especially if for the previous 90 days it never did such an action before. You can get more context about the user from the company documentation, [AWS tags](https://docs.aws.amazon.com/whitepapers/latest/tagging-best-practices/what-are-tags.html), or its interactions with other AWS services.
    
* \*\*Investigate the source IP address  
    \*\*If the svc-gitlab-cicd user suddenly logs in from a DigitalOcean IP after years of operating from the GitLab IP range, this is a strong breach indicator. Service users don't change their location unless the service is migrated to another data center. You can get IP context on various TI services, such as [iplocation.net](https://iplocation.net/).
    
* \*\*Investigate the AWS client (user-agent)  
    \*\*If your company heavily uses Java in AWS, you may see a lot of events with the aws-sdk-java/\* user-agent. But if you see a sudden switch to, let's say Boto3/\* (Python) or aws-cli/\* (CLI) user-agent, it's either your team switched to another AWS client, or the user is compromised. If you can't recognize the user-agent, always google it first, and then confirm with your IT!
    

**Practice**  
You will investigate a compromise of **michael.turner's** access key.  
The compromise led to an extortion attack on a private S3 bucket.  
Search for **All Time** and start from the `index=task4` query.

### Answer the questions below

1. What access key ID of Michael was used in the attack?
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770813065594/6f4993e0-d987-4eb4-87d5-764d4127520d.png align="center")
    
    `index="task4" user="michael.turner"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770813081775/c382cfa2-e691-4dc7-99b6-9a9a005253a9.png align="center")
    
2. What is the name of the S3 bucket accessed by the attackers?
    
    `index="task4" eventSource="s3.amazonaws.com"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770813336417/cd1c8771-295a-4e8c-a56f-ab566d552eb2.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770813377026/910e9763-c56a-40f7-81ea-dee6bc1465ee.png align="center")
    
3. How many files were exfiltrated and deleted by the adversary?
    
    `index="task4" user="michael.turner"`
    
    Then checked: `eventName` with focus on `DeleteObject` & `GetObject`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770813495837/685d042c-4fcb-4856-b0c0-1124172f61f3.png align="center")
    
4. Which file was uploaded to the bucket at the end of the attack?
    
    `index="task4" eventSource="s3.amazonaws.com" eventName=PutObject`
    
    check under `requestParameters`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770813611818/b098a1cd-14fa-4d65-a48f-86c58db7d9aa.png align="center")
    
5. Which AWS service was used most by the user who **did not** use access keys?  
    Answer Example: Amazon Redshift
    
    `index="task4" user="thomas.bennett"`
    
    check the `eventSource`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770813724934/0e91b4b1-1e35-4166-84df-f09f3c69773b.png align="center")
    

## Detecting IAM Role Abuse

## **IAM Roles for Services**

The primary use of IAM roles is to allow AWS services to interact with each other. Let's continue with the example from task 2, where a Python script on your EC2 instance has to process user avatars in one of the S3 buckets. To make it happen, you link the **UserAvatarsProcessor** role to the instance. Then, the instance will automatically assume the role and use it whenever needed. In CloudTrail, EC2 actions would look like this:

```js
// Simplified GetObject request made by the UserAvatarsProcessor role
{
  "eventTime": "2025-12-29T20:28:48Z",
  "eventSource": "s3.amazonaws.com",
  "eventName": "GetObject",
  "sourceIPAddress": "98.93.221.113",                       // Public IP of the EC2 instance
  "userAgent": "Boto3/1.34.46 md/Botocore#1.34.46[...] ",   // Boto3 is AWS client for Python
  "userIdentity": {
    "type": "AssumedRole",                                  // Is always set to "AssumedRole"
    "principalId": "AROAVZZK4G6ERIIVB7K4M:i-123456789",
    "arn": "arn:aws:sts::398985017225:assumed-role/UserAvatarsProcessor/i-123456789",
    "accountId": "398985017225",
    "accessKeyId": "ASIAVZZK4G6E7I2DBMBB",                  // Always starts with "ASIA"
    "sessionContext": { [...] }
  },
}
```

Take a look at the last part of the ARN field, **i-123456789**. This is a role session name, and its meaning can vary depending on who assumes the role. By default, AWS sets it to the instance ID for EC2 and a user name for IAM users. However, it is possible to customize the session name and name it, for example, **MyCustomSession**. In this scenario, you won't know who assumed the role and will need to perform an additional investigation. Below are some ARN examples when using roles:

```c
arn:aws:sts::[account-id]:assumed-role/[role-name]/[session-name]
arn:aws:sts::398985017225:assumed-role/UserAvatarsProcessor/i-123456789  // Likely an EC2 instance ID
arn:aws:sts::398985017225:assumed-role/US-ITSupportRole/john.doe         // Likely an IAM user name
arn:aws:sts::398985017225:assumed-role/SuperAdmin/MyCustomSession        // Need to investigate
```

## **AssumeRole Event**

To reiterate, IAM roles can't operate on their own. During alert triage, you must always point to a real patient zero, a user or service that assumed the role: your IT admin, EC2 instance, Lambda function, or even AWS cloud itself. In many cases, you can identify the actor by reading the last part of the ARN (the session name). However, if a custom session name is used, that approach won't work. Luckily, CloudTrail records the **AssumeRole** event, as shown in the example below:

```js
// SuperAdmin role assumed by john.doe
{
  "eventTime": "2025-12-29T22:08:01Z",
  "eventSource": "sts.amazonaws.com",
  "eventName": "AssumeRole",
  "sourceIPAddress": "77.23.56.75",
  "userAgent": "aws-cli/2.31.39 [...]",
  "requestParameters": {
    "roleArn": "arn:aws:iam::398985017225:role/SuperAdmin",
    "roleSessionName": "MyCustomSession"
  },
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDAVZZK4G6E5AAVXXXXX",
    "arn": "arn:aws:iam::398985017225:user/john.doe",
    "accountId": "398985017225",
    "accessKeyId": "AKIAVZZK4G6EXJOXXXXX",
    "userName": "john.doe"
  }
}
```

If the user above assumes the role and names the session **MyCustomSession**, all subsequent actions will be logged as coming from the role itself and won't refer to the original user name. As a result, you must trace back to the preceding **AssumeRole** event whenever you want to know who actually assumed the role. You will observe this behavior in the Splunk practice logs, and you are also encouraged to test it yourself using the AWS CLI.

## **IAM Role Abuse**

IAM roles can be abused by adversaries. For example, imagine your EC2 instance gets compromised (e.g., via SSH brute-force attack) and an attacker gains full control over it. If that instance has an IAM role attached with S3 permissions, the attacker effectively inherits the same permissions as the EC2 and gains access to the S3 bucket. This is exactly how Capital One Bank exposed **100 million credit cards** ([Article](https://edition.cnn.com/2019/07/29/business/capital-one-data-breach), [Demo](https://github.com/shayrm/CapitalOne-SSRF-demo)), so don't underestimate IAM roles! Below are a few monitoring tips:

* Detect a sudden change in the user-agent, same as with the access keys
    
* Hunt for first-seen API calls, especially those related to data exfiltration
    
* Protect the workloads, such as EC2 instances, that use IAM roles
    

## **Cross-Account Access**

IAM roles are also used for cross-account access, which is fun to investigate. Imagine a scenario: Sarah from IT logged in and assumed the **it-support** role in the US account, then switched to the **eu-helpdesk** role in the EU account, and then assumed the **rds-read** role to debug a database issue. This is called role chaining, and it is common in large companies with complex environments. To "unchain" the events, you will have to analyze the **AssumeRole** events one by one until you find the real user or service behind the activity:

![Illustration of IAM role chaining in an incident timeline. Sarah (IT Support) assumes the it-support role at 10:30, which then assumes eu-helpdesk at 10:45, then rds-read at 11:00, leading to an RDS login alert at 11:05. The SOC analyst investigates by tracing back who assumed each role to identify the original actor and close the alert.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1765935929759.svg align="left")

**Practice**  
In this task, you will see how regular users and AWS services use IAM roles.  
Search for **All Time**, use the **task5** index, and start from the `index=task5` query.

### Answer the questions below

1. Which EC2 instance ID used the **UserAvatarsProcessor** role?
    
    `index="task5" "userIdentity.arn"="arn:aws:sts::398985017225:assumed-role/UserAvatarsProcessor/<REDACTED>"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770813853780/d6111809-1c02-4c9d-8607-3f84486204f2.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770813887348/bc65b6d3-946f-4d73-a62a-ddcefa9cdf06.png align="center")
    
2. Someone assumed the **EU-RemoteSupport** IAM role.  
    How did they name the role session?
    
    `index="task5" user_arn="arn:aws:sts::398985017225:assumed-role/EU-RemoteSupport/<REDACTED>"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770814144078/1c5fcd93-1a89-4d45-acd5-ab82c2c2feb4.png align="center")
    
3. Which user assumed the IAM role from the question above?
    
    `index="task5" eventName=AssumeRole requestParameters.roleArn="*EU-RemoteSupport*"`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770814357554/da2592d9-6229-47af-aec0-477e0d06aca2.png align="center")
    

## Detecting IAM Changes

## **Early Threat Detection**

The previous tasks focused on the moment of exploitation. However, the conditions for many attacks often form months or even years earlier. A common scenario: in 2023, an IT administrator created a root access key for testing and accidentally exposed it while recording a YouTube video guide. The key went unnoticed until 2025, when it was finally discovered and exploited. The ideal outcome for SOC would have been to detect and prevent the creation of the key in 2023, right?

## **IAM Misconfigurations**

It is a good idea to set up monitoring rules not only for signs of compromise but also for misconfiguration. In other words, hunt for adversaries compromising IAM credentials, and for your IT administrators assigning overly privileged policies or creating unnecessary access keys. Below are the most common IAM events you should monitor for misconfigurations:

| **Event Names** | **Description** | **Suggestions** |
| --- | --- | --- |
| `PutUserPolicy`, `PutRolePolicy`, `PutGroupPolicy` | An existing AWS policy has been modified | Look for naming mismatches, such as "ReadOnlyAccess" policy modified to allow EC2 launch and termination |
| \`Attach\[User | Group | Role\]Policy`,` AddUserToGroup\` |
| `DeleteVirtualMFADevice`, `DeactivateMFADevice` | A user deactivated their own, or someone else's MFA | Such events are legitimate **only** if followed by a new MFA registration (e.g. when a user loses their mobile phone and needs to set up a new MFA device) |
| `CreateAccessKey`, `UpdateAccessKey` | A user created or activated IAM access key | Always verify with your IT if the access key was expected, especially for privileged users. Leaked access keys are the most common Initial Access vector in AWS |

## **Real-World Example**

The [Permiso blog post](https://permiso.io/blog/s/unmasking-guivil-new-cloud-threat-actor/) shows a great example of how IAM features can be abused both for Initial Access and Persistence. Here, the threat actors accessed the victim's AWS environment via a leaked access key and then created another access key for persistence, in case the first one is deactivated by the IT team. Check out the diagram below or the original blog post for more details:

*Attack lifecycle of the GUI-vil threat group (Permiso)*

## **Key Monitoring Points**

* Monitor root user activity and prevent all attempts to create root access keys
    
* Monitor the learned IAM events, especially the **CreateAccessKey** one
    
* Always communicate with your IT about the potential misconfigurations
    

**Practice**  
You will audit logs of an insecure integration between Splunk and CloudTrail.  
Search for **All Time**, use the **task6** index, and start from the `index=task6` query.

### Answer the questions below

1. Under which ARN does the Splunk integration authenticate?  
    SOC Note: This is an exceptionally insecure configuration!
    
    `index=task6 eventName=createAccessKey`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770814425705/035ddaf0-47ae-484d-af19-42cea015bc25.png align="center")
    
2. When was the over-privileged integration access key created?  
    Answer Example: 2025-12-25 15:30:45
    
    `index=task6 eventName=createAccessKey`
    
    check `eventTime`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1770814441059/f93eb843-754b-419a-b0b8-45fac21cda6d.png align="center")
    

## Conclusion

This room focused on the Initial Access and showed how attackers abuse management console passwords, access keys, and IAM roles to start an attack, and how to detect it with CloudTrail logs. From there, things can branch out in many interesting directions: cloud extortion, cryptomining, and much more. In the next rooms, you will explore more cloud entry points and dive deeper into the following attack stages. Hope you enjoyed the room!

## **Note for Other Clouds**

Both AWS access keys and IAM roles have near 1:1 equivalents in other major cloud providers. In Google Cloud, these map to service account keys and service accounts. In Microsoft Azure, they correspond to service principal credentials and managed identities. You are encouraged to experiment with AWS to understand the high-level ideas, so that applying the same concepts to other clouds becomes much easier.

The room focused on **Initial Access** attacks:

* **Console passwords** → Phishing campaigns
    
* **Access keys** → Leaked credentials (most common breach vector)
    
* **IAM roles** → Compromised workloads inheriting permissions
    

**Attack outcomes covered:**

* S3 data exfiltration and ransomware
    
* Email abuse campaigns
    
* Persistence mechanisms
    

**What's next:** Future rooms will cover:

* More cloud entry points
    
* Post-exploitation stages (lateral movement, privilege escalation)
    
* Other attack types (cryptomining, etc.)
    

---

## **Multi-Cloud Note**

The conclusion mentions AWS concepts have equivalents in other clouds:

| AWS | Google Cloud | Azure |
| --- | --- | --- |
| Access Keys | Service Account Keys | Service Principal Credentials |
| IAM Roles | Service Accounts | Managed Identities |

**Bottom line:** Master AWS IAM → easier to apply concepts to GCP/Azure! 🎯