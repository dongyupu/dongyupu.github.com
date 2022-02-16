---
layout: post
title: "Gerrit-Review-connect-Jira"
subtitle:   "How to connect Jira and Gerrit"
date:       2015-08-15 23:35:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---
+  Gerrit and Jira connect , there are many ways

	+  In Jira : [JIRA Gerrit Plugin](https://marketplace.atlassian.com/plugins/com.meetme.plugins.jira.gerrit-plugin)
	+  In gerrit :[Plugin to integrate with Atlassian JIRA](https://gerrit-review.googlesource.com/#/admin/projects/plugins/its-jira)

	+  google Answers:
		+  <http://stackoverflow.com/questions/21328402/how-to-connect-jira-and-gerrit>
		+  <https://jira.neos.io/browse/COORD-4>
	+  在etc/gerrit.config中设置commentlink ,这样会在gerrit的commit massage中生成一个连接直达jira的问题链接  
	
~~~yml
       [commentlink "jira"]
			match = ([A-Z]+-[0-9]+)
			link = http://jira.cmri.cn/browse/$1	
~~~
	
+  its-jira 这个没试，还是看上面那个连接吧 

~~~yml
        [commentlink "JIRA"]
			match = ([A-Z]+-[0-9]+)
			link = https://jira.typo3.org/browse/$1
			association = SUGGESTED
        [its-jira]
			url=http://jira.typo3.org
        [Jira]
			#commentOnChangeAbandoned=true
			commentOnChangeCreated=false
			#commentOnChangeMerged=true
			#commentOnChangeRestored=true
			#commentOnCommentAdded=true
			#commentOnFirstLinkedPatchSetCreated=true
			commentOnPatchSetCreated=false
			commentOnRefUpdatedGitWeb=false
~~~
