---
title: "Windows PowerShell"
datePublished: Thu Oct 24 2024 20:40:25 GMT+0000 (Coordinated Universal Time)
cuid: cm2nrpa4b000209kz39zs33ss
slug: windows-powershell
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1729802392758/141dffd8-85b9-4467-b06c-dabef02d6800.png
tags: windows, powershell, cybersecurity-1

---

In this article, I will write a write-up for the Windows PowerShell that covers What Powershell is, Powershell Basics, Navigating the File System and Working with Files, Piping, Filtering and Sorting Data, System and Network information, Real-time System Analysis as well as Scripting.

1. What do we call the advanced approach used to develop PowerShell? `object-oriented`
    
2. How would you retrieve a list of commands that start with the verb `Remove`? \[for the sake of this question, avoid the use of quotes (" or ') in your answer\] `Get-Command -Name Remove*`
    
3. What cmdlet has its traditional counterpart `echo` as an alias? `Write-Output`
    
4. What is the command to retrieve some example usage for the cmdlet `New-LocalUser`? `Get-Help New-LocalUser -examples`
    
5. What cmdlet can you use instead of the traditional Windows command `type`? `Get-Content`
    
6. What PowerShell command would you use to display the content of the "C:\\Users" directory? \[for the sake of this question, avoid the use of quotes (" or ') in your answer\] `Get-ChildItem -Path C:\\Users`
    
7. How many items are displayed by the command described in the previous question? `4`
    
8. How would you retrieve the items in the current directory with size greater than 100? \[for the sake of this question, avoid the use of quotes (" or ') in your answer\] `Get-ChildItem | Where-Object -Property Length -gt 100`  
      
    this will help with breakdowns like `-gt` represents `greater than` then we add the `100` that was mentioned and the `-Property`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729802185678/269717d9-7fab-4bca-aea5-a7bf2f13c93c.png align="center")
    
9. Other than your current user and the default "Administrator" account, what other user is enabled on the target machine? `p1r4t3`
    

First, you have to run the ssh captain@ip\_address Followed by running the PowerShell command which will open up the right path the image below answers questions 9 & 10  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729802139023/a0983027-ac3e-4ed0-8894-ba8c417d56aa.png align="center")

10. This lad has hidden his account among the others with no regard for our beloved captain! What is the motto he has so bluntly put as his account's description? `A merry life and a short one.`
    
11. Now a small challenge to put it all together. This shady lad that we just found hidden among the local users has his own home folder in the "C:\\Users" directory. Can you navigate the filesystem and find the hidden treasure inside this pirate's home? `THM{p34rlInAsh3ll}`
    

you have to `cd p1r4t3` then `ls` followed by `cd hidden-treasure-chest` then follow the rest of the commands  

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729802099622/68695b5e-e75f-40a8-9353-150f99adfb4f.png align="center")

12. In the previous task, you found a marvellous treasure carefully hidden in the target machine. What is the hash of the file that contains it?`71FC5EC11C2497A32F8F08E61399687D90ABE6E204D2964DF589543A613F3E08`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729802071975/8a0123a3-b779-4c73-80d3-dabd5bd73917.png align="center")
    
13. What property retrieved by default by `Get-NetTCPConnection` contains information about the process that has started the connection? `OwningProcess`
    
14. It's time for another small challenge. Some vital service has been installed on this pirate ship to guarantee that the captain can always navigate safely. But something isn't working as expected, and the captain wonders why. Investigating, they find out the truth, at last: the service has been tampered with! The shady lad from before has modified the service `DisplayName` to reflect his very own motto, the same that he put in his user description. With this information and the PowerShell knowledge you have built so far, can you find the service name? `p1r4t3-s-compass`  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1729802047558/3e96c98e-9e6c-44ba-a54c-fe1c5c2e1307.png align="center")
    
15. What is the syntax to execute the command `Get-Service` on a remote computer named "RoyalFortune"? Assume you don't need to provide credentials to establish the connection. \[for the sake of this question, avoid the use of quotes (" or ') in your answer\] `Invoke-Command -ComputerName RoyalFortune -ScriptBlock { Get-Service }`
    

Thank you for reading my article. Please leave any questions or comments on improving my learning journey and the THM challenges. We can also connect more on [**LinkedIn**](https://www.linkedin.com/in/sharon-jebitok) or [**X**](https://x.com/SharonJebitok)**.**