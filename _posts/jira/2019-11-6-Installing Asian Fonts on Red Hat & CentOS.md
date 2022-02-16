---
layout: post
title: "Installing Asian Fonts on Red Hat & CentOS"
subtitle:   "jira 报告饼图中无法显示中文"
date:       2019-11-06 0:09:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
## Jira饼图无法显示中文（中文乱码）问题

[Installing Asian Fonts on Red Hat & CentOS](https://help.accusoft.com/PrizmDoc/v12.2/HTML/Installing_Asian_Fonts_on_Red_Hat_and_CentOS.html)
## Version 6
In the RHEL 6 / CentOS 6 release, the language font group packages are no longer available. Instead, these releases have "groupinstall" lists which bundle multiple packages together. In order to see all language packages available through this interface, you can type the following command:

Example	
* yum grouplist

This will output all group packages, the last section of which is "Available Language Groups". Note that some packs may already be installed, in which case, they will be visible under "Installed Language Packs" at the top of the list. In this case, "chinese-fonts" is now "Chinese Support".

To install this package, run the following command:

Example	
~~~
# yum groupinstall "Chinese Support"
~~~
To install all languages listed in the help file, run the following commands:

Example	
~~~
# yum groupinstall "Chinese Support"
# yum groupinstall "Japanese Support"
# yum groupinstall "Korean Support"
# yum groupinstall "Kannada Support"
# yum groupinstall "Hindi Support"
~~~~

## Version 7/8 

In the RHEL 7 / CentOS7 release, the language font group packages are no longer available. Instead, these releases have "groupinstall" lists which bundle multiple packages together. There is a single Fonts package which includes support for these languages:
<br>
Example	
~~~
# yum groupinstall Fonts
~~~
