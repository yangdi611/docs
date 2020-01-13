# 访问应用

我们的应用运行在minikube的虚拟节点上，要访问我们的应用，需要先得到minikube虚拟机的IP地址：

```bash
$ minikube ip
192.168.99.100
```

现在打开浏览器然后访问 192.168.99.100，端口31074。

![&#x901A;&#x8FC7;&#x6D4F;&#x89C8;&#x5668;&#x8BBF;&#x95EE;&#x5E94;&#x7528;](../../.gitbook/assets/image%20%284%29.png)

我们还可以运行以下minikube命令，该命令在浏览器中显示该应用程序：

```bash
$ minikube service web-service
Opening kubernetes service default/web-service in default browser...
```

我们可以看到Nginx欢迎页面，该页面由运行在所创建Pod中的Web服务器应用程序显示。由于服务在其端点（endpoint）之前充当负载平衡器（Load Balancer），因此可以由服务按逻辑分组的三个端点之一来满足我们的请求。

