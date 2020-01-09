# 自定义资源

在kubernetes里，资源是一个存储API对象的API端点。比如，一个Pod资源包含所有的Pod对象。

尽管在大多数情况下，现有的Kubernetes资源足以满足我们的要求，但是我们也可以使用自定义资源来创建新资源。使用自定义资源，我们不必修改Kubernetes源。

自资源定义是动态的，并且它们可以随时在已经运行的集群中显示和消失。

为了使资源具有声明性，我们必须创建并安装一个自定义控制器，该控制器可以解释资源结构并执行所需的操作。可以在已运行的集群中部署和管理自定义控制器。

有两种添加自定义资源的方法：

* [Custom Resource Definitions \(CRDs\)](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)

  这是一个简单的添加自定义资源的方法，它不需要任何的编程知识。尽管，创建自定义控制器是需要一些编程知识的。

* [API Aggregation](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/apiserver-aggregation/) 为了获得更细粒度的控制，我们可以编写API聚合器。它们是位于主API服务器后面的从属API服务器。主API服务器充当所有传入API请求的代理-它根据其功能和对下级API服务器的其他请求的代理来处理请求。

