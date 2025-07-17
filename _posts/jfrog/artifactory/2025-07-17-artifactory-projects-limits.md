---
layout: post
title: "Jfrog Artifactory pro create project in pg DB"
subtitle:   "7.104.4 ARTIFACTORY: How to debug the 'You have reached the permitted limit of the number of projects that can be created' error"
date:       2025-07-17:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
### crack artifactory 7.104.7 enterprise plus 
### solution
[不适用我的](https://jfrog.com/help/r/artifactory-how-to-debug-the-you-have-reached-the-permitted-limit-of-the-number-of-projects-that-can-be-created-error/artifactory-how-to-debug-the-you-have-reached-the-permitted-limit-of-the-number-of-projects-that-can-be-created-error)

* [crack](https://github.com/Lama3L9R/ArtifactoryKeygen)
* 我集群安装，2个节点，出现无法创建project的限制 "You have reached the permitted limit of the number of projects that can be created"，没找到如何配置license.projects.total
* 尝试直接在数据库里插入绕过程序端的限制

* 涉及到的几个表：access_projects、access_platform_config、access_project_users、access_roles、access_project_resources
### Notice
* 如果复制其它项目的数据直接改name和projectkey 插入，会导致项目绑定的repo出现混乱，更新配置时可能会提示你 "Resources [docker-local] already exists in another project, consider removing them first from source project or use force=true query param to force override resources" 之类的error，此时需要在请求的URL中加上force=true
* 我用postman通过 restapi修改的 https://myartifactory.com/access/api/v1/projects/devops?force=true

