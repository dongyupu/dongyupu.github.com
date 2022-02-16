---
layout: post
title: "Failed install the following Android SDK packages as some license not been accepted"
subtitle:   "android sdk r24.4.1"
date:       2020-06-04 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
### 错误日志
~~~
Failed install the following Android SDK packages as some license not been accepted
    build-tools:29.0.2 Android SDK Build-Tools 29.0.2
~~~

### 老版本有sdkmanager的解决方法

~~~
./sdkmanager "build-tools:29.0.2"
~~~


### 没有sdkmanager的解决方法
~~~
android update sdk --no-ui --all --filter build-tools:29.0.2
~~~
