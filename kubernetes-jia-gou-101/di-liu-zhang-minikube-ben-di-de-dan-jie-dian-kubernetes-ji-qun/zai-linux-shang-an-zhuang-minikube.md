# 在linux上安装minikube

让我们学习如何在带有VirtualBox v6.0的Ubuntu Linux 18.04 LTS上安装Minikube v1.0.1。

> **NOTE: For other versions, the installation steps may vary! Check the** [**Minikube installation**](https://kubernetes.io/docs/tasks/tools/install-minikube/)**!**

#### 安装[VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)管理程序

添加仿生发行版的源存储库（Ubuntu 18.04），下载并注册公钥，更新并安装：

```bash
$ sudo bash -c 'echo "deb https://download.virtualbox.org/virtualbox/debian bionic contrib" >> /etc/apt/sources.list'
$ wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install -y virtualbox-6.0
```

####  安装Minikube

我们可以从[Minikube发布页面](https://github.com/kubernetes/minikube/releases)下载最新版本。在编写本课程时，最新的Minikube版本是v1.0.1。下载后，我们需要使其可执行并添加到我们的PATH中：

```bash
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.0.1/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

> Note：用/ latest /替换/v1.0.1/将始终下载最新版本。

#### 启动minikube

我们可以使用minikube start命令启动Minikube（忽略“无法读取... / docker / config ...”和“没有匹配的凭据...”警告）：

```bash
$ minikube start
minikube v1.0.1 on linux (amd64)
Downloading Minikube ISO ...
142.88 MB / 142.88 MB [============================================] 100.00% 0s
Downloading Kubernetes v1.14.1 images in the background ...
Creating virtualbox VM (CPUs=2, Memory=2048MB, Disk=20000MB) ...
"minikube" IP address is 192.168.99.100
Configuring Docker as the container runtime ...
Version of container runtime is 18.06.3-ce
Waiting for image downloads to complete ...
Preparing Kubernetes environment ...
Downloading kubeadm v1.14.1
Downloading kubelet v1.14.1
Pulling images required by Kubernetes v1.14.1 ...
Launching Kubernetes v1.14.1 using kubeadm ... 
Waiting for pods: apiserver proxy etcd scheduler controller dns
Configuring cluster permissions ...
Verifying component health .....
kubectl is now configured to use "minikube"
For best results, install kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/
Done! Thank you for using minikube!切人
```

#### 确认状态

使用minikube status命令，我们显示Minikube的状态：

```bash
$ minikube status
host: Running
kubelet: Running
apiserver: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100
```

#### 停止minikue

使用minikube stop命令，我们可以停止Minikube：

```bash
$ minikube stop
Stopping "minikube" in virtualbox ...
"minikube" stopped.
```

