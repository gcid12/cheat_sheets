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

Then you clone your dirty repo:

```	
$ git clone --mirror git://example.com/some-big-repo.git
```	

This wil be a bare repo, you won't see your files, but it have all your history


####3.2 Run BFG 

<code>bfg</code> it's an alias of <code>java -jar bfg.jar</code> both work exactly the same

```	
java -jar bfg.jar --strip-biggest-blobs 500 some-big-repo.git
```	

The BFG will update your commits and all branches and tags so they are clean, but it doesn't physically delete the unwanted stuff. Examine the repo to make sure your history has been updated, and then use the standard git gc command to strip out the unwanted dirty data, which Git will now recognise as surplus to requirements:

```	
$ cd some-big-repo.git
$ git reflog expire --expire=now --all
$ git gc --prune=now --aggressive
```	

Finally, once you're happy with the updated state of your repo, push it back up (note that because your clone command used the <code>--mirror</code> flag, this push will update all refs on your remote server):

```	
$ git push
```	

----
**BFG USAGES:**

Delete all files named 'id_rsa' or 'id_dsa' :

```	
$ bfg --delete-files id_{dsa,rsa}  my-repo.git
```	

Remove all blobs bigger than 1 megabyte :

```	
$ bfg --strip-blobs-bigger-than 1M  my-repo.git
```	


Replace all passwords listed in a file (prefix lines 'regex:' or 'glob:' if required) with ***REMOVED*** wherever they occur in your repository :

```	
$ bfg --replace-text passwords.txt  my-repo.git
```	


In my very particular case,I uploaded sftp access, so I'm trying:

```	
$ java -jar bfg-1.11.7.jar --delete-files sftp-config{.json,-alt.json}  my-repo.git
```	





