---
layout: post
title: "docker base"
subtitle:   "docker base"
date:       2017-11-2 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

* docker 的核是一个被称作docker引擎的基于单主机运行的守护进程，我们可以通过这个守护进程来创建和管理容器。

* docker-machine 是一个docker工具，可以通过它在公有云上创建云主机并安装docker，自动配置本地docker客户端来使用远程docker主机。

* 一个容器的标准生命周期：创建、启动、停止、终止和移除。

* dockerfile是一个用于描述如何构建容器镜像的文件。

* docker的强大之处在于通过组合服务来运行应用程序。

* 要想在容器中运行/bin/bash,需要使用 -t 和 -i 参数来获得一个交互式会话。

* 以后台方式运行docker容器，需要使用 -d 参数。

* 可以通过 exec 命令来启动一个bash shell，再次进入到容器中。

  ~~~bash
    docker exec -ti jenkins /bin/bash
  ~~~
  
* supervison 用来控制多个进程的普通程序。

* --link 选项可以将两个容器的网络自动配置，从而可以互相访问。这样可以充分利用容器的隔离优势，对应用程序的不同组件进行解耦。

* 备份在容器中运行的数据 
  * 将docker主机上的卷挂载到容器中
  * 使用 docker exec 命令执行mysqldump

* 在容器直接共享数据
  * -v 选项 如果省略了宿主机中的路径，那么就会创建一个称为 `数据容器` 的容器。可以通过 inspect 命令来查看这个数据卷被保存到了宿主机的什么位置。
  * 可以通过 --volumes-from 来挂载 `数据容器` 中的卷。
  * 在删除 `数据容器` 时，如果没有使用 rm -v 选项 ，那么系统中会遗留很多没有被使用的卷。
  
* docker cp 命令将文件从正在运行的容器复制到docker主机。










