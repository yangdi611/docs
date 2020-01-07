# 卷类型

安装在Pod内的目录由基础卷类型支持。卷类型决定目录的属性，例如大小，内容，默认访问模式等。卷类型的一些示例包括：

* **emptyDir** 一旦在工作程序节点上调度了Pod，就会为该Pod创建一个空的Volume。该卷的使用寿命与Pod紧密相连。如果Pod终止，则emptyDir的内容将被永久删除。
* **hostPath** 使用hostPath Volume Type，我们可以共享主机到Pod的目录。如果Pod终止，则卷的内容在主机上仍然可用。
* **gcePersistentDisk** 使用gcePersistentDisk卷类型，我们可以将[Google Compute Engine（GCE）](https://cloud.google.com/compute/docs/disks/)永久磁盘安装到Pod中。
* **awsElasticBlockStore** 使用awsElasticBlockStore卷类型，我们可以将[AWS EBS卷](https://aws.amazon.com/ebs/)安装到Pod中。
* **azureDisk** 使用azureDisk，我们可以将[Microsoft Azure Data Disk](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/managed-disks-overview)安装到Pod中。
* **azureFile** With **azureFile** we can mount a [Microsoft Azure File Volume](https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md) into a Pod.
* **cephfs** With **cephfs**, an existing CephFS volume can be mounted into a Pod. When a Pod terminates, the volume is unmounted and the contents of the volume are preserved.
* **nfs** With [nfs](https://en.wikipedia.org/wiki/Network_File_System), we can mount an NFS share into a Pod.
* **iscsi** With [iscsi](https://en.wikipedia.org/wiki/ISCSI), we can mount an iSCSI share into a Pod.
* **secret** With the **secret** Volume Type, we can pass sensitive information, such as passwords, to Pods. We will take a look at an example in a later chapter.
* **configMap** With **configMap** objects, we can provide configuration data, or shell commands and arguments into a Pod.
* **persistentVolumeClaim** We can attach a [PersistentVolume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) to a Pod using a **persistentVolumeClaim**. We will cover this in our next section. 

