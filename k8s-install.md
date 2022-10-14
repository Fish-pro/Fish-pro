## kubernetes v1.24.1 install

### 主机信息
```
集群类型	        名词	             ip
控制节点-1	k8s-m1_10-29-14-45	10.29.14.45
计算节点-1	k8s-n1_10-29-14-47	10.29.14.47
计算节点-2	k8s-n2_10-29-14-49	10.29.14.49
```

### 所有机器上配置于host
```shell
cat >> /etc/hosts << EOF
10.29.14.45 master 
10.29.14.47 node01
10.29.14.49 node02
EOF
```

### 在所有节点上关闭swap分区
```shell
swapoff -a ; sed -i '/fstab/d' /etc/fstab
```

### 所有节点关闭防火墙
```shell
systemctl status firewalld.service
systemctl stop firewalld.service
```


### 所有节点更新yum源
```shell
# 先用已有的yum源安装wget
yum install wget
rm -rf /etc/yum.repos.d/*  ; wget ftp://ftp.rhce.cc/k8s/* -P /etc/yum.repos.d/
yum clean all
```

### 安装containerd
```shell
yum install containerd.io cri-tools  -y
crictl config runtime-endpoint unix:///var/run/containerd/containerd.sock
```

master上生成配置文件/etc/containerd/config.toml
```
containerd config default > /etc/containerd/config.toml
```

编辑/etc/containerd/config.toml

+ 搜索mirrors，把
```
      [plugins."io.containerd.grpc.v1.cri".registry.mirrors]
改成
      [plugins."io.containerd.grpc.v1.cri".registry.mirrors]
        [plugins."io.containerd.grpc.v1.cri".registry.mirrors."docker.io"]
          endpoint = ["https://frz7i079.mirror.aliyuncs.com"]
```

+ 搜索sandbox，把
```
sandbox_image = "k8s.gcr.io/pause:3.6"
改为
sandbox_image = "registry.aliyuncs.com/google_containers/pause:3.7"
```

+ 搜索SystemdCgroup，把
```
          [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
            BinaryName = ""
            CriuImagePath = ""
            CriuPath = ""
            CriuWorkPath = ""
            IoGid = 0
            IoUid = 0
            NoNewKeyring = false
            NoPivotRoot = false
            Root = ""
            ShimCgroup = ""
            SystemdCgroup = false
```
改成
```
          [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
            SystemdCgroup = true
```

同步更改到node01 node01
```shell
scp /etc/containerd/config.toml node01:/etc/containerd/
scp /etc/containerd/config.toml node02:/etc/containerd/
```

所有节点重启containerd服务，并设置开机自动启动
```shell
systemctl enable containerd  ; systemctl restart containerd
```

### 安装containerd客户端工具nerdctl

到下面的连接下载最新版的nerdctl，本实验时最新版是0.20.0版本的
https://github.com/containerd/nerdctl/releases

先在master上下载，然后同步到node01 node02即可
```shell
tar zxf nerdctl-0.20.0-linux-amd64.tar.gz -C /usr/bin/ nerdctl
scp /usr/bin/nerdctl node01:/usr/bin/
scp /usr/bin/nerdctl node02:/usr/bin/
```

到下面的地址下载nerdctl所需要的cni插件
https://github.com/containernetworking/plugins/releases

先在master上做，然后同步到node01 node02即可
```shell
mkdir -p /opt/cni/bin/
tar zxf cni-plugins-linux-amd64-v1.1.1.tgz -C /opt/cni/bin/
scp -r /opt/cni/ vms72:/opt/
```

修改/etc/profile，在第二行添加如下两行内容
```
head -3 /etc/profile
# /etc/profile
source <(nerdctl completion bash)
export CONTAINERD_NAMESPACE=k8s.io
```

让设置生效
```shell
source /etc/profile
```

### 加载模块及修改参数

在所有节点上加载模块
```
modprobe overlay ; modprobe br_netfilter
```

在所有机器上执行下面的命令，目的是系统重启时模块能自动加载
```shell
cat > /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF
```

在所有机器上执行下面的命令，目的是实现重启系统后，参数也能继续生效
```shell
cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
```

让上述参数立即生效
```shell
sysctl -p /etc/sysctl.d/k8s.conf
```

### 安装kubernetes
查看当前源里有哪些版本
```shell
yum list --showduplicates kubeadm --disableexcludes=kubernetes
```

在本试验时最新的版本是v1.24.1，所以本次就安装v1.24.1版本的

所有节点上安装软件包
```shell
yum install -y kubelet-1.24.1-0 kubeadm-1.24.1-0 kubectl-1.24.1-0  --disableexcludes=kubernetes
```

所有节点上启动kubelet并设置开机自动启动
```shell
systemctl enable kubelet --now
```

此时kubelet状态是activating的，不是active的
```shell
systemctl is-active kubelet
activating
```

在master上初始化集群
```shell
kubeadm init --image-repository registry.aliyuncs.com/google_containers --kubernetes-version=v1.24.1 --pod-network-cidr=10.244.0.0/16
```

等待一会之后，在master执行提示的命令
```shell
mkdir -p $HOME/.kube 
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config 
sudo chown $(id -u):$(id -g) $HOME/.kube/config 
```

在node01 node02上加入集群
```shell
kubeadm join 10.29.14.45:6443 --token okpvme.01x40qr53pin40tz --discovery-token-ca-cert-hash sha256:4d5a1a590ccc1c84956c866d7773bbb76701dab473e70061bdab9447442be84d
```

在master上查看节点状态。
```shell
kubectl get no
NAME          STATUS      ROLES           AGE    VERSION
10-29-14-45   NotReady    control-plane   13m    v1.24.1
10-29-14-47   NotReady    <none>          5m     v1.24.1
10-29-14-49   NotReady    <none>          5m     v1.24.1
```

### 安装calico
到下面链接下载最新版的calico.yaml
```shell
wget -O calico.yaml https://docs.projectcalico.org/manifests/calico.yaml
```

同`kubeadm init`中`--pod-network-cidr`参数, 将
```
          # - name: CALICO_IPV4POOL_CIDR
          #   value: "192.168.0.0/16"
```
修改为
```
          - name: CALICO_IPV4POOL_CIDR
            value: "10.244.0.0/16"
```


在master上安装calico，不需要在node01 node02上做什么
```shell
kubectl apply -f calico.yaml
```

在master上再次查看节点状态
```shell
kubectl get no
NAME          STATUS   ROLES           AGE    VERSION
10-29-14-45   Ready    control-plane   111m   v1.24.1
10-29-14-47   Ready    <none>          82m    v1.24.1
10-29-14-49   Ready    <none>          82m    v1.24.1
```

### 安装metric server
```
kubectl apply -f components.yaml
```
