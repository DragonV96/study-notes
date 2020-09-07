# k8s命令笔记

## 1 基本命令

### 1.1 查看

1.查看容器组中所有 Pod 信息

````shell
kubectl get pod -n kube-system -o wide
````

2.查看 master 节点初始化结果

````shell
kubectl get nodes -o wide
````

3.查看指定 Pod 完整信息

````shell
kubectl describe pod [Pod NAME] -n kube-system

# 示例
kubectl describe pod coredns-66db54ff7f-ddz8k -n kube-system
````

### 1.2 删除

1.删除 Pod

````shell
kubectl delete pod [Pod NAME] -n kube-system

# 示例
kubectl delete pod kube-flannel-ds-amd64-8l25c -n kube-system
````

2.移除 worker 节点

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

### 2.2 创建和更新 k8s 对象

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

## 3 常见问题排查

