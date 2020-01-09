# 配额管理

当有许多用户共享给定的Kubernetes集群时，总是存在公平使用的问题。用户不应利用不当优势。为了解决此问题，管理员可以使用ResourceQuota API资源，该资源提供了限制每个命名空间的合计资源消耗的约束。

我们可以为每个命名空间设置以下配额类型：

* **Compute Resource Quota** We can limit the total sum of compute resources \(CPU, memory, etc.\) that can be requested in a given Namespace.
* **Storage Resource Quota** We can limit the total sum of storage resources \(PersistentVolumeClaims, requests.storage, etc.\) that can be requested.
* **Object Count Quota** We can restrict the number of objects of a given type \(pods, ConfigMaps, PersistentVolumeClaims, ReplicationControllers, Services, Secrets, etc.\).



