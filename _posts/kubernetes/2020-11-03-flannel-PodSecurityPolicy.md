---
layout: post
title: "TKE k8s集群被安装了flannel插件，导致Jenkins无法通过域名上传制品到artifactory"
subtitle:   "TKE k8s集群被安装了flannel插件，导致Jenkins无法通过域名上传制品到artifactory"
date:       2020-11-03 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---




## TKE k8s集群被安装了flannel插件，导致Jenkins无法通过域名上传制品到artifactory

### 问题描述：

现象是Jenkins无法通过域名上传制品到artifactory


### 问题排查分析过程：

于tke客服联系排查问题，测试了网络连接，发现网络不通，排查发现tke集群被安装了flannel插件，tke客服确认了不是他们使用的该插件，内部询问也不知道谁安装的。

由于对flannel插件不熟悉，最开始只是删除了flannel的ds，之后测试发现网络都是通的，但是域名就是无法访问。

今天在部署upsource时发现一个node节点的annotations中含有几个flannel信息，于是意识到flannel插件可能并没真正卸载掉。



### 解决方案：

按照官方安装指导的yaml排查集群中相关资源，然后删除掉这些资源即可。

https://github.com/coreos/flannel/blob/master/Documentation/kube-flannel.yml


删除yaml中安装的 PodSecurityPolicy、ClusterRole、ClusterRoleBinding、ServiceAccount、ConfigMap、DaemonSet


kubectl delete -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
