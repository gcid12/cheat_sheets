##Gitignoring after commiting sensitive data


So we fucked it up, this is how to repair it.



###0. Change Passwords keys and tokens:
At this point be sure that every password,key and token was compromised. You need to change them all, including your collaborators.

1- Access via command line to your server and change SSH access of root and every other USER

```	
$passwd username
```	

2-Generate new SSH keys and get rid of old ones. Be careful: Don't lock yourself out. The downside of changing your key unnecessarily is now you have to update the authorized_keys file on every machine you might ever log into. That's a bit tedious.

Detailed instructions [here](http://www.democritos.it/activities/IT-MC/documentation/newinterface/pages/change_ssh-key.html)


2- Check your users list

```
$ cat /etc/passwd

$ cat /etc/passwd | cut -d: -f5
```


3- Check your sudo permissions, be sure only you and authorized 

	
```		
visudo
```

4- Check your logs for weird activity




##1. Understand the problem:
1. Locate in Github (exposed repository) those files that have sensitive information. Make a list of those you need to list on the .gitignore and those you need to erase from history.

To check 





---

###2. Stop de bleeding:

Create a .gitignore file in the root of your project, rules are very straigtforward, more info [here](https://github.com/github/gitignore)

to ignore a file literally:

```	
/folder/filename.html
```	
To ignore a folder 

```	
*/foldername
```	
Then you can add exceptions

```	
*/foldername/!keepthis.html
```	

The minimum you should include:

```	
# Logs and databases #
######################
*.log
*.sql
*.sqlite


# OS generated files #
######################
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Sublime access #
######################
/sftp-config-alt.json
/sftp-config.json

```	

###3. Remove from Cache:

Remove from cache to stop showing that change..  more info [here](https://help.github.com/articles/ignoring-files)

```	
git rm --cached fileyouwanttoremove.html
```	

###4. Erase from git history :

But removing from cache is not enough, because if you go back in the versions you can definitely find that sensitive information, so in order to remove the data from all previous versions you can use 

1. The complicated expert way via git-filter-branch : 
2. Or [BFG](http://rtyley.github.io/bfg-repo-cleaner/) who does the job perfectly well : 

So let's do it the BFG way:

####3.1 Download BFG

Download BFG from the website [website](http://rtyley.github.io/bfg-repo-cleaner/)

Change the name of this file from <code>BFG_VERSIONNUMBER.jar</code> to simply <code>bfg.jar</code>

Then clone your dirty ass repo (with mirror option):

```	
$ git clone --mirror git://example.com/some-big-repo.git
```	

This wil be a bare repo, you won't see your files, but it have all your history


####3.2 Run BFG 

Run BFG specifying the file you want to delete:

```	
$ java -jar bfg.jar --delete-files name_of_file  my-repo.git
```	


```	
$ java -jar bfg.jar --delete-files sftp-config.json  my-repo.git
```	

Once finished move inside the .git repo folder 

```	
$ cd my-repo.git
```	

Expire reflog

```	
$ git reflog expire --expire=now --all
```	

Delete everything with GIT GC and PRUNE

```	
$ git gc --prune=now --aggressive
```	

Once you are done push the changes

```	
$ git push
```	




Please check BFG website project [here](http://rtyley.github.io/bfg-repo-cleaner/)
And also to understand <code>git gc</code> and <code>prune</code> go [here](http://gitfu.wordpress.com/2008/04/02/git-gc-cleaning-up-after-yourself/)




