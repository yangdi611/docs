# TCP 活性探针

使用TCP Liveness Probe，kubelet尝试打开运行应用程序的容器的TCP socket。如果成功，则认为该应用程序运行状况良好，否则kubelet会将其标记为运行状况不佳，然后重新启动受影响的容器。

```yaml
livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 20
```

