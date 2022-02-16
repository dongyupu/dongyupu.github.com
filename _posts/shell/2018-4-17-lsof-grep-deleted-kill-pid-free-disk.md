---
layout: post
title: "delete files and free disks"
subtitle:   "lsof | grep deleted & kill pid free disk"
date:       2018-04-17 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

今天发现jenkins的日志无限膨胀到25g了，删除log文件后， 使用df -h 查看硬盘空间还是没有被释放出来。

解决办法：
1.重启系统
2.kill 掉占用文件的进程

~~~bash
lsof |grep deleted
kill -9 pid
df -h
~~~

重启其实和杀掉进程一个道理。
