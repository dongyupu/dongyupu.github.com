---
layout: post
title: "iptables -nL"
subtitle:   "iptables -nL 查看规则"
date:       2018-04-17 2:21:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
docker 添加的iptables规则，如果iptables重启，docker添加的规则就会被情况，要么自己手动添加，要么重启docker守护进程。
这个坑有点大。

~~~bash
# iptables -nL
Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
DOCKER-USER  all  --  0.0.0.0/0            0.0.0.0/0           
DOCKER-ISOLATION  all  --  0.0.0.0/0            0.0.0.0/0           
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            ctstate RELATED,ESTABLISHED
DOCKER     all  --  0.0.0.0/0            0.0.0.0/0           
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         

Chain DOCKER (1 references)
target     prot opt source               destination         
ACCEPT     tcp  --  0.0.0.0/0            192.168.0.2          tcp dpt:50000
ACCEPT     tcp  --  0.0.0.0/0            192.168.0.2          tcp dpt:8080

Chain DOCKER-ISOLATION (1 references)
target     prot opt source               destination         
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           

Chain DOCKER-USER (1 references)
target     prot opt source               destination         
RETURN     all  --  0.0.0.0/0            0.0.0.0/0          
~~~



