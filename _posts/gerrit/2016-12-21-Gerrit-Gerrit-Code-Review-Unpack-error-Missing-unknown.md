---
layout: post
title: "Gerrit Code Review - Unpack error Missing unknown"
subtitle:   "Gerrit Unpack error Missing unknown"
date:       2016-12-21 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---
## Gerrit Code Review - Unpack error Missing unknown
<br>
gerrit如果删除 仓库 ，但有些提交尚未关闭，重新建同名仓库后再提交新的代码就会出现<br>
error: unpack failed: error Missing unknown<br>
如下

~~~ruby
error: unpack failed: error Missing unknown b5470d3842218f50e21f2932e52f9c1fd1304f8a
fatal: Unpack error, check server log
To ssh://weidengke@10.2.41.167:29418/aosp/platform/frameworks/base
 ! [remote rejected] HEAD -> refs/for/main-7.0.0_r21 (n/a (unpacker error))
error: failed to push some refs to 'ssh://weidengke@10.2.41.167:29418/aosp/platform/frameworks/base'
~~~


那么就需要去gerrit的数据库中将相应的提交做关闭处理<br>
具体操作：<br>
mysql就不说了，<br>
h2数据库进入的办法：<br>
需要先停止gerrit进程，然后进入到review_site目录，执行<br>

* sudo java -jar bin/gerrit.war gsql

~~~bash
    select * from changes where change_id=1603；
    update changes set open='N',status='A' where change_id=1603；
~~~

chang_id 可以在web界面上获取到。<br>
设置相应的提交为关闭状态后就可以提交新的代码了。<br>
