# 在windows中安装minikube

让我们学习如何在带有VirtualBox v6.0.6的Windows 10上安装Minikube 1.0.1。

> 对于其他版本，安装步骤可能会有所不同！检查[Minikube安装](https://kubernetes.io/docs/tasks/tools/install-minikube/)！
>
> Windows支持当前处于试验阶段，安装过程中可能会遇到问题。

#### 在Windows中安装VirtualBox

下载并安装.exe软件包。

> 确保在运行VirtualBox时禁用Hyper-V（如果先前已安装和使用）。

#### 安装minikube

我们可以从[Minikube发布页面](https://github.com/kubernetes/minikube/releases)下载最新版本。在编写本课程时，最新的Minikube版本是v1.0.1。下载后，我们需要确保将其添加到我们的PATH中。

在Minikube v1.0.1下有两个可供Windows下载的.exe软件包：

* **minikube-windows-amd64.exe** 需要加入到PATH中**:** 手动
* **minikube-installer.exe** 可以自动加载进PATH中。

下载并安装Minikube v1.0.1下的minikube-installer.exe软件包。

#### 启动minikube

我们可以使用minikube start命令启动Minikube（忽略“无法读取... docker \ config ...”和“没有匹配的凭据...”警告）。使用“以管理员身份运行”选项打开PowerShell，然后执行以下命令：

```bash
PS C:\WINDOWS\system32> minikube start
minikube v1.0.1 on windows (amd64)
Downloading Kubernetes v1.14.1 images in the background ...
Creating virtualbox VM (CPUs=2, Memory=2048MB, Disk=20000MB) ...
Downloading Minikube ISO ...
 0 B / 142.88 MB [-----------------------------------------------------]   0.00%
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

#### 查看状态

我们可以使用minikube status命令查看Minikube的状态。使用\`\`以管理员身份运行''选项打开PowerShell，然后执行以下命令：

```bash
PS C:\WINDOWS\system32> minikube status
host: Running
kubelet: Running
apiserver: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100
```

#### 停止minikube

我们可以使用minikube stop命令停止Minikube。使用\`\`以管理员身份运行''选项打开PowerShell，然后执行以下命令：

```bash
PS C:\WINDOWS\system32> minikube stop
Stopping "minikube" in virtualbox ...
"minikube" stopped.
```

