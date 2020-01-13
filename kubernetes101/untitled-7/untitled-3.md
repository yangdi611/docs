# 自动缩放

尽管手动缩放几个Kubernetes对象相当容易，但是对于部署了成百上千个对象的生产就绪型集群来说，这可能不是一个实用的解决方案。我们需要一个动态扩展解决方案，该解决方案可以根据资源利用率，可用性和需求从群集中添加或删除对象。

可以通过控制器在Kubernetes集群中实现自动缩放，控制器可以根据单个，多个或自定义指标定期调整运行对象的数量。Kubernetes中提供了各种类型的自动缩放器，可以将其单独实现或组合使用以获得更强大的自动缩放解决方案：

* [Horizontal Pod Autoscaler \(HPA\)](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/#algorithm-details)  HPA是基于算法的控制器[API资源](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/autoscaling/horizontal-pod-autoscaler.md#horizontalpodautoscaler-object)，可根据CPU利用率自动调整ReplicaSet，Deployment或Replication Controller中的副本数。
* [Vertical Pod Autoscaler \(VPA\)](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/autoscaling/vertical-pod-autoscaler.md)  VPA自动在Pod中设置容器资源需求（CPU和内存），并根据历史利用率数据，当前资源可用性和实时事件在运行时动态调整它们。
* [Cluster Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)  当没有足够的资源可用于预期要计划的新Pod或群集中的节点利用率不足时，Cluster Autoscaler会自动调整Kubernetes群集的大小。

