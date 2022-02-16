---
layout: post
title: "Jenkins : HTTP ERROR 403 no valid crumb was included in the request"
subtitle:   "configureSecurity : crumb issuer"
date:       2020-03-30 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---

## 问题

* Jenkins : HTTP ERROR 403 no valid crumb was included in the request

## 解决方案
* Jenkins 启用代理兼容

### 设置方法
* Manage Jenkins 》configureSecurity（全局安全设置）》跨站请求伪造保护
* 启用代理兼容
  * Some HTTP proxies filter out information that the default crumb issuer uses to calculate the nonce value. If an HTTP proxy sits between your browser client and your Jenkins server and you receive a 403 response when submitting a form to Jenkins, checking this option may help. Using this option makes the nonce value easier to forge.
