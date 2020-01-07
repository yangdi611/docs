# 探索Labels和Selectors 2

**List the Pods, along with their attached Labels**  
通过kubectl get pods命令的-L选项，我们在输出中添加了额外的列，以列出Pod及其附加的Label键及其值。在以下示例中，我们列出了带有标签键k8s-app和label2的Pod：

```bash
$ kubectl get pods -L k8s-app,label2
NAME                         READY   STATUS    RESTARTS   AGE   K8S-APP     LABEL2
webserver-74d8bd488f-dwbzz   1/1     Running   0          14m   webserver   
webserver-74d8bd488f-npkzv   1/1     Running   0          14m   webserver   
webserver-74d8bd488f-wvmpq   1/1     Running   0          14m   webserver 
```

列出了所有Pod，因为每个Pod都有Label键k8s-app，其值设置为webserver。我们可以在K8S-APP列中看到这一点。由于所有Pod都没有label2 Label键，因此LABEL2列下没有列出任何值。



