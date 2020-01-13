# 暴露应用二

列出service：

```bash
$ kubectl get services
NAME          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes    ClusterIP   10.96.0.1      <none>        443/TCP        1d
web-service   NodePort    10.110.47.84   <none>        80:31074/TCP   22s
```

现在，我们的Web服务已创建，其ClusterIP为10.110.47.84。在“端口”部分，我们看到80：31074的映射，这意味着我们已在节点上保留了静态端口31074。如果我们连接到该端口上的节点，则我们的请求将被代理到端口80上的ClusterIP。

It is not necessary to create the Deployment first, and the Service after. They can be created in any order. A Service will find and connect Pods based on the Selector.

To get more details about the Service, we can use the **kubectl describe** command, as in the following example:

```bash
$ kubectl describe service web-service
Name:                     web-service
Namespace:                default
Labels:                   run=web-service
Annotations:              <none>
Selector:                 app=nginx
Type:                     NodePort
IP:                       10.110.47.84
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  31074/TCP
Endpoints:                172.17.0.4:80,172.17.0.5:80,172.17.0.6:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

 **web-service** uses **app=nginx** as a Selector to logically group our three Pods, which are listed as endpoints. When a request reaches our Service, it will be served by one of the Pods listed in the **Endpoints** section.

