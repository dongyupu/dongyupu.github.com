---
layout: post
title: "Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password)."
subtitle:   " Authentication refused: bad ownership or modes for file /home/appuser/.ssh/authorized_keys"
date:       2018-06-25 00:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

配置ssh免密登录时，发现登录不上去，总是要求输入密码，ping是通的。
<br>
用root权限查看日志`/var/log/secure`，可以查到报错

~~~bash
Jun 25 16:50:00  sshd[10957]: Connection closed by 10.25.90.207
Jun 25 16:53:06  sshd[11169]: Authentication refused: bad ownership or modes for file /home/appuser/.ssh/authorized_keys
~~~

`authorized_keys`  这个文件权限不对，改成600权限

~~~bash
chmod 600 /home/appuser/.ssh/authorized_keys
~~~


