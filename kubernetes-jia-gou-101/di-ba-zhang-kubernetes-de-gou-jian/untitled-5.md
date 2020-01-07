# Deployments III

一旦ReplicaSet B及其3个Pod版本1.9.1准备就绪，部署将开始主动管理它们。但是，部署将其先前的配置状态保存为修订，这些修订在部署的回滚功能中起关键作用-返回到先前的已知配置状态。在我们的示例中，如果新的nginx：1.9.1的性能不令人满意，则可以将部署回滚到先前的版本，在这种情况下，可以从版本2退回到运行nginx：1.7.9的版本1。

![Deployment Points to ReplicaSet B](../../.gitbook/assets/image%20%284%29.png)



