---
layout: post
title: "grafana admin password"
subtitle:   "grafana 7.1.5"
date:       2020-09-03 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
### istio grafana deployment

~~~
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "4"
  creationTimestamp: "2019-09-04T07:42:40Z"
  generation: 4
  labels:
    app: grafana
    chart: grafana
    heritage: Tiller
    release: istio
  name: grafana
  namespace: istio-system
  resourceVersion: "77338379"
  selfLink: /apis/apps/v1beta2/namespaces/istio-system/deployments/grafana
  uid: 92f16829-cee7-11e9-8dc2-5254006b5d25
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: grafana
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      annotations:
        prometheus.io/scrape: "true"
        sidecar.istio.io/inject: "false"
      creationTimestamp: null
      labels:
        app: grafana
        chart: grafana
        heritage: Tiller
        release: istio
    spec:
      affinity:
        nodeAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - preference:
              matchExpressions:
              - key: beta.kubernetes.io/arch
                operator: In
                values:
                - amd64
            weight: 2
          - preference:
              matchExpressions:
              - key: beta.kubernetes.io/arch
                operator: In
                values:
                - ppc64le
            weight: 2
          - preference:
              matchExpressions:
              - key: beta.kubernetes.io/arch
                operator: In
                values:
                - s390x
            weight: 2
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: beta.kubernetes.io/arch
                operator: In
                values:
                - amd64
                - ppc64le
                - s390x
      containers:
      - env:
        - name: GRAFANA_PORT
          value: "3000"
        - name: GF_AUTH_BASIC_ENABLED
          value: "false"
        - name: GF_AUTH_ANONYMOUS_ENABLED
          value: "true"
        - name: GF_AUTH_ANONYMOUS_ORG_ROLE
          value: Admin
        - name: GF_PATHS_DATA
          value: /data/grafana
        image: grafana/grafana:7.1.5
        imagePullPolicy: IfNotPresent
        name: grafana
        ports:
        - containerPort: 3000
          protocol: TCP
        readinessProbe:
          failureThreshold: 3
          httpGet:
            path: /login
            port: 3000
            scheme: HTTP
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
        resources:
          requests:
            cpu: 10m
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
        - mountPath: /var/lib/grafana/plugins
          name: plugins
        - mountPath: /data/grafana
          name: data
        - mountPath: /var/lib/grafana/dashboards/istio/galley-dashboard.json
          name: dashboards-istio-galley-dashboard
          readOnly: true
          subPath: galley-dashboard.json
        - mountPath: /var/lib/grafana/dashboards/istio/istio-mesh-dashboard.json
          name: dashboards-istio-istio-mesh-dashboard
          readOnly: true
          subPath: istio-mesh-dashboard.json
        - mountPath: /var/lib/grafana/dashboards/istio/istio-performance-dashboard.json
          name: dashboards-istio-istio-performance-dashboard
          readOnly: true
          subPath: istio-performance-dashboard.json
        - mountPath: /var/lib/grafana/dashboards/istio/istio-service-dashboard.json
          name: dashboards-istio-istio-service-dashboard
          readOnly: true
          subPath: istio-service-dashboard.json
        - mountPath: /var/lib/grafana/dashboards/istio/istio-workload-dashboard.json
          name: dashboards-istio-istio-workload-dashboard
          readOnly: true
          subPath: istio-workload-dashboard.json
        - mountPath: /var/lib/grafana/dashboards/istio/mixer-dashboard.json
          name: dashboards-istio-mixer-dashboard
          readOnly: true
          subPath: mixer-dashboard.json
        - mountPath: /var/lib/grafana/dashboards/istio/pilot-dashboard.json
          name: dashboards-istio-pilot-dashboard
          readOnly: true
          subPath: pilot-dashboard.json
        - mountPath: /etc/grafana/provisioning/datasources/datasources.yaml
          name: config
          subPath: datasources.yaml
        - mountPath: /etc/grafana/provisioning/dashboards/dashboardproviders.yaml
          name: config
          subPath: dashboardproviders.yaml
      dnsPolicy: ClusterFirst
      nodeSelector:
        apps: istio
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext:
        fsGroup: 472
        runAsUser: 472
      terminationGracePeriodSeconds: 30
      volumes:
      - configMap:
          defaultMode: 420
          name: istio-grafana
        name: config
        # 挂载NFS盘 data和plugins
      - name: data
        nfs:
          path: /istio/grafana/data
          server: 172.16.131.173
      - name: plugins
        nfs:
          path: /istio/grafana/plugins
          server: 172.16.131.173
      - configMap:
          defaultMode: 420
          name: istio-grafana-configuration-dashboards-galley-dashboard
        name: dashboards-istio-galley-dashboard
      - configMap:
          defaultMode: 420
          name: istio-grafana-configuration-dashboards-istio-mesh-dashboard
        name: dashboards-istio-istio-mesh-dashboard
      - configMap:
          defaultMode: 420
          name: istio-grafana-configuration-dashboards-istio-performance-dashboard
        name: dashboards-istio-istio-performance-dashboard
      - configMap:
          defaultMode: 420
          name: istio-grafana-configuration-dashboards-istio-service-dashboard
        name: dashboards-istio-istio-service-dashboard
      - configMap:
          defaultMode: 420
          name: istio-grafana-configuration-dashboards-istio-workload-dashboard
        name: dashboards-istio-istio-workload-dashboard
      - configMap:
          defaultMode: 420
          name: istio-grafana-configuration-dashboards-mixer-dashboard
        name: dashboards-istio-mixer-dashboard
      - configMap:
          defaultMode: 420
          name: istio-grafana-configuration-dashboards-pilot-dashboard
        name: dashboards-istio-pilot-dashboard
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2019-09-04T07:42:52Z"
    lastUpdateTime: "2019-09-04T07:42:52Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2019-09-04T07:42:41Z"
    lastUpdateTime: "2020-09-03T06:06:29Z"
    message: ReplicaSet "grafana-694cd96bb8" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 4
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1

~~~

* 登录，默认的用户名密码为 `admin/admin` ，登录成功会要求你设置新的密码。

* 如上，istio中部署grafana时指定了`GF_PATHS_DATA`值。grafana.db存储在这个位置。





