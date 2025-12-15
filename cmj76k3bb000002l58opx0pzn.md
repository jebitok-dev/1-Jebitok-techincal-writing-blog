---
title: "Containers - DoorDasher's Demise (TryHackMe) 🎉🧑‍🎄"
datePublished: Mon Dec 15 2025 13:19:12 GMT+0000 (Coordinated Universal Time)
cuid: cmj76k3bb000002l58opx0pzn
slug: containers-doordashers-demise-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765803772105/982e0a4f-c71d-4434-bad5-0bd84ab56dbf.png
tags: cybersecurity, devsecops, tryhackme, containersecurity, adventofcyber2025

---

Modern cloud environments rely heavily on containers to deliver fast, scalable services, but this convenience comes with new security challenges. While containers are often perceived as “isolated,” misconfigurations in container runtimes, exposed sockets, or overly permissive privileges can quickly turn isolation into illusion. In real-world breaches, attackers frequently pivot through low-privilege containers, such as monitoring or utility services, to reach more sensitive workloads or even the host system itself.

This challenge mirrors a common cloud security failure: trusting internal services too much. Components like uptime monitors, reverse proxies (such as NGINX), or internal admin containers are often overlooked during hardening because they are not directly user-facing. However, once compromised, these services can act as stepping stones for container escapes, credential harvesting, or supply-chain attacks. Understanding how Docker, container engines, and runtime sockets operate is therefore essential for defenders working in cloud-native environments.

## Introduction

**The Story**

