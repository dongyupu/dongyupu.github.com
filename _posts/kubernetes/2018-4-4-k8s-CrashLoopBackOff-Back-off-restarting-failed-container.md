---
layout: post
title: "CrashLoopBackOff Back-off restarting failed container"
subtitle:   "A container exits when its main process exits. to hold the container open, run while loop to keep it running forever."
date:       2018-04-4 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
A container exits when its main process exits. to hold the container open, run while loop to keep it running forever.
<br>
k8s提示容器处于 CrashLoopBackOff 状态，一直不停的重启，想了半天怎么回事，后来一想启动脚本退出时执行 exit 了，主进程退出容器关闭了，导致k8s不停重启container。

~~~bash
[root@101.201.100.5/10.25.112.37 bus-merchant-dev]# kubectl describe pod wk-scanpay-gin-wk-scanpay-gin-657f8cc6b5-8t4nm --namespace=testing
Name:           wk-scanpay-gin-wk-scanpay-gin-657f8cc6b5-8t4nm
Namespace:      testing
Node:           cn-beijing.i-2zecnfjkjgujdq5zacmn/172.16.110.216
Start Time:     Tue, 03 Apr 2018 09:01:10 +0800
Labels:         app=wk-scanpay-gin
                pod-template-hash=2139477261
                release=wk-scanpay-gin
Annotations:    kubernetes.io/created-by={"kind":"SerializedReference","apiVersion":"v1","reference":{"kind":"ReplicaSet","namespace":"testing","name":"wk-scanpay-gin-wk-scanpay-gin-657f8cc6b5","uid":"7f86f335-36da-1...
Status:         Running
IP:             10.10.5.13
Controlled By:  ReplicaSet/wk-scanpay-gin-wk-scanpay-gin-657f8cc6b5
Containers:
  wk-scanpay-gin:
    Container ID:   docker://6884a801e99ec46cb9bdc06083ac9d683b58c92a2d53f8593ace4494c497cf06
    Image:          registry-vpc.cn-beijing.aliyuncs.com/yupu/wk-scanpay-gin:latest
    Image ID:       docker-pullable://registry-vpc.cn-beijing.aliyuncs.com/yupu/wk-scanpay-gin@sha256:69ff961ad7fc72e514b8a6d3f16758d55fc54feaef051b0a13518387c4862499
    Port:           9070/TCP
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Error
      Exit Code:    1
      Started:      Tue, 03 Apr 2018 09:13:14 +0800
      Finished:     Tue, 03 Apr 2018 09:14:15 +0800
    Ready:          False
    Restart Count:  6
    Environment:    <none>
    Mounts:
      /WK/Scanpay from nas1 (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-6bsx7 (ro)
Conditions:
  Type           Status
  Initialized    True 
  Ready          False 
  PodScheduled   True 
Volumes:
  nas1:
    Type:    FlexVolume (a generic volume resource that is provisioned/attached using an exec based plugin)
    Driver:      Options:  %v

    FSType:     alicloud/nas
    SecretRef:  
    ReadOnly:   <nil>
%!(EXTRA bool=false, map[string]string=map[path:/k8s server:jiufu-0givfdak-atu48.cn-beijing.nas.aliyuncs.com vers:4.0 mode:755])  default-token-6bsx7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-6bsx7
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.alpha.kubernetes.io/notReady:NoExecute for 300s
                 node.alpha.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason                 Age                From                                        Message
  ----     ------                 ----               ----                                        -------
  Normal   Scheduled              14m                default-scheduler                           Successfully assigned wk-scanpay-gin-wk-scanpay-gin-657f8cc6b5-8t4nm to cn-beijing.i-2zecnfjkjgujdq5zacmn
  Normal   SuccessfulMountVolume  14m                kubelet, cn-beijing.i-2zecnfjkjgujdq5zacmn  MountVolume.SetUp succeeded for volume "default-token-6bsx7"
  Normal   SuccessfulMountVolume  14m                kubelet, cn-beijing.i-2zecnfjkjgujdq5zacmn  MountVolume.SetUp succeeded for volume "nas1"
  Normal   Pulling                10m (x4 over 14m)  kubelet, cn-beijing.i-2zecnfjkjgujdq5zacmn  pulling image "registry-vpc.cn-beijing.aliyuncs.com/yupu/wk-scanpay-gin:latest"
  Normal   Pulled                 10m (x4 over 14m)  kubelet, cn-beijing.i-2zecnfjkjgujdq5zacmn  Successfully pulled image "registry-vpc.cn-beijing.aliyuncs.com/yupu/wk-scanpay-gin:latest"
  Normal   Created                10m (x4 over 14m)  kubelet, cn-beijing.i-2zecnfjkjgujdq5zacmn  Created container
  Normal   Started                10m (x4 over 14m)  kubelet, cn-beijing.i-2zecnfjkjgujdq5zacmn  Started container
  Warning  FailedSync             9m (x5 over 12m)   kubelet, cn-beijing.i-2zecnfjkjgujdq5zacmn  Error syncing pod
  Warning  BackOff                4m (x18 over 12m)  kubelet, cn-beijing.i-2zecnfjkjgujdq5zacmn  Back-off restarting failed container

~~~




