# HTTP活性请求

在下面的示例中，kubelet在端口8080上将HTTP GET请求发送到应用程序的/healthz端点。否则，它将认为该应用程序仍然有效。

```yaml
livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
        httpHeaders:
        - name: X-Custom-Header
          value: Awesome
      initialDelaySeconds: 3
      periodSeconds: 3
```

