---
layout: post
title: "Gerrit 2.15 starts very slowly on centos 7."
subtitle:   "Gerrit 2.15 starts very slowly on centos 7"
date:       2018-1-23 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

As suggested in this post, [Gerrit 2.13.6 takes ages to start](https://bugs.chromium.org/p/gerrit/issues/detail?id=5667),
 it is probably due to missing entropy for the RNG. You can have a try to install haveged [How to Setup Additional Entropy for Cloud Servers Using Haveged](https://www.digitalocean.com/community/tutorials/how-to-setup-additional-entropy-for-cloud-servers-using-haveged).

<br>
for centos 

~~~bash
yum install haveged
chkconfig haveged on
service haveged start

gerrit.sh restart
~~~

这样重启gerrit就很快启动了，之前少则15分钟多则半小时。
