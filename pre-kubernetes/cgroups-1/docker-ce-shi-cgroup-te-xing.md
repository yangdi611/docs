# Docker测试cgroup特性

## Docker测试cgroup特性

### 测试内存

1. 首先先用dockerfile建立一个docker image，安装stress压测工具：

   ```text
   [root@localhost stress]# cat Dockerfile 
   FROM centos:7.5.1804
   RUN yum install -y epel-release&&yum install -y stress
   [root@localhost stress]#
   ```

2. 然后进行build：

   \`\`\`shell

   \[root@localhost stress\]\# docker build -t centos/stress .

   Sending build context to Docker daemon 2.048 kB

   Step 1/2 : FROM centos:7.5.1804

   ---&gt; cf49811e3cdb

   Step 2/2 : RUN yum install -y epel-release&&yum install -y stress

   ---&gt; Using cache

   ---&gt; fea13fdcdb71

   Successfully built fea13fdcdb71

\[root@localhost stress\]\# docker images REPOSITORY TAG IMAGE ID CREATED SIZE stress/nginx v1 741f20c6591b 4 hours ago 556 MB centos/stress latest fea13fdcdb71 4 hours ago 349 MB

```text
3. 运行这个docker：
```shell
[root@localhost stress]# docker run -itd  --name stress1 -m 128M centos/stress
3de866c9da2f1f1a0ca844423c620f523b37700762ec9922c937f4e4ef689803
[root@localhost stress]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                   PORTS                  NAMES
3de866c9da2f        centos/stress       "/bin/bash"              4 seconds ago       Up 3 seconds                                    stress1
a08c8a72ac21        centos/stress       "stress1"                19 seconds ago      Created                                         awesome_hodgkin
93c455a30734        stress/nginx:v1     "/bin/bash"              4 hours ago         Up 2 hours               0.0.0.0:8091->80/tcp   stresstest
bf64ad9ff8c7        mysql               "docker-entrypoint..."   4 hours ago         Exited (0) 4 hours ago                          mysql3
292d858cba4c        mysql:latest        "docker-entrypoint..."   8 days ago          Exited (0) 4 days ago                           mysql2
09d933547192        wordpress           "docker-entrypoint..."   8 days ago          Exited (0) 8 days ago                           quirky_pasteur
8b21c01a0b5c        nginx               "nginx -g 'daemon ..."   8 days ago          Exited (0) 8 days ago                           admiring_poincare
5b2a9996c107        mysql:latest        "docker-entrypoint..."   8 days ago          Exited (0) 4 days ago                           mysql
ee5d068c9a8c        mysql               "docker-entrypoint..."   8 days ago          Exited (1) 8 days ago                           mysql-wordpress
6ac02f99cd94        mysql               "docker-entrypoint..."   8 days ago          Exited (1) 8 days ago                           jovial_shaw
[root@localhost stress]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
3de866c9da2f        centos/stress       "/bin/bash"         5 seconds ago       Up 4 seconds                               stress1
93c455a30734        stress/nginx:v1     "/bin/bash"         4 hours ago         Up 2 hours          0.0.0.0:8091->80/tcp   stresstest
```

