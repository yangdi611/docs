# Authorization II

## 授权二

授权模块（第1部分）：

* **Node Authorizer** Node authorization is a special-purpose authorization mode which specifically authorizes API requests made by kubelets. It authorizes the kubelet's read operations for services, endpoints, nodes, etc., and writes operations for nodes, pods, events, etc. For more details, please review the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/node/).
* **Attribute-Based Access Control \(ABAC\) Authorizer**  
  With the ABAC authorizer, Kubernetes grants access to API requests, which combine policies with attributes. In the following example, user _student_ can only read Pods in the Namespace **lfs158**.

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

*   **`Webhook Authorizer`**

