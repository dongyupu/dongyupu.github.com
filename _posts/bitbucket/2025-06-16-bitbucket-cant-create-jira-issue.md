---
layout: post
title: "bitbucket datacenter 7.15.2 create jira issue JiraErrors{errorMessages=[问题不存在], errors={}}"
subtitle:   "Platform Notice: Data Center Only"
date:       2025-06-16 1:22:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

- 这个错误通常表明 Bitbucket Data Center 在尝试创建 Jira 问题时调用了一个 Jira API，而该 API（通常是 createmeta endpoint）在 Jira 9.x 及以上版本中已被废弃或移除，导致 Jira 返回“问题不存在”的错误信息。实际上，Bitbucket Data Center 7.15.2 使用了旧版 API，因此在新版 Jira 环境下会出现这种不兼容的问题。

- 解决办法：

  - 升级 Bitbucket Data Center： Atlassian 已确认这一问题，推荐升级 Bitbucket 至 7.16 或更高版本，后续版本已修复对新版 Jira API 的调用，能够兼容 Jira 9.x 及以上版本。

  - 检查应用链接配置： 升级前也可以确认 Jira 与 Bitbucket 之间的应用链接设置和认证方式是否符合最新要求，不过根本原因还是 API 调用的问题，因此升级才是根本解决方案。

- 总之，如果你遇到“JiraErrors{errorMessages=[问题不存在], errors={}}”这样的错误，请考虑升级 Bitbucket Data Center 至 7.16 或更新版本，以确保与 Jira 9.x 及以上版本正常集成。


