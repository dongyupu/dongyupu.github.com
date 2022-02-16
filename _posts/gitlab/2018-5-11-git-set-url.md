---
layout: post
title: "gerrit 同步代码到 gitlab"
subtitle:   "git set-url"
date:       2018-05-11 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

公司新来得架构不熟悉gerrit，不想做代码评审，用过gitlab，要求我们从gerrit切换到gitlab。

步骤：
1.在Gitlab上建立对应的工程

2.登录gerrit对应的工程，git clone库到本地

3.使用命令，将远程仓库路径改为Gitlab项目仓库路径：

~~~bash
git remote `set-url` origin http://gitlab.com/xxx.git
~~~

4.检出所有分支

5.推送所有分支和标签

~~~bash
git push --all 
~~~

6.Gitlab上查看代码分支


