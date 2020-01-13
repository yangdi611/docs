# 部署Ingress资源

部署Ingress Controller后，我们可以使用kubectl create命令创建一个Ingress资源。例如，如果我们使用在Ingress 2部分中看到的基于名称的虚拟主机Ingress规则定义创建了virtual-host-ingress.yaml文件，则可以使用以下命令创建Ingress资源：

```bash
$ kubectl create -f virtual-host-ingress.yaml
```

