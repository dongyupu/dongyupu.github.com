---
layout: post
title: "docker dead but subsys locked"
subtitle:   "docker dead but subsys locked"
date:       2018-1-11 16:45:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
停止了一下docker，然后就起不来了

~~~bash
service docker status
docker dead but subsys locked
~~~

处理方法：

~~~bash
rm -rf /var/run/docker.*

rm -rf /var/lock/subsys/docker 

rm -rf /var/lib/docker/*
~~~

删除 /var/lib/docker/* 也就删除了所以的本地镜像

~~~bash
[root@39.107.59.105/10.81.236.10 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
[root@39.107.59.105/10.81.236.10 ~]#
~~~


`谨慎操作！`
