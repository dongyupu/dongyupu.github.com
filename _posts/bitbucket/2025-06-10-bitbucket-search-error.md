---
layout: post
title: "bitbucket unable-to-establish-a-connection-to-elasticsearch-during-index-synchronisation"
subtitle:   "Platform Notice: Data Center Only"
date:       2025-06-10 1:22:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
## Update 2025.6.13
7.15.2 版本：仅删除报错的目录还是无法启动es进程，然后直接删除掉整个<bitbucket-home>/shared/search/data 才正常了。
这里AI给出的解释：
### 触发重建索引（Reindex）
- 对于大多数情况，建议使用搜索索引重建功能来让 Bitbucket 重新生成索引数据。主要做法包括：

- 删除（或重命名）现有索引数据 停止 Bitbucket Data Center，然后将 <BITBUCKET_HOME>/shared/search/data/bitbucket_search 目录备份后删除（或将其改名），强制 Bitbucket 在下次启动时重新生成完整的搜索索引。

- 使用 REST API 触发重建 如果你不希望手动删除数据，也可以使用 Bitbucket 提供的 REST API 来重建索引。具体步骤如下：

- 确保搜索服务器正在运行。

- 发送一个 POST 请求（例如使用 curl）到重建索引的端点：

~~~bash
curl -X POST -u <adminUsername>:<adminPassword> http://<bitbucket_base_url>/rest/indexing/latest/rebuild
~~~
- 该操作会让 Bitbucket 重新构建搜索索引。

- 重启 Bitbucket 删除索引数据后或触发重建操作后，重启 Bitbucket Data Center，系统会自动开始生成新的搜索索引。可通过监控 <BITBUCKET_HOME>/log/search/ 内的日志文件确认重建进度。






# [Unable to establish a connection to Elasticsearch during index synchronisation](https://support.atlassian.com/bitbucket-data-center/kb/unable-to-establish-a-connection-to-elasticsearch-during-index-synchronisation/)

### Summary
* Problem
* BItbucket logs report the following error when connecting to Elasticsearch:
~~~
2020-03-02 15:17:13,933 WARN  [Caesium-1-3]  c.a.b.i.s.i.IndexingSynchronizationService Unable to establish a connection to Elasticsearch during index synchronisation.
~~~


* There is no other apparent error in the logs.
* However, looking at elasticsearch log, bitbucket-search.log for embedded Elasticsearch, has the following error:
~~~
[2020-03-02T07:55:44,546][ERROR][o.e.g.GatewayMetaState   ] [bitbucket_bundled]failed to read local state, exiting...
java.lang.IllegalStateException: index and alias names need to be unique, but the following duplicates were found [bitbucket-search (alias of [bitbucket-search-v1/Q0X8oUSNT9GiVCILGg6iXw])]
	at org.elasticsearch.cluster.metadata.MetaData$Builder.build(MetaData.java:1111) ~[elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.gateway.MetaStateService.loadFullState(MetaStateService.java:72) ~[elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.gateway.GatewayMetaState.<init>(GatewayMetaState.java:86) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.node.Node.<init>(Node.java:499) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.node.Node.<init>(Node.java:265) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.bootstrap.Bootstrap$5.<init>(Bootstrap.java:212) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:212) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:333) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:136) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:127) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:124) [elasticsearch-cli-6.5.3.jar:6.5.3]
	at org.elasticsearch.cli.Command.main(Command.java:90) [elasticsearch-cli-6.5.3.jar:6.5.3]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:93) [elasticsearch-6.5.3.jar:6.5.3]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:86) [elasticsearch-6.5.3.jar:6.5.3]
[2020-03-02T07:55:44,559][ERROR][o.e.b.Bootstrap          ] [bitbucket_bundled]Exception
~~~


### Diagnosis
The error in elasticsearch logs points to an alias, which, in the above example, is due to an earlier index name, bitbucket-search-v1, that Bitbucket Server used.

### Cause
As the error indicates, index and alias names must be unique.

### Solution
* Resolution
  - Stop Bitbucket Server (this will also stop embedded Elasticsearch)

  - Remove the alias folder, <bitbucket-home>/shared/search/data/node/0/indices/Q0X8oUSNT9GiVCILGg6iXw in the above example

  - Restart Bitbucket Server



