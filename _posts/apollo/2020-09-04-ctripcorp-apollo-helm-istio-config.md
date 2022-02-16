---
layout: post
title: "ctripcorp apollo deploy on k8s"
subtitle:   "helm install with istio"
date:       2020-09-04 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

### 携程Apollo配置管理中心部署

## 代码显示有问题，需要的去GitHub看post

### 遇到的问题：
* mysql数据库需要先进行初始化
* service的名字是 apollo-service-apollo-adminservice、apollo-service-apollo-configservice；配置metaserver时需要注意
* 接入active directory后，默认的超级管理员apollo会被禁用，需要在数据库的ServerConfig表中更新 superAdmin的值，添加新的管理员。
* 记得更新value.yaml中的configdb.host为mysql的service的name即可（coreDNS内部解析）


### helm chart

### Apollo service

* value.yaml

~~~ yaml
configdb:
  name: apollo-configdb
  # apolloconfigdb host
  host: "apollo-configdb"
  port: 3306
  dbName: ApolloConfigDB
  # apolloconfigdb user name
  userName: "ApolloConfigDB"
  # apolloconfigdb password
  password: "password"
  connectionStringProperties: characterEncoding=utf8&useSSL=false
  service:
    # whether to create a Service for this host or not
    enabled: false
    fullNameOverride: ""
    port: 3306
    type: ClusterIP

configService:
  name: apollo-configservice
  fullNameOverride: ""
  replicaCount: 1
  containerPort: 8080
  image:
    repository: apolloconfig/apollo-configservice
    pullPolicy: IfNotPresent
  imagePullSecrets: []
  service:
    fullNameOverride: ""
    port: 8080
    targetPort: 8080
    type: ClusterIP
  liveness:
    initialDelaySeconds: 100
    periodSeconds: 10
  readiness:
    initialDelaySeconds: 30
    periodSeconds: 5
  config:
    # spring profiles to activate
    profiles: "github,kubernetes"
    # override apollo.config-service.url: config service url to be accessed by apollo-client 
    configServiceUrlOverride: ""
    # override apollo.admin-service.url: admin service url to be accessed by apollo-portal 
    adminServiceUrlOverride: ""
  # environment variables passed to the container, e.g. JAVA_OPTS
  env: {}
  strategy: {
    JAVA_OPTS: -Duser.timezone=Asia/Shanghai
  }
  resources: {}
  nodeSelector: {
    atlassian: crucible
  }
  tolerations: []
  affinity: {}

adminService:
  name: apollo-adminservice
  fullNameOverride: ""
  replicaCount: 1
  containerPort: 8090
  image:
    repository: apolloconfig/apollo-adminservice
    pullPolicy: IfNotPresent
  imagePullSecrets: []
  service:
    fullNameOverride: ""
    port: 8090
    targetPort: 8090
    type: ClusterIP
  liveness:
    initialDelaySeconds: 100
    periodSeconds: 10
  readiness:
    initialDelaySeconds: 30
    periodSeconds: 5
  config:
    # spring profiles to activate
    profiles: "github,kubernetes"
  # environment variables passed to the container, e.g. JAVA_OPTS
  env: {
    JAVA_OPTS: -Duser.timezone=Asia/Shanghai
  }
  strategy: {}
  resources: {}
  nodeSelector: {
    atlassian: crucible
  }
  tolerations: []
  affinity: {}
~~~


* templates/apolloservicegateway.yaml

~~~ yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: apollo-service-gateway
spec:
  selector:
    istio: ingressgateway # use istio default controller
  servers:
  - hosts:
    - "apollo.config.hyxfjr.com"
    port:
      name: tcp
      number: 80
      protocol: TCP
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: apollo-service-vs
spec:
  hosts:
  - "config.hyxfjr.com"
  gateways:
    - apollo-service-gateway
  tcp:
  - match:
    - port: 80
    route:
    - destination:
        host: apollo-configservice
        port:
          number: 8090
~~~


* templates/deployment-adminservice.yaml

