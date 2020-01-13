# Namespaces

如果多个用户和团队使用同一个Kubernetes集群，我们可以使用[命名空间](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)将该集群划分为虚拟子集群。在名称空间内创建的资源/对象的名称是唯一的，但在群集中的整个名称空间中不是唯一的。

要列出所有命名空间，我们可以运行以下命令：

```bash
$ kubectl get namespaces
NAME              STATUS       AGE
default           Active       11h
kube-node-lease   Active       11h
kube-public       Active       11h
kube-system       Active       11h
```

通常，Kubernetes创建四个默认命名空间：kube-system，kube-public，kube-node-lease和default。kube系统命名空间包含由Kubernetes系统创建的对象，主要是控制平面代理。默认名称空间包含由管理员和开发人员创建的对象和资源。默认情况下，我们连接到默认命名空间。kube-public是一个特殊的命名空间，它不受任何人的保护和读取，用于特殊目的，例如公开有关集群的公共（非敏感）信息。最新的命名空间是kube-node-lease，它保存用于节点心跳数据的节点租用对象。但是，好的做法是创建更多的命名空间，以为用户和开发人员团队虚拟化群集。

使用[资源配额](https://kubernetes.io/docs/concepts/policy/resource-quotas/)，我们可以在命名空间中划分群集资源。我们将在以后的章节中简要介绍资源配额。

