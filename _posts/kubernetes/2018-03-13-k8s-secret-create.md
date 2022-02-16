---
layout: post
title: "k8s Secret"
subtitle:   "Secret 对象类型用来保存敏感信息，例如密码、OAuth 令牌和 ssh key。将这些信息放在 secret 中比放在 pod 的定义或者 docker 镜像中来说更加安全和灵活"
date:       2018-03-13 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
## 通过secret yaml文件创建pull image所用的secret

https://kubernetes.io/zh/docs/concepts/configuration/secret/

手动创建 Secret
<br>
您也可以先以 json 或 yaml 格式在文件中创建一个 secret 对象，然后创建该对象。
<br>
每一项必须是 base64 编码：

~~~bash
base64 -w 0 ~/.docker/config.json
~~~

yaml文件内容

~~~yml
registrykey-registry-vpc.cn-beijing.aliyuncs.com.yaml 
apiVersion: v1
kind: Secret
metadata:
  name: registry-vpc.cn-beijing.aliyuncs.com
  namespace: default
data:
  .dockerconfigjson: ewoJImF1dGhzIjogewoJCSJodHRwczovL2luZGV4LmRvY2tlci5pby92MS8iOiB7CgkJCSJhdXRoIjogIlpHOXVaM2wxY0hVNlptRnVaekV5TWpnPSIKCQl9LAoJCSJyZWdpc3RyeS1pbnRlcm5hbC5jbi1iZWlqaW5nLmFsaXl1bmNzLmNvbSI6IHsKCQkJImF1dGgiOiAiZDJGdWEyRm9ZV2wzWVdsZmRHVnpkRUE1Wm1KaGJtczZiMk14TWpNME5UWT0iCgkJfSwKCQkicmVnaXN0cnkuY24tYmVpamluZy5hbGl5dW5jcy5jb20iOiB7CgkJCSJhdXRoIjogImRHVnpkRjlqYUhWemFIVnFkV0Z1UURsbVltRnVhenBwTTBoQ1VHWTBVQ1JPYWpBPSIKCQl9Cgl9LAoJIkh0dHBIZWFkZXJzIjogewoJCSJVc2VyLUFnZW50IjogIkRvY2tlci1DbGllbnQvMTguMDYuMS1jZSAobGludXgpIgoJfQp9
type: kubernetes.io/dockerconfigjson
~~~

~~~bash
kubectl create -f registrykey-registry-vpc.cn-beijing.aliyuncs.com.yaml

secret "registry-vpc.cn-beijing.aliyuncs.com" created
~~~
