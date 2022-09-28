### install go in linux

1.download file
```shell
wget https://dl.google.com/go/go1.18.linux-amd64.tar.gz
```
2.unzip file
```shell
tar -C /usr/local/ -xzf go1.18.linux-amd64.tar.gz
```
3.add to PATH
```shell
cat > /etc/profile.d/go.sh << EOF
export GOROOT=/usr/local/go
export GOPATH=/data/go
export PATH=$PATH:$GOROOT/bin:$GOPATH
export GO111MODULE="on" 
export GOPROXY=https://goproxy.cn,direct
EOF
```
4.new termainal or source
```shell
source /etc/profile
```
