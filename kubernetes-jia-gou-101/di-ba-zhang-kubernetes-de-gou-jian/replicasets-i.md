# ReplicaSets I

[ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)是下一代ReplicationController。ReplicaSets支持基于相等的选择器和基于集合的选择器，而ReplicationControllers仅支持基于相等的选择器。当前，这是唯一的区别。

借助ReplicaSet，我们可以缩放运行特定容器应用程序映像的Pod的数量。缩放可以手动完成，也可以通过使用自动缩放器完成。



![ReplicaSet\(&#x73B0;&#x5728;&#x7684;&#x72B6;&#x6001;&#x548C;&#x9700;&#x6C42;&#x7684;&#x72B6;&#x6001;&#x4E00;&#x81F4;\)](../../.gitbook/assets/image%20%281%29.png)

