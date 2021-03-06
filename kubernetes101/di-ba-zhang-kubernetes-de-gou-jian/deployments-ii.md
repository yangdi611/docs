# Deployments II

现在，在部署中，我们更改Pods的模板，并将容器映像从nginx：1.7.9更新为nginx：1.9.1。部署为版本为1.9.1的新容器映像触发新的副本集B，此关联表示部署版本2的新记录状态。两个副本集之间的无缝过渡，从版本为3.7.9的3个Pod的副本集A到版本1.7.9的无缝过渡。具有3个新Pod版本1.9.1或从版本1到版本2的新ReplicaSet B是Deployment滚动更新。

当我们更新部署的Pods模板时，将触发滚动更新。扩展或标记部署等操作不会触发滚动更新，因此不会更改修订号。

滚动更新完成后，部署将显示副本集A和B，其中A缩放到0（零）个Pod，B缩放到3个Pod。这就是部署将其先前状态配置设置记录为修订的方式。

![Deployment \(ReplicaSet B Created\)](https://github.com/yangdi611/docs/tree/9182ec0aacc614915b8202361f45684572b4ddf8/.gitbook/assets/image%20%2836%29.png)

