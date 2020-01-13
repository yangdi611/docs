# kubectl

通常在安装Minikube之前先安装kubectl，但我们也可以在之后安装。安装后，kubectl会自动接收其配置以用于Minikube Kubernetes集群访问。但是，在其他Kubernetes集群设置中，我们可能需要配置kubectl访问集群所需的集群访问点和证书。

[Kubernetes文档](https://kubernetes.io/docs/tasks/tools/install-kubectl/)中提到了可用于安装kubectl的不同方法。为了获得最佳结果，建议将kubectl与Minikube运行的Kubernetes保持相同的版本-在编写本课程时，最新的稳定版本为v1.14.1。接下来，我们将看一些在Linux，macOS和Windows系统上安装它的步骤。

