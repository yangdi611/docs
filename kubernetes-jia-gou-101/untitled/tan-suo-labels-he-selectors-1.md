# 探索Labels和Selectors 1

之前，我们已经看到标签Labels和选择器Selectors在对可以执行操作的对象子集进行分组中扮演着重要的角色。接下来，我们将仔细研究它们。

**Look at a Pod's Details**  
We can look at an object's details using **kubectl describe** command. In the following example, you can see a Pod's description:

```bash
$ kubectl describe pod webserver-74d8bd488f-dwbzz
Name:           webserver-74d8bd488f-dwbzz
Namespace:      default
Priority:       0
Node:           minikube/10.0.2.15
Start Time:     Wed, 15 May 2019 13:17:33 -0500
Labels:         k8s-app=webserver
                pod-template-hash=74d8bd488f
Annotations:    <none>
Status:         Running
IP:             172.17.0.5
Controlled By:  ReplicaSet/webserver-74d8bd488f
Containers:
  webserver:
    Container ID:   docker://96302d70903fe3b45d5ff3745a706d67d77411c5378f1f293a4bd721896d6420
    Image:          nginx:alpine
    Image ID:       docker-pullable://nginx@sha256:8d5341da24ccbdd195a82f2b57968ef5f95bc27b3c3691ace0c7d0acf5612edd
    Port:           <none>
    State:          Running
      Started:      Wed, 15 May 2019 13:17:33 -0500
    Ready:          True
    Restart Count:  0
...
```

 kubectl describe命令显示Pod的更多详细信息。但是，目前，我们将重点关注“标签”字段，在这里我们将“标签”设置为k8s-app = webserver。

The **kubectl describe** command displays many more details of a Pod. For now, however, we will focus on the **Labels** field, where we have a Label set to **k8s**-**app=webserver**.

