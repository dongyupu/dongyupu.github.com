---
layout: post
title: "彻底清除Github上某个文件的历史（针对误上传密码文件等情况）"
subtitle:   "删除库，再把本地的分支历史reset，最后重新上传!"
date:       2018-5-10 13:45:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---

github提供api了，可以用关键字去爬。公司有专门的爬虫做检测，所以如果被关键字命中···

那么如何删除误传的文件和历史记录呢，

使用git filter-branch命令可以删掉

~~~bash
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch FILE_PATH' --prune-empty --tag-name-filter cat -- --all
~~~

但是，但是，根本没有用，还是可以在GitHub上被检索到历史。
<br>
无奈之下我只好先clone到本地备份，然后把GitHub上的仓库删除，再重新创建一个同名仓库，把本地的备份reset到没有关键字的某次提交，最后重新push

~~~bash
git push -u origin master
~~~
