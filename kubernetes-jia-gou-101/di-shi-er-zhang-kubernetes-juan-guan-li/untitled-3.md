# 持久卷

在典型的IT环境中，存储由存储/系统管理员管理。最终用户将仅收到使用存储的说明，但不参与底层存储管理。

在容器化的世界中，我们希望遵循类似的规则，但是鉴于我们之前看到的许多卷类型，它变得具有挑战性。Kubernetes使用PersistentVolume（PV）子系统解决了这个问题，该子系统为用户和管理员提供了API，以管理和使用持久性存储。要管理该卷，它使用PersistentVolume API资源类型，并使用PersistentVolumeClaim API资源类型来使用它。

持久卷卷是群集中网络连接的存储，由管理员提供。

![PersistentVolume](../../.gitbook/assets/image%20%2818%29.png)

可以基于StorageClass资源动态设置PersistentVolumes。StorageClass包含预定义的配置程序和参数以创建PersistentVolume。用户使用PersistentVolumeClaims发送动态创建PV的请求，该请求将连接到StorageClass资源。

一些支持使用PersistentVolume管理存储的卷类型为：

* GCEPersistentDisk
* AWSElasticBlockStore
* AzureFile
* AzureDisk
* CephFS
* NFS
* iSCSI.

