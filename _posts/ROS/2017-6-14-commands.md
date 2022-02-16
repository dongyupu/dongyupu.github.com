---
layout: post
title: "ROS(Robot Operating System) commands "
subtitle:   "Robot Operating System commands"
date:       2017-6-13 0:00:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
[核心 ROS 教程](http://wiki.ros.org/cn/ROS/Tutorials)

## rosclean

~~~bash
Usage: rosclean <command>

Commands:
  rosclean check        Check usage of log files
  rosclean purge        Remove log files
~~~

* rosclean check

Report the disk usage of ROS log files.

~~~bash
$ rosclean check
348K ROS node logs
20K rosmake logs
~~~

* rosclean purge

rosclean purge will remove directories associated with storing ROS-related log files. You will be asked to confirm each deletion and it is important that you verify the command that rosclean purge executes is correct. Otherwise you may lose important files.

~~~bash
$ rosclean purge
Purging ROS node logs.
PLEASE BE CAREFUL TO VERIFY THE COMMAND BELOW!
Okay to execute:

rm -rf /u/username/.ros/log
(y/n)?
~~~

## rosmake
for old build tools

## catkinmake
new build tool

## roscore
roscore is a collection of nodes and programs that are pre-requisites of a ROS-based system. You must have a roscore running in order for ROS nodes to communicate. It is launched using the roscore command.

NOTE: If you use roslaunch, it will automatically start roscore if it detects that it is not already running.

roscore will start up:

  * a ROS Master

  * a ROS Parameter Server

  * a rosout logging node

The roscore can be launched using the roscore executable:

~~~bash
roscore
~~~
然后你会看到类似下面的输出信息:

~~~bash
... logging to ~/.ros/log/9cf88ce4-b14d-11df-8a75-00251148e8cf/roslaunch-

machine_name-13039.log
Checking log directory for disk usage. This may take awhile.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://machine_name:33919/
ros_comm version 1.4.7

SUMMARY
========

PARAMETERS
 * /rosversion
 * /rosdistro

NODES

auto-starting new master
process[master]: started with pid [13054]
ROS_MASTER_URI=http://machine_name:11311/

setting /run_id to 9cf88ce4-b14d-11df-8a75-00251148e8cf
process[rosout-1]: started with pid [13067]
started core service [/rosout]
~~~

## rosnode
rosnode 显示当前运行的ROS节点信息。rosnode list 指令列出活跃的节点:

~~~bash
$ rosnode list
~~~

## rosrun
rosrun 允许你使用包名直接运行一个包内的节点(而不需要知道这个包的路径)。
用法:

~~~bash
$ rosrun [package_name] [node_name]
~~~

## rostopic
rostopic命令工具能让你获取有关ROS话题的信息。

你可以使用帮助选项查看rostopic的子命令：

~~~bash
$ rostopic -h
rostopic bw     display bandwidth used by topic
rostopic echo   print messages to screen
rostopic hz     display publishing rate of topic
rostopic list   print information about active topics
rostopic pub    publish data to topic
rostopic type   print topic type
~~~

 * rostopic echo可以显示在某个话题上发布的数据。
     rostopic echo [topic]
 * rostopic list能够列出所有当前订阅和发布的话题。
     
 * rostopic type 命令用来查看所发布话题的消息类型。
     rostopic type [topic]
 * rostopic pub
     rostopic pub可以把数据发布到当前某个正在广播的话题上。
     rostopic pub [topic] [msg_type] [args]
     ~~~ shell
     $ rostopic pub /turtle1/command_velocity turtlesim/Velocity -r 1 -- 2.0  -1.8
     ~~~
## 使用 rqt_plot
注意：如果你使用的是electric或更早期的ROS版本，那么rqt命令是不可用的，请使用rxplot命令来代替。

rqt_plot命令可以实时显示一个发布到某个话题上的数据变化图形。这里我们将使用rqt_plot命令来绘制正在发布到/turtle1/pose话题上的数据变化图形。首先，在一个新终端中运行rqt_plot命令：

~~~bash
$ rosrun rqt_plot rqt_plot
~~~
这会弹出一个新窗口，在窗口左上角的一个文本框里面你可以添加需要绘制的话题。

## rosservice
rosservice可以很轻松的使用 ROS 客户端/服务器框架提供的服务。rosservice提供了很多可以在topic上使用的命令，如下所示：

使用方法:

~~~bash
rosservice list         输出可用服务的信息
rosservice call         调用带参数的服务
rosservice type         输出服务类型
rosservice find         依据类型寻找服务find services by service type
rosservice uri          输出服务的ROSRPC uri
~~~

* rosservice type
    rosservice type [service]
我们来看看clear服务的类型:

~~~bash
$ rosservice type clear
std_srvs/Empty
~~~
服务的类型为空（empty),这表明在调用这个服务是不需要参数（比如，请求不需要发送数据，响应也没有数据）。下面我们使用rosservice call命令调用服务：

* rosservice call
使用方法:

~~~bash
rosservice call [service] [args]
~~~

## rosparam
rosparam使得我们能够存储并操作ROS 参数服务器（Parameter Server）上的数据。参数服务器能够存储整型、浮点、布尔、字符串、字典和列表等数据类型。rosparam使用YAML标记语言的语法。一般而言，YAML的表述很自然：1 是整型, 1.0 是浮点型, one是字符串, true是布尔, [1, 2, 3]是整型列表, {a: b, c: d}是字典. rosparam有很多指令可以用来操作参数，如下所示:

使用方法:

~~~bash
rosparam set            设置参数
rosparam get            获取参数
rosparam load           从文件读取参数
rosparam dump           向文件中写入参数
rosparam delete         删除参数
rosparam list           列出参数名
~~~

## roslaunch
roslaunch可以用来启动定义在launch文件中的多个节点。

用法：

~~~bash
$ roslaunch [package] [filename.launch]
~~~








