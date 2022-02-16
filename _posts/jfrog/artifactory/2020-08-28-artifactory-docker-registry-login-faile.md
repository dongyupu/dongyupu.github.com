---
layout: post
title: "Jfrog Artifactory docker registry login failed"
subtitle:   "Repository Path metho with port 8081"
date:       2020-08-28 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

### 如果artifactory配置了域名而没有使用https，docker login会失败。
* 使用官方镜像，通过istio的gateway暴露了80（8081）
* HTTP 配置使用 Repository Path method

### 解决办法
* 暴露8081端口（tcp）


~~~ yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: jfrog-artifactory-pro-gateway
spec:
  selector:
    istio: ingressgateway # use istio default controller
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "artifactory.hyxfjr.com"
  - port:
      number: 8081
      name: tcp
      protocol: TCP
    hosts:
    - "artifactory.hyxfjr.com"
 
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: jfrog-artifactory-pro-vs
spec:
  hosts:
  - "artifactory.hyxfjr.com"
  gateways:
    - jfrog-artifactory-pro-gateway
  http:
  - match:
    - port: 80
    route:
    - destination:
        host: jfrog-artifactory-pro
        port:
          number: 8081
  tcp:
  - match:
    - port: 8081
    route:
    - destination:
        host: jfrog-artifactory-pro
        port:
          number: 8081
~~~

