---
layout: post
title: "RocketMQ-console-ng : login settings"
subtitle:   "RocketMQ-console-ng login settings"
date:       2019-06-20 22:22:22 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---

## RocketMQ-console-ng login settings

* RocketMQ-console-ng 开启登录认证需要修改两个地方：

  * `rocketmq-externals/rocketmq-console/src/main/resources/application.properties`
  
  ~~~yml
  #Must create userInfo file: ${rocketmq.config.dataPath}/users.properties if the login is required
  rocketmq.config.loginRequired=true
  ~~~
  
  * `rocketmq-externals/rocketmq-console/src/main/resources/users.properties`
  
  ~~~yml
  # This file supports hot change, any change will be auto-reloaded without Console restarting.
  # Format: a user per line, username=password[,N] #N is optional, 0 (Normal User); 1 (Admin)

  # Define Admin
  admin=password,1

  # Define Users
  user1=user1password
  user2=user2password
  ~~~
  
  * 重新编译生产docker镜像部署即可
  
  ~~~bash
  mvn clean package -Dmaven.test.skip=true docker:build
  ~~~
