Title: Updating and Deploying on Github.
Category: Pelican
Date: 2019-06-17
Author: Gordon Reeder

Last year I showed you how I published my Pelican web site to Netlify. Although I am a big fan of Netlify, I decided to investigate some of the many other hosting options. Because the source material for this web site is hosted in a Github repository, it just made sense to keep everything together and host the site on Github as well. Before I get into the details I want to discuss some of the other options.

### Options

 - **Netlify**. Still a very good option. Free to use. The setup is fairly straight forward and their support staff will help you over any speed bumps you encounter. Deployment is easy because you link to a github repository. To deploy changes you simply push the changes to Github. Netlify is notified of the changes and rebuilds your site. 
 - **AWS**. A popular option. Very low cost for low traffic sites. Setup is easy if you follow an on-line tutorial. But then you are bound to have a few issues that you have to work out yourself. Deploying updates requires visiting the AWS account page.
 - **Microsoft Azure**. The new kid on the block that is threatening AWS. It also is a good option for low traffic web sites. I didn't actually take Azure for a test drive so I have no comment on its usability.
 - **NeoCities**. What Geo Cities should have been. A free option allows you to have one site. But no custom URLs. Super easy setup. A desktop app lets you publish changes directly from your desktop.
 - **Python Anywhere**. Intended to host web sites created from Python frameworks such as Django, Flask, etc. A simple hack lets you host static sites as well. Easy setup. The free option gives you one site.
 - **Github Pages**. A service of Github. Free free free. As many sites as you want. Custom URLs. Don't let Microsoft know what a great service they are offering for free. The setup is easy. Publishing updates is as easy as pushing a commit.



###Updating

It's been a while since I have been here. So I need to update some tools. Open a terminal in the eval-pelican project and activate the virtual environment. Then, update pip:
>pip install -U pip

Next update the pip installed packages:
>pip freeze — local | grep -v ‘^\-e’ | cut -d = -f 1 | xargs -n1 pip install -U

This will generate a lot of output on the screen which can be ignored. When it is finished, the packages installed in the virtual environment should be updated. This includes Pelican which should (as of this date) be V4.0.1 and includes a bug fix by Yours Truly. Now refreeze pip:
>pip freeze > requirements.txt

Fix the pkg-resources bug by opening requirements.txt in a text editor (nano will do) and remove the pkg-resources line.

###Github hosting

First, what is Github pages. We all know that Github is the most popular software repository. But, it can also host web sites. Think about it; Since Github holds lots of files, some of them can be HTML and CSS files. So it isn't hard to run a web server to serve up those files. That is what Github-pages is all about. In fact, Github works so well as a static site host, it has Jekyll support built in. Pfft! If it works with Jekyll it will work with Pelican. I already have an eval-pelican repository setup. It is in this repository that I will host the web site as a project site.

The astute among you will notice that only the site source files are under revision control. The website itself is in the output folder, and that doesn't get pushed to Github. Fortunately, there are others who have gone before us that have solved this problem. The solution is the ghp-import tool. What ghp-import does is to create a gh-pages fork of the repository and then stuff the content of the output folder in it. Github is then told to serve files from the gh-pages fork.

###Publishing to Github

First, I checked the status of the repository and added any updated or new files. With the repository up to date, I committed the changes and pushed to the master branch. Github and my local machine were now in sync.

Then, I used pip to install ghp-import:
>pip install ghp-import

Now the first run of ghp-import:
>ghp-import output

>git push origin gh-pages

Now I go to Github and sign in and notice the project repository now has a gh-pages branch. It was put there by ghp-import. I open the branch and see that it has the contents of the output folder, and nothing else. This is good. There is now an update path from the output folder on my local machine, through the gh-pages branch of the local repository, and into the gh-pages branch of the Github repository.

Time to activate hosting of that branch. Click on settings, scroll down to the Github Pages section. Choose the gh-pages branch from the source pulldown. And I am successful, I see a green bar that shows the URL for my site. Copy that and paste it somewhere.

Trying to visit the URL I see a broken site. That's because the last build of the site wasn't built with Github pages hosting in mind. Let's fix that.

 - On my local machine, edit publishconf.py and add the line: SITEURL = *'the URL I copied from Github'*.

 - Also edit pelicanconf.py and add the line: THEME = 'themes/my_simple'

With these changes made, I add the changes to Git and commit the changes.

With that done, I rebuild the site and publish:
>pelican content -o output -s publishconf.py

>ghp-import output

>git push origin master

>git push origin gh-pages

Because I changed the configuration files, and committed those changes to the master branch, those commits need to be pushed to Github. ghp-import then updates the gh-pages branch with the output folder and that needs to be pushed too. This is good housekeeping that keeps the source files in the master branch in sync with the output in the gh-pages branch.

Now, when I visit the URL I should see my site in all it's glory.



