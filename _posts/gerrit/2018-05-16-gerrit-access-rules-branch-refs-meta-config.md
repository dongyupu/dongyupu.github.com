---
layout: post
title: "gerrit access rules branch refs/meta/config"
subtitle:   " fetch refs/meta/config and modify access rules "
date:       2018-5-16 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-06.jpg"
---

除了通过gerrit web页面配置仓库权限以外，我们还可以通过之间修改权限分支的方法来配置项目权限，首先把仓库clone到本地，然后fetch 权限分支refs/meta/config，checkout出config分支，本地修改，最后push到gerrit即可。
<br>
批量创建项目时可以考虑填写好权限模版，然后用脚本去实现自动化。web页面手动填写容易出错。
<br>
说明：”
refs/meta/config： 这个分支是gerrit的权限配置分支
<br>
具体步骤：
<br>
以AI_center的权限配置为例
<br>
1.	检测仓库

~~~bash
git clone ssh://Admin@gerrit_server:29418/AI/AI_Center
~~~

2.	直接clone项目时refs/meta/config这个分支不会同步，需要手动fetch

~~~bash
git fetch origin refs/meta/config:config
~~~

3.	检出分支

~~~bash
git checkout config
~~~


4.	修改权限文件
<br>
Groups #如果没有就新建一个，这个文件标示了分组信息

~~~bash
# UUID                                  	Group Name
#
16f1ab5aab72a53ea4eb534f9578ad95c6cd82cf	Administrators
785fc4f9f4a6c3c79178b5008f3944a218e6a5c9	AI_Center_Reviewer
d924995678ef1c69f1dee95ed37be5ee6c43a731	Non-Interactive Users
f698ab48646337e40b74b9823daaed6a89782986	AI_Center_dev
global:Project-Owners                   	Project Owners
~~~

说明：如何获取Group Name的UUID
<br>
登录到gerrit，一次点击 People 》 List Groups 》 Group Name 》 General
<br>

project.config #该文件存储具体的权限配置信息，如果没请创建，内容类似如下格式
<br>
~~~yml
[access]
	inheritFrom = All-Projects
[access "refs/heads/*"]
	read = group AI_Center_Reviewer
	read = group AI_Center_dev
	read = group Administrators
	read = group Non-Interactive Users
	create = group AI_Center_Reviewer
	create = group Administrators
	create = group Non-Interactive Users
	createTag = group AI_Center_Reviewer
	createTag = group Administrators
	createTag = group Non-Interactive Users
	createSignedTag = group AI_Center_Reviewer
	createSignedTag = group Administrators
	createSignedTag = group Non-Interactive Users
	pushMerge = group AI_Center_Reviewer
	pushMerge = group AI_Center_dev
	pushMerge = group Administrators
	pushMerge = group Non-Interactive Users
	label-Code-Review = -2..+2 group AI_Center_Reviewer
	label-Code-Review = -2..+2 group Administrators
	label-Code-Review = -2..+2 group Non-Interactive Users
	label-Code-Review = -2..+1 group AI_Center_dev
	submit = group AI_Center_Reviewer
	submit = group Administrators
	submit = group Non-Interactive Users
	push = group AI_Center_Reviewer
	push = group AI_Center_dev
	push = group Administrators
	push = group Non-Interactive Users
[access "refs/for/refs/*"]
	read = group AI_Center_Reviewer
	read = group AI_Center_dev
	read = group Administrators
	read = group Non-Interactive Users
	push = group AI_Center_Reviewer
	push = group AI_Center_dev
	push = group Administrators
	push = group Non-Interactive Users
	pushMerge = group AI_Center_Reviewer
	pushMerge = group AI_Center_dev
	pushMerge = group Administrators
	pushMerge = group Non-Interactive Users
[access "refs/heads/master"]
	read = group AI_Center_Reviewer
	read = group AI_Center_dev
	read = group Administrators
	read = group Non-Interactive Users
	label-Code-Review = -2..+2 group AI_Center_Reviewer
	label-Code-Review = -2..+2 group Administrators
	label-Code-Review = -2..+2 group Non-Interactive Users
	label-Code-Review = -2..+1 group AI_Center_dev
	submit = group AI_Center_Reviewer
	submit = group Administrators
	submit = group Non-Interactive Users
[access "refs/meta/config"]
	read = group AI_Center_Reviewer
	read = group AI_Center_dev
	read = group Administrators
	read = group Non-Interactive Users
	read = group Project Owners
~~~

5.	commit提交更改

~~~bash
git add *
git commit –m “modify project access rules”
git push origin config:refs/meta/config
~~~

使用图形工具push时注意修改remote分支的名字，需要手动填写。
 







