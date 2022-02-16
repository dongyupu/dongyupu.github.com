---
layout: post
title: "sonar ms ad ldap configuration"
subtitle:   "sonar ms ad ldap"
date:       2018-11-28 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
###  安装 LDAP plugin
* 下载 LDAP Plugin 插件，地址：https://docs.sonarqube.org/display/SONARQUBE67/LDAP+Plugin
* 将下载的插件，放到 SONARQUBE_HOME/extensions/plugins 目录

或者
* 管理员登录》配置》Marketplace 搜索 Ldap  》install


### 配置文件路径：SONARQUBE_HOME/conf/sonar.properties
~~~
# LDAP configuration
# General Configuration
sonar.security.realm=LDAP
ldap.url=ldap://your_ip_or_url:389
ldap.bindDn=cn=test,DC=domain,DC=com
ldap.bindPassword=password
   
# User Configuration
ldap.user.baseDn=DC=9fgroup,DC=local
ldap.user.request=(&(objectClass=user)(sAMAccountName={login}))
ldap.user.realNameAttribute=displayName
#ldap.user.emailAttribute=mail
  
# Group Configuration
#ldap.group.baseDn=DC=domain,DC=local
#ldap.group.request=(&(objectClass=posixGroup)(memberUid={uid}))

~~~

保存并重启即可  
~~~
sh -x SONARQUBE_HOME/bin/linux-x86-64/sonar.sh restart
~~~
