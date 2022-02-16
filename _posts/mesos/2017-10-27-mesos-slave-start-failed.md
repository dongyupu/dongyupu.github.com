---
layout: post
title: "mesos-slave.service: main process exited, code=exited, status=1/FAILUR"
subtitle:   "Unit mesos-slave.service entered failed state.mesos-slave.service failed."
date:       2017-10-27 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

### slave（agent） 启动失败

~~~bash
[root@192 zookeeper]# systemctl status mesos-slave 
● mesos-slave.service - Mesos Slave
   Loaded: loaded (/usr/lib/systemd/system/mesos-slave.service; enabled; vendor preset: disabled)
   Active: activating (auto-restart) (Result: exit-code) since 五 2017-10-27 11:07:06 CST; 7s ago
  Process: 24286 ExecStart=/usr/bin/mesos-init-wrapper slave (code=exited, status=1/FAILURE)
 Main PID: 24286 (code=exited, status=1/FAILURE)
   Memory: 16.0K
   CGroup: /system.slice/mesos-slave.service

10月 27 11:07:06 192.168.1.91 systemd[1]: mesos-slave.service: main process exited, code=exited, status=1/FAILURE
10月 27 11:07:06 192.168.1.91 systemd[1]: Unit mesos-slave.service entered failed state.
10月 27 11:07:06 192.168.1.91 systemd[1]: mesos-slave.service failed.

~~~

* 查看详细错误信息

~~~bash
[root@192 zookeeper]#  journalctl -f -u  mesos-slave
-- Logs begin at 五 2017-10-27 08:28:12 CST. --
10月 27 11:10:50 192.168.1.91 mesos-slave[25406]: I1027 11:10:50.738179 25411 zookeeper.cpp:262] A new leading master (UPID=master@192.168.1.91:5050) is detected
10月 27 11:10:50 192.168.1.91 mesos-slave[25406]: E1027 11:10:50.792980 25414 slave.cpp:6286] EXIT with status 1: Failed to perform recovery: Collect failed: Failed to run 'docker -H unix:///var/run/docker.sock ps -a': exited with status 1; stderr='Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
10月 27 11:10:50 192.168.1.91 mesos-slave[25406]: '
10月 27 11:10:50 192.168.1.91 mesos-slave[25406]: To remedy this do as follows:
10月 27 11:10:50 192.168.1.91 mesos-slave[25406]: Step 1: rm -f /var/lib/mesos/meta/slaves/latest
10月 27 11:10:50 192.168.1.91 mesos-slave[25406]:         This ensures agent doesn't recover old live executors.
10月 27 11:10:50 192.168.1.91 mesos-slave[25406]: Step 2: Restart the agent.
10月 27 11:10:50 192.168.1.91 systemd[1]: mesos-slave.service: main process exited, code=exited, status=1/FAILURE
10月 27 11:10:50 192.168.1.91 systemd[1]: Unit mesos-slave.service entered failed state.
10月 27 11:10:50 192.168.1.91 systemd[1]: mesos-slave.service failed.

~~~

* `Is the docker daemon running?` 需要docker的守护进程，所以启动docker服务，然后

~~~bash
systemctl start docker.service
~~~

* 这样就可以启动成功了。

~~~bash
[root@192 zookeeper]# systemctl start mesos-slave 
[root@192 zookeeper]# systemctl status mesos-slave 
● mesos-slave.service - Mesos Slave
   Loaded: loaded (/usr/lib/systemd/system/mesos-slave.service; enabled; vendor preset: disabled)
   Active: active (running) since 五 2017-10-27 11:12:12 CST; 7s ago
 Main PID: 25976 (mesos-slave)
   Memory: 6.5M
   CGroup: /system.slice/mesos-slave.service
           ├─25976 /usr/sbin/mesos-slave --master=zk://192.168.1.91:2181,192.168.1.90:2181,192.168.1.92:2181/mesos --log_dir=/var/log/mes...
           ├─25990 logger -p user.info -t mesos-slave[25976]
           └─25991 logger -p user.err -t mesos-slave[25976]

10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.475162 25992 detector.cpp:152] Detected a new leader: (id='21')
10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.475375 25992 group.cpp:700] Trying to get '/mesos/json.info_00000...Keeper
10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.482832 25992 zookeeper.cpp:262] A new leading master (UPID=master...tected
10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.482908 25992 slave.cpp:971] New master detected at master@192.168...1:5050
10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.482919 25992 slave.cpp:995] No credentials provided. Attempting t...cation
10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.482931 25992 slave.cpp:1006] Detecting new master
10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.482949 25992 status_update_manager.cpp:177] Pausing sending status updates
10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.950929 25995 slave.cpp:1286] Re-registered with master master@192...1:5050
10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.950951 25995 slave.cpp:1323] Forwarding total oversubscribed resources {}
10月 27 11:12:15 192.168.1.91 mesos-slave[25991]: I1027 11:12:15.951009 25995 status_update_manager.cpp:184] Resuming sending stat...pdates
Hint: Some lines were ellipsized, use -l to show in full.
[root@192 zookeeper]# 
~~~













