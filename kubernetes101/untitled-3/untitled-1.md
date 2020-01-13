# 在Pods里使用Secrets

容器中的Secret将Secret用作装入的数据卷或环境变量，并以其整体或特定键值对其进行引用。

## 将Secret用作环境变量

下面我们仅引用my-password密码的密码密钥，并将其值分配给WORDPRESS\_DB\_PASSWORD环境变量：

```yaml
....
spec:
  containers:
  - image: wordpress:4.7.3-apache
    name: wordpress
    env:
    - name: WORDPRESS_DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-password
          key: password
....
```

## 将Secrets用作文件在Pod里

我们也可以将Secret作为卷mount在Pod里。以下示例为每个my-password Secret密钥创建一个文件（其中文件以密钥名称命名），该文件包含Secret的值：

```yaml
....
spec:
  containers:
  - image: wordpress:4.7.3-apache
    name: wordpress
    volumeMounts:
    - name: secret-volume
      mountPath: "/etc/secret-data"
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: my-password
....
```

