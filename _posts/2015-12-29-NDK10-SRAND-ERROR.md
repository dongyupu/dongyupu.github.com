---
layout: post
title: "NDK10-SRAND-ERROR"
subtitle: "java.lang.UnsatisfiedLinkError dlopen failed: cannot locate symbol srand referenced by"
date:       2015-12-29 10:27:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---

因为要支持android5.0 所以改用NDK10编译so，结果在5.0以下的系统中无法加载so

捕获的崩溃log: 

~~~bash
>java.lang.UnsatisfiedLinkError
dlopen failed: cannot locate symbol "srand" referenced by "libtxcore20151224_110.so"...
com.cmri.wimodlna.client.core.UpnpManager.initUpnpServe
~~~

上面意思很明显是无法找到srand这个东西，Google之后发现很多人都遇到了这个问题，这是NDK10编译器的一个bug，srand应该是POSIX标准接口，理论上所有的Linux都应该支持的。

## 解决方案：

- 根治》这个也不知道算不算bug，如果官方不修改的话就只能改自己的代码了，改造下srand函数。
- 参考 [这个连接](http://stackoverflow.com/questions/25475055/android-ndk-load-library-cannot-locate-sra) 这里提到了srand/atof这个2个函数，都会有这样的问题，很不幸都调用了。 

