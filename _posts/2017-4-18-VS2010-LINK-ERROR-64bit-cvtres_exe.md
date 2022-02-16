---
layout: post
title: "Visual Studio 2010 >LINK : failure during conversion to COFF: file invalid or corrupt"
subtitle:   "Visual Studio 2010 >LINK : failure during conversion to COFF: file invalid or corrupt"
date:       2017-04-18 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
error
LINK : failure during conversion to COFF: file invalid or corrupt

解决办法：

64bit操作系统，查找是否有两个cvtres.exe。


`C:\Program Files(x86)\Microsoft Visual Studio 10.0\vc\bin\cvtres.exe `

`C:\Windows\Microsoft.NET\Framework\v4.0.30319\cvtres.exe `


右键属性|详细信息 查看两者版本号，删除/重命名较旧的版本，或者重新设置Path变量
