---
title: "Hiding API Keys before Committing Code to GitHub and Deploys to Netlify"
datePublished: Sat Oct 03 2020 20:41:22 GMT+0000 (Coordinated Universal Time)
cuid: ckfu5626r06dsu9s12qtz7rdv
slug: hiding-api-keys-before-committing-code-to-github-and-deploys-to-netlify
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1601752885139/zZsHTRV8U.png
tags: apis, reactjs, netlify

---

Over my last two react projects I've had to work with API from  [Google Developer Console](https://console.developers.google.com/apis/). API keys contain sensitive information even when you're going through the implementation sample on [npmjs](https://www.npmjs.com/package/) you're advised not to push the code with API keys to GitHub.

Since I'm using npm mostly I always have to push my code to GitHub especially when ```$ npm start``` doesn't run I don't know the correlation but whenever I have this challenge once I've committed my code to GitHub npm start runs successfully. 

So I wasn't sure where to hide my config folder with the JSON file when I wanted to consider hiding on ```.gitignore``` I wasn't sure if Netlify would deploy well without the API keys on the GitHub repo. 

Luckily I came across the conversation on [Twitter](https://twitter.com/kvncnls/status/1309927434695913472?s=19) and thought I could compile some solutions that work well for other developers: 

- storing the API keys on the  .env, for example, ```API_KEY = 123bc``` then use the variable to refer to the API and add it to .gitignore

- deploy to Netlify you can use [netlify CLI](https://docs.netlify.com/cli/get-started/) or alternatively you can configure the environment variables on Netlify when deploying your projects.

> proxying API keys on lambda/ [Netlify](https://medium.com/frontend-digest/proxying-apis-with-netlify-functions-b95a60061dfc) functions

- use of [vaultproject.io](https://www.vaultproject.io/) which manages secrets and sensitive data using a UI, CLI, or HTTP API.

> It stores all app passwords, API keys and you can retrieve them but its expensive for small projects

- storing environment variables as 'secrets' on GitHub repo.

Interestingly there are GitHub features that will warn you or help you clean the repo when you accidentally commit sensitive keys like [GitGuardian detected an API key from Google ](https://github.com/GitGuardian) that alerted me.

[sops](https://github.com/mozilla/sops) is a great tool to share API with other contributors if you're working as a team.

Hoping this will also be helpful to you. You can share other API key handling tips especially for react projects.