# StatefulSets

[StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)控制器用于需要唯一身份的有状态应用程序，例如名称，网络标识，严格排序等。例如，MySQL群集，etcd群集。

StatefulSet控制器提供身份，并保证对部署和扩展到Pod的顺序。与部署类似，StatefulSet使用ReplicaSet作为中介Pod控制器，并支持滚动更新和回滚。

