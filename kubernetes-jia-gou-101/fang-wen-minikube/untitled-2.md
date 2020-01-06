# 在macOS上安装kubectl

有两种方法可以在macOS上安装Kubectl：手动和使用Homebrew软件包管理器。接下来，我们将提供两种方法的说明。

要手动安装kubectl，请下载最新的稳定的kubectl二进制文件，使其可执行并使用以下命令将其移至PATH：

```bash
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

> 要下载并设置特定版本的Kubectl（例如v1.14.1），请发出以下命令：

```bash
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.14.1/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

要使用[Homebrew软件包管理器](https://brew.sh/)安装kubectl，请发出以下命令：

```bash
$ brew install kubernetes-cli
```



