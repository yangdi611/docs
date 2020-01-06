# 节点之间Pod到Pod之间的通信

在Kubernetes集群中，Pod被随机安排在节点上。无论主机节点如何，Pods都希望能够与群集中的所有其他Pod通信，而这一切都无需实现网络地址转换（NAT）。这是Kubernetes中任何网络实施的基本要求。

Kubernetes网络模型旨在降低复杂性，并将Pod视为网络上的VM，其中每个VM都接收IP地址，因此每个Pod都接收IP地址。该模型称为“每个IP的IP”，可确保Pod到Pod的通信，就像VM能够相互通信一样。

我们不要忘记容器。它们共享Pod的网络名称空间，并且必须像在VM上的应用程序一样协调Pod内部的端口分配，同时还必须能够在Pod内部的localhost上相互通信。但是，通过使用[CNI插件](https://github.com/containernetworking/cni#3rd-party-plugins)支持的[Container Network Interface](https://github.com/containernetworking/cni)（CNI），容器与整个Kubernetes网络模型集成在一起。CNI是一组规范和库，允许插件为容器配置网络。尽管有一些[核心插件](https://github.com/containernetworking/plugins#plugins)，但是大多数CNI插件是实施Kubernetes网络模型的第三方软件定义网络（SDN）解决方案。除了满足网络模型的基本要求之外，某些网络解决方案还提供对网络策略的支持。[Flannel](https://github.com/coreos/flannel/)，[Weave](https://www.weave.works/oss/net/)和[Calico](https://www.projectcalico.org/)是少数可用于Kubernetes集群的SDN解决方案。

![Container Network Interface\(CNI\)](../../../.gitbook/assets/image%20%2810%29.png)

容器运行时将IP分配卸载给CNI，该CNI连接到基础配置的插件（例如Bridge或MACvlan）以获取IP地址。一旦相应插件提供了IP地址，CNI便会将其转发回请求的容器运行时。

