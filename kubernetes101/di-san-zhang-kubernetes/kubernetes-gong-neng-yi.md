# Kubernetes功能一

Kubernetes为容器编排提供了非常丰富的功能。其完全支持的功能包括：

* 自动垃圾箱包装
  * Kubernetes会根据资源需求和约束条件自动调度容器，以在不牺牲可用性的情况下最大化利用率。
* 自我修复
  * Kubernetes会自动替换发生故障的节点并重新安排容器的时间。根据现有规则/策略，它将杀死并重新启动对运行状况检查无响应的容器。它还可以防止流量路由到无响应的容器。
* 服务发现和负载平衡
  * 容器从Kubernetes接收自己的IP地址，为一个容器集分配一个域名系统（DNS）名称，以帮助该容器集内的各个容器进行负载平衡请求。


