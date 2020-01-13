# Container runtime容器运行时

尽管Kubernetes被描述为“容器编排引擎”，但它不具有直接处理容器的能力。为了运行和管理容器的生命周期，Kubernetes要求在要调度Pod及其容器的节点上进行容器运行。Kubernetes支持许多容器运行时：

* [Docker](https://www.docker.com/) - 尽管使用容器化的容器平台作为容器运行时，但它是Kubernetes中使用最广泛的容器运行时
* [CRI-O](https://cri-o.io/) - Kubernetes的轻量级容器运行时，它还支持Docker映像注册表
* [containerd ](https://containerd.io/)- 一个简单且可移植的容器运行时，提供强大的功能
* [rkt](https://github.com/rkt/rkt) - Pod原生容器引擎，它还运行Docker映像
* [rktlet](https://github.com/kubernetes-incubator/rktlet) - 使用rkt的Kubernetes [Container Runtime Interface](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md) \(CRI\) 实现。  

