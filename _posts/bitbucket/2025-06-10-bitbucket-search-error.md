---
layout: post
title: "bitbucket unable-to-establish-a-connection-to-elasticsearch-during-index-synchronisation"
subtitle:   "Platform Notice: Data Center Only"
date:       2025-06-10 1:22:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

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
