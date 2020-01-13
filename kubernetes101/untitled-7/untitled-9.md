# 监控和记录

在Kubernetes中，我们必须按Pod，服务，节点等收集资源使用情况数据，以了解整体资源消耗情况并做出扩展给定应用程序的决策。两种流行的Kubernetes监视解决方案是Kubernetes Metrics Server和Prometheus。

* **Metrics Server**  [Metrics Server](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-metrics-pipeline/#metrics-server)是资源使用情况数据的群集范围内的聚合器-Kubernetes中的一项相对较新的功能。
* **Prometheus**[ _\*\*_](https://prometheus.io/)[Prometheus](https://prometheus.io/)现在是[CNCF](https://www.cncf.io/)（Cloud Native Computing Foundation）的一部分，也可以用于从不同的Kubernetes组件和对象中抓取资源使用情况。使用其客户端库，我们还可以检测应用程序的代码。

故障排除和调试的另一个重要方面是日志记录，其中我们从给定系统的不同组件收集日志。在Kubernetes中，我们可以从不同的集群组件，对象，节点等收集日志。但是，不幸的是，Kubernetes默认情况下不提供集群范围的日志记录，因此需要第三方工具来集中和聚集集群日志。收集日志的最常见方法是使用Elasticsearch，它使用流利的自定义配置作为节点上的代理。fluentd是一个开源数据收集器，它也是CNCF的一部分。

