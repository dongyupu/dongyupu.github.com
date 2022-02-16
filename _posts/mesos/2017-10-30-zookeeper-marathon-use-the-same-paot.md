---
layout: post
title: "marathon > java.net.BindException: Address already in use"
subtitle:   "zookeeper and marathon use the same port,this will make marathon.service failed."
date:       2017-10-30 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

## marathon 启动失败

* 查看失败信息

~~~bash
 journalctl -f -u  marathon
 ...
10月 30 08:14:57 192.168.1.91 marathon[10486]: java.net.BindException: Address already in use
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at sun.nio.ch.Net.bind0(Native Method)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at sun.nio.ch.Net.bind(Net.java:433)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at sun.nio.ch.Net.bind(Net.java:425)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at org.eclipse.jetty.server.ServerConnector.open(ServerConnector.java:326)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at org.eclipse.jetty.server.AbstractNetworkConnector.doStart(AbstractNetworkConnector.java:80)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at org.eclipse.jetty.server.ServerConnector.doStart(ServerConnector.java:244)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at org.eclipse.jetty.server.Server.doStart(Server.java:384)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at org.eclipse.jetty.util.component.AbstractLifeCycle.start(AbstractLifeCycle.java:68)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at mesosphere.chaos.http.HttpService.startUp(HttpService.scala:20)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at mesosphere.chaos.http.HttpService$$EnhancerByGuice$$35270d8f.CGLIB$startUp$1(<generated>)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at mesosphere.chaos.http.HttpService$$EnhancerByGuice$$35270d8f$$FastClassByGuice$$9fc472c9.invoke(<generated>)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at com.google.inject.internal.cglib.proxy.$MethodProxy.invokeSuper(MethodProxy.java:228)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at com.google.inject.internal.InterceptorStackCallback$InterceptedMethodInvocation.proceed(InterceptorStackCallback.java:72)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at mesosphere.marathon.DebugModule$MetricsBehavior$$anonfun$invoke$1.apply(DebugConf.scala:82)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at mesosphere.marathon.metrics.Metrics.timed(Metrics.scala:28)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at mesosphere.marathon.DebugModule$MetricsBehavior.invoke(DebugConf.scala:82)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at com.google.inject.internal.InterceptorStackCallback$InterceptedMethodInvocation.proceed(InterceptorStackCallback.java:72)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at com.google.inject.internal.InterceptorStackCallback.intercept(InterceptorStackCallback.java:52)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at mesosphere.chaos.http.HttpService$$EnhancerByGuice$$35270d8f.startUp(<generated>)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at com.google.common.util.concurrent.AbstractIdleService$DelegateService$1.run(AbstractIdleService.java:60)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at com.google.common.util.concurrent.Callables$3.run(Callables.java:100)
10月 30 08:14:57 192.168.1.91 marathon[10486]:         at java.lang.Thread.run(Thread.java:748)
...
10月 30 08:14:58 192.168.1.91 systemd[1]: marathon.service: main process exited, code=exited, status=137/n/a
10月 30 08:14:58 192.168.1.91 systemd[1]: Unit marathon.service entered failed state.
10月 30 08:14:58 192.168.1.91 systemd[1]: marathon.service failed.
~~~

* 这种情况一般是端口被占用了
  * `java.net.BindException: Address already in use `
  * `at org.eclipse.jetty.server.ServerConnector.open(ServerConnector.java:326)`


* 查看端口占用情况

~~~bash
[root@192 ~]# netstat -plten |grep 8080
tcp6       0      0 :::8080                 :::*                    LISTEN      0          1350684    23149/java        

[root@192 ~]# ps -ef |grep 23149
root     11628 10358  0 08:22 pts/0    00:00:00 grep --color=auto 23149
root     23149     1  0 10月27 ?      00:09:47 java -Dzookeeper.log.dir=/usr/local/zookeeper/bin/../logs -Dzookeeper.log.file=zookeeper-root-server-192.168.1.91.log -Dzookeeper.root.logger=INFO,CONSOLE -XX:+HeapDumpOnOutOfMemoryError -XX:OnOutOfMemoryError=kill -9 %p -cp /usr/local/zookeeper/bin/../build/classes:/usr/local/zookeeper/bin/../build/lib/*.jar:/usr/local/zookeeper/bin/../lib/slf4j-log4j12-1.7.5.jar:/usr/local/zookeeper/bin/../lib/slf4j-api-1.7.5.jar:/usr/local/zookeeper/bin/../lib/netty-3.10.5.Final.jar:/usr/local/zookeeper/bin/../lib/log4j-1.2.17.jar:/usr/local/zookeeper/bin/../lib/jline-2.11.jar:/usr/local/zookeeper/bin/../lib/jetty-util-9.2.18.v20160721.jar:/usr/local/zookeeper/bin/../lib/jetty-servlet-9.2.18.v20160721.jar:/usr/local/zookeeper/bin/../lib/jetty-server-9.2.18.v20160721.jar:/usr/local/zookeeper/bin/../lib/jetty-security-9.2.18.v20160721.jar:/usr/local/zookeeper/bin/../lib/jetty-io-9.2.18.v20160721.jar:/usr/local/zookeeper/bin/../lib/jetty-http-9.2.18.v20160721.jar:/usr/local/zookeeper/bin/../lib/javax.servlet-api-3.1.0.jar:/usr/local/zookeeper/bin/../lib/jackson-mapper-asl-1.9.11.jar:/usr/local/zookeeper/bin/../lib/jackson-core-asl-1.9.11.jar:/usr/local/zookeeper/bin/../lib/commons-cli-1.2.jar:/usr/local/zookeeper/bin/../zookeeper-3.5.3-beta.jar:/usr/local/zookeeper/bin/../src/java/lib/*.jar:/usr/local/zookeeper/bin/../conf: -Xmx1000m -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.local.only=false org.apache.zookeeper.server.quorum.QuorumPeerMain /usr/local/zookeeper/bin/../conf/zoo.cfg
~~~ 

* 发现是zookeeper的进程占用了8080端口，导致marathon启动失败。搜索之后发现，zookeeper在新版本中增加了内嵌的管理控制台是通过jetty启动，也会占用8080 端口。

* 通过查看zookeeper的官方文档，发现有3种解决途径：
  * 1.删除jetty。
  * 2.修改端口。
    * 修改方法的方法有两种，一种是在启动脚本中增加 -Dzookeeper.admin.serverPort=你的端口号.一种是在zoo.cfg中增加admin.serverPort=没有被占用的端口号
  * 3.停用这个服务，在启动脚本中增加"-Dzookeeper.admin.enableServer=false"
  
* 我通过修改zoo.cfg中的参数后重启zookeeper，然后marathon就可以正常启动了。

 




