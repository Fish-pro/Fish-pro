## pprof

```sh
go tool pprof http://localhost:port/debug/profile
go tool pprof http://localhost:port/debug/heap
go tool pprof http://localhost:port/debug/goroutine
```

### kubelet

master 节点开启代理

```sh
kubectl proxy --address='0.0.0.0'  --accept-hosts='^*$'
```

获取性能指标

```sh
go tool pprof http://127.0.0.1:8001/api/v1/nodes/192-168-249-10/proxy/debug/pprof/heap
go tool pprof http://127.0.0.1:8001/api/v1/nodes/192-168-249-10/proxy/debug/pprof/profile?seconds=120s
go tool pprof http://127.0.0.1:8001/api/v1/nodes/192-168-249-10/proxy/debug/pprof/goroutine
```

### Linux 安装 graphviz

Precompiled binaries are available attached to releases on Gitlab, https://gitlab.com/graphviz/graphviz/-/releases. You may also find it useful to try one of the following third-party sites.

*Ubuntu packages*

```sh
sudo apt install graphviz
```

*Fedora project*

```sh
sudo yum install graphviz
```

*Debian packages*

```sh
sudo apt install graphviz
```

*Stable and development rpms for Redhat Enterprise, or CentOS systems*

```sh
sudo yum install graphviz
```

## 查看火焰图

```sh
go tool pprof --http=0.0.0.0:3001 <file path>
```

## 查看 goroutine 运行情况

```sh
curl http://127.0.0.1:8001/api/v1/nodes/192-168-249-10/proxy/debug/pprof/goroutine?debug=2 > file
cat file
```
