# kubelet - CRI shims

* dockershim

  使用dockershim，可以使用安装在工作程序节点上的Docker创建容器。在内部，Docker使用容器来创建和管理容器。

![dockershim](../../../../.gitbook/assets/image%20%2822%29.png)

* cri-containerd

  使用cri-containerd，我们可以直接使用Docker的较小后代containerd来创建和管理容器。

![cri-containerd](../../../../.gitbook/assets/image%20%2825%29.png)

* CRI-O

  CRI-O可以与Kubernetes一起使用任何Open Container Initiative（OCI）兼容的运行时。在创建本课程时，CRI-O支持runC和Clear Containers作为容器运行时。但是，原则上，可以插入任何符合OCI的运行时。

![CRI-O](../../../../.gitbook/assets/image%20%286%29.png)

