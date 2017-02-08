+++
categories = ["开发"]
tags = ["Cpp","Golang","Python"]
date = "2017-02-08T14:46:28+08:00"
title = "kubernetes安装简明教程"

+++

1. kubernetes通过 `kubeadm` 方式安装需要以下四个包

+ kubelet
+ kubernetes-cni
+ kubeadm
+ kubectl

这四个包可以通过 `https://github.com/kubernetes/kubernetes` 仓库下载后执行 `make` 获取安装包，如果需要rpm包需要下载 release 版本。

2. 安装 kubernetes 环境
    1. 安装管理节点
    > kubeadmin init  --api-advertise-addresses [管理节点IP]

    这一步会生成一个token在工作节点加入的时候用。

    kubernetes所有组件以 `POD` 运行在kubernetes中，下面这些镜像是在安装过程中在安装的需要提前准备好，具体版本号会根据kubernetes版本不同而不同，注意匹配，否则可能会有安装失败。

    ```
     # 所有节点都会安装
    gcr.io/google_containers/pause-amd64
    gcr.io/google_containers/kube-proxy-amd64

     # 管理节点安装
    gcr.io/google_containers/etcd-amd64
    gcr.io/google_containers/kube-apiserver-amd64
    gcr.io/google_containers/kube-scheduler-amd64
    gcr.io/google_containers/kube-discovery-amd64
    gcr.io/google_containers/kube-controller-manager-amd64
    gcr.io/google_containers/exechealthz-amd64

     # 网络插件， 所有节点都会安装
    weaveworks/weave-kube
    weaveworks/weave-npc

     # dns 插件，管理节点安装
    gcr.io/google_containers/kubedns-amd64
    gcr.io/google_containers/kube-dnsmasq-amd64
    gcr.io/google_containers/dnsmasq-metrics-amd64


     # web界面，管理节点安装
    gcr.io/google_containers/kubernetes-dashboard-amd64

    ```

    启动 `kubelet` 服务

    > systemctl start kubelet

    2. 安装工作节点

    > kubeadm join --token=[上一步生成的token] [管理节点IP]

    3. 安装 POD 网络
    POD网络有很多实现，可以选择有flannel，weave等，下载weave的 `yaml` 直接在 kubernetes 中运行

    > kubectl apply -f weave-daemonset.yaml

    4. 运行dashborad
    方法同POD网络

    > kubectl apply -f weave-daemonset.yaml

3. 其他组件模块
   所有模块在kubernetes中是运行在POD中。

   1. Ingress 模块实现需要依赖 `traefik` 的 `Daemon Set` ；
   2. dashboard模块需要 `kubernetes-dashboard-amd64`;
   3. 监控模块需要运行 `heapster`， `influxdb`， `grafana` 三个 `service`；


