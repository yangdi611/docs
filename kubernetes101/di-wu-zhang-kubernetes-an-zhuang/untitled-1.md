# Kubernetes的安装工具和资源

在讨论安装配置和底层基础结构时，让我们看一下一些可用的有用工具/资源：

* **kubeadm** [kubeadm](https://github.com/kubernetes/kubeadm) 是Kubernetes生态系统的一等公民。这是引导单节点或多节点Kubernetes集群的安全且推荐的方法。它具有一组用于设置集群的构建块，但是可以轻松扩展以添加更多功能。请注意，kubeadm不支持主机配置。
* **kubespray** 使用[kubespray](https://kubernetes.io/docs/setup/custom-cloud/kubespray/)（以前称为kargo），我们可以在AWS，GCE，Azure，OpenStack或裸机上安装高可用性Kubernetes集群。Kubespray基于Ansible，并且在大多数Linux发行版中都可用。这是一个[Kubernetes孵化器](https://github.com/kubernetes-sigs/kubespray)项目。
* **kops** 使用[kops](https://kubernetes.io/docs/setup/custom-cloud/kops/)，我们可以从命令行创建，销毁，升级和维护生产级，高可用性的Kubernetes集群。它也可以配置机器。目前，正式支持AWS。计划在将来对GCE进行支持，并在alpha阶段对VMware vSphere进行支持，并且计划在将来提供其他平台。探索[kops项目](https://github.com/kubernetes/kops)以获取更多详细信息。
* **kube-aws** 借助[kube-aws](https://github.com/kubernetes-incubator/kube-aws)，我们可以从命令行在AWS上创建，升级和销毁Kubernetes集群。Kube-aws也是Kubernetes孵化器项目。

如果现有的解决方案和工具不符合我们的要求，那么我们[可以从头开始安装](https://v1-12.docs.kubernetes.io/docs/setup/release/building-from-source/)Kubernetes（尽管Kubernetes v1.12中的日期链接仍然是有效的解决方案）。

值得一试的是Kelsey Hightower撰写的[Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way) GitHub项目，该项目共享了引导Kubernetes集群所涉及的手动步骤。

