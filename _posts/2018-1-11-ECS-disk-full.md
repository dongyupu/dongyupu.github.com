---
layout: post
title: "云服务器 ECS Linux 磁盘空间满（含inode满）问题排查方法"
subtitle:   "jenkins log too big"
date:       2018-01-11 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
今天发现阿里云的磁盘满了，Jenkins的日志竟然有27g之大。
[排查方法](https://help.aliyun.com/knowledge_detail/42531.html)

目录：/var/log/jenkins

解决方法：

1、每次手动删除 /var/log/jenkins/jenkins.log 该文件，而且删除后需要重启下jenkins，不然空间不能释放出来。

2、点击 系统管理-System Log-日志级别，然后增加  javax.jmdns为off
~~~bash
        Manage Jenkins -> System Log -> Log Levels (on the left)

        Name: javax.jmdns

        Level: off
~~~


问题描述

在云服务器ECS Linux系统内创建文件时，出现类似如下空间不足提示：

~~~bash
No space left on device …
~~~

问题原因

导致该问题的可能原因包括：

磁盘分区空间使用率达到百分之百。
磁盘分区inode使用率达到百分之百。
僵尸文件：已删除文件因句柄被占用未释放导致相应空间未释放。
挂载点覆盖：在原有文件系统的相应目录下已经存在大量文件。挂载了新磁盘后，导致使用 df 命令能统计到相关空间使用，而使用 su 命令统计不到。
处理办法

不同的原因需要通过不同的方法解决：

分区容量满
inode容量满
修改inode数量
僵尸文件分析删除
挂载点覆盖
分区容量满

如果是分区容量满导致磁盘空间满，按以下步骤操作：

远程连接Linux实例。

运行 df -h 查看磁盘使用率。返回结果如下图所示。

注意：
返回结果里 Mounted on 下显示的是挂载目录。
查看磁盘使用率

循环执行如下指令，找到容量比较大的目录并进入目录，直到找到最精确的文件或目录，再结合业务情况等判断，删除相关文件或目录。您也可以购买更大的数据盘来分担处理。

~~~bash
cd /
du -sh *
~~~

查看目录

inode容量满

如果是inode容量满导致磁盘空间满，按以下步骤操作：

远程连接Linux实例。

运行以下命令分析根目录下每个目录下面有多少个文件。

~~~bash
for i in /*; do echo $i; find $i | wc -l; done
~~~

返回结果如下图所示。
查根目录下有多少个文件

逐层进入inode占用最高的目录，继续执行上述指令，逐步定位占用过高空间的文件或目录，最后进行相应清理。

修改inode数量

ECS Linux 实例的inode节点中，记录了文件的类型、大小、权限、所有者、文件连接的数目、创建时间与更新时间等重要的信息，还有一个比较重要的内容就是指向数据块的指针。一般情况不需要特殊配置；如果存放文件很多，需要配置。有时磁盘空间有剩余但是不能存放文件，可能是由于inode耗尽所致。

按以下步骤调整inode节点数量：

注意：
inode的调整需要重新格式化磁盘，请确保您已经备份了数据再执行以下操作。
远程连接Linux实例。

运行以下命令查询inode使用情况。

~~~bash
df  -i
~~~

返回结果如下图所示。
查看inode使用情况

运行以下命令卸载系统文件。假设卸载的文件系统为 /home。

~~~bash
umount /home
~~~

运行以下命令重新建立文件系统，指定inode节点数。

~~~bash
mkfs.ext3 /dev/xvdb -N 1638400
~~~

（可选）运行命令 vim /etc/fstab 修改fstab文件。

运行以下命令查看修改后的inode节点数。

~~~bash
dumpe2fs -h /dev/xvdb | grep node
~~~

返回结果如下图所示。
查看修改后的inode节点数

僵尸文件分析删除

如果磁盘和inode都没有问题，则需要查看是否存在未被清除句柄的僵死文件。这些文件实际上已经被删除，但是有服务程序在使用这些文件，导致这些文件一直被占用，无法释放磁盘空间。如果这些文件过多，会占用很大的磁盘空间。

按以下步骤查看并删除僵尸文件：

远程连接Linux实例。

运行以下命令安装lsof。

yum install lsof -y
运行以下命令查看僵尸文件占用情况。

lsof |grep delete | more
返回结果示例如下图所示。
查看僵尸文件数量

采用以下方法释放句柄，以清除僵尸文件：

重启服务器。
正常停止或杀掉占用这些文件的服务进程。
挂载点覆盖

先取消磁盘挂载，再检查原挂载目录下的空间占用情况。
