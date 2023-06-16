---
title: "Git Workflow"
datePublished: Thu Jan 21 2021 14:45:18 GMT+0000 (Coordinated Universal Time)
cuid: ckk6yw3gf01cw7as13hyr2ea0
slug: git-workflow
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1611240080741/QIIjYEtCu.png
tags: development, workflow, git

---

More often we push our code through our version control, [Git](https://git-scm.com/) to hubs like GitHub or bitbucket which is a "Code Collaboration & Version Control". Personally, I'm conversant with Git and GitHub so I'll be talking about workflow in relation to it. Git-flow observes strict branching.

Gitflow is common or important for purposes:-
- Code review
- development teams(can't push directly to deployment)
- continuous software development
- implementation of DevOps practices

Normally with your GitHub account, you create a new repository and inside the repository, you have a default branch which is always Master/Main unless you create a new branch and make it the default branch. 

So with git workflow, you are advised to only push code to master/main when it's for app releases otherwise you have to create a ``` Development Branch ``` that you will set as your default branch. You also don't push your code directly to the development branch you create ```feature-branch(es)``` then merge it to the ```default branch(development)``` through pull requests when features are complete features are complete.

### Installation & initialitize git-flow
```
install git-flow
git flow init
(git-flow is a wrapper around git)

``` 
### Develop & Master Branch
**master** records release history whereas **development** is the integration branch.


```
git checkout -b develop
git checkout develop
git checkout -b feature_branch
git flow feature start feature_branch
git checkout develop
git merge feature_branch
git flow finish feature_branch
``` 
### Release Branch

Fork the **Release branch** off develop. It's meant to:-
- fix bugs
- documentation generation
- release oriented tasks
Merge **Release branch** into master tagged with version number and also merge to develop for well-defined phases of development.

```
git checkout develop
git  checkout -b release 0.1.0
git flow release start 0.1.0
``` 
### Hotfix Branch
It's a maintenance branch that is based on the **master** not **develop**.  Fork it directly off master. Merge it into both **master**
& **develop**


```
git checkout master
git checkout -b hotfix_branch
git flow hotfix start hotfix_branch
``` 

