# pprof

## 提供 pprof 的接口能力

```go
package main

import (
	"log"
	"net/http"
	_ "net/http/pprof"
	"time"
)

var datas []string

func main() {
	go func() {
		for {
			log.Printf("len: %d", Add("go-programming-tour-book"))
			time.Sleep(time.Millisecond * 10)
		}
	}()

	_ = http.ListenAndServe("0.0.0.0:6060", nil)
}

func Add(str string) int {
	data := []byte(str)
	datas = append(datas, string(data))
	return len(datas)
}
```

访问 `http://127.0.0.1:6060/debug/pprof/` 进入入口界面

+ allocs：查看过去所有内存分配的样本，访问路径为 $HOST/debug/pprof/allocs。
+ block：查看导致阻塞同步的堆栈跟踪，访问路径为 $HOST/debug/pprof/block。
+ cmdline： 当前程序的命令行的完整调用路径。
+ goroutine：查看当前所有运行的 goroutines 堆栈跟踪，访问路径为 $HOST/debug/pprof/goroutine。
+ heap：查看活动对象的内存分配情况， 访问路径为 $HOST/debug/pprof/heap。
+ mutex：查看导致互斥锁的竞争持有者的堆栈跟踪，访问路径为 $HOST/debug/pprof/mutex。
+ profile： 默认进行 30s 的 CPU Profiling，得到一个分析用的 profile 文件，访问路径为 $HOST/debug/pprof/profile。
+ threadcreate：查看创建新 OS 线程的堆栈跟踪，访问路径为 $HOST/debug/pprof/threadcreate。

## 查看可视化界面

```sh
$ wget http://127.0.0.1:6060/debug/pprof/profile   
```

默认需要等待 30 秒，执行完毕后可在当前目录下发现采集的文件 profile，针对可视化界面我们有两种方式可进行下一步分析：

1. ** 方法一（推荐）： ** 该命令将在所指定的端口号运行一个 PProf 的分析用的站点。
   
```sh
$ go tool pprof -http=:6001 profile
```

2. ** 方法二： ** 通过 web 命令将以 svg 的文件格式写入图形，然后在 Web 浏览器中将其打开。

```sh
$ go tool pprof profile
Type: cpu
Time: Feb 1, 2020 at 12:09pm (CST)
Duration: 30s, Total samples = 60ms (  0.2%)
Entering interactive mode (type "help" for commands, "o" for options)
(pprof) web
```

如果出现错误提示 Could not execute dot; may need to install graphviz.，那么意味着你需要安装 graphviz 组件。

另外方法一所运行的站点，实际上包含了方法二的内容（svg 图片），并且更灵活，因此非特殊情况，我们会直接使用方法一的方式运行站点来做观察和分析。

## 通过测试用例做剖析

首先我们将先前所编写的 Add 方法挪到独立的 package 中，命名为 add.go 文件，然后新建 add_test.go 文件，写入如下测试用例代码：

```go
func TestAdd(t *testing.T) {
	_ = Add("go-programming-tour-book")
}

func BenchmarkAdd(b *testing.B) {
	for i := 0; i < b.N; i++ {
		Add("go-programming-tour-book")
	}
}
```

在完成代码编写后，我们回到命令行窗口执行如下采集命令：

```sh
$ go test -bench=. -cpuprofile=cpu.profile
```

执行完毕后会在当前命令生成 cpu.profile 文件，然后只需执行 `go tool pprof cpu.profile` 命令就可以进行查看了
另外除了对 CPU 进行剖析以外，我们还可以调整选项，对内存情况进行分析，如下采集命令：

```sh
$ go test -bench=. -memprofile=mem.profile
```

接下来和上面一样，执行 `go tool pprof mem.profile` 命令进行查看
