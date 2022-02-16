---
layout: post
title: "Redmine Tencent qq exmail config"
subtitle:   "redmine腾讯企业邮箱异步配置"
date:       2018-08-01 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

redmine邮箱发送默认是阻塞式的，也就是说只有邮件发送成功后才能进行其它操作，一般改成异步的

使用官方的docker镜像tag 3.2来运行

/usr/src/redmine/config/configuration.yml

~~~yml
# default configuration options for all environments
default:
  # Outgoing emails configuration
  # See the examples below and the Rails guide for more configuration options:
  # http://guides.rubyonrails.org/action_mailer_basics.html#action-mailer-configuration
  email_delivery:
    #默认设置模式，现在估计不能用了，必须启用ssl使用https方式
    #delivery_method: :smtp
    #smtp_settings:
      #address: "smtp.exmail.qq.com"
      #port: 25
      #domain: "smtp.exmail.qq.com" 
      #authentication: :login
      #user_name: "YourQQexmail"
      #password: 'Password'
    delivery_method: :async_smtp
    async_smtp_settings:
      address: "smtp.exmail.qq.com"
      ssl: true
      port: 465
      domain: "smtp.exmail.qq.com"
      authentication: :login
      user_name: "YourQQexmail"
      password: 'Password'
~~~






