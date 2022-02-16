---
layout: post
title: "add baidu analysis for Jekyll bootstrap"
subtitle:   "just add the scripts in head.html"
date:       2015-12-08 17:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---
按照百度统计的官方说明，在每个页面的head里面添加上百度的代码即可

在_includes/head.html中添加你的百度代码即可

```javascript
<!-- baidu analyze -->
<script>
	var _hmt = _hmt || [];
	(function() {
	  var hm = document.createElement("script");
	  hm.src = "//hm.baidu.com/hm.js?7068b1fbc824972f73bae59d1842763c";
	  var s = document.getElementsByTagName("script")[0]; 
	  s.parentNode.insertBefore(hm, s);
	  })();
	</script>
```
