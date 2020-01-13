# Authorization II

## 授权二

授权模块（第1部分）：

* **Node Authorizer**

  节点授权是一种专用授权模式，专门授权kubelet发出的API请求。它授权kubelet对服务，端点，节点等进行读取操作，并对节点，pod，事件等进行写入操作。有关更多详细信息，请查看[Kubernetes文档](https://kubernetes.io/docs/reference/access-authn-authz/node/)。

* **Attribute-Based Access Control \(ABAC\) Authorizer**  
  借助ABAC授权者，Kubernetes授予对API请求的访问权限，这些请求将策略与属性结合在一起。在下面的示例中，学生学生只能阅读命名空间lfs158中的Pod。

  ```yaml
  {
    "apiVersion": "abac.authorization.kubernetes.io/v1beta1",
    "kind": "Policy",
    "spec": {
      "user": "student",
      "namespace": "lfs158",
      "resource": "pods",
      "readonly": true
    }
  }
  ```

  要启用ABAC授权者，我们需要使用--authorization-mode = ABAC选项启动API服务器。我们还需要使用--authorization-policy-file = PolicyFile.json指定授权策略。有关更多详细信息，请查看Kubernetes文档。

* **Webhook Authorizer**

  借助Webhook授权器，Kubernetes可以为某些第三方服务提供授权决策，如果授权成功，则返回true；如果失败，则返回false。为了启用Webhook授权者，我们需要使用--authorization-webhook-config-file = SOME\_FILENAME选项启动API服务器，其中SOME\_FILENAME是远程授权服务的配置。有关更多详细信息，请参阅Kubernetes文档。

