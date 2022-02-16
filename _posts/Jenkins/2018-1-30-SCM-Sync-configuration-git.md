---
layout: post
title: "SCM Sync configuration with git"
subtitle:   "jenkins configuration backup"
date:       2018-1-30 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---


[SCM Sync configuration plugin](https://wiki.jenkins.io/display/JENKINS/SCM+Sync+configuration+plugin)

当jenkins运行在容器中时，需要注意容器中启动jenkins的用户是否拥有备份仓库的访问权限，否则会导致备份无法提交。

[查看日志](https://wiki.jenkins.io/display/JENKINS/ScmSyncConfig+Troubleshootings)

我使用的官方的`jenkinsci/blueocean`镜像，里面没有ssh秘钥，也没有配置git用户信息，需要自己映射文件到镜像里以便插件在使用时可以同步代码。

jenkins docker-compose.yml

~~~yml
jenkins: 
  image: jenkinsci/blueocean
  user: root
  volumes: 
  - /app/jenkins-home:/var/jenkins_home
  - /var/run/docker.sock:/var/run/docker.sock
  - /root/.ssh:/root/.ssh
  - /root/.gitconfig:/root/.gitconfig
  ports: 
  - "8080:8080"
  - "50000:50000"
~~~

为了获取运行docker的权限，可能需要使用root用户来启动jenkins。默认情况下，只有root用户或者docker组内的用户才能访问`/var/run/docker.sock`文件。
默认的jenkins容器通过jenkins用户来启动服务器。jenkins用户不属于docker用户组，不过即使jenkins属于容器内的docker用户组，仍然不能访问docker socket，
因为宿主机和容器中的组ID与用户ID并不一致（有一个例外就是root用户，其ID总是0）。





