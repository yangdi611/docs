# Pod到外部世界的通信

对于在Kubernetes集群内的Pods中成功部署的容器化应用程序而言，它需要外界的可访问性。Kubernetes通过服务，封装在群集节点上的网络规则定义的复杂结构实现了外部可访问性。通过使用kube-proxy将服务公开给外部世界，可以通过虚拟IP从群集外部访问应用程序。
