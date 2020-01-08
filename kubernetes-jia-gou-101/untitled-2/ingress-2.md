# Ingress 2

使用Ingress，用户不会直接连接到服务。用户到达Ingress端点，然后从那里将请求转发到所需的服务。您可以在下面看到一个示例Ingress定义的示例：

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: virtual-host-ingress
  namespace: default
spec:
  rules:
  - host: blue.example.com
    http:
      paths:
      - backend:
          serviceName: webserver-blue-svc
          servicePort: 80
  - host: green.example.com
    http:
      paths:
      - backend:
          serviceName: webserver-green-svc
          servicePort: 80
```

在上面的示例中，对blue.example.com和green.example.com的用户请求将转到同一个Ingress端点，然后从那里将它们转发到webserver-blue-svc和webserver-green-svc， 分别。这是基于名称的虚拟主机Ingress规则的示例。

当对example.com/blue和example.com/green的请求分别转发到webserver-blue-svc和webserver-green-svc时，我们也可以有Fanout Ingress规则：

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: fan-out-ingress
  namespace: default
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /blue
        backend:
          serviceName: webserver-blue-svc
          servicePort: 80
      - path: /green
        backend:
          serviceName: webserver-green-svc
          servicePort: 80
```

![Ingress URL Mapping](../../.gitbook/assets/image%20%2838%29.png)

Ingress资源本身不执行任何请求转发，它仅接受流量路由规则的定义。Ingress由Ingress controller实现，我们将在下面讨论。

