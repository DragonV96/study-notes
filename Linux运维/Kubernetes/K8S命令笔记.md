# K8S命令笔记

## 1 基本命令

### 1.1 操作 Node

#### 1.1.1 查看 Node

1.查看 master 节点初始化结果

````shell
kubectl get nodes -o wide
````

#### 1.1.3 删除 Node

1.移除 worker 节点

①在需要移除的 worker 节点（如 NAME=worker）上执行命令

````shell
kubeadm reset
````

②在第一个 master 节点上执行命令

> 通过在 master 上执行 `kubectl get nodes -o wide` 来获取节点 NAME 等相关信息。

````shell
kubectl delete node [NODE NAME]

# 示例
kubectl delete node worker
````

### 1.2 操作 Pod

#### 1.2.1 查看 Pod

1.查看容器组中所有 Pod 信息

````shell
kubectl get pod -n kube-system -o wide
````

2.查看指定 Pod 完整信息

````shell
kubectl describe pod [Pod NAME] -n [Namespace NAME]

# 示例
kubectl describe pod coredns-66db54ff7f-ddz8k -n kube-system
````

3.查看指定 Pod 日志

````shell
# -f 表示实时刷新
# --tail=n，tail 表示查看日志尾部，n 表示显示n行
# -n 表示查看指定 Namespace
kubectl logs -f --tail=n [Pod NAME] -n [Namespace NAME]

# 示例
kubectl logs -f --tail=500 coredns-66db54ff7f-ddz8k -n kube-system
````

#### 1.2.3 删除 Pod

1.删除指定 Pod

````shell
kubectl delete pod [Pod NAME] -n kube-system

# 示例
kubectl delete pod kube-flannel-ds-amd64-8l25c -n kube-system
````

### 1.3 操作 Namespace

#### 1.3.1 查看 Namespace

1.查看命名空间

````shell
kubectl get ns
````

#### 1.3.3 删除 Namespace

1.删除指定 Namespace 命名空间

①方式一：

````shell
kubectl delete namespaces [Namespace NAME]

# 示例
kubectl delete namespaces kube-logs
````

②方式二：

````shell
kubectl delete ns [Namespace NAME]

# 示例
kubectl delete ns kube-logs
````

### 1.4 操作 StorageClass

#### 1.4.1 查看 StorageClass

1.查看命名空间

````shell
kubectl get sc
````

#### 1.4.3 删除 StorageClass

1.删除指定 StorageClass 命名空间

````shell
kubectl delete sc [StorageClass NAME]

# 示例
kubectl delete sc efk-nfs-client
````

### 1.5 操作 PersistentVolume

#### 1.5.1 查看 PersistentVolume

1.查看命名空间

````shell
kubectl get pv
````

#### 1.5.3 删除 PersistentVolume

1.删除指定 PersistentVolume 命名空间

````shell
kubectl delete pv [PersistentVolume NAME]

# 示例
kubectl delete pv efk-nfs-client-agkj9iow
````

## 2 高级命令

### 2.1 获取 token

1.获取 Master 节点的管理员 token

- 拥有 ClusterAdmin 的权限，可以执行所有操作

````shell
echo $(kubectl -n kube-system get secret $(kubectl -n kube-system get secret | grep kuboard-user | awk '{print $1}') -o go-template='{{.data.token}}' | base64 -d)
````

2.获取 Master 节点的只读 token

- view 可查看名称空间的内容
- system:node 可查看节点信息
- system:persistent-volume-provisioner 可查看存储类和存储卷声明的信息

````shell
echo $(kubectl -n kube-system get secret $(kubectl -n kube-system get secret | grep kuboard-viewer | awk '{print $1}') -o go-template='{{.data.token}}' | base64 -d)
````

### 2.2 创建和更新 K8S 对象

1.更新更新 k8s 对象，修改了 yaml 配置文件使其生效**（不推荐使用）**

````shell
kubectl replace -f [xxx.yaml 配置文件]

# 示例
kubectl replace -f nginx-deployment.yaml
````

2.创建或更新 k8s 对象，创建或修改了 yaml 配置文件使其生效**（官方推荐使用，便于版本管理）**

````shell
kubectl apply -f [xxx.yaml 本地或远程配置文件]

# 示例（本地）
kubectl apply -f nginx-deployment.yaml

# 示例（远程）
kubectl apply -f https://kuboard.cn/install-script/v1.16.2/nginx-ingress.yaml
````

## 3 搭建流程

### 3.1 Kubeadm 搭建 K8S 单 Master 节点

可视化组件采用开源框架 Kuboard。

#### 3.1.1 环境要求

**1. 配置及要求**

