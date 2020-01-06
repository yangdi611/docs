# Pods

[Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)是最小和最简单的Kubernetes对象。它是Kubernetes中的部署单位，代表应用程序的单个实例。Pod是一个或多个容器的逻辑集合，其中：

* 与Pod一起安排在同一主机上
* 共享相同的网络名称空间
* 有权装载（mount）相同的外部存储（卷 Volume）。

![Pods](../../.gitbook/assets/image%20%2833%29.png)

Pods本质上是短暂的，它们没有自我修复的能力。这就是它们与处理Pod的复制、容错、自我修复等功能的控制器一起使用的原因。控制器的示例包括Deployment，ReplicaSets，ReplicationControllers等。我们使用Pod模板将嵌套的Pod规范附加到控制器对象上。就像我们在上一节中看到的那样。

以下是采用YAML格式的Pod对象配置的示例：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.15.11
    ports:
    - containerPort: 80
```

apiVersion字段必须为Pod对象定义指定v1。第二个必填字段是用于指定Pod对象类型的种类。第三个必填字段元数据包含对象的名称和标签。第四个必填字段spec标记了定义Pod对象所需状态的块的开始-也称为PodSpec。我们的Pod从Docker Hub创建一个运行nginx：1.15.11镜像的容器。

