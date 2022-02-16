---
layout: post
title: "jenkins gitlab Generic Webhook Trigger config"
subtitle:   "Generic Webhook Trigger config"
date:       2019-09-09 12:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
本文旨在介绍如何将Jenkins和Gitlab进行集成。
Jenkins使用[Generic Webhook Trigger Plugin](https://wiki.jenkins.io/display/JENKINS/Generic+Webhook+Trigger+Plugin)来和Gitlab进行集成。

### Jenkins Job配置
需要配置两个地方：

* Job > 配置 > 构建触发器 > 触发远程构建 (例如,使用脚本) > 填写token
<br>
<img src="{{ site.baseurl }}/img/2019/Jenkins-gitlab/1.png" />
<br>
 Job > 配置 > 构建触发器 > Generic Webhook Trigger > 如果你需要一些自定义参数的话，请参考Generic Webhook Trigger Plugin官方示例
<br>
<img src="{{ site.baseurl }}/img/2019/Jenkins-gitlab/2.png" />

* 配置触发分支 Generic Webhook Trigger > Post content parameters > 增加
<br>

* 我们需要配置三个参数 
  * ref：分支名
  * repository.url：git仓库的ssh地址
  * user_name：触发构建的用户名   
<br>
<img src="{{ site.baseurl }}/img/2019/Jenkins-gitlab/3.png" />
<br>
* Optional filter配置：只有Text中的参数与正则表达式匹配时才会触发构建，我们这里使用了$repository.url_$ref，即 仓库地址_分支名
<br>
<img src="{{ site.baseurl }}/img/2019/Jenkins-gitlab/4.png" />
<br>
### Gitlab配置
* 配置gitlab webhooks：Project > Settings > Integrations > Web Hooks
<br>
<img src="{{ site.baseurl }}/img/2019/Jenkins-gitlab/5.png" />
<br>
* web hooks 格式如下：
  * http://jenkins_username:API Token@172.16.131.4:8080/jenkins/generic-webhook-trigger/invoke?token=test
  * 参数说明：
    * jenkins_username ：登录jenkins的用户名，该用户需要拥有执行构建的权限
    * API Token：登录jenkins后，点击右上角自己的用户名 > 设置 > API Token 
    * token：jenkins job中配置的  触发远程构建  > 身份验证令牌 的值




