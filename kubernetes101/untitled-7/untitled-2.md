# 配额管理

当有许多用户共享给定的Kubernetes集群时，总是存在公平使用的问题。用户不应利用不当优势。为了解决此问题，管理员可以使用ResourceQuota API资源，该资源提供了限制每个命名空间的合计资源消耗的约束。

我们可以为每个命名空间设置以下配额类型：

* **计算资源配额** 我们可以限制在给定命名空间中可以请求的计算资源（CPU，内存等）的总和。
* **存储资源配额** 我们可以限制可以请求的存储资源的总和（PersistentVolumeClaims，requests.storage等）。
* **对象数量配额** 我们可以限制给定类型（pod，ConfigMap，PersistentVolumeClaims，ReplicationControllers，Services，Secret等）的对象数量。

