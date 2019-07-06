## 什么是Docker
Docker is a platform for developers and sysadmins to develop, deploy, and run applications with containers. The use of Linux containers to deploy applications is called containerization. Containers are not new, but their use for easily deploying applicatio
Docker是一个供开发人员和系统管理员使用容器开发、部署和运行应用程序的平台。使用Linux容器部署应用程序称为容器化。容器并不新鲜，但是它们用于方便地部署应用程序却是新鲜的

# Doker安装
## 老版本的Docker被称为docker或docker-engine。如果安装了这些，请将其卸载
$ apt-get remove docker docker-engine docker.io
## Ubuntu安装社区版docker CE，前置步骤
https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#os-requirements
# 安装最新版本docker-ce
$ apt-get update
$ apt-get install docker-ce
# 安装特定版本docker-ce
# List the available versions.
$ apt-cache madison docker-ce
$ apt-get install docker-ce=<VERSION>

# 重启docker服务，类似于mysql命令
$ sudo service docker restart


# 验证安装是否ok

$ docker run hello-world

> 做了以下几步：
1. The Docker client contacted the Docker daemon. //docker 客户端连接docker守护进程
2. The Docker daemon pulled the "hello-world" image from the Docker Hub. //docker守护进程从docker中心拉取helle-world镜像
3. The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.  
//docker守护进程用 运行生产你看到的内容的可执行程序的镜像 创建一个新的容器
4. The Docker daemon streamed that output to the Docker client, which sent it to your terminal
//docker守护进程输出运行结果到docker客户端，客户端把它显示在终端

# 卸载docker-ce
$ sudo apt-get purge docker-ce
# 主机上的镜像，容器，卷或自定义配置文件不会自动删除。删除所有镜像，容器和卷：

$ sudo rm -rf /var/lib/docke

# 创建docker应用程序

https://docs.docker.com/get-started/part2/

# 创建了一个Docker镜像（确保在Dockerfile同级目录）

$ docker build -t {image_name} .

# List your docker images
$ docker images
或较新的
$ docker image ls

# 运行应用程序,将计算机的端口4000映射到容器的已发布端口80

$ docker run -p 4000:80 {image_name}

# use curl test published url
$ curl http://localhost:4000
#以分离模式运行docker app，后台运行
$ docker run -d -p 4000:80 {image_name}

# 列出正在运行的容器
$ docker container ls

# 进入docker容器
$ sudo docker exec -it 775c7c9ee1e1 /bin/bash
# 停止容器
$ docker container stop <Container NAME or ID\>
# docker 搜索镜像
$ docker search {keyword}}
# 下载镜像
docker pull {image_name}[:latest]
