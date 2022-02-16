---
layout: post
title: "change jenkins system time from system scripts"
subtitle:   "jenkins System.setProperty"
date:       2018-3-12 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

从docker运行jenkins时，如果系统的时间默认是UTC时间的话，就需要修改下jenkins的时区以纠正时间。
具体操作步骤：
<br>
系统管理》脚本命令行》
<br>

~~~bash
System.setProperty('org.apache.commons.jelly.tags.fmt.timeZone', 'Asia/Shanghai')
~~~

<br>
参考链接：https://wiki.jenkins.io/display/JENKINS/Change+time+zone
<br>
这种方式的话，如果重启jenkins镜像重启可能会失效，需要再执行一次即可。

<br>
根本的解决办法是：修改系统时区，我们的jenkins镜像是Alpine Linux 3.7的系统，在/etc/TZ下应该可以修改，具体做法就是在启动jenkins时使用自己写的TZ挂载劲容器覆盖掉原来的。具体的我没有查是不是可行。因为上面的方法也很方便。