![Task banner for day 1](https://tryhackme-images.s3.amazonaws.com/user-uploads/6228f0d4ca8e57005149c3e3/room-content/6228f0d4ca8e57005149c3e3-1763378735412.png align="left")

It seemed as the sun rose this morning, it had already been decided that today would be another day of chaos in Wareville. At least that’s the feeling all the folks at “DoorDasher” got. DoorDasher is Warevilles local food delivery site, a favourite of the workers in The Best Festival Company, especially on long days when they get home from work and just can’t bring themself to make dinner. We’ve all been there, I’m sure.

Well, one Wareville resident was feeling particularly tired this morning and so decided to order breakfast. Only to find King Malhare and his bandit bunny battalions had seized control of yet another festive favourite. DoorDasher had been completely rebranded as Hopperoo. All of the ware’s favourite dishes had been changed as well. Reports started flooding into the DoorDasher call centre. And not just from customers. The health and safety food org was on the line too, utterly panicked. Apparently, multiple Wareville residents were choking on what turned out to be fragments of Santa’s beard. Wareville authorities were left tangled in confusion today as Hopperoo faced mounting backlash over reports of “culinary impersonation.” Customers across the region claim to have been served what appears to be authentic strands of Santa’s beard in place of traditional noodles.

A spokesperson for the Health & Safety Food Bureau confirmed that several diners required “gentle untangling” and one incident involved a customer “achieving accidental facial hair synchronisation.”

![Beardgate news coverage](https://tryhackme-images.s3.amazonaws.com/user-uploads/6228f0d4ca8e57005149c3e3/room-content/6228f0d4ca8e57005149c3e3-1763634056631.png align="left")

Immediately, one of the security engineers managed to log on and make a script that would restore DoorDasher to its original state, but just before he was able to run it, Sir CarrotBaine caught wind of his attempt and locked him out of the system. All was lost, until the SOC team realised they still had access to the system via their monitoring pod, an uptime checker for the site. Your job? As a SOC team member of DoorDasher, can you escape the container and escalate your privileges so you can finish what your team started and save the site!

## **Learning Objectives**

* Learn how containers and Docker work, including images, layers, and the container engine
    
* Explore Docker runtime concepts (sockets, daemon API) and common container escape/privilege-escalation vectors
    
* Apply these skills to investigate image layers, escape a container, escalate privileges, and restore the DoorDasher service
    
* DO NOT order “Santa's Beard Pasta”
    

## Container Security

## **What Are Containers?**

To understand what a container is, we first need to understand the problem it fixes. Put plainly, modern applications can be quite complex:

* **Installation**: Depending on the environment the application is being installed in, it’s not uncommon to run into “*configuration quirks*” which make the process time-consuming and frustrating. 
    
* **Troubleshooting**: When an application stops working, a lot of time can be wasted determining if it is a problem with the application itself or a problem with the environment it is running in.
    
* **Conflicts**: Sometimes multiple versions of an application need to be run, or perhaps multiple applications which need (for example) different versions of Python to be installed. This can sometimes lead to conflicts, complicating the process further.
    

If reading these problems, your brain conjured up the word “isolation” as a solution, well, you’re onto the right track. Containerisation solves these problems by packing applications, along with their dependencies, in one isolated environment. This package is known as a container. In addition to solving all the above problems, containerisation also offers further benefits. One key benefit is how lightweight they are. To understand this, we will now take a (brief) look at container architecture.

**Containers vs VMs**

To illustrate container architecture, let's examine another form of virtualisation: Virtual Machines. Virtual Machines, like the ones you have been booting up on this platform throughout Advent of Cyber.

![Virtualisation diagram](https://tryhackme-images.s3.amazonaws.com/user-uploads/6228f0d4ca8e57005149c3e3/room-content/6228f0d4ca8e57005149c3e3-1763634135270.png align="left")

A virtual machine runs on a hypervisor (software that emulates and manages multiple operating systems on one physical host). It includes a full guest OS, making it heavier but fully isolated. Containers share the host OS kernel, isolating only applications and their dependencies, which makes them lightweight and fast to start. Virtual machines are ideal for running multiple different operating systems or legacy applications, while containers excel at deploying scalable, portable micro-services.

**Applications at Scale**

Microservices are a switch in the style of application architecture, where before it was a lot more common to deploy apps in a monolithic fashion (built as a single unit, a single code base, usually as a single executable ), more and more companies are choosing to break down their application into parts based on business function. This way, if a specific part of their application receives high traffic loads, they can scale that part without having to scale the entire application. This is where the lightweight nature of containers comes into play, making them incredibly easy to scale to meet increasing demands. They became the go-to choice for this (increasingly popular) architecture, and this is why, over the last 10 years, you have started seeing the term more and more. 

You may have noticed a layer labelled “Container Engine” in the diagram. A container engine is software that builds, runs, and manages containers by leveraging the host system’s OS kernel features like namespaces and cgroups. One example of a container engine is Docker, which we will examine in more detail. Docker is a popular container engine that uses Dockerfiles, simple text scripts defining app environments and dependencies, to build, package, and run applications consistently across different systems. Docker is the container engine of choice at “DoorDasher” and so is what we will be interacting with in our interactive lab. 

**Docker**

Docker is an open-source platform for developers to build, deploy, and manage containers. Containers are executable units of software which package and manage the software and components to run a service. They are pretty lightweight because they isolate the application and use the host OS kernel.

![Present Container](https://tryhackme-images.s3.amazonaws.com/user-uploads/6228f0d4ca8e57005149c3e3/room-content/6228f0d4ca8e57005149c3e3-1763634251845.png align="left")

**Escape Attack & Sockets**

A container escape is a technique that enables code running inside a container to obtain rights or execute on the host kernel (or other containers) beyond its isolated environment (escaping). For example, creating a privileged container with access to the public internet from a test container with no internet access. 

Containers use a client-server setup on the host. The CLI tools act as the client, sending requests to the container daemon, which handles the actual container management and execution. The runtime exposes an API server via Unix sockets (runtime sockets) to handle CLI and daemon traffic. If an attacker can communicate with that socket from inside the container, they can exploit the runtime (this is how we would create the privileged container with internet access, as mentioned in the previous example).

## **Challenge**

Your goal is to investigate the Docker layers and restore the defaced Hopperoo website to its original service: DoorDasher. We are going to walk through the steps to beat this challenge together! 

**Access Points**

Now it's time to start using the machine you booted up in task 1. Let's check which services are running with Docker. Run the following command:

`docker ps`

You should see a few services running:

![docker ps command output](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/61a7523c029d1c004fac97b3-1761169045152.png align="left")

The main service runs at `http://MACHINE_IP:5001`. Explore the web application, and notice the defaced system “Hopperoo”. We know DoorDasher is a food delivery service. Let's explore `uptime-checker`. Sounds interesting.

Run the following command to access the `uptime-checker` container:

`docker exec -it uptime-checker sh`

After logging in, check the socket access by running: `ls -la /var/run/docker.sock`

The Docker documentation mentions that by default, there is a setting called “Enhanced Container Isolation” which blocks containers from mounting the Docker socket to prevent malicious access to the Docker Engine. In some cases, like when running test containers, they need Docker socket access. The socket provides a means to access containers via the API directly. Let's see if we can. Your output should be something like:

![socket permissions](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/61a7523c029d1c004fac97b3-1761169453775.png align="left")

I wonder what happens if we try to run Docker commands inside the container. By running `docker ps` again, we can confirm we can perform Docker commands and interact with the API; in other words, we can perform a Docker Escape attack! 

Let's instead try to access the `deployer` container, which is a privileged container. Run the following command:

`docker exec -it deployer bash`

Type `whoami` and check which user we are currently logged in as. Explore the container and find the flag.

Okay, we made it! We have managed to make it to the container where the script to restore the site to its former version is! Save the day, run the recovery\_script in the root directory to recover the app using sudo:

`sudo /recovery_script.sh`

With that run, you can see for yourself by refreshing the site (`http://MACHINE_IP:5001`)! As a reward, you should be able to find a flag in the same directory as the script (`/`), which you can read using the `cat` command.

### Answer the questions below

1. What exact command lists running Docker containers?
    
2. What file is used to define the instructions for building a Docker image?
    
3. What's the flag?  
      
    `docker ps`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765803909892/346ac306-b096-45a7-ba11-ef74ebe5acfd.png align="center")
    
    `docker exec -it uptime-checker sh`  
      
    `docker exec -it deployer bash`  
      
    `whoami`  
      
    `ls -la`  
      
    `sudo /recovery_script.sh`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765804029517/ba177cc9-f1cf-4a52-9f6d-f9a08c08da97.png align="center")
    
    `ls -la /`  
      
    `cat /flag.txt`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765804044855/8c82ae76-d511-4431-80dc-679a2525b3c3.jpeg align="center")
    
4. Bonus Question: There is a secret code contained within the news site running on port `5002`; this code also happens to be the password for the deployer user! They should definitely change their password. Can you find it?  
      
    I tried running Wareville in the same way we ran the first challenge and found that there were two HTML files, most notably, index.html. The only interesting thing was the alias “Jester, “but I couldn’t find the password openly  
      
    `docker exec -it wareville-times sh   `  
    `whoami`  
      
    `pwd`  
      
    `ls -la`  
      
    `ls /usr/share/nginx/html`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765804213202/41f60460-b06d-446a-af0e-1f09816904a5.png align="center")
    
    `cat /usr/share/nginx/html/index.html`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765804271616/60d4067a-5928-4d39-930d-1e04a9fa9ce8.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765804282029/11011ce2-1f98-4e85-88ae-7be4966561a0.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765804307618/a1ac2614-60da-4a67-92ad-a9d4955aadad.png align="center")
    
    Later checked the actual site and found some bold text which i combined and it passed as the password  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1765804574512/5f2f1784-8e09-4076-852c-fd9170c71739.jpeg align="center")
    
5. Liked the content? We have plenty more where this came from! Try our [Container Vulnerabilities](https://tryhackme.com/room/containervulnerabilitiesDG) room.
    

This exercise demonstrates how container security failures rarely stem from a single vulnerability, but rather from a chain of small oversights. Exposed Docker sockets, privileged containers, and insufficient separation between services allowed a seemingly harmless uptime checker to become a launch point for full system compromise. In cloud deployments, similar mistakes can enable attackers to move laterally across microservices, manipulate container images, or gain control over orchestration layers.

From a defensive perspective, this highlights the importance of least privilege, runtime isolation, and strict access controls around container engines. Services like NGINX may appear benign, but misconfigurations can leak sensitive data or credentials when combined with weak container boundaries. By understanding how attackers exploit these environments, defenders can better secure containerized workloads, reduce blast radius, and prevent minor access from escalating into full infrastructure compromise.