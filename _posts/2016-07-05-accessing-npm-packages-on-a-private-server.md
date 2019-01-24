title: Accessing npm packages on a private server
link: https://pratapgowda.wordpress.com/2016/07/05/accessing-npm-packages-on-a-private-server/
author: pratapgowda
description: 
post_id: 217
created: 2016/07/05 11:50:00
created_gmt: 2016/07/05 06:20:00
comment_status: open
post_name: accessing-npm-packages-on-a-private-server
status: publish
post_type: post

# Accessing npm packages on a private server

<p><a href="http://inedo.com/proget" target="_blank">Proget</a> is a universal package manager used to deploy and access private code packages within a company or publicly outside. It is a very useful deployment tool to deliver reusable packages across teams. </p> <p>If you have a nodejs package in any private repository, execute these commands .</p> <ol> <li>“npm config set registry {apiEndPoint}”  <li>“npm config strict-ssl false”&nbsp;&nbsp; -- Optional only if the hosted repository is http and not https  <li>“npm config set always-auth true”  <li>“npm adduser” – Add credentials to connect to the private repository</li></ol> <p>Once above steps are successful, one can install the package hosted in the server, using “npm install {package-name}”</p> <p>Happy coding!</p>

## Comments

**[Nagesh Kumar](#301 "2016-07-05 15:04:24"):** Another useful info for modern development environment. Thanks for sharing.

**[Pratap Bhaskar](#302 "2016-07-05 15:31:36"):** Yes, but would be limited to only Nuget packages. ProGet offers a lot of protocols such as Choclatey, NPM, Nuget etc.

**[Nagesh Kumar](#303 "2016-07-05 18:21:38"):** Excellent!

