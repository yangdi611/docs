# kube-proxy

所有工作节点都运行一个名为[kube-proxy](https://kubernetes.io/docs/concepts/services-networking/service/#virtual-ips-and-service-proxies)的守护进程，该守护进程监视主节点上的API服务器以了解服务和端点的添加和删除。在以下示例中，对于每个新服务，在每个节点上，kube-proxy配置iptables规则以捕获其ClusterIP的流量并将其转发到服务的一个端点。因此，任何节点都可以接收外部流量，然后根据iptables规则在群集内部进行内部路由。删除服务后，kube-proxy也会在所有节点上删除相应的iptables规则。

![kube-proxy, Services, and Endpoints](https://github.com/yangdi611/docs/tree/9182ec0aacc614915b8202361f45684572b4ddf8/.gitbook/assets/image%20%2839%29.png)

