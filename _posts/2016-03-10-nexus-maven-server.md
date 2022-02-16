---
layout: post
title: "使用Nexus搭建Maven私服"
subtitle:   "nexus-maven-server"
date:       2016-03-10 10:18:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-04.jpg"
---
- 由于项目的特殊性，为了方便版本发布和团队协作，我们使用maven私服来控制底层SDK二进制文件的发布，主要包括 lib 、dll  、exe 、so 、.a 等编译和打包时需要的文件，团队成员可以通过maven 或者 gradle来管理项目依赖。或者直接在浏览器中下载。<br>

- 主要目的是实现版本控制精细化，sdk和app版本分开管理，可以让app自行决定使用那个版本的sdk。<br>
- maven私服地址  http://10.2.48.55:8081/nexus<br>
- 下面来记录下安装过程：<br>

1.nexus    下载地址：http://www.sonatype.org/nexus/go   这个是免费版的 nexus-latest-bundle.zip

2.解压后进入相应的系统目录nexus-2.12.0-01\bin\jsw\windows-x86-32  运行 install-nexus.bat  然后你就可以访问

<http://localhost:8081/nexus/>   

<img src="{{ site.baseurl }}/img/2016/image2016-3-10 16-36-12.png" />

3.点击Log in 登录 默认的用户名是 admin 密码是 admin123

4.发布文件 点击 Repositories 这里就不在赘述了(有兴趣的自行百度)

<img src="{{ site.baseurl }}/img/2016/image2016-3-10 16-39-52.png" />

5.这就算是文件已经被发布了，可以通过maven或者gradle的依赖管理来下载了。

6.支持API发布新版本，也就是说可以和jenkins之类的编译服务器配合实现自动部署新版本

因为不是很熟悉maven所有还不知道如何用命令行下载依赖，有待后续研究。

发现maven还是玩JAVA好些，想把它应用到VS工程里还是不太方便，毕竟IDE党感觉无法接受。

## update:
* for VS project   使用gradle的脚本来下载文件<br>
主要解决下载指定文件的问题，当把dll或者lib库发布到maven仓库后，编译或者打包程序之前先更新gradle脚本，运行指定的任务即可。<br>

build.gradle

```xml
buildscript {
	repositories {
		jcenter()
		}
	}
		
	//plugin: https://github.com/michel-kraemer/gradle-download-task
	//Gradle 2.1 and higher
	plugins {
		id "de.undercouch.download" version "2.1.0"
	}
	import de.undercouch.gradle.tasks.download.Download
		
	task downloadMultipleFiles(type: Download) {
	src([
	'http://10.2.48.55:8081/nexus/service/local/repositories/thirdparty/content/WiMoSDK/dll/16.03.02.01/dll-16.03.02.01.dll',
	'http://10.2.48.55:8081/nexus/service/local/repositories/thirdparty/content/WiMoSDK/dll/16.03.02.01/dll-16.03.02.01.dll'
	])
	dest "bin"
	}
```
 
* 执行结果：

<img src="{{ site.baseurl }}/img/2016/image2016-3-10 18-27-53.png" />

<img src="{{ site.baseurl }}/img/2016/image2016-3-10 18-28-19.png" />

指定的文件已经被下载到指定的目录，好像默认会覆盖已有的文件。

至此，已经可以解决了所有需求的问题。
 
for java and android project，同理，直接使用gradle或者maven管理即可。
 
## Next：
使用脚本发布文件到nexus 。

## update ：脚本发布文件到maven仓库maven-publish插件


```xml
apply plugin: 'maven-publish'
publishing {
	publications {
		repositories.maven {
			url 'http://localhost:8081/nexus/content/repositories/thirdparty/'
			credentials {
					username "admin"
					password "yupu"
				}
			}
		maven(MavenPublication) {
			artifacts {
				groupId 'win32'
				artifactId 'core'
				version '16.03.15.3'
				artifact 'bin/core.dll'
				}
			}
	}
}
```


