---
layout: post
title: "nvm 配置 node_mirror 使用Jfrog Artifactory remote Repositories proxy"
subtitle:   "remote Repositories proxy "
date:       2021-05-08 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

- 在Jfrog Artifactory 创建remote Repositories proxy类型选择Generic，设置 https://npm.taobao.org/mirrors/node/

- nvm node_mirror <node_mirror_url>

  - 我这里代理仓库的URL为 http://artifactory.hyxfjr.com/artifactory/nvm/ 后面这个 '/' 要写，还是找不到要下载的版本


