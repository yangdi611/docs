# Services

在以下图形表示中，app是Label键，frontend和db是不同Pod的Label值。

![Grouping of Pods using Labels and Selectors](../../.gitbook/assets/image%20%2819%29.png)

使用选择器app == frontend和app == db，我们将Pod分为两个逻辑集合：一个具有3个Pod，一个具有单个Pod。

我们为逻辑分组分配一个名称，称为服务。在我们的示例中，我们创建了两个服务frontend-svc和db-svc，它们分别具有app == frontend和app == dbSelectors。

![Grouping of Pods using the Service object](../../.gitbook/assets/image%20%2830%29.png)

服务可以公开单个Pod，ReplicaSet，Deployment，DaemonSet和StatefulSet。

