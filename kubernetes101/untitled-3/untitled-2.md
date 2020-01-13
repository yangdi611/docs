# 从文件创建Secret并显示其详细信息

要从文件创建Secret，我们可以使用kubectl create secret命令。

首先，我们对敏感数据进行编码，然后将编码后的数据写入文本文件：

```bash
 $ echo mysqlpassword | base64
 bXlzcWxwYXNzd29yZAo=

 $ echo -n 'bXlzcWxwYXNzd29yZAo=' > password.txt
```

现在我们可以从password.txt文件创建Secret：

```bash
$ kubectl create secret generic my-file-password --from-file=password.txt
secret/my-file-password created
```

成功创建Secret后，我们可以使用get和describe命令对其进行分析。他们没有透露Secret的内容。该类型列为Opaque。

```bash
$ kubectl get secret my-file-password
NAME               TYPE     DATA   AGE 
my-file-password   Opaque   1      8m

$ kubectl describe secret my-file-password
Name:          my-file-password
Namespace:     default
Labels:        <none>
Annotations:   <none>

Type  Opaque

Data
====
password.txt:  13 bytes
```

