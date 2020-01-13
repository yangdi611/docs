# 创建ConfigMap并显示其详细信息

可以使用kubectl create命令创建ConfigMap，我们可以使用kubectl get命令显示其详细信息。

**创建ConfigMap**

```bash
$ kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
configmap/my-config created
```

**显示my-config的ConfigMap详细信息**

```yaml
$ kubectl get configmaps my-config -o yaml
apiVersion: v1
data:
  key1: value1
  key2: value2
kind: ConfigMap
metadata:
  creationTimestamp: 2019-05-31T07:21:55Z
  name: my-config
  namespace: default
  resourceVersion: "241345"
  selfLink: /api/v1/namespaces/default/configmaps/my-config
  uid: d35f0a3d-45d1-11e7-9e62-080027a46057
```

使用-o yaml选项，我们请求kubectl命令以YAML格式吐出输出。如我们所见，该对象具有ConfigMap类型，并且在数据字段内部具有键值对。ConfigMap的名称和其他详细信息是元数据字段的一部分。

