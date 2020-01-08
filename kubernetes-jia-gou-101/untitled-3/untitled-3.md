# 手动创建Secret

我们可以从YAML配置文件中手动创建一个Secret。下面的示例文件名为mypass.yaml。Secret中有两种用于敏感信息的映射：data和stringData。

对于data映射，必须使用base64对敏感信息字段的每个值进行编码。如果要为我们的Secret创建配置文件，则必须首先为密码创建base64编码：

```bash
$ echo mysqlpassword | base64
bXlzcWxwYXNzd29yZAo=
```

然后在配置文件中使用它：

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-password
type: Opaque
data:
  password: bXlzcWxwYXNzd29yZAo=
```

请注意，base64编码并不意味着加密，任何人都可以轻松解码我们的编码数据：

```bash
$ echo "bXlzcWxwYXNzd29yZAo=" | base64 --decode
mysqlpassword
```

因此，请确保您不在源代码中提交Secret的配置文件。

使用stringData映射，无需对每个敏感信息字段的值进行编码。创建my-password Secret时，将对敏感字段的值进行编码：

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-password
type: Opaque
stringData:
  password: mysqlpassword
```

现在，使用mypass.yaml配置文件，可以使用kubectl create命令创建一个Secret：

```bash
$ kubectl create -f mypass.yaml
secret/my-password created
```



