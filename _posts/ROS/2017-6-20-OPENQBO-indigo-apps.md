---
layout: post
title: "ROS Indigo app rosmake issues"
subtitle:   "ROS Indigo app rosmake issues"
date:       2017-6-20 0:00:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

* Same problem on System:
* Ubuntu 14.04 arm
* Opencv 2.4.9
* ROS Indigo

## [ROS qbo_listen 编译问题](http://blog.csdn.net/ppp2006/article/details/39934399)

结论：在ubuntu12.04中直接安装libjulius这个deb就会生成libsent.a文件，这个已经可以用以连接了。

而使用julius源码安装的libsent.a位于/usr/local/lib/libsent.a，这个库文件缺少sf_open等函数的定义。此时还未搞清楚如何能将它编译出来。./configure make install的方式是不行的。最终使用 deb安装包的方法，成功编译成功了qbo_listen.

## ROS qbo_object_recognition 编译问题
1.
[SurfFeatureDetector type-specifier问题](http://blog.csdn.net/holybin/article/details/41144705)
If you're using opencv 2.4, SURF and SIFT interfaces are changed to nonfree folder. You can use it by including this line

~~~bash
#include <opencv2/nonfree/features2d.hpp>
~~~
then you can use SurfFeatureDetector as before.

2. 
[undefined reference to `cv::ocl::integral](https://stackoverflow.com/questions/26553659/linking-opencv-nonfree-components-sift-features-and-ocl-specifically)

~~~bash
Linking CXX executable ../bin/orbit_client
/usr/local/lib/libopencv_nonfree.so.2.4.9: undefined reference to `cv::ocl::integral(cv::ocl::oclMat const&, cv::ocl::oclMat&)'
collect2: error: ld returned 1 exit status
~~~
[issue](https://github.com/tum-vision/lsd_slam/issues/27)

[solution](https://stackoverflow.com/questions/26553659/linking-opencv-nonfree-components-sift-features-and-ocl-specifically)

I fixed it.

I needed to turn off the CUDA/OpenCL components of opencv. So instead of running Cmake like this:

~~~bash
cmake -D WITH_CUDA=ON -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr .
~~~

I ran it like this:

~~~bash
cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENCL=OFF -D CMAKE_INSTALL_PREFIX=/usr/local .
~~~

And everything compiled just fine!

3.
[qbo_webi运行时问题](http://blog.csdn.net/ppp2006/article/details/39988867)

折腾一天的问题，SurfFeatureDetector type-specifier问题解决了。需要在cv.h中添加include "opencv2/nonfree/features2d.hpp。
~~~bash
from poster.encode import multipart_encode 
ImportError: No module named poster.encode

sudo apt-get install Python-poster
sudo apt-get install python-xmmsclient

sudo apt-get install python-pam                  python调用pam模块进行Linux用户验证 


sudo apt-get install xmms2   音乐播放器。没有的话，就报错。 

安装abo_questions
~~~

