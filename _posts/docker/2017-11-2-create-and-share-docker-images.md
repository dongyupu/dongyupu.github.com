---
layout: post
title: "create and share docker images"
subtitle:   "create and share images"
date:       2017-11-2 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

* 2.1 将对容器的修改提交到镜像
  * 通过docker commit 命令提交你对容器做出的修改，并创建一个新的镜像。
  * 可以通过 docker diff 命令来查看在容器中对镜像做出的修改 eg: docker diff 69079aaaaaab1
  
* 2.2 将镜像和容器保存为 tar 文件进行共享
  * 对于已有镜像，可以使用 docker 命令行的 save 和 load 命令创建一个压缩包（tarball）
  * 对于容器，可以使用 import 和 export 进行导入导出操作。
  * 这两种方法非常类似。不同的是，save 保存 一个镜像会保留它的历史，export 导出 容器将对它的历史进行压缩。
  
* 2.3 编写dockerfile
  * dockerfile 是一个文本文件，它定义了一个镜像是如何构建的，以及基于该镜像的容器运行时会进行什么处理。通过 FROM、ENTRYPOINT 和 CMD这三个简单的指令，你已经可以构建一个能完全正常工作的镜像了。
  
  * 如果希望在构建镜像的时候使用在其他位置保存的dockerfile，可以使用 docker build -f 参数来指定 dockerfile 文件的位置。
  * 通过 docker build 命令的 -t 参数 设置 仓库名 和 标签 信息。
  
  * docker build 命令有几个选项可以用来设置如何处理构建过程中的临时容器。

* 2.4 
  * RUN 指令允许你在构建镜像的过程中执行指定的shell。
  * ADD 指令可以将宿主机文件复制到镜像内。
  * CMD 指令设置容器启动时将会执行的命令。
  
* 2.5 优化dockerfile
  * （1）在每个容器中只运行一个进程。
  * （2）容器是临时的，会被停止和重新启动。你应该把它们当作不可变的实体，这意味着你不应该对其进行修改，而应该从基础镜像重新创建它们。因此，需要将运行时配置和数据独立于容器和镜像进行管理。
  * （3）使用 .dockerignore 文件。在镜像构建过程中，docker会将dockerfile所在文件夹下的内容（即 build context）复制到构建环境中。
  * （4）利用已有官方镜像，而不是自己从头编写。
  * （5）最后，最大限度的减少镜像层的数量，并利用镜像缓存的优点。docker使用联合文件系统存储镜像。这意味着每个镜像都由一个基础镜像和一组添加了必要修改的变更构成。每个变更都表示一个额外的镜像层。这对你编写dockerfile以及如何使用各种指令会产生直接影响。
  
 * 2.6 通过标签对镜像进行版本管理
  * 通过 docker tag 命令为镜像打标签。这允许你对已有镜像进行重命名，或者为同一个镜像名创建新的标签。
  * 镜像标签是可选的。如果不指定标签，dokcer默认会使用latest作为标签。
  
* 2.8 使用packer构建docker镜像
  * packer(https://www.packer.io)是一个基于单个模版定义为多个平台创建相同计算机镜像的工具。
  * packer 支持shell、ansible、chef、puppet 和 salt 等 provisioner 。
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  

  
  
  





