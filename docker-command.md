## docker 命令

```shell
#xshell+docker使用
sudo docker ps -a#查看模块ID
sudo docker attach 743b1c2e9453#跳转指定的功能模块
tail -f nohup.out#查看日志详细信息
ctril+c#推出日志文件
uname -r#查看内核版本

linux docker 安装：
wget -qO- https://get.docker.com/ | sh#获取最新版本的 Docker 安装包
sudo service docker start#启动docker 后台服务
docker run hello-world#测试运行hello-world

#容器内运行应用程序
docker run ubuntu:15.10 /bin/echo "Hello world"
#通过docker的两个参数 -i -t，让docker运行的容器实现"对话"的能力
docker run -i -t ubuntu:15.10 /bin/bash
#创建一个以进程方式运行的容器
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
#容器运行状态
docker ps
#在容器内使用docker logs命令，查看容器内的标准输出
docker logs 2b1b7a428627
#使用 docker stop 命令来停止容器:
docker stop 容器ID
# 删除容器
docker rm 容器ID
#查看所有命令选项
docker command --help
#运行一个web应用
docker pull training/webapp  # 载入镜像
docker run -d -P training/webapp python app.py
#查看 WEB 应用程序日志
docker logs -f bf08b7f2cd89(程序ID)
docker logs [ID或者名字] #可以查看容器内部的标准输出
#查看WEB应用程序容器的进程
docker top wizardly_chandrasekhar

#检查 WEB 应用程序
docker inspect #查看 Docker 的底层信息

#停止 WEB 应用容器
docker stop wizardly_chandrasekhar   

#重启WEB应用容器
docker start wizardly_chandrasekhar
docker ps -l #查询最后一次创建的容器

#移除WEB应用容器,删除容器时，容器必须是停止状态
docker rm wizardly_chandrasekhar

docker images #来列出本地主机上的镜像

#使用版本为15.10的ubuntu系统镜像来运行容器
docker run -t -i ubuntu:15.10 /bin/bash 
#使用版本为14.04的ubuntu系统镜像来运行容器
docker run -t -i ubuntu:14.04 /bin/bash 

#获取一个新的镜像
docker pull ubuntu:13.10
#查找镜像
docker search httpd

#创建了一个 python 应用的容器
docker run -d -P training/webapp python app.py
docker run -d -p 5000:5000 training/webapp python app.py
docker run -d -p 127.0.0.1:5001:5000 training/webapp python app.py
docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py
#docker port 命令可以让我们快捷地查看端口的绑定情况
docker port adoring_stonebraker 5000
#当我们创建一个容器的时候，docker 会自动对它进行命名。另外，我们也可以使用 --name 标识来命名容器
# 创建容器
docker run -d -P --name runoob training/webapp python app.py
docker create --name stroybook_server nginx:latest

# 运行程序
docker run -d -P training/webapp python app.py
docker run -d -p 5000:5000 training/webapp python app.py
# 指定数组解析外部连接
sudo docker run --name stroybook_sever -p 0.0.0.0:8888:80/tcp -d nginx:latest
-it nginx:latest /bin/bash
         
sudo docker run -i -t -d --name=storybook_server -p 0.0.0.0:8006:8000 -v /home/hbb/storybook_sever/:/root ubuntu:latest

sudo docker start *********
nohup python3 manage.py runserver 0.0.0.0:8000 &
nohup redis-server &
   
# 启动一个docker，docker中运行终端服务 
sudo docker run -t -i --name public_server -p 0.0.0.0:8090:8090/tcp ubuntu /bin/bash
        
#指定Dockerfile构建镜像 
docker build -t public_server ./
#根据镜像启动服务
docker run -d --name public_server -p 0.0.0.0:8080:8080 public_server:lates       
docker exec -ti 96c60ba62427 /bin/bash

# 文件映射指定容器
sudo docker run -v /root/hobby:/root/hobby -t -i --name hobby_server -p 0.0.0.0:8888:8888/tcp ubuntu /bin/bash

```
