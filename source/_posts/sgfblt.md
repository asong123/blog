---
title: 30、整合Dubbo+Zookeeper
urlname: sgfblt
date: '2021-07-09 20:44:43 +0800'
tags: []
categories: []
---

# 1、基础知识

## 、分布式理论

### 什么是分布式系统？

在《分布式系统原理与范型》一书中有如下定义：“分布式系统是若干独立计算机的集合，这些计算机对 于用户来说就像单个相关系统”；
分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分 布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是**利 用更多的机器，处理更多的数据**。
分布式系统（distributed system）是建立在网络之上的软件系统。
首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的 提升（加内存、加磁盘、使用更好的 CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时 候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于 分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又 会引入更多的机制、协议，带来更多的问题。。。

### Dubbo 文档

随着互联网的发展，网站应用的规模不断扩大，常规的垂直应用架构已无法应对，分布式服务架构以及 流动计算架构势在必行，急需**一个治理系统**确保架构有条不紊的演进。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834684233-a4ff59c2-501e-461f-b99f-4a8cbc43aab3.jpeg#)在 Dubbo 的官网文档有这样一张图

### 单一应用架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简 化增删改查工作量的数据访问框架(ORM)是关键。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834684753-bfb39793-9979-440d-836a-d252c9202fdb.png#)

适用于小型网站，小型管理系统，将所有功能都部署到一个功能里，简单易用。

### 缺点：

1、性能扩展比较难
2、协同开发问题
3、不利于升级维护

### 垂直应用架构

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用拆成互不相干的几个应用，以提 升效率。此时，用于加速前端页面开发的 Web 框架(MVC)是关键。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834685195-c3c00fea-2be6-430e-a238-b598ddc839f4.png#)
通过切分业务来实现各个模块独立部署，降低了维护和部署的难度，团队各司其职更易管理，性能扩展 也更方便，更有针对性。
缺点： 公用模块无法重复利用，开发性的浪费

### 分布式服务架构

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定 的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的**分布式服 务框架(RPC)**是关键。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834685707-81997107-2c35-46c9-ba2b-c3fe48a9faf7.jpeg#)

### 流动计算架构

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问 压力实时管理集群容量，提高集群利用率。此时，用于**提高机器利用率的资源调度和治理中心\*\***(SOA)[ Service Oriented Architecture]是关键\*\*。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834686185-5902d5d4-b50e-456d-9d49-8c4190b2d2f6.png#)

## 、什么是 RPC？

