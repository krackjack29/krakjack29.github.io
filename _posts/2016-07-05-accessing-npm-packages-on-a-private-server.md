---
title: Accessing npm packages on a private server
date: 2016/07/05 11:50:00 +530
layout: single
comments: true
categories: 
   - Tech
tags:
   - tools
   - api
   - postman
   - azure
   - cloud
---

[Proget](http://inedo.com/proget) is a universal package manager used to deploy and access private code packages within a company or publicly outside. It is a very useful deployment tool to deliver reusable packages across teams.

If you have a nodejs package in any private repository, execute these commands .

1. “npm config set registry {apiEndPoint}”
2. “npm config strict-ssl false”   — Optional only if the hosted repository is http and not https
3. “npm config set always-auth true”
4. “npm adduser” – Add credentials to connect to the private repository

Once above steps are successful, one can install the package hosted in the server, using “npm install {package-name}”

Happy coding!