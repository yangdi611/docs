# 在macOS上安装minikube

让我们学习如何在带有VirtualBox v6.0的Mac OS X上安装Minikube v1.0.1。

> NOTE: 对于其他版本，安装步骤可能会有所不同！检查[Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)！

尽管VirtualBox是Minikube的默认管理程序，但在Mac OS X上，我们可以使用--vm-driver = xhyve或= hyperkit 启动选项在启动时将Minikube配置为使用其他管理程序。

#### 安装VirtualBox hypervisor

下载和安装.dmg包文件

#### 安装minikube

我们可以从Minikube发布页面[下载](https://github.com/kubernetes/minikube/releases)最新版本。在编写本课程时，最新的Minikube版本是v1.0.1。下载后，我们需要使其可执行并添加到我们的PATH中：

```bash
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.0.1/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

#### 启动minikube

我们可以使用minikube start命令启动Minikube（忽略“无法读取... / docker / config ...”和“没有匹配的凭据...”警告）：

```bash
$ minikube start
minikube v1.0.1 on darwin (amd64)
Downloading Kubernetes v1.14.1 images in the background ...
Creating virtualbox VM (CPUs=2, Memory=2048MB, Disk=20000MB) ...
Downloading Minikube ISO ...
142.88 MB / 142.88 MB [============================================] 100.00% 0s
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
Done! Thank you for using minikube!
```

#### 检查状态

使用minikube status命令，我们显示Minikube的状态：

```bash
$ minikube status
host: Running
kubelet: Running
apiserver: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100
```

停止minikube

使用minikube stop命令，我们可以停止Minikube：

```bash
$ minikube stop
Stopping "minikube" in virtualbox ...
"minikube" stopped.
```

