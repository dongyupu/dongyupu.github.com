---
layout: post
title: "Jenkins：Gerrit trigger settings"
subtitle:   "Verify closed ：Gerrit Verified Commands"
date:       2016-04-25 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

* 如果gerrit没有 强制开启 Verify 的话，那么在与jenkins集成时使用gerrit trigger时要注意，默认的

~~~bash
	* Gerrit Verified Commands
		
		Started			gerrit review <CHANGE>,<PATCHSET> --message 'Build Started <BUILDURL> <STARTED_STATS>'  --verified <VERIFIED> --code-review <CODE_REVIEW>
		
		Successful		gerrit review <CHANGE>,<PATCHSET> --message 'Build Successful <BUILDS_STATS>' --verified <VERIFIED> --code-review <CODE_REVIEW> --submit
		
		Failed			gerrit review <CHANGE>,<PATCHSET> --message 'Build Failed <BUILDS_STATS>' --verified <VERIFIED> --code-review <CODE_REVIEW>
		
		Unstable		gerrit review <CHANGE>,<PATCHSET> --message 'Build Unstable <BUILDS_STATS>' --verified <VERIFIED> --code-review <CODE_REVIEW>W>
		
		Not built		gerrit review <CHANGE>,<PATCHSET> --message 'No Builds Executed <BUILDS_STATS>' --verified <VERIFIED> --code-review <CODE_REVIEW>
~~~

* 默认的message中会带着  --verified <VERIFIED> 这样会导致Gerrit Verified Commands执行失败，无法在gerrit中看到构建信息。
需要删除--verified <VERIFIED> 才能执行成功，这样就可以在geerit中看到jenkins 的gerrit trigger发送的构建信息了。
