---
layout: post
title: "shell parameter $"
subtitle:   "shell parameters"
date:       2017-09-27 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---

~~~bash
$0 Shell本身的文件名
$1～$n 添加到Shell的各参数值。$1是第1参数、$2是第2参数…
$$ Shell本身的PID（ProcessID）
$! Shell最后运行的后台Process的PID
$? 最后运行的命令的结束代码（返回值）
$- 使用Set命令设定的Flag一览
$* 所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有数。
$@ 所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
$# 添加到Shell的参数个数
~~~
