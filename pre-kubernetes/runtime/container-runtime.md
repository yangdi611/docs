# container runtime

## 目录

**1. 容器运行时（runtime）**

**2. Docker（compose、build、run、registry、network、volume）**

## 1. 容器运行时（runtime）

> _Reference:_ Open Container Initiative: [https://www.opencontainers.org/about](https://www.opencontainers.org/about) Runtime-spec: [https://github.com/opencontainers/runtime-spec/blob/master/runtime.md](https://github.com/opencontainers/runtime-spec/blob/master/runtime.md) 走进docker：[https://segmentfault.com/a/1190000009583199](https://segmentfault.com/a/1190000009583199)

### 1.1 什么是运行时（runtime）

容器运行时可以理解成是一个通过调用各个系统层的功能来实现容器的建立、运行、回收等等的能力。

OCI组织定义了容器运行时的一系列标准，OCI是2015年由Docker发起并由众多IT、容器厂家参与的开源组织，旨在使容器的运行时和容器镜像标准化，大大方便了容器的使用者。

**1.1.1 容器runtime标准**

在linux平台上，跟容器runtime有关的规范主要有：

* `Runtime and Lifecycle`（[runtime.md](https://github.com/opencontainers/runtime-spec/blob/master/runtime.md)）
* `Container Configuration file`（[config.md](https://github.com/opencontainers/runtime-spec/blob/master/config.md)）
* `Linux Container`（[runtime-linux.md](https://github.com/opencontainers/runtime-spec/blob/master/runtime-linux.md)）
* `Linux Container Configuration`（[config-linux.md](https://github.com/opencontainers/runtime-spec/blob/master/config-linux.md)）
* `Filesystem Bundle`（\[bundle.md\]\([https://github.com/opencontainers/runtime-spec/blob/master/config-linux.md](https://github.com/opencontainers/runtime-spec/blob/master/bundle.md)\)）

**1.1.2 环境准备**

**获取image**

为了更好地理解运行时（runtime）标准，我们先准备一个环境来看一下。 首先我们先用skopeo拉取一个符合oci的镜像：

```text
[root@localhost test]# skopeo copy docker://hello-world oci:hello-world
Getting image source signatures
Copying blob 1b930d010525 done
Copying config a1c2dc0436 done
Writing manifest to image destination
Storing signatures
[root@localhost test]# tree
.
└── hello-world
    ├── blobs
    │   └── sha256
    │       ├── 1b930d010525941c1d56ec53b97bd057a67ae1865eebf042686d2a2d18271ced
    │       ├── 51214b9592e74acb2d0cf345fcb571005a94d4c8031553ee3c3e11a128228f29
    │       └── a1c2dc0436ebd2a8c85aac010c5002b907780ef21c921e67bf68052e316bdbd5
    ├── index.json
    └── oci-layout
```

然后通过oci-image-tools来看一下这个image里有什么东西，是一个可执行的文件`hello`：

```text
[root@localhost test]# oci-image-tool unpack --ref platform.os=linux hello-world hello-world-filesystem       
[root@localhost test]# ll
total 0
drwxr-xr-x. 3 root root 55 Aug 30 10:33 hello-world
drwxr-x---. 2 root root 19 Aug 30 10:37 hello-world-filesystem
[root@localhost test]# cd hello-world-filesystem/
[root@localhost hello-world-filesystem]# ll
total 4
-rwxr-xr-x. 1 root root 1840 Aug 30 10:37 hello
[root@localhost hello-world-filesystem]# file hello
hello: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, BuildID[sha1]=1db23291fc8346e8622488be91d4c5438b760315, stripped
```

**根据image生成runtime的bundle**

runtime的bundle就是运行容器时需要的东西的集合：

```text
[root@localhost test]# oci-image-tool create --ref platform.os=linux hello-world hello-world-bundle   
[root@localhost test]# ll
total 0
drwxr-xr-x. 3 root root 55 Aug 30 10:33 hello-world
drwxr-x---. 3 root root 39 Aug 30 10:42 hello-world-bundle
drwxr-x---. 2 root root 19 Aug 30 10:37 hello-world-filesystem
```

我们来看一下这个集合里有什么：

```text
[root@localhost test]# cd hello-world-bundle/
[root@localhost hello-world-bundle]# ll
total 4
-rw-r--r--. 1 root root 216 Aug 30 10:42 config.json
drwxr-x---. 2 root root  19 Aug 30 10:42 rootfs
[root@localhost hello-world-bundle]# tree
.
├── config.json
└── rootfs
    └── hello

1 directory, 2 files
```

从这里生成的bundle可以看出，bundle里面就是一个配置文件加上rootfs，rootfs里面的东西就是image里面的文件系统部分，config.json是对容器的描述，比如rootfs的路径，容器启动后要运行什么命令等。

**1.1.3 容器Filesystem Bundle标准**

我们先来看一下filesystem bundle里的内容：

```text
[root@localhost hello-world-bundle]# tree
.
├── config.json
└── rootfs
    └── hello

1 directory, 2 files
```

bundle中包含了运行容器所需要的所有信息，有了这个bundle后，符合runtime标准的程序（比如runc）就可以根据bundle启动容器了。

bundle包含一个config.json文件和容器的根文件系统目录，config.json就是后面要介绍的`Container Configuration file`，标准要求该配置文件必须叫这个名字，不过对容器的根文件系统目录没有要求，只要在config.json里面将路径配置正确就可以了，不过一般约定俗成都叫rootfs。

实际使用过程中，根文件系统目录可能在其它的地方，只要config.json里面配置正确的路径就可以了，但如果bundle需要打包和其它人分享的话，必须将根文件系统和config.json打包在一起，并且不包含外层的文件夹。

**1.1.4 Container Configuration file**

首先我们来看一下config.json里面的内容：

```javascript
{
  "ociVersion": "1.0.0",
  "process": {
    "terminal": true,
    "user": {
      "uid": 0,
      "gid": 0
    },
    "args": [
      "/hello"
    ],
    "env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    ],
    "cwd": "/"
  },
  "root": {
    "path": "rootfs"
  },
  "linux": {}
}
```

这里我们看的config文件内容很少，我们先删除掉他，然后用runc去生成一个config看看：

```text
[root@localhost hello-world-bundle]# runc spec
[root@localhost hello-world-bundle]# ll
total 4
-rw-r--r--. 1 root root 2618 Aug 30 15:42 config.json
drwxr-x---. 2 root root   19 Aug 30 10:42 rootfs
[root@localhost hello-world-bundle]#
```

然后我们再来看一下config的内容：

```yaml
[root@localhost hello-world-bundle]# cat config.json 
{
        "ociVersion": "1.0.1-dev",
        "process": {
                "terminal": true,
                "user": {
                        "uid": 0,
                        "gid": 0
                },
                "args": [
                        "sh"
                ],
                "env": [
                        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                        "TERM=xterm"
                ],
                "cwd": "/",
                "capabilities": {
                        "bounding": [
                                "CAP_AUDIT_WRITE",
                                "CAP_KILL",
                                "CAP_NET_BIND_SERVICE"
                        ],
                        "effective": [
                                "CAP_AUDIT_WRITE",
                                "CAP_KILL",
                                "CAP_NET_BIND_SERVICE"
                        ],
                        "inheritable": [
                                "CAP_AUDIT_WRITE",
                                "CAP_KILL",
                                "CAP_NET_BIND_SERVICE"
                        ],
                        "permitted": [
                                "CAP_AUDIT_WRITE",
                                "CAP_KILL",
                                "CAP_NET_BIND_SERVICE"
                        ],
                        "ambient": [
                                "CAP_AUDIT_WRITE",
                                "CAP_KILL",
                                "CAP_NET_BIND_SERVICE"
                        ]
                },
                "rlimits": [
                        {
                                "type": "RLIMIT_NOFILE",
                                "hard": 1024,
                                "soft": 1024
                        }
                ],
                "noNewPrivileges": true
        },
        "root": {
                "path": "rootfs",
                "readonly": true
        },
        "hostname": "runc",
        "mounts": [
                {
                        "destination": "/proc",
                        "type": "proc",
                        "source": "proc"
                },
                {
                        "destination": "/dev",
                        "type": "tmpfs",
                        "source": "tmpfs",
                        "options": [
                                "nosuid",
                                "strictatime",
                                "mode=755",
                                "size=65536k"
                        ]
                },
                {
                        "destination": "/dev/pts",
                        "type": "devpts",
                        "source": "devpts",
                        "options": [
                                "nosuid",
                                "noexec",
                                "newinstance",
                                "ptmxmode=0666",
                                "mode=0620",
                                "gid=5"
                        ]
                },
                {
                        "destination": "/dev/shm",
                        "type": "tmpfs",
                        "source": "shm",
                        "options": [
                                "nosuid",
                                "noexec",
                                "nodev",
                                "mode=1777",
                                "size=65536k"
                        ]
                },
                {
                        "destination": "/dev/mqueue",
                        "type": "mqueue",
                        "source": "mqueue",
                        "options": [
                                "nosuid",
                                "noexec",
                                "nodev"
                        ]
                },
                {
                        "destination": "/sys",
                        "type": "sysfs",
                        "source": "sysfs",
                        "options": [
                                "nosuid",
                                "noexec",
                                "nodev",
                                "ro"
                        ]
                },
                {
                        "destination": "/sys/fs/cgroup",
                        "type": "cgroup",
                        "source": "cgroup",
                        "options": [
                                "nosuid",
                                "noexec",
                                "nodev",
                                "relatime",
                                "ro"
                        ]
                }
        ],
        "linux": {
                "resources": {
                        "devices": [
                                {
                                        "allow": false,
                                        "access": "rwm"
                                }
                        ]
                },
                "namespaces": [
                        {
                                "type": "pid"
                        },
                        {
                                "type": "network"
                        },
                        {
                                "type": "ipc"
                        },
                        {
                                "type": "uts"
                        },
                        {
                                "type": "mount"
                        }
                ],
                "maskedPaths": [
                        "/proc/kcore",
                        "/proc/latency_stats",
                        "/proc/timer_list",
                        "/proc/timer_stats",
                        "/proc/sched_debug",
                        "/sys/firmware",
                        "/proc/scsi"
                ],
                "readonlyPaths": [
                        "/proc/asound",
                        "/proc/bus",
                        "/proc/fs",
                        "/proc/irq",
                        "/proc/sys",
                        "/proc/sysrq-trigger"
                ]
        }
```

**1.1.5 Container Configuration file**

* ociVersion（必须）：对应的OCI标准版本
* root（必须）：根文件系统的位置
* mounts：需要挂载哪些目录到容器里面。如果是
* Linux平台，这里面必须要包含/proc、/sys，/dev/pts，/dev/shm这四个目录
* process：容器启动后执行什么命令
* hostname：容器的主机名，相关原理可参考UTS namespace \(CLONE\_NEWUTS\)
* platform（必须）：平台信息，如 amd64 + Linux
* linux：Linux平台的特殊配置，这里包含下面要介绍的Linux Container Configuration里面的内容
* hooks：配置容器运行生命周期中会调用的hooks，包括prestart、poststart和poststop，容器的生命周期见后面Runtime and Lifecycle介绍。
* annotations：容器的注释，相当于容器标签，key:value格式

**1.1.6 Linux Container Configuration**

namespaces: namespace相关的配置，相关原理可参考Namespace概述及这些namespace（UTS、IPC、mount、pid、network、user 1、user 2）。 uidMappings，gidMappings：配置主机和容器用户/组之间的对应关系，原理可参考user namespacedevices：设置哪些设备可以在容器内被访问到。除了这里指定的设备外，/dev/null、/dev/zero、/dev/full、/dev/random、/dev/urandom、/dev/tty、/dev/console（如果在process的配置里面启动terminal的话）和/dev/ptmx这些设备默认就能在容器内访问到，即runtime的实现需要默认将这些设备bind到容器内，dev/tty和/dev/ptmx的原理可以参考TTY/PTS概述 cgroupsPath：cgroup的路径，可参考Cgroup概述 resources：Cgroup中具体子项的配置，包括device whitelist, memory, cpu, blockIO, hugepageLimits, network\(net\_cls cgroup和net\_prio cgroup\), pid sintelRdt：和Intel Resource Director Technology有关 sysctl：调整容器运行时的kernel参数，主要是一些网络参数，因为每个network namespace都有自己的协议栈，所以可以修改自己协议栈的参数而不影响别人 seccomp：和安全相关的配置，见Seccomp rootfsPropagation：设置Propagation类型。可以参考Shared subtrees maskedPaths：设置容器内的哪些目录对用户不可见 readonlyPaths：设置容器内的哪些目录是只读的mount Label：和Selinux有关。

**1.1.7 Runtime and Lifecycle**

该规范主要定义了跟容器运行时相关的三部分内容，容器的状态、容器相关的操作以及容器的生命周期。

**容器的状态**

当查询容器的状态时，返回的状态里面至少包含如下信息：

```text
{
    "ociVersion": "0.2.0",
    "id": "oci-container1",
    "status": "running",
    "pid": 4422,
    "bundle": "/containers/redis",
    "annotations": {
        "myKey": "myValue"
    }
}
```

* `ociVersion`\(必须\)： 创建该容器时使用的OCI runtime的版本
* `id` \(必须\): 容器ID，本机全局唯一
* `status` \(必须\): 容器的运行时状态，包含如下状态:

  \`\`\`

  creating: 创建中

  created: 创建完成

  running: 运行中

  stopped: 运行结束

实现runtime时可以包含更多的状态，但不能改变这几个状态的含义

```text
* `pid` (容器是running状态时必须)： 容器内第一个进程在系统初始pid namespace中的pid，即在容器外面看到的pid
* `bundle` (REQUIRED)： bundle所在位置的绝对路径。bundle里面包含了容器的配置文件和根文件系统。
* `annotations`： 容器的注释，相当于容器标签，来自于容器的配置文件，key：value格式。

##### 容器相关的操作

该部分定义了一个符合runtime标准的实现（如runc）至少需要实现下面这些命令：

* `state`: 返回容器的状态，包含上面介绍的那些内容
* `create`： 创建容器，这一步执行完成后，容器创建完成，修改bundle中的config.json将不再对已创建的容器产生影响
* `start`： 启动容器，执行config.json中process部分指定的进程
* `kill`： 通过给容器发送信号来停止容器，信号的内容由kill命令的参数指定
* `delete`： 删除容器，如果容器正在运行中，则删除失败。删除操作会删除掉create操作时创建的所有内容。

##### 容器的生命周期


1. 执行命令`runc create`创建容器，参数中指定bundle的位置以及容器的ID，容器的状态变为creating
2. runc根据bundle中的config.json，准备好容器运行时需要的环境和资源，但不运行process中指定的进程，这步执行完成之后，表示容器创建成功，修改config.json将不再对创建的容器产生影响，这时容器的状态变成`created`。
4. 执行命令`runc start`启动容器
6. runc执行config.json中配置的prestart钩子
8. runc执行config.json中process指定的程序，这时容器状态变成了`running`
10. runc执行poststart钩子。
12. 容器由于某些原因退出，比如容器中的第一个进程主动退出，挂掉或者被kill掉等。这时容器状态变成了`stoped`
14. 执行命令`runc delete`删除容器，这时runc就会删除掉上面第2步所做的所有工作。
16. runc执行poststop钩子

#### 1.1.7 Linux Runtime


该规范是Linux平台上对Runtime and Lifecycle的补充，目前该规范很简单，只要求容器运行起来后，里面必须建立下面这些软连接：

```shell
# ls -l /dev/fd /dev/std*
lrwxrwxrwx    1 root     root            13 May  4 12:32 /dev/fd -> /proc/self/fd
lrwxrwxrwx    1 root     root            15 May  4 12:32 /dev/stderr -> /proc/self/fd/2
lrwxrwxrwx    1 root     root            15 May  4 12:32 /dev/stdin -> /proc/self/fd/0
lrwxrwxrwx    1 root     root            15 May  4 12:32 /dev/stdout -> /proc/self/fd/1
```

### 1.2 runC

`runC`是一个遵循OCI标准的用来运行容器的命令行工具\(CLI Tool\)，它也是一个Runtime的实现。尽管你可能对这个概念很陌生，但实际上，你的电脑上的docker底层正在使用它。

```text
[root@localhost ~]# docker info
Containers: 4
 Running: 3
 Paused: 0
 Stopped: 1
Images: 6
Server Version: 1.13.1
Storage Driver: overlay2
 Backing Filesystem: xfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: journald
Cgroup Driver: systemd
Plugins: 
 Volume: local
 Network: bridge host macvlan null overlay
Swarm: inactive
Runtimes: docker-runc runc                                 <-------------
Default Runtime: docker-runc                               <------------- 其实docker runc和runc区别不大，也可以改成runc
Init Binary: /usr/libexec/docker/docker-init-current
containerd version:  (expected: aa8187dbd3b7ad67d8e5e3a15115d3eef43a7ed1)
runc version: 9c3c5f853ebf0ffac0d087e94daef462133b69c7 (expected: 9df8b306d01f59d3a8029be411de015b7304dd8f)
init version: fec3683b971d9c3ef73f284f176672c44b448662 (expected: 949e6facb77383876aeff8a6944dde66b3089574)
Security Options:
 seccomp
  WARNING: You're not using the default seccomp profile
  Profile: /etc/docker/seccomp.json
 selinux
Kernel Version: 3.10.0-957.el7.x86_64
Operating System: CentOS Linux 7 (Core)
OSType: linux
Architecture: x86_64
Number of Docker Hooks: 3
CPUs: 4
Total Memory: 3.683 GiB
Name: localhost
ID: IY24:3256:QW2Z:GOBZ:K4RC:H22L:UOAL:SWEP:SBMN:HWBS:DDDA:W27U
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
Registries: docker.io (secure)
[root@localhost ~]#
```

RunC 是一个轻量级的工具，它是用来运行容器的，只用来做这一件事，并且这一件事要做好。我们可以认为它就是个命令行小工具，可以不用通过 docker 引擎，直接运行容器。事实上，runC 是标准化的产物，它根据 OCI 标准来创建和运行容器。而 OCI\(Open Container Initiative\)组织，旨在围绕容器格式和运行时制定一个开放的工业化标准。OCI 由 docker、coreos 以及其他容器相关公司创建于 2015 年，目前主要有两个标准文档：容器运行时标准 （runtime spec）和 容器镜像标准（image spec）。

RunC由Go语言实现，当前\(2018.12\)最新版本是v1.0.0-rc6,代码的结构可分为两大块,一是根目录下的go文件，对应各个runC命令，二是负责创建/启动/管理容器的libcontainer，可以说runC的本质都在libcontainer：

![74f5416c7f148bd968935f296194222c.png](en-resource://database/4496:1)

### runC的实现（代码级别）

**创建并运行容器**

我们就不在这里以查看代码的方式来研究runc，我们只需要知道整个流程即可：

1. 我们先记住这里的调用关系：

   ```text
     setupSpec(context)
     startContainer(context, spec, CT_ACT_CREATE, nil) 
       |- createContainer
          |- specconv.CreateLibcontainerConfig
          |- loadFactory(context)
             |- libcontainer.New(......)
          |- factory.Create(id, config)
       |- runner.run(spec.Process)
          |- newProcess(*config, r.init) 
          |- r.container.Start(process)
             |- c.createExecFifo()
             |- c.start(process)
                |- c.newParentProcess(process)
                |- parent.start()
   ```

2. 创建（create）的入口在create.go中，当我们输入`runc create mycontainerd`的时候，会开始执行注册Action并把参数存放在Context中。接着调用setupSpec函数。
3. `setupSpec(context)` :

   从命令行输入中找到-b 指定的 OCI bundle 目录，若没有此参数，则默认是当前目录。读取config.json文件，将其中的内容转换为Go的数据结构specs.Spec，该结构定义在文件 github.com/opencontainers/runtime-spec/specs-go/config.go,里面的内容都是OCI标准描述的。接着调用startContainer函数。

4. `startContainer`: 尝试创建启动容器，注意这里的第三个参数是 CT\_ACT\_CREATE, 表示仅创建容器。本文使用linux平台，因此实际调用的是 utils\_linux.go 中的startContainer\(\)。startContainer\(\)根据用户将用户输入的 id 和刚才的得到的 spec 作为输入，调用 createContainer\(\) 方法创建容器，再通过一个runner.run\(\)方法启动它。

   ```go
    func startContainer(context *cli.Context, spec *specs.Spec, action CtAct, criuOpts *libcontainer.CriuOpts) (int, error) {
        id := context.Args().First()

        container, err := createContainer(context, id, spec)

        r := &runner{
            container:       container,
            action:          action,
            init:            true,
            ......
        }
        return r.run(spec.Process)
    }
   ```

5. run\(\) 可分为两部分调用 newProcess\(\) 方法, a\) 用 spec.Process 创建 libcontainer.Process,注意第二个参数是 true ，表示新创建的 process 会作为新创建容器的第一个 process。b\) 根据 r.action 的值决定如何操作得到的 libcontainer.Process
6. 之后会根据配置文件进行namespace的创建以及资源配置并通过之前建立的管道来启动容器内所需要启动的应用进程了，当然在这之前会有很多的校验工作，这里就不在复述。

所以，我们可以看到runc通过调用系统中的功能去创建一个进程，并用namespace和cgroup把它限制起来就变成了我们的容器。

