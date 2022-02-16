---
layout: post
title: "TestRail integration: auth active directory"
subtitle:   "auth active directory"
date:       2019-11-3 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

[TestRail配置MS active director](http://docs.gurock.com/testrail-integration/auth-activedirectory)

~~~
define('AUTH_HOST', 'domain.com');            # ip或者域名
define('AUTH_PORT', 389);
define('AUTH_BIND_DN', 'CN=yourusername,CN=Users,DC=domain,DC=com');  #bind dn
define('AUTH_BIND_PASSWORD', 'yourpassword');
define('AUTH_DN', 'DC=domain,DC=com');
define('AUTH_DOMAIN', 'domain');    #这里根据你的情况配置
define('AUTH_NAME_ATTRIBUTE', 'sAMAccountName');
define('AUTH_FILTER', '');
define('AUTH_MAIL_ATTRIBUTE', 'mail');
define('AUTH_CREATE_ACCOUNT', false);
define('AUTH_FALLBACK', true);
define('AUTH_MEMBERSHIP', '');
~~~