1. 通过bash登入到这个docker中查看这个容器是否可用：

   ```text
   [ststester@localhost ~]$ id
   uid=1002(ststester) gid=1003(docker) groups=1003(docker),1002(ststester)
   [ststester@localhost ~]$ docker exec -it 3de866c9da2f bash
   [root@3de866c9da2f /]# id
   uid=0(root) gid=0(root) groups=0(root)
   [root@3de866c9da2f /]# 
   [root@3de866c9da2f /]# 
   [root@3de866c9da2f /]# 
   [root@3de866c9da2f /]# 
   [root@3de866c9da2f /]# 
   [root@3de866c9da2f /]# ll
   total 0
   lrwxrwxrwx   1 root root   7 May 31  2018 bin -> usr/bin
   drwxr-xr-x   5 root root 360 Aug 16 06:17 dev
   drwxr-xr-x   1 root root  66 Aug 16 06:17 etc
   drwxr-xr-x   2 root root   6 Apr 11  2018 home
   lrwxrwxrwx   1 root root   7 May 31  2018 lib -> usr/lib
   lrwxrwxrwx   1 root root   9 May 31  2018 lib64 -> usr/lib64
   drwxr-xr-x   2 root root   6 Apr 11  2018 media
   drwxr-xr-x   2 root root   6 Apr 11  2018 mnt
   drwxr-xr-x   2 root root   6 Apr 11  2018 opt
   dr-xr-xr-x 154 root root   0 Aug 16 06:17 proc
   dr-xr-x---   1 root root  18 Aug 16 01:55 root
   drwxr-xr-x   1 root root  21 Aug 16 01:56 run
   lrwxrwxrwx   1 root root   8 May 31  2018 sbin -> usr/sbin
   drwxr-xr-x   2 root root   6 Apr 11  2018 srv
   dr-xr-xr-x  13 root root   0 Aug 16 03:23 sys
   drwxrwxrwt   1 root root   6 Aug 16 01:56 tmp
   drwxr-xr-x   1 root root  28 May 31  2018 usr
   drwxr-xr-x   1 root root  52 May 31  2018 var
   [root@3de866c9da2f /]#
   ```

2. 进入容器后，运行压力测试，由于之前我们限制内存的使用是128M，所以我们用压测工具stress压到150M看看会有什么效果：

   ```text
   [root@93c455a30734 /]# stress --vm 1 --vm-bytes 150M
   stress: info: [33] dispatching hogs: 0 cpu, 0 io, 1 vm, 0 hdd
   ```

   然后在host上观察内存的使用情况：

   \`\`\`shell

   Path Tasks %CPU Memory Input/s Output/s

/ 87 84.1 674.3M - - /system.slice - 81.1 180.7M - - /system.slice/docker-28d34095dfa92c4c2e3564f52508f054b3bb562c53e1205a162e1e1a2862e51b.scope 4 80.8 127.8M - - ...

```text
结论，我们看到内存已经被限制在了128M，并没有继续增加，这跟之前我们设想的情况一致，资源被限制了。

同样，我们在cgroup的虚拟目录中可以看到：
```shell
[root@localhost docker-28d34095dfa92c4c2e3564f52508f054b3bb562c53e1205a162e1e1a2862e51b.scope]# pwd
/sys/fs/cgroup/memory/system.slice/docker-28d34095dfa92c4c2e3564f52508f054b3bb562c53e1205a162e1e1a2862e51b.scope
[root@localhost docker-28d34095dfa92c4c2e3564f52508f054b3bb562c53e1205a162e1e1a2862e51b.scope]# cat memory.stat
cache 0
rss 134172672
rss_huge 0
mapped_file 0
swap 24797184
pgpgin 1485506050
pgpgout 1634831439
pgfault 1582111240
pgmajfault 102324711
inactive_anon 67219456
active_anon 66953216
inactive_file 0
active_file 0
unevictable 0
hierarchical_memory_limit 134217728
hierarchical_memsw_limit 268435456
total_cache 0
total_rss 134172672
total_rss_huge 0
total_mapped_file 0
total_swap 24797184
total_pgpgin 1485506050
total_pgpgout 1634831439
total_pgfault 1582111240
total_pgmajfault 102324711
total_inactive_anon 67219456
total_active_anon 66953216
total_inactive_file 0
total_active_file 0
total_unevictable 0
```

### CPU测试

**第一种方式，通过修改cpu共享权重来限制cpu的使用**

