---
layout: post
title: "jenkins git gerrit trigger config"
subtitle:   "git and gerrit trigger config"
date:       2017-12-15 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---
使用git管理源码的设置
<br>
需要配置

~~~yml
Refspec ：$GERRIT_REFSPEC
Branches to build ：$GERRIT_BRANCH
Additional Behaviours	：
Strategy for choosing what to build ：
Choosing strategy：gerrit trigger
~~~
<br>
<img src="{{ site.baseurl }}/img/2017/git-gerrit-conf.jpg" />


<br>
gerrit trigger conf 

<img src="{{ site.baseurl }}/img/2017/gerrit-trigger-conf.jpg" />




