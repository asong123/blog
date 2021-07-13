---
title: Docker容器与镜像
urlname: hgnidv
date: '2021-07-12 14:05:06 +0800'
author: 阿松
top: true
hide: false
cover: false
toc: false
mathjax: false
summary: Docker容器与镜像
categories: docker
tags:
  - docker
  - 后端
  - 容器
---

## Docker

### 1. Docker 官网

[https://www.docker.com/](https://www.docker.com/)

### 2. Docker 文档

[https://docs.docker.com/](https://docs.docker.com/)

### 3. Mac 安装 Docker（方式一）

#### 下载 Docker Desktop on Mac

![image-20191103141842224.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575955562210-76a92e1f-9bb3-4c11-bdfc-39f8b83ae743.png#height=604&id=ceXio&name=image-20191103141842224.png&originHeight=604&originWidth=1121&originalType=binary∶=1&size=140065&status=done&style=none&width=1121)

> 第一次使用用户必须注册，然后按照步骤操作即可。

#### 安装 Docker

> 下载完 docker.dmg 后直接安装文件即可。
> 在启动台找到 docker 点击运行。

- docker version 查看版本

### 4. VietualBox（方式二）

#### 下载地址

[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

#### 安装

> 运行 VirtualBox-xxx.dmg

### 5. Vagrant

#### 下载地址

[https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)

#### 安装

> 运行 vagrant_2.2.6_x86_64.dmg

#### 查看版本

`vagrant --version`

### 6. Vagrant 创建 Centos7 虚拟机

- 创建一个目录`mkdir /Users/webxudong/docker/centos7` 进入目录
- 执行 `vagrant init centos/7`
  - 生成 vagrantfile 文件，vagrantfile 也可以自己编写
  - 例如：vagrantfile [https://app.vagrantup.com/centos/boxes/7](https://app.vagrantup.com/centos/boxes/7)
- 执行 `vagrant up` 命令会下载 centos7 镜像会使用比较久的时间
- 执行 `vagrant ssh` 进入虚拟机
- 执行 `exit` 退出虚拟机
- 执行 `vagrant status` 查看虚拟机状态
- 执行 `vagrant halt`停止虚拟机运行
- 执行 `vagrant destroy`删除虚拟机

### 7. Centos7 安装 Docker 社区版

#### 教程地址

[https://docs.docker.com/install/linux/docker-ce/centos/](https://docs.docker.com/install/linux/docker-ce/centos/)

> 下面的步骤都从官网教程中剪切出来的，最好按照官网最新教程来操作

#### 卸载

```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

#### 安装

```shell
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

```shell
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

```shell
$ sudo yum install docker-ce
```

运行 docker `$ sudo systemctl start docker`

查看版本 `$ sudo docker version`

运行 docker 的容器 hello-world  `$ sudo docker run hello-world`

#### 配置用户

- 创建用户组：`$ sudo groupadd docker`
- 添加当前用户到 docker 用户组：`$ sudo gpasswd -a vagrant(用户名) docker`
- 重启 docker 进程`$ sudo service docker restart`
- 重新登录 linux 主机

### 8. 创建 Docker 容器 docker-machine

- 创建 `docker-machine create demo`
- 查看 docker 列表 `docker-machine ls`
- 进入 docker `docker-machine ssh demo`
- 查看版本 `docker version`
- 退出`exit`
- 删除 `docker-machine remove demo`
- 查看 docker-machine 命令 `docker-machine`

### 9. Docker Image

#### 什么是 Image

- image 就是一个镜像
- image 是基于文件和 meta data 的集合（root filesystem）创建的
- 分层的，并且每一层都可以添加改变删除文件，成为一个新的 image。`centos image 添加文件后 成为image #2`
- 不同的 image 可以共享相同的 layer `image #2与image #4都有同一个layer centos image`
- image 本身是 read-only 的

![image-20191103170830822.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575955666350-9f5acf46-2f99-4fe0-9ad4-3769c181ce3f.png#height=327&id=ORITg&name=image-20191103170830822.png&originHeight=327&originWidth=777&originalType=binary∶=1&size=64098&status=done&style=none&width=777)

#### Image 获取

- **Build from Dockerfile** 编译 dockerfile 获取 image

![image-20191103172145055.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575955680266-56f997d4-48c9-4b1b-8515-3cbe6c3f8724.png#height=185&id=haGkg&name=image-20191103172145055.png&originHeight=185&originWidth=529&originalType=binary∶=1&size=76738&status=done&style=none&width=529)

```
from：基础Image
label：标识（作者）
run：运行命令
expose：暴露端口
entpypoint：程序的起点（启动redis-server）
```

- **Pull from Registry** 拉取 image 镜像
  `docker pull ubuntu:14.04`
  ![image-20191103172931077.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575955716945-b6044a18-d275-4db9-a9dd-39cef23b44cf.png#height=250&id=G7itS&name=image-20191103172931077.png&originHeight=250&originWidth=1122&originalType=binary∶=1&size=167254&status=done&style=none&width=1122)
  `docker镜像地址` ： [https://hub.docker.com/](https://hub.docker.com/)

#### Image 创建

- 创建一个 C 的 hell-word 程序

```shell
$ sudo mkdir hello-word
$ sudo cd hello-word/
$ sudo hello-word]# vim hello.c
hello.c =>
#include<stdio.h>
int main(){
  printf("hello docker\n");
}
$ sudo yum install gcc
$ sudo yum install glibc-static
$ sudo gcc -static hello.c  -o hello
$ sudo ls
hello  hello.c
```

- 创建 Dockerfile 文件

```shell
$ sudo vim Dockerfile
Dockerfile =>
FROM scratch
ADD hello /
CMD ["/hello"]
```

- 编译 DockerFile

```shell
$ sudo docker build -t wenxudong/hello .
Sending build context to Docker daemon  868.9kB
Step 1/3 : FROM scratch
 --->
Step 2/3 : ADD hello /
 ---> 8df0d79a4994
Step 3/3 : CMD ["/hello"]
 ---> Running in 246fe1df6b4b
Removing intermediate container 246fe1df6b4b
 ---> 373d2ce282c2
Successfully built 373d2ce282c2
Successfully tagged wenxudong/hello:latest
```

- 查看

```shell
$ sudo docker image ls
REPOSITORY                                          TAG                 IMAGE ID            CREATED             SIZE
wenxudong/hello                                     latest              373d2ce282c2        2 minutes ago       865kB

$ sudo docker history 373d2ce282c2	//查看docker Image的分层 373d2ce282c2（IMAGE ID）
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
373d2ce282c2        6 minutes ago       /bin/sh -c #(nop)  CMD ["/hello"]               0B
8df0d79a4994        6 minutes ago       /bin/sh -c #(nop) ADD file:8041f15efa9650de5…   865kB
```

- 运行

```shell
$ sudo docker run wenxudong/hello
hello docker
```

#### Image 操作

- 查看 image 列表 `$sudo docker image ls` 或 `$ docker images`
- 查看 image 的分层 `$sudo docker history image_id`
- 删除 image `$ docker image rm 67759sju213(image id)` 或 `$ docker rmi 67759sju213(image id)`
- [https://docs.docker.com/engine/reference/commandline/image/](https://docs.docker.com/engine/reference/commandline/image/)

### 10. Docker Container

#### 什么是 Container

- 通过 Image 创建（copy）
- 在 Image layer 之上建立一个 container layer（可读写）
- 类比面向对象：类（image）和实例（container）
- Image 负责 app 的存储和分发，Container 负责 app 运行

![image-20191104200420240.png](https://cdn.nlark.com/yuque/0/2019/png/656687/1575955788236-296de3a3-19f9-4559-9292-4df4e67bbc40.png#height=412&id=hdpAe&name=image-20191104200420240.png&originHeight=412&originWidth=614&originalType=binary∶=1&size=83764&status=done&style=none&width=614)

#### Container 操作

- 查看当前正在运行的 container（容器） `$ docker container ls` 或 `$ docker ps`
- 列举出当前所有的 container（容器） `$ docker container ls -a` 或 `$ docker ps -a`
- 创建一个 container，运行并退出 `$ docker run centos(镜像名称。centos:7指定版本为7)`
- 创建一个 container，交互式运行 `$ docker run -it image_id`
- 删除一个 container `$ docker container rm 732879e51ecf(container id)` 或 `$ docker rm 732879e51ecf(container id)`
- 创建并**后台运行**container `$ docker run -d centos(image名称或id)`

  > -d：使用时必须在镜像中要有一个常驻内存的进程，比如 nginx，db，不会停的 shell 脚本。
  > 否则使用命令`$ docker run -it -d image_name(id)`即可

- 创建并设置**环境变量**并后台运行 `$ docker run -d -e NAME=WINXUDONG image_name(id)`
- 创建 container 后台运行并**指定名称**：`docker run -d --name=demo image_name(id)` `
- 删除所有 container

```shell
[vagrant@10 ~]$ docker rm $(docker container ls -aq)
b0487b7a51a1
3bfb29a0856f
af858e310d31
4ffe8dc01ed7
a9cd04c8bf49
09bcc2b5a418
[vagrant@10 ~]$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

- 删除所有退出的 container

```shell
[vagrant@10 ~]$ docker rm $(docker container ls -f "status=exited" -q)
a5ddf882b73d
05440bc8ccc4
605292d5ac48
9921e7264142
9844b372a81a
```

- 进入到正在运行的 container 中：`$ docker exec -it dedf40b94b72(container id) /bin/bash`
- 打印正在运行的 container 的 ip 地址：`docker exec -it dedf40b94b72(container id) ip a`
- 停止正在运行的 container：`$ docker stop ded(container id简写)` 或者 `$ docker stop demo(container name)`
- 启动 container：`$ docker start demo(container name或者id)`
- 查看 container 信息：`$ docker inspect dedf40b94b72(container_id)`
- [https://docs.docker.com/engine/reference/commandline/container/](https://docs.docker.com/engine/reference/commandline/container/)

### 11. 构建 Docker 镜像

### 方式一

#### 创建一个交互的 container

```shell
[vagrant@10 ~]$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              0f3e07c0138f        4 weeks ago         220MB
[vagrant@10 ~]$ docker run -it centos
[root@16f742ae68c5 /]#
```

#### 在 container 中进行操作

`[root@16f742ae68c5 /]# yum install -y vim`

#### 退出 container

`[root@16f742ae68c5 /]# exit`

#### 将 container 进行 commit

```shell
[vagrant@10 ~]$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
16f742ae68c5        centos              "/bin/bash"         6 minutes ago       Exited (0) 2 minutes ago                       quizzical_zhukovsky
[vagrant@10 ~]$ docker commit quizzical_zhukovsky winn/centos-vim
sha256:f26be7ad148164c236ad8ec9eed879b5b596158d8594823fb5045f919159407f
```

#### 查看 images 构建完成

```shell
[vagrant@10 ~]$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
winn/centos-vim     latest              f26be7ad1481        About a minute ago   286MB
centos              latest              0f3e07c0138f        4 weeks ago          220MB
```

#### 查看 image 层次结构

```shell
[vagrant@10 ~]$ docker history 0f3e07c0138f
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
0f3e07c0138f        4 weeks ago         /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>           4 weeks ago         /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B
<missing>           4 weeks ago         /bin/sh -c #(nop) ADD file:d6fdacc1972df524a…   220MB
[vagrant@10 ~]$ docker history f26be7ad1481
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
f26be7ad1481        3 minutes ago       /bin/bash                                       66.8MB
0f3e07c0138f        4 weeks ago         /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>           4 weeks ago         /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B
<missing>           4 weeks ago         /bin/sh -c #(nop) ADD file:d6fdacc1972df524a…   220MB
```

### 方式二

#### 创建一个 Dockerfile

```shell
[vagrant@10 ~]$ mkdir docker-centos-vim
[vagrant@10 ~]$ cd docker-centos-vim/
[vagrant@10 docker-centos-vim]$ vim Dockerfile
```

#### Dockerfile 写入内容

```shell
FROM centos
RUN yum install -y vim
```

#### docker build 构建镜像

```shell
[vagrant@10 docker-centos-vim]$ docker build -t winn/centos-vim-new .
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM centos
 ---> 0f3e07c0138f
Step 2/2 : RUN yum install -y vim
 ---> Running in 83e39a2050c8

 Complete!
Removing intermediate container 83e39a2050c8
 ---> f74e84c2089a
Successfully built f74e84c2089a
Successfully tagged winn/centos-vim-new:latest
```

> 因为 image 是只读的，所以在 build 的过程中会先创建一个临时的 container`Running in 83e39a2050c8` 执行 `yum install -y`   完成之后删除临时 container `Removing intermediate container 83e39a2050c8`

### 12. Dockerfile 语法与实践

#### FROM

> FROM 一般应用于 Dockerfile 文件最开始的位置。
> FROM：一般用于指定我们制作的 image 的 base image 是什么，也就是说我们要在哪个 image 之上制作一个新的 image。
> `FROM scratch`  # scratch 表示从头制作制作 base image
> `FROM centos` # 基于 centos 的 base image 制作一个 image

#### LABEL

> `LABEL` maintainer = "[winn@gmail.com](mailto:winn@gmail.com)" # 作者
> `LABEL` version = "1.0" # 版本
> `LABEL` description = "This is description" # 描述

#### RUN

> `RUN 执行命令并创建新的image`

```shell
RUN yum update && yum install -y vim \
		python-dev # 反斜线换行
```

> Run 执行 shell 命令
> 尽量将多条命令合并为一行 反斜线换行

#### WORKDIR

> `WORKDIR`/test # 如果没有会自动创建 test 目录
> `WORKDIR` demo
> `RUN`pwd # 输出结果应该是 /test/demo
> 使用 WORKDIR，不要使用 RUN cd！尽量使用绝对目录！

#### ADD and COPY

> 本地文件添加到 image 里面，ADD 不仅可以添加文件还可以`解压缩`
> 添加远程文件可以试用 `curl`或者`wget`

```shell
ADD hello /

ADD test.tar,gz / #添加到根目录并解压
```

```shell
WORKDIR /root
ADD hello test/	#/root/test/hello

WORKDIR /root
COPY hello test/	#/root/test/hello
```

#### ENV

> 设置常量

```shell
ENV MYSQL_VERSION 5.6	#设置常量
RUN apt-get install -y mysql-server= "${MYSQL_VERSION}"	#引用常量
```

#### VOLUME and EXPOSE

> 存储和网络
> EXPOSE
>
> - 暴露端口 `EXPOSE 2000` (暴露 2000 端口)

#### CMD 与 ENTPYPOINT

> CMD
>
> - `设置容器启动后默认执行的命令和参数` 启动后
> - 如果 docker run 指定了其他命令，CMD 命令会被忽略
>   - 例如：`docker run -it [image] /bin/bash`
>   - 因为 run 时指定了`/bin/bash` 则 CMD 命令不会执行
> - 如果定义了多个 CMD，只会执行最后一个命令
>
> ENTPYPOINT
>
> - `设置容器启动时运行的命令` 启动时
> - 让容器以应用程序或者服务的形式运行
> - 不会被忽略，一定会执行
> - 最佳实践：写一个 shell 脚本作为 entrypoint

```shell
COPY docker-entrypoint.sh /usr/local/bin
ENTPYPOINT ["docker-entrypoint.sh"]

EXPOSE 27017
CMD ["mongodb"]
```

##### Shell 与 Exec 格式

```shell
RUN apt-get install -y vim
CMD echo "hello docker"
ENTPYPOINT echo "hello docker"	#shell
```

```shell
RUN ["age-get", "install", "-y", "vim"]
CMD ["bin/echo", "hello docker"]
ENTPYPOINT ["bin/echo", "hello docker"]	#exec
[]中
第一个为命令 例如age-get，bin/echo
后面的全部为命令的参数 例如install，vim，hello docker
```

#### github dockerfile

[https://github.com/docker-library](https://github.com/docker-library)

#### Dockerfile 官方文档

[https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)

### 13. Image 镜像发布

#### 方式一

#### 登录自己的 docker hub

```shell
[vagrant@10 ~]$ docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
winn/centos-vim-new   latest              f74e84c2089a        2 hours ago         286MB
winn/centos-vim       latest              f26be7ad1481        2 hours ago         286MB
centos                latest              0f3e07c0138f        4 weeks ago         220MB
[vagrant@10 ~]$ docker login
输入用户名密码
```

#### 发布镜像到 docker hub

`$ docker push wenxudong/centos-vim`

> 注意 push 的时候 / 前面的部分必须与登录 docker hub 的用户名一致

#### 方式二

> docker hub 与 github 关联，在 github 上传 Dockerfile，docker 会同步 Dockerfile 然后创建你的 docker 镜像
> 也可在本地搭建 docker hub

### 14. 容器资源限制

- 创建并运行 container 时限制内存：`$ docker run --memory=200M winn/centos-vim`
- 设置容器使用 cpu 的权重：`$ docker run --cpu-shares=10 winn/centos-vim`
- [https://docs.docker.com/config/containers/resource_constraints/](https://docs.docker.com/config/containers/resource_constraints/)

### 15. Docker 底层技术支持

> - Namespaces:：做隔离 pid，net，ipc，uts
> - Control group：做资源限制
> - Union file system：Container 和 Image 分层
