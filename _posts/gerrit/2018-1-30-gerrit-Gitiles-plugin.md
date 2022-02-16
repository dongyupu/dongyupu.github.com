---
layout: post
title: "A simple JGit repository browser for gerrit"
subtitle:   "Gitiles is a simple repository browser for Git repositories, built on JGit. "
date:       2018-1-30 3:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---

[官方](https://gerrit.googlesource.com/gitiles/)

### Gitiles - A simple JGit repository browser

Gitiles is a simple repository browser for Git repositories, built on JGit. Its guiding principle is simplicity: it has no formal access controls, no write access, no fancy Javascript, etc.

Gitiles automatically renders *.md Markdown files into HTML for simplified documentation. Refer to the Markdown documentation for details.


### Configuration

Gitiles is configurable in a git-style configuration file named gitiles.config. Refer to the [configuration documentation](https://gerrit.googlesource.com/gitiles/+/HEAD/Documentation/config.md) for details.


[2.15下载地址](https://gerrit-ci.gerritforge.com/view/Plugins-stable-2.15/job/plugin-gitiles-bazel-master-stable-2.15/)
 * 下载 `gitiles.jar` 拷贝到 `plugins`目录下
 * 修改gerrit配置文件 `etc/gerrit.config`添加如下内容
 
~~~yml
[Gitiles]
        type = custom
        linkname = gitiles
        url = plugins/gitiles/
        revision = ${project}/+/${commit}
        project = ${project}
        branch = ${project}/+/${branch}
        filehistory = ${project}/+log/${branch}/${file}
~~~

重启gerrit即可。

我参考的[这里](http://permalink.gmane.org/gmane.comp.version-control.repo/10069)
