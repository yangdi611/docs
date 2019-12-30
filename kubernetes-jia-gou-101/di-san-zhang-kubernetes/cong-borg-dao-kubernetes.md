# 从Borg到Kubernetes

根据2015年Google Borg论文的摘要，

> Google的Borg系统是一个集群管理器，它跨多个集群运行成千上万个作业，这些作业来自成千上万个不同的应用程序，每个集群中都有多达数万台机器。

十多年来，Borg一直是Google的秘密，在生产中运行其全球范围内的容器化工作负载。我们从Google使用的服务（例如Gmail，云端硬盘，地图，文档等）均通过Borg提供服务。

Kubernetes的一些最初作者是Google员工，他们曾经使用过Borg并进行过开发。他们在设计Kubernetes时投入了宝贵的知识和经验。Kubernetes的某些功能/对象可以追溯到Borg或从Borg中学到的经验教训：

* API servers
* Pods
* IP-per-Pod
* Services
* Labels.



