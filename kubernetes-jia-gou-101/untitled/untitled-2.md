# 活性（Liveness）命令

 在以下示例中，我们正在检查文件/tmp/healthy是否存在：

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - name: liveness
    image: k8s.gcr.io/busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```

配置为使用periodSeconds参数每5秒检查一次/tmp/healthy文件是否存在。initialDelaySeconds参数请求kubelet在第一次探测之前等待5秒。在容器上运行命令行参数时，我们将首先创建/tmp/healthy文件，然后在30秒后将其删除。删除文件将触发运行状况失败，并且我们的Pod将重新启动。

