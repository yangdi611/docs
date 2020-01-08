# Ingress Controller

[Ingress Controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)是一种应用程序，可监视主节点的API服务器中入口资源的变化并相应地更新第7层负载平衡器。Kubernetes支持不同的Ingress Controller，如果需要，我们也可以构建自己的Ingress Controller。[GCE L7 Load Balancer Controller](https://github.com/kubernetes/ingress-gce/blob/master/README.md)和[Nginx Ingress Controller](https://github.com/kubernetes/ingress-gce/blob/master/README.md)是常用的Ingress Controller。其他控制器是Istio，Kong，Traefik等。

#### 使用Minikube启动Ingress Controller

Minikube随附Nginx Ingress Controller安装程序作为插件，默认情况下处于禁用状态。通过运行以下命令可以轻松启用它：

```yaml
$ minikube addons enable ingress
```



