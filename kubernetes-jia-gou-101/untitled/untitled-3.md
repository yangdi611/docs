# 使用CLI部署应用二

#### 创建具有部署详细信息的YAML配置文件

让我们创建具有以下内容的webserver.yaml文件：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```

使用kubectl，我们将从YAML配置文件创建Deployment。结合使用-f选项和kubectl create命令，我们可以将YAML文件作为对象的规范传递，或者将URL从Web传递到配置文件。在以下示例中，我们将创建一个Web服务器部署：

```bash
$ kubectl create -f webserver.yaml
deployment.apps/webserver created
```

这还将创建一个副本集和Pod，如YAML配置文件中所定义。

```bash
$  kubectl get replicasets
NAME                  DESIRED   CURRENT   READY     AGE
webserver-b477df957   3         3         3         45s

$ kubectl get pods
NAME                        READY     STATUS    RESTARTS   AGE
webserver-b477df957-7lnw6   1/1       Running   0          2m
webserver-b477df957-j69q2   1/1       Running   0          2m
webserver-b477df957-xvdkf   1/1       Running   0          2m
```

