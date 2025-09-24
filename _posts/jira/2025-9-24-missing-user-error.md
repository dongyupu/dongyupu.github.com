---
layout: post
title: "Could not add the following missing users to group 'jira-software-users' "
subtitle:   "无法添加用户到项目角色，但是可以搜索到用户名，无法查看用户信息"
date:       2025-9-24 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
## Could not add the following missing users to group 'jira-software-users'

Jira出现用户无法同步的问题，可以搜索到用户但是查看或添加用户到项目时却不能成功
同步日志里提示 missing user，但在 Jira 后台搜索时又能搜到用户名，这正是前面 NPE 的根源。原因通常不是“用户真的不存在”，而是 Jira 的目录缓存和用户目录（LDAP/AD 或 Crowd 内部目录）之间出现了不一致。
🔎 可能原因
- 缓存层不一致   Jira 使用 DbCachingDirectory，会把远程目录的用户信息缓存到本地数据库。
  - 搜索时，Jira 可能是直接查本地索引（Lucene/数据库），所以能搜到用户名。
  - 但同步或渲染用户详情时，需要加载完整的 ApplicationUser 对象 → 结果 fullUser() 返回 null。
- 用户对象残缺   用户在 cwd_user 表里有记录，但缺少有效的 directory_id 或者该目录被禁用/配置变更。
- 目录配置变动   例如修改了 LDAP 过滤器，导致同步时用户被过滤掉，但 Jira 本地还保留旧的引用。
- 已知 Jira Bug   某些版本在处理 LazyLoadingApplicationUser 时没有做 null-check，导致直接抛 NPE。
🛠 不动数据库的解决办法
1. 强制全量同步
  - 管理后台 → 用户目录 → 选择目录 → 点击“同步”。
  - 这会刷新缓存，很多“missing user”警告会消失。
2. 检查目录过滤器
  - 确认 LDAP/AD 的 User Filter / Group Filter 没有把这些用户排除。
  - 如果用户应该存在，调整过滤器后再全量同步。
3. 检查目录顺序和优先级
  - 如果有多个目录（内部目录 + LDAP），确保顺序正确。
  - Jira 会优先匹配第一个找到的用户，可能导致“影子用户”。
4. 禁用再启用目录
  - 在后台把相关目录禁用再启用，迫使 Jira 丢弃缓存并重新加载。
5. 用户清理（UI 层面）
  - 如果确认用户已离职/不再需要，可以在 UI 中移除他们的组成员关系。
  - 如果 UI 无法操作，可以考虑用 REST API 清理（比直接改数据库安全）。
6. 示例：
7. bash
curl -u admin:password -X DELETE \
"http://<jira-host>/rest/api/2/group/user?groupname=品牌设计&username=sujiang"
1. 升级 Jira
  - 如果你用的是旧版本，升级到最新维护版本可能已经修复了 LazyLoadingApplicationUser 的 NPE 问题。
✅ 总结
- 根因：目录缓存和实际用户目录不同步，导致“幽灵用户”现象。
- 短期解法：全量同步、禁用/启用目录，或用 REST API 清理组成员关系。
- 长期解法：检查 LDAP 过滤器、目录配置，升级 Jira。
