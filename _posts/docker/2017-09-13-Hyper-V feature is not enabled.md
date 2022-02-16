---
layout: post
title: "Hyper-V feature is not enabled."
subtitle:   "Note: VirtualBox/Vmware workstation will no longer work."
date:       2017-09-13 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---

### on windows 10 Hyper-V feature for docker and virtualbox/wmware workstation 
~~~bash
Hyper-V feature is not enabled.
Do you want to enable it for Docker to be able to work properly?
Your computer will restart automatically.
Note: VirtualBox will no longer work.
~~~

###  开启/关闭Hyper-V feature

~~~yml
> 控制面板
> 程序和功能
> 启动或关闭Windows功能
> Hyper-V 
~~~

### 开启Hyper-V feature 时virtualbox 只能创建32位虚拟机，无法创建64位系统虚拟机，wmware workstation无法工作。
### 关闭Hyper-V feature 时 docker 无法工作。
