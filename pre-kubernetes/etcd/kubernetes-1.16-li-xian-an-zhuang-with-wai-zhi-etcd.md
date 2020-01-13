# Kubernetes 1.16 离线安装 with 外置etcd

系统环境：

| 系统环境： |  |
| :--- | :--- |
| system | CentOS Linux release 7.6.1810 \(Core\) |
| kernel | 3.10.0-957.el7.x86\_64 |
| ESXI | 6.5 |

版本说明：

> _Kubernets 1.16.3 版_

| 名称 | 版本 \(or TAG\) | 安装方式 |
| :--- | :--- | :--- |
| kubeadm | 1.16.3 | yum |
| kubelet | 1.16.3 | yum |
| kubectl | 1.16.3 | yum |
| etcd | 3.3.18 | binary |
| etcdctl | 3.3.18 | binary |
| k8s.gcr.io/kube-apiserver:v1.16.3 | v1.16.3 | harbor private registry |
| k8s.gcr.io/kube-controller-manager | v1.16.3 | harbor private registry |
| k8s.gcr.io/kube-scheduler | v1.16.3 | harbor private registry |
| k8s.gcr.io/kube-proxy | 3.1 | harbor private registry |
| k8s.gcr.io/etcd | 3.3.15-0 | harbor private registry |
| k8s.gcr.io/coredns | 1.6.2 | harbor private registry |
| calico/node | v3.10.1 | harbor private registry |
| calico/cni | v3.10.1 | barbor private registry |
| calico/pod2daemon-flexvol | v3.10.1 | harbor private registry |

主机环境：

> _calico随机_

| hostname | ip | purpose |
| :--- | :--- | :--- |
| bootstrap | 10.0.209.140 | ansible |
| master1-143.vmwlab | 10.0.209.143 | scheduler, apiserver, controller, keepalived |
| master2-144.vmwlab | 10.0.209.144 | scheduler, apiserver, controller, keepalived |
| master3-145.vmwlab | 10.0.209.145 | scheduler, apiserver, controller, keepalived |
| etcd1-146.vmwlab | 10.0.209.146 | etcd in service |
| etcd2-147.vmwlab | 10.0.209.147 | etcd in service |
| etcd3-148.vmwlab | 10.0.209.148 | etcd in service |
| worker1-155.vmwlab | 10.0.209.155 | worker1 |
| worker2-156.vmwlab | 10.0.209.156 | worker2 |
| worker3-157.vmwlab | 10.0.209.157 | worker3 |
| worker4-158.vmwlab | 10.0.209.158 | worker3 |

服务拓扑：

img here

### Kubernets 1.16 集群安装（内网环境/外置etcd）

#### 介质准备

