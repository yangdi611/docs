# Ingress 1

对于服务，路由规则与给定服务相关联。只要服务存在，它们就存在，并且有很多规则，因为集群中有很多服务。如果我们能够以某种方式将路由规则与应用程序分离，并集中进行规则管理，那么我们就可以更新应用程序而不必担心其外部访问。可以使用Ingress资源完成此操作。

根据[Kubernetes.io](https://kubernetes.io/docs/concepts/services-networking/ingress/): “Ingress是允许入站连接到达群集服务的规则的集合。”

为了允许入站连接到达群集服务，Ingress为服务配置了第7层HTTP / HTTPS负载均衡器，并提供以下内容：

* TLS \(Transport Layer Security\)
* Name-based virtual hosting 
* Fanout routing
* Loadbalancing
* Custom rules.

![Ingress](../../.gitbook/assets/image%20%281%29.png)





