# 使用dashboard部署应用三

创建Web服务器部署后，可以使用仪表板左侧的资源导航面板在默认命名空间中显示Deployment，ReplicaSet和Pod的详细信息。仪表板显示的资源与通过Kubectl从CLI显示的一对一资源匹配。

#### 列出所有部署

我们可以使用kubectl get deploys命令在默认命名空间中列出所有Deployment：

```bash
$ kubectl get deployments
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
webserver   3/3     3            3           9m
```

 **列出所有的ReplicaSets**

我们可以使用kubectl get copysets命令在默认命名空间中列出所有ReplicaSet：

```bash
$ kubectl get replicasets
NAME                   DESIRED   CURRENT   READY   AGE
webserver-74d8bd488f   3         3         3       9m
```

 **列出所有pods**

我们可以使用kubectl get pods命令在默认名称空间中列出所有Pod：

```bash
$ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
webserver-74d8bd488f-dwbzz    1/1     Running   0          9m
webserver-74d8bd488f-npkzv    1/1     Running   0          9m
webserver-74d8bd488f-wvmpq    1/1     Running   0          9m 
```

\*\*\*\*