| 配置项             | 要求                             |
| ------------------ | -------------------------------- |
| 服务器             | 2台及以上 2核4G内存 20G+固态更优 |
| 操作系统           | Cent OS 7.6 / 7.7 / 7.8          |
| Kubernetes 版本    | 1.18.x（当前为 1.18.8）          |
| Kuboard 版本       | 2.0.4.3                          |
| calico 版本        | 3.13.1                           |
| nginx-ingress 版本 | 1.5.5                            |
| Docker 版本        | 19.03.8                          |

**2. 环境检查**

1）检查 centos/hostname

````shell
# 在 master 节点和 worker 节点都要执行
cat /etc/redhat-release

# 此处 hostname 的输出将会是该机器在 Kubernetes 集群中的节点名字
# 不能使用 localhost 作为节点的名字，且不能包含下划线、小数点、大写字母
hostname

# 请使用 lscpu 命令，核对 CPU 信息
# Architecture: x86_64    本安装文档不支持 arm 架构
# CPU(s):       2         CPU 内核数量不能低于 2
lscpu
````

2）检查网络

所有节点上 Kubernetes 所使用的 IP 地址必须可以互通（无需 NAT 映射、无安全组或防火墙隔离），且为固定的内网 IP 地址。

````shell
# 查看服务器的默认网卡，通常是 eth0，如 default via 172.21.0.23 dev eth0
ip route show

# 显示默认网卡的 IP 地址，Kubernetes 将使用此 IP 地址与集群内的其他节点通信，如 172.17.216.80
ip addr
````

#### 3.1.2 配置环境

**所有服务器**都需要**执行**此配置环境的步骤。

**1. 修改 hostname**

````shell
# 修改 hostname（根据自己实际情况命名，如 master 节点命名为 master，worker 节点命名为 worker）
hostnamectl set-hostname master
# 查看修改结果
hostnamectl status
# 设置 hostname 解析
echo "127.0.0.1   $(hostname)" >> /etc/hosts
````

**2. 安装 docker、kubelete、kubeadm 和 kubectl**

方式一（推荐）：将下方 `#!/bin/bash` 开头的脚本保存为 install_env.sh 文件并执行命令即可。

````shell
# 创建执行脚本
vim install_env.sh

# 将下方脚本整个粘贴进来并保存，然后赋予其执行权限 
# 1.18.8 表示 K8S 的安装版本，可根据实际情况选择
chmod +x install_env.sh && ./install_env.sh 1.18.8
````

方式二：挨个命令执行。

````shell
#!/bin/bash

# 卸载旧版本
yum remove -y docker \
docker-client \
docker-client-latest \
docker-ce-cli \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-selinux \
docker-engine-selinux \
docker-engine

# 设置 yum repository
yum install -y yum-utils \
device-mapper-persistent-data \
lvm2
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 安装并启动 docker
yum install -y docker-ce-19.03.8 docker-ce-cli-19.03.8 containerd.io
systemctl enable docker
systemctl start docker

# 安装工具 vim
yum install -y vim

# 关闭 防火墙
systemctl stop firewalld
systemctl disable firewalld

# 关闭 SeLinux
setenforce 0
sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config

# 关闭 swap
swapoff -a
yes | cp /etc/fstab /etc/fstab_bak
cat /etc/fstab_bak |grep -v swap > /etc/fstab

# 修改 /etc/sysctl.conf
# 如果有配置，则修改
sed -i "s#^net.ipv4.ip_forward.*#net.ipv4.ip_forward=1#g"  /etc/sysctl.conf
sed -i "s#^net.bridge.bridge-nf-call-ip6tables.*#net.bridge.bridge-nf-call-ip6tables=1#g"  /etc/sysctl.conf
sed -i "s#^net.bridge.bridge-nf-call-iptables.*#net.bridge.bridge-nf-call-iptables=1#g"  /etc/sysctl.conf
sed -i "s#^net.ipv6.conf.all.disable_ipv6.*#net.ipv6.conf.all.disable_ipv6=1#g"  /etc/sysctl.conf
sed -i "s#^net.ipv6.conf.default.disable_ipv6.*#net.ipv6.conf.default.disable_ipv6=1#g"  /etc/sysctl.conf
sed -i "s#^net.ipv6.conf.lo.disable_ipv6.*#net.ipv6.conf.lo.disable_ipv6=1#g"  /etc/sysctl.conf
sed -i "s#^net.ipv6.conf.all.forwarding.*#net.ipv6.conf.all.forwarding=1#g"  /etc/sysctl.conf
# 可能没有，追加
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
echo "net.bridge.bridge-nf-call-ip6tables = 1" >> /etc/sysctl.conf
echo "net.bridge.bridge-nf-call-iptables = 1" >> /etc/sysctl.conf
echo "net.ipv6.conf.all.disable_ipv6 = 1" >> /etc/sysctl.conf
echo "net.ipv6.conf.default.disable_ipv6 = 1" >> /etc/sysctl.conf
echo "net.ipv6.conf.lo.disable_ipv6 = 1" >> /etc/sysctl.conf
echo "net.ipv6.conf.all.forwarding = 1"  >> /etc/sysctl.conf
# 执行命令以应用
sysctl -p

