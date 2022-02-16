---
layout: post
title: "Jenkins通过ssh命令 启动agent失败"
subtitle:   "Jenkins通过ssh命令 启动agent失败"
date:       2018-10-16 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---

启动代理节点时抛出如下异常，这个错误是由于JDK的版本的问题，换成 OpenJDK 即可

~~~bash
Exception in thread "main" java.lang.ClassFormatError: hudson.remoting.Launcher (unrecognized class file version)
~~~

Centos  JDK安装及环境变量设置
查看yum库中都有哪些jdk版本

~~~bash
yum search java|grep jdk
yum install java-1.8.0-openjdk java-1.8.0-openjdk-devel
~~~

设置环境变量

~~~bash
vi /etc/profile

JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.151-1.b12.el6_9.x86_64
JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=$JRE_HOME/lib:$JAVA_HOME/lib/:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
~~~

让修改生效

~~~bash
source /etc/profile
~~~

查看版本

~~~bash
java –version
javac -version
~~~
