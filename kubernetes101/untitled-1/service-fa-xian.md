# service发现

由于服务是Kubernetes中主要的通信方式，因此我们需要一种在运行时发现它们的方式。Kubernetes支持两种发现服务的方法：

* **环境变量 Environment Variables**  
  当Pod在任何工作节点上启动后，在该节点上运行的kubelet守护程序就会在Pod中为所有活动服务添加一组环境变量。例如，如果我们有一个称为redis-master的活动服务，该服务公开端口6379，并且其ClusterIP为172.17.0.6，则在新创建的Pod上，我们可以看到以下环境变量：

  **REDIS\_MASTER\_SERVICE\_HOST=172.17.0.6**  
  **REDIS\_MASTER\_SERVICE\_PORT=6379**  
  **REDIS\_MASTER\_PORT=tcp://172.17.0.6:6379**  
  **REDIS\_MASTER\_PORT\_6379\_TCP=tcp://172.17.0.6:6379**  
  **REDIS\_MASTER\_PORT\_6379\_TCP\_PROTO=tcp**  
  **REDIS\_MASTER\_PORT\_6379\_TCP\_PORT=6379**  
  **REDIS\_MASTER\_PORT\_6379\_TCP\_ADDR=172.17.0.6**  
  
  使用此解决方案时，我们在订购服务时需要小心，因为Pod不会为在Pod创建后创建的Services设置环境变量。

* **DNS**

  Kubernetes有一个[DNS](https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/dns) [附加组件](https://github.com/kubernetes/kubernetes/blob/master/cluster/addons/README.md)，可为每个服务创建DNS记录，其格式为my-svc.my-namespace.svc.cluster.local。同一命名空间中的服务仅按名称查找其他服务。如果我们在my-ns命名空间中添加服务redis-master，则同一命名空间中的所有Pod都将仅通过服务名称redis-master查找该服务。来自其他命名空间的Pod通过添加相应的命名空间作为后缀（例如redis-master.my-ns）来查找同一Service。  

这是最常见且强烈推荐的解决方案。例如，在上一节的图像中，我们看到已配置了一个内部DNS，该DNS将我们的Services frontend-svc和db-svc分别映射到172.17.0.4和172.17.0.5。

