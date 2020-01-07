# 就绪探针

有时，应用程序必须满足某些条件才能为流量提供服务。这些条件包括确保依赖服务准备就绪，或者确认需要加载大型数据集等。在这种情况下，我们使用就绪探针并等待特定条件发生。只有这样，应用程序才能为流量提供服务。 

带有未报告就绪状态的容器的Pod不会从Kubernetes Services接收流量。

```yaml
readinessProbe:
  exec:
    command:
    - cat
    - /tmp/healthy
  initialDelaySeconds: 5
  periodSeconds: 5
```

准备就绪探针的配置与活性探针类似。它们的配置也保持不变。

请查看[Kubernetes文档](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)以获取更多详细信息。