# 配置 K8S 的 yum 源
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
       http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

# 卸载 K8S 旧版本
yum remove -y kubelet kubeadm kubectl

# 安装kubelet、kubeadm、kubectl、kubernetes-cni（K8S 依赖）
# 将 ${1} 替换为 kubernetes 其他版本号，例如 1.18.8
yum install -y kubelet-${1} kubeadm-${1} kubectl-${1} kubernetes-cni-0.6.0

# 修改docker Cgroup Driver为systemd
# # 将/usr/lib/systemd/system/docker.service文件中的这一行 ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
# # 修改为 ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --exec-opt native.cgroupdriver=systemd
# 如果不修改，在添加 worker 节点时可能会碰到如下错误
# [WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". 
# Please follow the guide at https://kubernetes.io/docs/setup/cri/
sed -i "s#^ExecStart=/usr/bin/dockerd.*#ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --exec-opt native.cgroupdriver=systemd#g" /usr/lib/systemd/system/docker.service

# 配置 docker 的镜像源
cat <<EOF > /etc/docker/daemon.json
{
"registry-mirrors":["https://registry.docker-cn.com","https://pee6w651.mirror.aliyuncs.com","http://hub-mirror.c.163.com","https://docker.mirrors.ustc.edu.cn"]
}
EOF

# 重启 docker，并启动 kubelet
systemctl daemon-reload
systemctl restart docker
systemctl enable kubelet && systemctl start kubelet

docker version
````

#### 3.1.3 初始化 master 节点

只在 **master 节点执行**如下所有步骤。

**1. 设置环境变量**

