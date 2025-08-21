---
layout: post
title: "Jira10.3.5 SQL查询所有登录过的用户"
subtitle:   "SQL查询所有登录过的用户"
date:       2025-08-21 1:22:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

~~~
SELECT DISTINCT
    u.user_name,
    u.display_name,
    u.email_address,
    FROM_UNIXTIME(
        MAX(CASE WHEN a.attribute_name = 'login.lastLoginMillis' THEN a.attribute_value END) / 1000,
        '%y-%m-%d %H:%i:%s'
    ) AS last_login_time,
    FROM_UNIXTIME(
        MAX(CASE WHEN a.attribute_name = 'login.previousLoginMillis' THEN a.attribute_value END) / 1000,
        '%y-%m-%d %H:%i:%s'
    ) AS previous_login_time
FROM
    cwd_user u
JOIN
    cwd_user_attributes a ON u.id = a.user_id
WHERE
    a.attribute_name IN ('login.lastLoginMillis', 'login.previousLoginMillis')
GROUP BY
    u.user_name, u.display_name, u.email_address
ORDER BY
    last_login_time DESC;
~~~
