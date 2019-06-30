Title: Pelican Startup Guide
Category: Pelican
Date: 2018-08-09
Modified: 2018-08-10


Pelican is a static web site generator written in Python. It allows you to write content using simple a markup format. The site it creates is completely static making it easy to host anywhere. Before creating a website, there is some setup and configuration needed to create a proper development environment. The setup is a bit long and may look complicated. That is because I simplify nothing. Not only will I setup Pelican, I will be setting up and using a Python virtual environment, setting up and using Git revision control. Once I get past the setup, Pelican is an easy to use tool. Among other things, it gives me the ability to separate the tasks of site design and layout from content creation. Furthermore, changes to the site design can be made without affecting content, and vice versa.

First off: all this is done on a Linux system. Although things will be similar on a Macbook. If you are a Windows user... uh, dude, If you are going to be a developer you really need to learn to use a real computer. 

In the instructions below, the final web site and project are named eval-pelican. I will use the word *you* a lot as a stand in for your username. IE: /home/*you*/Documents to refer to your Documents folder on your system. Replace *you* with your own username.

I also assume that you know how to open and use a command line terminal. The indented lines below are commands that are entered in a terminal window. The '$' is the prompt and shouldn't be entered. Your prompt string will be different, but it will end with a '$'.

How it all works:

 - First I will install some tools. I'm starting from square one. So I will check the version of Python, install the Python virtual environment and the git version control system.
 - With all the tools installed, I setup the project folder, setup a few basic configuration files and install Pelican.
 - And that's about where i'm going leave it.  In the next part I will use markdown and pelican to create a basic web site with some initial content.
 - After checking the web site on my local machine, I will commit the site to git and push it to Github.
 - Then I'll head over to Netlify and sign up for a free hosting account and link it to Github for continuous deployment.

---
## Getting set up
First install some tools. **Python:** I'm running a Linux system, it should have Python already pre-installed. I Check it by typing:
>$ python --version

It will probably come up with a version of Python2. Python3 is probably also installed. Check for it by typing :
>$ python3 --version

If for some reason you need to install python:
>$ sudo apt-get install python3.6

**pip and virtualenv** Pip is the Python package installer, it should have been installed with Python. Virtualenv is the Python virtual environment tool. It creates a controlled environment on a per project basis that keeps your project dependencies separate from each other.
>$ sudo apt-get install python-pip python-virtualenv

**Git** is a revision control system. **Github** is a web site that works with git. I will use git to push the project to a github repository. Netlify will then use that repository to support their deployment. It's really quite slick. To install Git if you don't have it.
>$ sudo apt install git

The first thing that needs to be done after installing Git is to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits are created:

>$ git config --global user.name "John Doe"

>$ git config --global user.email johndoe@example.com

This needs to be done only once if the --global option is passed, because then Git will always use that information for anything that's done on that system. To override this with a different name or email address for specific projects, run the command without the --global option when you’re in that project.

**Markdown** is a simple text formatting language. It will be used to create content for the web site.  In Ubuntu, you will find the re-text markdown editor in the software center. You could just use your GUI text editor (Mousepad or Gedit), but, re-text provides a nice preview feature and syntax highlighting.  

