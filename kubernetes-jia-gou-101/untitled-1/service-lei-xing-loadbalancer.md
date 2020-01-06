# service类型：LoadBalancer

使用LoadBalancer ServiceType：

* NodePort and ClusterIP are automatically created, and the external load balancer will route to them
* The Service is exposed at a static port on each worker node
* The Service is exposed externally using the underlying cloud provider's load balancer feature.

![LoadBalancer](../../.gitbook/assets/image%20%2830%29.png)

仅当基础基础架构支持自动创建负载均衡器并在Kubernetes中具有相应的支持时，LoadBalancer ServiceType才能工作，例如Google Cloud Platform和AWS。如果未配置任何此类功能，则不会填充LoadBalancer IP地址字段，并且该服务将以与NodePort类型的服务相同的方式工作。

