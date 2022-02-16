---
layout: post
title: "jira、confluence、crowd add MS active directory"
subtitle:   "username、hosts config"
date:       2019-08-30 12:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
### 修改host
* 如果给运维给你的是IP的话，你就需要在hosts文件里做个域名映射了
* 比如baseDN:DC=domain,DC=com
* 在hosts文件中增加一条：AD_IP domain.com

### 用户名设置
* 如果直接用用户名不能登录的话，需要加上 @domain.com 也就是加上basedn
* username@domain.com


