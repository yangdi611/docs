# 注释

使用[注释](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/)，我们可以将任意非标识元数据以键值格式附加到任何对象：

```yaml
"annotations": {
  "key1" : "value1",
  "key2" : "value2"
}
```

与Labels不同，注释不用于标识和选择对象。注释可用于：

* 存储构建/发行ID，PR号，git分支等。
* 负责人的电话/寻呼机号码，或指定可在何处找到此类信息的目录条目
* 指向日志记录，监视，分析，审计存储库，调试工具等的指针。
* 等等

例如，在创建展开时，我们可以添加如下所示的描述：

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: webserver
  annotations:
    description: Deployment based PoC dates 2nd May'2019
....
```

描述对象时显示注释：

```yaml
$ kubectl describe deployment webserver
Name:                webserver
Namespace:           default
CreationTimestamp:   Fri, 03 May 2019 05:10:38 +0530
Labels:              app=webserver
Annotations:         deployment.kubernetes.io/revision=1
                     description=Deployment based PoC dates 2nd May'2019
...
```

