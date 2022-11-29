### install go in linux

1.下载文件
```shell
wget https://dl.google.com/go/go1.18.linux-amd64.tar.gz
```
2.解压文件
```shell
tar -C /usr/local/ -xzf go1.18.linux-amd64.tar.gz
```
3.将二进制文件添加到PATH中
```shell
mkdir go
```
```shell
cat > /etc/profile.d/go.sh << EOF
export GOROOT=/usr/local/go
export GOPATH=/root/go
export PATH=$PATH:$GOROOT/bin:$GOPATH
export GO111MODULE="on" 
export GOPROXY=https://goproxy.cn,direct
EOF
```
4.新开终端或者应用更改
```shell
source /etc/profile
```

### install docker in linux
1.下载安装介质
```shell
wget https://download.docker.com/linux/static/stable/x86_64/docker-18.06.1-ce.tgz
```
2.解压文件
```shell
tar zxf docker-18.06.1-ce.tgz
sudo cp docker/* /usr/bin/
```
3.注册docker服务
```shell
cat > /usr/lib/systemd/system/docker.service << EOF
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target
 
[Service]
Type=notify
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
LimitNOFILE=infinity
LimitNPROC=infinity
TimeoutStartSec=0
Delegate=yes
KillMode=process
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s
 
[Install]
WantedBy=multi-user.target
EOF
```
3.启动docker服务
```shell
systemctl start/stop docker
```
4.开机自启动
```shell
systemctl enable/disable docker
```

## go跨平台编译
```shell
CGO_ENABLEd=1 GOOS=linux GOARCH=arm64 go build -o main main.go
```

## ssh免密码登陆
```shell
# 生成公私钥
ssh-keygen -t rsa 
# 把公钥传递到远端机器
ssh-copy-id root@york-ebpf
```

## 修改网关
网关信息：10.29.0.1
### CentOS
```shell
nmtui
```
Ubuntu
`vim /etc/netplan/ens192.yaml`
```shell
network:
  version: 2
  renderer: networkd
  ethernets:
    eno1:
      dhcp4: false
      dhcp6: false
     addresses:
      - 172.19.4.152/24 #修改为你的静态ip地址
     routes:
      - to: default
        via: 172.19.4.1 #配置网关（原有gateway4已弃用）
     nameservers:
      addresses: [114.114.114.114]
version: 2
```

## 端口冲突检查
```shell
lsof -i:9090
netstat -nap|grep :9090
```

## kubernetes etcd操作
```shell
kubectl -n kube-system exec -it <pod_name>  -- /bin/bash
1.ETCDCTL_API=3 etcdctl --cert /etc/etcd/peer.crt --key /etc/etcd/peer.key --cacert /etc/etcd/ca.crt --endpoints https://20.8.247.55:2379 get /kubernetes.io/namespaces/<namespace_name> 
2.kubectl get ns <namespace_name> -o json
3. 删除deleteTimestample,修改Termination--->Active
4. 将json内容折为一行
5. 1.ETCDCTL_API=3 etcdctl --cert /etc/etcd/peer.crt --key /etc/etcd/peer.key --cacert /etc/etcd/ca.crt --endpoints https://20.8.247.55:2379 put /kubernetes.io/namespaces/<namespace_name>  'json一行内容'
```

## 管道排序
```shell
kubectl top po | grep -v NAME | sort -k 2rn
```

## 节点容器操作
```shell
docker ps -a | grep <pod>
docker top <continer_id>
docker inspect <continer_id> | grep -i pid
nsenter -t pid iftop
```

## 查看负载转换
```shell
ipvsadm -Ln
```

## 分片压缩
```shell
tar zcvf - dsp-appserver.tar.gz | split -b 20m - dsp-appserver.tar.gz
cat dsp-appserver.tar.gz* | tar zxvf -
```

## DCE 获取集群证书
```shell
ps aux | grep kube-apiserver
cd /proc/2979/root/etc/ssl/private/
kubectl get ep
kubectl --kubeconfig /tmp/config config set-cluster dce --server=https://10.6.124.52:16443 --certificate-authority=ca.crt --embed-certs
kubectl --kubeconfig /tmp/config config set-credentials kube-admin --client-certificate kube-admin.crt --client-key kube-admin.key --embed-certs
kubectl --kubeconfig /tmp/config config set-context dce --cluster dce --user kube-admin
kubectl --kubeconfig /tmp/config config use-context dce
kubectl --kubeconfig /tmp/config get no
kubectl --kubeconfig /tmp/config config view
cat /tmp/config
```

