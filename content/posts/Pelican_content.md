Title: Creating Pelican Content
Category: Pelican
Date: 2018-08-11

In a previous article I showed you how to set up a basic Pelican static web site generator. If you followed that article, you installed some basic system tools, and setup your project. But, I never got around to explaining how to actually create the web site. In this article I'll rectify that. Here I will go into how to create content (IE: articles, blog posts, etc) for the site.

But wait! What about style sheets, headers, sidebars, menus?  Fortunately, you can ignore all that for now. Pelican has installed a simple startup theme for the site. You can use that for now. Let's get started.

---
## Write an Article
Fire up the re-Text editor installed in part 1. Before you start in on the article, add some Metadata tags to your article:

```
Title: My first Article
Category: General
Date: 2018-08-11
```
Only the title is mandatory. Without it the site build will fail. As you create more and more content for your great web site, your readers will find it hard to find what they are looking for. The category tag will help gather your articles together into meaningful groups. The date tag is formatted as *yyyy-mm-dd*. You can optionally include a time *yyyy-mm-dd hh:mm*.  There are other tags, but this is good for now.

These tags go at the very top of the article. Leave at least one blank line beneath them and begin to wax eloquent about a subject that is near and dear to your heart. As you're writing, refer to the markdown reference section below for information on how to include formatting for your text.

re-Text has a preview button that you can click to check on how your formatting will look, sort of. re-Text doesn't have access to your css files, so it can't format its preview exactly. But it will give you a good indication of the flow.

When you are done, save the file to Pelican's content directory. Then get it under revision control:
>$ git status

Will show you that you have a new file in the content folder. Add it to git and push a commit with:
>$ git add .

>$ git commit -m"Added first article"


Now that you have some content. Create the site. But first, make sure that your virtual environment is running. Then run the pelican command, specifying the path to your content:
>$ pelican content

If everything goes well, you can view your site by running a simple HTTP server.
>$ cd output

>$ python -m http.server

Once the basic server has been started, you can preview your site in your web browser. Startup your browser and put in *http://localhost:8000/* as the URL.

---
## Deploy the site.

This is all nice. But so far only you can view your hard work. To share it with the world you need to deploy it to a web host. In this case I'll show you how to deploy to [Netlify](https://www.netlify.com). The normal way to deploy a static site is to push the contents of the output folder to a web host. Netlify does things a little differently. You push the source to Github then Netlify will pull it in and run Pelican to generate the static files on their servers.

Since Netlify will build the site from github, make sure the git repository is all up to date with:
>$ git status

It should show everything up to date. So push the repository to github:
>$ git push origin master

Now click the Netlify link (above), then click the Get Started for Free button and choose Github as your sign up. Follow the signup procedure by allowing Netlify to access your github repository. When asked, type *pelican content* as the build command. If everything works out, you should see Netlify access your github repository and build your site. 

Did something go wrong? Click on the Deploys tab and select the failed deploy from the list. It will open up a deploy log. Start reading up from the bottom until you find the place where things went haywire. In my case:

 - I had the wrong info in the runtime file
 - There was a bug in the requirements.txt file. I warned you about it in the first article.
 
Google is your friend. Others have been down this road before you and have documented their experience. Don't be afraid to reach out to the Netlify support team. If the site fails to deploy, you will get an e-mail from them. 

If (When) all goes well, go back to the Netlify site overview page and click the Site Settings tag. Look for the Change site name button and give the site a better name.

---
## Workflow

At this point you have a deployed web site, with one article, using the basic Pelican theme. You can continue to add articles to it:

 - Write a new article.
 - Add it to git with *git add*
 - Make a commit with *git commit -m"Some comment"*
 - Push the commit with *git push origin master*

That's it. When you do the *git push*, Github will notify Netlify of the change. Then, Netlify will pull the repository and automagicly add the new content to your site.


### Drafts

There are two ways to deal with content that you are not yet ready to publish.

 - Include the meta-tag *Status: draft* in the document. Pelican will ignore the article when it builds the output.
 - Create a drafts folder in the project root. Only the content folder is processed, so the drafts folder will be ignored.


### Images

To put an image in your post. First you create a directory inside your content directory, where your posts are. Name this directory 'images' for easy reference. Now, you have to tell Pelican to use it. Find the pelicanconf.py file where you configure the system, and add a variable that contains the directory with your images:

> STATIC_PATHS = ['images']

In your post you refer to the image like this:

```
 ![alt text]({filename}/images/IMAGE_NAME.jpg)
```

For external links and images and Embedding Youtube videos, see the Markdown reference below.


---
## Site pages

You can create static site pages such as an About or Contact Us page. Create a directory named 'pages' inside your content directory. Within that directory, you can place markdown formatted text files. For instance; about.md will generate the about.html file. Pelican will automatically place the file on the menu bar.  Add a 'Status: hidden' metadata tag to the document if you want to keep the page off the menu. You would do this for a custom 404 page, for instance.

---
## Markdown reference
### Metadata Tags
The top of your document is a listing of Meta tags. While not part of Markdown, Pelican will look for these tags at the top of each document.  Aside from the title, none of this article metadata is mandatory. However, the date and category tags are strongly recommended.

```
Title: My super title
Date: 2010-12-03 10:20
Modified: 2010-12-05 19:30
Category: Python
Tags: pelican, publishing
Slug: my-super-post
Authors: Alexis Metaireau, Conan Doyle
Summary: Short version for index and feeds

This is the content of my super blog post. Bla Bla Bla.
```


### Headings 

```
# Heading 1 
## Heading 2
### Heading 3
###### All the way to H6
```

### Empisis

```
** bold **

*italicized text*

> Blockquote

~~ Strikethrough ~~
```

### Lists 
```
1. First item
2. Second
3. Third
```
alpha ordered
```
 a. first item
 b. next item
```

Or, unordered
```
- first item
- second item
```


###Links and images
```
Link
[title](https://www.example.com)

Image
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")
![alt text]({filename}/images/IMAGE_NAME.jpg)

Youtube video
[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg)](http://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE)

```

### Code blocks
Code blocks are fenced in by three back ticks

```python
s = python syntax highlighting
print s
```