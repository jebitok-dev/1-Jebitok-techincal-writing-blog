---
title: "Deploying your Webpages with Netlify"
datePublished: Sun Oct 18 2020 01:48:16 GMT+0000 (Coordinated Universal Time)
cuid: ckghd3c8h01fu5os1a65nhutz
slug: deploying-your-webpages-with-netlify
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1603161689989/Ccuhp8zJl.png
tags: github, deployment, authorization, netlify

---

As you continue to learn and grow your career into Software Development you tend to work on projects using HTML5, CSS, and its frameworks together with JavaScript either for front-end with React and other of its frameworks or for back-end with Nodejs with its framework and either serverless DB like MongoDB or SQL like PostgreSQL. 

At most times you build a web application that needed to be hosted on live pages. Most people prefer using GitHub pages since it's beginner-friendly and can host static web pages but you might also consider using Netlify to deploy your web pages. 

When using Netlify you can deploy with authorization through your GitHub repository or use the drag and drop way.

*Using GitHub repository*

First, it's easier to create an account on Netlify if you have got an account on GitHub you authorize for it. 
Visit the page with sites or create a new site option then you'll choose the GitHub option then choose the repository and the branch that has the code that needs to be deployed. This allows for continuous deployment.

*Drag and drop*

You just search for Netlify drag and drop on your browser then there's an active area on the page that you'll drag and drop the folder that will be deployed from your local folders on PC then Setup deploy config. Changes will be made when you update through another drag and drop no continuous deployment.

**Deploying React Applications through Netlify**

At times when you deploy through Netlify, you might end up with a 'Page Not Found' error. The best way to overcome this when dealing with create-react-app :
 
- *Netlify CLI*
install create-react-app then set up and install Netlify CLI

```
npx create-react-app react1
cd react1
npm run build

npm install netlify-cli -g
netlify deploy
```
- *Using index.toml file*
Add index.toml file on your build folder. The file should look like this;

```
/*    /index.html   200
``` 
During deploy Netlify checks for index.html and at times that file might be part of the /dist on .gitignore. So using either of this will help ease deploy when determining the publish folder.


