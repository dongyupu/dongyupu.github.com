---
layout: post
title: "shell 脚本文件格式"
subtitle:   "dos / unix"
date:       2016-05-25 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

# shell 脚本文件格式

* shell脚本在windows下编辑后再拿到linux下执行时会报错

 `/bin/ksh^M: bad interpreter :No such file or directory`
 
* 此时在 vi 中打开脚本，查看文件格式 set ff? 会输出 fileformat=dos ，文件是dos格式的。
  需要转换成unix格式  set ff=unix  然后 wq 保存退出即可。

  
* 记得有一个linux下面的工具dos2unix  和 unix2dos 可以把脚本文件格式来回转换。在windows下使用MKS或者cygwin运行shell时可能需要来回转换。
