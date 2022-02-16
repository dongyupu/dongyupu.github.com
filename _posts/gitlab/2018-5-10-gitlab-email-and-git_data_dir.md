---
layout: post
title: "gitlab set git_data_dir and qq enterprise email"
subtitle:   "gitlab email and git_data_dir"
date:       2018-05-10 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

gitlab 版本 gitlab-7.6.2_omnibus.5.3.0.ci.1-1.el6.x86_64.rpm

配置文件路径 `/etc/gitlab/gitlab.rb`

~~~ruby

# set qq enterprise email
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.exmail.qq.com"
gitlab_rails['smtp_port'] = 465
gitlab_rails['smtp_user_name'] = "email"
gitlab_rails['smtp_password'] = "passwd"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = true
gitlab_rails['gitlab_email_from'] = 'youremail'

## If you want to use a single non-default directory to store git data use:
git_data_dir "/app/gitlab/git-data"

~~~

重新加载配置

~~~bash
1.gitlab-ctl reconfigure

2.gitlab-ctl stop

3.gitlab-ctl start

~~~

问题：

1.timeout: down: nginx: 0s, normally up, want up

solution：

~~~bash
#查看日志首先
cat /var/log/gitlab/postgresql/current 
2018-05-09_08:31:39.60224 FATAL:  could not open lock file "/tmp/.s.PGSQL.5432.lock": Permission denied
~~~

杀掉所有的postgresql进程

~~~bash
ps -ef|grep postgresql

kill all_postgresql_pid
~~~

删除 /tmp/.s.PGSQL.5432.lock

~~~bash
rm -rf /tmp/.s.PGSQL.5432.lock
~~~

重新启动gitlab

~~~bash
gitlab-ctl start
~~~

不再报错了。



2.阿里云防火墙问题，通过外网域名无法访问
solution:
修改iptables，添加白名单。





