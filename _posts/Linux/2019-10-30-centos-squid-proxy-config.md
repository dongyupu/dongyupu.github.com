---
layout: post
title: "centos squid proxy"
subtitle:   "centos squid proxy"
date:       2019-10-30 2:21:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
* https://blog.csdn.net/qingfengxulai/article/details/80853870

## Server端配置
### Squid介绍

Squid是Linux自带的代理软件，与其它代理软件如Apache、Socks等相比，下载安装简单，配置灵活，支持缓存和多种协议。

* 安装

~~~
yum install squid -y
yum install httpd-tools -y
~~~

* 生成密码文件

~~~
touch /etc/squid/passwords
htpasswd -cd /etc/squid3/passwords dongyupu
~~~

* 测试密码文件

~~~
/usr/lib64/squid/basic_ncsa_auth /etc/squid3/passwords
~~~

* 配置squid.conf文件

~~~
vi /etc/squid/squid.conf
/etc/squid/squid.conf
#
# Recommended minimum configuration:
#

# Example rule allowing access from your local networks.
# Adapt to list your (internal) IP networks from where browsing
# should be allowed
#acl localnet src 10.0.0.0/8	# RFC1918 possible internal network
#acl localnet src 172.16.0.0/12	# RFC1918 possible internal network
#acl localnet src 192.168.0.0/16	# RFC1918 possible internal network
acl localnet src fc00::/7       # RFC 4193 local private network range
acl localnet src fe80::/10      # RFC 4291 link-local (directly plugged) machines

# test acl 从这里设置允许的ip
acl localnet src 172.16.87.19

acl SSL_ports port 443
acl Safe_ports port 80		# http
acl Safe_ports port 21		# ftp
acl Safe_ports port 443		# https
acl Safe_ports port 70		# gopher
acl Safe_ports port 210		# wais
acl Safe_ports port 1025-65535	# unregistered ports
acl Safe_ports port 280		# http-mgmt
acl Safe_ports port 488		# gss-http
acl Safe_ports port 591		# filemaker
acl Safe_ports port 777		# multiling http
acl CONNECT method CONNECT

#
# Recommended minimum Access Permission configuration:
#
# Deny requests to certain unsafe ports
http_access deny !Safe_ports

# Deny CONNECT to other than secure SSL ports
http_access deny CONNECT !SSL_ports

# Only allow cachemgr access from localhost
http_access allow localhost manager
http_access deny manager

# We strongly recommend the following be uncommented to protect innocent
# web applications running on the proxy server who think the only
# one who can access services on "localhost" is a local user
#http_access deny to_localhost

#
# INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
#

# Example rule allowing access from your local networks.
# Adapt localnet in the ACL section to list your (internal) IP networks
# from where browsing should be allowed
http_access allow localnet
http_access allow localhost

# And finally deny all other access to this proxy
http_access deny all

# Squid normally listens to port 3128
http_port 3128

# Uncomment and adjust the following to add a disk cache directory.
#cache_dir ufs /var/spool/squid 100 16 256

# Leave coredumps in the first cache dir
coredump_dir /var/spool/squid

#
# Add any of your own refresh_pattern entries above these.
#
refresh_pattern ^ftp:		1440	20%	10080
refresh_pattern ^gopher:	1440	0%	1440
refresh_pattern -i (/cgi-bin/|\?) 0	0%	0
refresh_pattern .		0	20%	4320


#auth config
#auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwords
#auth_param basic realm proxy
#auth_param basic children 5
#auth_param basic credentialsttl 24 hours
#acl authenticated proxy_auth REQUIRED
#acl allowed_ips src "/etc/squid/allowed_ips.txt"
http_access allow localnet
http_access allow localhost
#http_access allow allowed_ips  #不知道为什么这么设置不生效
http_port 2233
http_access deny all
#http_access allow authenticated

~~~

* 日志

`squid的日志位于/var/log/squid/目录下。`

* 启动

~~~
#启动start（停止stop) 
systemctl start squid.service
#配置开机自启动
systemctl enable squid.service
~~~

* Linux客户端

全局代理

~~~
vi /etc/profile
#在最后加入
export http_proxy="http://xiaodong:123456@proxy_ip:port"
export http_proxy="http://xiaodong:123456@proxy_ip:port"
~~~

* yum代理

~~~
vi /etc/yum.conf
# Proxy
proxy=http://username:password@proxy_ip:port/
~~~

* Windows客户端

windows客户端通过全局代理上网，建议采用Proxifier软件。Proxifier是一款功能非常强大的socks5客户端。
