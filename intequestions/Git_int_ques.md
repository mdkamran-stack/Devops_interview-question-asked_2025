## Hope you are familiar with the version controlling tools, can you brief about what branching strategy you're following and what is the purpose of using that branching strategy?
  

We have created our on branching strategies called trunk based startegies where we have main branch bunch of feature brach rlease and hot fix branch. main branch is always updated with latest code.

main branch and bunch of feature branch if any feature is ready we will merge it to main branch and the will create relase branch where QE engineer 
perform testing if testing passed then will create tag on it and perform relase.
if any thing goes wrog we will create hotfix branch push to release branch as well main branch in case any developers working on feature branch will send them notification make sure you take those changes on feature branches.
 
## How do you resolve the merge conflicts in Git?

If there is merge conflict i will setup a call with both the devlopers and to handle the merge conflict coz i dont know the source code i am not a developer.
Resolving the conflict & run CI/CD pipeline on it.and then complete this merge conflict.

## What is git merge vs git rebase

Git merge combines two branches and create a new merge commit preserving full history.  

Git rebase moves your branch commit on top of another branch, creating a cleaner linear histroy.  

## What is the difference between Git fetch and Git pull?
    
git fetch → Only downloads updates, no changes to your working branch.

git pull → Downloads + integrates updates into your current branch.

## What is Git cherry-pick?
git cherry-pick is a Git command that applies a specific commit (or commits) from one branch onto another.

Suppose you have:

feature-branch with commits A, B, C

main branch only needs commit B, not A and C  

## what is git-stash 

git stash lets you temporarily shelve changes you don’t want to commit yet, so you can work on something else and later reapply them.

## 

