---
layout: post
title: "Linux 文件权限"
subtitle:   " 文件权限"
date:       2019-02-20 00:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

## 文件权限
ls
 -l中显示的内容如下：
 
~~~bash
-rw------- 1 root root   227 Feb 19 18:05 ssh_host_ecdsa_key
~~~

* 10个字符确定不同用户能对文件干什么
  * 第一个字符代表 `文件（-）、目录（d），链接（l）`
  * 其余字符每3个一组（rwx），`读（r）、写（w）、执行（x）`
  * 第一组rwx：文件所有者的权限是读、写和执行
  * 第二组rw-：与文件所有者同一组的用户的权限是读、写但不能执行
  * 第三组r--：不与文件所有者同组的其他用户的权限是读不能写和执行
* 也可用数字表示为：`r=4，w=2，x=1`  因此rw=4+2=6
  * 1 表示连接的文件数
  * root 表示用户
  * root表示用户所在的组
  * 227 表示文件大小（字节）
  * Feb 19 18:05 表示最后修改日期
  * ssh_host_ecdsa_key 表示文件名
### 改变权限的命令
* chmod 改变文件或目录的权限
* chmod 755 ssh_host_ecdsa_key：赋予ssh_host_ecdsa_key权限rwxr-xr-x
* chmod u=rwx，g=rx，o=rx ssh_host_ecdsa_key：同上u=用户权限，g=组权限，o=不同组其他用户权限
* chmod u-x，g+w ssh_host_ecdsa_key：给ssh_host_ecdsa_key去除用户执行的权限，增加组写的权限
* chmod a+r ssh_host_ecdsa_key：给所有用户添加读的权限

