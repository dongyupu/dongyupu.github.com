---
layout: post
title: "deploy gitlab with istio"
subtitle:   "config istio-ingressgateway,gitlab-ce-gateway,gitlab-virtualservice"
date:       2019-09-05 14:16:22 +0800
author:     "Dongyupu"
header-img: "img/bg-starfield.gif"
---
### 使用istio部署gitlab-ce

* 1.修改istio-ingressgateway的端口，打开22端口

~~~ yaml
kubectl -n istio-system edit service/istio-ingressgateway
# i（insert） 添加 22 端口 tcp 
  - name: tcp-22-22
    port: 22
    protocol: TCP
    targetPort: 22 
# wp 保存退出应用更新
~~~

* 2.部署gitlab-ce的helm chart (已包含了Gateway和VirtualService的部署)

[参考](https://github.com/dongyupu/helm-charts)


* 3.部署Gateway 和 VirtualService (可以单独部署)

~~~ yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: gitlab-ce-gateway
spec:
  selector:
    istio: ingressgateway # use istio default controller
  servers:
    - hosts:
      - "gitlab-ce.your_url.com"
      port:
        name: http
        number: 80
        protocol: HTTP
    - hosts:
      - "gitlab-ce.your_url.com"
      port:
        name: tcp
        number: 22
        protocol: TCP
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: gitlab-ce-vs
spec:
  hosts:
  - "gitlab-ce.your_url.com"
  gateways:
    - gitlab-ce-gateway
  http:
    - match:
      - port: 80
      route:
        - destination:
            host: gitlab-ce
            port:
              number: 80
  hosts:
  - "gitlab-ce.your_url.com"
  gateways:
    - gitlab-ce-gateway
  tcp:
    - match:
      - port: 22
      route:
        - destination:
            host: gitlab-ce
            port:
              number: 22
~~~




