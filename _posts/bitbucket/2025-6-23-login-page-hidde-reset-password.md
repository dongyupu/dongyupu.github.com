---
layout: post
title: "bitbucket隐藏登录页面的忘记密码链接"
subtitle:   "Platform Notice: Data Center Only"
date:       2025-06-23 1:22:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
### bitbucket隐藏登录页面的忘记密码链接
- 背景是这样的，使用了crowd来做登录认证，所以bitbucket中是无法修改密码的。这就需要隐藏忘记密码按钮以免有些用户在bitbucket中操作浪费时间。
- bitbucket datacenter 9.3.2 版本，系统安全增强，无法直接使用系统自带的Announcement Banner来注入JS或者css了，尝试使用scriptrunner进行注入也失败了。
### Solution
两种方案，一种是直接修改$BITBUCKET_INSTALL/app/static/bitbucket/internal/page/login/login.soy文件，注释掉链接，另一种方案则比较简单，安装Announcement Banner for Bitbucket插件，这个插件还可以注入js或者css，而且还能控制Visibility Level，这样就完美解决了隐藏忘记密码的需求。

~~~
请使用邮箱前缀xxx（xxx@dreame.tech）登录，密码为飞连的密码。如果忘记密码，请使用手机飞连（飞书登录），使用动态码修改密码。<a title="忘记密码" href="https://feilian.it.starmotor.tech:10443/profile-safety" >忘记密码</a>
<style type="text/css">
  /* 隐藏 href="/passwordreset" 的链接 */
  nav.css-7c5c0 a[href="/passwordreset"] {
    display: none !important;
  }
</style>
~~~
