---
layout: post
title: "GIT创建一个空分支"
subtitle:   "使用 git checkout的--orphan参数"
date:       2018-6-21 13:45:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

有时候我们需要在GIT里面创建一个空分支，该分支不继承任何提交，没有父节点，完全是一个干净的分支，例如我们需要在某个分支里存放项目文档。
<br>
使用传统的git checkout命令创建的分支是有父节点的，意味着新branch包含了历史提交，所以我们无法直接使用该命令。
<br>
创建分支
<br>
使用 git checkout的--orphan参数:
<br>

~~~bash
git checkout --orphan doc
~~~

该命令会创建一个名为doc的分支，并且该分支下有前一个分支下的所有文件。
查看--orphan的帮助：

~~~bash
Create a new orphan branch, named <new_branch>, started from <start point> and switch to it.
 The first commit made on the new branch will have no parents 
 and it will be the root of a new history totally disconnected from all the other branchs and commits.
~~~



