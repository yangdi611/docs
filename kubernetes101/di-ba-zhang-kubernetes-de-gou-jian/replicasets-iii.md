# ReplicaSets III

ReplicaSet将检测到当前状态不再与所需状态匹配。ReplicaSet将创建一个附加的Pod，从而确保当前状态与所需状态匹配。

![ReplicaSet\(&#x6839;&#x636E;&#x9884;&#x8BBE;&#x65B0;&#x5EFA;&#x7ACB;&#x4E00;&#x4E2A;pod\)](../../.gitbook/assets/image%20%2819%29.png)

ReplicaSets可以单独用作Pod控制器，但它们仅提供有限的功能集。部署（Deployment）是Pod业务流程的推荐控制器，它提供了一组补充功能。部署管理Pod的创建，删除和更新。部署会自动创建一个ReplicaSet，然后再创建一个Pod。无需分别管理副本集和Pod，部署将代表我们管理它们。

