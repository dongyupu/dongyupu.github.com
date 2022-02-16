---
layout: post
title: "Redhat 6.5 vsftpd configs"
subtitle:   "vsftpd config"
date:       2017-09-11 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---
## add user 创建用户，并指定分组和主目录

~~~bash 
useradd -d /opt/test -s /sbin/nologin -g ftpGroup -G root ftpUser
~~~ 

~~~yml
              useradd 添加用户ftpUser
              -d 指定用户根目录为/opt/test
              -s 指定bash脚本为/sbin/nologin，表示不允许bash登录
              -g 创建分组ftpGroup
              -G 指定root分组
        PS：创建有问题可以删除重新创建 userdel -r ftpUser
~~~

## 设定密码
　　passwd ftpUser
  
### edit /etc/vsftpd/vsftpd.conf

~~~yml
　　anonymous_enable=NO
　　chroot_list_enable=YES
　　chroot_list_file=/etc/vsftpd/chroot_list
　　userlist_enable=YES
　　userlist_deny=NO
　　userlist_file=/etc/vsftpd/user_list
~~~

### 打开/etc/vsftpd/user_list并增加一行：
　　ftpUser
  
### 新建文件/etc/vsftpd/chroot_list并增加一行
　　ftpUser
### /etc/init.d/vsftpd restart

~~~bash
ftp -i -n 127.0.0.1
ftp>user ftpUser 123456
ftp>ls
~~~
  
