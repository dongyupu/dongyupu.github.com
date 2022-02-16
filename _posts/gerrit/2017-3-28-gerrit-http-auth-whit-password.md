---
layout: post
title: "Gerrit Code Review - http auth whit password "
subtitle:   "download-commands插件配置"
date:       2017-3-28 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

# download-commands插件配置

~~~bash
[download]               
  scheme = ssh
  scheme = http　　　　　 #认证http下载
  scheme = anon_http   　#匿名http下载
  scheme = anon_git
  scheme = repo_download
~~~

If download.scheme is not specified, SSH, HTTP and Anonymous HTTP downloads are allowed.

+ gerrit配置认证方式为HTTP时，认证过程由代理服务器完成，如果没有设置HTTP Password 的话，那么就无法通过HTTP的url来clone和push代码。
<br>
<img src="{{ site.baseurl }}/img/2017/gerrit-http-auth-password.PNG" />
