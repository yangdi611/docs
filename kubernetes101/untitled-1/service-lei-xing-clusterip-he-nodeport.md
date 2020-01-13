# service类型：ClusterIP和NodePort

**ClusterIP**是默认的ServiceType。服务接收一个虚拟IP地址，称为其ClusterIP。该虚拟IP地址用于与服务进行通信，并且只能在群集中访问。

使用NodePort ServiceType，除了ClusterIP之外，还从所有工作节点将从默认范围30000-32767动态选择的高端口映射到相应的服务。例如，如果服务frontend-svc的映射NodePort是32233，则如果我们连接到端口32233上的任何工作节点，则该节点会将所有流量重定向到分配的ClusterIP-172.17.0.4。如果我们希望使用特定的高端口号，则可以从默认范围将该高端口号分配给NodePort。

![NodePort](https://github.com/yangdi611/docs/tree/9182ec0aacc614915b8202361f45684572b4ddf8/.gitbook/assets/image%20%2824%29.png)

当我们想使我们的服务可以从外部访问时，NodePort ServiceType很有用。最终用户连接到指定高端口上的任何工作节点，该节点在内部将请求代理到服务的ClusterIP，然后将请求转发到在集群内部运行的应用程序。要从外部访问多个应用程序，管理员可以配置反向代理-ingress，并定义针对群集内服务的规则。

