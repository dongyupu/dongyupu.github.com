---
layout: post
title: "Centos7 run shell during boot"
subtitle:   "to run scripts during boot"
date:       2017-10-11 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---
Setup 1 <br>
  touch your_shell.sh
  
  ~~~bash
  #!/bin/bash
  ./review-site/bin/gerrit.sh restart
  service nginx restart
  ~~~
Setup 2 <br>

  ~~~bash
  add executed permission
  chmod +x path_to_your_shell.sh
  ~~~
  
Setip 3 <br>

  ~~~bash
  edit /etc/rc.d/rc.local ,add shell to run 
  eg:/home/gerrit/start_gerrit_nginx.sh
  ~~~
  
Setp 4 <br>

  Read  /etc/rc.d/rc.local  <br>
  chmod +x /etc/rc.d/rc.local <br>
  
  ~~~bash
  Centos 7
  #!/bin/bash
  # THIS FILE IS ADDED FOR COMPATIBILITY PURPOSES
  #
  # It is highly advisable to create own systemd services or udev rules
  # to run scripts during boot instead of using this file.
  #
  # In contrast to previous versions due to parallel execution during boot
  # this script will NOT be run after all other services.
  #
  # Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
  # that this script will be executed during boot.
  ~~~

  ~~~bash
  Centos 6.5
  #!/bin/sh
  #
  # This script will be executed *after* all the other init scripts.
  # You can put your own initialization stuff in here if you don't
  # want to do the full Sys V style init stuff.
  ~~~
  
End reboot system

