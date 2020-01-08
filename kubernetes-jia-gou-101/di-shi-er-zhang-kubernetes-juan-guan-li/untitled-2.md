# PersistentVolumeClaim（PVC）

PersistentVolumeClaim（PVC）是用户存储请求。用户根据类型，访问模式和大小请求PersistentVolume资源。共有三种访问模式：ReadWriteOnce（由单个节点读写），ReadOnlyMany（由多个节点只读）和ReadWriteMany（由多个节点读写）。一旦找到合适的PersistentVolume，它将绑定到PersistentVolumeClaim。

![PersistentVolumeClaim](../../.gitbook/assets/image%20%2835%29.png)

成功绑定后，可以在Pod中使用PersistentVolumeClaim资源。

![PersistentVolumeClaim Used In a Pod](../../.gitbook/assets/image%20%2831%29.png)

用户完成工作后，可以释放附加的PersistentVolumes。然后可以回收基础PersistentVolume（用于管理员验证和/或汇总数据），删除（删除数据和卷）或回收以供将来使用（仅删除数据）。

