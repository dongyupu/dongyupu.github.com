---
layout: post
title: "zookeeper Cluster "
subtitle:   "zookeeper集群中，有超过半数的机器挂掉时，集群就无法工作了。"
date:       2017-10-30 0:21:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

### zookeeper集群选举leader需要票数过半，也就是说必须有一半的节点是正常工作的。

* ZK集群的机制是只要超过半数的节点OK，集群就能正常提供服务。只有ZK节点挂得太多，只剩一半或不到一半节点能工作，集群才失效。

* 所以
* 3个节点的cluster可以挂掉1个节点(leader可以得到2票>1.5)
* 2个节点的cluster就不能挂掉任何1个节点了(leader可以得到1票<=1)

