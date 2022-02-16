---
layout: post
title: "Gerrit Allow HTTP authentication & anonymous access"
subtitle:   "Allow HTTP authentication & anonymous access"
date:       2015-08-01 18:35:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-04.jpg"
---
gerrit的auth type=HTTP时，如何设置匿名的Anonymous用户浏览和下载呢？
+  Google之后找到了答案：<https://code.google.com/p/gerrit/issues/detail?id=1306> 

下面来说如何设置：

+  首先，你需要在etc/gerrit.config中设置loginUrl

~~~yml
	   [auth]
              type = HTTP
              loginUrl = http://yourserver/gerrit/login/  
       [download]
              scheme = ssh
              scheme = anon_http
              scheme = anon_git
~~~

+  然后，你需要设置你的反向代理服务器，我这里以nginx为例，你需要分别配置http://yourserver/gerrit/login/和http://yourserver/gerrit/  
	
+  edit /etc/nginx/sites-available/defualt
	
~~~bash
        location /gerrit/login/{
          proxy_pass http://127.0.0.1:8081; 
          proxy_set_header X-Forward-For $remote_addr;
          proxy_set_header Host $host;
          auth_basic "Gerrit Code Review";
          auth_basic_user_file /etc/nginx/passwd/gerrit.passwd;
          }
        location /gerrit/ { 
          proxy_pass http://127.0.0.1:8081;
          proxy_set_header X-Forward-For $remote_addr; 
          proxy_set_header Host $host;
          }
~~~

 最后，你需要配置相应仓库的权限，允许Anonymous Users读取相应的分支
 
