---
layout: page
title: How to?
---
# How to?

## index.html
This is the homepage

## Gemfile, Gemfile.lock and poole-for-jekyll.gemspec
These are used by jekyll to build the webpage. Can be safely ignored.

## \_config.yml
The main configuration file.

## about.md
The about page

## atom.xml
File to prove atom feed (like RSS). Programs that keep track of updates to this website will use this feed.

## styles.scss
Imports the .scss files in \_sass to create the final css for the webpage.

## \_includes
Contain header.html and sidebar.html which are the header and the sidebar respectively

## \_layouts
Contains the default page layout default.html. This file uses the files in the \_includes folder to build the default webpage.
Two optional layouts page.html and post.html extend the default layout as necessary.
Whenever a new page of the webpage is creates, it should use one of the above three layouts. 
If it used the page layout then a link appears in the sidebar. If it uses the post layout then it appears on the home webpage. default layout is for the home page.

## \_sass
* **variables.scss** : Defines a few global variables to tweak the theme.
* **base.scss** : The base theme.
* ****