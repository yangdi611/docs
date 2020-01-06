# 在Windows上安装kubectl

要安装kubectl，我们可以直接下载二进制文件或从CLI使用curl。下载完成后，需要将二进制文件添加到PATH中。

 **直接下载二进制文件：**

{% embed url="https://storage.googleapis.com/kubernetes-release/release/v1.14.1/bin/windows/amd64/kubectl.exe" %}

> 从下面的链接获取最新的kubectl稳定版本版本号，如果需要，请从上面编辑二进制文件的下载链接：
>
> [https://storage.googleapis.com/kubernetes-release/release/stable.txt](https://storage.googleapis.com/kubernetes-release/release/stable.txt)

#### 用curl命令下载：

```bash
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.14.1/bin/windows/amd64/kubectl.exe
```

下载后，将kubectl二进制文件移至PATH中。

