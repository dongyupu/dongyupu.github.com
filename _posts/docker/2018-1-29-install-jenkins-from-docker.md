---
layout: post
title: "使用容器安装运行docker"
subtitle:   "在运行在容器中的jenkins里共享宿主机的docker的socket文件"
date:       2018-1-29 16:45:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
[按照官方的安装指导](https://jenkins.io/doc/book/installing/)

挂载docker的文件时需要注意，如果是使用的`jenkinsci/blueocean` 镜像，那么镜像里已经安装了docker，不需要再把宿主机里的`/usr/bin/docker`挂载到容器中了，否则会导致docker命令不可用。
<br>
网上好多教程里都使用的`jenkins`镜像，这个里面好像默认没有安装docker所以需要进行挂载。
<br>
这样的话容器中的docker和宿主机的docker就可以共享存储了
