---
layout: post
title: "docker 配置"
subtitle:   "docker 配置"
date:       2017-11-4 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

* 4.7 远程访问Docker守护进程
  * 默认的docker守护进程坚挺UNIX套接字（/var/run/docker.sock），因此你只能在本地主机上访问到docker守护进程。
  * 想要远程访问docker的守护进程，需要修改配置文件 /etc/default/docker 来切换docker守护进程的监听协议，启用远程API调用。
  * 在 /etc/default/docker 文件中，添加一行设置 DOCKER_HOST 使用 tcp ，端口为 2375 .然后重启启动docker的守护进程。
    * DOCKER_OPTS="-H tcp://127.0.0.1:2375"
  * 这种方法没有使用加密和身份验证机制。你不应该在一台通过公网可以访问的主机上使用这样的配置，否则你的docker守护进程将暴露给所有人。如果想在生产环境中从远程访问docker守护进程，你需要确保docker守护进程的安全性。

* 4.8 通过docker远程API完成自动化任务

* 4.9 从远程安全访问docker守护进程
  * 为docker守护进程配置基于TLS的访问。这将使用公共密钥来对docker客户端和启用了TLS的docker守护进程之间的同学进行加密和身份验证。
  * 在正规的基础设施中，你应该联系你熟悉的CA，并从这个CA 获得一个服务器证书。
  
* 4.10 使用 docker-py访问远程docker守护进程
  * docker-py是一个由python编写的docker客户端。

* 4.12 更改存储驱动程序
  * 通过修改配置文件你可以设置docker使用的后端存储
  * DOCKER_OPTS="-s overlay"
  * Linux 内核自3.18开始支持overlay文件系统。从3.18开始，Linux上游（主线）内核已经开始支持overlay文件系统，而AUFS则没有进入到上游内核。你可以考虑使用overlay文件系统。
  
  * docker可以使用多种存储后端来存储镜像和容器的文件系统。docker存储抽象通过将镜像和容器文件系统保存在层中，并只对层与层之间的变动进行追踪，以减小存储镜像和容器文件系统所需要的空间。docker存储依赖联合文件系统来实现上述功能
