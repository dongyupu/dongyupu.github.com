---
layout: post
title: "sonarqube 7.9.1 LTS community edition ldap AD config"
subtitle:   "ad config"
date:       2019-12-3 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---

# [官方文档](https://docs.sonarqube.org/latest/instance-administration/delegated-auth/)

~~~
# dongyupu 2019.11.28
# LDAP MS active directry configuration
# LDAP configuration
# General Configuration
sonar.security.realm=LDAP
ldap.url=ldap://your_ip:389
ldap.bindDn=CN=sonarqube,CN=Users,DC=yupu,DC=com
ldap.bindPassword=password

# User Configuration
ldap.user.baseDn=DC=domain,DC=com
ldap.user.request=(&(objectClass=user)(sAMAccountName={login}))
ldap.user.realNameAttribute=displayName
ldap.user.emailAttribute=mail

# Group Configuration 分组不能用下面的配置
#ldap.group.baseDn=cn=groups,dc=yupu,dc=com
#ldap.group.request=(&(objectClass=group)(member={dn}))
#ldap.group.idAttribute=sAMAccountName
~~~
