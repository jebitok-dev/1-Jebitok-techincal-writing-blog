---
title: "Running ESlint among other Linters using GitHub Actions on a JavaScript Project"
datePublished: Sun May 16 2021 19:28:31 GMT+0000 (Coordinated Universal Time)
cuid: ckorkn1dj09q5bvs1hpjv4cnp
slug: running-eslint-among-other-linters-using-github-actions-on-a-javascript-project
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1621193246833/PIuet5x1N.png
tags: javascript, eslint, github-actions-1

---


Linters used on a project vary depending on the programming language you're working on and you can find Linters for most programming Languages for example ESlint for JavaScript, Rubocop for Ruby, and for an HTML & CSS project, you'll use style-lint, Web-hint, and LightHouse. In this article, we will focus on Eslint and other Linters that that is added to a JavaScript project.

Linters are essential when working on various projects they are tools that analyze your source code to flag programming errors, bugs, stylistic errors, and suspicious constructs. They are important to use Linters on your project since they make catching syntax errors more efficient, there's no need to debug simple errors like typos since the Linter does it for you, makes the entire codebase looks like it was written by one person, and helps programmers focus on solving problems instead of cleaning up the code.

There are various ways in which you can use Linters in your workflow i.e 
- *Code Editor Plugin* for example, the extensions on VS Code you'll find ESlint, Web-Hint, Rubocop, among other Linter extensions.

- *GitHub Actions* i.e in the GitHub [market place](https://github.com/marketplace?type=actions) for actions you'll find several Linters and GitHub actions that you can add to your project folder specifically for the Linters. Also some organizations or project documentations can provide for the appropriate Linter and Github action that you'll use for your project.

 ### Linters and Github Actions Usage

Github actions can only work when you create a pull request so ensure you adhere to the git workflow i.e don't your code directly to the main branch either make the development branch the base branch then create feature branches from the development branch so that once you're done working on given features you can create a pull request from ``feature branch to development branch`` once you create a pull request the Github actions automatically run the Linters and incase of Linter errors you'll be able to see either both ESlint & Stylelint run were unsuccessful or only that of ESlint maybe given the hint. For Linters to be successfully used you'll have to fix the Linter errors. Whenever you push code to your branch once you have created a pull request the GitHub actions will always run the Linters and if you have an error there will be a red cross on the specific run and you'll get an automated email notifying you of the Linter errors

GitHub actions automate projects once you create a pull-request so on your project if you want to run the Linters you add a folder called .github inside it you create another folder called workflows and inside the folder ``.githbub/workflows``, you create linters.yml file since we want GitHub actions to run the Linters for a JavaScript project you'll copy the linter.yml code into your project.

````
// .github/workflows/Linters.yml

name: Linters

on: pull_request

env:
  FORCE_COLOR: 1

jobs:
  eslint:
    name: ESLint
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - name: Setup ESLint
        run: |
          npm install --save-dev eslint@7.x eslint-config-airbnb-base@14.x eslint-plugin-import@2.x babel-eslint@10.x
          [ -f .eslintrc.json ] || wget https://raw.githubusercontent.com/microverseinc/linters-config/master/javascript/.eslintrc.json
      - name: ESLint Report
        run: npx eslint .
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
          npm install --save-dev stylelint@13.x stylelint-scss@3.x stylelint-config-standard@21.x stylelint-csstree-validator@1.x
          [ -f .stylelintrc.json ] || wget https://raw.githubusercontent.com/microverseinc/linters-config/master/javascript/.stylelintrc.json
      - name: Stylelint Report
        run: npx stylelint "**/*.{css,scss}"
````
Secondly, we now add the specific Linters that go with programming so mostly for a JavaScript project apart from JavaScript related code i.e vanilla Javascript, Reactjs, Nextjs, TypeScript, Nuxtjs, etc you'll use some styling it could be ordinary CSS, SASS, or SCSS, therefore you'll also have to add Stylelint alongside the ESlint linter file. 

Add this for ESlint to lint JS related code on your JS project and ensure you have installed Eslint locally with related dependencies or run this ``$ npm install --save-dev eslint@7.x eslint-config-airbnb-base@14.x eslint-plugin-import@2.x babel-eslint@10.x``
````
// .eslintrc.json

{
  "env": {
    "browser": true,
    "es6": true,
    "jest": true
  },
  "parser": "babel-eslint",
  "parserOptions": {
    "ecmaVersion": 2018,
    "sourceType": "module"
  },
  "extends": ["airbnb-base"],
  "rules": {
    "no-shadow": "off",
    "no-param-reassign": "off",
    "eol-last": "off"
  },
  "ignorePatterns": [
    "dist/",
    "build/"
  ]
}
````
Add this for Stylelint to lint style-related code on your JS project. Ensure you have installed Stylelint locally to avoid some errors realated to stylelint-config-based you can this ``$ npm install --save-dev stylelint@13.x stylelint-scss@3.x stylelint-config-standard@21.x stylelint-csstree-validator@1.x``

````
// .stylelintrc.json

{
  "extends": ["stylelint-config-standard"],
  "plugins": ["stylelint-scss", "stylelint-csstree-validator"],
  "rules": {
    "at-rule-no-unknown": null,
    "scss/at-rule-no-unknown": true,
    "csstree/validator": true
  },
  "ignoreFiles": ["build/**", "dist/**", "**/reset*.css", "**/bootstrap*.css"]
}
````
From here you can go ahead and work on your JavaScript project after setting up the linter amongst other folder components. Once done working on the given feature or entire project you can go to GitHub on the specific branch you're working from and create a pull request there which GitHub Actions run the Linters. In case of Linter errors, you'll check for hints. For ESlint errors, it could be an indentation, single quote, comma-dangle missing trailing comma errors amongst other errors. Before rectifying the error manually for ESlint you can run the ``npx eslint . --fix`` this will fix some errors automatically after which you can push your code back to GitHub and let the Github Actions run your Linters again incase of more errors you can fix them manually maybe it could be a spelling error or an error that you can easily fix by yourself. In advanced cases, there could be errors that if you're meant to fix they alter the entire codebase, unlike your intention. In that case, identify the lines that have those errors and use this

````
/* eslint-disable */
// lines of code with errors that you don't intend to fix
// since it might alter code base or follows given convention
/* eslint-enable */
```
Incase you have Stylelint related errors use `` npx stylelint "**/*.{css,scss}" --fix`` to fix the errors but mostly for JavaScript project the Stylelint linter will only require to have atleast a line of code on the CSS it won't check much of CSS style conventions like missing closing tags or order of styling. 

I find using Linters on projects as an helpful way to make your project neat and also helps you debug your code. Most organizations and open-source projects use Linters together with github actions so its good to get used to having it on your project workflows alongside the other benefits you get on your individual code base. As programmer you can also customize or come up with your Linters.