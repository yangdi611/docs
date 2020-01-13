# 创建Secret并显示其详细信息

要创建一个Secret，我们可以使用Kubectl create secret命令：

```bash
$ kubectl create secret generic my-password --from-literal=password=mysqlpassword
```

上面的命令将创建一个名为my-password的secret，该secret密码的值设置为mysqlpassword。

成功创建secret后，我们可以使用get和describe命令对其进行分析。他们没有透露secret的内容。该类型列为不透明。

```bash
$ kubectl get secret my-password
NAME          TYPE     DATA   AGE 
my-password   Opaque   1      8m

$ kubectl describe secret my-password
Name:          my-password
Namespace:     default
Labels:        <none>
Annotations:   <none>

Type  Opaque

Data
====
password:  13 bytes
```

