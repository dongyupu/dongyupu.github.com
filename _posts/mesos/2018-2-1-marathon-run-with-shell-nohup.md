---
layout: post
title: "run yum install marathon with nohup command & "
subtitle:   "Mesos 1. 里，slave会更名为agent"
date:       2018-2-1 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-04.jpg"
---

[nohup是永久执行](http://blog.csdn.net/zhang_red/article/details/52789691)

&是指在后台运行

运行 nohup --help
Run COMMAND, ignoring hangup signals. 可以看到是“运行命令，忽略挂起信号”

就是指，用nohup运行命令可以使命令永久的执行下去，和用户终端没有关系，例如我们断开SSH连接都不会影响他的运行，注意了nohup没有后台运行的意思；&才是后台运行

&是指在后台运行，但当用户推出(挂起)的时候，命令自动也跟着退出

那么，我们可以巧妙的吧他们结合起来用就是
nohup COMMAND &
这样就能使命令永久的在后台执行


## 运行marathon
使用yum安装marathon无法正常启动，提示没有master，所以我改为直接用脚本来启动

~~~bash
nohup marathon --master zk://ip:2181,ip:2181,ip:2181/mesos --zk zk://ip:2181,ip:2181,ip:2181/marathon &
~~~

