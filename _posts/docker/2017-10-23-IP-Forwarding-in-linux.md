---
layout: post
title: "How to Enable IP Forwarding in Linux"
subtitle:   "docker WARNING: IPv4 forwarding is disabled. Networking will not work."
date:       2017-10-23 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-04.jpg"
---

### Centos7 启动 docker镜像时报的错误,导致镜像中的web服务无法访问

~~~bash
I1023 10:58:31.075482 32368 exec.cpp:162] Version: 1.4.0
I1023 10:58:31.077666 32370 exec.cpp:237] Executor registered on agent 224f89b4-8c19-4909-a343-8f4380f4037a-S0
I1023 10:58:31.078027 32370 executor.cpp:120] Registered docker executor on 192.168.1.90
I1023 10:58:31.078045 32370 executor.cpp:160] Starting task nginx.0d04229c-b79e-11e7-9cd8-02423d8425a9

WARNING: IPv4 forwarding is disabled. Networking will not work.
~~~

* IP-Forwarding    IP转发。 一种路由协议。
* IP转发是操作系统的一种选项，支持主机起到路由器的功能。在一个系统中含有两块以上的网卡，并将IP转发选项打开，这样该系统就可以作为路由器进行使用了。

<br>

[开启IP-Forwarding](http://www.ducea.com/2006/08/01/how-to-enable-ip-forwarding-in-linux/)


* By default any modern Linux distributions will have IP Forwarding disabled. This is normally a good idea, as most peoples will not need IP Forwarding, but if we are setting up a Linux router/gateway or maybe a VPN server (pptp or ipsec) or just a plain dial-in server then we will need to enable forwarding. This can be done in several ways that I will present bellow.

### Check if IP Forwarding is enabled

* We have to query the `sysctl kernel` value `net.ipv4.ip_forward` to see if forwarding is enabled or not: Using `sysctl`:

~~~bash
sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 0
~~~

or just checking out the value in the `/proc` system:

~~~bash
cat /proc/sys/net/ipv4/ip_forward
0
~~~

As we can see in both the above examples this was disabled (as show by the value 0).

### Enable IP Forwarding on the fly

As with any sysctl kernel parameters we can change the value of net.ipv4.ip_forward on the fly (without rebooting the system):
~~~bash
sysctl -w net.ipv4.ip_forward=1
~~~

or

~~~bash
echo 1 > /proc/sys/net/ipv4/ip_forward
~~~

the setting is changed instantly; the result will not be preserved after rebooting the system.

### Permanent setting using /etc/sysctl.conf

* If we want to make this configuration permanent the best way to do it is using the file /etc/sysctl.conf where we can add a line containing net.ipv4.ip_forward = 1

~~~bash
/etc/sysctl.conf:
net.ipv4.ip_forward = 1
~~~ 

if you already have an entry net.ipv4.ip_forward with the value 0 you can change that 1.

To enable the changes made in sysctl.conf you will need to run the command:

~~~bash
sysctl -p /etc/sysctl.conf
~~~

On RedHat based systems this is also enabled when restarting the network service:

~~~bash
service network restart
~~~

and on Debian/Ubuntu systems this can be also done restarting the procps service:
~~~bash
/etc/init.d/procps.sh restart
~~~

### Using distribution specific init scripts

* Although the methods presented above should work just fine and you would not need any other method of doing this, I just wanted to note that there are also other methods to enable IP Forwarding specific to some Linux distributions. For example Debian based distributions might use the setting:

~~~bash
/etc/network/options:
ip_forward=no
~~~

set it to yes and restart the network service. Also RedHat distributions might set this using:

~~~bash
/etc/sysconfig/network:
FORWARD_IPV4=true
~~~

and again restart the network service.

* Regardless the method you have used once you have completed this you can check it out using the same method shown above:

~~~bash
sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 1

cat /proc/sys/net/ipv4/ip_forward
1
~~~

If the result is 1 then the Linux system will start forwarding IP packets even if they are not destined to any of its own network interfaces.






