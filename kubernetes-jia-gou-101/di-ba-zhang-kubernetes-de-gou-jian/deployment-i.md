# Deployment I

部署对象为Pod和副本集提供声明性更新。DeploymentController是主节点的控制器管理器的一部分，它确保当前状态始终与所需状态匹配。它允许通过部署和回滚进行无缝的应用程序更新和降级，并且它直接管理其ReplicaSets以进行应用程序扩展。

在以下示例中，新的Deployment创建了ReplicaSet A，然后创建了3个Pod，每个Pod模板配置为运行一个nginx：1.7.9容器映像。在这种情况下，副本集A与nginx：1.7.9关联，表示部署的状态。此特定状态记录为修订版1。

![Deployment \(ReplicaSet A Created\)](../../.gitbook/assets/image%20%2822%29.png)

