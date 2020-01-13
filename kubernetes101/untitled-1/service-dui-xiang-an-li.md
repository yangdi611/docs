# Service对象案例

以下是服务对象定义的示例：

```yaml
kind: Service
apiVersion: v1
metadata:
  name: frontend-svc
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
```

在此示例中，我们通过选择所有将Label key = app设置为value = frontend的Pod来创建frontend-svc服务。默认情况下，每个服务仅接收在群集内部可路由的IP地址，称为ClusterIP。在我们的示例中，我们分别将172.17.0.4和172.17.0.5作为ClusterIP分配给了我们的frontend-svc和db-svc服务。

![Accessing the Pods using Service Object](../../.gitbook/assets/image%20%283%29.png)

现在，用户/客户端通过其ClusterIP连接到服务，该服务将流量转发到与其连接的Pod之一。默认情况下，服务在选择Pod进行流量转发时会提供负载平衡。

当服务将流量转发到Pod时，我们可以选择Pod上接收流量的targetPort。在我们的示例中，frontend-svc服务在端口80上接收来自用户/客户端的请求，然后将这些请求转发到targetPort 5000上的附加Pod之一。如果未明确定义targetPort，则流量将转发到Pods在服务接收流量的端口上。

Pod的IP地址的逻辑集以及targetPort称为服务端点。在我们的示例中，frontend-svc服务具有3个端点：10.0.1.3:5000，10.0.1.4:5000和10.0.1.5:5000。端点由服务自动创建和管理，而不是由Kubernetes集群管理员创建和管理。