1. 我们建立三个带有stress工具的docker，并运行3个docker，stress1和stress2都对cpushare不做限制-默认是1024，对stress3进行限制cpushare到512：

   \`\`\`shell

   \[root@localhost ~\]\# docker run -itd --name stress3 -m 128M -c=512 centos/stress

\[root@localhost ~\]\# docker ps CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES 853359ceee2a centos/stress "/bin/bash" 3 minutes ago Up 3 minutes stress3 28d34095dfa9 centos/stress "/bin/bash" 4 hours ago Up 4 hours stress2 3de866c9da2f centos/stress "/bin/bash" 5 hours ago Up 4 hours stress1

```text
2. 我们进入stress1到docker中运行stress，把cpu压起来：
```shell
[root@3de866c9da2f /]# stress --cpu 8
stress: info: [56] dispatching hogs: 8 cpu, 0 io, 0 vm, 0 hdd
```

1. 我们通过systemd-cgtop可以看到目前stress1占用100%：

   \`\`\`shell

/ 90 100.0 568.0M - - /system.slice - 98.7 67.2M - - /system.slice/docker-3de866c9da2f1f1a0ca844423c620f523b37700762ec9922c937f4e4ef689803.scope 11 98.6 2.1M -

```text
4. 然后进入到stress2中运行stress，并观察cpu使用率，可以看到两个容器都占有50%的cpu：
```shell
/                                                                                                                                     90   99.9   567.8M        -        -
/system.slice                                                                                                                          -   98.5    67.4M        -        -
/system.slice/docker-28d34095dfa92c4c2e3564f52508f054b3bb562c53e1205a162e1e1a2862e51b.scope                                           13   49.4     2.7M        -        -
/system.slice/docker-3de866c9da2f1f1a0ca844423c620f523b37700762ec9922c937f4e4ef689803.scope                                           11   49.0     2.1M        -        -
```

1. 我们再进入到stress3中运行stress，观察到的cpu使用率如下：

   \`\`\`shell

/ 90 100.0 568.1M - - /system.slice - 98.5 67.7M - - /system.slice/docker-28d34095dfa92c4c2e3564f52508f054b3bb562c53e1205a162e1e1a2862e51b.scope 13 39.4 2.7M - - /system.slice/docker-3de866c9da2f1f1a0ca844423c620f523b37700762ec9922c937f4e4ef689803.scope 11 39.3 2.1M - - /system.slice/docker-853359ceee2aac474e34309133d5f610fa4a8f254f25d84c373d22adef989923.scope 11 19.8 1.0M - -

```text
这时，我们看到stress1和stress2占用cpu 80%，stress3占用20% cpu使用率。这和我们设想的是一样的：第一个和第二个容器的权重是默认权重1024，而第三个容器的cpu使用权重是512，所以，他们的使用率是2:2:1，即40%：40%：20%。有兴趣的同事可以自行去看一下`/sys/fs/cgroup/cpu/systemd/容器ID.scope/`下的cpu配置文件。

#### 第二种方式，通过修改占用物理cpu核心（通过频率的计算）
一共有两个可调参数来限制cpu的使用核数（或者核数的百分比），一个是cpu.cfs_quota_us，一个是cpu.cfs_period_us，这样限制的好处是所有cgroup都被限制死在一个百分比或核数，坏处是无法在系统空闲的时候充分利用cpu的性能。
1. 我们先通过修改参数启动stress1，占用40%的cpu，
```shell
[root@localhost cpu]# docker run -itd --cpu-period=40000 --cpu-quota=100000 --name=stress1 centos/stress
80b7c0afe188f734b05e05c9ac27141aa7a3a4dcd3b44e3776143a3c40860e55
[root@localhost cpu]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
80b7c0afe188        centos/stress       "/bin/bash"         2 seconds ago       Up 2 seconds                            stress1
```

cpu\_period=40000 cpu\_quota=100000表示在100ms为周期的情况中占用40%的cpu时间。

1. 我们启动容器中的stress对cpu进行压测，然后来看一下这个容器的cpu占用率：

   ```text
   /                                                                                                                                     88   42.8   554.1M        -        -
   /system.slice                                                                                                                          -   41.3    61.2M        -        -
   /system.slice/docker-5903d0f3a3ec1427483c17373412384fd6e027e2ab5247444a55b8770d72c175.scope                                           11   41.2     1.0M        -        -
   ```

   由于这台主机的cpu只有1c，所以我们看到cpu的使用被限制在了40%，这和我们预想的一样。

