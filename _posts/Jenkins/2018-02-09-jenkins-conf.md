---
layout: post
title: "jenkins configs"
subtitle:   "jenkins config"
date:       2018-2-9 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-04.jpg"
---

持续集成服务器配置指导手册
<br>
版本号	日期	版本说明	作者	备注
V0.1	2018.2.9	新建	董玉璞	

<br>
本手册旨在说明jenkins的相关配置，以便相关使用人员可以快速部署自己的任务到持续集成服务器。

<br>
Jenkins 运行方式
<br>
ECS主机：ip
<br>
运行模式：docker container
<br>
Image：jenkinsci/blueocean
<br>
docke- compose.yml

~~~yml
jenkins: 
  image: jenkinsci/blueocean
  user: root
  volumes: 
  - /app/jenkins-home:/var/jenkins_home
  - /var/run/docker.sock:/var/run/docker.sock
  - /root/.ssh:/root/.ssh
  - /root:/home/SYSTEM
  - /root/.gitconfig:/root/.gitconfig
  - /root:/root
  ports: 
  - "8080:8080"
  - "50000:50000"
~~~

<br>
位置：/app/docker-compose/Jenkins/docker-compose.yml
<br>
启动命令：docker-compose up –d
<br>
停止命令：docker-compose stop
<br>
Jenkins Home使用的挂载目录<br>
/app/jenkins-home 
请勿随意更改<br>
Gerrit trigger 插件配置<br>
Gerrit插件需要做一些特殊配置，我们的server并没有开启verify功能，所以需要在高级配置中将Gerrit Verified Commands 中的 –verify <VERIFY> 去掉，否则jenkins无法向gerrit添加评论，因为command中存在无效参数 –verify。
<br> 
自动submit<br>
如果需要自动submit，请将Code Review 》 Successful 的值设置为2，并在Gerrit Verified Commands 中的Successful 命令中追加 –submit<br>
针对不同的需求，分别使用不同的gerrit trigger server<br>
设置两个server实例，一个执行自动submit，一个不执行自动submit，在配置具体的job触发器时选择不同的server可以实现不同的行为，以满足不同分支的submit需求。比如我们针对60.206.93.190同时配置了两个trigger server，一个构建成功自动+2 submit。另一则执行默认操作。
 <br>
Query and Trigger Gerrit Patches<br>
可能会遇到一些特殊情况，比如网络中断导致jenkins的job没有被触发，此时可以使用<br>
Query and Trigger Gerrit Patches 来手动重新触发job<br>
 <br>
触发构建的事件类型说明
<br>
Which type of Gerrit events should trigger the build.
•	Patch set created Trigger when a new change or patch set is uploaded (default)
•	Draft published Trigger when a draft change or patch set is published (default, if available in Gerrit version)
•	Change abandoned Trigger when a change is abandoned
•	Change merged Trigger when a change is merged/submitted
•	Change restored Trigger when a change is restored
•	Comment added Trigger when a review comment is left with the indicated vote category and value
•	Reference updated Trigger when a reference (e.g., branch or tag) is updated

<br>
Multiple SCMs
可以在一个job里同时配置多个git或svn仓库的插件。<br>
 
SCM Sync configuration插件<br>
该插件可以将jenkins的配置文件备份到指定的git、svn仓库。<br>
注意权限问题，在jenkins重启时，该插件会自动提交所有变更，如果使用git仓库备份的话，请注意jenkins默认使用的SYSTEM用户来运行插件，所有在容器中需要配置用户的ssh和known_hosts，请参加docker-compose.yml中的挂载目录，我们为容器挂载了一个SYSTEM目录以便该插件可以使用正确的ssh pub key来提交变更。
 <br>
邮件通知配置<br>
邮件配置时需要注意，用户名和系统管理员邮件地址必须一致，否则无法发送邮件
 <br>
 
Extended E-mail Notification<br>
系统自动的邮件发送可能无法满足我们的需求，此时可能需要安装Extended E-mail Notification插件，并做相应配置来达到更精细的邮件通知策略。
 <br>
在项目中使用邮件通知<br>
根据自己的需要选择使用哪种邮件通知方式
 <br>
Credentials管理<br>
可以通过配置全局的证书供系统插件使用，用户/密码 或 ssh
<br>
 
全局安全配置<br>
为指定的用户分配指定的权限，并在具体job中启用项目安全已到达权限控制的目的。
 <br>
 
系统日志<br>
Jenkins系统日志：所有系统日志<br>
如果想查看某个插件的具体日志，新建日志记录器，指定logger名字，比如过滤查看SCM Sync configuration的日志
 <br>

管理用户<br>
创建/删除/修改Jenkins用户<br>
 
插件管理<br>
一般情况下，在系统运行稳定的情况下不建议升级系统或插件，升级所带来的风险无法预知，所以需要谨慎操作。
<br>
插件也可以在高级选项里手动上传指定版本的插件。<br>

Ssh远程登录和执行脚本<br>
SSH plugin<br>
配置该插件后，可以实现远程shell执行
Ssh免秘钥登录<br><br>
在目标主机~/.ssh/authorized_keys中添加本机的ssh pubkey（id_rsa.pub）<br>
需要注意id_rsa的权限，不可以太高，否则会登录失败提示too open<br>
还需要注意的是，如果无法登录需要确认known_hosts是否已经添加了目标主机ip<br>
另外，我们的部分ecs主机做了安全限制，部分appuser用户无法通过外网的ip登录，但是可以使用内网ip登录<br>
Job配置<br>
单一git仓库项目配置<br>
如果job只需要一个git仓库来触发构建，那么在使用gerrit trigger来触发时，源代码管理选择git时需要做特殊配置，以便可以在代码还未submit前把代码同步到jenkins的工作区来进行构建验证、单元测试、自动化测试、代码静态检测等工作。
 <br>
如上图所示，需要在高级中指定

~~~yml
Refspec ：$GERRIT_REFSPEC
Branches to build ：$GERRIT_BRANCH
Additional Behaviours	：
Strategy for choosing what to build ：
Choosing strategy：gerrit trigger
~~~

构建触发器选择gerrit event
 <br>
指定触发构建的gerrit server，触发构建的事件类型，触发构建的仓库和分支名称。<br>
构建后操作<br>
为了永久保存构建结果，需要将构建成功后的产物存档，以便后期使用。<br>
构建存档的位置：$JENKINS_HOME/jobs/job_name/builds/id/archive/
 
多依赖项目配置<br>
如果某个仓库无法单独编译，或者需要几个git仓库代码来一同编译，需要使用Multiple SCMs插件来解决源代码管理问题。<br>
在配置多个git仓库时，需要做一些特殊的调整。构建触发器依然使用gerrit trigger，配置多个项目可以触发构建。<br>
Git仓库配置<br>
Refspec留空，后面使用脚本来拉取指定项目的待评审代码。<br>
并且去掉

~~~yml
Additional Behaviours	：
Strategy for choosing what to build ：
Choosing strategy：gerrit trigger
~~~

<br>
构建触发器设置示例，配置多个项目触发构建
 
<br>
脚本拉取gerrit待评审的代码

~~~bash
echo $GERRIT_PROJECT $GERRIT_BRANCH $GERRIT_REFSPEC
#cd ${GERRIT_PROJECT##*/}
cd ${GERRIT_PROJECT:12}
git fetch ssh://Admin@ip:29418/$GERRIT_PROJECT $GERRIT_REFSPEC && git checkout FETCH_HEAD
echo "code checkout done !"
~~~
<br>
至此，代码就准备就绪了，接下来就是编译、部署的脚本工作了，在此不再展开。
<br>



