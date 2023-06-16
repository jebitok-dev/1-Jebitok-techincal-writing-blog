---
title: "Linters by GitHub Actions"
datePublished: Fri Dec 04 2020 17:07:16 GMT+0000 (Coordinated Universal Time)
cuid: ckiaitj9s01djpqs195ntcz87
slug: linters-by-github-actions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1607101545657/Zq_cDpToL.png
tags: github-actions-1

---

GitHub has introduced [Github Action](https://github.com/features/actions) in a [previous article of Github-Africa meet-up](https://jebitok.hashnode.dev/octobers-github-africa-virtual-meetup-ckgpjenz207ydncs1gauq41pf) I had touched lighted on GitHub Actions and part of the work it does including automation of Pull Requests and easing code reviews, especially in large open-source organizations or projects.

Today want to talk about how to add Linters to your projects in order to ensure that it meets the standards of the language or technology stack you're using, for example, HTML, JavaScript, CSS/SCSS, etc. With GitHub actions, you can add a linters.yml file in your project on the .github/workflows folder. 

When you are working on a feature-branch on GitHub and not directly pushing code directly to the master branch. After you're done with the development you can create a pull request

*.github/workflows/linters.yml*

```
name: Linters

on: pull_request

env:
  FORCE_COLOR: 1

jobs:
  lighthouse:
    name: Lighthouse
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - name: Setup Lighthouse
        run: npm install -g @lhci/cli@0.4.x
      - name: Lighthouse Report
        run: lhci autorun --upload.target=temporary-public-storage --collect.staticDistDir=.
  webhint:
    name: Webhint
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - name: Setup Webhint
        run: |
          npm install --save-dev hint@6.0.x
          [ -f .hintrc ] || wget https://raw.githubusercontent.com/microverseinc/linters-config/master/html-css/.hintrc
      - name: Webhint Report
        run: npx hint --telemetry=off .
  stylelint:
    name: Stylelint
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - name: Setup Stylelint
        run: |
          npm install --save-dev stylelint@13.3.x stylelint-scss@3.17.x stylelint-config-standard@20.0.x stylelint-csstree-validator
          [ -f .stylelintrc.json ] || wget https://raw.githubusercontent.com/microverseinc/linters-config/master/html-css/.stylelintrc.json
      - name: Stylelint Report
        run: npx stylelint "**/*.{css,scss}"
``` 
When you create a new pull request the Linters will run to ensure your code meets standards and you'll either:-
- get checked with a green tick (if all tests pass) meaning you can merge 

![Screenshot_2020-12-04 Write Blog Post.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1607101068026/O8axTip3G.png)
- a red cross if you have Web-hint or Stylelint errors meaning you have to correct them before merging 


![Screenshot_2020-12-04 create the form by jebitok-dev · Pull Request #3 · jebitok-dev hello(1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1607100577037/8RsK702NA.png)

To check the errors click details beside the error check and it will show this 
![Screenshot_2020-12-04 create the form by jebitok-dev · Pull Request #3 · jebitok-dev hello(2).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1607100635526/ViAcOGV9d.png)
you can follow the instructors to make corrections manually on your HTML or CSS file.

Alternatively, you can run on your terminal this will fix all the stylesheet errors

```
npx stylelint "**/*.{css,scss}" --fix
``` 
When all tests pass you can have someone review your code or just go ahead and merge your pull request. 

Thanks for reading my article if interested you can read through previous articles or link up on [Twitter](https://twitter.com/Jsebitok).