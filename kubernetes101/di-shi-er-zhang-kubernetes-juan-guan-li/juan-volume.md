# 卷 Volume

众所周知，在Pod中运行的容器本质上是短暂的。如果容器崩溃，则将删除容器中存储的所有数据。但是，kubelet将以干净的状态重新启动它，这意味着它将不包含任何旧数据。

为了克服这个问题，Kubernetes使用[卷](https://kubernetes.io/docs/concepts/storage/volumes/)。卷本质上是由存储介质支持的目录。存储介质，内容和访问模式由卷类型决定。

![Volumes](https://github.com/yangdi611/docs/tree/9182ec0aacc614915b8202361f45684572b4ddf8/.gitbook/assets/image%20%2832%29.png)

在Kubernetes中，卷被附加到Pod，并且可以在该Pod的容器之间共享。该卷具有与Pod相同的寿命，并且其寿命超过了Pod的容器-这允许在容器重新启动后保留数据。

