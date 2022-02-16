---
layout: post
title: "pgrep和pkill精准查找和结束进程"
subtitle:   "pgrep和pkill精准查找和结束进程"
date:       2018-08-13 2:21:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

* 在jenkins中使用ssh 远程执行命令结束进程时，经常需要获取进程pid然后使用kill命令来结束进程



* 所有我们转而使用pgrep -f 命令来查找进程

### pgrep命令
* 以名称为依据从运行进程队列中查找进程，并显示查找到的进程id。每一个进程ID以一个十进制数表示，通过一个分割字符串和下一个ID分开，默认的分割字符串是一个新行。对于每个属性选项，用户可以在命令行上指定一个以逗号分割的可能值的集合。

* 语法
* pgrep(选项)(参数)
* 选项
* -o：仅显示找到的最小（起始）进程号；
* -n：仅显示找到的最大（结束）进程号；
* -l：显示进程名称；
* -P：指定父进程号；
* -g：指定进程组；
* -t：指定开启进程的终端；
* -u：指定进程的有效用户ID。
* 参数
* 进程名称：指定要查找的进程名称，同时也支持类似grep指令中的匹配模式

### pkill命令
* 可以按照进程名杀死进程。pkill和killall应用方法差不多，也是直接杀死运行中的程序；如果您想杀掉单个进程，请用kill来杀掉。

* 语法
* pkill(选项)(参数)
* 选项
* -o：仅向找到的最小（起始）进程号发送信号；
* -n：仅向找到的最大（结束）进程号发送信号；
* -P：指定父进程号发送信号；
* -g：指定进程组；
* -t：指定开启进程的终端。
* 参数
* 进程名称：指定要查找的进程名称，同时也支持类似grep指令中的匹配模式。



* -f 正则表达式模式将执行与完全进程参数字符串 (从/proc/nnnnn/psinfo文件的pr_psargs字段获得)匹配。如果没有 -f 选项，表达式仅对执行文件名称(从/proc/nnnnn/psinfo文件pr_fname字段获得)匹配。

*  示例

~~~
pkill -f /www/webcode/channel-admin/conf

pgrep -f /www/webcode/channel-admin/conf
~~~

~~~
set -x
if $Compile
then
    export JAVA_HOME=/app/jenkins-home/tools/hudson.model.JDK/JDK7/jdk1.7.0_80
    /app/jenkins_workspace/tools/hudson.tasks.Maven_MavenInstallation/maven3.5.2/bin/mvn -v
    cd channel-admin
    /app/jenkins_workspace/tools/hudson.tasks.Maven_MavenInstallation/maven3.5.2/bin/mvn clean -Dmaven.test.skip=true install -X
fi
cd $WORKSPACE
 
if $Deploy
then
    scp channel-admin/target/*.war appuser@10.31.153.59:/www/webcode/channel-admin/webapps
 
    ssh -Tq appuser@10.31.153.59 <<EOF
    set -ex
    export JAVA_HOME=/app/jdk7;
    #stop app
    echo "开始部署 10.25.114.193"
    sh -x /www/webcode/channel-admin/bin/shutdown.sh >/dev/null 2>&1
    sleep 10
    #check stop（检查应用是否停掉）
    ps -ef |grep /www/webcode/channel-admin/conf|grep -v grep |awk '{print \$2}'
    pkill -f /www/webcode/channel-admin/conf
 
    #backup
    mv /www/webcode/channel-admin/webapps/ROOT /www/webcode/channel-admin/webapps/ROOT_$(date "+%Y%m%d%H%M%S")
     
    #deploy（部署）
    cd /www/webcode/channel-admin/webapps
    #tar -xf ROOT_${BUILD_ID}.tar
    #cp -rf /config.properties /app/wkCheck/apache-tomcat-8.5.24/webapps/ROOT/WEB-INF/classes/
     
    #Clear cache（清理缓存）
    rm -rf /www/webcode/channel-admin/work/*
    rm -rf /www/webcode/channel-admin/temp/*
    rm -rf /www/webcode/channel-admin/logs/*
    #mv /app/wkCheck/apache-tomcat-8.5.24/logs /app/wkCheck/apache-tomcat-8.5.24/log_$(date "+%Y-%m-%d-%H-%M-%S")
    #mkdir /app/wkCheck/apache-tomcat-8.5.24/logs
    #start（启服务）
    sh /www/webcode/channel-admin/bin/startup.sh >/dev/null 2>&1
    sleep 10
    tail -5000 /www/webcode/channel-admin/logs/catalina.out
    echo "获取到的进程已启动";
    ps -ef |grep /www/webcode/channel-admin/conf|grep -v grep |awk '{print \$2}'
    pgrep -f /www/webcode/channel-admin/conf
    exit
     
EOF
fi

~~~
