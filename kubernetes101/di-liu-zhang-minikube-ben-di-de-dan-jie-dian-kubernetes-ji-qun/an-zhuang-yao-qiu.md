# 安装要求

Minikube已安装并直接在本地Linux，macOS或Windows工作站上运行。但是，为了充分利用Minikube提供的所有功能，应在本地工作站上安装[Type-2 hypervisor](https://en.wikipedia.org/wiki/Hypervisor)管理程序，以与Minikube一起运行。这并不意味着我们需要使用此Hypervisor创建带有来宾操作系统的任何VM。

只要在我们的工作站上安装了Type-2 hypervisor管理程序，Minikube就会构建其所有基础结构。Minikube调用Hypervisor创建单个VM，然后该VM托管一个单节点Kubernetes集群。因此，我们需要确保拥有Minikube所需的必要硬件和软件来构建其环境。下面我们概述了在本地工作站上运行Minikube的要求：

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) **kubectl**是用于访问和管理任何Kubernetes集群的二进制文件。它与Minikube分开安装。由于我们将在Minikube安装之后安装kubectl，因此我们可能会在Minikube初始化期间看到警告-暂时可以忽略，但请记住，我们将必须安装kubectl才能管理Kubernetes集群。我们将在以后的章节中更详细地探讨kubectl。
  * Type-2 Hypervisor
  * On Linux [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [KVM](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#kvm2-driver)
  * On macOS [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [HyperKit](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#hyperkit-driver), or [VMware Fusion](http://www.vmware.com/products/fusion.html)
  * On Windows [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [Hyper-V](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md#hyperv-driver)  **NOTE:** Minikube支持**--vm-driver = none**选项，该选项直接在主机OS上而不是在VM内部运行Kubernetes组件。使用此选项时，需要安装Docker并在本地工作站上安装Linux操作系统，但无需安装管理程序。如果使用**--vm-driver = none**，请确保为Docker指定一个桥接网络。否则，它可能会在网络重新启动之间发生变化，从而导致与群集的连接丢失。
* 必须在BIOS的本地工作站上启用VT-x / AMD-v虚拟化
* Minikube首次运行时的Internet连接-下载初始化Minikube Kubernetes集群所需的软件包，依赖项，更新和拉取镜像。仅当需要从容器存储库中提取新的Docker镜像或已部署的容器化应用程序需要它时，后续运行才需要Internet连接。提取镜像后，无需连接互联网即可重复使用。

在本章中，我们将VirtualBox用作所有三个操作系统（Linux，macOS和Windows）上的虚拟机管理程序，以允许Minikube设置承载单节点Kubernetes集群的VM。

从正式的[Kubernetes文档](https://kubernetes.io/docs/setup/minikube/)或[GitHub](https://github.com/kubernetes/minikube)中了解有关Minikube的更多信息。