~~~ yaml
# configmap for apollo-adminservice
kind: ConfigMap
apiVersion: v1
metadata:
  {{- $adminServiceFullName := include "apollo.adminService.fullName" . }}
  name: {{ $adminServiceFullName }}
data:
  application-github.properties: |
    spring.datasource.url = jdbc:mysql://{{include "apollo.configdb.serviceName" .}}:{{ .Values.configdb.service.port }}/{{ .Values.configdb.dbName }}{{ if .Values.configdb.connectionStringProperties }}?{{ .Values.configdb.connectionStringProperties }}{{ end }}
    spring.datasource.username = {{ required "configdb.userName is required!" .Values.configdb.userName }}
    spring.datasource.password = {{ required "configdb.password is required!" .Values.configdb.password }}

---
kind: Deployment
apiVersion: apps/v1
metadata:
  name: {{ $adminServiceFullName }}
  labels:
    {{- include "apollo.service.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.adminService.replicaCount }}
  selector:
    matchLabels:
      app: {{ $adminServiceFullName }}
  {{- with .Values.adminService.strategy }}
  strategy:
    {{- toYaml . | nindent 4 }}
  {{- end }}
  template:
    metadata:
      annotations:
        sidecar.istio.io/inject: "false"
      labels:
        app: {{ $adminServiceFullName }}
    spec:
      {{- with .Values.adminService.imagePullSecrets }}
        imagePullSecrets:
          {{- toYaml . | nindent 8 }}
      {{- end }}
      volumes:
        - name: volume-configmap-{{ $adminServiceFullName }}
          configMap:
            name: {{ $adminServiceFullName }}
            items:
              - key: application-github.properties
                path: application-github.properties
            defaultMode: 420
      containers:
        - name: {{ .Values.adminService.name }}
          image: "{{ .Values.adminService.image.repository }}:{{ .Chart.AppVersion }}"
          imagePullPolicy: {{ .Values.adminService.image.pullPolicy }}
          ports:
            - name: http
              containerPort: {{ .Values.adminService.containerPort }}
              protocol: TCP
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: {{ .Values.adminService.config.profiles | quote }}
          {{- range $key, $value := .Values.adminService.env }}
            - name: {{ $key }}
              value: {{ $value }}
          {{- end }}
          volumeMounts:
            - name: volume-configmap-{{ $adminServiceFullName }}
              mountPath: /apollo-adminservice/config/application-github.properties
              subPath: application-github.properties
          livenessProbe:
            tcpSocket:
              port: {{ .Values.adminService.containerPort }}
            initialDelaySeconds: {{ .Values.adminService.liveness.initialDelaySeconds }}
            periodSeconds: {{ .Values.adminService.liveness.periodSeconds }}
          readinessProbe:
            httpGet:
              path: /health
              port: {{ .Values.adminService.containerPort }}
            initialDelaySeconds: {{ .Values.adminService.readiness.initialDelaySeconds }}
            periodSeconds: {{ .Values.adminService.readiness.periodSeconds }}
          resources:
            {{- toYaml .Values.adminService.resources | nindent 12 }}
    {{- with .Values.adminService.nodeSelector }}
      nodeSelector:
        {{- toYaml . | nindent 8 }}
    {{- end }}
    {{- with .Values.adminService.affinity }}
      affinity:
        {{- toYaml . | nindent 8 }}
    {{- end }}
    {{- with .Values.adminService.tolerations }}
      tolerations:
        {{- toYaml . | nindent 8 }}
    {{- end }}

~~~


* templates/deployment-configservice.yaml

~~~ yaml

# configmap for apollo-configservice
kind: ConfigMap
apiVersion: v1
metadata:
  {{- $configServiceFullName := include "apollo.configService.fullName" . }}
  name: {{ $configServiceFullName }}
data:
  application-github.properties: |
    spring.datasource.url = jdbc:mysql://{{include "apollo.configdb.serviceName" .}}:{{ .Values.configdb.service.port }}/{{ .Values.configdb.dbName }}{{ if .Values.configdb.connectionStringProperties }}?{{ .Values.configdb.connectionStringProperties }}{{ end }}
    spring.datasource.username = {{ required "configdb.userName is required!" .Values.configdb.userName }}
    spring.datasource.password = {{ required "configdb.password is required!" .Values.configdb.password }}
    apollo.config-service.url = {{ include "apollo.configService.serviceUrl" .}}
    apollo.admin-service.url = {{ include "apollo.adminService.serviceUrl" .}}

---
kind: Deployment
apiVersion: apps/v1
metadata:
  name: {{ $configServiceFullName }}
  labels:
    {{- include "apollo.service.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.configService.replicaCount }}
  selector:
    matchLabels:
      app: {{ $configServiceFullName }}
  {{- with .Values.configService.strategy }}
  strategy:
    {{- toYaml . | nindent 4 }}
  {{- end }}
  template:
    metadata:
      annotations:
        sidecar.istio.io/inject: "false"
      labels:
        app: {{ $configServiceFullName }}
    spec:
      {{- with .Values.configService.imagePullSecrets }}
        imagePullSecrets:
          {{- toYaml . | nindent 8 }}
      {{- end }}
      volumes:
        - name: volume-configmap-{{ $configServiceFullName }}
          configMap:
            name: {{ $configServiceFullName }}
            items:
              - key: application-github.properties
                path: application-github.properties
            defaultMode: 420
      containers:
        - name: {{ .Values.configService.name }}
          image: "{{ .Values.configService.image.repository }}:{{ .Chart.AppVersion }}"
          imagePullPolicy: {{ .Values.configService.image.pullPolicy }}
          ports:
            - name: http
              containerPort: {{ .Values.configService.containerPort }}
              protocol: TCP
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: {{ .Values.configService.config.profiles | quote }}
          {{- range $key, $value := .Values.configService.env }}
            - name: {{ $key }}
              value: {{ $value }}
          {{- end }}
          volumeMounts:
            - name: volume-configmap-{{ $configServiceFullName }}
              mountPath: /apollo-configservice/config/application-github.properties
              subPath: application-github.properties
          livenessProbe:
            tcpSocket:
              port: {{ .Values.configService.containerPort }}
            initialDelaySeconds: {{ .Values.configService.liveness.initialDelaySeconds }}
            periodSeconds: {{ .Values.configService.liveness.periodSeconds }}
          readinessProbe:
            httpGet:
              path: /health
              port: {{ .Values.configService.containerPort }}
            initialDelaySeconds: {{ .Values.configService.readiness.initialDelaySeconds }}
            periodSeconds: {{ .Values.configService.readiness.periodSeconds }}
          resources:
            {{- toYaml .Values.configService.resources | nindent 12 }}
    {{- with .Values.configService.nodeSelector }}
      nodeSelector:
        {{- toYaml . | nindent 8 }}
    {{- end }}
    {{- with .Values.configService.affinity }}
      affinity:
        {{- toYaml . | nindent 8 }}
    {{- end }}
    {{- with .Values.configService.tolerations }}
      tolerations:
        {{- toYaml . | nindent 8 }}
    {{- end }}

~~~


* templates/service-adminservice.yaml

~~~ yaml
kind: Service
apiVersion: v1
metadata:
  name: {{ include "apollo.adminService.serviceName" . }}
  labels:
    {{- include "apollo.service.labels" . | nindent 4 }}
spec:
  type: {{ .Values.adminService.service.type }}
  ports:
    - name: http
      protocol: TCP
      port: {{ .Values.adminService.service.port }}
      targetPort: {{ .Values.adminService.service.targetPort }}
  selector:
    app: {{ include "apollo.adminService.fullName" . }}
~~~


* templates/service-configservice.yaml

~~~ yaml
kind: Service
apiVersion: v1
metadata:
  name: {{ include "apollo.configService.serviceName" . }}
  labels:
    {{- include "apollo.service.labels" . | nindent 4 }}
spec:
  type: {{ .Values.configService.service.type }}
  ports:
    - name: http
      protocol: TCP
      port: {{ .Values.configService.service.port }}
      targetPort: {{ .Values.configService.service.targetPort }}
  selector:
    app: {{ include "apollo.configService.fullName" . }}
~~~



* 删除不用 templates/service-configdb.yaml


### Apollo portal


* value.yaml

~~~ yaml
name: apollo-portal
fullNameOverride: ""
replicaCount: 1
containerPort: 8070
image:
  repository: apolloconfig/apollo-portal
  pullPolicy: IfNotPresent
imagePullSecrets: []
service:
  fullNameOverride: ""
  port: 8070
  targetPort: 8070
  type: ClusterIP
  sessionAffinity: ClientIP
ingress:
  enabled: false
  annotations: {}
  hosts:
    - host: ""
      paths: []
  tls: []
liveness:
  initialDelaySeconds: 100
  periodSeconds: 10
readiness:
  initialDelaySeconds: 30
  periodSeconds: 5
# environment variables passed to the container, e.g. JAVA_OPTS
env: {}
strategy: {}
resources: {}
nodeSelector: {
  atlassian: crucible
}
tolerations: []
affinity: {}

config:
  # spring profiles to activate
  profiles: "github,ldap"
  # specify the env names, e.g. dev,pro
  envs: "dev"
  # specify the meta servers, e.g.
  # dev: http://apollo-configservice-dev:8080
  # pro: http://apollo-configservice-pro:8080
  metaServers: {
    dev: "http://apollo-service-apollo-configservice:8080"
  }
  # specify the context path, e.g. /apollo
  contextPath: ""
  # extra config files for apollo-portal, e.g. application-ldap.yml
  # MS active directory config
  files: 
    application-ldap.yml: |
      spring:
        ldap:
          base: "dc=domain,dc=com"
          username: "adname"
          password: "password"
          searchFilter: "(sAMAccountName={0})"
          urls:
          - "ldap://yourtIP:389"

      ldap:
        mapping:
          objectClass: "user"
          loginId: "sAMAccountName"
          userDisplayName: "displayName"
          email: "mail"

portaldb:
  name: apollo-portaldb
  # apolloportaldb host
  host: apollo-portaldb
  port: 3306
  dbName: ApolloPortalDB
  # apolloportaldb user name
  userName: ApolloPortalDB
  # apolloportaldb password
  password: password
  connectionStringProperties: characterEncoding=utf8&useSSL=false
  service:
    # whether to create a Service for this host or not
    enabled: false
    fullNameOverride: ""
    port: 3306
    type: ClusterIP
~~~


* templates/apolloportalgateway.yaml

~~~ yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: apollo-portal-gateway
spec:
  selector:
    istio: ingressgateway # use istio default controller
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "apollo.hyxfjr.com"

---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: apollo-portal-vs
spec:
  hosts:
  - "apollo.hyxfjr.com"
  gateways:
    - apollo-portal-gateway
  http:
  - match:
    - port: 80
    route:
    - destination:
        host: apollo-portal
        port:
          number: 8070

~~~


* templates/deployment-portal.yaml

~~~ yaml

# configmap for apollo-portal
kind: ConfigMap
apiVersion: v1
metadata:
  {{- $portalFullName := include "apollo.portal.fullName" . }}
  name: {{ $portalFullName }}
data:
  application-github.properties: |
    spring.datasource.url = jdbc:mysql://{{include "apollo.portaldb.serviceName" .}}:{{ .Values.portaldb.service.port }}/{{ .Values.portaldb.dbName }}{{ if .Values.portaldb.connectionStringProperties }}?{{ .Values.portaldb.connectionStringProperties }}{{ end }}
    spring.datasource.username = {{ required "portaldb.userName is required!" .Values.portaldb.userName }}
    spring.datasource.password = {{ required "portaldb.password is required!" .Values.portaldb.password }}
    {{- if .Values.config.envs }}
    apollo.portal.envs = {{ .Values.config.envs }}
    {{- end }}
    {{- if .Values.config.contextPath }}
    server.servlet.context-path = {{ .Values.config.contextPath }}
    {{- end }}
  apollo-env.properties: |
    {{- range $env, $address := .Values.config.metaServers }}
    {{ $env }}.meta = {{ $address }}
    {{- end }}
{{- range $fileName, $content := .Values.config.files }}
{{ $fileName | indent 2 }}: |
{{ $content | indent 4 }}
{{- end }}

---
kind: Deployment
apiVersion: apps/v1
metadata:
  name: {{ $portalFullName }}
  labels:
    {{- include "apollo.portal.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ $portalFullName }}
  {{- with .Values.strategy }}
  strategy:
    {{- toYaml . | nindent 4 }}
  {{- end }}
  template:
    metadata:
      annotations:
        sidecar.istio.io/inject: "false"
      labels:
        app: {{ $portalFullName }}
    spec:
      hostAliases:
        - hostnames:
          - "domain.com"
          ip: "your ms ad ip if you can't access your URL domain.com"
      {{- with .Values.imagePullSecrets }}
        imagePullSecrets:
          {{- toYaml . | nindent 8 }}
      {{- end }}
      volumes:
        - name: configmap-{{ $portalFullName }}
          configMap:
            name: {{ $portalFullName }}
            items:
              - key: application-github.properties
                path: application-github.properties
              - key: apollo-env.properties
                path: apollo-env.properties
            {{- range $fileName, $content := .Values.config.files }}
              - key: {{ $fileName }}
                path: {{ $fileName }}
            {{- end }}
            defaultMode: 420
      containers:
        - name: {{ .Values.name }}
          image: "{{ .Values.image.repository }}:{{ .Chart.AppVersion }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: {{ .Values.containerPort }}
              protocol: TCP
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: {{ .Values.config.profiles | quote }}
          {{- range $key, $value := .Values.env }}
            - name: {{ $key }}
              value: {{ $value }}
          {{- end }}
          volumeMounts:
            - name: configmap-{{ $portalFullName }}
              mountPath: /apollo-portal/config/application-github.properties
              subPath: application-github.properties
            - name: configmap-{{ $portalFullName }}
              mountPath: /apollo-portal/config/apollo-env.properties
              subPath: apollo-env.properties
          {{- range $fileName, $content := .Values.config.files }}
            - name: configmap-{{ $portalFullName }}
              mountPath: /apollo-portal/config/{{ $fileName }}
              subPath: {{ $fileName }}
          {{- end }}
          livenessProbe:
            tcpSocket:
              port: {{ .Values.containerPort }}
            initialDelaySeconds: {{ .Values.liveness.initialDelaySeconds }}
            periodSeconds: {{ .Values.liveness.periodSeconds }}
          readinessProbe:
            httpGet:
              path: {{ .Values.config.contextPath }}/health
              port: {{ .Values.containerPort }}
            initialDelaySeconds: {{ .Values.readiness.initialDelaySeconds }}
            periodSeconds: {{ .Values.readiness.periodSeconds }}
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
    {{- with .Values.nodeSelector }}
      nodeSelector:
        {{- toYaml . | nindent 8 }}
    {{- end }}
    {{- with .Values.affinity }}
      affinity:
        {{- toYaml . | nindent 8 }}
    {{- end }}
    {{- with .Values.tolerations }}
      tolerations:
        {{- toYaml . | nindent 8 }}
    {{- end }}

~~~

* templates/service-portal.yaml

~~~ yaml
kind: Service
apiVersion: v1
metadata:
  name: {{ include "apollo.portal.serviceName" . }}
  labels:
    {{- include "apollo.portal.labels" . | nindent 4 }}
spec:
  type: {{ .Values.service.type }}
  ports:
    - name: http
      protocol: TCP
      port: {{ .Values.service.port }}
      targetPort: {{ .Values.service.targetPort }}
  selector:
    app: {{ include "apollo.portal.fullName" . }}
  sessionAffinity: {{ .Values.service.sessionAffinity }}
~~~


* 删除不用 templates/service-portadb.yaml 

* ingress也不用，删除也无妨。

### mysql的部署就不放了，集群内访问，不用配置域名或ingress啥的，直接服务名访问即可。
