Title: We have a Custom Domain Name!
Category: Pelican
Date: 2019-09-09
Author: Gordon Reeder

In the last article I showed you how I deployed this site to Github pages. It's easy and it works. But, oh boy, the URL! I just want to be able to go to *www.eval-pelican.com*, not *username.github.io/eval-pelican*. Turns out I can. Git hub pages supports the use of custom domains. Even at the free level. It can be a bit confusing to set up because some of the procedures have changed over the years. So make sure you are reading the most up to date documentation. Also, you will have to make setups on two servers; Github and your domain name registrar. Once you get sorted out what changes go where, it all goes quite quickly.
But first you need to get Github sorted out. Since eval-pelican is project page I can forget about the user/organization page options.  Github supports the use of custom apex domains such as *my-domain.com*, and sub domains such as *www.my-domain.com* or *blog.my-domain.com*. In this case, like most web sites, I'll use the www sub domain.

The basic steps are as follows:

 - Buy a custom domain name from a domain registrar.
 - Point the domain nameserver at Github.
 - Tell Github about the custom domain name.
 - Adjust your project to manage the CNAME file. More about this later.

Let's get started.

###Register a domain name

Step one is to register a custom domain name with a domain registrar. I used Name Cheap, mostly because I have other domain names registered there. As a bonus, they provide the advanced DNS settings that Github hosting requires. 

Now that *eval-pelican.com* is mine, I need to head over to Github and tell them about it. On GitHub, navigate to the GitHub Pages site's repository.

Under your repository name, click Settings on the upper menu bar. 

Once there, scroll down to the 'Custom domain" box. Enter the domain name in there; IE: *www.eval-pelican.com*. Then click the save button to make it stick. Github will display a yellow warning box. This is OK, it will be fixed later. I have to enter the custom domain at this time to prevent the hosting from being hijacked.

### Setup the DNS server.

Now I'll head back over to Name Cheap and tell it how to direct traffic to eval-pelican. 

On the dashboard, find the line for the new custom domain name. Click the manage button. Then click on Advanced DNS.

There are two DNS records that point to Name Cheap's default parking page for the domain. I don't need this, so I just delete those two records.

Add a CNAME record (click Add New Record and choose the CNAME record type). In the host column, enter www. In the value column, enter the old *username.github.io* gh-pages domain name. Set the TTL (Time To Live) column to 30 min.

In a similar fashion, add four A records as follows:

```
    Host,  Value, TTL
     @,  185.199.108.153,  30
     @,  185.199.109.153,  Automatic
     @,  185.199.110.153,  Automatic
     @,  185.199.111.153,  Automatic
```

Save the changes and exit.

Then I head back over to Github and remove the Custom Domain setting. Then re-enter it. I'm not sure why, but this has to be done to Github whenever changes are made to the DNS server records.

Test to see if it all works by typing the new domain name into a web browser and see if the site comes up. If it doesn't seem to work at first, just give it time. It can take several hours for DNS changes to propagate. Although, in my case, it only took about 10 minutes.

### Managing the CNAME file.

At this point you have a working custom domain. But, a quick glance at the gh-pages branch of the site repository, reveals that a CNAME file has been added. It was put there by Github when the Custom domain setting was entered. And it needs to stay there. There are two problems with it though: First: it runs counter to the work flow. That is: Pelican creates the output, ghp-import places the output in the gh-pages git branch, Git push sends the branch to Github. Second: If you try to update your site now, git will have a hissy fit about the CNAME file. It represents a change that GIT doesn't know about locally. To resolve this we need to do two things:

 - merge the remote CNAME file to the local git repro.
 - modify the project to allow Pelican to create the CNAME file with each build.

First the merge. Switch to the local gh-pages branch then pull down the remote branch:

> $ git checkout gh-pages

> $ git pull origin gh-pages

Then switch back to the master branch

> $ git checkout master

To set up Pelican to create the CNAME file on every build.
Create the content/extra/ directory and add a CNAME file to it. Edit the CNAME file and enter the URL of your site IE: *www.eval-pelican.com*.

Now tell Pelican where to find this file and to copy it to the output directory by adding the following lines to publishconf.py:

```
	STATIC_PATHS = ['images', 'extra/CNAME']
	EXTRA_PATH_METADATA = {'extra/CNAME': {'path': 'CNAME'},}
```
And that should be it. Commit the changes to the project, use ghp-import to rebuild the gh-pages branch. Verify that the CNAME file is in the gh-pages branch and it contains the domain name