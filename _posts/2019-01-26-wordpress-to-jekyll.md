---
title: Wordpress to Jekyll
layout: single
comments: true
date : 2019/01/26
categories:
    - blogs
tags:
    - github
    - pages
    - jekyll
    - wordpress
---

I have been blogging on WordPress platform since 2010 (even though I wish to write more), I had always found it very difficult to share code, considering most of my blogs are technical and need code snippets to explain better. I used [Open Live Writer](http://openlivewriter.org/) to write the blogs offline and publish them to [wordpress](https://www.pratapgowda.wordpress.com). 

Since 2013 all of my projects have been on git (Github, Gitlab, Bitbucket and even TFS Git) and all the necessary documentation has been using the markdown files. Either because of the simplicity of markdown files or the inner developer in me, I am most comfortable writing documents using markdown. So I started looking for a way to write my blogs using markdown files, there were a couple of plug-ins available for WordPress, but plugins are only available for a business plan.

I had earlier heard about [github-pages](https://pages.github.com/) which allow you to host static websites using your github account and for free, yes a** highly available static website for free**. All one has to do is create a repository with your git-user name. 

In the same line of googling, I came across a gem (literally a ruby-gem) called [Jekyll](https://jekyllrb.com/) which is a ***simple, open-source, customizable, blogging platform*** that can be hosted using github pages for free. 

Jekyll has a lot of open-source themes to build you websites and some of them are [officially supported](https://pages.github.com/themes/) by github too. As cool as they are, they weren't really what i was looking and then I found ["minimal mistakes"](https://github.com/mmistakes/minimal-mistakes) theme for Jekyll.

I am hereby sharing my journey of moving my blogs from a free wordpress account to another free for all github-pages using Jekyll.

## Setup github pages repository
* Create a github repository with name as username.github.io, in my case repository is [pratap-dotnet.github.io](https://github.com/pratap-dotnet/pratap-dotnet.github.io)
* Clone it to your local system

## Setup Jekyll locally
* Linux based systems are best for executing jekyll, I chose WSL (windows subsystem for linux) works fine on both.
* Step by step installation guide in the official Jekyl [docs](https://jekyllrb.com/docs/installation/windows/).
* Jekyll has a specific [folder](https://jekyllrb.com/docs/structure/) structure and mostly runs on configurations in _config.yml file.

## Backup and convert old posts to markdown files
* [Wordpress](https://en.support.wordpress.com/export/) has export functionality, which allows you to download all the posts and comments ever written into an XML file. 
* I used [wordpress to markdown exporter](https://github.com/dreikanter/wp2md) to convert the wordpress exported content to markdown files.

## Install minimal mistakes theme
* In the _config.yml file select "**theme**" value as **"minimal-mistakes-jekyll"**
* Setup disqus account by specifying disqus as the comments/provider and shortname from disqus account. Also one needs to enable comments in the blog using "comments: true" section in the post induvidually.
* Enable google search as the default search provider, by registering the website in cse.google.com
* Enable google analytics by just setting up the tracking id. 
* Posts are written in mark down file in _posts folder.

To run and test the website locally, one has to run the command 
```bash
bundle exec jekyll serve
```
and access the website at http://localhost:4000

## Missing functionality
Ofcourse there are some limitations,
* Integration with social network - Wordpress would automatically post the latest blogpost url to the social network. I am using IFTTT for now :blush:
* Anything goes wrong, you are on your own. Not neccessarily a limitation for developers who can understand whats going on underneath.
* No Follow me or subscribe to the blog button by default. 

Setting up this entire thing and migrating all the old posts took me about a day's work. Worth IT!!!

Happy Coding!!!!