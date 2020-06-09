---
layout:     post
title:      "macOS 安装Docker"
subtitle:   "macOS 安装Docker"
date:       2019-05-29T02:53:47+08:00
author:     "Ely Xiao"
header-img: "img/common.jpg"
catalog: true
tags:
    - docker
---

## QA
#### 1. Kubernetes is Starting...
##### 解决方法1:
```
rm -rf ~/.kube
open global vpn, fq,
then reset Kubernetes cluster
Waiting 2-5 minutes
```

##### 解决方法2:
###### 1. `Preferences` -> `Reset` -> `Reset to factory defaults`
###### 2. Daemon
https://registry.docker-cn.com
![](/img/in-post/post-docker/15591117804985.jpg)

###### 3. Kubernetes
勾选以下3个
![](/img/in-post/post-docker/15591116616667.jpg)
 

###### 4. 安装依赖镜像
```
git clone https://github.com/maguowei/k8s-docker-for-mac.git
cd k8s-docker-for-mac/
./load_img.sh
```

### 集成kubernetes-dashboard
```
# 安装
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml

# 启动
kubectl proxy

# 修改配置
kubectl -n kube-system edit service kubernetes-dashboard
```

### 查看日志
打开 `Console.app` 搜索 docker

### 参考
* [Logs and troubleshooting](https://docs.docker.com/docker-for-mac/troubleshoot/#check-the-logs) 
* ["Kubernetes is starting" indefinitely on macOS Mojave](https://github.com/docker/for-mac/issues/3327)

