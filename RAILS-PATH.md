###The Golden Path to create a Rails Project from Scratch


----
____


###1. XCODE

1. Download Xcode from AppStore
2. Register as Developer
3. Install Developer Tools

###2. HOMEBREW

```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```
		
###3. GIT

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


###4. SERVER FOR DEVELOPMENT

Options:

- Apache 1 or 2
- Apache 2 + Passenger / mod_rails
- Nginx
- Lighttpd
- Mongrel
- WEBrick

Reccomended: WEBrick

1.takes your browser request
2.Talk with rail app
3.return results



###5. RVM and Ruby

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


###7. RubyGems

	Rails use a lot of gems, rails its itself a Gem
	#Gems should be pre-installed in your OSX
	
	#show version
	gem -v
	
	#show where it is
	which gem
	
	#to see what gems you have installed
	gem list

	#to update 
	gem update —system 


8. Bundler (Managing Gems for Rails, it keep track of them)

	gem install bundler

	#Because you are using RVM you need to rehash whenever something is going to have a command line command and needs to be available.(When you need to have something that you just installed immediately )

	rbenv rehash

	bundle -v
	which bundle


9. Installing Rails

	
	gem install rails --no-ri --no-rdoc
	rails --version


10. Create New Rails Appp
	#Go where the new App will be
	$ rails new sample_app
	# Move inside the just created project (cd sample_app)and run it
	$ rails server
	#Visit the local
	http://localhost:3000

	#Later you need to add Gems and Migrate Databases (jump to step:)


11.ALT  You can also Clone a project from GitHub (Gems ready)
	https://github.com/RailsApps


12. Set .gitIgnore

	go to github to find an update gitignore for your project and paste it on the root of your project.
	https://github.com/github/gitignore

13. SET GIT and 1st commit
	git init
	git add .
	git commit -m ‘first commit’

14. LINK This GIT with Github  (Jump straight to 9.3 for NO first timers)

	#9.1 Generate SSH Keys 
	https://help.github.com/articles/generating-ssh-keys

	#9.2 SetUp Git
	https://help.github.com/articles/set-up-git

	#9.3 Set Remote (For this new project)
	git remote add origin <remote repository URL> 
	MORE INFO: 
	https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line 

	#verifies the remote
	git remote -v     

	#9.4 Push the changes to GITHUB
	git push origin master  


15. Creating Remote
	
	#First you need to setup all the Remote Environment and give your user credentials

	#Independently if its DIGITALOCEAN, Rackspace or Heroku you need to define Unix distribution and specific server configuration. Always choose UBUNTU 
	#They will always send you via mail or on screen the first SSH password. Put attention, they won’t send it again (security reasons.)
	



16. Access to Remote as Root for the first time and change password


	#Change password
	$psswd

17. Create Admin and grant root permits:

	#Create Admin and set passwords
	$adduser admin


	#Grant Sudo access to Admin
	
		
	visudo
	sudo visudo  #in case you exit root and logged in back as admin (recommended)
	

	#change the following:

	root        ALL=(ALL:ALL) ALL
	newuser    ALL=(ALL:ALL) ALL

18. Create Group and give folder permits

	
	#CREATE NEW GROUP

	groupadd foogroup


	#ASSIGN GROUP TO FLDER

	chown -R :foogroup foldername


	#CHECK FOLDER PERMITS

	ls -la foldername
	
	#it will look like this
	drwxr-xr-x 6 gcid foogroup 4096 Jul 29 23:46 foldername


	#CHANGE FOLDER PERMITS

	chmod -R g+w foldername

  	#INFO ABOUT A USER GROUPS

	id username 


	#ADD NON-EXISTENT USER TO A GROUP

	useradd -G developers newusername
	useradd -g developers newusername

	#ADD EXISTENT USERS TO A GROUP

	usermod -g developers username
	usermod -g developers username




19. SET SUBLIME TEXT FOR THE ACTION:

	Define Remote Mapping

	"host": " this can be the IP number or the domain in case is ready ",
    	"user": "admin",
    	"password": “XXXX”,  
   	 //"port": "22",
    
    	"remote_path": "/var/www/html",

20. Committing changes
	
	git add .
	git commit -m “Message here”
	git push origin master 


OTHER THINGS THAT YOU SHOULD BE DOING (SERVER):
	- Changing the port (From 22 to other thing)
	-Setting strict Rules to access 
	-Creating SSH to access the server without password.
	-Setting DNS on your new domain
	-Setting GMAIL DNS in case you need to send out mail from your virtual server
	-Creating SSL Certificate for your website





