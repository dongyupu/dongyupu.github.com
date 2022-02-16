---
layout: post
title: "gitlab 更改管理员密码"
subtitle:   "更改管理员密码"
date:       2019-04-08 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---

~~~bash
   #gitlab-rails console production

    Loading production environment (Rails 4.8.5.2)
    irb(main):001:0> user = User.where(id: 1).first
    => #<User id: 1, email: "admin@example.com", ...
    irb(main):002:0> user.password=123456
    => 123456
    irb(main):003:0> user.password_confirmation=123456
    => 123456
    irb(main):004:0> user.save!
    => true
    irb(main):005:0> quit
~~~
