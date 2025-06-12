---
layout: post
title: "behind a reverse proxy server"
subtitle:   "behind a reverse proxy server"
date:       2025-06-12 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
### centos 7.9 httpd 2.4.6 apache subversion 1.7.14 域名配置https，LB转发后端http协议

### 修复
~~~
vi /etc/httpd/conf/httpd.conf

# behind a reverse proxy https
RequestHeader edit Destination ^https http early

~~~


https://stackoverflow.com/questions/47630758/subversion-edge-behind-reverse-proxy-server-returns-http-status-502-bad-gateway

This error occurs because nginx passes the Destination header to the subversion server without modification. If you use https on the nginx server but only http on the subversion server, subversion will not be able to execute a COPY or MOVE method because the Destination header contains a https:// value.

You can confirm this by checking nginx's access.log; if you see a COPY or MOVE request with a 502 response, followed by a DELETE, then this is likely the issue, e.g.:

… COPY /foo/!svn/rvr/111/trunk HTTP/1.1" 502 …
… DELETE /foo/!svn/txn/111-6v HTTP/1.1" 204 …
You can solve this by rewriting the Destination header on either server:

Apache on the Subversion server
RequestHeader edit Destination ^https http early
Or nginx proxy server:
set $dest $http_destination;
if ($http_destination ~ "^https://(.+)$") {
    set $dest http://$1;
}
proxy_set_header Destination $dest;
