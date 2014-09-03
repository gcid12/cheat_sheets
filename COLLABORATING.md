##Collaborating


inside the local repository

```	
$ git remote -v
```	

This will show you something like this:

```	
origin	https://github.com/gcid12/repositoryname.git (fetch)
origin	https://github.com/gcid12/repositoryname.git (push)
```	

Then you want to receive all changes done by other collaborators to the main repository.

```	
git remote add upstream https://github.com/repositorytofollow.git
```	


The again check:

```	
$ git remote -v
```	

It will show:

```	
origin	https://github.com/gcid12/avispa.git (fetch)
origin	https://github.com/gcid12/avispa.git (push)
upstream	https://github.com/MyRing/repositoryname.git (fetch)
upstream	https://github.com/MyRing/repositoryname.git (push)
```	



-------
```	
git fetch upstream
```	



```	
git merge upstream/master
```	



####BRANCHING

to see what branches exist

```	
git branch
```	


to create a branch from a master

```	
git branch branch2
```	

To switch to new branch

```	
git checkout branch2
```	

####MERGE

Checkout Master, and then merge the other branch back:

```	
git merge branch2
```	

Then you can delete it, or keep it alive (A branch is just a temporary workspace, you should delete it and create new ones)

```	
git branch -d branch2
```	

####CREATING A BRANCH BASED ON AN ISSUE

```	
$ git checkout -b iss53
```	

Which is the same as:

```	
$ git branch iss53
$ git checkout iss53
```	



