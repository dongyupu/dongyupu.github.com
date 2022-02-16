---
layout: post
title: "docker network"
subtitle:   "docker netwrk"
date:       2017-11-3 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

* 3.1 查看容器的IP地址
  * eg: docker inspect --format '{{ .NetworkSettings.IPAddress }}' nginx
  * 命令行 ip add | grep global  或  cat /etc/hosts
  
* 3.2 将容器端口暴露到主机上
  * docker run的 -p 选项将容器内的端口动态绑定到宿主机上。
  * 端口映射通过两种机制实现
    * 首先，默认情况下docker会修改宿主机的iptables 规则。 查看命令 iptables -L 
    * 其次，docker会在宿主机上启动一个轻量级代理程序。这个进程监听宿主机的所有网络接口，监听端口是为容器动态分配的端口。查看系统进程，就能看到这个代理程序。 ps -ef | grep docker
    * 可以修改这些默认设置来禁止docker修改iptables，这时你必须自己处理网络相关功能。
  
* 3.3 容器连接
  * 在单台主机上 --link 即可，但在一个大规模系统中，需要使用其他的服务发现方式。键值存储 和 DNS 的解决方案是很不错的选择。
  * 如果你重启了一个容器，那么这个容器的 /etc/hosts 文件会自动更新，而其环境变量保持不变。因此，推荐使用 /etc/hosts 文件对所链接的容器进行IP地址解析
  * 当你启动容器时，可以为每个容器都设置名称。容器名称可以用来定义容器链接。格式: --link <container_name>:<alias>
  * 基于DNS 或 动态容器注册机制的系统具备扩展性和自动更新功能。

* 3.4 理解docker容器网络
  * 在默认情况下，docker会在宿主机上创建一个名为 docker0 的linux网桥设备。该网桥设备拥有一个私有网络地址地址及其所属子网。
  * 所有容器都会链接到该网桥设备上，并从中分配一个位于子网中的IP地址。容器链接到网桥的网络接口会把docker0网络设备作为网关。
  * 创建新容器时，docker会创建一对网络设备接口，并将它们放到两个独立的网络命名空间：一个网络设备放到容器的网络命名空间（即 eth0）；另一个网络设备放到宿主机的网络命名空间，并链接到docker0 网桥设备上。
  * ip命令 ： ip -d link show
  * 如果安装了 bridge-utils 包，也可以使用 brctl工具。 eg： brctl show
* 3.4.3 讨论
  * 访问外部的网络流量将会通过IP转发功能转发到docker主机的其他网络接口上，并通过 iptables 的地址伪装规则进行NAT地址转换。
    * cat /proc/sys/net/ipv4/ip_forward
  * 如果关闭IP转发功能，你就会发现容器将不能再链接到外部网络。
  * 查看为外部通信而创建的用于IP伪装的NAT规则 iptables -t nat -L
  
* 3.5 选择容器网络模式
  * 通过 --net 选项 选择 容器启动时的网络模式 
    * none 不带任何网络功能的容器
    * host 使用与宿主机相同的网络 不建议使用这种模式，可能会导致你在容器中不小心对主机中的网络进行了修改。但是这种模式非常适合那些对网络I/O性能特别敏感的进程。
    * container:CONTAINER_OR_ID 与已经启动的容器共享一个网络命名空间。
  
* 3.6 配置docker守护进程iptables和IP转发设置
  * --ip-forward 和 --iptables
  * --icc=false 这将会为docker网桥上的数据包添加一个默认丢弃的规则，容器之间也不能互相访问。

* 3.7 通过pipework理解容器网络
  * pipework 通过 cgroups 和 网络命名空间 来创建容器网络。
  * 阅读一下它的bash脚本，可以学习到创建一个容器网络所需的所有详细步骤。
  
* 3.8 定制docker网桥设备

* 3.9 在docker中使用OVS
  * 虚拟交换机软件 Open Switch （OVS）
  
* 3.10 在docker主机间创建GRE隧道
  * 你需要在位于不同docker主机上的容器中使用它们独立的IP地址进行网络通信。
  * 可以构建一个通用路由封装（Generic Routing Encapsulation GRE）来对IPv4通信进行封装，并为容器之间互连提供基于容器私有地址的路由。
  
* 3.11 在weave网络上运行容器
  * 你想要为跨越多个数据中心的、规模从单台到数千台的主机上运行的容器创建一个网络，该网络具备自动IP地址分配功能，并且集成基于DNS的服务发现。
  * host1$ weave launch
  * host2$ weave launch host1_ip
  
  * scope launch
  
* 3.11.4 在AWS上运行 Weave网络

* 3.13 在docker主机上部署flannel覆盖网络

* 3.14 在多台docker主机中使用docker network
  * VXLAN
  * 使用consul服务来存储 键值
  
* 3.15 深入docker network命名空间配置

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
