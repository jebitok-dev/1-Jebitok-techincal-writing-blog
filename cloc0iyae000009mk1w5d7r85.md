---
title: "Setting up apps and tools after installing a new OS"
datePublished: Sat Oct 28 2023 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cloc0iyae000009mk1w5d7r85
slug: setting-up-apps-and-tools-after-installing-a-new-os
tags: ubuntu, beginners, installation

---

Over the weekend I got assistance to switch to a new Operating System since I have been using Manjaro Linux for over a year now I thought it would be great to try a new one and try out its features.

Since tomorrow is Monday I needed to make sure the system works as normal and I can access everything that I normally need for daily work experience so I had to start installing. I thought I should do an article putting down some basic requirements that I need for my day-to-day experience on my Personal Computer and how to install them in terms of guides or the articles I use over the day.

1. ***Browser***: I already had Mozilla as the available one but I’m a fan of Brave Browser so I had to install it before moving to other basic requirements. The Brave website has an installation guide for Linux and I was targeting [Debian, Ubuntu, and Mint](https://brave.com/linux/#debian-ubuntu-mint), it has some commands that I was able to run on the terminal. After installing it I moved immediately to the new browser then started working from their browser and set the default search engine as Google (I like the search results UI and the user experience is familiar).
    
2. ***Setting up Gmail workspace:*** I had to add my personal email as the default and also accessed its calendar, I always like to bookmark them and show the bookmark below the tabs in order to ease navigation. I also added my work email checked its calendar, and bookmarked both. I had to check Slack through the work email, sign in, and bookmark it too. Tomorrow being Monday anything can go wrong.
    
3. ***Installed Node.js that came with npm***, since I’m a software developer who often interacts with Javascript projects, this is a major prerequisite. From [Nodejs official documentation](https://nodejs.org/en/download/package-manager#debian-and-ubuntu-based-linux-distributions) for installing Node.js via Package Manager on Debian, Ubuntu, and Mint, I was redirected to [Nodesource](https://github.com/nodesource/distributions) under distributions on GitHub and I was able to install it successfully.
    
4. ***NVM***: I used an article on FreeCodeCamp to install it since they had a section with Linux/Mac so it made it easy to pick the command and run on the terminal.
    
5. ***Rust***: I’ve been working on a project that needs Rust installed in order for the program to be successful so I need to install it. I’ve needed mixed reactions installing Rust recently so I checked a couple of websites in order to install it
    
    1. Rust official website with install guides
        
    2. An Article by Digital Ocean under their tutorials on [How to Install Rust on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/install-rust-on-ubuntu-linux).
        
6. ***Grammarly***: As I’m writing this article I realize I have grammar errors “But why isn’t it being highlighted” I’ve been using [Grammarly extension](https://app.grammarly.com/) since 2020 when I joined a program that needed everyone to use the extension to help fix grammar and have you focusing on code and what’s beyond you. I’ve installed it [here](https://chrome.google.com/webstore/detail/grammarly-grammar-checker/kbfnbcaeplbcioakkpcpgfkobkghlhen/related).
    
7. ***Git***: It is a prerequisite for me as a developer to install Git. I used an article by GitHub on [Git Guide: Install Git](https://github.com/git-guides/install-git). I was more interested in commands that would be seamless to install without issues and those that support Ubuntu.
    
8. ***VS Code***: VS code is a code editor by Microsoft that I frequently use so it’s part of the top list of requirements. I checked the VS Code official installation guides and also found an [article: How to Install VS Code on Ubuntu? on Phoenixnap](https://phoenixnap.com/kb/install-vscode-ubuntu) which was more elaborate and worked for me. At first, the official suggested downloading the Debian version but it didn’t work well so I opted for an option for installing using commands.
    
9. ***Setting up*** ***Github through Backup and Sync Setting***: on VS code Github plays an important role in helping backup data from previous VS Code workspace from integrations like Jira/Bitbucket, and Wakatime (I was stressed I would have to look for Wakatime API Key once again), Workspace settings like background colors, extensions, spacing, etc. All this is synced once you click on the settings button &gt; ***Backup and Sync Setting &gt; Sign in with Github or Microsoft &gt; Re-directed to Sign in to GitHub on the web once signed in the sync begins***
    
10. ***Udemy***: used the email invite and then bookmarked it.
    
11. Other websites with dashboards that can be accessed through sent email or credentials etc (can’t give details)
    
12. ***Spotify***: this is helpful during coding. I had to install the Spotify Desktop. I used installation guides given on [Spotify Installation Guidelines for Downloading on Linux](https://www.spotify.com/de-en/download/linux/).
    
13. ***Twitter***, ***Gitlab***, ***Linkedin,*** and ***Notion*** sign-in
    
14. Set up ongoing projects and made sure that it's working well since its sensitive to required dependencies.
    

This took a couple of hours I know there are more browser extensions like Metamask and other dependencies that I will need to install but these felt like a priority for me. The new Ubuntu OS feels new to me as compared to what I’m used to on Manjaro Linux but I will give it a try till I get used to it. One thing I noted was the screenshot tool, ***Spectacle*** that is used on Manjaro Linux I thought it was a universal tool on all Linux OS. The last time I used Ubuntu I used to install a [Screenshot tool extension](https://chrome.google.com/webstore/detail/screenshot-tool-screen-ca/edlifbnjlicfpckhgjhflgkeeibhhcii) but when I switched to Manjaro I didn’t have the extension anymore. I have Installed the tool but still figuring out how to make it the default screenshot taker. I learned more of it on Bard. I will attach it here.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1698616485208/003bfb93-7e0f-46e7-9206-ad4460989a9d.png align="center")

Was more interested in GNOME

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1698616505584/1b672fde-dfe5-4745-8cf6-53da781da79a.png align="center")

How to install the Spectacle Screenshot tool on Linux

```plaintext
$ sudo snap install spectacle 
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1698616533457/81f57239-56d1-47f5-8600-3b3d13d27f47.png align="center")

This was a most random article, it’s been a long since I wrote the previous article and thought I could document this journey. Thank you for reading this article I hope I will be able to write more and engage the technical writing community more. We can connect more on [X formerly Twitter](https://twitter.com/SharonJebitok) or [Linkedin](https://www.linkedin.com/in/sharon-jebitok/)