RPC【Remote Procedure Call】是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用 程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用 代码基本相同。
也就是说两台服务器 A，B，一个应用部署在 A 服务器上，想要调用 B 服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。为什么要用 RPC 呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署 应用。RPC 就是要像调用本地的函数一样去调远程函数；
[推荐阅读文章：https://www.jianshu.com/p/2accc2840a1b](https://www.jianshu.com/p/2accc2840a1b)

### RPC 基本原理

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834686498-25ed7d51-930b-40d9-a558-7a933532af99.jpeg#)
**步骤解析：**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834686808-69d0852a-900a-48f7-9416-5e17722f0012.jpeg#)
RPC 两个核心模块：通讯，序列化。

# 2、Dubbo

## 、什么是 dubbo？

Apache Dubbo |ˈdʌbəʊ| 是一款高性能、轻量级的开源 Java RPC 框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。
[dubbo 官网](http://dubbo.apache.org/zh-cn/index.html) 1.了解 Dubbo 的特性 2.查看官方文档**dubbo 基本概念**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834687301-926f658b-7975-4a40-8b22-60672539b6ae.jpeg#)

**服务提供者\*\***（Provider）**：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务。
**服务消费者\***\*（Consumer）**: 调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如 果调用失败，再选另一台调用。
**注册中心\*\***（Registry）**：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者
**监控中心\***\*（Monitor）**：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

### 调用关系说明

l 服务容器负责启动，加载，运行服务提供者。
l 服务提供者在启动时，向注册中心注册自己提供的服务。
l 服务消费者在启动时，向注册中心订阅自己所需的服务。
l 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
l 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
l 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

## 、Dubbo 环境搭建

点进 dubbo 官方文档，推荐我们使用[Zookeeper 注册中心](http://dubbo.apache.org/zh-cn/docs/user/references/registry/introduction.html)什么是 zookeeper 呢？可以查看[官方文档](http://dubbo.apache.org/zh-cn/docs/user/references/registry/zookeeper.html)

## 、window 下安装 zookeeper

1、下载 zookeeper ：[地址](http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.14/)， 我们下载 3.4.14 ， 最新版！ 解压 zookeeper 2、运行/bin/zkServer.cmd ，初次运行会报错，没有 zoo.cfg 配置文件； 可能遇到问题：闪退 !

解决方案：编辑 zkServer.cmd 文件末尾添加方便找到原因。
pause
。这样运行出错就不会退出，会提示错误信息，

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834687785-27359e19-6a2f-419c-bf12-4263286f0fb3.png#)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834688057-13ce44b8-d1b4-42b1-8eb7-de5018b66226.png#)
3、修改 zoo.cfg 配置文件
将 conf 文件夹下面的 zoo_sample.cfg 复制一份改名为 zoo.cfg 即可。注意几个重要位置：
dataDir=./ 临时数据存储的目录（可写相对路径）
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834688338-993517bf-0952-48b8-9b43-7d543bead29b.png#)clientPort=2181 zookeeper 的端口号修改完成后再次启动 zookeeper
4、使用 zkCli.cmd 测试
ls /：列出 zookeeper 根下保存的所有节点

| 1   | [zk: 127.0.0.1:2181(CONNECTED) 4] ls / |
| --- | -------------------------------------- |
| 2   | [zookeeper]                            |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834688590-c56a2cdc-0f5b-4d9c-b5fa-c090b677f363.png#)create –e /kuangshen 123：创建一个 kuangshen 节点，值为 123
get /kuangshen：获取/kuangshen 节点的值

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834688848-34dd6e5f-5b49-402e-ab58-51cefcda467f.png#)

我们再来查看一下节点
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834689067-482e57be-5cec-4b61-841d-32ea4019a470.png#)

## 、window 下安装 dubbo-admin

dubbo 本身并不是一个服务软件。它其实就是一个 jar 包，能够帮你的 java 程序连接到 zookeeper，并利 用 zookeeper 消费、提供服务。
但是为了让用户更好的管理监控众多的 dubbo 服务，官方提供了一个可视化的监控程序 dubbo-admin， 不过这个监控即使不装也不影响使用。
我们这里来安装一下：

### 1、下载 dubbo-admin

地址 ：[https://github.com/apache/dubbo-admin/tree/master](https://github.com/apache/dubbo-admin/tree/master)[
](<[https://github.com/apache/incubator-dubbo-ops](https://github.com/apache/incubator-dubbo-ops)>)

### 2、解压进入目录

修改 dubbo-admin\src\main\resources \application.properties 指定 zookeeper 地址

| 1   | server.port=7001                                  |
| --- | ------------------------------------------------- |
| 2   | spring.velocity.cache=false                       |
| 3   | spring.velocity.charset=UTF-8                     |
| 4   | spring.velocity.layout-url=/templates/default.vm  |
| 5   | spring.messages.fallback-to-system-locale=false   |
| 6   | spring.messages.basename=i18n/message             |
| 7   | spring.root.password=root                         |
| 8   | spring.guest.password=guest                       |
| 9   |                                                   |
| 10  | dubbo.registry.address=zookeeper://127.0.0.1:2181 |

**3、在项目目录下**打包 dubbo-admin

| 1   | mvn | clean | package | -Dmaven.test.skip=true |
| --- | --- | ----- | ------- | ---------------------- |

### 第一次打包的过程有点慢，需要耐心等待！直到成功！

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834689416-09df0716-493c-4703-b38f-c5bcb1367337.png#)
4、执行 dubbo-admin\target 下的 dubbo-admin-0.0.1-SNAPSHOT.jar

java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
1

【注意：zookeeper 的服务一定要打开！】
执行完毕，我们去访问一下 http://localhost:7001/ ， 这时候我们需要输入登录账户和密码，我们都是默认的 root-root；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834690055-f7765668-4aa4-4991-83bd-89b4cbbfb561.jpeg#)登录成功后，查看界面
安装完成！

# 3、SpringBoot + Dubbo + zookeeper

## 、框架搭建

### 启动 zookeeper ！

1. **IDEA 创建一个空项目；**

### 创建一个模块，实现服务提供者：provider-server ， 选择 web 依赖即可

1. **项目创建完毕，我们写一个服务，比如卖票的服务；**

编写接口

package com.kuang.provider.service;

public interface TicketService { public String getTicket();
}
1
2
3
4
5

编写实现类

| 1   | package com.kuang.provider.service;                       |
| --- | --------------------------------------------------------- |
| 2   |                                                           |
| 3   | public class TicketServiceImpl implements TicketService { |
| 4   | @Override                                                 |
| 5   | public String getTicket() {                               |
| 6   | return "《狂神说 Java》";                                 |
| 7   | }                                                         |
| 8   | }                                                         |

### 创建一个模块，实现服务消费者：consumer-server ， 选择 web 依赖即可

1. **项目创建完毕，我们写一个服务，比如用户的服务；**

编写 service

package com.kuang.consumer.service;

public class UserService {
//我们需要去拿去注册中心的服务
}
1
2
3
4
5

**需求：现在我们的用户想使用买票的服务，这要怎么弄呢 ？**

## 、服务提供者

### 1、将服务提供者注册到注册中心，我们需要整合 Dubbo 和 zookeeper，所以需要导包

**我们从 dubbo 官网进入 github，看下方的帮助文档，找到 dubbo-springboot，找到依赖包**

<!-- Dubbo Spring Boot Starter -->
<dependency>
<groupId>org.apache.dubbo</groupId>
<artifactId>dubbo-spring-boot-starter</artifactId>
<version>2.7.3</version>
</dependency>
1
2
3
4
5
6

### zookeeper 的包我们去 maven 仓库下载，zkclient；

<!-- [https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient](https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient) -->
<dependency>
<groupId>com.github.sgroschupf</groupId>
<artifactId>zkclient</artifactId>
<version>0.1</version>
</dependency>
1
2
3
4
5
6
7

**【新版的坑】zookeeper 及其依赖包，解决日志冲突，还需要剔除日志依赖；**

| 1   | <!-- 引入zookeeper --> |
| --- | ---------------------- |
| 2   | <dependency>           |

<groupId>org.apache.curator</groupId>
<artifactId>curator-framework</artifactId>
<version>2.12.0</version>
</dependency>
<dependency>
<groupId>org.apache.curator</groupId>
<artifactId>curator-recipes</artifactId>
<version>2.12.0</version>
</dependency>
<dependency>
<groupId>org.apache.zookeeper</groupId>
<artifactId>zookeeper</artifactId>
<version>3.4.14</version>

<!--排除这个slf4j-log4j12-->
<exclusions>
<exclusion>
<groupId>org.slf4j</groupId>
<artifactId>slf4j-log4j12</artifactId>
</exclusion>
</exclusions>
</dependency>
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
18
19
20
21
22
23
24

### 2、在 springboot 配置文件中配置 dubbo 相关属性！

| 1   | #当前应用名字                                       |
| --- | --------------------------------------------------- |
| 2   | dubbo.application.name=provider-server              |
| 3   | #注册中心地址                                       |
| 4   | dubbo.registry.address=zookeeper://127.0.0.1:2181   |
| 5   | #扫描指定包下服务                                   |
| 6   | dubbo.scan.base-packages=com.kuang.provider.service |
| 7   |                                                     |

**3、在 service 的实现类中配置服务注解，发布服务！注意导包问题**

| 1   | import org.apache.dubbo.config.annotation.Service;        |
| --- | --------------------------------------------------------- |
| 2   | import org.springframework.stereotype.Component;          |
| 3   |                                                           |
| 4   | @Service //将服务发布出去                                 |
| 5   | @Component //放在容器中                                   |
| 6   | public class TicketServiceImpl implements TicketService { |
| 7   | @Override                                                 |
| 8   | public String getTicket() {                               |
| 9   | return "《狂神说 Java》";                                 |
| 10  | }                                                         |
| 11  | }                                                         |
| 12  |                                                           |

**逻辑理解 ： 应用启动起来，dubbo 就会扫描指定的包下带有@component 注解的服务，将它发布在指定的注册中心中！**

## 、消费者

### 1、导入依赖，和之前的依赖一样；

| 1   | <!--dubbo-->                       |
| --- | ---------------------------------- |
| 2   | <!-- Dubbo Spring Boot Starter --> |

| 3   | <dependency>                                                                                                                                |     |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| 4   | <groupId>org.apache.dubbo</groupId>                                                                                                         |     |
| 5   | <artifactId>dubbo-spring-boot-starter</artifactId>                                                                                          |     |
| 6   | <version>2.7.3</version>                                                                                                                    |     |
| 7   | </dependency>                                                                                                                               |     |
| 8   | <!--zookeeper-->                                                                                                                            |     |
| 9   | <!-- [https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient](https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient) | --> |
| 10  | <dependency>                                                                                                                                |     |
| 11  | <groupId>com.github.sgroschupf</groupId>                                                                                                    |     |
| 12  | <artifactId>zkclient</artifactId>                                                                                                           |     |
| 13  | <version>0.1</version>                                                                                                                      |     |
| 14  | </dependency>                                                                                                                               |     |
| 15  | <!-- 引入zookeeper -->                                                                                                                      |     |
| 16  | <dependency>                                                                                                                                |     |
| 17  | <groupId>org.apache.curator</groupId>                                                                                                       |     |
| 18  | <artifactId>curator-framework</artifactId>                                                                                                  |     |
| 19  | <version>2.12.0</version>                                                                                                                   |     |
| 20  | </dependency>                                                                                                                               |     |
| 21  | <dependency>                                                                                                                                |     |
| 22  | <groupId>org.apache.curator</groupId>                                                                                                       |     |
| 23  | <artifactId>curator-recipes</artifactId>                                                                                                    |     |
| 24  | <version>2.12.0</version>                                                                                                                   |     |
| 25  | </dependency>                                                                                                                               |     |
| 26  | <dependency>                                                                                                                                |     |
| 27  | <groupId>org.apache.zookeeper</groupId>                                                                                                     |     |
| 28  | <artifactId>zookeeper</artifactId>                                                                                                          |     |
| 29  | <version>3.4.14</version>                                                                                                                   |     |
| 30  | <!--排除这个slf4j-log4j12-->                                                                                                                |     |
| 31  | <exclusions>                                                                                                                                |     |
| 32  | <exclusion>                                                                                                                                 |     |
| 33  | <groupId>org.slf4j</groupId>                                                                                                                |     |
| 34  | <artifactId>slf4j-log4j12</artifactId>                                                                                                      |     |
| 35  | </exclusion>                                                                                                                                |     |
| 36  | </exclusions>                                                                                                                               |     |
| 37  | </dependency>                                                                                                                               |     |
| 38  |                                                                                                                                             |     |

2、**配置参数**

| 1   | #当前应用名字                                     |
| --- | ------------------------------------------------- |
| 2   | dubbo.application.name=consumer-server            |
| 3   | #注册中心地址                                     |
| 4   | dubbo.registry.address=zookeeper://127.0.0.1:2181 |
| 5   |                                                   |

### 本来正常步骤是需要将服务提供者的接口打包，然后用 pom 文件导入，我们这里使用简单的方式，直 接将服务的接口拿过来，路径必须保证正确，即和服务提供者相同；

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834690350-2c3d7f24-5375-4ce3-b006-e66985755aec.png#)

1. **完善消费者的服务类**

| 1
2
3
4
5
6
7
8
9
10 | package com.kuang.consumer.service;

import com.kuang.provider.service.TicketService; import org.apache.dubbo.config.annotation.Reference; import org.springframework.stereotype.Service;

@Service //注入到容器中 public class UserService {

@Reference //远程引用指定的服务，他会按照全类名进行匹配，看谁给注册中心注册了这个全
类名 | |
| --- | --- | --- |
| 11 | | TicketService ticketService; |
| 12 | | |
| 13 | | public void bugTicket(){ |
| 14 | | String ticket = ticketService.getTicket(); |
| 15 | | System.out.println("在注册中心买到"+ticket); |
| 16 | | } |
| 17 | | |
| 18 | } | |
| 19 | | |

1. **测试类编写；**

@RunWith(SpringRunner.class) @SpringBootTest
public class ConsumerServerApplicationTests {

@Autowired
UserService userService;

@Test
public void contextLoads() {

userService.bugTicket();

}

}
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
16

## 、启动测试

### 开启 zookeeper

1. **打开 dubbo-admin 实现监控【可以不用做】**

### 开启服务者

1. **消费者消费测试，结果：**

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834690603-8d66b317-62ed-4836-bf67-4d958a46ac7f.png#)

### 监控中心 ：

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834691004-98e57792-3b20-4a60-ab09-2e7a1dc8997d.png#)

**ok , 这就是 SpingBoot + dubbo + zookeeper 实现分布式开发的应用，其实就是一个服务拆分的思想；**
