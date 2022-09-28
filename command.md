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
cat > /etc/profile.d/go.sh << EOF
export GOROOT=/usr/local/go
export GOPATH=/data/go
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
