# Kubernetes联邦

使用[Kubernetes Cluster Federation](https://kubernetes.io/docs/concepts/cluster-administration/federation/)，我们可以从单个控制平面管理多个Kubernetes集群。我们可以跨联合集群同步资源，并进行跨集群发现。这使我们能够跨区域执行部署，使用全局DNS记录访问它们并实现高可用性。

尽管联合身份验证仍然是Alpha的功能，但在我们要构建混合解决方案时非常有用，在该解决方案中，我们可以在私有数据中心内运行一个集群，而在公共云中运行另一个集群，从而避免提供商锁定。我们还可以为联盟中的每个群集分配权重，以根据自定义规则分配负载。

