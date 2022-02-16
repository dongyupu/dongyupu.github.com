---
layout: post
title: "use the git update-ref command. To remove the last commit in the bare repo"
subtitle:   "remove the last commit in the bare repo"
date:       2017-07-06 020:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---

[You can use the git update-ref command. To remove the last commit, you would use:](https://stackoverflow.com/questions/4624881/how-can-i-uncommit-the-last-commit-in-a-git-bare-repository)

~~~bash
$ git update-ref HEAD HEAD^
~~~

Or if you're not in the branch from which you cant to remove the last commit:

~~~bash
$ git update-ref refs/heads/branch-name branch-name^
~~~

You could also pass a sha1 if you want:

~~~bash
$ git update-ref refs/heads/branch-name a12d48e2
~~~

See the documentation of the [git-update-ref](http://schacon.github.com/git/git-update-ref.html) command.
