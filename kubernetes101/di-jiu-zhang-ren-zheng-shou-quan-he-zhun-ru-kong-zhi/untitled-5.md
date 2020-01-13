# Authorization III

授权模块（第2部分）：

* **Role-Based Access Control \(RBAC\) Authorizer**

  通常，使用RBAC，我们可以根据个人用户的角色来规范对资源的访问。在Kubernetes中，我们可以将不同的角色附加到用户，服务帐户等主题上。在创建角色时，我们通过特定的操作（例如创建，获取，更新，修补等）限制资源访问。这些操作是称为动词。  
  ****  
  在RBAC中，我们可以创建两种角色：  
  **Role**  
  使用Role，我们可以授予对特定命名空间中资源的访问权限。  
  **ClusterRole**  
  可以使用ClusterRole授予与Role相同的权限，但是其作用域是整个群集。

  
  在本课程中，我们将专注于第一种角色。在下面，您将找到一个示例：

  ```text
  kind: Role
  apiVersion: rbac.authorization.k8s.io/v1
  metadata:
    namespace: lfs158
    name: pod-reader
  rules:
  - apiGroups: [""] # "" indicates the core API group
    resources: ["pods"]
    verbs: ["get", "watch", "list"]
  ```

  如您所见，它创建了一个pod-reader角色，该角色只能访问lfs158命名空间的Pod。创建角色后，我们可以使用RoleBinding绑定用户。



  有两种RoleBindings：

  _**RoleBinding**_

  它允许我们将用户绑定到与角色相同的名称空间。我们还可以在RoleBinding中引用一个ClusterRole，它将授予对RoleBinding的命名空间中ClusterRole中定义的命名空间资源的权限。

  _**ClusterBinding**_

  它允许我们在群集级别和所有命名空间授予对资源的访问权限。



  在本课程中，我们将专注于第一种RoleBinding。在下面，您将找到一个示例：

  ```yaml
  kind: RoleBinding
  apiVersion: rbac.authorization.k8s.io/v1
  metadata:
    name: pod-read-access
    namespace: lfs158
  subjects:
  - kind: User
    name: student
    apiGroup: rbac.authorization.k8s.io
  roleRef:
    kind: Role
    name: pod-reader
    apiGroup: rbac.authorization.k8s.io
  ```

  如您所见，它使学生用户可以阅读lfs158命名空间的Pods。

  要启用RBAC授权者，我们需要使用--authorization-mode = RBAC选项启动API服务器。借助RBAC授权者，我们可以动态配置策略。有关更多详细信息，请查看Kubernetes文档。



