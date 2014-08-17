
#The Golden Path to create a Rails Project from Scratch

#### Because for new comers not everything is obvious.

<br/>
This is a proven way to install a working Rails environment Local and remote. It might debatable if it's the best way but it get you started and gives you a great understanding on how things work. Then yes of course, you might need to tweak it to make it perfect.

<br/>

This guide  It's divided in 2 parts:

1. Setting up Local Development environment
2. Setting up Remote Development environment

Notes:

-Try to follow it in order, In a lot of cases the sequence is important.
<br/>

-Rails Gurus: Pull requests are more than accepted :)
<br/><br/>

------


## PART 1: Setting up Local

<br/><br/>

### XCODE

1. Download Xcode from AppStore
2. Register as Developer
3. Install Developer Tools



### HOMEBREW

```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```



### GIT

```
brew install git  
```
Check version

```    
git --version
```
Define your Name

```
git config --global user.name "Your Actual Name"
```

Your email should be the same as your Github account

```
git config --global user.email "Your Actual Email"
```

Check Config

```
git config -l
```

<br/>
### LOCAL SERVER (Dev)

Server Flavors:

- Apache 1 or 2
- Apache 2 + Passenger / mod_rails
- Nginx
- Lighttpd
- Mongrel
- WEBrick
- Thin

Local Server basic actions:

1. takes your browser request
2. Talk with rail app
3. return results

Reccomended: For Dev the default version WEBrick is fine (no action required), in case you are deploying to Heroku install thin.



<br/>

### RVM & RUBY

```
	\curl -L https://get.rvm.io | bash -s stable
```

close terminal and reopen it, check if everything ok

```
type rvm | head -n 1
```

Now install Ruby

```
rvm use ruby --install --default
```
Check Ruby Version

```
ruby -v
```


### RUBY GEMS

Rails use a lot of gems, rails its itself a Gem
Gems should be pre-installed in your OSX.
	
show version

```
gem -v
```
	
show where it is

```
which gem
```
	
to see what gems you have installed
	
```
gem list
```

to update 

```
gem update —system 
```


### BUNDLER 
(Manage Gems for Rails, keep track of them)

```
gem install bundler
```

When you need to have something that you just installed immediately 

```
rbenv rehash
```

Bundle Version

```
bundle -v
```

Location

```
which bundle
```


###INSTALLING RAILS


Let's install without documentation.

```	
gem install rails --no-ri --no-rdoc
```

Checking version

```
rails --version
```


### NEW RAILS APP:
#### Option 1: Create fresh App
<br/>

Go where the new App will be

```
$cd <location>
```
Create App

```
$ rails new my_app
```

Move inside the just created project (cd sample_app)and run it


```
$ cd my_app
$ rails server
```

Visit the local in your browser

```
http://localhost:3000
```

Later you need to add Gems and Migrate Databases

---

#### Option 2: Clone an app from Github


https://github.com/RailsApps
<br/><br/><br/>

---

###.GITIGNORE

- go to github to find an updated gitignore for your project 
- Create a file called .gitignore in the root of your project
- Paste the content

	https://github.com/github/gitignore
	

### GIT
####and 1st commit

This will happen in your computer, no Github yet

```
git init
```

```
git add .
```

```
git commit -m ‘first commit’
```


###GITHUB 
####Setting Github for your projects

If you have SSH keys already jump to step 3


1- Generate SSH Keys 

