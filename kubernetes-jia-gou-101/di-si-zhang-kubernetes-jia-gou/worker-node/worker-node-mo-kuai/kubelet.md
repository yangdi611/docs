# kubelet

kubelet是在每个节点上运行的代理，并与主节点上的控制平面组件进行通信。它主要从API服务器接收Pod定义，并与节点上的容器运行时进行交互以运行与Pod关联的容器。它还监视Pod正在运行的容器的运行状况。

kubelet使用  [**Container Runtime Interface**](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md) **\(CRI\)**连接到容器运行时。CRI由协议缓冲区，gRPC API和库组成。

![container runtime interface](../../../../.gitbook/assets/image%20%285%29.png)

如上所示，充当grpc客户端的kubelet连接到充当grpc服务器的CRI垫片以执行容器和镜像操作。CRI实现了两项服务：ImageService和RuntimeService。ImageService负责所有与镜像相关的操作，而RuntimeService负责与Pod和容器相关的所有操作。

容器运行时曾经在Kubernetes中进行过硬编码，但是随着CRI的发展，Kubernetes现在变得更加灵活，可以使用不同的容器运行时而无需重新编译。Kubernetes可以使用任何实现CRI的容器运行时来管理Pod，容器和容器映像。

