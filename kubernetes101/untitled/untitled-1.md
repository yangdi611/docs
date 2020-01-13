# 暴露应用一

在上一章中，我们探讨了不同的ServiceType。使用ServiceTypes，我们可以定义服务的访问方法。对于NodePort ServiceType，Kubernetes在所有woker node上打开一个静态端口。如果我们从任何节点连接到该端口，则将代理到该服务的ClusterIP。接下来，让我们在创建服务时使用NodePort服务类型。

**创建具有以下内容的webserver-svc.yaml文件：**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
  labels:
    run: web-service
spec:
  type: NodePort
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: nginx 
```

**使用kubectl创建服务：**

```bash
$ kubectl create -f webserver-svc.yaml
service/web-service created
```

创建服务的一种更直接的方法是公开以前创建的Deployment（此方法需要现有的Deployment）。

**使用kubectl暴露命令公开部署：**

```bash
$ kubectl expose deployment webserver --name=web-service --type=NodePort
service/web-service exposed
```





