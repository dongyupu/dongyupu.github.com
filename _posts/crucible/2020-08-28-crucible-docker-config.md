---
layout: post
title: "crucible 官方镜像部署配置"
subtitle:   " crucible server config"
date:       2020-08-28 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
使用 helm部署
* 版本：atlassian/crucible:4.8.3
* user:fecru
* disk：腾讯云CBS

### 问题1 CBS权限
* CBS挂载到系统时默认时root用户挂载的，权限需要修改为 `fecru:fecru` (101:102),如何找到你的cbs盘》helm安装挂载cbs（pod启动失败此时），然后到node上执行 `df -h`找到你的cbs盘后，执行`chown -R 101:102 folder_to_cbs`

### 配置问题 server_site
* Server > Site URL > http://crucible.domain.com

