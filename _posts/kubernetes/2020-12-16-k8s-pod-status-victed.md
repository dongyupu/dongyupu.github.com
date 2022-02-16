---
layout: post
title: "k8s pod status victed"
subtitle:   "k8s pod status victed"
date:       2020-12-16 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

eviction，即驱赶的意思，意思是当节点出现异常时，kubernetes将有相应的机制驱赶该节点上的Pod。
多见于资源不足时导致的驱赶。

解决方案
排查资源和异常原因，防止新的驱赶产生。
使用如下命令删除旧驱赶的遗留

~~~
kubectl -n atlassian get pods | grep Evicted | awk '{print $1}' | xargs kubectl -n atlassian delete pod
~~~

