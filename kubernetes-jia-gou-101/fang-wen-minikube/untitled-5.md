# kubectl proxy

运行`kubectl proxy`命令，使`kubectl`在主节点上的API服务器上进行身份验证，并使仪表板在与之前版本稍有不同的URL上可用，这次是通过代理端口8001。

 第一，我们看一下 `kubectl proxy`命令: 

```bash
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

只要proxy正在运行，它就会锁定终端。通过proxy运行，我们可以通过新URL访问仪表板（只需在下面单击它-它就可以在您的工作站上运行）。一旦我们停止代理（使用CTRL + C），仪表板将不再可用。

```bash
http://127.0.0.1:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard:/proxy/#!/overview?namespace=default 
```

![Kubernetes Dashboard over the proxy](../../.gitbook/assets/image%20%2812%29.png)

