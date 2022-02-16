---
layout: post
title: "gerrit rename project"
subtitle:   "rename project. "
date:       2018-9-20 3:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
* gerrit项目重命名有两个办法：1.使用插件 2.手动重命名，并刷新gerrit缓存
<br>
1.gerrit有个插件支持重命名项目
<br>
https://gerrit.googlesource.com/plugins/rename-project/
<br>
2.命令行方式
<br>
http://lifuzu.com/blog/2014/04/09/rename-repository-on-gerrit/
<br>
1. Move the repository folder on server:

~~~bash
# on gerrit server
$ cd $site_path/git;
$ mv old_name.git new_name.git
~~~

2. Flush the caches on server:

~~~bash
# On another client image:
$ ssh -p 29418 Admin@gerrit_ip gerrit flush-caches
~~~

3. Run sql statement to update the changes:
新版本默认开启notedb，不需要执行下面的命令

~~~bash
# On another client image (need "Access Database" capability):
$ ssh -p 29418 gerrit.server.com gerrit gsql [Enter]
gerrit> USE {Database};
gerrit> SELECT * FROM changes WHERE dest_project_name = 'old_name';
# if have more than 1, run the following command to update, otherwise, quit with \q;
gerrit> UPDATE changes SET dest_project_name = 'new_name' WHERE
dest_project_name = 'old_name';
gerrit> \q
~~~

References:
<br>
https://groups.google.com/forum/#!topic/repo-discuss/ltIxBipUPKI
