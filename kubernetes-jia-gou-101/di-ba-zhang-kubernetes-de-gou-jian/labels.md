# Labels

Labels（标签）是附加在Kubernetes对象上的键/值对（例如Pod，ReplicaSets）。标签用于根据适当的要求组织和选择对象的子集。许多对象可以具有相同的标签。标签不能为对象提供唯一性。控制器使用标签将逻辑上解耦的对象组合在一起，而不是使用对象的名称或ID。

![Labels](../../.gitbook/assets/image%20%2821%29.png)

在上图中，我们使用了两个Label键：app和env。根据我们的要求，我们为四个Pod赋予了不同的值。Label env = dev逻辑上选择并分组顶部的两个Pod，而Lable app = frontend逻辑上选择并分组左侧的两个Pod。我们可以通过选择两个标签来选择四个Pod之一-左下角：app = frontend和env = qa。