4. INSTALL LOCAL MySQL (You might have it already, jump to next step)  
	#Download MySQL and install it  (DMG not TAR)
	http://dev.mysql.com/downloads/mysql/


	#You can also do it with homebrew… don’t waste your time

	#check if its installed
	$ mysql —version

	#you might need to restart your computer to have it running.. or you can try to restart it (Don’t waste your time)

	#Secure Root Password
	mysqladmin -u root password

	#install set MYSQL2 GEM!!!!

	gem install mysql2

	#some handy commands
	mysql.server start
	mysql.server restart
	mysql.server status


	
	

21. SET MYSQL UP FOR YOUR RAILS PROJECT



	#Create Database for your project, you need to be logged in as root, or someone with access

	mysql -u root -p

	mysql> CREATE DATABASE test_db01;


	
	#Create a user for this project  

	 CREATE USER 'newuser'@'localhost' IDENTIFIED BY ‘PASSWORD’;
	
	#..and grant privileges

	GRANT ALL PRIVILEGES ON * . * TO ‘newuser'@'localhost';

	[k123l123a123u123n123-h123a123p123p123y123112321233123]


22. CONFIGURE  /config/database.yml

	#By default you are in SQL Lite, to move to MySQL follow this instructions:

	http://stackoverflow.com/questions/1670154/convert-a-ruby-on-rails-app-from-sqlite-to-mysql


	#Use RAKE to connect to DB and retrieve schema… at this point we don’t have any table created,
	#but we know that if it doesn’t mark an error, the connection at least is good.


	rake db:schema:dump


	#IMPORTANT!!!!
	#Before Rake, Run:  
		bundle install 
	#Just to be sure everything is updated…


	#AFTER ALLMOST 2 hours of pulling my hair off, and seeing this message:


gcid$ rake db:schema:dump
rake aborted!
LoadError: dlopen(/Users/gcid/.rvm/gems/ruby-2.1.2/extensions/x86_64-darwin-13/2.1.0-static/mysql2-0.3.16/mysql2/mysql2.bundle, 9): Library not loaded: libmysqlclient.18.dylib
  Referenced from: /Users/gcid/.rvm/gems/ruby-2.1.2/extensions/x86_64-darwin-13/2.1.0-static/mysql2-0.3.16/mysql2/mysql2.bundle
  Reason: image not found - /Users/gcid/.rvm/gems/ruby-2.1.2/extensions/x86_64-darwin-13/2.1.0-static/mysql2-0.3.16/mysql2/mysql2.bundle
/Users/gcid/.rvm/gems/ruby-2.1.2/gems/mysql2-0.3.16/lib/mysql2.rb:8:in `require'
/Users/gcid/.rvm/gems/ruby-2.1.2/gems/mysql2-0.3.16/lib/mysql2.rb:8:in `<top (required)>'
/Users/gcid/.rvm/gems/ruby-2.1.2/gems/bundler-1.7.0/lib/bundler/runtime.rb:76:in `require'
/Users/gcid/.rvm/gems/ruby-2.1.2/gems/bundler-1.7.0/lib/bundler/runtime.rb:76:in `block (2 levels) in require'
/Users/gcid/.rvm/gems/ruby-2.1.2/gems/bundler-1.7.0/lib/bundler/runtime.rb:72:in `each'
/Users/gcid/.rvm/gems/ruby-2.1.2/gems/bundler-1.7.0/lib/bundler/runtime.rb:72:in `block in require'
/Users/gcid/.rvm/gems/ruby-2.1.2/gems/bundler-1.7.0/lib/bundler/runtime.rb:61:in `each'
/Users/gcid/.rvm/gems/ruby-2.1.2/gems/bundler-1.7.0/lib/bundler/runtime.rb:61:in `require'
/Users/gcid/.rvm/gems/ruby-2.1.2/gems/bundler-1.7.0/lib/bundler.rb:133:in `require'
/Users/gcid/Sites/tcb/TCB_Rails/config/application.rb:7:in `<top (required)>'
/Users/gcid/Sites/tcb/TCB_Rails/Rakefile:4:in `<top (required)>'
(See full trace by running task with --trace)



	I realize that the problem was the permissions , so Thanks to an amazing StackOverflow dude I finally solved:

http://stackoverflow.com/questions/12812613/brew-link-mysql-did-not-complete

	He was recommending:  
	sudo chown -R $(whoami) /usr/local/lib/

	Then I tweak it to:

	sudo chown -R $(whoami) /usr/local/include

	And it finally worked. damn I need a beer
	
	
	
	
	----
	
	Relevant Links
	
	More info: http://installrails.com/steps/choose_os_version

	


	






	
	
	


	






