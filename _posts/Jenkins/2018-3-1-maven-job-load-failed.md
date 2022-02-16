---
layout: post
title: "jenkins restart : all maven job load failed"
subtitle:   "maven plugin stoped ,jenkins canot load maven jobs"
date:       2018-3-1 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---


今天安装插件后重启jenkins，重启之后发现所有的maven项目全部都不见了，
<br>查看系统日志：
~~~bash
Failed Loading item XXX
com.thoughtworks.xstream.mapper.CannotResolveClassException: maven2-moduleset
  at com.thoughtworks.xstream.mapper.DefaultMapper.realClass(DefaultMapper.java:79)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.DynamicProxyMapper.realClass(DynamicProxyMapper.java:55)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.PackageAliasingMapper.realClass(PackageAliasingMapper.java:88)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.ClassAliasingMapper.realClass(ClassAliasingMapper.java:79)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.ArrayMapper.realClass(ArrayMapper.java:74)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.SecurityMapper.realClass(SecurityMapper.java:71)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at hudson.util.XStream2$CompatibilityMapper.realClass(XStream2.java:285)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at hudson.util.xstream.MapperDelegate.realClass(MapperDelegate.java:43)
	at com.thoughtworks.xstream.mapper.MapperWrapper.realClass(MapperWrapper.java:30)
	at com.thoughtworks.xstream.mapper.CachingMapper.realClass(CachingMapper.java:48)
	at com.thoughtworks.xstream.core.util.HierarchicalStreams.readClassType(HierarchicalStreams.java:29)
	at com.thoughtworks.xstream.core.TreeUnmarshaller.start(TreeUnmarshaller.java:133)
	at com.thoughtworks.xstream.core.AbstractTreeMarshallingStrategy.unmarshal(AbstractTreeMarshallingStrategy.java:32)
	at com.thoughtworks.xstream.XStream.unmarshal(XStream.java:1189)
	at hudson.util.XStream2.unmarshal(XStream2.java:114)
	at com.thoughtworks.xstream.XStream.unmarshal(XStream.java:1173)
	at com.thoughtworks.xstream.XStream.fromXML(XStream.java:1053)
	at hudson.XmlFile.read(XmlFile.java:150)
~~~

既然全都是和maven相关的job，于是去查看maven配置，发现系统管理中maven的配置不见了，查看插件状态，发现maven插件 未`启用`

* 解决：启用maven插件，重启jenkins。恢复正常了。