**Code editor** This specialized editor is needed to create and modify page design files like HTML templates, and CSS style sheets. Although a simple text editor can be used, a code editor provides extra functionality that makes producing code easier. Here are three good ones:
[Gedit](https://wiki.gnome.org/Apps/Gedit#Download), [Sublime Text 3](https://www.sublimetext.com/3), and [Atom](https://atom.io/). Click on the links for download and install instructions.

You will notice that the Pelican site generator tool has not yet been installed. That will happen shortly.

---
## Getting started.
In the Documents or Projects directory, create a folder for the project. Then change into it.
>~/Documents$ mkdir eval-pelican

>$ cd eval-pelican

Create a virtual environment for the project.
>$ virtualenv project

Or you can specify the Python interpreter of your choice (like python3.5.2).
>$ virtualenv -p /usr/bin/python3.5.2 project

*virtualenv project* will create a folder in the current directory which will contain the Python executable files, and a copy of the pip library which you can use to install other packages. The name of the virtual environment (in this case, it was project) can be anything; omitting the name will place the files in the current directory instead, which is not a good thing. 

Now you need to activate the virtualenv to use it:
>$ source project/bin/activate

You will see that the name of the virtual environment is now shown within prenthesis in the command prompt.

Now install the Pelican and Markdown packages as usual. But, instead of being installed as system wide resource they will be installed in the local virtual environment. When installing packages in a virtual environment, don't invoke super user privileges. 
>$ pip install pelican markdown


**Housekeeping**

To keep the environment consistent, it’s a good idea to “freeze” the current state of the environment packages. Do this by creating a requirements.txt file. This file has many uses. But, as far as you are concerned, It is need to tell netlify which packages to load when they build the site. To do this, run:
>$ pip freeze > requirements.txt

Next, create a few housekeeping files.

Use a text editor (not word processor!) to create each of the files below. Add the content or make the edits below and save the files into the project directory.

*ReadMe.md* is not used by any tool in the project. Although Github will want to see it. It is a place for you to keep notes and instructions for others. For now, just put "Hey look! I'm building a web site!" in it.

*.gitignore* is used to tell the git revision control system to ignore certain files. For instance, you don't want to put the virtual environment folder under git control. Add the following lines to .gitignore:
```
output/*
*.py[cod]
project/
develop_server.sh
```
Please notice the 3rd line. It is the name of your virtual environment folder. If you named your environment something else, be sure that you have the correct name there.

*runtime.txt* is used to tell Netlify which version of Python to use when building the site. Netlify currently supports versions 2.7, 3.4, 3.5, and 3.6. Put the version number (just the number) in this file. This number should match the version number of Python in your virtual environment.

*requirements.txt* Already exists. You made it with *pip freeze*. But, if you are using Ubuntu you have to edit this file to deal with a bug. Look for this line in the file: *pkg-resources==0.0.0*. and delete it. 

**git**

If you haven't done so already, head over to github or gitlab and create a user account. Don't worry, it's free. As part of the setup process, github will have you create your first repository. You can choose to make it public or private. Also don't initialize the repository with a ReadMe. When you have created the repository, github will take you to a page with instructions on how to link your local git repository to github.

But first you need to create that local repository. Be sure you are in the project root directory and run:
>$ git init

This creates a kind of 'basement' in your project where old versions of the project will be stored. Now, link it to github.
When you create a repository with github, it will give you a cut-and-paste http link. Use it with the remote add command:

>$ git remote add origin https://github.com/*you*/eval-pelican.git

Everything past the http is the link that Github provided to you right after you created the repository.

**Pelican**

Alright! with all that out of the way it's finally time to run Pelican.
Make sure that the virtual environment is active and you are in the root of the project folder, then run:
>$ pelican-quickstart

Answer the questions in the script that follows. You can mostly just accept the defaults.
This will create a basic skeleton site. You will see several new files and folders have been added to the project.

```
eval_pelican/
├── content
├── output
├── develop_server.sh
├── fabfile.py
├── Makefile
├── pelicanconf.py       # Main settings file
└── publishconf.py       # Settings to use when ready to publish
```

With the initial file structure for the project created, and all the initial housekeeping out of the way, commit everything to revision control. It may seem strange to do that now. After all, you haven't done any actual work. But, by doing it now, you save a copy of the original files. Then, if ever you completely munge up a file, you can reach back into the repository and retrieve the original version. So do that now. First check the status of your files.
>$ git status

Git will spit out a list of files. At this point, all the files are reported as untracked, meaning that git is not keeping track of revisions on any of them (yet). Look at that list. MAKE DAMN SURE THAT YOUR *VIRTUAL ENVIRONMENT FOLDER* IS NOT ON THE LIST. I can not emphasis that enough. If a file is listed in the .gitignore file, git will be ignoring it and it won't appear here. If it does, then git is not ignoring it and you need to correct the .gitignore file. 

Once you are happy with the git status, tell git to start tracking the files. Again, make sure that the files in the *git status* output are all the files, and nothing but the files, that you want to track. Then do:
>$ git add .

Yes, you must include that dot at the end of the command. It means "all the files here". Now, if you do a *git status*, all of the files are listed again, but they are reported as being staged. This means that they are not yet put in the repository, they are half way there. To get them all the way there, do:
>$ git commit -m"This is the initial commit"

Whenever you do a commit, you need to provide a comment about what changes you made since the last commit. That is what the -m"This is the initial commit" is all about. For future commits you will want to change that to something like: "Changed typografy and layout to improve readability" or "Added article Well, A Special Article"

Whew! That's about enough for now. At this point you have installed the necessary development tools. Set up a project under a virtual Python environment. You have created the skeleton of the web site and placed it under revision control. 
At this point a fork in the road has been reached. You can either start creating content, or you can work on the design, layout and style of the web site. Although, Pelican comes with a simple default design.  I'll leave you with some reference information so that you can look deeper into the tools that have been installed. In the next article I'll go into creating content and deploying the site to a web server.

---
## virtualenv reference
virtualenv is a tool to create isolated Python environments. virtualenv creates a folder which contains all the necessary executables to use the packages that a Python project would need.

Create a virtual environment for your project. If you haven't already done so, change into your project's directory.
>$ cd [folder]

>$ virtualenv [environment name]

Or you can specify the Python interpreter of your choice (like python3.5.2).
>$ virtualenv -p /usr/bin/python[v.r] [environment name]

*virtualenv [environment name]* will create a folder in the current directory which will contain the Python executable files, and a copy of the pip library which you can use to install other packages. The name of the virtual environment can be anything; omitting the name will place the files in the current directory instead.

You need to activate the virtualenv to use it:
>$ source [environment name]/bin/activate

You can install packages as usual
>$ pip install pelican Markdown typography

In order to keep your environment consistent, it’s a good idea to “freeze” the current state of the environment packages. To do this, run:
>$ pip freeze > requirements.txt

Requirements.txt is required for the Netlify Continuous Deployment feature. Except that there is a bug in Ubuntu. So open the requirements.txt file in a text editor and remove the line: *pkg-resources==0.0.0*.  it doesn't do anything except mess up Netlify.

Here is a line to add to the Make file so that you have a 'make freeze' command.
```
freeze:
    pip freeze | grep -v "pkg-resources" > requirements.txt
``` 

When you are done working in the virtual environment for the moment, you can deactivate it:
>$ deactivate

If, for some reason, you decide you need to delete the virtual environment, just delete its folder.
>$ rm -rf [environment name]

Visit [This site](https://docs.python-guide.org/dev/virtualenvs/) for more information on using virtual environments.

---
## GIT reference
Git is a version control system that will keep a history of the changes to our project. It allows easy backup, restore, rollbacks, and more. You can also use it to push a copy of our projects to other machines (IE: a web server). Git is needed to use Github. You need Github for GH-pages or Netlify Continuous Deployment.

Install git:
>$ sudo apt install git

### Configure git
The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating:

>$ git config --global user.name "John Doe"

>$ git config --global user.email johndoe@example.com

you need to do this only once if you pass the --global option, because then Git will always use that information for anything you do on that system. If you want to override this with a different name or email address for specific projects, you can run the command without the --global option when you’re in that project.

Linking to github:
When you create a repository with github, it will give you a cut-and-paste http link. Use it with the remote add command:

>$ git remote add [short-name] [site URL]

[site URL] is the link that Github gave you when you created the repository.

### Using git

First, create a text file called .gitignore. Place it in the eval-pelican project folder.
```
#ignore the output folder. Netlify will rebuild the site on their servers from the content folder.
output/*

#ignore python cache etc files.
*.py[cod]

#keep your virtual environment out of the repository
project/ 

#No need to track this.
develop_server.sh
```
Next, turn on git revision control for the project. Be sure you are in the project root directory.
>$ git init

This creates a kind of basement in your project where old versions of the project will be stored.

To add all the files to the repository
>$ git add .

Or specific files:
>$ git add [filespec]

To check the status of the repository.
>$ git status

And finally; to make the changes stick:
>$ git commit -m"A comment about what changes you made"

You can now push the commit to github:
>$ git push origin master

To see your revision history:

>$ git log

This part of the output is called the revision hash: *commit 12df17072473d3d244a02c6397f8d6a8bd29e9cd*. You don't need to enter the entire number in commands that call for the revision hash. Just the first 5 or 6 digits will do.

To retrieve a single file from an old commit, simply use:

>$ git checkout [revision_hash] [file_name]

You can use the HEAD pointer as the [revision_hash] if you want to:

   - HEAD - Points to the Last Commit on the current repository;
   - HEAD^ - Last Commit - 1;
   - HEAD^^ - Last Commit - 2;
   - HEAD~10 - 10 commits behind of HEAD;


Visit [This site](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control) for more information than you will ever need about git

---
## Pelican reference.

If you haven't already done so, Fire up your virtual environment and install pelican:
>$ pip install pelican

Start your project. Make sure that your virtual environment is active and run:
>$ pelican-quickstart

Answer the questions in the script that follows. You can mostly just accept the defaults.
This will create this basic skeleton site.

```
eval_pelican/
├── content
├── output
├── develop_server.sh
├── fabfile.py
├── Makefile
├── pelicanconf.py       # Main settings file
└── publishconf.py       # Settings to use when ready to publish
```
Now create some content using the markdown format. See the Markdown section below. Be sure to save the file with the .md extension. Also, make sure to include some meta tags. Save the document to the content directory.

Now that you have some content. Create the site by running the pelican command, specifying the path to your content:
>$ pelican content

If everything goes well, you can view your site by running a simple HTTP server.
>$ cd output

>$ python -m http.server

Once the basic server has been started, you can preview your site at *http://localhost:8000/*

While the pelican command is the canonical way to generate your site, automation tools can be used to streamline the generation and publication flow. One of the questions asked during the pelican-quickstart process pertains to whether you want to automate site generation and publication. If you answered “yes” to that question, a fabfile.py and Makefile will be generated in the root of your project. These tools “wrap” the pelican command and can simplify the process of generating, previewing, and uploading your site.

If you want to use make to generate your site using the settings in pelicanconf.py, run:
>$ make html

To generate the site for production, using the settings in publishconf.py, run:
>$ make publish

To serve the generated site so it can be previewed in your browser at *http://localhost:8000/*
>$ make serve

