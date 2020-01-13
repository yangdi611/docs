# namespace

### Namespace

#### Brief

`Namespace` 是 Linux 内核提供的一个功能，可以实现系统资源的隔离，如：PID、User ID、Network 等。Linux 中的 chroot 命令可以将当前目录设置为根目录，使得用户的操作被限制在当前目录之下而不影响其他目录。

**为什么需要namespace**

假设一个IaaS的用户购买了一个实例在运行自己的应用。如果其他某些用户能够进入到其他人的实例中，修改或关闭其他实例中应用的状态，那么就会导致不同用户之间相互影响； 用户的某些操作可能需要 root 权限，假如我们给每个用户都赋予了 root 权限，如果他们没有被隔离的话，那么我们的机器也就没有任何安全性可言了。 使用 Namespace，Linux 可以做到 UID 级别的隔离，也就是说，UID 为 n 的用户在自己的 Namespace 中是有 root 权限的，但是在真实的物理机上，他仍然是 UID 为 n 的用户。

**namespace可以进行隔离的资源\(RH\)**

* Mount

  隔离文件系统挂载点，每个进程都被限制在他自有的文件系统下。

  The mount namespace isolates file system mount points, enabling each process to have a distinct filesystem space within wich to operate.

* UTS

  Hostname和NIS domain name

* IPC

  信号量隔离

  System V IPC, POSIX message queues

* PID

  进程ID

* Network

  网络设备，堆栈（协议等），端口等等

  Network devices, stacks, ports, etc.

* User

  用户和组ID

  User and group IDs

* Control Groups

  资源管理器

  Isolates cgroups

**Cgroups**

Namespace 帮助进程隔离出自己的单独空间，而 Cgroups 则可以限制每个空间的大小。Cgroups 提供了对一组进程及将来子进程的资源限制、控制和统计的能力。

