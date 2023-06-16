---
title: "GitHub monthly Africa MeetUp"
datePublished: Wed Oct 14 2020 19:51:34 GMT+0000 (Coordinated Universal Time)
cuid: ckgb99s2j02j1o9s10r9o2wji
slug: github-monthly-africa-meetup
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1602792106142/kDumD2vqZ.png
tags: opensource, meetup, vscode, github-actions-1

---

GitHub just announced its second [Africa Meetup](https://twitter.com/github/status/1316363282807230465?s=19) with African techies that will be hosted on that day among them being [Ruth Ikegah](https://hashnode.com/@ikegah_ruth) open-source advocate

![IMG_20201014_190048.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1602693087673/jz8nDMJSi.jpeg)

Maybe you're wondering what the meetup is all about and if you should attend or not. If you're in the tech space it's a great chance to  Meetup with fellow African techies and to hear opportunities and technologies present for us. Last month GitHub held its first monthly Africa meetup and I was lucky to attend the session. Here are some of the speakers and respective tech topics covered by them:-

 **CodeQL**

Session facilitated by  [Xavier Corail](https://twitter.com/XCorail) from [GitHub Security Lab](https://twitter.com/GHSecurityLab) 

> code the broader pattern to adapt to specific codebases and launch several [CodeQL](https://securitylab.github.com/tools/codeql). Contribute to community queries and get paid. (open-source) 

 **Quick GitHub Packages**

Session facilitated by [@unicodeveloper](https://twitter.com/unicodeveloper)  who is an open-source fanboy, PHP & Laravel community, and shares resources for beginners on his repo.

*Building packages [GitHub Packages](https://docs.github.com/en/free-pro-team@latest/packages/using-github-packages-with-your-projects-ecosystem/configuring-npm-for-use-with-github-packages)*

> are *Software Packages* hosting services that allow you to host your software packages privately/publicly and use packages as dependencies in your project.

*Why release code*

Since everything one place:-
- using your fav package tools eg [npm](https://www.npmjs.com/package/) 
- integration with GitHub actions, GitHub container registry i.e [npm](https://nodejs.org/en/), [Ruby](https://rubygems.org/), [Java](http://maven.apache.org/), [.NET](https://www.nuget.org/)

*Publishing a package*

Access Token
- Go to [GitHub](https://github.com) select settings/tokens/new
- Select scope i.e repo/ write package/ read/ delete
- Give access token a name then copy token to a secure space
Authenticate with GitHub
- Package client of choice eg npm then log in via npm using your GitHub username, password, and email
- configure package client to work with the GitHub package on  package.json file. You can [Read More](https://docs.github.com/en/free-pro-team@latest/packages/using-github-packages-with-your-projects-ecosystem/configuring-npm-for-use-with-github-packages).

 **Vs Code for Remote Development**

Session facilitated by [Lawrence Muthoga](https://twitter.com/larrygify  who talked of development in the context of remote machine/ container and client tools linked to remote services/extensions that makes magic happen like: 
> [Remote WSL](https://code.visualstudio.com/docs/remote/wsl-tutorial) (for linux)

> [Remote SSH](https://code.visualstudio.com/docs/remote/ssh) 
   Raspeberry Pi (Raspbian for vs code )

> [Remote containers](https://code.visualstudio.com/docs/remote/containers-tutorial) development environment like [Code SandBox](https://codesandbox.io/) which avoids impacting local machine config. It also doesn't require the installation of environments like Nodejs or PHP

> [Code Space](https://visualstudio.microsoft.com/services/github-codespaces/) allows you to work beyond PC. On your GitHub open Code on the browser and work on your online.

> [LiveShare](https://visualstudio.microsoft.com/services/live-share/) extension that allows real-time collaboration

You can access related resources [here](https://aka.ms/GHAvscode)!

**Open-Source Terminilogies**

Session by [Samson Goddy](https://twitter.com/Samson_Goddy) co-founder of [Open Source Community Africa](https://twitter.com/oscafrica) and also terms himself as Commissioner of Open-source.

> OS can be read, modified, and shared by others. It's design and code which is publicly accessible.

> Contributing to open-source can be a rewarding way to learn, teach and build experience in just about any skill you can imagine

*Why OS*

- make an impact
- career growth
- people skills
- remote work experience
- paid opportunities like [Outreachy](https://www.outreachy.org/)

> "Nobody did it because everybody thought somebody could do what anybody could have done"

*Create a role*

Either as Designer/ Developer/ Technical writer/ Engangement/ Product manager/ Translator/ User/ Donor/ QA & Tester/ Researcher

*Getting started*

- Find OS project to contribute to  
- Join contributors 
- Make a meaningful contribution (quality > quantity)
        Contribute | Learn | Grow 
 
*OS Opportunities*

[Google Summer of Code](https://summerofcode.withgoogle.com/archive/),
[Outreachy](https://www.outreachy.org/),
[Google Season of Docs](https://developers.google.com/season-of-docs),
[Open-Source Friday](https://opensourcefriday.com/),
[Collections](https://github.com/collections),
[First Contributions](https://firstcontributions.github.io/),
[Up-for-grabs](https://up-for-grabs.net/#/)

> Think of ways to use GitHub to give back to the community. Either by:- 
                code | donation |Technical skills | design 

**Terraform with GitHub Actions**

Session facilitated by [Cobus Bernard](https://twitter.com/cobusbernard) from Cape Town, SA. This session was a whole new topic for me I've never dived in Cloud or microservices but this gave me insights. Here is what was shared:-

*Infrastructure as code*

> create and deploy resources through code 
-  make infrastructure changes
- GitHub/ workflow
> Declarative "I tell you what to do " whereas Imperative, "I tell you what I need  to operate"

*Terraform AWS* 

> creating AWS resources through GitHub actions that can run terraform to create resources in QA, lead, production, etc.
> commit to developing: build resources in production


*Goals*

> make infrastructure changes repeatable and predictable
- release infrastructure changes using similar tools & code changes
- replicate the production environment in staging env
> Use multiple accounts i.e
- user data on development/test/Production

You can Learn more [here](https://learn.hashicorp.com/tutorials/terraform/github-actions)!

Thank you for reading my article. With this, you'll realize that the Github Africa meetup covers all areas of tech and it's a resourceful session for us as developers. Don't miss the next meetup register [here](https://www.meetup.com/GitHub-Africa/events/273641234/)!