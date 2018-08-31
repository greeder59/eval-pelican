Title: Pelican Themes
Category: Pelican
Date: 2018-08-22

Up until now you have been using the default web design theme that came with Pelican. But, what if you don't like it? Well, you can change it. Changes to the Pelican theme can range from simple tweaks to all out redesign depending on your needs and skill level.

In this article I'll show you three ways to change the default theme.

 - Completely replace the default theme.
 - Make simple style changes to the colors and fonts.
 - Create a whole new theme from scratch.

---
## Housekeeping

Before we begin you need to do some housekeeping.

 - Open your project folder and create a new directory under content named posts. Move your articles there.
 - If you haven't done so already, create a directory named pages in your content directory.
 - And, if  you haven't already done so, create a directory called images in your content directory.
 - Create a folder in the root of your project directory named themes.



---
## Installing a new theme

Start by replacing the default theme. 

 - Head over to [pelican themes](http://www.pelicanthemes.com) and browse through the gallery until you find a theme you like.
 - Click on the link for the theme to get to the Github repro for the theme. 
 - Click the *Clone or Download* button and choose *Download zip*. The zip file will probably end up in your downloads folder.
 - Unzip the downloaded archive and move the resulting folder into your project's *theme* folder. 

Now, rebuild your site with your new theme.

>$ pelican content -t themes/new_theme/

If you like the new look of your site, you can make it permanent by specifying the THEME setting in pelicanconf.py

> THEME = themes/new_theme/

It's that easy.

---
## Modifications

One of the great things about Pelican is the level of creative control you have. Once you have downloaded a theme, it is yours to modify to your hearts content. Or, you can create a new theme from scratch.

Pelican comes with two built in themes. You have been using one already. It is the default theme. It has a name; Notmyidea. If you want to modify it, make a local copy of it in your project/theme folder. You can find the default theme buried in your virtual environment folder at.

>project/env/lib/python3.5/site-packages/pelican/themes/notmyidea/

You will also find another theme named *simple*. I have pulled both *notmyidea* and *simple* into the themes folder of the repository to make it easy to modify or edit. There is also another theme named *css_only*. It is explained in the Pelican documentation section on [Creating themes](http://docs.getpelican.com/en/stable/themes.html).

Teaching you HTML, CSS, and Jinja templates is well beyond the scope of this tutorial. 

 - Go to [W3schools](https://www.w3schools.com/) site for in depth tutorials on HTML and CSS. 
 - The [Jinja](http://jinja.pocoo.org/) web site has extensive documentation on the Jinja template language.
 - Jinja templates are used in other web building tools such as Flask. Be sure to look around for more ideas.


---
## Pelicanconf Modifications

There are a number of things you can do with your site simply by modifying pelicanconf.py.

 - If you are the only author, don't create the authors page. AUTHOR_SAVE_AS = ''
 - Simularly, if you are not using tags. TAG_SAVE_AS = ''
 - While you are experimenting with themes you may see some unusual results. Try: LOAD_CONTENT_CACHE = False DELETE_OUTPUT_DIRECTORY = True
 - If the site is connected to github: GITHUB_URL = 'http://github.com/you/' to make a github ribbon.
 - If you want longer article summaries on your pages: SUMMARY_MAX_LENGTH = 50. Or larger.





