---
layout: post
title: "centos7中修改docker默认保存位置"
subtitle:   "centos7中修改docker默认保存位置"
date:       2018-1-30 4:45:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
centos7中修改docker默认保存位置
编辑`/usr/lib/systemd/system/docker.service `

~~~yml
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required


# for containers run by docker `#change storage dir to /app/docker --graph /app/docker` 
ExecStart=/usr/bin/dockerd --graph /app/docker


ExecReload=/bin/kill -s HUP $MAINPID

# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
#TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process, not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target

~~~



