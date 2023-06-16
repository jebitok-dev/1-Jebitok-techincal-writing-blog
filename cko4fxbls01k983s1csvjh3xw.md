---
title: "SheCodeAfrica Contributhon x Jenkins"
datePublished: Fri Apr 30 2021 14:57:51 GMT+0000 (Coordinated Universal Time)
cuid: cko4fxbls01k983s1csvjh3xw
slug: shecodeafrica-contributhon-x-jenkins
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1619752274692/5Ow5f74f6.png
tags: opensource, mentorship, jenkins, shecodeafrica

---

Over the past month from the beginning of April I among other 16 mentees got accepted to SheCodeAfrica's Contibuthon the first cohort of Open-source Bootcamp training where mentees are attached to open-source organizations that have partnered with SheCodeAfrica to offer mentors who would guide program mentees through the open-source contribution mostly as first-time contributors or new to open-source. The goal of the program is to introduce mentees to the open-source world how to use git commands, set up projects locally, make changes and create pull requests and also participate in the open-source community.

Personally, I was attached to [Jenkins](https://Jenkins.io), a leading open source automation server, which provides hundreds of plugins to support building, deploying, and automating any project. This was my first time coming across Jenkins and I was so impressed first I was worried since most Jenkins code-base is written using Java so I thought I would have a hard time learning the new Language but all the code or contributions we had to make were based on HTML code within the specific or assigned plugins code-base. 

Basically, over the four weeks, we had two weekly meetings with our mentors and fellow mentees. The meetings were on Mondays and Fridays, on this meetings mentees would share individual progress based on the given tasks on the documentation that was shared at the beginning of the mentorship program that had steps of setting up:- installation ``$ javac version`` | ``$ mvn version``, setup Jenkins locally, running Jenkins on the blue ocean, running simple application of stack like Java & maven, react & node.js among others on the Jenkins on the blue ocean by adding JenkinsFile to your project for continuous delivery/automation.

```
// Jenkinsfile (for React & node.js app)
pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3000:3000' 
        }
    }
     environment {
        CI = 'true' 
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
```
With this JenkinsFile you can use your GitHub repository when creating a pipeline then use [docker to run Jenkins](https://www.jenkins.io/doc/administration/requirements/jenkins-on-java-11/#running-jenkins-with-docker) on blue ocean.
 ### How to setup & contribute to OS 
```
// forked repository
$ git clone (https-or-ssh_key:link-to-the-forked-repository)
$ git checkout -b branch-name
$ git remote add upstream link-to-the-main repo
$ git pull upstream master
$ git commit -m "commit message"
$ git push -u origin HEAD
```
Jenkins is more about the various plugins so we were able to build plugins by forking and cloning already existing plugins then running ``$ mvn -ntp clean -DskipTests verify
`` after which one would create plugin through the manage plugin page on Jenkins by uploading the .hpi file. 

After these steps of familiarizing ourselves with the Jenkins environment, we started contributing to Jenkin's already existing plugins that had issues. Each of us was assigned a plugin(s) depending on issues one covers. Basically one had to identify the plugin that needed help then fork the plugin repository set it up locally then make changes and compile the plugin in order to confirm that the changes reflect on the pipeline syntax. localhost 8080 is the default URL path of Jenkins. 

Over this one-month program, I was unable to make many changes only made one [pull request](https://github.com/jenkinsci/ws-cleanup-plugin/pull/65) that was approved. I would like to treat this opportunity or little experience as an eye-opener to getting into the world of the open-source project as a developer. Over time I'll keep checking open issues on the Jenkins GitHub repository that I can contribute, join their new contributor's channel since we have been added to the Gitter community. Even if I get to contribute to a different open-source project the key takeaway from the program and Jenkins is that OS is more of building communities, asking for help where you're stuck, and making the community's docs or code of better experience to users.

I would like to thank Jenkins for being of great impact during this period. I also like to thank SheCodeAfrica for considering me among other applicants and a successful mentee to undergo this Open-source program that is of great impact to us as Ladies in tech. As much as this is the first cohort of the program I believe it went well and added impact to each of us I'm hoping that the future cohorts will be more than a month to make project contribution more sustainable and of help to newbies. 

Thank you for reading through you can check on [SheCodeAfrica](https://www.shecodeafrica.org/https://www.shecodeafrica.org/) & [Jenkins](https://jenkins.io) and join their communities if you're interested in tech or want to be a part of their mission & programs. You can participate in Jenkins open-source programs through various [Outreach Programs](https://www.jenkins.io/sigs/advocacy-and-outreach/outreach-programs/) like [Google Summer of Code](https://summerofcode.withgoogle.com/), [Outreachy](https://www.outreachy.org/), [Community Bridge](), [Google Season of Docs](https://developers.google.com/season-of-docs), [Hacktoberfest](https://hacktoberfest.digitalocean.com/) only runs in October check GitHub issues marked Hacktober fest or [reach out to Jenkin's community as newcomer contributor](https://gitter.im/jenkinsci/newcomer-contributors)



