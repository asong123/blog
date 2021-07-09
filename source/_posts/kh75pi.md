---
title: 41、Docker下
urlname: kh75pi
date: '2021-07-09 20:51:59 +0800'
tags: []
categories: []
---

# 狂神说 Docker（下半场）

版权声明：关于狂神说 Java
不为任何机构站台，编程是爱好，恭喜你发现宝藏男孩一枚~希望你们关注我是因为喜欢我！ 所有的课程都是免费的，任何利用我课程收费的都是骗子，请大家注意！
B 站唯一账号：狂神说 Java 唯一公众号：狂神说
学习前，三连关注分享支持，是最基本的尊重，拒绝白嫖！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835121550-a07b0d05-172d-4328-86a1-d99c80290282.jpeg#)

所有学习，官网！

# Docker Compose

## 简介

Docker
DockerFile build run 手动操作，单个容器！ 微服务。100 个微服务！依赖关系。
Docker Compose 来轻松高效的管理容器 i。定义运行多个容器。

官方介绍
定义、运行多个容器。
YAML ﬁle 配置文件。
single command。 命令有哪些？
Compose is a tool for deﬁning and running multi-container Docker applications. With Compose, you use a YAML ﬁle to conﬁgure your application’s services. Then, with a single command, you create and start all the services from your conﬁguration. To learn more about all the features of Compose, see [the list of features](https://docs.docker.com/compose/#features).
所有的环境都可以使用 Compose。
Compose works in all environments: production, staging, development, testing, as well as CI workﬂows. You can learn more about each case in [Common Use Cases](https://docs.docker.com/compose/#common-use-cases).

#### 三步骤：

Using Compose is basically a three-step process:

1. Deﬁne your app’s environment with a

Dockerfile
Dockerﬁle 保证我们的项目在任何地方可以运行。

1. Deﬁne the services that make up your app in together in an isolated environment.

docker-compose.yml
services 什么是服务。
docker-compose.yml 这个文件怎么写！
docker-compose up

so it can be reproduced anywhere.

so they can be run

1. Run

启动项目
and Compose starts and runs your entire app.

作用：批量容器编排。
我自己理解
Compose 是 Docker 官方的开源项目。需要安装！
让程序在任何地方运行。 web 服务。 redis、mysql、nginx ... 多个容器。 run
Dockerfile
Compose

version: '2.0' services:
web:
build: . ports:

- "5000:5000"
  volumes:

- .:/code
- logvolume01:/var/log links:
- redis redis:

image: redis volumes:
logvolume01: {}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15

docker-compose up 100 个服务。
Compose ：重要的概念。
服务 services， 容器。应用。（web、redis、mysql. ）
项目 project。 一组关联的容器。 博客。web、mysql。

## 安装

1、下载

| 1   | sudo curl -L                                                                                                                                       |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
|     | "https://github.com/docker/compose/releases/download/1.26.2/docker-                                                                                |
|     | compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose                                                                                  |
| 2   |                                                                                                                                                    |
| 3   | # 这个可能快点！                                                                                                                                   |
| 4   | curl -L                                                                                                                                            |
|     | [https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-](https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-) |
|     | compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose                                                                                      |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835122007-05584159-9053-4c30-b2e5-339104b085e1.png#)
2、授权

| 1   | sudo | chmod | +x  | /usr/local/bin/docker-compose |
| --- | ---- | ----- | --- | ----------------------------- |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835122442-4625dcca-25e2-4cc4-9ccc-fc25ee2b05e2.png#)

多看官网.....

## 体验

地址：[https://docs.docker.com/compose/gettingstarted/](https://docs.docker.com/compose/gettingstarted/) python 应用。计数器。 redis！

1、应用 app.py
2、Dockerﬁle 应用打包为镜像
3、Docker-compose yaml 文件 （定义整个服务，需要的环境。 web、redis） 完整的上线服务！
4、启动 compose 项目（docker-compose up）

流程：
1、创建网络
2、执行 Docker-compose yaml 3、启动服务。
Docker-compose yaml
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835122973-022c6cef-8030-42a3-ac14-7ea3d90861b1.png#)Creating composetest_web_1 ... done Creating composetest_redis_1 ... done
1、文件名 composetest 2、服务

version: '3' services:
web:
build: . ports:

- "5000:5000"
  redis:
  image: "redis:alpine"
  1
  2
  3
  4
  5
  6
  7
  8

自动的默认规则？

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835123470-2f838ced-8e9e-4858-b829-91ab6ce4a54c.png#)
docker imgaes
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835124006-a1fd6a9a-45e3-4bd5-a7d8-e12069507671.png#)

| 1   | [root@kuangshen ~]# docker service ls                                     |
| --- | ------------------------------------------------------------------------- |
| 2   | Error response from daemon: This node is not a swarm manager. Use "docker |
|     | swarm init" or "docker swarm join" to connect this node to swarm and try  |
|     | again.                                                                    |

默认的服务名 文件名*服务名 * num
多个服务器。集群。A B \_num 副本数量服务 redis 服务 => 4 个副本。
集群状态。服务都不可能只有一个运行实例。 弹性、10 HA 高并发。
kubectl service 负载均衡。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835124537-9e366186-776e-40cf-87bb-881ba904515e.png#)3、网络规则
10 个服务 => 项目 （项目中的内容都在同个网络下。域名访问）

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835125011-7c919404-1408-4cfb-8b26-004cd9db4197.png#)
如果在同一个网络下，我们可以直接通过域名访问。
HA！

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835125554-903e4307-d9dc-4643-aaae-0a98265a6f09.png#)停止： docker-compose down ctrl+c

docker-compose
以前都是单个 docker run 启动容器。
docker-compose。 通过 docker-compose 编写 yaml 配置文件、可以通过 compose 一键启动所有服务，停止。！

#### Docker 小结：

1、Docker 镜像。 run => 容器
2、DockerFile 构建镜像（服务打包）
3、docker-compose 启动项目（编排、多个微服务/环境）
4、Docker 网络

**yaml 规则**
docker-compose.yaml 核心。！
[https://docs.docker.com/compose/compose-ﬁle/#compose-ﬁle-structure-and-examples](https://docs.docker.com/compose/compose-file/#compose-file-structure-and-examples)

1 # 3 层！
2

version: '' # 版本
services: # 服务服务 1: web

# 服务配置 images build

network
.....
服务 2: redis
....
服务 3: redis

# 其他配置 网络/卷、全局规则

volumes: networks: configs:
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835126100-ceeacf69-777a-4f68-8b0f-7e4e730f340d.jpeg#)

学习，要掌握规律！
只要多写，多看。 compose.yaml 配置。！
1、官网文档
[https://docs.docker.com/compose/compose-ﬁle/#specifying-durations](https://docs.docker.com/compose/compose-file/#specifying-durations) 2、开源项目 compose.yaml
redis、mysql、mq！

## 开源项目

### 博客

下载程序、安装数据库、配置.....
compose 应用。=> 一键启动！
1、下载项目（docker-compose.yaml） 2、如果需要文件。Dcokerﬁle
3、文件准备齐全（直接一键启动项目！）

前台启动
docker -d
docker-compose up -d
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835126640-dd8d14f7-4666-4b9e-8743-2390ccf4355f.png#)
一切都很简单！

## 实战

1、编写项目微服务
2、dockerﬁle 构建镜像
3、docker-compose.yaml 编排项目
4、丢到服务器 docker-compose up

#### 小结：

未来项目只要有 docker-compose 文件。 按照这个规则，启动编排容器。！ 公司： docker-compose。 直接启动。
网上开源项目： docker-compose 一键搞定。
假设项目要重新部署打包

| 1   | docker-compose | up  | --build | #   | 重新构建！ |
| --- | -------------- | --- | ------- | --- | ---------- |

#### 总结：

**工程、服务、容器**
项目 compose：三层
工程 Porject
服务 服务
容器 运行实例！ docker k8s 容器.

Docker Compose 搞定！

# Docker Swarm

集群

## 购买服务器

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835127172-a651e045-47e7-41ce-a5f3-a63e0e5c4e12.jpeg#)
4 台服务器 2G！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835127634-f66de3d0-387d-434a-9919-ae42c083b3e1.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835128203-f1951bd7-5afd-47d7-b57f-0205883b3fbb.jpeg#)
到此，服务器购买完毕！1 主，3 从！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835128803-3265c57e-735f-4b84-a473-d21e0b7c69e6.png#)

## 4 台机器安装 Docker

和我们单机安装一样
技巧：xshell 直接同步操作，省时间。！

## 工作模式

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835129376-15f11698-2c1e-4886-bc5f-2fb0028f5bcb.jpeg#)

**搭建集群**

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835129837-96a52b09-bcc6-4cfb-a189-0c66f42b44b0.png#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835130396-1102482c-5408-4089-a0e2-7f9b798abaad.png#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835130923-f949a79d-a4ab-4246-bce9-928301717c8a.png#)

私网、公网！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835131362-e5f944ae-38cb-4dac-a6cd-2da752ef5ea5.png#)172.24.82.149 用自己的！
初始化节点
docker swarm join 加入 一个节点！
docker swarm init

| 1   | # 获取令牌   |            |         |
| --- | ------------ | ---------- | ------- |
| 2   | docker swarm | join-token | manager |
| 3   | docker swarm | join-token | worker  |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835131867-8b77ed79-7f01-4151-b765-7158969c7878.png#)
把后面的节点都搭建进去！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835132522-2c51bf57-2b67-4c53-a916-c5438e9eaea4.jpeg#)
100 台！
1、生成主节点 init
2、加入（管理者、worker） 目标：双主双从！

## Raft 协议

双主双从： 假设一个节点挂了！其他节点是否可以用！
Raft 协议： 保证大多数节点存活才可以用。 只要>1 ，集群至少大于 3 台！ 实验：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835132934-fb39cffb-fbef-440a-9abe-064c6393ccd3.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835133445-6c115999-b4c9-415a-bda2-f321f58d9d30.png#)1、将 docker1 机器停止。宕机！ 双主，另外一个主节点也不能使用了！
2、可以将其他节点离开

3、work 就是工作的、管理节点操作！ 3 台机器设置为了管理节点。十分简单：集群，可用！ 3 个主节点。 > 1 台管理节点存活！
Raft 协议： 保证大多数节点存活，才可以使用，高可用！

## 体会

弹性、扩缩容！集群！ 以后告别 docker run！
docker-compose up！ 启动一个项目。单机！ 集群： swarm
docker serivce
容器 => 服务！
容器 => 服务！=> 副本！
redis 服务 => 10 个副本！（同时开启 10 个 redis 容器） 体验：创建服务、动态扩展服务、动态更新服务。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835133988-09a7fd08-2173-4ac2-9d61-5849460d7f5e.png#)
灰度发布：金丝雀发布！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835134503-dfc207c9-627b-4ae3-a522-d92137eb43ac.png#)

| 1   | docker | run 容器启动！不具有扩缩容器            |
| --- | ------ | --------------------------------------- |
| 2   | docker | service 服务！ 具有扩缩容器，滚动更新！ |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835135023-30badbe1-3856-4aca-a3cb-8c613fb3f876.jpeg#)查看服务 REPLICAS
动态扩缩容

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835135575-d2e3f24c-0a4e-45dc-8f9e-46fc85e39d14.jpeg#)

服务，集群中任意的节点都可以访问。服务可以有多个副本动态扩缩容实现高可用！ 弹性、扩缩容！
10 台！ 10000 台！ 卖给别人！ 虚拟化！ 服务的高可用，任何企业，云！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835136083-8acd4f42-3688-4bc7-a5f3-80194ac7b7a4.png#)

移除！
docker swarm 其实并不难
只要会搭建集群、会启动服务、动态管理容器就可以了！

## 概念总结

#### swarm

集群的管理和编号。 docker 可以初始化一个 swarm 集群，其他节点可以加入。（管理、工作者）

#### Node

就是一个 docker 节点。多个节点就组成了一个网络集群。（管理、工作者）

#### Service

任务，可以在管理节点或者工作节点来运行。核心。！用户访问！

#### Task

容器内的命令，细节任务！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835136570-c7776669-04da-41d8-945f-ee13faa931ad.jpeg#)

#### 逻辑是不变的。

命令 -> 管理 -> api -> 调度 -> 工作节点（创建 Task 容器维护创建！）

服务副本与全局服务

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835136974-5157e747-4de7-4ab9-bb11-fd692bca271c.jpeg#)

调整 service 以什么方式运行

| 1   | --mode string                                                         |
| --- | --------------------------------------------------------------------- |
| 2   | Service mode (replicated or global) (default "replicated")            |
| 3   |                                                                       |
| 4   | docker service create --mode replicated --name mytom tomcat:7 默认的  |
| 5   |                                                                       |
| 6   | docker service create --mode global --name haha alpine ping baidu.com |
| 7   | #场景？日志收集                                                       |
| 8   | 每一个节点有自己的日志收集器，过滤。把所有日志最终再传给日志中心      |
| 9   | 服务监控，状态性能。                                                  |

拓展：网络模式： "PublishMode": "ingress" Swarm:
Overlay:
ingress : 特殊的 Overlay 网络！ 负载均衡的功能！ IPVS VIP！

虽然 docker 在 4 台机器上，实际网络是同一个！ ingress 网络 ，是一个特殊的 Overlay 网络

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835137696-ee0a87eb-ac9a-4ca8-9392-22e52599fc49.png#)

整体！

k8s！

# Docker Stack

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835138055-cb99277d-3d4a-415b-907a-ce6cd2a45b30.png#)

docker-compose 单机部署项目！
Docker Stack 部署，集群部署！

| 1   | # 单机                              |
| --- | ----------------------------------- |
| 2   | docker-compose up -d wordpress.yaml |
| 3   | # 集群                              |
| 4   | docker stack deploy wordpress.yaml  |
| 5   |                                     |

| 6   | # docker-compose 文件     |       |
| --- | ------------------------- | ----- |
| 7   | version: '3.4'            |       |
| 8   | services:                 |       |
| 9   | mongo:                    |       |
| 10  | image: mongo              |       |
| 11  | restart: always           |       |
| 12  | networks:                 |       |
| 13  | - mongo_network           |       |
| 14  | deploy:                   |       |
| 15  | restart_policy:           |       |
| 16  | condition: on-failure     |       |
| 17  | replicas: 2               |       |
| 18  | mongo-express:            |       |
| 19  | image: mongo-express      |       |
| 20  | restart: always           |       |
| 21  | networks:                 |       |
| 22  | - mongo_network           |       |
| 23  | ports:                    |       |
| 24  | - target: 8081            |       |
| 25  | published: 80             |       |
| 26  | protocol: tcp             |       |
| 27  | mode: ingress             |       |
| 28  | environment:              |       |
| 29  | ME_CONFIG_MONGODB_SERVER: | mongo |

| 30
31
32
33
34
35
36
37 | ME_CONFIG_MONGODB_PORT: 27017
deploy: restart_policy:
condition: on-failure replicas: 1
networks: mongo_network:
external: true | |

# Docker Secret

安全！配置密码，证书！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835138387-dd460b06-3338-45f2-86f3-aee2c6961d20.png#)

# Docker Conﬁg

配置
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835138803-a70a17dc-36c1-440f-a8ef-306adc492575.jpeg#)

#### Docker 下半场！

Docker Compose Swarm！ 了解，学习方式：
网上找案例跑起来试试！查看命令帮助文档 --help， 官网！

# 拓展到 K8S

### 云原生时代

Go 语言！必须掌握！ Java、Go！ 并发语言！
B 语言 C 语言的创始人 。Unix 创始人 V8
[**Go**（又称 **Golang**）是 Google 的 Robert Griesemer，Rob Pike 及 Ken Thompson 开发的一种静态强类型、](https://baike.baidu.com/item/%E5%BC%BA%E7%B1%BB%E5%9E%8B)[编译型语言](https://baike.baidu.com/item/%E7%BC%96%E8%AF%91%E5%9E%8B%E8%AF%AD%E8%A8%80/9564109)[。Go 语言语法与 ](https://baike.baidu.com/item/%E5%BC%BA%E7%B1%BB%E5%9E%8B)[C ](https://baike.baidu.com/item/C/7252092)[相近，但功能上有：内存安全，](https://baike.baidu.com/item/%E5%BC%BA%E7%B1%BB%E5%9E%8B)[GC](https://baike.baidu.com/item/GC/66426)[（垃圾回收），](https://baike.baidu.com/item/%E5%BC%BA%E7%B1%BB%E5%9E%8B)[结构形态](https://baike.baidu.com/item/%E7%BB%93%E6%9E%84%E5%BD%A2%E6%80%81/5942010)[及](https://baike.baidu.com/item/%E5%BC%BA%E7%B1%BB%E5%9E%8B)CSP-style [并发计算](https://baike.baidu.com/item/%E5%B9%B6%E5%8F%91%E8%AE%A1%E7%AE%97/9939802)。
go 指针！

罗伯特·格瑞史莫（Robert Griesemer），罗勃·派克（Rob Pike）及[肯·汤普逊](https://baike.baidu.com/item/%E8%82%AF%C2%B7%E6%B1%A4%E6%99%AE%E9%80%8A/7585160)（Ken Thompson）于 2007 年 9 月开始设计 Go，稍后 Ian Lance Taylor、Russ Cox 加入项目。Go 是基于[Inferno](https://baike.baidu.com/item/Inferno)操作系统所开发的。Go 于 2009 年 11 月正式宣布推出，成为开放源代码项目，并在[Linux](https://baike.baidu.com/item/Linux)及[Mac OS X](https://baike.baidu.com/item/Mac%20OS%20X)平台上进行了实现，后来追加了 Windows 系统下的实现。在 2016 年，Go 被软件评价公司 TIOBE 选为“TIOBE 2016 年最佳语言”。 目前，Go 每半年发布一个二级版本（即从 a.x 升级到 a.y）

java => Go zijie => Go
