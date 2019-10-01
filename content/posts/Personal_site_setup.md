Title: I have a personal site!
Category: Pelican
Date: 2019-09-30
Author: Gordon Reeder

Actually, I have had a personal site for quite some time. With the successful deployment of eval-pelican completed I think it is time to re-deploy my personal web site. After all, eval-Pelican is an evaluation of the Pelican static site generator. Although, it seems to have morphed from evaluation, to tutorial, to blog. But web sites are like that. Ultimately my goal is to use Pelican to create, maintain and update my personal web site.

###Background

Let me fill you in on the background of my personal site. In 2001 I became curious about how web sites were built. So, I bought a book and learned about HTML, CSS, and Javascript. Armed with that knowledge I set out to create a web site. And it was a fairly nice web site. All hand crafted in HTML, CSS and Javascript. As you can imagine, without a CMS, making updates and changes was difficult. Usually I would just ignore the site. Then, once a year I would remember it and hack out a update or two. Then I would go back to ignoring it again. Even so, the site did manage to grow to a fairly large number of pages.

Realizing that the current state of affairs was not sustainable, I began to look around at alternatives such as Joomla, Wordpress, and a few other solutions. Over the course of several years tried using PHP to do server side preprocessing of the common page elements (headers, menu bar, etc). I experimented with using Javascript to pull down and page.write the common elements. I even dabled with the Flask and Django Python web frameworks. Although any of this could have worked, there was always something wrong. Either the workflow or design was rigid, pushing updates were unnecessarily difficult, or it required too much actual programming effort.

Then I found out about static site generators. A static site generator takes your content, that you create on your own machine, and creates a stack of static HTML and CSS files. You then dump (or push) them up to a web host. Unlike a web framework like Flask, Rails, or Django, there is no code to deploy or run on the web host. Content is created using a simple word processor or text editor as a Markdown (or ASCIIdoc, ReText, or even HTML) file. The page layout with the common elements is in separate HTML templates. There is no server side database or code to maintain. All of the original content is retained on your local machine. Deployment is a simple git push command. And, you have full creative control. Perfect.

###Let's go

In situations like this it is best to start back at square one. Following my own instructions in the previous articles, I set up a new project, and extracted the page.html and base.html templates from Pelican. I made a couple of quick edits to them.  Then I copied over the CSS file from eval-Pelican. I grabbed 4 articles from my old web site, stripped the HTML, and converted them the Markdown. With the basic project files in place I ran Pelican and checked the result with the local web server. After a few tweaks, I was happy with the results.

OK, to be honest, the next thing I did was to park the thing up on Neo Cities. Since I now had eval-Pelican hosted on gh-pages, it made sense that I should bring my personal site over as well. In an attempt to head off the nastiness with the CNAME file, I  set up Pelican to create the CNAME file on every build. I created the content/extra/ directory and added a CNAME file to it. then edited the CNAME file and put the URL *www.gordonreeder.com* in it. Then I edited publishconf.py to tell Pelican where to find this file and to copy it to the output directory (see previous article).

Continuing to follow the steps in the previous article, I set up a Github repository and configured the local git remotes. Since I don't want to expose the source files for the web site, I made the repository private. FWIW: Github private repositories are great way to backup a project. After creating and pushing the gh-pages branch I hit my first roadblock: Github free doesn't allow web site hosting on private repositories. F! The solution is to create another public repository called *webPublish*. Then configure a remote to it. Now I have a private repo safe from prying eyes for the source, and a public repo for the gh-pages publishing branch.


### Setup the DNS server.

Now it's time to head over to Name Cheap and tell it how to direct traffic to *gordonreeder.com*. The process is exactly the same. First delete the Namecheap default parking page records. Then add the same 4 A records and the CNAME record. The DNS records are exactly the same because they are pointing to Github. Github will use the CNAME file in your repository to figure out the final location to serve the web site from.

Then head back over to Github and remove the Custom Domain setting. Then re-enter it. I'm not sure why, but this has to be done to Github whenever changes are made to the DNS server records. Unfortunately, this means that the CNAME file has been changed any needs to be merged back into the local repository. See the instructions in the previous article. I was hoping to avoid this. 

Well, the process wasn't as smooth as I hoped. In the end I got my personal web page moved up to Github-pages hosting under its custom domain name. It doesn't look all that great. But, we will be addressing that in the next article.  You can see it at *www.gordonreeder.com. 