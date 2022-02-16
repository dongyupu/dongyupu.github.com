---
layout: post
title: "Jenkins node 节点 slave 配置"
subtitle:   "Jenkins node slaves(windows & linux)"
date:       2016-04-10 10:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---


### 启动方法
- Controls how Jenkins starts this slave.
  - Launch slave agents on Unix machines via SSH
    - Starts a slave by sending commands over a secure SSH connection. The slave needs to be reachable from the master, and you will have to supply an account that can log in on the target machine. No root privileges are required.
  - Launch slave agents via Java Web Start
    - Starts a slave by launching an agent program through JNLP. The launch in this case is initiated by the slave, thus slaves need not be IP reachable from the master (e.g. behind the firewall.) It is still possible to start a launch without GUI, for example as a Windows service.
  - Launch slave via execution of command on the Master
    - Starts a slave by having Jenkins execute a command from the master. Use this when the master is capable of remotely executing a process on a slave, such as through ssh/rsh.
  - Let Jenkins control this Windows slave as a Windows service
    - Starts a Windows slave by a remote management facility built into Windows. Suitable for managing Windows slaves. Slaves need to be IP reachable from the master.

### jenkins node Linux slave 

- Linux 相当简单些，启动方法选择 ssh用户名+密码就可以了

<img src="{{ site.baseurl }}/img/2016/node-slave-linux.JPG" />



### jenkins node windows slave

- windows 我尝试 Let Jenkins control this Windows slave as a Windows service 的方式并未成功 Access is denied. [0x00000005] 

- 然后尝试了 Launch slave agents via Java Web Start 这个是可以的，就是要在slave机器上手动运行agent，并且需要在Java控制台>安全>编辑例外站点>添加 你的jenkins URL  [参考这个文章](http://www.cnblogs.com/itech/archive/2011/11/09/2243025.html)

<img src="{{ site.baseurl }}/img/2016/slave-web-start.JPG" />

<img src="{{ site.baseurl }}/img/2016/java-site.JPG" />

<img src="{{ site.baseurl }}/img/2016/slave-agent.JPG" />

<img src="{{ site.baseurl }}/img/2016/node-slave-windows.JPG" />


