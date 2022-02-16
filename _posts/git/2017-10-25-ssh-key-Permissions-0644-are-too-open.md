---
layout: post
title: "Permissions 0644 for '/root/.ssh/id_rsa' are too open."
subtitle:   "This private key will be ignored."
date:       2017-10-25 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
复制.ssh文件夹重用ssh key时，可能出现权限问题，导致ssh链接被拒。

~~~bash
git clone ssh://Admin@192.168.64.68:29418/mesos/jenkins-on-mesos
正克隆到 'jenkins-on-mesos'...
The authenticity of host '[192.168.64.68]:29418 ([192.168.64.68]:29418)' can't be established.
ECDSA key fingerprint is SHA256:MmFho1QRFeDMOHww2JUclzwTA1pSUw5odhRX330vWII.
ECDSA key fingerprint is MD5:87:b0:5b:d0:27:5f:8f:3e:f1:92:fa:72:2d:6c:15:3e.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[192.168.64.68]:29418' (ECDSA) to the list of known hosts.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/root/.ssh/id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/root/.ssh/id_rsa": bad permissions
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

~~~

`Permissions 0644 for '/root/.ssh/id_rsa' are too open.`

* 解决方案：将权限由0644降低为0600

~~~bash
chmod 0600 ~/.ssh/id_rsa
~~~


