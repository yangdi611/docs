# Kubernetes对象模块

Kubernetes具有非常丰富的对象模型，代表了Kubernetes集群中的不同持久性实体。这些实体描述：

* 我们正在运行哪些容器化应用程序以及在哪个节点上
* 应用程序资源消耗
* 附加到应用程序的不同策略，例如重新启动/升级策略，容错等。

对于每个对象，我们都会在“规范”部分中声明我们的意图或所需状态。Kubernetes系统管理对象的状态部分，在其中记录对象的实际状态。在任何给定的时间点，Kubernetes控制平面都会尝试将对象的实际状态与对象的期望状态进行匹配。

Kubernetes对象的示例包括Pod，副本集，部署，命名空间等。我们将在下面进行探讨。

创建对象时，必须将规范字段下方的对象的配置数据部分提交给Kubernetes API服务器。规范部分描述了所需的状态以及一些基本信息，例如对象的名称。创建对象的API请求必须包含spec部分以及其他详细信息。尽管API服务器接受JSON格式的对象定义文件，但大多数情况下我们会以YAML格式提供此类文件，该文件由kubectl转换为JSON负载并发送到API服务器。

以下是YAML格式的[Deployement](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)对象配置示例：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.11
        ports:
        - containerPort: 80
```

apiVersion字段是第一个必填字段，它指定我们要连接的API服务器上的API端点;它必须与定义的对象类型的现有版本匹配。第二个必填字段是实物，指定对象类型-在我们的例子中是Deployment，但可以是Pod，Replicaset，Namespace，Service等。第三个必填字段metadata包含对象的基本信息，例如名称，标签，命名空间等。我们的示例显示了两个spec字段（spec和spec.template.spec）。第四个必填字段指定标记了块的开始，该块定义了Deployment对象的所需状态。在我们的示例中，我们要确保在任何给定时间都在运行3个Pod。使用在spec.template中定义的Pod模板创建Pod。嵌套对象（例如Pod是Deployment的一部分）保留了其元数据和规范，并且丢失了apiVersion和kind-两者均已被模板替换。在spec.template.spec中，我们定义Pod的所需状态。我们的Pod使用Docker Hub创建一个运行nginx：1.15.11镜像的容器。

创建Deployment对象后，Kubernetes系统会将status字段附加到该对象；我们将在以后进行探讨。

