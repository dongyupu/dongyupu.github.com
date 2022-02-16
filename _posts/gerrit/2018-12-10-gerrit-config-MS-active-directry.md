---
layout: post
title: "gerrit config MS active directry"
subtitle:   "gerrit ladp (ms ad) config. "
date:       2018-12-10 3:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
# gerrit接入AD配置

~~~yml
[ldap]
  server = ldap://LDAP_IP:389
  username = your_username
  password = your_PASSWD
  accountBase = DC=your_dc,DC=local
  accountPattern = (&(objectClass=person)(sAMAccountName=${username}))
  accountFullName = displayName
  accountEmailAddress = mail
  accountSshUserName = sAMAccountName
  localUsernameToLowerCase = true
  groupBase = DC=your_dc,DC=local
~~~



