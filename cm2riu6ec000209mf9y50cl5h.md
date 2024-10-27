---
title: "Linux Fundamentals: Linux Fundamentals Part 3 (TryHackMe)"
datePublished: Sun Oct 27 2024 11:43:22 GMT+0000 (Coordinated Universal Time)
cuid: cm2riu6ec000209mf9y50cl5h
slug: linux-fundamentals-linux-fundamentals-part-3-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730029360253/2c6fe70b-bd9e-400d-9bed-5718dfa6b296.png
tags: linux, tryhackme, linux-for-beginners, write-up

---

In this article, I will write a write-up for the Linux Fundamentals Part 3 that covers the Deploy Your Linux Machines, Terminal Text Editors, General/Useful Utilities, and Processes 101, Maintaining Your System: Automation, Maintaining Your System: Package Management, and Maintaining Your System: Logs.

1. Edit "task3" located in "tryhackme"'s home directory using Nano. What is the flag? `THM{TEXT_EDITORS}`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730027402635/4335dcba-96e5-43c4-80ce-8d9cf469db5a.png align="center")
    
2. Ensure you are connected to the deployed instance (MACHINE\_IP)
    

Now, use Python 3's "HTTPServer" module to start a web server in the home directory of the "tryhackme" user on the deployed instance.

Download the file [http://MACHINE\_IP:8000/.flag.txt](http://machine_ip:8000/.flag.txt) onto the TryHackMe AttackBox. Remember, you will need to do this in a new terminal. What are the contents? `THM{WGET_WEBSERVER}`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730027443984/b470786a-781f-498b-9ba6-7b7dddd2d01a.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730028313085/d36b7d1c-57ef-411c-8123-58ce611ce42e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730027535461/1e1c5d06-cf1b-4d8c-8d20-8f8e5777ae4f.png align="center")

3. If we were to launch a process where the previous ID was "300", what would the ID of this new process be? `301`
    
4. If we wanted to **cleanly** kill a process, what signal would we send it? `SIGTERM`
    
5. Locate the process that is running on the deployed instance (MACHINE\_IP). What flag is given? `THM{PROCESSES}`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730027663078/1c5cdc00-73d1-4467-b5e8-d6ed0000437f.png align="center")
    
      
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730028434328/2f626f97-cc98-4adf-8e12-6e04b13e0998.png align="center")
    
6. What command would we use to stop the service "myservice"? `systemctl stop myservice`
    
7. What command would we use to start the same service on the boot-up of the system? `systemctl enable myservice`
    
8. What command would we use to bring a previously backgrounded process back to the foreground? `fg`
    
9. When will the crontab on the deployed instance (MACHINE\_IP) run? `@reboot`
    

using the `crontab -e` crontab -edit it opens this which shows that there’s an ongoing on @reboot process

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730028969097/803010e0-4a3c-4285-838c-da68146296da.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730028898010/b0df3809-2d19-43e6-b967-8a210868a23c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730029109548/746ddf58-e1f4-4eed-9b46-b65530cfb970.png align="center")

  

10. What is the IP address of the user who visited the site? `10.9.232.111`  
    
11. What file did they access? `catsanddogs.jpg`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**