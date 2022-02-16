---
layout: post
title: "Jenkins role strategy plugin and multibranch pipeline"
subtitle:   "Multibranch pipeline project auth"
date:       2019-11-22 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

* We have two types of projects, a free-style and a multibranch pipeline. I want to have a developer role to get only read access to the jobs, to analyse pipeline log and archived artifacts. For the free-style project works great, but for multibranch, a user within developer role can't see anything, appears the message 'This folder is empty` and triggered builds doesn't appears. Can someone give a light please?

* [google](https://stackoverflow.com/questions/53015798/jenkins-role-strategy-plugin-and-multibranch-pipeline)

* 即使在project role中正则表达式匹配到了项目名称，但是实际上权限还是没有生效，只有使用 `^Myproject-name(.*)` 把所有分支的权限都给到这个role才行
### solution

* 那么我们可以通过屏蔽部分分支扫描来规避权限的这个bug，具体操作就是在配置分支源时，在 `行为` 中 add `根据名称过了（支持通配符）` 》 `排除` master 
* 这样就不会扫描master（想要屏蔽的分支），权限也就受到了限制，然后master可以单独建一个job来控制权限

