##GIT 



UPDATING FORK



Assuming you want to update the master branch, check it out

```	
git checkout master
```	

Add a remote for the upstream repository


```	
git remote add upstream https://github.com/jekyll/jekyll.git
```	

Fetch everything from that upstream repository:

```	
git fetch upstream
```	

Merge all the changes from upstream’s master to your own:

```	
git merge upstream/master
```	

Your local fork of the repository should now be update with the latest changes from the original upstream repository.

---


####RESOLVING CONFLICTS:

In case you have 2 conflicted files Git will let you know where is located:

```	
On branch branch-b
You have unmerged paths.
(fix conflicts and run "git commit")
```	

It will also modify the "Conflicted" file , it will add some lines.

```
the number of planets are
<<<<<<< HEAD
nine
=======
eight
>>>>>>> branch-a
```

Simply go to the file, choose one of the options, save, add and change the file.

```
git add .
git commit -m "Conflict resolved"
```

Done!