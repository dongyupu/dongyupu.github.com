---
layout: post
title: "Atlassian Crowd data center config"
subtitle:   "Installing Crowd Data Center"
date:       2021-06-22 17:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

* https://confluence.atlassian.com/crowd/installing-crowd-data-center-935369773.html

* 4. Add the remaining nodes to the cluster
- Copy the Crowd installation directory to the new node. 
- Create a home directory, like you did for the first node, and mount shared as its sub-directory.
- Edit crowd-init.properties, and enter the path to the home directory that you just created. 

- Where can I find crowd-init.properties?

- `Go to <installation-directory>/apache-tomcat/conf/Catalina/localhost, and delete the openidserver.xml file. This is needed because currently the CrowdID component doesn't support clustering, and it must be enabled only on the first node. The component will work as usual.`
- Start Crowd. It will read the configuration from the shared directory, and start without any extra setup.
- Take a look around the new Crowd instance. Verify that user and group management, directory synchronization, and any custom integrations all work as expected.
- Again, verify that the node was added to the cluster. In Crowd, go to  > Clustering.

### Update

- 使用NFS挂载时需要注意文件夹权限的问题，官方镜像使用的crowd用户启动的服务，文件夹权限应该修改为crowd组可读写，否则会导致权限问题。
- 遇到2次权限问题导致的服务无法启动，通过查看日志发现shared目录权限背修改为root了，改回来即可

~~~ yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  labels:
    app: crowd
    release: crowd
  name: crowd-node
  namespace: atlassian
spec:
  replicas: 1
  selector:
    matchLabels:
      app: crowd
      release: crowd
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      annotations:
        sidecar.istio.io/inject: "false"
      labels:
        app: crowd
        release: crowd
    spec:
      containers:
      - env:
        - name: TZ
          value: Asia/Shanghai
        - name: CROWD_URL
          value: http://loaclhost:8095/crowd
        - name: LOGIN_BASE_URL
          value: http://loaclhost:8095
        - name: SPLASH_CONTEXT
          value: ROOT
        - name: CROWD_CONTEXT
          value: crowd
        - name: CROWDID_CONTEXT
          value: openidserver
        - name: OPENID_CLIENT_CONTEXT
          value: openclient
        - name: DEMO_CONTEXT
          value: demo
        - name: JAVA_OPTS
          value: -javaagent:/var/atlassian/crowd/agent/atlassian-agent.jar -Duser.timezone=Asia/Shanghai
        - name: JVM_MINIMUM_MEMORY
          value: 1024m
        - name: JVM_MAXIMUM_MEMORY
          value: 2048m
        image: dongyupu/crowd:3.5.0
        imagePullPolicy: IfNotPresent
        name: crowd
        ports:
        - containerPort: 8095
          name: http
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
        - mountPath: /var/atlassian/crowd
          name: crowd-data
        - mountPath: /var/atlassian/crowd/shared
          name: crowd-shared-data
      dnsPolicy: ClusterFirst
      hostAliases:
      - hostnames:
        - domain.com
        ip: 172.16.87.66
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
      volumes:
      - name: crowd-data
        nfs:
          path: /atlassian/crowd-node1
          server: 172.16.131.173
      - name: crowd-shared-data
        nfs:
          path: /atlassian/crowd/shared
          server: 172.16.131.173






