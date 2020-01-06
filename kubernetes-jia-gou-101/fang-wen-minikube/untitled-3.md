# 在linux上安装kubectl

要在Linux上安装kubectl，请按照以下说明进行操作：

直接下载最新稳定的kubectl二进制文件版本，然后修改文件属性使其可执行，然后加入到PATH中：

```bash
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

> 要下载和设置特定版本的kubectl（例如v1.14.1），请发出以下命令：

```bash
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.14.1/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

