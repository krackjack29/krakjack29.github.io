---
title: 'Moving from Windows 10 to Ubuntu'
layout: single
comments: true
date : 2019/11/02
categories:
    - Tech
tags:
    - windows
    - ubuntu
    - tools
---

I have been an afficianado of whatever Microsoft builds, since I was introduced to the computers. I have used windows on personal computer since win'98 days, followed by Windows NT, 2000, XP, 8, 8.1 and 10, even had a computer with god awful Vista on it for a while. Since microsoft officially announced "Microsoft Loves Linux", I took a keen interest in using linux (specifically Ubuntu using Windows Subsystems). After using it for a while, I understood why its a hit amongst geeks and CLI lovers. 

For all the good things that Windows 10 packs, it also has some really frustrating quirks like Updates (latest one broke my PC and to reset to old version), no full support for Linux kernel yet on WSL. So I decided to move to Ubuntu on my PC, while doing so also collated a script to easily load up the softwares neccessary. 

## Steps to Install Ubuntu
1. Download Ubuntu from official [downloads](https://ubuntu.com/download/desktop) page.
2. Create a bootable USB disk using [Universal USB Installer](https://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/).
3. Plugin the USB to your PC.
4. Restart and select boot from USB
5. Follow the install instructions from Ubuntu installer screen.

## Softwares Installed after successful installation
1. Overwrite the APT repositories to add security patches and other software repositories.
2. Utility softwares which are kind of dependencies for most of the software that i installed.
3. Cloud CLIs (Since I work on projects using the Big 3, I installed Azure CLI, AWS CLI and GCloud CLI)
4. Powershell Core
5. Apt-File finder, allows you to search the particular file from available repositories.
6. Spotify :-)
7. GIMP, photo editor app.
8. Visual Studio Code
9. Favorite Programming languages (Mine: NodeJs, Dotnet, Go)
10. Blogging tools, since I use Jekyll for blogging I need ruby, jekyll
11. Terminal beautifier like Zsh, Oh-my-Zsh and Oh-my-Posh, More about them in my [previous blog](https://www.cloudmanav.com/tech/beautify-windows-terminal/)
12. Container tools (docker, kuectl)
13. Well some tools like Visual studio still needs Windows, so I installed virtual box for creating a VM if needed in future, currently my go to IDE is Visual Studio Code.

I have created a github gist containing all the neccessary installation commands for above said software.
<script src="https://gist.github.com/pratap-dotnet/87332d983ea39da346fca0b97e5587d1.js"></script>

**UI** : Ubuntu packages GNOME by default, all I had to do was to install Gnome-Tweaks tool to add themes and icons to Ubuntu.

## References
I found www.itfoss.com to be really handy while setting this up
[https://itsfoss.com/gnome-tweak-tool/](https://itsfoss.com/gnome-tweak-tool/)
[https://itsfoss.com/create-live-usb-of-ubuntu-in-windows/](https://itsfoss.com/create-live-usb-of-ubuntu-in-windows/)

Happy Coding!!!
