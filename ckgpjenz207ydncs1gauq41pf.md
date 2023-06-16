---
title: "October's GitHub-Africa Virtual MeetUp"
datePublished: Sat Oct 24 2020 18:31:46 GMT+0000 (Coordinated Universal Time)
cuid: ckgpjenz207ydncs1gauq41pf
slug: octobers-github-africa-virtual-meetup
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1603655716147/8t0qCxm51.png
tags: documentation, meetup, oss, github-actions-1

---

Early this week second [GitHub-Africa MeetUp](https://www.youtube.com/watch?v=QO0qcjC7luQ) was held the virtual event was hosted by [Brian Douglas](https://twitter.com/bdougieYO). The event covered *GitHub Actions, GitHub Open-source tools, OSS documentation, OS for beginners and front-end development roadmap*. Here is some content from the session:-


### Workflow Automation with Github Actions

In the first session, we were taken through by [Shodipo Ayomide](https://twitter.com/developerayo) a GitHub star, Community Evalengist, and Cloudinary media Developer expert. 

*GitHub Actions allow you to*:- 

- write/setup individual action combines them to bring about complete workflow automation.
- Pull Request/Deploy to firebase
- After opening PR what should happen next
- It's helpful with large open-source projects. You can increase productivity by automating everything and make it easier.

*Events that happen in your repo that might trigger automated action*:-

- PR pushed
- issue created 
- new branch created
- deployment etc

*GitHub Actions* has reusable code like how [npmjs](https://www.npmjs.com/) works as a package manager. There's a [GitHub Actions Direcory](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions) you can either use already created or create new actions.

*How GitHub Action works*
```
.github/workflows/main.yaml
``` 
- cache already created actions
- setup environment 
- close state issue 
- visit GitHub Actions market place
> Each action owns their own GitHub repo your can fork and customize the way you want it. e.g [pull-request-action .yml file should be inside](https://github.com/vsoch/pull-request-action)

*[GitHub Actions billing](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions)*

> GitHub is super generous with GitHub Actions commercialized its available with GitHub Free, GitHub Pro, GitHub Free for organizations, GitHub Team, GitHub Enterprise Cloud, GitHub Enterprise Server, and GitHub One.

*GitHub Actions Log*

As a developer you should be able to read a log, GitHub Actions has:
- Search log
- Time token

For more on [Developer Ayo's talk on GitHub Actions you can check here](https://speakerdeck.com/developerayo/automate-workflow-processes-using-github-actions)!

### GitHub Actions and Open Source Tools 

In the second session, we were taken through by [Martins Ewere](https://www.linkedin.com/in/martins-ae/) a Nigerian based in SanFransisco from Github. He started off by explaining the essence of GitHub Africa monthly meetups

> According to [GitHub Octoverse Report](https://octoverse.github.com/), Asia and Africa are leading in worldwide open-source contributions. In Africa Nigeria, Morocco and Kenya are leading among other countries in the continent and they saw it important to bring together the community through the monthly virtual event.

He also covered the open-source tools GitHub Actions, GitHub Mobile, GitHub Free, and GitHub Enterprise Cloud for startups.

*GitHub Actions*

- home for all developers & world's code
- it's elastic to any scale
- fully managed  packages 
- supports all OS for CI/CD
- largest ecosystem which is community-led automation

*GitHub Mobile*

> access your OS project on the go and quickly access your issues and PR on the dashboard.

*GitHub Free*
> its essence was to reduce the barrier for all developers around the world and the realization that many developers needed private repositories. 

*GitHub Enterprise Cloud for startups*

> Project supported by Microsoft. Startups are encouraged to signup for [Microsoft for Startups](https://startups.microsoft.com/en-us/) to access GitHub Enterprise Cloud which ensures to get all talent to collaborate the G.E.C is available for free in all African countries.

### Get Involved in OS as a Beginner in Open Source

In the third session, we were taken through by [Ruth Ikegah](https://twitter.com/IkegahRuth) who is an open-source contributor, Python developer, and technical writer. 

> Open source means anybody is free to use, distribute the project.

*Debunking beliefs among Beginners*

- not just about code designers can also contribute 
- any level of skillset welcome in OS contribution (beginner/ junior/ expert)
- Quality over quantity
- there are amazing benefits that come with contributing to OS i.e *people skills, paid opportunities, internships*, etc

*How to get involved*

- engaging in [HacktoberFest](https://hacktoberfest.digitalocean.com/)
> Find open issues on [use GitHub's search button ''First time only'' "beginner-friendly"](https://github.com/) to tackle
> Write and contribute to project Documentation.

- planning & organizing events/projects 
- answering questions, reviewing code, and helping out in the community.
- supporting a project or open-sourcing your project.

*Resources*

*[GitHub Explore](https://github.com/explore), [Open Source Friday ](https://opensourcefriday.com/), [CodeTriage](https://github.com/codetriage/CodeTriage), [24 Pull Requests](https://24pullrequests.com/), [Up for Grabs](https://up-for-grabs.net/#/), [Contribution-ninja](https://github.com/ninjaframework/ninja/contribute), [First Contributions](https://github.com/firstcontributions/first-contributions/blob/master/README.md), [SourceSort](https://www.producthunt.com/posts/sourcesort), [open source design](https://opensourcedesign.net/)*, etc

### Documenting Your Open Source Software

The fourth session was facilitated by [Shedrack Akintayo](https://twitter.com/coder_blvck) who is a developer advocate at Cloud Foundry Foundation. 


```
          Read the Docs Read Them
``` 
> Documentation is text, illustration, media that gives detailed info about a particular software.

*Forms of Documentation*

- written text like ReadME, release notes
- illustrations i.e roadmaps, graphs
- videos 
- audio
- comments in code

*Importance of Documentation in OSS*

- educate people & keep track of all aspects of the software
- improve the quality of software
- inform people on changes made on the software including versions update i.e release notes.
- inform people on what aspect of the software currently being worked on by the maintainers of the OSS project i.e roadmaps 

*Tips for writing effective OSS Docs*

- define the style guide of your documentation & enforce it
- pick a markup style and stick with it
- focus on using an active voice i.e you /me/ we
- make use of simple & short sentences for effective communication
- make sure format docs properly for easier reading
>including sample code & examples
> use visual devices to represent information i.e illustration, tables, graphs
- Always spell-check your documentation to avoid errors 
- provide contribution guidelines

*Traits of Successful OSS Docs*

- easy access to info regarding core aspects of the software(sidebar)
- well structured
- helps new developers learn to use the OSS quickly
- simplify usage of OSS
- cuts amount of time users of the OSS need support

*Tools for Writing Good OSS Docs*

- [Docusaurus](https://docusaurus.io/) open-source documentation maintained by Facebook.
- [Sphinx](https://www.sphinx-doc.org/en/master/) a tool that makes it easy to create intelligent and beautiful documentation, written by Georg Brandl and licensed under the BSD license.
- [Github & GitHub Pages](https://github.com/github/docs)
 > GitHub readME markdown 
 - [docsify](https://docsify.js.org/#/)
 - [mkdocs](https://www.mkdocs.org/) etc
- [Read THE Docs](https://readthedocs.org/)

*Possible Structure for your Documentation*

- Getting Started
- Architectural Design
- Production Usage guide
- Use Cases i.e links (how they can use the project)
- References
- Roadmap

*Examples of Good OSS Docs*

*[Gatsby Docs](https://www.gatsbyjs.com/), [Vuejs Docs](https://vuejs.org/v2/guide/), [Reactjs Docs](https://reactjs.org/), [Chakra-ui Docs](https://chakra-ui.com/getting-started), [GitHub Open API Docs](https://docs.github.com/en/free-pro-team@latest/rest), [Cloud Foundry Docs experieces on Kubernetes](https://www.cloudfoundry.org/get-started/), [Bootstrap Docs](https://getbootstrap.com/docs/4.5/getting-started/introduction/),* etc 

### Frontend Development [Roadmap](https://roadmap.sh/frontend) 

The last session was facilitated by [Ikram Babs Lawal]() who is a young talented front-end her confidence was something that stood out all through the presentation. She started off by talking about web development that one can either be a Front-end / Back-end or FullStack Developer. Though she'll be focusing on Fron-end.

*Fron-end Web Development*

> converting graphical interface design using HTML, CSS, Bootstrap, or any other CSS framework, JavaScript with its libraries or frameworks.

*Why learn Fron-end Development*

- on-demand skills
- self-employment
- lucrative
- flexibility i.e working remotely
- creative problem solving
> inorder to *build /customize user experience/ interaction on a web application*
- many career opportunities:- increase online presence due to client-user engagement
> freelance
> cooperation/ NGOs
- constantly evolving field 
> constantly interacting with new tools & learning new skills. This keeps you engaged 

*resources*

 *[roadmap](https://roadmap.sh/frontend)*

![Screenshot_2020-10-25 Learn to become a modern frontend developer.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603653907283/uk23wV9GX.png)
*[Compiled Front-end Resources by Denic Marko](https://twitter.com/denicmarko/status/1307997683651678208)*

*Becoming a good Fron-end Developer*

- Determination
- Be willing to learn
- Ready to push self to do more
- code code code more projects
> Personal sites using HTML & CSS
> JavaScript - build quiz game, 
- Read docs 
- Put concepts into practice 
> making projects
> refactoring code
> showing projects to others

This marked the end of the event just to note if you're to speak during the GitHub Africa monthly MeetUp you can reach out to GitHub on Twitter or the OSCA community. [GitHub Universe](https://githubuniverse.com/) will also be hosting its virtual yearly event in December to talk about the features that are being shipped and their 2020 report.

Thanks for reading my article you can my previous articles and comments will highly be appreciated.