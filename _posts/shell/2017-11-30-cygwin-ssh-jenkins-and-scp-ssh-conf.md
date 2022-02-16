---
layout: post
title: "Jenkins & scp ssh settings on windows"
subtitle:   "Could not create directory '/home/SYSTEM/.ssh'."
date:       2017-11-30 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
## [scp 免密码的方法](http://blog.csdn.net/nfer_zhuang/article/details/42646849)

### 建立SSH的信任关系

*在这里先介绍两个概念：SSH公钥 ~/.ssh/id_rsa.pub 和公钥授权文件 ~/.ssh/authorized_keys ，这两个文件的作用具体可以参考ssh的man手册：

 ~~~bash
 ~/.ssh/id_rsa.pub
         Contains the public key for authentication.  These files are not sensitive and can (but need not) be readable by anyone.
 ~/.ssh/authorized_keys
         Lists the public keys (DSA/ECDSA/RSA) that can be used for logging in as this user. 
		 The format of this file is described in the sshd(8) manual page.  
		 This file is not highly sensitive, but the recommended permissions are read/write for the user, and not accessible by others.
~~~

* add your public key to remote machine's  `~/.ssh/authorized_keys `

~~~bash
cat ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDse/fAKoC453sudZGYS05gRFBfPgpvGIz9N231SaiijU44t6i3SV7KTkWSOB1nHn6Wkp1bFf71HBgiZIOaq+PZePbG6iJ+0rFTRP65saBw2Fdgw7/i1vWRjiniupwXYVZqa/UROhWp2PMkOp8t8yHQyjTuKUuaNMLTby18B3FnHtvMr4AfBoJ2I6SzQ1y9Y/KIh55092Vt1WVzncT4WVDV1sKeAgEaObdSMKvQ0dkCgQd599cCQDp+5FZnK2xurhcLAh6D4eel+f9Uz51KM5PUHaXgvAHNZTDlBOAUfEU7XQRQ6H1pLyqCrZpR7XVonRdJI+uqriP1j7XRMQ24bOiR 55@55-PC
~~~

## Jenkins 在Windows系统中是以SYSTEM 用户的身份启动

* 安装cygwin后，在使用SSH的时候默认会去 /home/SYSTEM/.ssh 目录中查找private key ，当这个用户的目录不存在时就会出现error

~~~bash
Could not create directory '/home/SYSTEM/.ssh'.
Host key verification failed.
~~~

cygwin中没有安装ssh的时候，会默认使用Windows系统的ssh key，需要在

~~~bash
C:\Windows\System32\config\systemprofile\.ssh
~~~

 将ssh key拷贝一份到该目录即可。
 
 
 
 
 
 
 
 
 





