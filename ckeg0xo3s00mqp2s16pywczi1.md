---
title: "Git bash commands"
datePublished: Sat Aug 29 2020 18:54:24 GMT+0000 (Coordinated Universal Time)
cuid: ckeg0xo3s00mqp2s16pywczi1
slug: git-bash-commands-1
tags: github

---

Since I started learning software development GitHub has been the best tool that I've understood to maneuver around I remember the first month I would create a repo but my code wouldn't commit successfully onetime I accidentally deleted an existing folder on my PC since I thought 
```
git rm -rf 
``` 
would make the wrong commit reappear on my repository successfully. 

> 
Moreover, there's a debate by experienced developers on whether or not to go by daily commits to GitHub. 

For me as a developer who is still actively learning, I consider daily commit as a tool to boost consistency and help as proof of work on the projects that you've worked on and considering the fact that most hosting/deployment sites have integrated to GitHub to ease hosting and continuous deployment when new changes are made to the code and pushed to GitHub. 

Here are the main git bash commands:

``` 
$ git init 
``` 
initialized new repository

```
$ git status
``` 
to check changes made to folders or files that have not been added or committed to the GitHub repository

```
$ git add .
``` 
adds all the changes that had not been added making them ready for commit

```
$ git commit -m "commit message"
``` 
commits code that will be pushed. Commit message is helpful for reference and in case of pull request the code reviewer will easily know the feature that was made or changes that was done on the committed code.


```
$ git remote add origin <Link to your GitHub repo>
``` 
the commits will be delivered to the repo <Link to your GitHub repo> that you created after you have pushed


```
$ git checkout -b <branchName eg: develop>
```
this creates a new branch that you'll be working from.

> 
it's advisable to push code that is ready for deployment or clean code to GitHub master branch 

```
$ git push -u origin master
                or
$ git push -u origin <branchName eg: develop>
                or
$git push --no-verify

``` 
alternative ways of pushing your code to GitHub.


