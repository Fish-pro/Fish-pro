## ebpf env

### Create vm

```bash
vm create ebpf ubuntu22
```

### Modify gateway

`vi /etc/netplan/ens192.yaml`
```bash
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
        via: 172.19.4.1 #配置网关（原有gateway4已弃用）(10.29.0.1)
     nameservers:
      addresses: [114.114.114.114]
version: 2
```
```bash
reboot
```

### Install simple basic tools
```bash
apt-get update
apt-get install openssh-server vim git tree
```

### Install golang
```
wget https://dl.google.com/go/go1.19.linux-amd64.tar.gz
tar -C /usr/local/ -xzf go1.19.linux-amd64.tar.gz
mkdir go
cat >> .profile << EOF
export GOROOT=/usr/local/go
export GOPATH=/root/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
export GO111MODULE="on" 
export GOPROXY=https://goproxy.cn,direct
EOF
source .profile
```

### Installation dependency
```bash
apt install clang llvm
```

### Configure environment variables
```bash
export BPF_CLANG=clang
```

### Clone cilium/ebpf to a local directory
```bash
mkdir -p $GOPATH/src/github.com/cilium
cd $GOPATH/src/github.com/cilium
git clone https://github.com/cilium/ebpf.git
```

### test
```bash
root@10-29-4-11:~/go/src/github.com# cd cilium/ebpf/examples/kprobe
root@10-29-4-11:~/go/src/github.com/cilium/ebpf/examples/kprobe# tree
.
├── bpf_bpfeb.go
├── bpf_bpfeb.o
├── bpf_bpfel.go
├── bpf_bpfel.o
├── kprobe.c
└── main.go

0 directories, 6 files
root@10-29-4-11:~/go/src/github.com/cilium/ebpf/examples/kprobe# rm -rf *.o bpf_*.go
root@10-29-4-11:~/go/src/github.com/cilium/ebpf/examples/kprobe# go generate
Compiled /root/go/src/github.com/cilium/ebpf/examples/kprobe/bpf_bpfel.o
Stripped /root/go/src/github.com/cilium/ebpf/examples/kprobe/bpf_bpfel.o
Wrote /root/go/src/github.com/cilium/ebpf/examples/kprobe/bpf_bpfel.go
Compiled /root/go/src/github.com/cilium/ebpf/examples/kprobe/bpf_bpfeb.o
Stripped /root/go/src/github.com/cilium/ebpf/examples/kprobe/bpf_bpfeb.o
Wrote /root/go/src/github.com/cilium/ebpf/examples/kprobe/bpf_bpfeb.go
root@10-29-4-11:~/go/src/github.com/cilium/ebpf/examples/kprobe# tree
.
├── bpf_bpfeb.go
├── bpf_bpfeb.o
├── bpf_bpfel.go
├── bpf_bpfel.o
├── kprobe.c
└── main.go

0 directories, 6 files
```
```bash
root@10-29-4-11:~/go/src/github.com/cilium/ebpf/examples/kprobe# go build
root@10-29-4-11:~/go/src/github.com/cilium/ebpf/examples/kprobe# tree
.
├── bpf_bpfeb.go
├── bpf_bpfeb.o
├── bpf_bpfel.go
├── bpf_bpfel.o
├── kprobe
├── kprobe.c
└── main.go
root@10-29-4-11:~/go/src/github.com/cilium/ebpf/examples/kprobe# sudo ./kprobe
2023/03/07 06:45:20 Waiting for events..
2023/03/07 06:45:32 sys_execve called 0 times
2023/03/07 06:45:33 sys_execve called 0 times
2023/03/07 06:45:34 sys_execve called 0 times
2023/03/07 06:45:35 sys_execve called 0 times
2023/03/07 06:45:36 sys_execve called 1 times
2023/03/07 06:45:37 sys_execve called 1 times
2023/03/07 06:45:44 sys_execve called 1 times
2023/03/07 06:45:45 sys_execve called 24 times
2023/03/07 06:45:46 sys_execve called 34 times
```
