# APIs

众所周知，Kubernetes具有API服务器，并且操作员/用户从外部连接到该服务器以与集群进行交互。使用CLI和Web UI，我们可以连接到在主节点上运行的API服务器以执行不同的操作。只要我们可以访问主节点并具有正确的凭据，我们就可以使用其API端点直接连接到API服务器并向其发送命令。

在下面，我们可以看到Kubernetes的HTTP API空间的一部分：

![HTTP API Space of Kubernetes](../../.gitbook/assets/image%20%2811%29.png)

Kubernetes的HTTP API空间可以分为三个独立的组：

* Core Group\(/api/v1\)

  该组包括诸如Pod，Services，节点，名称空间，configmaps，secret等的对象。

* Named Group

  该组包括/ apis / $ NAME / $ VERSION格式的对象。这些不同的API版本意味着不同级别的稳定性和支持：

  _Alpha级别 - 可能会随时将其丢弃，恕不另行通知。例如，/ apis / batch / v2alpha1。_

  _Beta级别 - 经过充分测试，但是对象的语义可能会在随后的Beta或稳定版本中以不兼容的方式更改。例如，_ `/apis/certificates.k8s.io/v1beta1`

  _Stable级别 - 出现在许多后续版本的已发布软件中。例如，_`/apis/networking.k8s.io/v1`

* System-wide

  该组由系统范围的API端点组成，例如/ healthz，/ logs，/ metrics，/ ui等。

我们可以通过调用相应的API端点或通过CLI / Web UI直接连接到API服务器。

