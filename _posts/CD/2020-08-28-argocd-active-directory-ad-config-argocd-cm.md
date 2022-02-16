---
layout: post
title: "argocd msad config"
subtitle:   "argocd ActiveDirectory config"
date:       2020-11-23 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
* 参考 https://dexidp.io/docs/connectors/ldap/

* argocd msad config
* 映射ad IP 和域名到 argo-cd-argocd-dex-server

~~~
apiVersion: v1
data:
  application.instanceLabelKey: argocd.argoproj.io/instance
  dex.config: |-
    connectors:
      - type: ldap
        name: ActiveDirectory
        id: ad
        config:
          host: Your_Ad_Ip:389
          insecureNoSSL: true
          insecureSkipVerify: true

          bindDN: cn=your login name,cn=users,dc=domain,dc=com
          bindPW: password
          usernamePrompt: sAMAccountName
          userSearch:
            baseDN: DC=domain,DC=com
            filter: "(objectClass=user)"
            username: sAMAccountName
            nameAttr: displayName
            emailAttr: mail
            idAttr: sAMAccountName
  url: https://argocd.hyxfjr.com
kind: ConfigMap
metadata:
  creationTimestamp: "2020-11-13T09:01:17Z"
  labels:
    app.kubernetes.io/component: server
    app.kubernetes.io/instance: argo-cd
    app.kubernetes.io/managed-by: Tiller
    app.kubernetes.io/name: argocd-cm
    app.kubernetes.io/part-of: argocd
    helm.sh/chart: argo-cd-12
  name: argocd-cm
  namespace: argocd
  resourceVersion: "99049719"
  selfLink: /api/v1/namespaces/argocd/configmaps/argocd-cm
  uid: 4fb4aa67-068e-4316-a780-6e6f4ef1a2a4

~~~