- **APISERVER_NAME** 不能是 master 的 hostname
- **APISERVER_NAME** 必须全为小写字母、数字、小数点，不能包含减号
- **POD_SUBNET** 所使用的网段不能与 master节点/worker节点 所在的网段重叠。该字段的取值为一个 [CIDR](https://kuboard.cn/glossary/cidr.html) 值，如果您对 CIDR 这个概念还不熟悉，请仍然执行 export POD_SUBNET=10.100.0.1/16 命令，不做修改

````shell
# 替换 192.168.1.4 为 master 节点的内网 IP，根据实际情况填写
# export 命令只在当前 shell 会话中有效，开启新的 shell 窗口后，如果要继续安装过程，请重新执行此处的 export 命令
export MASTER_IP=192.168.1.4
# 替换 apiserver.demo 为自己的 dnsName
export APISERVER_NAME=apiserver.demo
# Kubernetes 容器组所在的网段，该网段安装完成后，由 kubernetes 创建，事先并不存在于您的物理网络中
export POD_SUBNET=10.100.0.1/16
echo "${MASTER_IP}    ${APISERVER_NAME}" >> /etc/hosts
````

方式二：挨个命令执行。

**2. 初始化 master**

方式一（推荐）：将下方 `#!/bin/bash` 开头的脚本保存为 install_master.sh 文件并执行命令即可。

````shell
# 创建执行脚本
vim install_master.sh

# 将下方脚本整个粘贴进来并保存，然后赋予执行权限 
# 1.18.8 表示 K8S 的安装版本，可根据实际情况选择
chmod +x install_master.sh && ./install_master.sh 1.18.8
````

方式二：挨个命令执行。

````shell
#!/bin/bash

# 脚本出错时终止执行
set -e

if [ ${#POD_SUBNET} -eq 0 ] || [ ${#APISERVER_NAME} -eq 0 ]; then
  echo -e "\033[31;1m请确保您已经设置了环境变量 POD_SUBNET 和 APISERVER_NAME \033[0m"
  echo 当前POD_SUBNET=$POD_SUBNET
  echo 当前APISERVER_NAME=$APISERVER_NAME
  exit 1
fi

# 配置 K8S 的启动配置文件
# 查看完整配置选项 https://godoc.org/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm/v1beta2
rm -f ./kubeadm-config.yaml
cat <<EOF > ./kubeadm-config.yaml
apiVersion: kubeadm.k8s.io/v1beta2
kind: ClusterConfiguration
kubernetesVersion: v${1}
imageRepository: registry.aliyuncs.com/k8sxio
controlPlaneEndpoint: "${APISERVER_NAME}:6443"
networking:
  serviceSubnet: "10.96.0.0/16"
  podSubnet: "${POD_SUBNET}"
  dnsDomain: "cluster.local"
EOF

# kubeadm init
# 根据您服务器网速的情况，您需要等候 3 - 10 分钟
kubeadm init --config=kubeadm-config.yaml --upload-certs

# 配置 kubectl
rm -rf /root/.kube/
mkdir /root/.kube/
cp -i /etc/kubernetes/admin.conf /root/.kube/config

# 安装 calico 网络插件
# 参考文档 https://docs.projectcalico.org/v3.13/getting-started/kubernetes/self-managed-onprem/onpremises
echo "安装calico-3.13.1"
rm -f calico-3.13.1.yaml
wget https://kuboard.cn/install-script/calico/calico-3.13.1.yaml
kubectl apply -f calico-3.13.1.yaml
````

**3. 查看初始化结果**

````shell
# 执行如下命令，等待 3-10 分钟，直到所有的容器组处于 Running 状态
watch kubectl get pod -n kube-system -o wide

# 查看 master 节点初始化结果
kubectl get nodes -o wide
````

#### 3.1.4 初始化 worker 节点

**1. 设置环境变量**

只在 **worker 节点执行**如下步骤。

- **APISERVER_NAME** 不能是 worker 的 hostname
- **APISERVER_NAME** 必须全为小写字母、数字、小数点，不能包含减号
- **POD_SUBNET** 所使用的网段不能与 master节点/worker节点 所在的网段重叠。该字段的取值为一个 [CIDR](https://kuboard.cn/glossary/cidr.html) 值，如果您对 CIDR 这个概念还不熟悉，请仍然执行 export POD_SUBNET=10.100.0.1/16 命令，不做修改

````shell
# 替换 192.168.1.5 为 worker 节点的内网 IP，根据实际情况填写
# export 命令只在当前 shell 会话中有效，开启新的 shell 窗口后，如果要继续安装过程，请重新执行此处的 export 命令
export MASTER_IP=192.168.1.5
# 替换 apiserver.demo 为自己的 dnsName
export APISERVER_NAME=apiserver.demo
# Kubernetes 容器组所在的网段，该网段安装完成后，由 kubernetes 创建，事先并不存在于您的物理网络中
export POD_SUBNET=10.100.0.1/16
echo "${MASTER_IP}    ${APISERVER_NAME}" >> /etc/hosts
````

**2. 获取 master 节点的 join 命令参数**

此步骤在  **master 节点执行**。

````shell
# 创建 worker 节点的 join 命令
kubeadm token create --print-join-command

# kubeadm token create 命令的输出
kubeadm join apiserver.demo:6443 --token n1jsw9.0iqur09ootszdst7 \
    --discovery-token-ca-cert-hash sha256:e69288ca1cb8b40329b23e78dc6c8b4ec7e249cd0060f2ece27186b100866361
````

> 该 token 的有效时间为 2 个小时，2小时内，您可以使用此 token 初始化任意数量的 worker 节点。

**3. 初始化 worker**

只在 **worker 节点执行**如下步骤。

````shell
# 替换为 master 节点上 kubeadm token create 命令的输出（上个步骤中的 join 命令）
kubeadm join apiserver.demo:6443 --token n1jsw9.0iqur09ootszdst7 \
    --discovery-token-ca-cert-hash sha256:e69288ca1cb8b40329b23e78dc6c8b4ec7e249cd0060f2ece27186b100866361
````

**4. 查看初始化结果**

此步骤在  **master 节点执行**。

````shell
# 执行如下命令，等待 3-10 分钟，直到所有的容器组处于 Running 状态
watch kubectl get pod -n kube-system -o wide

# 查看 master 节点初始化结果
kubectl get nodes -o wide
````

#### 3.1.5 安装 Ingress Controller

在 **master 节点执行**。

````shell
kubectl apply -f https://kuboard.cn/install-script/v1.18.x/nginx-ingress.yaml
````

#### 3.1.6 安装 Kuboard

**1. 安装 Kuboard 可视化 UI**

````shell
# 安装稳定版 Kuboard
kubectl apply -f https://kuboard.cn/install-script/kuboard.yaml
kubectl apply -f https://addons.kuboard.cn/metrics-server/0.3.7/metrics-server.yaml
````

**2. 查看 Kuboard 运行状态**

````shell
kubectl get pods -l k8s.kuboard.cn/name=kuboard -n kube-system
````

**3. 获取 token**

查看本文档 `2 高级命令` 下的 `2.1 获取 token` 的获取 token 步骤。

**4. 访问 Kuboard web 界面**

Kuboard Service 使用了 NodePort 的方式暴露服务，NodePort 为 32567，访问地址为：

```
http://任意一个Worker节点的IP地址:32567/
```

输入上一步获取的 token 即可进入界面。

### 3.2 Kubeadm 搭建 K8S 高可用集群

### 3.3 K8S 搭建 nfs

### 3.4 K8S 搭建 efk



## 4 常见问题排查

