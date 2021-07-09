---
title: 32、SpringCloudNetflix-H版
urlname: bn84il
date: '2021-07-09 20:45:15 +0800'
tags: []
categories: []
---

SpringCloud

# 聊聊现在和未来

这是我们学习到最后的微服务架构 SpringCloud； 大家目前应该已经掌握如下知识
MyBatis Spring SpringMVC SpringBoot Maven，Git Ajax，JSON Vue
。。。等等技术
**聊聊现在和未来**，回顾以前，架构！

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
17
三层架构 + MVC
架构 -----> 结构
开发框架
Spring
IOC
AOP
IOC: 控制翻转约泡：
泡温泉，泡茶 ，泡友
附近的人，打招呼。加微信，聊聊，天天聊，---> 约泡浴场： 温泉，茶庄...
直接进温泉，就有人和你一起了！
原来我们都是自己一步步操作，现在交给容器了！我们需要什么就去拿就可以了

AOP：切面 (本质：动态代理)
为了解决什么？不影响业务本来的情况下，实现动态增加功能，大量应用在日志，
事务，等方面
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
Spring 是一个轻量级的 Java 开源框架，容器
目的：解决企业开发的复杂性问题
Spring 是春天，觉他是春天，但是随着时间推移，也开始慢慢复杂起来，配置文件多！
SpringBoot
SpringBoot 并不是新东西，就是 Spring 的升级版！
新一代 JavaEE 的开发标准，开箱即用！-> 拿过来就可以用！ 它自动帮我们配置了非常多的东西，我们拿来即用！
特性：约定大约配置！
随着公司体系越来越大，用户越来越多！ 微服务架构 ---> 新架构
模块化！功能化！
用户，支付，签到...；
人越来越多：一台服务器解决不了了；我们可以再增加一台服务器， 横向扩展假设 A 服务器占用了 98%的资源，B 服务器只占用了 10%。 --负载均衡

36

1.      将原来的整体项目（all in one），分成模块化，用户管理就是一个单独的项目，签到也是一个单独的项目；项目之间需要进行通信？那如何通信？
1.  用户非常多，签到十分少！ 给用户多一点服务器，给签到少一点服务器！

39

1. 微服务架构问题？
1. 分布式会遇到的四个核心问题？
1. 1. 这么多服务，客户端该如何去访问？
1. 2. 这么多服务，服务之间如何进行通信？
1. 3. 这么多服务，如何治理呢？
1. 4. 服务挂了，怎么办？

46

1. 解决方案：
1. SpringCloud，是一套生态，就是来解决以上分布式架构的 4 个问题
1. 想使用 SpringCloud，必须要掌握 SpringBoot，因为 SpringCloud 是基于 SpringBoot；

50

1. 1.Spring Cloud NetFlix，出来了一套解决方案！一站式解决方案。我们需要的东西它都有。
1. api 网关，zuul 组件
1. Feign-->HttpClient-->Http 的通信方式，同步并阻塞
1. 服务注册与发现，Eureka
1. 熔断机制，Hystrix 56

57 2018 年年底，NetFlix 宣布无限期停止维护，生态不再维护，就会脱节。58

1. 2.Apache Dubbo zookeeper，第二套解决方案
1. API：没有！要么找第三方组件，要么自己实现
1. Dubbo 是一个高性能的基于 Java 实现的 RPC 通信框架！ 2.6.X
1. 服务注册与发现，zookeeper：动物园管理者（Hadoop，Hive）
1. 没有：借助 Hystrix 64

65 Dubbo 这个并不完善！ 66
67 3.Spring Cloud Alibaba 一站式解决方案！
68
69

1. 目前又推出服务网格的概念
1. 服务网格：下一代微服务标准，Server Mesh
1. 代表解决方案：istio（你们未来可能需要掌握！）

73

1. 万变不离其宗，一通百通！
1. 1.API 网关，服务路由
1. 2.HTTP，RPC 框架，异步调用
1. 3.服务注册与发现，高可用
1. 4.熔断机制，服务降级 79

80 如果，你们基于这些问题，研发出一套解决方案，也可叫 SpringCloud！
81
82 为什么要解决这些问题？ 本质：网络是不可靠的！
83
84 程序猿！

问大家几个问题
1、什么是微服务？
2、微服务之间是如何独立通讯的？

3、SpringCloud 和 Dubbo 有哪些区别？
4、SpringBoot 和 SpringCloud，请你谈谈对他们的理解
5、什么是服务熔断？什么是服务降级
6、微服务的优缺点是分别是什么？说下你在项目开发中遇到的坑
7、你所知道的微服务技术栈有哪些？请列举一二
8、eureka 和 zookeeper 都可以提供服务注册与发现的功能，请说说两个的区别？

# 微服务概述

