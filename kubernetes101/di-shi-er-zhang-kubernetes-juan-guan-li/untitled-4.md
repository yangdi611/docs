# 卷类型

安装在Pod内的目录由基础卷类型支持。卷类型决定目录的属性，例如大小，内容，默认访问模式等。卷类型的一些示例包括：

* **emptyDir** 一旦在工作程序节点上调度了Pod，就会为该Pod创建一个空的Volume。该卷的使用寿命与Pod紧密相连。如果Pod终止，则emptyDir的内容将被永久删除。
* **hostPath** 使用hostPath Volume Type，我们可以共享主机到Pod的目录。如果Pod终止，则卷的内容在主机上仍然可用。
* **gcePersistentDisk** 使用gcePersistentDisk卷类型，我们可以将[Google Compute Engine（GCE）](https://cloud.google.com/compute/docs/disks/)永久磁盘安装到Pod中。
* **awsElasticBlockStore** 使用awsElasticBlockStore卷类型，我们可以将[AWS EBS卷](https://aws.amazon.com/ebs/)安装到Pod中。
* **azureDisk** 使用azureDisk，我们可以将[Microsoft Azure Data Disk](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/managed-disks-overview)安装到Pod中。
* **azureFile** 使用azureFile，我们可以将[Microsoft Azure File Volume](https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md)安装到Pod中。
* **cephfs** 使用cephfs，可以将现有的CephFS卷挂载到Pod中。当Pod终止时，将卸载该卷并保留该卷的内容。
* **nfs** 使用[nfs](https://en.wikipedia.org/wiki/Network_File_System)，我们可以将NFS共享安装到Pod中。
* **iscsi** 使用[iscsi](https://en.wikipedia.org/wiki/ISCSI)，我们可以将iSCSI共享安装到Pod中。
* **secret** 使用secret的卷类型，我们可以将敏感信息（例如密码）传递给Pods。我们将在下一章中看一个例子。
* **configMap** 使用configMap对象，我们可以将配置数据或shell命令和参数提供到Pod中。
* **persistentVolumeClaim** 我们可以使用persistentVolumeClaim将[PersistentVolume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)附加到Pod。我们将在下一部分中对此进行介绍。

您可以在[Kubernetes文档](https://kubernetes.io/docs/concepts/storage/volumes/)中了解有关卷类型的更多详细信息。

