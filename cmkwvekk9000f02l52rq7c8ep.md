---
title: "Committed - Git Commits & Logs (TryHackMe)"
datePublished: Tue Jan 27 2026 17:28:42 GMT+0000 (Coordinated Universal Time)
cuid: cmkwvekk9000f02l52rq7c8ep
slug: committed-git-commits-and-logs-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769534912852/f65a1841-ba0d-4c43-add8-9a97562f5cce.png
tags: git, tryhackme, ctf-writeup, commit-history, git-forensics

---

## Introduction

This challenge explores a critical security vulnerability in software development: accidentally committing sensitive credentials to version control systems. The scenario simulates a real-world incident where a developer committed a password (flag) to a Git repository and attempted to cover their tracks by removing it in a subsequent commit.

* **Challenge Category:** Git Forensics / Source Code Analysis
    
* **Skills Required:** Git command-line operations, commit history analysis, and understanding version control
    
* **Key Learning Points:** Git retains complete history even after deletions, examining commit diffs, and the importance of proper secrets management
    

The challenge emphasizes that simply removing sensitive data in a later commit doesn't erase it from Git's history—the information remains accessible to anyone who knows how to examine the repository's commit log.

## Committed

Oh no, not again! One of our developers accidentally committed some sensitive code to our GitHub repository. Well, at least, that is what they told us... the problem is, we don't remember what or where! Can you track down what we accidentally committed?

Access this challenge by deploying the machine attached to this task by pressing the green "Start Machine" button. You will need to use the in-browser view to complete this room. Don't see anything? Press the "Show Split Screen" button at the top of the page.

The files you need are located in **/home/ubuntu/commited** on the VM attached to this task.

Check out similar content on TryHackMe:

