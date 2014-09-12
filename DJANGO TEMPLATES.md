CREATING DJANGO TEMPLATES



Create a “templates” directory in your project directory

```	
mkdir templates
```	



Modify ```	mysyte/setting.py```	  to add the template directory

```	
TEMPLATE_DIRS = [os.path.join(BASE_DIR, 'templates’)]
```	

Create a directory called admin inside templates

```	
mkdir admin
```	