[https://help.github.com/articles/generating-ssh-keys](https://help.github.com/articles/generating-ssh-keys)


2-  SetUp Git


[https://help.github.com/articles/set-up-git](https://help.github.com/articles/set-up-git)


3- Set Remote (For this new project)

```
	git remote add origin <remote repository URL> 
```

More info:

[https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line) 
	
---
	
4- Verify Remote

```
git remote -v 
```    

5- Push the changes to GITHUB

```
git push origin master  
```

<br/><br/><br/><br/>

----


## PART 2: Setting up Remote

	
####MISSION: setup all the Remote Environment and give your user credentials
----

<br/>

###CREATE SERVER
- Independently if its DIGITALOCEAN, Rackspace or Heroku you need to define Unix distribution and specific server configuration. 

- For starters UBUNTU is the best option

- They will always send you via mail or on screen the first SSH password. Put attention, they won’t send it again (security reasons.)
	

###ACCESS FOR THE FIRST TIME


Access to Remote as Root for the first time and 

```
$ ssh root@your-ip-here
```

Change password

```
$psswd
```

###CREATE ADMIN

Create Admin and set passwords

```
	$adduser admin
```


Grant Sudo access to Admin
	
```		
	visudo
```
in case you exit root and logged in back as admin (recommended)

```	
	sudo visudo  
```	
	

	#change the following:

	root        ALL=(ALL:ALL) ALL
	newuser    ALL=(ALL:ALL) ALL

<br/><br/>
### PERMITS
Create Group for Admin and give folder permits
<br/><br/>

	
Create New Group

```	
	groupadd foogroup
```	

Assign Group to Folder

```	
	chown -R :foogroup foldername
```	

Check folder permits

```	
	ls -la foldername
```		
	
it will look like this:

```	
	drwxr-xr-x 6 gcid foogroup 4096 Jul 29 23:46 foldername
```	

Change Folder Permits

```	
	chmod -R g+w foldername
```		

Info about an user

```	
id username 
```	


Option1. ADD NON-EXISTENT USER TO A GROUP

```	
	useradd -G developers newusername
```	
<!--useradd -g developers newusername-->

Option2. ADD EXISTENT USERS TO A GROUP

```	
	usermod -g developers username
```	

<br/><br/>

### LOCAL MYSQL


you need to be logged in as root, or someone with access

```	
mysql -u root -p
```	

Create Data Base for the project

```	
mysql> CREATE DATABASE myProject_DB;
```	


Create a user for this project  

```	
CREATE USER 'newuser'@'localhost' IDENTIFIED BY ‘PASSWORD’;
```	
	
Grant privileges to this new user

```	
GRANT ALL PRIVILEGES ON * . * TO ‘newuser'@'localhost';
```	


### SUBLIME TEXT (IDE)
Assuming you have [Sublime Text](http://www.sublimetext.com/) installed , open the folder where your project is and Right click (or CTRL click) to see contextual menu, then look for FTP/SFTP / Remote Mapping

<br/><br/>
Define Remote Mapping:

```	
	"host": " this can be the IP number or the domain in case is ready ",
    	"user": "admin",
    	"password": “XXXX”,  
   	 //"port": "22",
    
    	"remote_path": "/var/www/html",
  ```	
    	

### COMMITING NEW CHANGES
	
	git add .
	git commit -m “Message here”
	git push origin master 








### INSTALL LOCAL MySQL 


check if its installed

```	
$ mysql —version
```	
Install it with Brew

```	
$ brew install mysql
```	

you might need to restart your computer to have it running.. or you can try to restart it (Don’t waste your time, just restart it)

Secure Root Password

```	
mysqladmin -u root password
```	

install set MYSQL2 GEM

```	
gem install mysql2
```		

some handy commands:

```	
	mysql.server start
	mysql.server restart
	mysql.server status
```	


	


###CONFIGURE DB IN RAILS
/config/database.yml

By default you are in SQL Lite, to move to MySQL follow
[this instructions](http://stackoverflow.com/questions/1670154/convert-a-ruby-on-rails-app-from-sqlite-to-mysql)

Just to be sure, regenerate the gems to be sure everything is updated

```	
bundle install 
```	

Use RAKE to connect to DB and retrieve schema… at this point we don’t have any table created,but we know that if it doesn’t mark an error, the connection at least is good.

```	
rake db:schema:dump
```	

Important Note: It might happen that when Bundler was installing MySQL , it didn't got access to install some libraries, if you dont fix that Rake won't be possible, the whole problem is described [here](http://stackoverflow.com/questions/12812613/brew-link-mysql-did-not-complete)

To fix it:

```	
	sudo chown -R $(whoami) /usr/local/lib/
```	
or

```	
	sudo chown -R $(whoami) /usr/local/include
```	

If it's still not working analyze the path it's trying to reach and replace it for one of the previous solutions.
	
	
	
<br/><br/><br/>	
	
----
----
	
		
###OTHER THINGS THAT YOU SHOULD BE DOING:
	
- Changing the port (From 22 to other thing)
- Setting strict Rules to access 
- Creating SSH to access the server without password.
- Setting DNS on your new domain
- Setting GMAIL DNS in case you need to send out mail from your virtual server
- Creating SSL Certificate for your website








	
	
	


	






