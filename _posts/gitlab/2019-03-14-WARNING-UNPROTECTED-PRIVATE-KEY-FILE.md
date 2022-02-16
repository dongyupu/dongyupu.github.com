---
layout: post
title: "gitlab :warning unproected private key file !"
subtitle:   "permissiongs 0755 for /etc/gitlab/ssh_host_*_key are too open"
date:       2019-03-14 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
## warning unproected private key file !


我在使用kubernetes部署gitlab（挂载阿里云NAS盘）时遇到：
gitlab 访问502 
`gilabt-ctl tail` 查看日志后发现如下错误

~~~bash
20182019032-148_13:01:02.92007 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:01:02.92010 @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
2019-03-14_13:01:02.92010 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:01:02.92010 Permissions 0755 for '/etc/gitlab/ssh_host_rsa_key' are too open.
2019-03-14_13:01:02.92010 It is required that your private key files are NOT accessible by others.
2019-03-14_13:01:02.92011 This private key will be ignored.
2019-03-14_13:01:02.92023 key_load_private: bad permissions
2019-03-14_13:01:02.92198 Could not load host key: /etc/gitlab/ssh_host_rsa_key
2019-03-14_13:01:02.92242 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:01:02.92243 @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
2019-03-14_13:01:02.92244 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:01:02.92244 Permissions 0755 for '/etc/gitlab/ssh_host_ecdsa_key' are too open.
2019-03-14_13:01:02.92245 It is required that your private key files are NOT accessible by others.
2019-03-14_13:01:02.92246 This private key will be ignored.
2019-03-14_13:01:02.92254 key_load_private: bad permissions
2019-03-14_13:01:02.92377 Could not load host key: /etc/gitlab/ssh_host_ecdsa_key
2019-03-14_13:01:02.92420 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:01:02.92422 @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
2019-03-14_13:01:02.92423 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:01:02.92424 Permissions 0755 for '/etc/gitlab/ssh_host_ed25519_key' are too open.
2019-03-14_13:01:02.92424 It is required that your private key files are NOT accessible by others.
2019-03-14_13:01:02.92425 This private key will be ignored.
2019-03-14_13:01:02.92434 key_load_private: bad permissions
2019-03-14_13:01:02.92548 Could not load host key: /etc/gitlab/ssh_host_ed25519_key
2019-03-14_13:01:02.92553 Server listening on 0.0.0.0 port 22.
2019-03-14_13:01:02.92555 Server listening on :: port 22.
2019-03-14_13:02:11.29316 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:02:11.29339 @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
2019-03-14_13:02:11.29353 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:02:11.29371 Permissions 0755 for '/etc/gitlab/ssh_host_rsa_key' are too open.
2019-03-14_13:02:11.29394 It is required that your private key files are NOT accessible by others.
2019-03-14_13:02:11.29411 This private key will be ignored.
2019-03-14_13:02:11.29429 key_load_private: bad permissions
2019-03-14_13:02:11.29447 Could not load host key: /etc/gitlab/ssh_host_rsa_key
2019-03-14_13:02:11.29501 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:02:11.29510 @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
2019-03-14_13:02:11.29527 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
2019-03-14_13:02:11.29545 Permissions 0755 for '/etc/gitlab/ssh_host_ecdsa_key' are too open.
2019-03-14_13:02:11.29555 It is required that your private key files are NOT accessible by others.
2019-03-14_13:02:11.29569 This private key will be ignored.
2019-03-14_13:02:11.29588 key_load_private: bad permissions
2019-03-14_13:02:11.29674 Could not load host key: /etc/gitlab/ssh_host_ecdsa_key
~~~

执行 gitlab-ctl reconfigure 或者 

~~~bash
chmod 0600 /etc/gitlab/gitlab.rb
chmod 0600 /etc/gitlab/ssh_host_rsa_key
chmod 0600 /etc/gitlab/ssh_host_ecdsa_key
chmod 0600 /etc/gitlab/ssh_host_ed25519_key
~~~

后就可以恢复正常访问了，但是一旦NAS盘被重新挂载几个文件权限就又被改到755了，后来发现是在挂载NAS的时候指定了`mode:755`，
根据阿里云官方的文档说明，如果指定了mode那么挂载点在被挂载的时候会执行 `chmod -R`的操作，所以文件权限会被更改，而且文件量比较大的时候会造成挂载缓慢。




