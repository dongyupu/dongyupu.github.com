---
layout: post
title: "Gerrit Code Review -gitweb config "
subtitle:   "Gerrit gitweb设置"
date:       2017-10-10 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---


gerrit.config中设置

path_to_review_site/etc/gerrit.config

gerrit gitweb 需要指定type = gitweb

## centos :

~~~bash
[gitweb]
	      type = gitweb
        cgi = /var/www/git/gitweb.cgi
~~~

## ubuntu:

~~~bash
[gitweb]
	      type = gitweb
        cgi=/usr/lib/cgi-bin/gitweb.cgi
~~~ 

权限配置：
<br>
默认只有项目owner和管理员才有权限浏览gitweb。
<br>
参考：http://blog.sina.com.cn/s/blog_4fb490ff01018i0v.html
<br>

~~~bash
Make read access to refs/meta/config by default exclusive to project owners
这个设定做在了All-Projects的ACL设定里，所以缺省被所有的Project继承。
于是在单独项目的ACL里为项目的用户组增加了对refs/meta/config的read权限。
果然用户可以正常显示gitweb页面了。但是，这个设定带来了一个不好的影响。
原来gerrit是用git做自身的ACL管理的，
因此所有对refs/meta/config有read权限的用户都可以在Project的Access页面里修改权限，
修改后虽然不能直接submit，但是可以作为patch提交review。
而且通过gitweb就能够看到project.config和groups文件的全部内容，
知道当前项目是如何设置权限的。这样是否合适就仁者见仁智者见智了。
另外，对于用户提交的关于权限修改的patch如果想commit的话需要对refs/meta/config做和refs/heads/*一样的权限设定，
也就说需要Label Code-Review，Label Verified，Submit三个权限才有可能commit patch。“”

增加这个refs权限后，项目其他成员可以浏览代码了。
~~~
