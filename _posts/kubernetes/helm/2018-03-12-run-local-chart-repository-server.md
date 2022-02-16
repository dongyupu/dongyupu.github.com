---
layout: post
title: "start a local helm http web server"
subtitle:   "starts a local chart repository server that serves charts from a local directory."
date:       2018-3-12 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

以我们的server为例，如果启动server时不指定ip地址的话，就只能从本地（127.0.0.1:8879）访问
<br>
This command starts a local chart repository server that serves charts from a local directory.
<br>
The new server will provide HTTP access to a repository. By default, it will scan all of the charts in ‘$HELM_HOME/repository/local’ and serve those over the local IPv4 TCP port (default ‘127.0.0.1:8879’).
<br>

~~~bash
helm serve
~~~

这样启动server的话，无法从其他机器安装在这台server上提交到chart，所以需要指定server的ip
<br>
官方文档参考：

### Options
~~~yml
      --address string     address to listen on (default "127.0.0.1:8879")
      --repo-path string   local directory path from which to serve charts
      --url string         external URL of chart repository
~~~

<br>
run_helm_server.sh

~~~bash
nohub helm serve --address your_ip:8879 &
~~~

比如：如果启动server时指定ip如下

~~~bash
nohup helm serve --address 10.25.112.37:8879 &
~~~

查看自己的本地库

~~~bash
helm repo list

NAME     	URL                                                                      
local    	http://127.0.0.1:8879/charts                                        
~~~

查找本地存储的chart

~~~bash
helm search bus
NAME                  	VERSION	DESCRIPTION                                  
local/bus-merchant    	0.1.0  	BUS merchant Helm chart for Kubernetes       
local/bus-merchant-dev	2.0    	bus merchant system Helm chart for Kubernetes
~~~

安装本地库提交的chart

~~~bash
helm install --name "bus-mechant-dev-${BUILD_ID}" --version bus-merchant-2.0 http://10.25.112.37:8879/charts/bus-merchant-dev-2.0.tgz
~~~

安装的chart路径指向server的ip即可。





