# Kubernetes配置

可以使用不同的配置来安装Kubernetes。以下简要介绍了四种主要安装类型：

* **All-in-One Single-Node Installation** 在此设置中，所有主组件和辅助组件均在单个节点上安装并运行。虽然它对于学习，开发和测试很有用，但不应在生产中使用。Minikube就是这样一个例子，我们将在以后的章节中进行探讨。
* **Single-Node etcd, Single-Master and Multi-Worker Installation** 在此设置中，我们有一个单主节点，该节点也运行一个单节点etcd实例。多个工作程序节点连接到主节点。
* **Single-Node etcd, Multi-Master and Multi-Worker Installation** 在此设置中，我们在HA模式下配置了多个主节点，但是我们有一个单节点etcd实例。多个工作程序节点连接到主节点。
* **Multi-Node etcd, Multi-Master and Multi-Worker Installation** 在此模式下，etcd被配置为集群HA模式，主节点都被配置为HA模式，并连接到多个工作节点。这是最先进和推荐的生产设置。

