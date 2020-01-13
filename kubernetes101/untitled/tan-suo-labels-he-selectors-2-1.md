# 探索Labels和Selectors 2

**选择具有给定标签的pods**  
要将选择器与kubectl get pods命令一起使用，我们可以使用-l选项。在以下示例中，我们选择将k8s-app Label键设置为值webserver的所有Pod：

```bash
$ kubectl get pods -l k8s-app=webserver
NAME                         READY     STATUS    RESTARTS   AGE
webserver-74d8bd488f-dwbzz   1/1       Running   0          17m
webserver-74d8bd488f-npkzv   1/1       Running   0          17m
webserver-74d8bd488f-wvmpq   1/1       Running   0          17m
```

在上面的示例中，我们列出了我们创建的所有Pod，因为所有Pod的k8s-app Label键均设置为值webserver。

**尝试使用k8s-app = webserver1作为选择器**

```bash
$ kubectl get pods -l k8s-app=webserver1
No resources found.
```

跟期望的一样，没有Pod被列出来。

