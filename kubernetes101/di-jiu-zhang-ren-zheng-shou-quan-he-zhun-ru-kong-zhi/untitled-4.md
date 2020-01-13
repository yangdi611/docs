# Admission Control

准入控制用于指定精细的访问控制策略，包括允许特权容器，检查资源配额等。我们使用不同的准入控制器（例如ResourceQuota，DefaultStorageClass，AlwaysPullImages等）强制执行这些策略。它们仅在API请求之后才生效经过认证和授权。

要使用许可控制，我们必须使用--enable-admission-plugins启动Kubernetes API服务器，该服务器采用逗号分隔的控制器名称的有序列表：

```bash
--enable-admission-plugins=NamespaceLifecycle,ResourceQuota,PodSecurityPolicy,DefaultStorageClass
```

Kubernetes默认情况下启用了一些访问控制器。有关更多详细信息，请查阅Kubernetes文档。

