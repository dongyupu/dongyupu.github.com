---
layout: post
title: "gerrit command line and rest api"
subtitle:   "gerrit ls-projects"
date:       2017-03-22 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---
# For code backup

~~~bash
#!/bin/sh -x
pwd
cd /home/wimo-server/jenkins_slave
ls
#clone all projects
for p in `ssh -p 29418  Admin@10.2.48.75 gerrit ls-projects`
do
  #mkdir -p `dirname "$p"`
  if [ ! -d "$p.git" ]; then
    git clone --mirror "ssh://Admin@10.2.48.75:29418/$p.git" "$p.git"
  else
    cd $p
    git fetch
    #cd $WORKSPACE
  fi
  cd /home/wimo-server/jenkins_slave
done
~~~

+ git clone --bare 和 --mirror 的区别在于 --mirror会向上游仓库注册,你可以在git仓库里执行 git fetch一遍和上游仓库保持同步.

+ ssh链接的时候需要指定用户名,否则会链接失败
