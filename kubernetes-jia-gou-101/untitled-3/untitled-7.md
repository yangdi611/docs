# 在Pods里使用ConfigMaps

#### **作为环境变量**

在容器内，我们可以检索整个ConfigMap的键值数据或特定ConfigMap键的值作为环境变量。

在以下示例中，所有myapp-full-container容器的环境变量都接收full-config-map ConfigMap键的值：

```yaml
...
  containers:
  - name: myapp-full-container
    image: myapp
    envFrom:
    - configMapRef:
      name: full-config-map
...
```

在以下示例中，myapp-specific-container容器的环境变量从来自两个单独的ConfigMap的特定键值对接收其值：

```yaml
...
  containers:
  - name: myapp-specific-container
    image: myapp
    env:
    - name: SPECIFIC_ENV_VAR1
      valueFrom:
        configMapKeyRef:
          name: config-map-1
          key: SPECIFIC_DATA
    - name: SPECIFIC_ENV_VAR2
      valueFrom:
        configMapKeyRef:
          name: config-map-2
          key: SPECIFIC_INFO
...
```

通过以上操作，我们将从config-map-1 ConfigMap中将SPECIFIC\_ENV\_VAR1环境变量设置为SPECIFIC\_DATA键值，并从config-map-2 ConfigMap中将SPECIFIC\_ENV\_VAR2环境变量设置为SPECIFIC\_INFO键值。

#### 作为卷

我们可以将vol-config-map ConfigMap作为卷安装在Pod中。对于ConfigMap中的每个键，都会在安装路径中创建一个文件（该文件以键名命名），并且该文件的内容成为相应键的值：

```yaml
...
  containers:
  - name: myapp-vol-container
    image: myapp
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: vol-config-map
...
```