什么是微服务
**什么是微服务？**微服务（Microservice Architecture）是近几年流行的一种架构思想，关于它的概念很难一言以蔽之。究竟什么是微服务呢？我们在此引用 ThoughtWorks 公司的首席科学家 Martin Fowler 于 2014 年提出的一段话：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834720008-c441ad31-8a4e-4f5a-9bd6-897c7221540c.jpeg#)
原 文 ：[https://martinfowler.com/articles/microservices.html ](https://martinfowler.com/articles/microservices.html)汉化：[https://www.cnblogs.com/liuning8023/p/4493156.html ](https://www.cnblogs.com/liuning8023/p/4493156.html)就目前而言，对于微服务，业界并没有一个统一的，标准的定义
但通常而言，微服务架构是一种架构模式，或者说是一种架构风格， **它提倡将单一的应用程序划分成一组小的服务**，每个服务运行在其独立的自己的进程内，服务之间互相协调，互相配置，为用户提供最终 价值。服务之间采用轻量级的通信机制互相沟通，每个服务都围绕着具体的业务进行构建，并且能够被 独立的部署到生产环境中，另外，应尽量避免统一的，集中式的服务管理机制，对具体的一个服务而 言，应根据业务上下文，选择合适的语言，工具对其进行构建，可以有一个非常轻量级的集中式管理来 协调这些服务，可以使用不同的语言来编写服务，也可以使用不同的数据存储；

## 可能有的人觉得官方的话太过生涩，我们从技术维度来理解下：

微服务化的核心就是将传统的一站式应用，根据业务拆分成一个一个的服务，彻底地去耦合，每一个微 服务提供单个业务功能的服务，一个服务做一件事情，从技术角度看就是一种小而独立的处理过程，类 似进程的概念，能够自行单独启动或销毁，拥有自己独立的数据库。

微服务与微服务架构

## 微服务

强调的是服务的大小，他关注的是某一个点，是具体解决某一个问题/提供落地对应服务的一个服务应 用，狭义的看，可以看做是 IDEA 中的一个个微服务工程，或者 Moudel

## 微服务架构

一种新的架构形式，Martin Fowler，2014 提出！
微服务架构是一种架构模式，它提倡将单一应用程序划分成一组小的服务，服务之间互相协调，互相配 合，为用户提供最终价值。每个服务运行在其独立的进程中，服务于服务间采用轻量级的通信机制互相 协作，每个服务都围绕着具体的业务进行构建，并且能够被独立的部署到生产环境中，另外，应尽量避 免统一的，集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言，工 具对其进行构建。

微服务优缺点

## 优点

每个服务足够内聚，足够小，代码容易理解，这样能聚焦一个指定的业务功能或业务需求； 开发简单，开发效率提高，一个服务可能就是专一的只干一件事；
微服务能够被小团队单独开发，这个小团队是 2~5 人的开发人员组成；
微服务是松耦合的，是有功能意义的服务，无论是在开发阶段或部署阶段都是独立的。 微服务能使用不同的语言开发。
易于和第三方集成，微服务允许容易且灵活的方式集成自动部署，通过持续集成工具，如 jenkins，
Hudson，bamboo
微服务易于被一个开发人员理解，修改和维护，这样小团队能够更关注自己的工作成果。无需通过 合作才能体现价值。
微服务允许你利用融合最新技术。

## 微服务只是业务逻辑的代码，不会和 HTML ， CSS 或其他界面混合

**每个微服务都有自己的存储能力，可以有自己的数据库，也可以有统一数据库**
**缺点：**
开发人员要处理分布式系统的复杂性
多服务运维难度，随着服务的增加，运维的压力也在增大系统部署依赖
服务间通信成本数据一致性
系统集成测试性能监控.....

微服务技术栈有哪些？

| **微服务条目**                           | **落地技术**                                                   |
| ---------------------------------------- | -------------------------------------------------------------- |
| 服务开发                                 | SpringBoot,Spring,SpringMVC                                    |
| 服务配置与管理                           | Netﬂix 公司的 Archaius、阿里的 Diamond 等                      |
| 服务注册与发现                           | Eureka、Consul、Zookeeper 等                                   |
| 服务调用                                 | Rest、RPC、gRPC                                                |
| 服务熔断器                               | Hystrix、Envoy 等                                              |
| 负载均衡                                 | Ribbon、Nginx 等                                               |
| 服务接口调用（客户端调用服务的简化工具） | Feign 等                                                       |
| 消息队列                                 | Kafka、RabbitMQ、ActiveMQ 等                                   |
| 服务配置中心管理                         | SpringCloudConﬁg、Chef 等                                      |
| 服务路由（API 网关）                     | Zuul 等                                                        |
| 服务监控                                 | Zabbix、Nagios、Metrics、Specatator 等                         |
| 全链路追踪                               | Zipkin、Brave、Dapper 等                                       |
| 服务部署                                 | Docker、OpenStack、Kubernetes 等                               |
| 数据流操作开发包                         | SpringCloud Stream(封装与 Redis，Rabbit，Kafka 等发送接收消息) |
| 事件消息总线                             | SpringCloud Bus                                                |

## Spring Cloud Alibaba

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834720499-1855e417-e526-4824-87df-f4ea0d2fa8fc.jpeg#)

为什么选择 SpringCloud 作为微服务架构
1、选型依据
整体解决方案和框架成熟度社区热度
可维护性学习曲线
2、当前各大 IT 公司用的微服务架构有哪些？ 阿里：dubbo+HFS
京东：JSF
新 浪 ：Motan 当当网 DubboX
.......
3、各微服务框架对比

| **功能点 / 服务框架** |
**Netﬂix/SpringCloud** |
**Motan** |
**gRPC** |
**Thrift** |
**Dubbo/DubboX** |
| --- | --- | --- | --- | --- | --- |
|
功能定位 |
完整的微服务框架 | RPC 框架，但整合了 ZK 或
Consul，实现集群环境的基本服务注册/发现 |
RPC 框
架 |
RPC 框架 |
服务框架 |
| 支持
Rest | 是，Ribbon 支持多种可插拔的序列化选择 | 否 | 否 | 否 | 否 |
| 支持
RPC | 否 | 是（Hession2） | 是 | 是 | 是 |
| 支持多语言 |
是（Rest 形式）？ |
否 |
是 |
是 |
否 |
|
负载均衡 | 是（服务端 zuul+客户端 Ribbon），zuul-服务，动态路由，云端负载均衡
Eureka（针对中间层服务器） |
是（客户端） |
否 |
否 |
是（客户端） |
| 配置服务 | Netﬁx Archaius，Spring Cloud Conﬁg Server 集中配置 | 是（zookeeper 提供） |
否 |
否 |
否 |
| 服务调用链监控 |
是（zuul），zuul 提供边缘服务，API 网关 |
否 |
否 |
否 |
否 |
| 高可用 / 容错 | 是（服务端 Hystrix+客户端 Ribbon） |
是（客户端） |
否 |
否 |
是（客户端） |
| 典型应用案例 |
Netﬂix |
Sina |
Google |
Facebook | |
| 社区活跃程度 |
高 |
一般 |
高 |
一般 | 2017 年后重新开始维护，之前中断了 5 年 |
| 学习难度 | 中断 | 低 | 高 | 高 | 低 |
| 文档丰富程度 |
高 |
一般 |
一般 |
一般 |
高 |

| **功能点 / 服务框架** |
**Netﬂix/SpringCloud** |
**Motan** |
**gRPC** |
**Thrift** |
**Dubbo/DubboX** |
| --- | --- | --- | --- | --- | --- |
|
其他 |
Spring Cloud Bus 为我们的应用程序带来了更多管理端点 |
支持降级 | Netﬂix 内部在开发集成 gRPC |
IDL 定义 |
实践的公司比较多 |

# SpringCloud 入门

SpringCloud 是什么
Spring 官网:[https://spring.io/](https://spring.io/)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834720868-fe37e4e7-1613-4d7f-84f4-7f7e438ba536.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834721415-30b1d6d1-c9e9-46ac-852b-ef4bb5093e94.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834721964-f5a1d4e6-dfd0-41fb-acf5-a43121adbf33.jpeg#)

SpringCloud, 基于 SpringBoot 提供了一套微服务解决方案，包括服务注册与发现，配置中心，全链路监控，服务网关，负载均衡，熔断器等组件，除了基于 NetFlix 的开源组件做高度抽象封装之外，还有一些选型中立的开源组件。
SpringCloud 利用 SpringBoot 的开发便利性，巧妙地简化了分布式系统基础设施的开发，SpringCloud 为 开发人员提供了快速构建分布式系统的一些工具，**包括配置管理，服务发现，断路器，路由，微代理， 事件总线，全局锁，决策竞选，分布式会话等等**，他们都可以用 SpringBoot 的开发风格做到一键启动和部署。
SpringBoot 并没有重复造轮子，它只是将目前各家公司开发的比较成熟，经得起实际考研的服务框架组合起来，通过 SpringBoot 风格进行再封装，屏蔽掉了复杂的配置和实现原理，**最终给开发者留出了一套简单易懂，易部署和易维护的分布式系统开发工具包**
SpringCloud 是 分布式微服务架构下的一站式解决方案，是各个微服务架构落地技术的集合体，俗称微服务全家桶。

SpringCloud 和 SpringBoot 关系
SpringBoot 专注于快速方便的开发单个个体微服务。
SpringCloud 是关注全局的微服务协调整理治理框架，它将 SpringBoot 开发的一个个单体微服务整合并 管理起来，为各个微服务之间提供：配置管理，服务发现，断路器，路由，微代理，事件总线，全局 锁，决策竞选，分布式会话等等集成服务。
SpringBoot 可以离开 SpringClooud 独立使用，开发项目，但是 SpringCloud 离不开 SpringBoot，属于依赖关系

## SpringBoot 专注于快速、方便的开发单个个体微服务，SpringCloud 关注全局的服务治理框架

Dubbo 和 SpringCloud 对比

|              | **Dubbo**     | **Spring**                  |
| ------------ | ------------- | --------------------------- |
| 服务注册中心 | Zookeeper     | Spring Cloud Netﬁlx Eureka  |
| 服务调用方式 | RPC           | REST API                    |
| 服务监控     | Dubbo-monitor | Spring Boot Admin           |
| 断路器       | 不完善        | Spring Cloud Netﬂix Hystrix |
| 服务网关     | 无            | Spring Cloud Netﬂix Zuul    |
| 分布式配置   | 无            | Spring Cloud Conﬁg          |
| 服务跟踪     | 无            | Spring Cloud Sleuth         |
| 消息总线     | 无            | Spring Cloud Bus            |
| 数据流       | 无            | Spring Cloud Stream         |
| 批量任务     | 无            | Spring Cloud Task           |

**最大区别：SpringCloud 抛弃了 Dubbo 的 RPC 通信，采用的是基于 HTTP 的 REST 方式。**
严格来说，这两种方式各有优劣。虽然从一定程度上来说，后者牺牲了服务调用的性能，但也避免了上 面提到的原生 RPC 带来的问题。而且 REST 相比 RPC 更为灵活，服务提供方和调用方的依赖只依靠一纸契 约，不存在代码级别的强依赖，这在强调快速演化的微服务环境下，显得更加合适。

## 品牌机与组装机的区别

很明显，Spring Cloud 的功能比 DUBBO 更加强大，涵盖面更广，而且作为 Spring 的拳头项目，它也能够与 Spring Framework、Spring Boot、Spring Data、Spring Batch 等其他 Spring 项目完美融合，这些对于微服务而言是至关重要的。使用 Dubbo 构建的微服务架构就像组装电脑，各环节我们的选择自由度很 高，但是最终结果很有可能因为一条内存质量不行就点不亮了，总是让人不怎么放心，但是如果你是一 名高手，那这些都不是问题；而 Spring Cloud 就像品牌机，在 Spring Source 的整合下，做了大量的兼容性测试，保证了机器拥有更高的稳定性，但是如果要在使用非原装组件外的东西，就需要对其基础有足 够的了解。

## 社区支持与更新力度

最为重要的是，DUBBO 停止了 5 年左右的更新，虽然 2017.7 重启了。对于技术发展的新需求，需要由开发者自行拓展升级（比如当当网弄出了 DubboX），这对于很多想要采用微服务架构的中小软件组织， 显然是不太合适的，中小公司没有这么强大的技术能力去修改 Dubbo 源码+周边的一整套解决方案，并 不是每一个公司都有阿里的大牛+真实的线上生产环境测试过。

## 总结：

曾风靡国内的开源 RPC 服务框架 Dubbo 在重启维护后，令许多用户为之雀跃，但同时，也迎来了一些质疑的声音。互联网技术发展迅速，Dubbo 是否还能跟上时代？Dubbo 与 Spring Cloud 相比又有何优势和差异？是否会有相关举措保证 Dubbo 的后续更新频率？
人物：Dubbo 重启维护开发的刘军，主要负责人之一
刘军，阿里巴巴中间件高级研发工程师，主导了 Dubbo 重启维护以后的几个发版计划，专注于高性能
RPC 框架和微服务相关领域。曾负责网易考拉 RPC 框架的研发及指导在内部使用，参与了服务治理平台、分布式跟踪系统、分布式一致性框架等从无到有的设计与开发过程。

## 解决的问题域不一样：Dubbo 的定位是一款 RPC 框架，Spring Cloud 的目标是微服务架构下的一站式解决方案

Dubbo 和 SpringCloud 对比
Distributed/versioned conﬁguration （分布式/版本控制配置） Service registration and discovery（服务注册与发现） Routing（路由）
Service-to-service calls（服务到服务的调用） Load balancing（负载均衡配置）
Circuit Breakers（断路器）
Distributed messaging（分布式消息管理）
....

SpringCloud 在哪下
官网：[http://projects.spring.io/spring-cloud/ ](http://projects.spring.io/spring-cloud/)这玩意的版本号有点特别
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834722519-fd52c9ca-8735-4a96-839b-ef4aa145ba86.jpeg#)
Spring Cloud 是一个由众多独立子项目组成的大型综合项目，每个子项目有不同的发行节奏，都维护着自己的发布版本号。Spring Cloud 通过一个资源清单 BOM（Bill of Materials）来管理每个版本的子项目清单。为避免与子项目的发布号混淆，所以没有采用版本号的方式，而是通过命名的方式。
这些版本名称的命名方式采用了伦敦地铁站的名称，同时根据字母表的顺序来对应版本时间顺序，比 如：最早的 Release 版本：Angel，第二个 Release 版本：Brixton，然后是 Camden、Dalston、
Edgware，Finchley,目前最新的是 Hoxton 版本。

参考：
[https://springcloud.cc/spring-cloud-netﬂix.html](https://springcloud.cc/spring-cloud-netflix.html)
中文 API 文档：[https://springcloud.cc/spring-cloud-dalston.html](https://springcloud.cc/spring-cloud-dalston.html)
SpringCloud 中国社区 [http://springcloud.cn/](http://springcloud.cn/)
SpringCloud 中文网 [https://springcloud.cc](https://springcloud.cc/)

上面和大家聊了这么多，希望大家能够认真吸收，这就是大家能够在面试中和别人的谈资。而且好像我 也没说什么废话，之后的代码都会和上面的理论挂钩 ！所以需要认真掌握哈！

# Rest 微服务构建

总体介绍
我们会使用一个 Dept 部门模块做一个微服务通用案例
Consumer 消费者（Client）通过 REST 调用 Provider 提供者（Server）提供的服务。 回忆 Spring，SpringMVC，MyBatis 等以往学习的知识。。。
Maven 的分包分模块架构复习

1 一个简单的 Maven 模块结构是这样的：
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
-- app-parent：一个父项目（app-parent）聚合很多子项目（app-util，app-dao，app-
web...）
|-- pom.xml
|
|-- app-core
|| pom.xml
|
|-- app-web
|| pom.xml
......

一个父工程带着多个子 Module 子模块
SpringCloud 父工程（Project）下初次带着 3 个子模块（Module） springcloud-api 【封装的整体 entity / 接口 / 公共配置等】
springcloud-provider-dept-8001【服务提供者】
springcloud-consumer-dept-80【服务消费者】

SpringCloud 版本选择

## 大版本说明

| **Spring Boot** |
**Spring Cloud** | **关系** |
| --- | --- | --- |
| 1.2.x | Angel 版本 (天使) | 兼容 Spring Boot 1.2.x |
| 1.3.x | Brixton 版本 (布里克斯顿) | 兼容 Spring Boot 1.3.x，也兼容 Spring Boot 1.4.x |
| 1.4.x | Camden 版本 (卡姆登) | 兼容 Spring Boot 1.4.x，也兼容 Spring Boot 1.5.x |
| 1.5.x | Dalston 版本 (多尔斯顿) | 兼容 Spring Boot 1.5.x，不兼容 Spring Boot 2.0.x |
| 1.5.x | Edgware 版本 (埃奇韦尔) | 兼容 Spring Boot 1.5.x，不兼容 Spring Boot 2.0.x |
| 2.0.x | Finchley 版本 (芬奇利) | 兼容 Spring Boot 2.0.x，不兼容 Spring Boot 1.5.x |
|
2.1.x | Greenwich 版本 (格林威治) | 兼容 Spring Boot 2.1.x |
| 2.2.x | Hoxton 版本 (霍斯顿) | 兼容 Spring Boot 2.2.x |

**实际开发版本关系**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834722976-e495253d-1a0b-477e-89ad-94c3f9f0ed32.jpeg#)

创建父工程
新建父工程 Maven 项目 springcloud-parent，切记 Packageing 是 pom 模式
主要是定义 POM 文件，将后续各个子模块公用的 jar 包等统一提取出来，类似一个抽象父类
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834723448-0204cd6a-b012-47dc-9030-bbe4590d838d.png#)

## pom.xml

1.  <?xml version="1.0" encoding="UTF-8"?>
1.  <project xmlns=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)"
1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"

1.      xsi:schemaLocation=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)[ http://maven.apache.org/xsd/maven-4.0.0.xsd](http://maven.apache.org/xsd/maven-4.0.0.xsd)">
1.  <modelVersion>4.0.0</modelVersion> 6
1.  <groupId>com.kuang</groupId>
1.  <artifactId>springcloud-parent</artifactId>
1.  <version>1.0-SNAPSHOT</version> 10

11
12 <packaging>pom</packaging> 13

1. <properties>
1. <junit.version>4.12</junit.version>
1. <log4j.version>1.2.17</log4j.version>
1. <lombok.version>1.16.18</lombok.version>
1. </properties> 19
1. <dependencyManagement>
1. <dependencies>
1. <!-- spring-cloud-dependencies -->
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-dependencies</artifactId>
1. <version>Hoxton.SR4</version>
1. <type>pom</type>
1. <scope>import</scope>
1. </dependency>
1. <!-- spring-boot-dependencies -->
1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-dependencies</artifactId>
1. <version>2.2.1.RELEASE</version>
1. <type>pom</type>
1. <scope>import</scope>
1. </dependency>
1. <!-- mysql-connector-java -->
1. <dependency>
1. <groupId>mysql</groupId>
1. <artifactId>mysql-connector-java</artifactId>
1. <version>5.1.47</version>
1. </dependency>

44 <!-- druid -->

1. <dependency>
1. <groupId>com.alibaba</groupId>
1. <artifactId>druid</artifactId>
1. <version>1.1.6</version>
1. </dependency>
1. <!-- mybatis-spring-boot-starter -->
1. <dependency>
1. <groupId>org.mybatis.spring.boot</groupId>
1. <artifactId>mybatis-spring-boot-starter</artifactId>
1. <version>1.3.0</version>
1. </dependency>
1. <dependency>
1. <groupId>ch.qos.logback</groupId>
1. <artifactId>logback-core</artifactId>
1. <version>1.2.3</version>
1. </dependency>

61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>${junit.version}</version>
</dependency>
<dependency>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
<version>${log4j.version}</version>
</dependency>
<dependency>
<groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
<version>${lombok.version}</version>
</dependency>
</dependencies>
</dependencyManagement>
</project>

创建 api 公共模块
新建 springcloud-api 模块
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834723932-cd9d6a63-18fe-44ec-9891-b9421d5ddb4f.png#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834724460-a4b87f67-4b1f-498b-85e2-38dc98903806.jpeg#)可以观察发现，在父工程中多了一个 Modules
编写 springcloud-api 的 pom.xml

1.  <?xml version="1.0" encoding="UTF-8"?>
1.  <project xmlns=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)"
1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1.      xsi:schemaLocation=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)[ http://maven.apache.org/xsd/maven-4.0.0.xsd](http://maven.apache.org/xsd/maven-4.0.0.xsd)">
1.  <parent>
1.  <artifactId>springcloud-parent</artifactId>
1.  <groupId>com.kuang</groupId>
1.  <version>1.0-SNAPSHOT</version>
1.  </parent>
1.  <modelVersion>4.0.0</modelVersion>
1.  <!--当前Module的名字-->

12
13
14
<artifactId>springcloud-api</artifactId>

<!--当前Module需要到的jar包，按自己需求添加，如果父项目已经包含了，可以不用写版本号-
->
15
16
17
18
19
20
21
22
<dependencies>
<dependency>
<groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
</dependency>
</dependencies>
</project>

创建部门数据库脚本，数据库名：springcloud01



| 1 | CREATE TABLE dept( |
| --- | --- |
| 2 | deptno BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT, |
| 3 | dname VARCHAR(60), |
| 4 | db_source VARCHAR(60) |
| 5 | ); |
| 6 |  |
| 7 | INSERT INTO dept(dname,db_source) VALUES('开发部',DATABASE()); |
| 8 | INSERT INTO dept(dname,db_source) VALUES('人事部',DATABASE()); |
| 9 | INSERT INTO dept(dname,db_source) VALUES('财务部',DATABASE()); |
| 10 | INSERT INTO dept(dname,db_source) VALUES('市场部',DATABASE()); |
| 11 | INSERT INTO dept(dname,db_source) VALUES('运维部',DATABASE()); |
| 12 |  |
| 13 | SELECT * FROM dept; |

编写实体类，注意：实体类都序列化！

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
package com.kuang.springcloud.pojo;
import lombok.Data;
import lombok.NoArgsConstructor; import lombok.experimental.Accessors;

import java.io.Serializable;

@NoArgsConstructor @Data
@Accessors(chain = true) //链式写法
public class Dept implements Serializable { //Dept(实体类)	orm
(表)	类表关系映射
mysql->Dept
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
25
private Long deptno; //主键
private String dname; //部门名称
//来自哪个数据库，因为微服务架构可以一个服务对应一个数据库，同一个信息被存到多个不同的  数据库
private String db_source;
public Dept(String dname) { this.dname = dname;
}

/*
链式写法：
Dept dept = new Dept()


26
27
28
dept.setDeptno(11L).setDname("school").setDb_source("DB01");
**/
}


创建provider模块
新建springcloud-provider-dept-8001模块编辑pom.xml


   1. <?xml version="1.0" encoding="UTF-8"?>
   1. <project xmlns=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)"
   1. xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
   1. 	xsi:schemaLocation=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)[ http://maven.apache.org/xsd/maven-4.0.0.xsd](http://maven.apache.org/xsd/maven-4.0.0.xsd)">
   1. <parent>
   1. <artifactId>springcloud-parent</artifactId>
   1. <groupId>com.kuang</groupId>
   1. <version>1.0-SNAPSHOT</version>
   1. </parent>
   1. <modelVersion>4.0.0</modelVersion> 11

12	<artifactId>springcloud-provider-dept-8001</artifactId> 13

1. <dependencies>
1. <!--引入自定义的模块，我们就可以使用这个模块中的类了-->

1. <dependency>
1. <groupId>com.kuang</groupId>
1. <artifactId>springcloud-api</artifactId>
1. <version>1.0-SNAPSHOT</version>
1. </dependency>
1. <dependency>
1. <groupId>junit</groupId>
1. <artifactId>junit</artifactId>
1. </dependency>
1. <dependency>
1. <groupId>mysql</groupId>
1. <artifactId>mysql-connector-java</artifactId>
1. </dependency>
1. <dependency>
1. <groupId>com.alibaba</groupId>
1. <artifactId>druid</artifactId>
1. </dependency>
1. <dependency>
1. <groupId>ch.qos.logback</groupId>
1. <artifactId>logback-core</artifactId>
1. </dependency>
1. <dependency>
1. <groupId>org.mybatis.spring.boot</groupId>
1. <artifactId>mybatis-spring-boot-starter</artifactId>
1. </dependency>
1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-starter-jetty</artifactId>
1. </dependency>

45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-test</artifactId>
</dependency>

<!-- spring-boot-devtools -->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-devtools</artifactId>
</dependency>
</dependencies>

</project>

编辑 application.yml

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
17
18
19
20
21
22
23
24
25
server:
port: 8001
#mybatis 的配置
mybatis:
config-location: classpath:mybatis/mybatis-config.xml type-aliases-package: com.kuang.springcloud.pojo mapper-locations:

- classpath:mybatis/mapper/\*_/_.xml
  #spring 的相关配置
  spring: application:
  name: springcloud-provider-dept datasource:
  type: com.alibaba.druid.pool.DruidDataSource # 数据源
  driver-class-name: org.gjt.mm.mysql.Driver

# mysql 驱动

url: jdbc:mysql://localhost:3306/springcloud01 #数据库名称
username: root password: 123456 dbcp2:
min-idle: 5
initial-size: 5
max-total: 5
max-wait-millis: 200 #数据库连接池的最小维持连接数 #初始化连接数 #最大连接数 #等待连接获取的最大超时时间

根据配置新建 mybatis-conﬁg.xml 文件

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

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN" ["http://mybatis.org/dtd/mybatis-3-config.dtd](http://mybatis.org/dtd/mybatis-3-config.dtd)">
<configuration>
<settings>
<!--开启二级缓存-->
<setting name="cacheEnabled" value="true"/>
</settings>

</configuration>

编写部门的 dao 接口

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
17
package com.kuang.springcloud.dao;
import com.kuang.springcloud.pojo.Dept; import org.apache.ibatis.annotations.Mapper;

import java.util.List;
@Mapper
public interface DeptDao {
public boolean addDept(Dept dept); //添加一个部门
public Dept queryById(Long id); //根据 id 查询部门 public List<Dept> queryAll(); //查询所有部门
}

接口对应的 Mapper.xml 文件 mybatis\mapper\DeptMapper.xml

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
17
18
19

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)">
<mapper  namespace="com.kuang.springcloud.dao.DeptDao">
<insert id="addDept" parameterType="Dept">
insert into dept (dname,db_source) values (#{dname},DATABASE());
</insert>

<select id="queryById" resultType="Dept" parameterType="Long">
select deptno,dname,db_source from dept where deptno = #{deptno};
</select>

<select id="queryAll" resultType="Dept"> select deptno,dname,db_source from dept;
</select>

</mapper>

创建 Service 服务层接口

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
package com.kuang.springcloud.service;
import com.kuang.springcloud.pojo.Dept;

import java.util.List;
public interface DeptService {
public boolean addDept(Dept dept); //添加一个部门
public Dept queryById(Long id); //根据 id 查询部门 public List<Dept> queryAll(); //查询所有部门
}

ServiceImpl 实现类

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
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
package com.kuang.springcloud.service.impl;
import com.kuang.springcloud.dao.DeptDao; import com.kuang.springcloud.pojo.Dept;
import com.kuang.springcloud.service.DeptService;
import org.springframework.beans.factory.annotation.Autowired; import org.springframework.stereotype.Service;

import java.util.List;
@Service
public class DeptServiceImpl implements DeptService {
//自动注入
@Autowired
private DeptDao deptDao;

@Override
public boolean addDept(Dept dept) { return deptDao.addDept(dept);
}

@Override
public Dept queryById(Long id) { return deptDao.queryById(id);
}

@Override
public List<Dept> queryAll() { return deptDao.queryAll();
}
}

DeptController 提供 REST 服务

| 1   | package com.kuang.springcloud.controller;         |
| --- | ------------------------------------------------- |
| 2   |                                                   |
| 3   | import com.kuang.springcloud.pojo.Dept;           |
| 4   | import com.kuang.springcloud.service.DeptService; |

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
25
26
27
28
29
30
31
32
33
34
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.\*;
import java.util.List;
@RestController @RequestMapping("/dept")
public class DeptController {
@Autowired
private DeptService service;

// @RequestBody
// 如果参数是放在请求体中，传入后台的话，那么后台要用@RequestBody 才能接收到
@PostMapping("/add")
public boolean addDept(@RequestBody Dept dept) { return service.addDept(dept);
}

@GetMapping("/get/{id}")
public Dept get(@PathVariable("id") Long id) { return service.queryById(id);
}

@GetMapping("/list")
public List<Dept> queryAll() { return service.queryAll();
}
}

编写 DeptProvider 的主启动类

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
package com.kuang.springcloud;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DeptProvider8001 {
public static void main(String[] args) { SpringApplication.run(DeptProvider8001.class,args);
}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834724773-05e36b4a-3e94-4a6f-bc0c-f17dc21ccf00.png#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834725038-d807c509-5963-4df9-b3f6-c9d65fd9dc7f.png#)启动测试，注意编写细节：

创建 consumer 模块
新建 springcloud-consumer-dept-80 模块编辑 pom.xml

1.  <?xml version="1.0" encoding="UTF-8"?>
1.  <project xmlns=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)"
1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1.      xsi:schemaLocation=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)[ http://maven.apache.org/xsd/maven-4.0.0.xsd](http://maven.apache.org/xsd/maven-4.0.0.xsd)">
1.  <parent>
1.  <artifactId>springcloud-parent</artifactId>
1.  <groupId>com.kuang</groupId>
1.  <version>1.0-SNAPSHOT</version>
1.  </parent>
1.  <modelVersion>4.0.0</modelVersion> 11
1.  <artifactId>springcloud-consumer-dept-80</artifactId>
1.  <description>部门微服务消费者</description> 14
1.  <dependencies>
1.  <dependency>
1.  <groupId>com.kuang</groupId>
1.  <artifactId>springcloud-api</artifactId>
1.  <version>1.0-SNAPSHOT</version>
1.  </dependency>
1.  <dependency>
1.  <groupId>org.springframework.boot</groupId>
1.  <artifactId>spring-boot-starter-web</artifactId>
1.  </dependency>

25 <!--热部署-->

1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-devtools</artifactId>
1. </dependency>
1. </dependencies> 31

32 </project>

application.yml 配置文件

1. server:
1. port: 80

新建一个 ConﬁgBean 包注入 RestTemplate！

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
package com.kuang.springcloud.config;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration; import org.springframework.web.client.RestTemplate;

@Configuration
public class ConfigBean {

@Bean

11
12
13
14
15
public RestTemplate getRestTemplate(){ return new RestTemplate();
}
}

创建 Controller 包，编写 DeptConsumerController 类

1 package com.kuang.springcloud.controller; 2

1. import com.kuang.springcloud.pojo.Dept;
1. import org.springframework.beans.factory.annotation.Autowired;
1. import org.springframework.web.bind.annotation.PathVariable;
1. import org.springframework.web.bind.annotation.RequestMapping;
1. import org.springframework.web.bind.annotation.RestController;
1. import org.springframework.web.client.RestTemplate; 9

10 import java.util.List; 11

1. @RestController
1. public class DeptConsumerController {
1. //理解：消费者，不应该有 service 层 15
1. // 使用 RestTemplate 访问 restful 接口非常的简单粗暴且无脑
1. // （url，requestMap，ResponseBean.class） 这三个参数分别代表
1. // REST 请求地址，请求参数，Http 响应转换 被 转换成的对象类型
1. @Autowired
1. private RestTemplate restTemplate; 21

22 private static final String REST_URL_PREFIX = "http://localhost:8001"; 23

1. @RequestMapping("/consumer/dept/add")
1. public boolean add(Dept dept){
1. return

restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept,Boolean.class); 27 }
28

1. @RequestMapping("/consumer/dept/get/{id}")
1. public Dept get(@PathVariable("id") Long id){
1. return

restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id,Dept.class); 32 }
33

1. @RequestMapping("/consumer/dept/list")
1. public List<Dept> list(){
1. return

restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class); 37 }
38
39 }

了解 RestTemplate：

| 1   | RestTemplate 提供了多种便捷访问远程 Http 服务的方法，是一种简单便捷的访问 restful 服务模板 |
| --- | ------------------------------------------------------------------------------------------ |
|     | 类，是 Spring 提供的用于访问 Rest 服务的客户端模板工具集                                   |
| 2   |                                                                                            |
| 3   | 使用 RestTemplate 访问 restful 接口非常的简单粗暴且无脑                                    |
| 4   | （url，requsetMap，ResponseBean.class） 这三个参数分别代表 REST 请求地址，请求参数，       |
|     | Http 响应转换 被 转换成的对象类型                                                          |

主启动类

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
package com.kuang.springcloud;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication public class DeptConsumer80 {
public static void main(String[] args) { SpringApplication.run(DeptConsumer80.class,args);
}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834725519-86988e00-8998-4127-8839-c11d9a3ccdb6.png#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834725787-5d38201f-3534-4cbc-b2c5-c383ebffba73.png#)访问测试：

# Eureka 服务注册与发现

什么是 Eureka
Eureka：怎么读？
Netﬂix 在设计 Eureka 时，遵循的就是 AP 原则

| 1   | CAP 原则又称 CAP 定理，指的是在一个分布式系统中                  |
| --- | ---------------------------------------------------------------- |
| 2   | 一致性（Consistency）                                            |
| 3   | 可用性（Availability）                                           |
| 4   | 分区容错性（Partition tolerance）                                |
| 5   | CAP 原则指的是，这三个要素最多只能同时实现两点，不可能三者兼顾。 |

Eureka 是 Netﬂix 的一个子模块，也是核心模块之一。Eureka 是一个基于 REST 的服务，用于定位服务， 以实现云端中间层服务发现和故障转移，服务注册与发现对于微服务来说是非常重要的，有了服务发现 与注册，只需要使用服务的标识符，就可以访问到服务，而不需要修改服务调用的配置文件了，功能类 似于 Dubbo 的注册中心，比如 Zookeeper；

原理讲解

## Eureka 的基本架构：

SpringCloud 封装了 NetFlix 公司开发的 Eureka 模块来实现服务注册和发现
Eureka 采用了 C-S 的架构设计，EurekaServer 作为服务注册功能的服务器，他是服务注册中心
而系统中的其他微服务。使用 Eureka 的客户端连接到 EurekaServer 并维持心跳连接。这样系统的维护人 员就可以通过 EurekaServer 来监控系统中各个微服务是否正常运行，SpringCloud 的一些其他模块（比 如 Zuul）就可以通过 EurekaServer 来发现系统中的其他微服务，并执行相关的逻辑；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834726162-d2059430-408e-4614-b6bc-a3d6d83e05cc.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834726646-c2de4f3f-d005-4e9d-98fd-66e66b440f66.jpeg#)和 Dubbo 架构对比：

Eureka 包含两个组件：**Eureka Server **和 **Eureka Client **。
Eureka Server 提供服务注册服务，各个节点启动后，会在 EurekaServer 中进行注册，这样 Eureka
Server 中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。
Eureka Client 是一个 Java 客户端，用于简化 EurekaServer 的交互，客户端同时也具备一个内置的，使用轮询负载算法的负载均衡器。在应用启动后，将会向 EurekaServer 发送心跳（默认周期为 30 秒）。如果 Eureka Server 在多个心跳周期内没有接收到某个节点的心跳，EurekaServer 将会从服务注册表中把这个服务节点移除掉（默认周期为 90 秒）

## 三大角色

Eureka Server：提供服务的注册于发现。
Service Provider：将自身服务注册到 Eureka 中，从而使消费方能够找到。
Service Consumer：服务消费方从 Eureka 中获取注册服务列表，从而找到消费服务。

服务构建
建立 springcloud-eureka-7001 模块编辑 pom.xml

1.  <?xml version="1.0" encoding="UTF-8"?>
1.  <project xmlns=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)"
1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1.      xsi:schemaLocation=["http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)[ http://maven.apache.org/xsd/maven-4.0.0.xsd](http://maven.apache.org/xsd/maven-4.0.0.xsd)">
1.  <parent>
1.  <artifactId>springcloud-parent</artifactId>
1.  <groupId>com.kuang</groupId>
1.  <version>1.0-SNAPSHOT</version>
1.  </parent>
1.  <modelVersion>4.0.0</modelVersion> 11

12 <artifactId>springcloud-eureka-7001</artifactId> 13

1. <dependencies>
1. <!-- eureka-server服务端 -->
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-eureka-server</artifactId>
1. <version>1.4.7.RELEASE</version>
1. </dependency>

21 <!--热部署-->

1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-devtools</artifactId>
1. </dependency>
1. </dependencies> 27

28 </project>

application.yml

1 server:
2 port: 7001
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
#Eureka 配置
eureka: instance:
hostname: localhost #eureka 服务端的实例名称
client:
register-with-eureka: false # 是否将自己注册到 Eureka 服务器中，本身是服务器，无需注册
fetch-registry: false # false 表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
service-url:
defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/

# 设置与 Eureka Server 交互的地址查询服务和注册服务都需要依赖这个 defaultZone 地址

编写主启动类

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
package com.kuang.springcloud;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication; import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;
@SpringBootApplication
@EnableEurekaServer //EurekaServer 服务器端启动类，接受其他微服务注册进来！ public class EurekaServer7001 {
public static void main(String[] args) { SpringApplication.run(EurekaServer7001.class,args);
}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834727094-12f16f01-021b-4188-b7ec-e9af2881b966.png#)启动，访问测试：
System Status：系统信息

DS Replicas：服务器副本
Instances currently registered with Eureka：已注册的微服务列表 General Info：一般信息
Instance Info：实例信息

Service Provider
将 8001 的服务入驻到 7001 的 eureka 中！
1、修改 8001 服务的 pom 文件，增加 eureka 的支持！

1. <!--将服务的provider注册到eureka中-->
1. <!-- [https://mvnrepository.com/artifact/org.springframework.cloud/spring-](https://mvnrepository.com/artifact/org.springframework.cloud/spring-) cloud-starter-eureka -->
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-eureka</artifactId>
1. <version>1.4.7.RELEASE</version>
1. </dependency>
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-config</artifactId>
1. </dependency>

2、yaml 中配置 eureka 的支持

1. #eureka 配置
1. eureka:
1. client:
1. service-url:
1. defaultZone: http://localhost:7001/eureka

3、8001 的主启动类注解支持

1
2
3
4
5
6
7
8
9
@SpringBootApplication
@EnableEurekaClient // 本服务启动之后会自动注册进 Eureka 中！
public class DeptProvider8001 {
public static void main(String[] args) { SpringApplication.run(DeptProvider8001.class,args);
}
}

截止目前：服务端也有了，客户端也有了，启动 7001，再启动 8001，测试访问

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834727672-f67bb02c-3738-49a7-82b5-bab2f8aef80f.png#)

actuator 与注册微服务信息完善

## 主机名称：服务名称修改

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834728160-c3d38f06-d75d-456e-a3bf-544db1d550c8.jpeg#)
在 8001 的 yaml 中修改一下配置

1. # eureka 配置
1. eureka:
1. client:
1. service-url:
1. defaultZone: http://localhost:7001/eureka
1. instance:
1. instance-id: springcloud-provider-dept8001 # 重点，和 client 平级

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834728709-c399a800-a0f8-4d20-968a-b0a5d1a2aaad.png#)重启，刷新后查看结果！

## 访问信息有 IP 信息提示

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834729107-7987a74d-aa4f-4074-9a29-1e42ab563555.png#)
yaml 中在增加一个配置

1. #eureka 配置
1. eureka:
1. client:
1. service-url:
1. defaultZone: http://localhost:7001/eureka
1. instance:
1. instance-id: springcloud-provider-dept8001
1. prefer-ip-address: true # true 访问路径可以显示 IP 地址

## info 内容构建

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834729444-46b4917a-f52c-46a9-971d-c0fdeb4bd0f1.jpeg#)现在点击 info，出现 ERROR 页面

修改 8001 的 pom 文件，新增依赖！

1. <!--actuator监控信息完善-->
1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-starter-actuator</artifactId>
1. </dependency>

在**总的父工程**中修改 pom.xml 添加 build 信息 【没必要做】

1. <build>
1. <finalName>springcloud</finalName>
1. <resources>
1. <resource>
1. <directory>src/main/resources</directory>
1. <filtering>true</filtering>
1. </resource>
1. </resources>
1. <plugins>

1. <plugin>
1. <groupId>org.apache.maven.plugins</groupId>
1. <artifactId>maven-resources-plugin</artifactId>
1. <configuration>
1. <delimiters>
1. <!--以$开头的或者结尾的在src/main/resources路径下的就能读取-->
1. <delimit>$</delimit>
1. </delimiters>
1. </configuration>
1. </plugin>
1. </plugins>
1. </build>

然后回到我们的 8001 的 yaml 配置文件中修改增加信息

1. #info 配置
1. info:
1. app.name: kuang-springcloud
1. company.name: [www.kuangstudy.com](http://www.kuangstudy.com/)
1. build.artifactId: ${project.artifactId}
1. build.version: ${project.version}

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834729815-17e2b34c-41c6-419f-a30a-d556cd6e844d.jpeg#)重启项目测试：7001、8001

Eureka 的自我保护机制
之前出现的这些红色情况，没出现的，修改一个服务名，故意制造错误！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834730199-db5ff774-9294-42c8-8dd2-fbae44d7c27c.jpeg#)

## 自我保护机制：好死不如赖活着

一句话总结：某时刻某一个微服务不可以用了，eureka 不会立刻清理，依旧会对该微服务的信息进行保存！
默认情况下，如果 EurekaServer 在一定时间内没有接收到某个微服务实例的心跳，EurekaServer 将会注 销该实例（默认 90 秒）。但是当网络分区故障发生时，微服务与 Eureka 之间无法正常通行，以上行为可能变得非常危险了--因为微服务本身其实是健康的，**此时本不应该注销这个服务**。Eureka 通过 **自我保护机制 **来解决这个问题--当 EurekaServer 节点在短时间内丢失过多客户端时（可能发生了网络分区故
障），那么这个节点就会进入自我保护模式。一旦进入该模式，EurekaServer 就会保护服务注册表中的信息，不再删除服务注册表中的数据（也就是不会注销任何微服务）。当网络故障恢复后，该
EurekaServer 节点会自动退出自我保护模式。
在自我保护模式中，EurekaServer 会保护服务注册表中的信息，不再注销任何服务实例。当它收到的心跳数重新恢复到阈值以上时，该 EurekaServer 节点就会自动退出自我保护模式。它的设计哲学就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。一句话：好死不如赖活着。
综上，自我保护模式是一种应对网络异常的安全保护措施。它的架构哲学是宁可同时保留所有微服务
（健康的微服务和不健康的微服务都会保留），也不盲目注销任何健康的微服务。使用自我保护模式， 可以让 Eureka 集群更加的健壮和稳定。

在 SpringCloud 中，可以使用
eureka.server.enable-self-preservation = false
模式 【不推荐关闭自我保护机制】
禁用自我保护

8001 服务发现 Discovery
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834730816-6f9450f8-e5e0-4f37-86d0-33301d8378fc.jpeg#)对于注册进 eureka 里面的微服务，可以通过服务发现来获得该服务的信息。【对外暴露服务】修改 springcloud-provider-dept-8001 工程中的 DeptController
新增一个方法

1
2
3
4
5
6
7
8
@GetMapping("/discovery") public Object discovery(){
//获得微服务列表清单
List<String> list = client.getServices(); System.out.println("client.getServices()==>"+list);
//得到一个具体的微服务！
List<ServiceInstance> serviceInstanceList = client.getInstances("springcloud-provider-dept");
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
for (ServiceInstance serviceInstance : serviceInstanceList) { System.out.println(
serviceInstance.getServiceId()+"\t"+ serviceInstance.getHost()+"\t"+ serviceInstance.getPort()+"\t"+ serviceInstance.getUri()
);
}
return this.client;
}

主启动类增加一个注解

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
@SpringBootApplication
@EnableEurekaClient // 本服务启动之后会自动注册进 Eureka 中！
@EnableDiscoveryClient //服务发现 public class DeptProvider8001 {
public static void main(String[] args) { SpringApplication.run(DeptProvider8001.class,args);
}
}

启动 Eureka 服务，启动 8001 提供者
访问测试 http://localhost:8001/dept/discovery
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834731309-88b1af6f-94a6-4068-8320-3d0d5c0f0dbb.jpeg#)后台输出：

consumer 访问服务
springcloud-consumer-dept-80

## 修改 DeptConsumerController 增加一个方法

1.  @GetMapping("/consumer/dept/discovery")
1.  public Object discovery(){
1.      return restTemplate.getForObject(REST_URL_PREFIX+"/dept/discovery",Object.class);

4 }

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834731875-721b31c0-1656-4168-b6a1-14e96c587787.jpeg#)启动 80 项目进行测试！

集群配置
新建工程 springcloud-eureka-7002、springcloud-eureka-7003 按照 7001 为模板粘贴 POM
修改 7002 和 7003 的主启动类
修改映射配置 , **windows 域名映射**
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834732247-062944b5-6450-451c-95a1-87c0da901357.png#)
集群配置分析
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834732521-640b7a38-bfc6-45ae-a9de-134ec68bc67a.jpeg#)

修改 3 个 EurekaServer 的 yaml 文件夹
7001：

1 server:
2 port: 7001
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
#Eureka 配置
eureka: instance:
hostname: eureka7001.com #eureka 服务端的实例名称
client:
register-with-eureka: false #false 表示不向注册中心注册自己
fetch-registry: false # false 表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
service-url: # 单机 defaultZone:
[http://${eureka.instance.hostname}:${server.port}/eureka/](http://$%7Beureka.instance.hostname%7D:$%7Bserver.port%7D/eureka/)

# 设置与 Eureka Server 交互的地址查询服务和注册服务都需要依赖这个 defaultZone 地址

14 defaultZone:
http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/

7002：

1 server:
2 port: 7002
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
#Eureka 配置
eureka: instance:
hostname: eureka7002.com #eureka 服务端的实例名称
client:
register-with-eureka: false #false 表示不向注册中心注册自己
fetch-registry: false # false 表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
service-url:

# 单 机 defaultZone: [http://${eureka.instance.hostname}:${server.port}/eureka/](http://$%7Beureka.instance.hostname%7D:$%7Bserver.port%7D/eureka/)

# 设置与 Eureka Server 交互的地址查询服务和注册服务都需要依赖这个 defaultZone 地址 defaultZone:

http://eureka7001.com:7001/eureka/,http://eureka7003.com:7003/eureka/

7003：

1 server:
2 port: 7003
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
#Eureka 配置
eureka: instance:
hostname: eureka7003.com #eureka 服务端的实例名称
client:
register-with-eureka: false #false 表示不向注册中心注册自己
fetch-registry: false # false 表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
service-url:

# 单 机 defaultZone: [http://${eureka.instance.hostname}:${server.port}/eureka/](http://$%7Beureka.instance.hostname%7D:$%7Bserver.port%7D/eureka/)

# 设置与 Eureka Server 交互的地址查询服务和注册服务都需要依赖这个 defaultZone 地址 defaultZone:

http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834732916-6aaec710-545a-47f9-bb64-1d442a2b40e0.png#)将 8001 微服务发布到 1 台 eureka 集群配置中，发现在集群中的其余注册中心也可以看到，但是平时我们保险起见，都发布！
启动集群测试！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834733307-3cc56e45-9ed0-4917-abe1-72f818e04f78.jpeg#)

对比 Zookeeper

## 回顾 CAP 原则

RDBMS （Mysql、Oracle、sqlServer）===>ACID
NoSQL（redis、mongdb）===> CAP

## ACID 是什么？

A（Atomicity）原子性
C（Consistency） 一致性 I （Isolation）隔离性 D（Durability）持久性

## CAP 是什么？

C（Consistency）强一致性 A（Availability）可用性 P（Partition tolerance）分区容错性
CAP 的三进二：CA、AP、CP
CAP 理论的核心
一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求
根据 CAP 原理，将 NoSQL 数据库分成了满足 CA 原则，满足 CP 原则和满足 AP 原则三大类：
CA：单点集群，满足一致性，可用性的系统，通常可扩展性较差 CP：满足一致性，分区容错性的系统，通常性能不是特别高 AP：满足可用性，分区容错性的系统，通常可能对一致性要求低一些

作为服务注册中心，Eureka 比 Zookeeper 好在哪里？
著名的 CAP 理论指出，一个分布式系统不可能同时满足 C（一致性）、A（可用性）、P（容错性）。由于分区容错性 P 在分布式系统中是必须要保证的，因此我们只能在 A 和 C 之间进行权衡。
Zookeeper 保证的是 CP；
Eureka 保证的是 AP；

## Zookeeper 保证的是 CP

当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接受服 务直接 down 掉不可用。也就是说，服务注册功能对可用性的要求要高于一致性。但是 zk 会出现这样一种情况，当 master 节点因为网络故障与其他节点失去联系时，剩余节点会重新进行 leader 选举。问题在
于，选举 leader 的时间太长，30~120s，且选举期间整个 zk 集群都是不可用的，这就导致在选举期间注册服务瘫痪。在云部署的环境下，因为网络问题使得 zk 集群失去 master 节点是较大概率会发生的事件， 虽然服务最终能够恢复，但是漫长的选举时间导致的注册长期不可用是不能容忍的。

## Eureka 保证的是 AP

Eureka 看明白了这一点，因此在设计时就优先保证可用性。**Eureka 各个节点都是平等的**，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而 Eureka 的客户端在向某个
Eureka 注册时，如果发现连接失败，则会自动切换至其他节点，只要有一台 Eureka 还在，就能保住注册服务的可用性，只不过查到的信息可能不是最新的，除此之外，Eureka 还有一种自我保护机制，如果在
15 分钟内超过 85%的节点都没有正常的心跳，那么 Eureka 就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况：

1. Eureka 不再从注册列表中移除因为长时间没收到心跳而应该过期的服务
1. Eureka 仍然能够接受新服务的注册和查询请求，但是不会被同步到其他节点上（即保证当前节点依然可用）
1. 当网络稳定时，当前实例新的注册信息会被同步到其他节点中

因此，Eureka 可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像 zookeeper 那样使整 个注册服务瘫痪

# Ribbon 负载均衡

鸡汤：
理论 和 实践 是同样重要的！
因为在你们去面试的时候，需要有谈资！！！
你需要先进去，才能够拥有干活的机会，所以要耐着性子学习！

概述

## Ribbon 是什么？

Spring Cloud Ribbon 是基于 Netﬂix Ribbon 实现的一套**客户端负载均衡的工具**。
简单的说，Ribbon 是 Netﬂix 发布的开源项目，主要功能是提供客户端的软件负载均衡算法，将 NetFlix 的 中间层服务连接在一起。Ribbon 的客户端组件提供一系列完整的配置项如：连接超时、重试等等。简单 的说，就是在配置文件中列出 LoadBalancer（简称 LB：负载均衡）后面所有的机器，Ribbon 会自动的 帮助你基于某种规则（如简单轮询，随机连接等等）去连接这些机器。我们也很容易使用 Ribbon 实现自 定义的负载均衡算法！

## Ribbon 能干嘛？

LB，即负载均衡（Load Balance），在微服务或分布式集群中经常用的一种应用。
负载均衡简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的 HA（高可用）。 常见的负载均衡软件有 Nginx，Lvs 等等
Dubbo、SpringCloud 中均给我们提供了负载均衡，**SpringCloud 的负载均衡算法可以自定义**
负载均衡简单分类： 集中式 LB
即在服务的消费方和提供方之间使用独立的 LB 设施
如之前学习的 Nginx，由该设施负责把访问请求通过某种策略转发至服务的提供方！ 进程式 LB
将 LB 逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选出一个合适的服务器。
Ribbon 就属于进程内 LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址！
Ribbon 的 github 地址 ： [https://github.com/NetFlix/ribbon](https://github.com/NetFlix/ribbon)

Ribbon 配置初步
复制 springcloud-consumer-dept-80 工程 springcloud-consumer-dept-ribbon-80
1、主启动类名字修改下，用作区分

1. @SpringBootApplication
1. public class DeptConsumerRibbon80 {
1. public static void main(String[] args) {
1. SpringApplication.run(DeptConsumerRibbon80.class,args); 5 }

6 }

2、修改 pom.xml，添加 Ribbon 相关的配置

1. <!--Ribbon相关-->
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-eureka</artifactId>
1. <version>1.4.7.RELEASE</version>
1. </dependency>
1. <!-- [https://mvnrepository.com/artifact/org.springframework.cloud/spring-](https://mvnrepository.com/artifact/org.springframework.cloud/spring-) cloud-starter-ribbon -->
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-ribbon</artifactId>
1. <version>1.4.7.RELEASE</version>
1. </dependency>
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-config</artifactId>
1. </dependency>

3、修改 application.yml ， 追加 Eureka 的服务注册地址

1
2
3
4
5
6
7
8
9
server:
port: 80
#Eureka 配置
eureka: client:
register-with-eureka: false #false 表示不向注册中心注册自己
service-url: defaultZone:
http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://
eureka7003.com:7003/eureka/

4、对里面的 ConﬁgBean 方法加上注解@LoadBalanced 在 获得 Rest 时加入 Ribbon 的配置；

1. @Bean
1. @LoadBalanced //Spring Cloud Ribbon 是基于 Netflix Ribbon 实现的一套客户端负载均衡的工具
1. public RestTemplate getRestTemplate(){
1. return new RestTemplate(); 5 }

5、主启动类 DeptConsumerRibbon80 添加@EnableEurekaClient

1. @EnableEurekaClient
1. @SpringBootApplication
1. public class DeptConsumerRibbon80 {
1. public static void main(String[] args) {
1. SpringApplication.run(DeptConsumerRibbon80.class,args); 6 }

7 }

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834733623-a7aaf0fd-2b44-4756-892a-dd3dc40c7d10.png#)6、修改 DeptConsumerController 客户端访问类，之前的写的地址是写死的，现在需要变化！

| 1   | //做 Ribbon 时候将此改为了微服务名字                                                                         |
| --- | ------------------------------------------------------------------------------------------------------------ |
| 2   | //private static final String REST_URL_PREFIX = "http://localhost:8001";                                     |
| 3   | private static final String REST_URL_PREFIX = ["http://SPRINGCLOUD-PROVIDER-](http://SPRINGCLOUD-PROVIDER-/) |
|     | DEPT";                                                                                                       |

7、先启动 3 个 Eureka 集群后，在启动 springcloud-provider-dept-8001 并注册进 eureka
8、启动 DeptConsumerRibbon80
9、测试
[http://localhost/consumer/dept/get/1](http://localhost/consumer/dept/get/1) [http://localhost/consumer/dept/list](http://localhost/consumer/dept/list)
10、小结

Ribbon 和 Eureka 整合后 Consumer 可以直接调用服务而不用再关心地址和端口号！

Ribbon 负载均衡
架构说明：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834733950-d81849df-ddb2-4041-ae22-b8f4c2ab86e3.png#)
Ribbon 在工作时分成两步
第一步先选择 EurekaServer，它优先选择在同一个区域内负载均衡较少的 Server。 第二步在根据用户指定的策略，在从 server 去到的服务注册列表中选择一个地址。 其中 Ribbon 提供了多种策略，比如**轮询**（默认），随机和根据响应时间加权重,,,等等

## 测试：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834734450-2b7a1542-4c7e-4248-8f5e-300703615076.jpeg#)参考 springcloud-provider-dept-8001，新建两份，分别为 8002,8003！ 全部复制完毕，修改启动类名称，修改端口号名称！
新建 8002/8003 数据库，各自微服务分别连接各自的数据库，复制 DB1！ 新建 springcloud01
新建 springcloud02
新建 springcloud03

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834735110-a502a94e-4387-4865-b5e5-9c71069f491f.jpeg#)
修改 8002/8003 各自的 YML 文件端口
数据库连接
实例名也需要修改

1 instance:
2
instance-id: springcloud-provider-dept8003

对外暴露的统一的服务实例名【三个服务名字必须一致！】

1 application:
2
name: springcloud-provider-dept

启动 3 个 Eureka 集群配置区

## 启动 3 个 Dept 微服务并都测试通过

http://localhost:8001/dept/list http://localhost:8002/dept/list http://localhost:8003/dept/list

## 启动 springcloud-consumer-dept-ribbon-80

客户端通过 Ribbon 完成负载均衡并访问上一步的 Dept 微服务
[http://localhost/consumer/dept/list](http://localhost/consumer/dept/list)
多刷新几次注意观察结果！

## 总结：

Ribbon 其实就是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和
Eureka 结合只是其中的一个实例。

Ribbon 核心组件 IRule
IRule：根据特定算法从服务列表中选取一个要访问的服务！

RoundRobinRule【轮询】
RandomRule【随机】
AvailabilityFilterRule【会先过滤掉由于多次访问故障而处于断路器跳闸的服务，还有并发的连接 数量超过阈值的服务，然后对剩余的服务列表按照轮询策略进行访问】
WeightedResponseTimeRule【根据平均响应时间计算所有服务的权重，响应时间越快服务权重越 大，被选中的概率越高，刚启动时如果统计信息不足，则使用 RoundRobinRule 策略，等待统计信息足够，会切换到 WeightedResponseTimeRule】
RetryRule【先按照 RoundRobinRule 的策略获取服务，如果获取服务失败，则在指定时间内会进行 重试，获取可用的服务】
BestAvailableRule【会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并 发量最小的服务】
ZoneAvoidanceRule【默认规则，复合判断 server 所在区域的性能和 server 的可用性选择服务器】

查看分析源码：

1. IRule
1. ILoadBalancer
1. AbstractLoadBalancer
1. AbstractLoadBalancerRule：这个抽象父类十分重要！核心
1. RoundRobinRule

分析一下方法

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
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
public Server choose(ILoadBalancer lb, Object key) { if (lb == null) {
log.warn("no load balancer"); return null;
}
Server server = null; int count = 0;
while (server == null && count++ < 10) {
List<Server> reachableServers = lb.getReachableServers(); List<Server> allServers = lb.getAllServers();
int upCount = reachableServers.size(); int serverCount = allServers.size();

if ((upCount == 0) || (serverCount == 0)) {
log.warn("No up servers available from load balancer: " + lb); return null;
}

int nextServerIndex = incrementAndGetModulo(serverCount);
//每一次得到下一个 ServerIndex，也就是所谓的轮询 server = allServers.get(nextServerIndex);

if (server == null) {
/_ Transient. _/ Thread.yield(); continue;
}

if (server.isAlive() && (server.isReadyToServe())) { return (server);
}

33
34
35
36
37
38
39
// Next.
server = null;
}
40
41
42
43
if (count >= 10) {
log.warn("No available alive servers after 10 tries from load balancer: "

- lb);
  }
  return server;
  }

**切换为随机策略实现试试**，在 ConﬁgBean 中添加方法

1. @Bean
1. public IRule myRule(){
1. //使用我们重新选择的随机算法，替代默认的轮询！
1. return new RandomRule(); 5 }

重启 80 服务进行访问测试，查看运行结果！【注意，可能服务长时间不使用会崩】
[http://localhost/consumer/dept/list](http://localhost/consumer/dept/list)

## 测试：new RetryRule() 算法

RetryRule【先按照 RoundRobinRule 的策略获取服务，如果获取服务失败，则在指定时间内会进行重 试，获取可用的服务】
1、在运行期间关闭掉一个服务提供者 8002
2、消费者再次测试！发现 404 后继续访问测试！看结果！！！

其余的不再挨个测试了，大家有时间可以去测试玩玩；
现在有一个新的需求，我们不需要这些默认的算法，我们需要自己重新定义，这该怎么办呢？

自定义 Ribbon

## 修改 springcloud-consumer-dept-ribbon-80

**主启动类添加@RibbonClient 注解**
在启动该微服务的时候就能去加载我们自定义的 Ribbon 配置类，从而使配置类生效，例如:

1 @RibbonClient(name="SPRINGCLOUD-PROVIDER- DEPT",configuration=MySelfRule.class)

## 注意配置细节

官方文档明确给出了警告：
这个自定义配置类不能放在@ComponentScan 所扫描的当前包以及子包下，否则我们自定义的这个配置类就会被所有的 Ribbon 客户端所共享，也就是说达不到特殊化定制的目的了！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834735612-eed6757c-8f38-4b42-a28c-280d668d693c.jpeg#)

## 步骤

1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834736034-7707ae05-330d-43f6-b43e-0e221a884f20.png#)由于有以上配置细节原因，我们建立一个包 com.kuang.myrule
1. 在这里新建一个自定义规则的 Rubbion 类

1
2
3
4
5
6
7
8
9
@Configuration
public class MySelfRule {
@Bean
public IRule myRule(){
return new RandomRule(); //Ribbon 默认是轮询，我们自定义为随机算法
}
}

1. 在主启动类上配置我们自定义的 Ribbon

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
@SpringBootApplication @EnableEurekaClient
//核心！
@RibbonClient(name="SPRINGCLOUD-PROVIDER- DEPT",configuration=MySelfRule.class) public class DeptConsumer80_App {
public static void main(String[] args) { SpringApplication.run(DeptConsumer80_App.class,args);
}
}

1. 启动所有项目，访问测试，看看我们编写的随机算法，现在是否生效！

**自定义规则深度解析**
1、问题：依旧轮询策略，但是加上新需求，每个服务器要求被调用 5 次，就是以前每一个机器一次，现 在每个机器 5 次；
2、解析源码：RandomRule.java ， IDEA 直接点击进去，复制出来，变成我们自己的类 KuangRondomRule
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834736277-cd8310d9-3bfe-419e-8932-6a9a61fba7a2.jpeg#)
仔细分析阅读源码

1 public class KuangRondomRule extends AbstractLoadBalancerRule { 2
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
@edu.umd.cs.findbugs.annotations.SuppressWarnings(value = "RCN_REDUNDANT_NULLCHECK_OF_NULL_VALUE")
//ILoadBalancer 选择的随机算法
public Server choose(ILoadBalancer lb, Object key) { if (lb == null) {
return null;
}
Server server = null;
while (server == null) {
//查看线程是否中断了
if (Thread.interrupted()) { return null;
}

//Reachable： 可及；可到达；够得到
List<Server> upList = lb.getReachableServers(); //活着的服务 List<Server> allList = lb.getAllServers(); //获取所有的服务

int serverCount = allList.size(); if (serverCount == 0) {
return null;

| 24 |

} | }

int index = chooseRandomInt(serverCount); //生成区间随机数！ server = upList.get(index); //从活着的服务中，随机取出一个

if (server == null) { Thread.yield(); continue;
}

if (server.isAlive()) { return (server);
}

server = null; Thread.yield();
}

return server;

}

//随机
protected int chooseRandomInt(int serverCount) {
return ThreadLocalRandom.current().nextInt(serverCount);
}

@Override
public Server choose(Object key) {
return choose(getLoadBalancer(), key);
}

@Override
public void initWithNiwsConfig(IClientConfig clientConfig) {

| }   |
| --- | --- | --- |
| 25  |     |     |
| 26  |     |     |
| 27  |     |     |
| 28  |     |     |
| 29  |     |     |
| 30  |     |     |
| 31  |     |     |
| 32  |     |     |
| 33  |     |     |
| 34  |     |     |
| 35  |     |     |
| 36  |     |     |
| 37  |     |     |
| 38  |     |     |
| 39  |     |     |
| 40  |     |     |
| 41  |     |     |
| 42  |     |     |
| 43  |     |     |
| 44  |     |     |
| 45  |     |     |
| 46  |     |     |
| 47  |     |     |
| 48  |     |     |
| 49  |     |     |
| 50  |     |     |
| 51  |     |     |
| 52  |     |     |
| 53  |     |     |
| 54  |     |     |
| 55  |     |     |
| 56  |     |     |
| 57  |     |     |
| 58  |     |     |
| 59  |     |     |
| 60  |     |     |

参考源码修改为我们需求要求的 KuangRondomRule.java

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
17
18
public class KuangRondomRule extends AbstractLoadBalancerRule {
// total = 0
// index = 0
当 total 数等于 5 以后，我们指针才能往下走
当前对外提供服务的服务器地址
// 如果 total 等于 5，则 index+1，将 total 重置为 0 即可！
// 问题：我们只有 3 台机器，所有 total>3 则将 total 置为 0；
private int total = 0;
private int currentIndex = 0;
//总共被调用的次数
//当前提供服务的机器序号！
//ILoadBalancer 选择的随机算法
public Server choose(ILoadBalancer lb, Object key) { if (lb == null) {
return null;
}
Server server = null;

| 19 |

//
// | while (server == null) {
//查看线程是否中断了
if (Thread.interrupted()) { return null;
}

//Reachable： 可及；可到达；够得到
List<Server> upList = lb.getReachableServers(); //活着的服务 List<Server> allList = lb.getAllServers(); //获取所有的服务

int serverCount = allList.size(); if (serverCount == 0) {
return null;
}

int index = chooseRandomInt(serverCount); //生成区间随机数！ server = upList.get(index); //从活着的服务中，随机取出一个

//+=====================================
if (total<5){
server = upList.get(currentIndex); total++;
}else {
total = 0; currentIndex++;
if (currentIndex>=upList.size()){ currentIndex = 0;
}
server = upList.get(currentIndex);
}
//+=====================================

if (server == null) { Thread.yield(); continue;
}

if (server.isAlive()) { return (server);
}

server = null; Thread.yield();
}

return server;

}

//随机
protected int chooseRandomInt(int serverCount) {
return ThreadLocalRandom.current().nextInt(serverCount);
}

@Override
public Server choose(Object key) {
return choose(getLoadBalancer(), key);
} |
| --- | --- | --- |
| 20 | | |
| 21 | | |
| 22 | | |
| 23 | | |
| 24 | | |
| 25 | | |
| 26 | | |
| 27 | | |
| 28 | | |
| 29 | | |
| 30 | | |
| 31 | | |
| 32 | | |
| 33 | | |
| 34 | | |
| 35 | | |
| 36 | | |
| 37 | | |
| 38 | | |
| 39 | | |
| 40 | | |
| 41 | | |
| 42 | | |
| 43 | | |
| 44 | | |
| 45 | | |
| 46 | | |
| 47 | | |
| 48 | | |
| 49 | | |
| 50 | | |
| 51 | | |
| 52 | | |
| 53 | | |
| 54 | | |
| 55 | | |
| 56 | | |
| 57 | | |
| 58 | | |
| 59 | | |
| 60 | | |
| 61 | | |
| 62 | | |
| 63 | | |
| 64 | | |
| 65 | | |
| 66 | | |
| 67 | | |
| 68 | | |
| 69 | | |
| 70 | | |
| 71 | | |
| 72 | | |
| 73 | | |
| 74 | | |
| 75 | | |
| 76 | | |

| 77 |

} |
@Override
public void initWithNiwsConfig(IClientConfig clientConfig) {

| }   |
| --- | --- | --- |
| 78  |     |     |
| 79  |     |     |
| 80  |     |     |
| 81  |     |     |
| 82  |     |     |
| 83  |     |     |

调用，在我们自定义的 IRule 方法中返回刚才我们写好的随机算法类

1. @Configuration
1. public class MySelfRule { 3

4
5
6
7
8
9
@Bean
public IRule myRule(){
return new KuangRondomRule(); //Ribbon 默认是轮询，我们自定义为 KuangRondomRule
}
}

测试

# Feign 负载均衡

简介
feign 是声明式的 web service 客户端，它让微服务之间的调用变得更简单了，类似 controller 调用
service。
Spring Cloud 集成了 Ribbon 和 Eureka，可在使用 Feign 时提供负载均衡的 http 客户端。只需要创建一个接口，然后添加注解即可！

feign ，主要是社区，大家都习惯面向接口编程。这个是很多开发人员的规范。调用微服务访问两种方法

1. 微服务名字 【ribbon】
1. 接口和注解 【feign 】

## Feign 能干什么？

Feign 旨在使编写 Java Http 客户端变得更容易
前面在使用 Ribbon + RestTemplate 时，利用 RestTemplate 对 Http 请求的封装处理，形成了一套模板化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多 处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以，
Feign 在此基础上做了进一步封装，由他 来帮助我们定义和实现依赖服务接口的定义，在 Feign 的实现下，我们只需要创建一个接口并使用注解的方式来配置它（类似于以前 Dao 接口上标注 Mapper 注解，现在是一个微服务接口上面标注一个 Feign 注解即可。）即可完成对服务提供方的接口绑
定，简化了使用 Spring Cloud Ribbon 时，自动封装服务调用客户端的开发量。

## Feign 集成了 Ribbon

利用 Ribbon 维护了 springcloud-Dept 的服务列表信息，并且通过轮询实现了客户端的负载均衡， 而与 Ribbon 不同的是，通过 Feign 只需要定义服务绑定接口且以声明式的方法，优雅而且简单的实现了服务调用。

Feign 使用步骤
1、参考 springcloud-consumer-dept-ribbon-80
2、新建 springcloud-consumer-dept-feign-80
修改主启动类名称
将 springcloud-consumer-dept-80 的内容都拷贝到 feign 项目中删除 myrule 文件夹
修改主启动类的名称为 DeptConsumerFeign80

3、springcloud-consumer-dept-feign-80 修改 pom.xml ， 添加对 Feign 的支持。

1. <!--Feign相关-->
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-feign</artifactId>
1. <version>1.4.7.RELEASE</version>
1. </dependency>

4、修改 springcloud-api 工程
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834736816-7ff7b5b7-11e7-4b75-b092-3b3623b7c6c0.jpeg#)pom.xml 添加 feign 的支持新建一个 Service 包

编写接口 DeptClientService，并增加新的注解@FeignClient。

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
@FeignClient(value = "SPRINGCLOUD-PROVIDER-DEPT")
public interface DeptClientService {
@GetMapping("/dept/get/{id}")
public Dept queryById(@PathVariable("id") Long id); //根据 id 查询部门

@GetMapping("/dept/list")
public List<Dept> queryAll(); //查询所有部门

@PostMapping(value = "/dept/add")
public boolean addDept(Dept dept); //添加一个部门
}

mvn 清理一下
5、springcloud-consumer-dept-feign-80 工程修改 Controller，添加上一步新建的 DeptClientService

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
17
18
19
20
21
22
@RestController
public class DeptConsumerController {
@Autowired
private DeptClientService service = null;

@RequestMapping("/consumer/dept/add") public boolean add(Dept dept){
return this.service.addDept(dept);
}

@RequestMapping("/consumer/dept/get/{id}") public Dept get(@PathVariable("id") Long id){
return this.service.queryById(id);
}

@RequestMapping("/consumer/dept/list") public List<Dept> list(){
return this.service.queryAll();
}
}

6、microservicecloud-consumer-dept-feign 工程修改主启动类，开启 Feign 使用！

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
@SpringBootApplication @EnableEurekaClient
@EnableFeignClients(basePackages = {"com.kuang.springcloud"}) @ComponentScan("com.kuang.springcloud")
public class DeptConsumer80_Feign_App {
public static void main(String[] args) { SpringApplication.run(DeptConsumer80_Feign_App.class,args);
}
}

7、测试
启动 eureka 集群

启动 8001，8002，8003
启动 feign 客户端
测试： [http://localhost/consumer/dept/list](http://localhost/consumer/dept/list)
结论：Feign 自带负载均衡配置项

小结
Feign 通过接口的方法调用 Rest 服务 ( 之前是 Ribbon+RestTemplate )
该请求发送给 Eureka 服务器 （[http://MICROSERVICECLOUD-PROVIDER-DEPT/dept/list](http://microservicecloud-provider-dept/dept/list)）
通过 Feign 直接找到服务接口，由于在进行服务调用的时候融合了 Ribbon 技术，所以也支持负载均衡作用！
feign 其实不是做负载均衡的,负载均衡是 ribbon 的功能,feign 只是集成了 ribbon 而已,但是负载均衡的功能 还是 feign 内置的 ribbon 再做,而不是 feign。
feign 的作用的替代 RestTemplate,性能比较低，但是可以使代码可读性很强。

# Hystrix 断路器

Hystrix 怎么读？

概述

## 分布式系统面临的问题

复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免的失败！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834737295-d79622cd-0a4e-4582-8493-7a4f442fe470.jpeg#)

## 服务雪崩

多个微服务之间调用的时候，假设微服务 A 调用微服务 B 和微服务 C，微服务 B 和微服务 C 又调用其他的微服务，这就是所谓的 “扇出”、如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务 A 的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的 “雪崩效应”。
对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒中内饱和。比失 败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张， 导致整个系统发生更多的级联故障，这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系 的失败，不能取消整个应用程序或系统。
我们需要 ·弃车保帅·

## 什么是 Hystrix

Hystrix 是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，Hystrix 能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。
“断路器” 本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），**向调用方返回一个服务预期的，可处理的备选响应（FallBack），而不是长时间的等待或者抛出 调用方法无法处理的异常，这样就可以保证了服务调用方的线程不会被长时间**，不必要的占用，从而避 免了故障在分布式系统中的蔓延，乃至雪崩.

## 能干嘛

服务降级服务熔断服务限流
接近实时的监控
.....

## 官网资料

[https://github.com/Netﬂix/Hystrix/wiki](https://github.com/Netflix/Hystrix/wiki)

服务熔断

## 是什么

熔断机制是对应雪崩效应的一种微服务链路保护机制。
当扇出链路的某个微服务不可用或者响应时间太长时，会进行服务的降级，进而熔断该节点微服务的调 用，快速返回 错误的响应信息。当检测到该节点微服务调用响应正常后恢复调用链路。在 SpringCloud 框架里熔断机制通过 Hystrix 实现。Hystrix 会监控微服务间调用的状况，当失败的调用到一定阈值，缺省 是 5 秒内 20 次调用失败就会启动熔断机制。
熔断机制的注解是 @HystrixCommand。

## 参考 springcloud-provider-dept-8001

新建 springcloud-provider-dept-hystrix-8001 将之前 8001 的所有东西拷贝一份

## 修改 pom

添加 Hystrix 的依赖

1. <!--Hystrix-->
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-hystrix</artifactId>
1. <version>1.4.7.RELEASE</version>
1. </dependency>

## 修改 yml

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834737638-42182644-858f-4bee-aed3-62a4949be394.png#)修改 eureka 实例的 id

## 修改 DeptController

1、@HystrixCommand 报异常后如何处理

| 1   | //一旦调用服务方法失败并抛出了错误信息后                              |
| --- | --------------------------------------------------------------------- |
| 2   | // 会自动调用 HystrixCommand 标注好的 fallbackMethod 调用类中指定方法 |
| 3   | @HystrixCommand(fallbackMethod = "processHystrix_Get")                |

2、代码内容

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
17
18
19
20
21
22
@RestController
public class DeptController {
@Autowired
private DeptService service;

//一旦调用服务方法失败并抛出了错误信息后
// 会自动调用 HystrixCommand 标注好的 fallbackMethod 调用类中指定方法@GetMapping("/dept/get/{id}") @HystrixCommand(fallbackMethod = "processHystrix_Get") public Dept get(@PathVariable("id") Long id) {
Dept dept = service.queryById(id); if (dept==null){
throw new RuntimeException("该 id："+id+"没有对应的的信息");
}
return dept;
}

public Dept processHystrix_Get(@PathVariable("id") Long id){ return new Dept().setDeptno(id)
.setDname("该 id："+id+"没有对应的信息，null--@HystrixCommand")
.setDb_source("no this database in MySQL");

| 23  |     | }   |
| --- | --- | --- |
| 24  |     |     |
| 25  | }   |     |
| 26  |     |     |

## 修改主启动类添加新注解 @EnableCircuitBreaker

3、修改主启动类的名称为 DeptProviderHystrix8001
4、代码

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
@SpringBootApplication
@EnableEurekaClient //本服务启动之后会自动注册进 Eureka 中！ @EnableDiscoveryClient //服务发现
@EnableCircuitBreaker //对 hystrix 熔断机制的支持 【==========new=======】
public class DeptProviderHystrix8001 {
public static void main(String[] args) { SpringApplication.run(DeptProviderHystrix8001.class,args);
}
}

## 测试

1、启动 Eureka 集群
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834738130-e8b21d89-af28-48ba-8764-68cf8f47b167.png#)2、启动主启动类 DeptProviderHystrix8001
3、启动客户端 springcloud-consumer-dept-80
4、访问 [http://localhost/consumer/dept/get/111](http://localhost/consumer/dept/get/111)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834738521-b9b4cf05-baac-453e-a311-37945fad00f4.png#)

服务降级

## 是什么

整体资源快不够了，忍痛将某些服务先关掉，待渡过难关，再开启回来。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834738856-fb873600-db4f-4bfe-8d62-d276f5e8ed60.jpeg#)

## 服务降级处理是在客户端实现完成的，与服务端没有关系

**修改 springcloud-api 工程，根据已经有的 DeptClientService 接口新建一个实现了 FallbackFactory 接 口的类 DeptClientServiceFallbackFactory**
【注意：这个类上需要@Component 注解！！！】

1. @Component //千万不要忘记
1. public class DeptClientServiceFallbackFactory implements FallbackFactory<DeptClientService> {

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
25
26
@Override
public DeptClientService create(Throwable throwable) { return new DeptClientService() {
@Override
public Dept queryById(Long id) { return new Dept().setDeptno(id)
.setDname("该 id："+id+"没有对应的信息，Consumer 客户端提供
的降级信息，此刻服务 Provider 已经关闭")
.setDb_source("no this database in MySQL");
}
@Override
public List<Dept> queryAll() { return null;
}

@Override
public boolean addDept(Dept dept) { return false;
}
};
}
}

## 修改 springcloud-api 工程，DeptClientService 接口在注解 @FeignClient 中添加 fallbackFactory 属性值

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834739403-a1435357-dcfd-4a96-972f-c272418c37a7.jpeg#)

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
@FeignClient(value = "SPRINGCLOUD-PROVIDER-DEPT",fallbackFactory = DeptClientServiceFallbackFactory.class)
public interface DeptClientService {
@GetMapping("/dept/get/{id}")
public Dept queryById(@PathVariable("id") Long id); //根据 id 查询部门

@GetMapping("/dept/list")
public List<Dept> queryAll(); //查询所有部门

@PostMapping(value = "/dept/add")
public boolean addDept(Dept dept); //添加一个部门
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834739666-8b387c7f-4186-4453-8a0d-20a034e08863.png#)**springcloud-api 工 程 mvn clean install springcloud-consumer-dept-feign-80 工程修改 YML**
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
server:
port: 80
feign:
hystrix: enabled: true

#Eureka 配置 eureka:
client:
register-with-eureka: false #false 表示不向注册中心注册自己

1.  service-url:
1.      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http:/

/eureka7003.com:7003/eureka/
14

**测试**

1. 启动 eureka 集群
1. 启动 springcloud-provider-dept-hystrix-8001
1. 启动 springcloud-consumer-dept-feign-80
1. 正常访问测试

[http://localhost/consumer/dept/get/1](http://localhost/consumer/dept/get/1)

1. 故意关闭微服务启动 springcloud-provider-dept-hystrix-8001
1. 客户端自己调用提示

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834739918-d1796815-656c-46e6-81ee-cc41a15d33d7.png#)[http://localhost/consumer/dept/get/1](http://localhost/consumer/dept/get/1)

此时服务端 provider 已经 down 了，但是我们做了服务降级处理，让客户端在服务端不可用时也会获得提示信息而不会挂起耗死服务器。

小结
服务熔断：一般是某个服务故障或者异常引起，类似现实世界中的 “保险丝” ， 当某个异常条件被触发， 直接熔断整个服务，而不是一直等到此服务超时！
服务降级：所谓降级，一般是从整体负荷考虑，就是当某个服务熔断之后，服务器将不再被调用，此时 客户端可以自己准备一个本地的 fallback 回调，返回一个缺省值。这样做，虽然服务水平下降，但好歹可用，比直接挂掉要强。

服务监控

## 服务监控 hystrixDashboard

除了隔离依赖服务的调用以外，Hystrix 还提供了准实时的调用监控（Hystrix Dashboard），Hystrix 会持续地记录所有通过 Hystrix 发起的请求的执行信息，并以统计报表和图形的形式展示给用户，包括每秒执行多少请求，多少成功，多少失败等等。
Netﬂix 通过 hystrix-metrics-event-stream 项目实现了对以上指标的监控，SpringCloud 也提供了 Hystrix Dashboard 的整合，对监控内容转化成可视化界面！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834740367-2f4d3b5a-a4d5-49db-8a97-9f8ddb73ba05.jpeg#)

## 新建工程 springcloud-consumer-hystrix-dashboard-9001 Pom.xml

复制之前 80 项目的 pom 文件，新增以下依赖！

1. <!--Hystrix-->
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-hystrix</artifactId>
1. <version>1.4.7.RELEASE</version>
1. </dependency>
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
1. <version>1.4.7.RELEASE</version>
1. </dependency>

## application.yaml 配置

1 server:
2 port: 9001

**主启动类改名 + 新注解@EnableHystrixDashboard**

1
2
3
4
5
package com.kuang.springcloud;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication; import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;
6
7
8
9
10
11
12
13
@SpringBootApplication @EnableHystrixDashboard
public class DeptConsumerDashBoardApp9001 { public static void main(String[] args) {
SpringApplication.run(DeptConsumerDashBoardApp9001.class,args);
}
}

**所有的 Provider 微服务提供类(8001/8002/8003) 都需要监控依赖配置**

1. <!--actuator监控信息完善-->
1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-starter-actuator</artifactId>
1. </dependency>

**启动 springcloud-consumer-hystrix-dashboard-9001 该微服务监控消费端**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834740942-1f7f87c7-f52c-45d1-873a-371719a7985b.jpeg#)http://localhost:9001/hystrix

## 这是动物是一只豪猪！ 测试一

1.  启动 eureka 集群
1.  启动 springcloud-consumer-hystrix-dashboard-9001
1.  在 springcloud-provider-dept-hystrix-8001 启动类中增加一个 bean

1.  @Bean
1.  public ServletRegistrationBean hystrixMetricsStreamServlet() {
1.      ServletRegistrationBean registration = new ServletRegistrationBean(new HystrixMetricsStreamServlet());
1.  registration.addUrlMappings("/actuator/hystrix.stream");
1.  return registration; 6 }

1.  启动 springcloud-provider-dept-hystrix-8001
    1. http://localhost:8001/dept/get/1
    1. http://localhost:8001/actuator/hystrix.stream 【查看 1 秒一动的数据流】

## 监控测试

多次刷新 http://localhost:8001/dept/get/1

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834741436-a6f52c17-82db-4b5c-b0dd-53d31264af05.jpeg#)观察监控窗口，就是那个豪猪页面添加监控地址

Delay : 该参数用来控制服务器上轮询监控信息的延迟时间，默认为 2000 毫秒，可以通过配置该属性来降低客户端的网络和 CPU 消耗
Title ： 该参数对应了头部标题 HystrixStream 之后的内容，默认会使用具体监控实例 URL，可以通过配置该信息来展示更合适的标题。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834741956-547efbd1-cdbf-4369-b38c-c9356afd1814.jpeg#)监控结果
如何看
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834742378-84e02b8b-f387-40bc-8283-a752f4ccbdc8.jpeg#)7 色
一圈
实心圆：公有两种含义，他通过颜色的变化代表了实例的健康程度它的健康程度从绿色<黄色<橙色<红色 递减
该实心圆除了颜色的变化之外，它的大小也会根据实例的请求流量发生变化，流量越 大，该实心圆就越大，所以通过该实心圆的展示，就可以在大量的实例中快速发现**故障 实例和高压力实例**。

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834742821-a019d2ed-bb8a-43da-8c3c-6ea9fbecd40e.png#)
一线
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834743023-896e7245-61bd-4682-a604-b86b11236a0d.png#)曲线：用来记录 2 分钟内流量的相对变化，可以通过它来观察到流量的上升和下降趋势！
整图说明
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834743451-354c6399-9370-4029-b9a0-67965ac84aac.jpeg#)
搞懂一个才能看懂复杂的

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834744045-6d5857e8-0f73-4995-9609-6c24ba7dbb97.jpeg#)

# Zuul 路由网关

概述

## 什么是 Zuul？

Zuul 包含了对请求的路由和过滤两个最主要的功能：
其中路由功能负责将外部请求转发到具体的微服务实例上，是实现外部访问统一入口的基础，而过滤器 功能则负责对请求的处理过程进行干预，是实现请求校验，服务聚合等功能的基础。Zuul 和 Eureka 进行整合，将 Zuul 自身注册为 Eureka 服务治理下的应用，同时从 Eureka 中获得其他微服务的消息，也即以后 的访问微服务都是通过 Zuul 跳转后获得。
注意：Zuul 服务最终还是会注册进 Eureka 提供：代理 + 路由 + 过滤 三大功能！

## Zuul 能干嘛？

路由过滤

官网文档：[https://github.com/Netﬂix/zuul](https://github.com/Netflix/zuul)

路由的基本配置

## 新建 Module 模块 springcloud-zuul-gateway-9527

**pom 文件**
添加 Eureka 和 zuul 的配置

1 <dependencies> 2 <!--Zuul-->

1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-zuul</artifactId>

1. <version>1.4.7.RELEASE</version>
1. </dependency>
1. <!--eureka相关-->
1. <dependency>
1. <groupId>org.springframework.cloud</groupId>
1. <artifactId>spring-cloud-starter-eureka</artifactId>
1. <version>1.4.7.RELEASE</version>
1. </dependency>
1. </dependencies>

## application.yaml 配置

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
server:
port: 9527
#spring 的相关配置
spring: application:
name: springcloud-zuul-gateway
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
#eureka 配置
eureka: client:
service-url: defaultZone:
http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http:/
/eureka7003.com:7003/eureka/ instance:
instance-id: gateway9527.com
prefer-ip-address: false #true 访问路径可以显示 IP 地址
#info 配置
info:
app.name: kuang-springcloud company.name: [www.kuangstudy.com](http://www.kuangstudy.com/) build.artifactId: ${project.artifactId} build.version: ${project.version}

**hosts 修改**
路径：C:\Windows\System32\drivers\etc\hosts

1 127.0.0.1 myzuul.com

## 主启动类

1. @SpringBootApplication
1. @EnableZuulProxy
1. public class SpringCloudZuulApp9527 {
1. public static void main(String[] args) {
1. SpringApplication.run(SpringCloudZuulApp9527.class,args); 6 }

7 }

**启动**
Eureka 集群
一个服务提供类：springcloud-provider-dept-8001
zuul 路由
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834744475-7ae0c06e-b221-42de-8186-7bc119dc2877.jpeg#)访问 ：http://localhost:7001/

## 测试

不用路由 ：http://localhost:8001/dept/get/2
使用路由 ：[http://myzuul.com:9527/springcloud-provider-dept/dept/get/2](http://myzuul.com:9527/springcloud-provider-dept/dept/get/2) 网关 / 微服务名字 / 具体的服务

路由访问映射规则
问题：[http://myzuul.com:9527/springcloud-provider-dept/dept/get/2](http://myzuul.com:9527/springcloud-provider-dept/dept/get/2) 这样去访问的话，就暴露了我们真实微服务的名称！这不是我们需要的！怎么处理呢?

## 修改：springcloud-zuul-gateway-9527 工程代理名称

yml 配置修改，增加 Zuul 路由映射！

1. #Zuul 路由映射
1. zuul:
1. routes:
1. mydept.serviceId: springcloud-provider-dept
1. mydept.path: /mydept/\*\*

配置前访问：[http://myzuul.com:9527/springcloud-provider-dept/dept/get/2](http://myzuul.com:9527/springcloud-provider-dept/dept/get/2) 配置后访问：[http://myzuul.com:9527/mydept/dept/get/2](http://myzuul.com:9527/mydept/dept/get/2)
问题，现在访问原路径依旧可以访问！这不是我们所希望的！

## 原真实服务名忽略

1. # Zuul 路由映射
1. zuul:
1. ignored-services: springcloud-provider-dept # 不能再使用这个服务名访问；

ignored：忽略

1. routes:
1. mydept.serviceId: springcloud-provider-dept
1. mydept.path: /mydept/\*\*

测试：现在访问[http://myzuul.com:9527/springcloud-provider-dept/dept/get/2](http://myzuul.com:9527/springcloud-provider-dept/dept/get/2) 就访问不了了上面的例子中，我们只写了一个，那要是有多个需要隐藏，怎么办呢?

1. #Zuul 路由映射
1. zuul:
1. ignored-services: "_" # 通配符 _ ， 隐藏全部的！
1. routes:
1. mydept.serviceId: springcloud-provider-dept
1. mydept.path: /mydept/\*\*

## 设置统一公共前缀

1. #Zuul 路由映射
1. zuul:
1. prefix: /kuang
1. ignored-services: springcloud-provider-dept
1. routes:
1. mydept.serviceId: springcloud-provider-dept
1. mydept.path: /mydept/\*\*

访问：[http://myzuul.com:9527/kuang/mydept/dept/get/2](http://myzuul.com:9527/kuang/mydept/dept/get/2) ，加上统一的前缀！kuang，否则，就访问不了了！