1. 复制一份yum源到自建的yum源里面去，这里使用ali的yum源： [https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86\_64/Packages/](https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/Packages/)
2. 下载etcd 二进制文件：[https://github.com/etcd-io/etcd/releases](https://github.com/etcd-io/etcd/releases)
3. 下载kubernetes的镜像并更改tag后上传至私有镜像库，如Harbor等，参考：[https://github.com/yangdi611/google-containers](https://github.com/yangdi611/google-containers)
4. 剩下的工具，确保你的源里面有：Docker、Ansible

#### 准备bootstrap节点

> 官方文档并没有说需要这个节点，这里说说为什么要准备bootstrap节点，因为如果想要安装一个比较纯净的k8s环境和进行一些测试都可以用这个节点来进行，可以是一个同网段的机器也可以是你自己笔记本上面的一个虚机，只要和cluster里面的机器通就可以。

1. 在节点上安装ansible： `yum install -y ansible`
2. 编辑ansible的hosts文件：`cat /etc/ansible/hosts`

   ```bash
   ...
   [master]
   10.0.209.143
   10.0.209.144
   10.0.209.145
   [etcd]
   10.0.209.146
   10.0.209.147
   10.0.209.148
   [worker]
   10.0.209.155
   10.0.209.156
   10.0.209.157
   10.0.209.158
   [preserve]
   10.0.209.149
   10.0.209.150
   10.0.209.151
   10.0.209.152
   10.0.209.153
   10.0.209.154
   10.0.209.159
   10.0.209.160
   [cluster:children]
   master
   etcd
   worker
   ```

3. 查看编辑ansible的配置文件，如：更改ssh端口号等：`cat /etc/ansible/ansible.cfg`

```bash
# config file for ansible -- https://ansible.com/
# ===============================================


[defaults]

# some basic default values...

#inventory      = /etc/ansible/hosts
#library        = /usr/share/my_modules/
#module_utils   = /usr/share/my_module_utils/
#remote_tmp     = ~/.ansible/tmp
#local_tmp      = ~/.ansible/tmp
#plugin_filters_cfg = /etc/ansible/plugin_filters.yml
#forks          = 5
#poll_interval  = 15
#sudo_user      = root
#ask_sudo_pass = True
#ask_pass      = True
#transport      = smart
remote_port    = 22
#module_lang    = C
#module_set_locale = False
...
```

1. 对bootstrap节点上的ssh进行免登录授权：`ssh-keygen` 一路确认即可，然后将生成的密匙拷贝到其他服务器上：`ssh-copy-id <ip>`
2. 进行用ansible进行ping测试：`ansible cluster -m ping`来检测通断性。

#### 准备cluster节点

> _如下都使用ansible-playbook来进行推送，如无备注都是ansible使用的yml文件，方便环境准备。其实可以写在一个文件中就可以，但是为了对安装所需要的步骤更直观，这里先分开执行。_

1. 修改各节点的hosts文件：

```yaml
- hosts: cluster
  tasks:
    - name: Add hosts to /etc/hosts
      blockinfile:
        path: /etc/hosts
        block: |
          #Vmware Lab Kubernetes 1.16 Cluster
          10.0.209.143  master1-143.vmwlab
          10.0.209.144  master2-144.vmwlab
          10.0.209.145  master3-145.vmwlab
          10.0.209.146  etcd1-146.vmwlab
          10.0.209.147  etcd2-147.vmwlab
          10.0.209.148  etcd3-148.vmwlab
          10.0.209.155  worker1-155.vmwlab
          10.0.209.156  worker2-156.vmwlab
          10.0.209.157  worker3-157.vmwlab
          10.0.209.158  worker4-158.vmwlab
```

1. 增加yum源，修改配置文件：

```yaml
- hosts: cluster
  tasks:
    - name: Add repos to yum
      blockinfile:
        path: /etc/yum.repos.d/local.repo
        block: |
          [epel]
          name=epel
          baseurl=http://10.0.209.10/Packages/epel
          enabled=1
          gpgcheck=0

          [kubernetes1.16]
          name=kubernetes1.16
          baseurl=http://10.0.209.10/Packages/kubernetes
          enabled=1
          gpgcheck=0
```

1. 修改SSH端口，用默认的也无所谓，改完之后记得改ansible的配置文件：

```yaml
- hosts: cluster
  tasks:
  - name: Add ssh port to /etc/ssh/sshd_config
    lineinfile:
      path: /etc/ssh/sshd_config
      regexp: '^server'
      insertafter: '^#'
      line: Port 58888
```

1. 停止firewalld：

```yaml
- hosts: cluster
  tasks:
    - name: stop and disable firwalld
      service:
        name: firewalld
        state: stopped
        enabled: no
```

1. 关闭swap：

```yaml
- hosts: cluster
  tasks:
    - name: turn swap off
      command: swapoff -a
```

1. 设置iptables 到legacy模式：

```yaml
- hosts: cluster
  tasks:
    - name: update iptables to legacy mode to compitable with kubeadm
      command: update-alternatives --set iptables /usr/sbin/iptables-legacy

- hosts: cluster
  tasks:
    - name: update iptables to legacy mode to compitable with kubeadm
      command: update-alternatives --set ip6tables /usr/sbin/iptables-legacy

- hosts: cluster
  tasks:
    - name: update iptables to legacy mode to compitable with kubeadm
      command: update-alternatives --set arptables /usr/sbin/iptables-legacy

- hosts: cluster
  tasks:
    - name: update iptables to legacy mode to compitable with kubeadm
      command: update-alternatives --set ebtables /usr/sbin/iptables-legacy
```

1. 安装docker、kubeadm、kubectl、kubelet：

```yaml
- hosts: cluster
  tasks:
    - name: install docker
      yum:
        name: docker-ce
        state: latest

    - name: install kubeadm kubectl kubelet
      yum:
        name: "{{packages}}"
      vars:
        packages:
        - kubeadm
        - kubectl
        - kubelet

    - name: start kubelet
      service:
        name: kubelet
        enabled: yes
```

1. 修改docker配置文件，将私库加入到配置文件：

```yaml
- hosts: cluster
  tasks:
    - name: Add Docker Daemon json
      blockinfile:
        path: /etc/docker/daemon.json
        create: yes
        marker: ""
        block: |
          {
            "insecure-registries":["harbor.io", "k8s.gcr.io", "gcr.io", "quay.io", "10.0.209.120"],
            "exec-opts": ["native.cgroupdriver=systemd"],
            "log-driver": "json-file",
            "log-opts": {
            "max-size": "100m"
            },
            "storage-driver": "overlay2"
          }
```

1. 增加ntp服务器并同步：

```yaml
- hosts: cluster
  tasks:
  - name: Delete origin NTP server
    lineinfile:
      path: /etc/chrony.conf
      regexp: '^server'
      insertbefore: '^#'
      state: absent
  - name: Add NTP server to config file /etc/chrony.conf
    lineinfile:
      path: /etc/chrony.conf
      regexp: '^server'
      insertafter: '^#'
      line: server 10.0.209.111 iburst
  - name: Restart Chrony service
    service:
      name: chronyd
      state: restarted
```

1. 增加快速命令：

```yaml
- hosts: cluster
  tasks:
    - name: install bash complete
      yum:
        name: bash-completion
        state: latest

    - name: Source complettion
      shell: source /usr/share/bash-completion/bash_completion

    - name: Source kubectl
      shell: source <(kubectl completion bash)

    - name: Source kubectl
      shell: echo "source <(kubectl completion bash)" >> ~/.bashrc
```

1. 将master上安装keepalived：

```yaml
- hosts: master
  tasks:
    - name: install KeepAlive
      yum:
        name: keepalived
        state: latest
```

1. 修改ulimit：

```yaml
- hosts: cluster
  tasks:
    - name: Add config to /etc/security/limits.conf
      blockinfile:
        path: /etc/security/limits.conf
        block: |
          * soft nofile 65536
          * hard nofile 65536
          * soft nproc 65536
          * hard nproc 65536
          * soft memlock unlimited
          * hard memlock unlimited
```

1. 修改iptables的配置：

```yaml
- hosts: cluster
  tasks:
    - name: touch /etc/sysctl.d/k8s.conf
      file:
        path: /etc/sysctl.d/k8s.conf
        state: touch
    - name: make sure not to bypass Iptables
      blockinfile:
        path: /etc/sysctl.d/k8s.conf
        block: |
          net.bridge.bridge-nf-call-ip6tables = 1
          net.bridge.bridge-nf-call-iptables = 1
    - name: sync sysctl
      command: sysctl --system
    - name: make sure br_netfilter module is loaded
      command: modprobe br_netfilter
```

#### 安装Etcd cluster（external）

1. 通过github下载运行文件：

   [https://github.com/etcd-io/etcd ](https://github.com/etcd-io/etcd)

2. 上传到主机上之后进行复制：

   ```bash
   mv etcd* /usr/local/bin/
   ```

3. 建立etcd服务：

```bash
[Unit] Description=Etcd Server After=network.target
[Service] Type=simple WorkingDirectory=/var/lib/etcd/ EnvironmentFile=/etc/etcd/etcd.conf ExecStart=/usr/local/bin/etcd Restart=always
[Install] WantedBy=multi-user.target
```

4. 编辑各个节点的etcd服务

etcd1:

```bash
mkdir /etc/etcd/
vim /etc/etcd/etcd.conf

#[Member]
ETCD_NAME="etcd1"
ETCD_DATA_DIR="/var/lib/etcd/default.etcd"
ETCD_LISTEN_PEER_URLS="https://10.0.209.146:2380"
ETCD_LISTEN_CLIENT_URLS="https://10.0.209.146:2379,http://127.0.0.1:2379"

#[Clustering]
ETCD_INITIAL_ADVERTISE_PEER_URLS="https://10.0.209.146:2380"
ETCD_ADVERTISE_CLIENT_URLS="https://10.0.209.146:2379"
ETCD_INITIAL_CLUSTER="etcd1=https://10.0.209.146:2380,etcd2=https://10.0.209.147:2380,etcd3=https://10.0.209.148:2380"
ETCD_INITIAL_CLUSTER_TOKEN="vmwlab-etcd-cluster"
ETCD_INITIAL_CLUSTER_STATE="new"

#[Security]
ETCD_CERT_FILE="/etc/kubernetes/pki/etcd/server.crt"
ETCD_KEY_FILE="/etc/kubernetes/pki/etcd/server.key"
ETCD_TRUSTED_CA_FILE="/etc/kubernetes/pki/etcd/ca.crt"
ETCD_CLIENT_CERT_AUTH="true"
ETCD_PEER_CERT_FILE="/etc/kubernetes/pki/etcd/peer.crt"
ETCD_PEER_KEY_FILE="/etc/kubernetes/pki/etcd/peer.key"
ETCD_PEER_TRUSTED_CA_FILE="/etc/kubernetes/pki/etcd/ca.crt"
ETCD_PEER_CLIENT_CERT_AUTH="true"
```

etcd2:

```bash
mkdir /etc/etcd/
vim /etc/etcd/etcd.conf

#[Member]
ETCD_NAME="etcd2"
ETCD_DATA_DIR="/var/lib/etcd/default.etcd"
ETCD_LISTEN_PEER_URLS="https://10.0.209.147:2380"
ETCD_LISTEN_CLIENT_URLS="https://10.0.209.147:2379,http://127.0.0.1:2379"

#[Clustering]
ETCD_INITIAL_ADVERTISE_PEER_URLS="https://10.0.209.147:2380"
ETCD_ADVERTISE_CLIENT_URLS="https://10.0.209.147:2379"
ETCD_INITIAL_CLUSTER="etcd1=https://10.0.209.146:2380,etcd2=https://10.0.209.147:2380,etcd3=https://10.0.209.148:2380"
ETCD_INITIAL_CLUSTER_TOKEN="vmwlab-etcd-cluster"
ETCD_INITIAL_CLUSTER_STATE="new"

#[Security]
ETCD_CERT_FILE="/etc/kubernetes/pki/etcd/server.crt"
ETCD_KEY_FILE="/etc/kubernetes/pki/etcd/server.key"
ETCD_TRUSTED_CA_FILE="/etc/kubernetes/pki/etcd/ca.crt"
ETCD_CLIENT_CERT_AUTH="true"
ETCD_PEER_CERT_FILE="/etc/kubernetes/pki/etcd/peer.crt"
ETCD_PEER_KEY_FILE="/etc/kubernetes/pki/etcd/peer.key"
ETCD_PEER_TRUSTED_CA_FILE="/etc/kubernetes/pki/etcd/ca.crt"
ETCD_PEER_CLIENT_CERT_AUTH="true"
```

etcd3:

```bash
mkdir /etc/etcd/
vim /etc/etcd/etcd.conf

#[Member]
ETCD_NAME="etcd1"
ETCD_DATA_DIR="/var/lib/etcd/default.etcd"
ETCD_LISTEN_PEER_URLS="https://10.0.209.148:2380"
ETCD_LISTEN_CLIENT_URLS="https://10.0.209.148:2379,http://127.0.0.1:2379"

#[Clustering]
ETCD_INITIAL_ADVERTISE_PEER_URLS="https://10.0.209.148:2380"
ETCD_ADVERTISE_CLIENT_URLS="https://10.0.209.148:2379"
ETCD_INITIAL_CLUSTER="etcd1=https://10.0.209.146:2380,etcd2=https://10.0.209.147:2380,etcd3=https://10.0.209.148:2380"
ETCD_INITIAL_CLUSTER_TOKEN="vmwlab-etcd-cluster"
ETCD_INITIAL_CLUSTER_STATE="new"

#[Security]
ETCD_CERT_FILE="/etc/kubernetes/pki/etcd/server.crt"
ETCD_KEY_FILE="/etc/kubernetes/pki/etcd/server.key"
ETCD_TRUSTED_CA_FILE="/etc/kubernetes/pki/etcd/ca.crt"
ETCD_CLIENT_CERT_AUTH="true"
ETCD_PEER_CERT_FILE="/etc/kubernetes/pki/etcd/peer.crt"
ETCD_PEER_KEY_FILE="/etc/kubernetes/pki/etcd/peer.key"
ETCD_PEER_TRUSTED_CA_FILE="/etc/kubernetes/pki/etcd/ca.crt"
ETCD_PEER_CLIENT_CERT_AUTH="true"
```

1. 通过脚本生成配置文件CA认证文件：

```bash
# Update HOST0, HOST1, and HOST2 with the IPs or resolvable names of your hosts
export HOST0=10.0.209.146
export HOST1=10.0.209.147
export HOST2=10.0.209.148

# Create temp directories to store files that will end up on other hosts.
mkdir -p /tmp/${HOST0}/ /tmp/${HOST1}/ /tmp/${HOST2}/

ETCDHOSTS=(${HOST0} ${HOST1} ${HOST2})
NAMES=("etcd1" "etcd2" "etcd3")

for i in "${!ETCDHOSTS[@]}"; do
HOST=${ETCDHOSTS[$i]}
NAME=${NAMES[$i]}
cat << EOF > /tmp/${HOST}/kubeadmcfg.yaml
apiVersion: "kubeadm.k8s.io/v1beta2"
kind: ClusterConfiguration
etcd:
    local:
        serverCertSANs:
        - "${HOST}"
        peerCertSANs:
        - "${HOST}"
        extraArgs:
            initial-cluster: ${NAMES[0]}=https://${ETCDHOSTS[0]}:2380,${NAMES[1]}=https://${ETCDHOSTS[1]}:2380,${NAMES[2]}=https://${ETCDHOSTS[2]}:2380
            initial-cluster-state: new
            name: ${NAME}
            listen-peer-urls: https://${HOST}:2380
            listen-client-urls: https://${HOST}:2379
            advertise-client-urls: https://${HOST}:2379
            initial-advertise-peer-urls: https://${HOST}:2380
EOF
done
```

1. 在HOST0上生成CA认证文件：

```bash
kubeadm init phase certs etcd-ca
```

> 如果你已经有了这些文件则将这些文件拷贝到如下目录 /etc/kubernetes/pki/etcd/ca.crt /etc/kubernetes/pki/etcd/ca.key

1. 生成所有节点的认证证书：

```bash
kubeadm init phase certs etcd-server --config=/tmp/${HOST2}/kubeadmcfg.yaml
kubeadm init phase certs etcd-peer --config=/tmp/${HOST2}/kubeadmcfg.yaml
kubeadm init phase certs etcd-healthcheck-client --config=/tmp/${HOST2}/kubeadmcfg.yaml
kubeadm init phase certs apiserver-etcd-client --config=/tmp/${HOST2}/kubeadmcfg.yaml
cp -R /etc/kubernetes/pki /tmp/${HOST2}/
# cleanup non-reusable certificates
find /etc/kubernetes/pki -not -name ca.crt -not -name ca.key -type f -delete

kubeadm init phase certs etcd-server --config=/tmp/${HOST1}/kubeadmcfg.yaml
kubeadm init phase certs etcd-peer --config=/tmp/${HOST1}/kubeadmcfg.yaml
kubeadm init phase certs etcd-healthcheck-client --config=/tmp/${HOST1}/kubeadmcfg.yaml
kubeadm init phase certs apiserver-etcd-client --config=/tmp/${HOST1}/kubeadmcfg.yaml
cp -R /etc/kubernetes/pki /tmp/${HOST1}/
find /etc/kubernetes/pki -not -name ca.crt -not -name ca.key -type f -delete

kubeadm init phase certs etcd-server --config=/tmp/${HOST0}/kubeadmcfg.yaml
kubeadm init phase certs etcd-peer --config=/tmp/${HOST0}/kubeadmcfg.yaml
kubeadm init phase certs etcd-healthcheck-client --config=/tmp/${HOST0}/kubeadmcfg.yaml
kubeadm init phase certs apiserver-etcd-client --config=/tmp/${HOST0}/kubeadmcfg.yaml
# No need to move the certs because they are for HOST0

# clean up certs that should not be copied off this host
find /tmp/${HOST2} -name ca.key -type f -delete
find /tmp/${HOST1} -name ca.key -type f -delete
```

1. 在HOST0中在每个节点上将pki拷贝到相应的目录中去

```bash
USER=root
 HOST=${HOST1}
 scp -r -P 58888 /tmp/${HOST}/* ${USER}@${HOST}:
 ssh ${USER}@${HOST} -p 58888
 USER@HOST $ sudo -Es
 root@HOST $ chown -R root:root pki
 root@HOST $ mv pki /etc/kubernetes/
```

1. 在各个节点上启动并设置开机启动etcd：

```bash
systemctl enable etcd
systemctl start etcd
```

1. 检查etcd集群状态：

```bash
etcdctl \
--cert-file /etc/kubernetes/pki/etcd/peer.crt \
--key-file /etc/kubernetes/pki/etcd/peer.key \
--ca-file /etc/kubernetes/pki/etcd/ca.crt \
--endpoints https://10.0.209.146:2379 cluster-health
```

1. 删除掉没用kubadmcfg.yaml

#### 安装Master节点

**配置keepalived和ipvs**

1. 在各个节点上配置keepalived， /etc/keepalived/keepalived.conf：

   master1

```javascript
! Configuration File for keepalived

global_defs {
   router_id LVS_K8s_Cluster1
}

vrrp_script CheckK8sMaster {
    script "curl -k https://10.0.209.142:6443"
    interval 3
    timeout 5
    fall 2
    rise 2
}

vrrp_instance VI_1 {
    state MASTER
    interface ens32
    virtual_router_id 51
    priority 100
    advert_int 1
    master_src_ip 10.0.209.143
    nopreempt
    authentication {
        auth_type PASS
        auth_pass ByronYangdiCluster1OK
    }
    unicast_peer {
        10.0.209.144
        10.0.209.145
    }
    virtual_ipaddress {
        10.0.209.142/24
    }
    track_script {
        CheckK8sMaster
    }
}
```

master2

```javascript
! Configuration File for keepalived

global_defs {
   router_id LVS_K8s_Cluster1
}

vrrp_script CheckK8sMaster {
    script "curl -k https://10.0.209.142:6443"
    interval 3
    timeout 5
    fall 2
    rise 2
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens32
    virtual_router_id 51
    priority 90
    advert_int 1
    master_src_ip 10.0.209.144
    nopreempt
    authentication {
        auth_type PASS
        auth_pass ByronYangdiCluster1OK
    }
    unicast_peer {
        10.0.209.143
        10.0.209.145
    }
    virtual_ipaddress {
        10.0.209.142/24
    }
    track_script {
        CheckK8sMaster
    }
}
```

master3

```javascript
! Configuration File for keepalived

global_defs {
   router_id LVS_K8s_Cluster1
}

vrrp_script CheckK8sMaster {
    script "curl -k https://10.0.209.142:6443"
    interval 3
    timeout 5
    fall 2
    rise 2
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens32
    virtual_router_id 51
    priority 80
    advert_int 1
    master_src_ip 10.0.209.145
    nopreempt
    authentication {
        auth_type PASS
        auth_pass ByronYangdiCluster1OK
    }
    unicast_peer {
        10.0.209.144
        10.0.209.145
    }
    virtual_ipaddress {
        10.0.209.142/24
    }
    track_script {
        CheckK8sMaster
    }
}
```

1. 配置ipvs：

```bash
cat > /etc/sysconfig/modules/ipvs.modules <<EOF
ipvs_modules="ip_vs ip_vs_lc ip_vs_wlc ip_vs_rr ip_vs_wrr ip_vs_lblc ip_vs_lblcr ip_vs_dh
ip_vs_sh ip_vs_fo ip_vs_nq ip_vs_sed ip_vs_ftp nf_conntrack"
for kernel_module in \${ipvs_modules}; do
/sbin/modinfo -F filename \${kernel_module} > /dev/null 2>&1
if [ $? -eq 0 ]; then
/sbin/modprobe \${kernel_module}
fi
done
EOF

chmod 755 /etc/sysconfig/modules/ipvs.modules && bash /etc/sysconfig/modules/ipvs.modules
&& lsmod | grep ip_vs
```

1. 启动和设置keepalived.service：

```bash
systemctl enable keepalived
systemctl start keepalived
```

**初始化master节点：**

1. 从任意一个etcd节点向master节点拷贝认证文件：

```bash
 export CONTROL_PLANE="root@10.0.209.143"
 scp -P 58888 /etc/kubernetes/pki/etcd/ca.crt "${CONTROL_PLANE}":
 scp -P 58888 /etc/kubernetes/pki/apiserver-etcd-client.crt "${CONTROL_PLANE}":
 scp -P 58888 /etc/kubernetes/pki/apiserver-etcd-client.key "${CONTROL_PLANE}":
```

1. 创建kubeadm config文件，注意修改私库地址：

```bash
vim kubeadm_config.yaml

apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
kubernetesVersion: 1.16.3
controlPlaneEndpoint: "10.0.209.142:6443"
imageRepository: "10.0.209.120/gcr.io/google_containers"
etcd:
    external:
        endpoints:
        - https://10.0.209.146:2379
        - https://10.0.209.147:2379
        - https://10.0.209.148:2379
        caFile: /etc/kubernetes/pki/etcd/ca.crt
        certFile: /etc/kubernetes/pki/apiserver-etcd-client.crt
        keyFile: /etc/kubernetes/pki/apiserver-etcd-client.key
```

1. 拷贝front-proxy的客户证书到另外两个master节点上：

```bash
scp -P 58888 front-proxy-client.* root@10.0.209.145:/etc/kubernetes/pki/
scp -P 58888 front-proxy-client.* root@10.0.209.146:/etc/kubernetes/pki/
```

1. 用初始化第一个master节点：

```bash
$ kubeadm init --config kubeadm_config.conf --upload-certs
[init] Using Kubernetes version: v1.16.3
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Activating the kubelet service
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Using existing ca certificate authority
[certs] Using existing apiserver certificate and key on disk
[certs] Using existing apiserver-kubelet-client certificate and key on disk
[certs] Using existing front-proxy-ca certificate authority
[certs] Using existing front-proxy-client certificate and key on disk
[certs] External etcd mode: Skipping etcd/ca certificate authority generation
[certs] External etcd mode: Skipping etcd/server certificate generation
[certs] External etcd mode: Skipping etcd/peer certificate generation
[certs] External etcd mode: Skipping etcd/healthcheck-client certificate generation
[certs] External etcd mode: Skipping apiserver-etcd-client certificate generation
[certs] Using the existing "sa" key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Using existing kubeconfig file: "/etc/kubernetes/admin.conf"
[kubeconfig] Using existing kubeconfig file: "/etc/kubernetes/kubelet.conf"
[kubeconfig] Using existing kubeconfig file: "/etc/kubernetes/controller-manager.conf"
[kubeconfig] Using existing kubeconfig file: "/etc/kubernetes/scheduler.conf"
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 31.505067 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.16" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Storing the certificates in Secret "kubeadm-certs" in the "kube-system" Namespace
[upload-certs] Using certificate key:
60da47c32551e837a6f4a88d7d93d94691bd88e51e5ee27ae3f418420fc9c89c
[mark-control-plane] Marking the node master1-143.vmwlab as control-plane by adding the label "node-role.kubernetes.io/master=''"
[mark-control-plane] Marking the node master1-143.vmwlab as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: bse0g2.pe0ldy103ba8zn2v
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 10.0.209.142:6443 --token bse0g2.pe0ldy103ba8zn2v \
    --discovery-token-ca-cert-hash sha256:6afe4c52fd6642687a71db477edf3bab5f0dfd677dbc02c3e9c40769a0f4b96c \
    --control-plane --certificate-key 60da47c32551e837a6f4a88d7d93d94691bd88e51e5ee27ae3f418420fc9c89c

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.0.209.142:6443 --token bse0g2.pe0ldy103ba8zn2v \
    --discovery-token-ca-cert-hash sha256:6afe4c52fd6642687a71db477edf3bab5f0dfd677dbc02c3e9c40769a0f4b96c
```

1. 拷贝管理配置文件到对应用户的目录里，主要是针对kubectl：

```bash
 mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

1. 初始化另外两个master节点：

```bash
 kubeadm join 10.0.209.142:6443 --token bse0g2.pe0ldy103ba8zn2v \
    --discovery-token-ca-cert-hash sha256:6afe4c52fd6642687a71db477edf3bab5f0dfd677dbc02c3e9c40769a0f4b96c \
    --control-plane --certificate-key 60da47c32551e837a6f4a88d7d93d94691bd88e51e5ee27ae3f418420fc9c89c
```

1. 在各个work节点上运行并加入到集群中去：

```bash
kubeadm join 10.0.209.142:6443 --token bse0g2.pe0ldy103ba8zn2v \
    --discovery-token-ca-cert-hash sha256:6afe4c52fd6642687a71db477edf3bab5f0dfd677dbc02c3e9c40769a0f4b96c \
    --control-plane --certificate-key 60da47c32551e837a6f4a88d7d93d94691bd88e51e5ee27ae3f418420fc9c89c
```

至此，所有的集群节点就初始化完成了，使用kubectl就可以控制这个集群了。

