# 在linux上安装minikube

让我们学习如何在带有VirtualBox v6.0的Ubuntu Linux 18.04 LTS上安装Minikube v1.0.1。

> **NOTE: For other versions, the installation steps may vary! Check the** [**Minikube installation**](https://kubernetes.io/docs/tasks/tools/install-minikube/)**!**

#### 安装[VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)管理程序

添加仿生发行版的源存储库（Ubuntu 18.04），下载并注册公钥，更新并安装：

```text
$ sudo bash -c 'echo "deb https://download.virtualbox.org/virtualbox/debian bionic contrib" >> /etc/apt/sources.list'
$ wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install -y virtualbox-6.0
```

####  安装Minikube

我们可以从[Minikube发布页面](https://github.com/kubernetes/minikube/releases)下载最新版本。在编写本课程时，最新的Minikube版本是v1.0.1。下载后，我们需要使其可执行并添加到我们的PATH中：

```text
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.0.1/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

