# 从文件创建ConfigMap

首先，我们需要使用以下配置数据创建文件Permission-reset.properties：

```bash
permission=read-only
allowed="true"
resetCount=3
```

然后，我们可以使用以下命令创建ConfigMap：

```bash
$ kubectl create configmap permission-config --from-file=<path/to/>permission-reset.properties
configmap/permission-config created
```

