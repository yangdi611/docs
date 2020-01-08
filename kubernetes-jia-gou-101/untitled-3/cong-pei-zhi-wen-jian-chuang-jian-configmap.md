# 从配置文件创建ConfigMap

首先，我们需要创建一个包含以下内容的配置文件：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: customer1
data:
  TEXT1: Customer1_Company
  TEXT2: Welcomes You
  COMPANY: Customer1 Company Technology Pct. Ltd.
```

我们在此处指定种类，元数据和数据字段，以API服务器的v1端点为目标。

如果我们使用上面的配置将文件命名为customer1-configmap.yaml，则可以使用以下命令创建ConfigMap：

```bash
$ kubectl create -f customer1-configmap.yaml
configmap/customer1 created
```



