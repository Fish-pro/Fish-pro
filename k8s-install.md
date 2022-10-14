## kubernetesv1.24 install

### 主机信息
```
集群类型	      名词	               ip
控制节点-1	k8s-m1_10-29-14-45	10.29.14.45
计算节点-1	k8s-n1_10-29-14-47	10.29.14.47
计算节点-2	k8s-n2_10-29-14-49	10.29.14.49
```

### host
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

```


### 更新yum源
```
# 安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
yum install -y yum-utils device-mapper-persistent-data lvm2
# 设置 yum 源
# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 安装containerd
```shell
yum install containerd -y

# 安装的版本是1.4.12

containerd config default > /etc/containerd/config.toml
systemctl restart containerd
systemctl status containerd

# 替换 containerd 默认的 sand_box 镜像，编辑 /etc/containerd/config.toml

sandbox_image = "registry.aliyuncs.com/google_containers/pause:3.2"

# 重启containerd
systemctl daemon-reload
systemctl restart containerd
```
```
kubeadm join 10.29.14.45:6443 --token okpvme.01x40qr53pin40tz --discovery-token-ca-cert-hash sha256:4d5a1a590ccc1c84956c866d7773bbb76701dab473e70061bdab9447442be84d
```
