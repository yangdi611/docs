# 用CLI部署应用一

要使用CLI部署应用程序，首先让我们删除之前创建的Deployment。

#### 删除之前的部署

我们可以通过`kubectl delete` 删除任何对象。下一步我们删除掉之前部署的webserver。

```bash
 kubectl delete deployments webserver
deployment.extensions "webserver" deleted
```

删除部署还会删除ReplicaSet及其创建的Pod：

```bash
$ kubectl get replicasets
No resources found.

$ kubectl get pods
No resources found.
```