* [Git Happens](https://tryhackme.com/room/githappens)
    
* [Git and Crumpets](https://tryhackme.com/room/gitandcrumpets)
    

## Answer the questions below

1. Discover the flag in the repository! `flag{a489a9dbf8eb9d37c6e0cc1a92cda17b}`
    

```bash
git log --all --oneline
28c3621 (HEAD -> master) Finished
4e16af9 (dbint) Reminder Added.
c56c470 Oops
3a8cc16 DB check
6e1ea88 Note added
9ecdc56 Database management features added.
26bcf1a Create database logic added
b0eda7d Connecting to db logic added
441daaa Initial Project.
ubuntu@thm-comitted:~/commited/commited/.git$ git log --all --pretty=format:"%an %ae %cn %ce"
fumenoid fumenoid@gmail.com fumenoid fumenoid@gmail.com
fumenoid fumenoid@gmail.com fumenoid fumenoid@gmail.com
fumenoid fumenoid@gmail.com fumenoid fumenoid@gmail.com
fumenoid fumenoid@gmail.com fumenoid fumenoid@gmail.com
fumenoid fumenoid@gmail.com fumenoid fumenoid@gmail.com
fumenoid fumenoid@gmail.com fumenoid fumenoid@gmail.com
fumenoid fumenoid@gmail.com fumenoid fumenoid@gmail.com
fumenoid fumenoid@gmail.com fumenoid fumenoid@gmail.com
fumenoid fumenoid@gmail.com fumenoid fumenoid@gmail.com
ubuntu@thm-comitted:~/commited/commited/.git$ git reflog
28c3621 (HEAD -> master) HEAD@{0}: checkout: moving from dbint to master
4e16af9 (dbint) HEAD@{1}: checkout: moving from master to dbint
28c3621 (HEAD -> master) HEAD@{2}: commit: Finished
9ecdc56 HEAD@{3}: checkout: moving from dbint to master
4e16af9 (dbint) HEAD@{4}: commit: Reminder Added.
c56c470 HEAD@{5}: commit: Oops
3a8cc16 HEAD@{6}: commit: DB check
6e1ea88 HEAD@{7}: commit: Note added
9ecdc56 HEAD@{8}: checkout: moving from master to dbint
9ecdc56 HEAD@{9}: commit: Database management features added.
26bcf1a HEAD@{10}: commit: Create database logic added
b0eda7d HEAD@{11}: commit: Connecting to db logic added
441daaa HEAD@{12}: commit (initial): Initial Project.
ubuntu@thm-comitted:~/commited/commited/.git$ git branch -a
  dbint
* master
ubuntu@thm-comitted:~/commited/commited/.git$ git checkout dbint
fatal: this operation must be run in a work tree
ubuntu@thm-comitted:~/commited/commited/.git$ cd ../../
ubuntu@thm-comitted:~/commited$ cd commited
git show c56c470
commit c56c470a2a9dfb5cfbd54cd614a9fdb1644412b5
Author: fumenoid <fumenoid@gmail.com>
Date:   Sun Feb 13 00:46:39 2022 -0800

    Oops

diff --git a/main.py b/main.py
index 54d0271..0e1d395 100644
--- a/main.py
+++ b/main.py
@@ -4,7 +4,7 @@ def create_db():
     mydb = mysql.connector.connect(
     host="localhost",
     user="root", # Username Goes Here
-    password="flag{a489a9dbf8eb9d37c6e0cc1a92cda17b}" # Password Goes Here
+    password="" # Password Goes Here
     )
 
     mycursor = mydb.cursor()
@@ -16,7 +16,7 @@ def create_tables():
     mydb = mysql.connector.connect(
     host="localhost",
     user="root", #username Goes here
-    password="flag{a489a9dbf8eb9d37c6e0cc1a92cda17b}", #password Goes here
+    password="", #password Goes here
     database="commited"
     )
 
@@ -29,7 +29,7 @@ def populate_tables():
     mydb = mysql.connector.connect(
     host="localhost",
     user="root",
-    password="flag{a489a9dbf8eb9d37c6e0cc1a92cda17b}",
+    password="",
     database="commited"
     )
 
ubuntu@thm-comitted:~/commited/commited$ 
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769534507896/0919494d-1c18-4c85-97b5-3896d2621ede.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769534528328/5eea8645-4140-4c6b-a537-1e21e7c4cc12.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769534553227/2262d782-5246-4fd4-9329-7004e9189085.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1769534565336/92eb8c0a-95e3-4d5c-b2f1-95285bf06027.png align="center")

## Conclusion

This challenge demonstrated a fundamental security principle: **Git never forgets**. Even though the developer removed the sensitive password in the "Oops" commit (c56c470), the original data remained permanently stored in the repository's history.

**Key Takeaways:**

* Git maintains a complete history of all changes, including deleted content
    
* The `git log` command reveals all commits, and suspicious commit messages like "Oops" are red flags worth investigating
    
* Using `git show <commit-hash>` allows examination of exactly what changed in any commit, including viewing deleted sensitive data
    
* Accidentally committed secrets cannot be fixed by simply deleting them in a new commit—the history must be rewritten or the repository recreated
    
* The `-` symbols in the diff output show what was removed (the flag), while `+` symbols show what was added (empty strings)
    

**Real-World Implications:**  
This is an extremely common security incident. Developers frequently commit API keys, passwords, tokens, and other secrets accidentally. Tools like `git-secrets`, `truffleHog`, and GitHub's secret scanning help prevent and detect these issues. If secrets are committed, proper remediation requires:

1. Rotating/changing the exposed credentials immediately
    
2. Using tools like `git filter-branch` or `BFG Repo-Cleaner` to remove sensitive data from history
    
3. Force-pushing the cleaned repository
    
4. Implementing pre-commit hooks to prevent future incidents
    

**Best Practices:**

* Use environment variables and `.env` files (with `.gitignore`) for sensitive configuration
    
* Implement pre-commit hooks to scan for secrets
    
* Use secret management tools like HashiCorp Vault, AWS Secrets Manager, or Azure Key Vault
    
* Never commit credentials, even temporarily for testing
    

This challenge serves as an important reminder that version control systems are designed to preserve history—a feature that becomes a liability when sensitive data is involved.