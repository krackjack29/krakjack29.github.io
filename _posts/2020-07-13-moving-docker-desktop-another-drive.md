---
title: 'Moving docker contents to another drive'
layout: single
comments: true
date : 2020/07/13
categories:
    - tools
tags:
    - docker
    - containers
    - tools
---

Docker desktop is now available with WSL integration, provided that you are on windows 10 1909 build, it brings in a lot of performance improvements to the entire WSL ecospace. Now, one of the problems with docker desktop is that it would install in the C drive and when you pull the containers they would also be stored in the same drive. I have limited space on C drive to begin with, so after a bit of research found a tool to move the contents of docker to another drive.

Once you have installed docker desktop from official installer, you can run the following query
```bash
wsl -l -v
```
![Results](/assets/images/docker/wsl2.png)

```docker-desktop``` and ```docker-desktop-data``` are the images that needs to be moved. 

I tried manually moving it :-) obviously it didn't work. While looking for another solution came across a tool which could do this without breaking anything. Its called [LxRunOffline](https://github.com/DDoSolitary/LxRunOffline).

Install the lxRunOffline tool using chocolatey

````bash
choco install lxoffline
````
Once installed stop the docker engine by simply quitting the application. Check the status of the WSLs, it should be in "Stopped" status. Run the following commands to move it any drive of your choice.

````bash
lxrunoffline move -n docker-desktop -d <Directory1>
lxrunoffline move -n docker-desktop-data -d <Directory2>
````
Directory1 and Directory2 should be unique, I also used the above command to move my actual WSL distro, no my C drive which is SSD will used only to install games ;-)

Happy coding!!!