2. 我们启动第二个容器stress2和stress3，分别限制cpu时间到每100ms占用40ms和20ms的cpu时间，我们再来看一下cpu的占用率：

   ```text
   /                                                                                                                                     90  100.0   570.0M        -        -
   /system.slice                                                                                                                          -   98.6    68.0M        -        -
   /system.slice/docker-5903d0f3a3ec1427483c17373412384fd6e027e2ab5247444a55b8770d72c175.scope                                           11   39.1     1.0M        -        -
   /system.slice/docker-cf6a54ed2f05632ab3a1ec27c5a0aa3ce41a00481d5a74614e4de16e7b629969.scope                                           11   39.1     1.0M        -        -
   /system.slice/docker-2011c5a1545a219322fdc671d59e028f21bc4afb0d0736759eaf3130c6965aa0.scope                                           11   20.3     1.0M        -        -
   ```

   正如我们设想的一样，第一个和第二个容器都是40%占用率，第三个容器占用20%的cpu。

3. 那么我们如果修改第三个容器到占有40% cpu时间的话会发生什么？

   ```text
   /                                                                                                                                     90   99.9   570.0M        -        -
   /system.slice                                                                                                                          -   98.7    68.2M        -        -
   /system.slice/docker-cf6a54ed2f05632ab3a1ec27c5a0aa3ce41a00481d5a74614e4de16e7b629969.scope                                           11   32.9     1.0M        -        -
   /system.slice/docker-5903d0f3a3ec1427483c17373412384fd6e027e2ab5247444a55b8770d72c175.scope                                           11   32.8     1.0M        -        -
   /system.slice/docker-7d7a49c03fc7ba31bf129c09c609d27028c38c356c9d501b1f04e0eac47aa455.scope                                           11   32.8     1.0M        -        -
   ```

   CPU的时间会被平均分配，这是因为每个容器都会对cpu时间进行争抢但由于他们对cpu的争抢权是一样的，所以他们得到的cpu时间是一样的。有兴趣的同事可以自行去看一下`/sys/fs/cgroup/cpu/systemd/容器ID.scope/`下的cpu配置文件看看是否有什么变化。

### 磁盘IO配额的限制

**device-write-bps**

1. xxx

   \`\`\`shell

   \[root@localhost cgroup\]\# docker run -itd --name stress1 --device-write-bps /dev/sda:1m centos/stress

   8e61e578434654093590f0d94d0d0846d78d204d47a9616ba8714fbefabd0e5c

   \[root@localhost cgroup\]\# docker ps

   CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES

   8e61e5784346 centos/stress "/bin/bash" 6 seconds ago Up 5 seconds stress1

   \[root@localhost cgroup\]\# docker exec stress1 bash

   ^C

   \[root@localhost cgroup\]\# docker -it exec stress1 bash

   unknown shorthand flag: 'i' in -it

   \[root@localhost cgroup\]\# docker exec -it stress1 bash

   \[root@8e61e5784346 /\]\#

   \[root@8e61e5784346 /\]\#

   \[root@8e61e5784346 /\]\# time dd if=/dev/zero of=test.out bs=1M count=1024 oflag=direct

   ^C179+0 records in

   179+0 records out

   187695104 bytes \(188 MB\) copied, 179.002 s, 1.0 MB/s

real 2m59.003s user 0m0.002s sys 0m0.036s

```text
可以看到速率被正确的限制到了1MB，跟设想的一样。
2. 我们查看这个容器的cgroup的blkio配置：
```shell
[root@localhost docker-8e61e578434654093590f0d94d0d0846d78d204d47a9616ba8714fbefabd0e5c.scope]# cat blkio.throttle.write_bps_device
8:0 1048576

brw-rw---- 1 root disk      8,   0 Aug 16 11:20 sda
```

8:0是sda的设备名，后面就是我们设置的1MB/s的速率限制。

