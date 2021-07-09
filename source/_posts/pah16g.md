---
title: 39、ActiveMQ
urlname: pah16g
date: '2021-07-09 20:49:32 +0800'
tags: []
categories: []
---

# 1、快速入门

## 、前言

MQ 理解

#### MQ = Message Queue = 消息队列 = 消息中间件

我们将其分为两个词理解：
1、消息理解
微信、短信、语言、漂流瓶.....
是消息就一定有一个 发送者 和一个 接收者
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834981044-fd34f50f-1a0f-4af5-a541-c5fe23367162.jpeg#)2、中间件理解

MQ 的产品种类
天上飞的理念必定有落地的实现，MQ 这么多怎么学？
任正非先生说过一句话：对着一个城墙口发起冲锋，总分总研究学习，找到学习方法总结经验，内化吸 收！
**MQ 产品 种类： **Kafka RabbitMQ RocketMQ
ActiveMQ （学会它，然后举一反三）
API 发送和接收
MQ 的高可用性
MQ 的集群和容错配置
MQ 的持久化
延时发送、定时投递签收机制
整合 Spring、SpringBoot

## 、为什么要 MQ

常见面试题：你不会说，就说明你对其不够理解！所以底子很重要！
1、在何种场景下使用了消息中间件？
2、为什么要在系统里引入消息中间件？
3、引入了消息中间件有哪些好处？又有哪些坏处？你是如何避免的！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834981521-059ce5c0-1aa0-4bcb-8492-4a8bc9977d80.jpeg#)
生活中的例子

#### MQ 能干嘛？

解耦消峰异步

为什么要引入 MQ，问题的产生背景

#### 系统之间直接调用在实际工程中存在的问题？

微服务架构后，链式调用是我们在写程序的时候的一般流程，为了完成一个整体功能会将其拆分成多个 函数（或子模块），比如模块 A 调用模块 B，模块 B 调用模块 C，模块 C 调用模块 D，但在大型分布式应用中，系统间的 RPC 交互繁杂，一个功能背后要调用上百个接口并非不可能，从单机架构过渡到分布式微服务架构的通例，这种架构会有哪些问题？

#### 1、系统之间接口耦合比较严重

每新增一个下游功能，都要对上游的相关接口进行改造。比如 系统 A 要发送数据给系统 B 和 C，发送给每个系统的数据可能有差异，因此系统 A 对要发送给每个系统的数据进行了组装，然后逐一发送；、
当代码上线后又新增了一个需求：
把数据也发给 D，新上了一个 D 系统也要接受 A 系统的数据。此时就需要修改 A 系统，让他感知到 D 的存在。同时把数据处理好再给 D。这个过程中你会看到，每接入一个下游系统，都要对 A 系统进行代码改 造，开发联调的效率很低。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834981960-fcc374cd-2927-42f1-a1dd-2e5c377b5269.jpeg#)

#### 2、面对大流量并发时，容易被冲垮

每个接口模块的吞吐能力是有限的，这个上限能力比作是堤坝，当大流量（洪水）来临时，容易被冲 垮。
比如秒杀业务：
上游系统发起下单购买操作，我就是下单一个操作，下游系统完成秒杀业务逻辑（读取订单，库存检 查，库存冻结，余额检查，余额冻结，订单生成，余额扣减，库存扣减，生成流水，余额解冻，库存解 冻）

#### 3、等待同步存在性能问题

RPC 接口基本上是同步调用，整体的服务性能遵循 “木桶理论”，即整体系统的耗时取决于链路中最慢的那个接口。
比如 A 调用 B/C/D 都是 50ms，但此时 B 又调用了 B1，花费 2000ms，那么直接就拖累了整个服务性能。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834982495-13b032d1-d91e-4966-a41e-16ddda18dd5e.jpeg#)

#### 需要有一种技术能够摆平上述情况：

1、要做到系统解耦，当新的模块接进来时，可以做到代码改动最小；**能够解耦**
2、设置流量缓冲池，可以让后端系统按照自身吞吐能力进行消费，不被冲垮； **能够削峰**

3、强弱依赖梳理能将非关键调用链路的操作异步化并提升整体系统的吞吐能力；**能够异步**

## 、是什么

定义
面向消息的中间件（message-oriented middleware）MOM 能够很好的解决以上问题。
是指利用高效可靠的消息传递机制进行与平台无关的数据交流，并基于数据通信来进行分布式系统的集 成。
通过提供**消息传递**和**消息排队**模型在分布式环境下提供应用解耦、弹性伸缩、冗余存储、流量削峰、异 步通信、数据同步等功能。
大致的过程如下：
发送者把消息发送给消息服务器，消息服务器将消息存放在若干队列/主题 中，在合适的时候，消息服务器会将消息转发给接受者。在这个过程中，发送和接收是异步的，也就是发送无需等待，而且发送者 和接收者的生命周期也没有必然关系。
尤其在发布 pub/订阅 sub 模式下，也可以完成一对多的通信，即让一个消息 有多个接受者。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834983075-41c8e9e8-cdb1-4109-b4eb-5bae028c7f11.jpeg#)

特点

#### 采用异步处理模式

消息发送者可以发送一个消息而无需等待响应。消息发送者将消息发送到一条虚拟的通道（队列、主 题）上；
消息接收者则订阅或监听该通道。一条信息可能最终转发给一个或多个消息接收者，这些接收者都无需 对消息发送者做出同步回应。整个过程都是异步的。
例子：
也就是说，一个系统跟另外一个系统之间进行通信的时候，假如系统 A 希望发送一个消息给系统 B，让他 去处理。
但是系统 A 不关注系统 B 到底怎么处理或者有没有处理好，所以系统 A 把消息发送给 MQ，然后就不管这条消息的 死活的，接着系统 B 从 MQ 里消费出来处理即可。至于怎么处理，是否处理完毕，什么时候处理，都是系统 B 的事儿，与系统 A 无关。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834983577-5bbe7734-861a-415c-b8b7-67649e2b2106.jpeg#)
这样的一种通信方式，就是所谓的 “异步” 通信方式，对于系统 A 来说，只要把消息发给 MQ，然后系统 B 就会异步的去进行处理了，系统 A 不需要 “同步” 的等待系统 B 处理完。这样的好处是什么呢？两个字 “解耦”。

#### 应用系统之间解耦合

发送者和接收者不必了解对方，只需要确认消息； 发送者和接收者不必同时在线。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834984170-b000e4b3-302c-42e5-8954-eada71d53230.jpeg#)

## 、下载

如何使用操作
实现高可用、高性能、可伸缩、易用和安全的企业级面向消息服务的系统异步消息的消费和处理
控制消息的消费顺序
可以和 Spring、SpringBoot 整合简化编码配置集群容错的 MQ 集群
.....

下载
官网地址：[http://activemq.apache.org/](http://activemq.apache.org/)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834984877-f3d41877-9bdb-45a0-bcf5-e14112e1ba6f.jpeg#)
翻译：
Apache ActiveMQ™ 是最流行的开源，多协议，基于 Java 的消息传递服务器。它支持行业标准协议，因此用户可以通过广泛的语言和平台从客户选择中受益。可以使用 C，C ++，Python，.Net 等进行连接。使用无处不在的**AMQP**协议集成您的多平台应用程序。在 Websocket 上使用**STOMP**在 Web 应用程序之间交换消息。使用**MQTT**管理您的 IoT 设备。支持您现有的**JMS**基础结构及其他。ActiveMQ 提供了强大的 功能和灵活性来支持任何消息传递用例。
ActiveMQ 当前有两种“风味”可用-“经典” 5.x 代理和“下一代” Artemis 代理。一旦 Artemis 与 5.x 代码库达到足够的功能奇偶校验级别，它将变为 ActiveMQ6。

#### 点击下方下载即可：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834985412-95077f3f-6c0c-4d47-8b35-9ecab1e1a368.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834985853-8f1ba008-25f8-40ba-a0c3-806280c81adb.jpeg#)
下载 Linux 版本！我们这里使用 Docker 安装！

## 、安装 ActiveMQ

systemctl stop docker

我们这里使用 Docker 安装！
1、启动 Docker
systemctl start docker
2、搜索 ActiveMQ 镜像
docker search activemq

停止 Docker

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834986728-f58c097f-bba9-44df-90c3-5acdc34e694b.jpeg#)

3、获取 ActiveMQ 镜像
docker pull webcenter/activemq
docker image
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834987264-29840912-b1bb-4d5a-970b-1c0603c650e4.jpeg#)
4、查看本地镜像

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834987722-05be20fe-5c1c-47d0-aea5-0ad7ad1c40b6.jpeg#)

5、docker 启动 ActiveMQ 命令
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834988252-8c283001-5dda-44b0-8b22-210fe63084c0.png#)
docker run -d --name activemq -p 61616:61616 -p 8161:8161 webcenter/activemq

61616 是 activemq 的容器使用端口，提供 JMS 服务（映射为 61616）
8161 是 web 页面管理端口（对外映射为 8161）
停止
docker stop 容器 id
6、使用 docker ps 查看 ActiveMQ 已经运行了
docker ps -a
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834988793-213d982d-6e0d-4d44-a63e-ac69a49d1a97.jpeg#)

7、使用 进入 ActiveMQ， 退出
docker exec -it activemq /bin/bash
exit
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834989313-08b6d079-467c-419b-8928-cc71fd43665c.png#)

8、Linux 开启端口号供外界访问

| 1   | #查看防火墙状态                                                                       |
| --- | ------------------------------------------------------------------------------------- |
| 2   | firewall-cmd --state                                                                  |
| 3   |                                                                                       |
| 4   | #开启防火墙                                                                           |
| 5   | systemctl start firewalld.service                                                     |
| 6   |                                                                                       |
| 7   | #开启 61616 端口                                                                      |
| 8   | firewall-cmd --zone=public --add-port=61616/tcp --permanent                           |
| 9   | firewall-cmd --zone=public --add-port=8161/tcp --permanent                            |
| 10  |                                                                                       |
| 11  | 解释这个命令：                                                                        |
| 12  | --zone=public：表示作用域为公共的；                                                   |
| 13  | --add-port=8161/tcp：添加 tcp 协议的端口 8161；                                       |
| 14  | --permanent：永久生效，如果没有此参数，则只能维持当前服务生命周期内，重新启动后失效； |
| 15  |                                                                                       |
| 16  | # 重启防火墙                                                                          |
| 17  | systemctl restart firewalld.service                                                   |
| 18  |                                                                                       |
| 19  | # 输入命令重新载入配置                                                                |
| 20  | firewall-cmd --reload                                                                 |
| 21  | #查看开启的端口列表                                                                   |
| 22  | firewall-cmd --permanent --list-port                                                  |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834989824-7dbd955a-c4b9-4d00-90e7-a5b332fd0573.png#)

#### 测试访问控制台：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834990282-fe50e765-c9d2-40ec-9a5d-e2918d3f57d5.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834991038-a1c1d415-2185-4a62-8c36-4f7ad0450e5b.jpeg#)192.168.0.110:8161/admin 默认账号密码、admin-admin

# 2、上手使用

## 、标准 API

基础测试项目构建
1、新建一个 Maven 项目 activemq-study
2、导入对应的 pom 依赖

1 <!-- 根据版本下载 -->
2 <dependency>

1. <groupId>org.apache.activemq</groupId>
1. <artifactId>activemq-all</artifactId>
1. <version>5.14.3</version>
1. </dependency>
1. <!-- [https://mvnrepository.com/artifact/org.apache.xbean/xbean-spring](https://mvnrepository.com/artifact/org.apache.xbean/xbean-spring) -->
1. <dependency>
1. <groupId>org.apache.xbean</groupId>
1. <artifactId>xbean-spring</artifactId>
1. <version>4.14</version>
1. </dependency> 13
1. <!-- [https://mvnrepository.com/artifact/org.projectlombok/lombok](https://mvnrepository.com/artifact/org.projectlombok/lombok) -->
1. <dependency>
1. <groupId>org.projectlombok</groupId>
1. <artifactId>lombok</artifactId>
1. <version>1.18.8</version>
1. </dependency>
1. <!-- [https://mvnrepository.com/artifact/junit/junit](https://mvnrepository.com/artifact/junit/junit) -->
1. <dependency>
1. <groupId>junit</groupId>
1. <artifactId>junit</artifactId>
1. <version>4.12</version>
1. </dependency>
1. <!-- [https://mvnrepository.com/artifact/org.slf4j/slf4j-api](https://mvnrepository.com/artifact/org.slf4j/slf4j-api) -->
1. <dependency>
1. <groupId>org.slf4j</groupId>
1. <artifactId>slf4j-api</artifactId>
1. <version>1.7.30</version>
1. </dependency>

JMS 编码总体架构
JMS 百度百科：什么是 JMS
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834991492-c4d91d55-3a81-456f-b0a3-14ac7d70a229.png#)
JMS 编码总体架构：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834992028-18a6d892-6cf8-47f6-a2f6-e23b0613195a.jpeg#)

回顾 JDBC 的编码套路：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834992526-f52441e7-3024-4876-a3c7-a68759fc52fc.jpeg#)

粗说目的地 Destination 队列和主题
消息队列包括两种模式，点对点模式（point to point， queue）和发布/订阅模式
（publish/subscribe，topic）。

#### 两大模式特性：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834993079-69261288-b3e3-49b8-aefc-852b50936d5a.jpeg#)

1.  **、队列**

**在点对点的消息传递域中，目的地被称为队列（Queue）**
1、每个消息只能有一个消费者，类似 1 对 1 的关系。好比个人快递自己领取自己的。
2、消息的生产者和消费者之间没有时间上的相关性。无论消费者在生产者发送消息的时候是否处于运行 状态，消费者都可以提取消息。好比我们的发送短信，发送者发送后不见得接收者会即收即看。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834993515-f08983fe-ffdc-4e78-8d77-704fd4b4217e.jpeg#)3、消息被消费后队列中不会再存储，所以消费者不会消费到已经被消费掉的消息。

**1、消息生产者**

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
package com.kuang.activemq.queue;
import org.apache.activemq.ActiveMQConnectionFactory;

import javax.jms.\*;

/\*\*

- @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com)
  \*/
  public class JmsProduce {

public static final String ACTIVEMQ_URL = "tcp://192.168.0.110:61616";

public static void main(String[] args) throws JMSException {

1.  // 1、创建连接工厂, 分析 ActiveMQConnectionFactory 得到参数！
1.  // 使用默认的用户名和密码
1.      ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);

18
19
20
21
22
23
24
25
// 2、通过连接工厂获得 Connection ，然后连接访问
Connection connection = factory.createConnection(); connection.start();
// 3、创建会话 Session
// 参数：事务、签收 默认：AUTO_ACKNOWLEDGE 自动确认
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
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
36
// 4、创建目的地（具体是队列还是 topic）
Queue queue = session.createQueue("queue01");
// 5、创建消息的生产者
MessageProducer producer = session.createProducer(queue);

// 6、通过 producer 生产 3 条消息发送到 MQ 的队列中
for (int i = 1; i <= 3; i++) {
// 7、创建消息
TextMessage textMessage = session.createTextMessage("msg=>" +
i);
37
38
39
40
41
42
43
44
45
46
47
48
49
// 8、通过生产者发布
producer.send(textMessage);
}
// 9、关闭资源
producer.close(); session.close(); connection.close();

System.out.println("消息发布到 MQ 完成");
}
}

发布完成之后，我们可以去控制台查看是否成功！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834994150-c1d257cd-5e08-47e2-8cf3-e62be697ddd7.jpeg#)

控制台说明：

| **Number Of Pending Messages** | **等待消费的消息** | **这个是当前未出队列的数量。总接收数-总出队列数** |
| ------------------------------ | ------------------ | ------------------------------------------------- |
| Number Of Consumers            | 消费者数量         | 消费者端的消费者数量                              |
| Messages Enqueued              | 进队消息数         | 进入队列的总数量，包括出队列的，只增不减          |
| Messages Dequeued              | 出队消息数         | 可以理解为是消费者消费掉的数量                    |

总结：
当有一个消息进入这个队列时，等待消费的消息是 1，进入队列的消息是 1。
当消息消费后，等待消费的消息是 0，进入队列的消息是 1，出队列的消息是 1。再来一条消息时，等待消费的消息是 1，进入队列的消息就是 2。

### 2、消息消费者-1

#### 通过接受消息进行消费

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
package com.kuang.activemq.queue;
import org.apache.activemq.ActiveMQConnectionFactory;

import javax.jms.\*;

/\*\*

- @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com)
  \*/
  public class JmsConsumer {

public static final String ACTIVEMQ_URL = "tcp://192.168.0.110:61616";
public static void main(String[] args) throws JMSException {
// 1、创建连接工厂, 分析 ActiveMQConnectionFactory 得到参数！
//
使用默认的用户名和密码
ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
18
19
20
21
22
23
24
25
// 2、通过连接工厂获得 Connection ，然后连接访问
Connection connection = factory.createConnection(); connection.start();
// 3、创建会话 Session
// 参数：事务、签收 默认：AUTO_ACKNOWLEDGE 自动确认
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
26
27
28
29
30
31
// 4、创建目的地（具体是队列还是 topic）
Queue queue = session.createQueue("queue01");
// 5、创建消息的消费者 consumer
MessageConsumer consumer = session.createConsumer(queue);

| 32 |

} |

} |
while (true){
// consumer.receive(); 一直等
// consumer.receive(long time); 超时就不等了
// 这里消费的类型格式一定要正确，和生产的类型一一对应！ TextMessage receive = (TextMessage) consumer.receive(); if (receive!=null){
System.out.println("消息接收者收到消息:"+receive.getText());
}else {
break;
}
}

// 9、关闭资源 consumer.close(); session.close(); connection.close();

| System.out.println("消息接收完毕"); |
| ----------------------------------- | --- | --- | --- |
| 33                                  |     |     |     |
| 34                                  |     |     |     |
| 35                                  |     |     |     |
| 36                                  |     |     |     |
| 37                                  |     |     |     |
| 38                                  |     |     |     |
| 39                                  |     |     |     |
| 40                                  |     |     |     |
| 41                                  |     |     |     |
| 42                                  |     |     |     |
| 43                                  |     |     |     |
| 44                                  |     |     |     |
| 45                                  |     |     |     |
| 46                                  |     |     |     |
| 47                                  |     |     |     |
| 48                                  |     |     |     |
| 49                                  |     |     |     |
| 50                                  |     |     |     |
| 51                                  |     |     |     |
| 52                                  |     |     |     |
| 53                                  |     |     |     |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834994563-1499dc62-ae61-4edb-a490-efb63227bb4c.jpeg#)成功获取数据：但是程序没有停止！
去查看 web 控制台的数据变化：可以发现消费者一直有一个，因为我们的程序没有停止！停止程序后则为 0；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834995127-ff8bf8e4-0345-40e1-ace9-99c0d60bd4ea.jpeg#)

#### receive 方法说明：

1. // consumer.receive(); 一直等
1. // consumer.receive(long time); 超过多少时间没有消费就结束不等了，单位 ms

**3、消息消费者-2**
**通过监听器的方式来消息消费**

| 1   | package com.kuang.activemq.queue;                     |
| --- | ----------------------------------------------------- |
| 2   |                                                       |
| 3   | import lombok.SneakyThrows;                           |
| 4   | import org.apache.activemq.ActiveMQConnectionFactory; |
| 5   |                                                       |

1. import javax.jms.\*;
1. import java.io.IOException; 8

9 /\*_
10 _ @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com) 11 \*/
12 public class JmsConsumer2 { 13
14 public static final String ACTIVEMQ_URL = "tcp://192.168.0.110:61616"; 15

1. public static void main(String[] args) throws JMSException, IOException

{

1.  // 1、创建连接工厂, 分析 ActiveMQConnectionFactory 得到参数！
1.  // 使用默认的用户名和密码
1.      ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);

20

1.  // 2、通过连接工厂获得 Connection ，然后连接访问
1.  Connection connection = factory.createConnection();
1.  connection.start(); 24
1.  // 3、创建会话 Session
1.  // 参数：事务、签收 默认：AUTO_ACKNOWLEDGE 自动确认
1.      Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

28

1. // 4、创建目的地（具体是队列还是 topic）
1. Queue queue = session.createQueue("queue01"); 31
1. // 5、创建消息的消费者 consumer
1. MessageConsumer consumer = session.createConsumer(queue); 34
1. // setMessageListener，通过监听器消费,异步非阻塞
1. consumer.setMessageListener(new MessageListener() {
1. public void onMessage(Message message) {
1. if (message!=null && message instanceof TextMessage){
1. TextMessage textMessage = (TextMessage)message;
1. try {
1. System.out.println(textMessage.getText());
1. } catch (JMSException e) {
1. e.printStackTrace();

44 }
45 }
46 }
47 });
48
49 // System.in.read(); 阻塞等待
50

1. // 关闭资源
1. consumer.close();
1. session.close();
1. connection.close(); 55

56 System.out.println("消息接收完毕");
57
58 }
59 }

### 4、消费者 3 大问题

1、先生产消息，只启动 1 号消费者，1 号消费者可以消费吗？
Y
2、先生产消息，先启动 1 号消费者，再启动 2 号消费者。2 号消费者可以消费吗？
N
3、先启动两个消费者，再生产 6 个消息，请问，消费情况如何？
两个消费者一人一半

### 5、小结

#### JMS 开发的基本步骤：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834995674-e7144c8f-77b6-4e48-ba56-1b3726d61483.jpeg#)
**两种消费方式：**
1、同步阻塞式：consumer.reveive()
2、异步非阻塞式：监听器 consumer.setMessageListener

## 、主题

#### 在发布订阅消息传递域中，目的地被称为主题（Topic）

1、生产者将消息发布到 Topic 中，每个消息可以有多个消费者，属于 1：N 的关系
2、生产者和消费者之间有时间上的相关性。订阅某一个主题的消费者只能消费自它订阅之后发布的消 息。
3、生产者生产时，Topic 不保存消息它是无状态的不落地，假如无人订阅就去生产，那就是一条废消息，所以，一般先启动消费者在启动生产者。
JMS 规范允许客户创建持久订阅，这在一定程度上放松了时间上的相关性要求。持久订阅允许消费者消费它在未处于激活状态时发送的消息。一句话，好比我们的微信公众号订阅

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834996109-ee644d53-0f51-4669-a601-ed6caf5d1487.jpeg#)

**1、生产者**
和队列的是一样的，只是创建的对象不同！
session.createTopic

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
package com.kuang.activemq.topic;
import org.apache.activemq.ActiveMQConnectionFactory;

import javax.jms.\*;

/\*\*

- @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com)
- 发布即生产者

\*/
public class JmsProduceTopic {

public static final String ACTIVEMQ_URL = "tcp://192.168.0.110:61616";
public static void main(String[] args) throws JMSException {
// 1、创建连接工厂, 分析 ActiveMQConnectionFactory 得到参数！
//
使用默认的用户名和密码
ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
19
20
21
22
23
24
25
26
// 2、通过连接工厂获得 Connection ，然后连接访问
Connection connection = factory.createConnection(); connection.start();
// 3、创建会话 Session
// 参数：事务、签收 默认：AUTO_ACKNOWLEDGE 自动确认
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
27
28
29
30
31
32
33
// 4、创建目的地（具体是队列还是 topic）
Topic topic = session.createTopic("topic-kuangshen");
// 5、创建消息的生产者
MessageProducer producer = session.createProducer(topic);

34
35
36
37

38
39
40
41
42
43
44
45
46
47
48
49
50
// 6、通过 producer 生产 3 条消息发送到 MQ 的队列中
for (int i = 1; i <= 3; i++) {
// 7、创建消息
TextMessage textMessage = session.createTextMessage("topic-
msg=>" + i);
// 8、通过生产者发布 producer.send(textMessage);
}
// 9、关闭资源
producer.close(); session.close(); connection.close();

System.out.println("topic 消息发布到 MQ 完成");
}
}

### 2、消费者

和队列的是一样的，只是创建的对象不同！
session.createTopic

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
package com.kuang.activemq.topic;
import org.apache.activemq.ActiveMQConnectionFactory;

import javax.jms.\*;
import java.io.IOException;

/\*\*

- @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com)
- 订阅即消费者

\*/
public class JmsConsumerTopic {

public static final String ACTIVEMQ_URL = "tcp://192.168.0.110:61616";

public static void main(String[] args) throws JMSException, IOException
{

1.  // 1、创建连接工厂, 分析 ActiveMQConnectionFactory 得到参数！
1.  // 使用默认的用户名和密码
1.      ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);

20
21
22
23
24
25
26
27
// 2、通过连接工厂获得 Connection ，然后连接访问
Connection connection = factory.createConnection(); connection.start();
// 3、创建会话 Session
// 参数：事务、签收 默认：AUTO_ACKNOWLEDGE 自动确认
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
28
29
// 4、创建目的地（具体是队列还是 topic）

30
31
32
33
34
35
36
37
38
39
40
Topic topic = session.createTopic("topic-kuangshen");

// 5、创建消息的消费者 consumer
MessageConsumer consumer = session.createConsumer(topic);
41
42
43
44
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
// setMessageListener，通过监听器消费
consumer.setMessageListener((message) -> {
if (message!=null && message instanceof TextMessage){ TextMessage textMessage = (TextMessage)message; try {
System.out.println("接收到 Topic 消
息："+textMessage.getText());
} catch (JMSException e) { e.printStackTrace();
}
}
});

System.in.read(); // 阻塞等待

// 关闭资源 consumer.close(); session.close(); connection.close();

System.out.println("消息接收完毕");

}
}

#### 注意启动顺序：先启动订阅再启动发布，不然发送的消息是废消息，可以启动多个消费者 注意启动顺序：先启动订阅再启动发布，不然发送的消息是废消息，可以启动多个消费者

**3、两大模式对比**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834996630-8f55d875-5501-45d2-8e7e-4a3f95e8e690.jpeg#)

| **比较项目** |
**Topic 模式队列** |
**Queue 模式队列** |
| --- | --- | --- |
| 工作模式 |
“订阅-发布”模式，如果当前没有订阅者，消息将会被丢弃，如果有多个订阅者，那么这些订阅者都会收到消息。 | 负载均衡 模式，如果当前没有消费者，消息也不会丢弃，如果有多个消费者，那么一条消息也只会发送给其中一个消费者，并且要求消费者 ack 信息。 |
| 有无状态 |
无状态 | Queue 数据默认会在 mq 服务器上以文件形式保存，比如 Active MQ 一般保存在
$AMQ_HOME\data\kr-store\data 下面，也可 以配置成 DB 存储。 |
| 传递完整性 |
如果没有订阅者，消息会被丢弃 |
消息不会丢弃 |
| 处理效率 | 由于消息要按照订阅者的数量进行复制，所以处理性能会随着订阅者的增加而明显降低，并且还要结合不同消息协议自身的性能差异 |
由于一条消息只发送给一个消费者，所以就算消费者再多，性能也不会有明显降低。当然不同消息协议的具体性能也是有差异的。 |

1.  **、JMS 规范**

**1、什么是 JMS**

回顾什么是 JavaEE
JavaEE 是一套使用 Java 进行企业级应用开发的大家一致遵循的 13 个核心规范工业标准。JavaEE 平台提供了一个基于组件的方法来加快设计、开发、装配及部署企业应用程序。
1、JDBC （Java Database） 数据库连接
2、JNDI （Java Naming and Directory Interfaces）Java 的命名和目录接口
3、EJB （Enterprise JavaBean）企业 JavaBean
4、RMI（Remote Method Invoke）远程方法调用
5、Java IDL（Interface Description Language）/ CORBA（CommonObject Broker Architecture）接口定义语言、公用对象请求代理程序体系结构
6、JSP （Java Server Pages）
7、Servlet
8、XML （Extensible Markup Language）可扩展的标记语言

#### 9、JMS（Java Message Service） Java 消息服务

10、JTA （Java Transaction API）Java 事务 API

11、JTS（Java Transaction Service）Java 事务服务
12、JavaMail
13、JAF （JavaBean Activation Framework）

Java Message Service （Java 消息服务是 JavaEE 中的一个技术）
什么是 Java 消息服务
Java 消息服务指的是两个应用程序之间进行异步通信的 API，它为标准消息协议和消息服务提供了一组通用接口，包括创建、发送、读取消息等，用于支持 JAVA 应用程序开发。在 JavaEE 中，当两个应用程序使 用 JMS 进行通信时，他们之间并不是直接相连的。而是通过一个共同的消息收发服务组件关联起来以达 到解耦、异步、削峰的效果。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834996858-c5c40c05-110a-4bd3-bac1-f54708338074.jpeg#)

### 2、各种 MQ 中间件

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834997192-e912d11b-67df-4e7a-a6c4-3fac89e33da0.jpeg#)

#### 消息队列的详细比较：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834997854-c9c52897-85e6-4e9b-b4e6-b84526471d58.jpeg#)

**3、JMS 组成元素**
1、JMS provider 实现 JMS 接口和规范的消息中间件，也就是我们的 MQ 服务器
2、JMS producer 消息生产者，创建和发送 JMS 消息的客户端应用
3、JMS consumer 消息消费者，接收和处理 JMS 消息的客户端应用
4、JMS message 消息，分为消息头，消息属性，消息体三部分

### 4、Message

消息头：不会带大家看全部的，只讲平时用的最多的五大特性！
可以通过 setJMS 来定义个属性！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834998290-634d51a7-f35b-475f-9f26-622825a53eb9.jpeg#)
通过 send 来发送，可以看到 send 方法的重载
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834998674-a831e2ba-d7b7-4216-9dc7-72dad79fcda5.jpeg#)
消息发送的目的地，主要是指 Queue 和 Topic
持久模式和非持久模式
JMSDestination
JMSDeliveryMode
一条持久性的消息：应该被传送 “一次仅仅一次”，这就意味着如果 JMS 提供者出现故障，该消息并不会丢失，它会在服务器恢复之后再次传递消息。
一条非持久的消息：最多会传送一次，这意味着服务器出现故障，该消息将永久丢失。 可以设置消息在一定时间后过期，默认是永不过期。
JMSExpiration
消息过期时间，等于 Destination 的 send 方法中的 timeToLive 值加上发送时刻的 GMT 时间值。如果 timeToLive 值等于零，则 JMSExpiration 被设为零，表示该消息永不过期。
如果发送后，在消息过期时间之后消息还没有被发送到目的地，则该消息被清除。 消息优先级
JMSPriority
从 0~9 十个级别，0~4 是普通消息，5~9 是加急消息。
JMS 不要求 MQ 严格按照这十个优先级发送消息，但必须保证加急消息要先于普通消息到达，默认是 4 级。
唯一识别每个消息的标识，由 MQ 产生。
JMSMessageID

消息体 : 封装具体的消息数据，发送和接收的消息体类型必须一致对应！

#### 五种消息体格式：

普通字符串消息，包含一个 string。 重点
TextMessage
一个 Map 类型的消息，key 为 String 类型，而值为 Java 的基本类型。 重点
MapMessage

| 1   | MapMessage mapMessage = session.createMapMessage(); |
| --- | --------------------------------------------------- |
| 2   | mapMessage.setString("k1","v1");                    |
| 3   |                                                     |
| 4   | // 发送和接收的消息体类型必须一致对应！             |

二进制数组消息，包含一个 byte[] 。
BytesMessage
Java 数据流消息，用标准流操作来顺序的填充和读取。 对象消息，包含一个可序列化的 Java 对象。
StreamMessage
ObjectMessage

#### 发送和接收的消息体类型必须一致对应！

**发送和接收的消息体类型必须一致对应！**

消息属性
如果需要除消息头字段以外的值，那么可以使用消息属性！ 识别、去重、重点标注等操作非常有用的方法！

#### 什么是消息属性：

他们是以属性名和属性值对的形式指定的。可以 将属性是为 消息头得扩展，属性指定一些消息头没有包括的附加信息，比如可以在属性里指定消息选择器。
消息的属性就像可以分配给一条消息的附加消息头一样。他们允许开发者添加有关消息的不透明附加信 息。
它们还用于暴露消息选择器在消息过滤时使用的数据。

1. TextMessage message = session.createTextMessage();
1. message.setText(text);
1. message.setStringProperty("username","kuangshen"); // 自定义属性

### 5、消息的可靠性-持久

#### PERSISTENT : 持久性

持久和非持久 队列
队列设置 持久化模式即可！

| 1   | // 默认就是持久化的！                                     |
| --- | --------------------------------------------------------- |
| 2   | MessageProducer producer = session.createProducer(topic); |
| 3   |                                                           |
| 4   | // 非持久化：当服务器宕机，消息不存在。                   |
| 5   | producer.setDeliveryMode(DeliveryMode.NON_PERSISTENT);    |
| 6   |                                                           |
| 7   | // 持久化：当服务器宕机，消息依旧存在                     |
| 8   | producer.setDeliveryMode(DeliveryMode.PERSISTENT);        |

持久化消息：

这是队列的默认传送模式，此模式保证这些消息只被传送一次和成功使用一次。对于这些消息，可靠性 是优先考虑的因素。
可靠性的另一个重要方面是确保持久性消息传送至目标后，消息服务在向消费者传送他们之前不会丢失 这些信息。

持久和非持久 Topic

#### 先启动订阅在启动发布！ 类似于微信订阅公众号

发布方，需要先设定持久化，然后再连接！连接顺序需要变化~

1 package com.kuang.activemq.topic; 2
3 import org.apache.activemq.ActiveMQConnectionFactory; 4
5 import javax.jms.\*; 6
7 /\*\*

1. - @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com)
1. - 发布持久化

10 \*/
11 public class JmsProduceTopic_Persist { 12
13 public static final String ACTIVEMQ_URL = "tcp://192.168.0.110:61616"; 14
15 public static void main(String[] args) throws JMSException { 16

1.      ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
1.  Connection connection = factory.createConnection();
1.      Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
1.  Topic topic = session.createTopic("topic-kuangshen-Persist");
1.  MessageProducer producer = session.createProducer(topic); 22
1.  // 设置持久化策略
1.  producer.setDeliveryMode(DeliveryMode.PERSISTENT); 25 // =========== 在这里进行连接 ==============

26 connection.start(); 27
28
29 for (int i = 1; i <= 3; i++) {

1.      TextMessage textMessage = session.createTextMessage("topic- msg=>" + i);
1.  producer.send(textMessage); 32 }

33

1. producer.close();
1. session.close();
1. connection.close(); 37

38 System.out.println("topic 消息发布到 MQ 完成");
39
40 }
41 }
订阅方，需要创建持久订阅者！

1 package com.kuang.activemq.topic; 2
3 import org.apache.activemq.ActiveMQConnectionFactory; 4

1. import javax.jms.\*;
1. import java.io.IOException; 7

8 /\*\*

1. - @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com)
1. - 订阅持久化

11 \*/
12 public class JmsConsumerTopic_Persist { 13
14 public static final String ACTIVEMQ_URL = "tcp://192.168.0.110:61616"; 15

1. public static void main(String[] args) throws JMSException, IOException

{

1.      ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
1.  Connection connection = factory.createConnection(); 19
1.  // 设置 id，好比你的微信 id;
1.  connection.setClientID("qinjiang"); 22
1.      Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
1.  Topic topic = session.createTopic("topic-kuangshen-Persist"); 25
1.  // 创建主题持久用户(参数二：订阅者)
1.      TopicSubscriber topicSubscriber = session.createDurableSubscriber(topic, "狂神说");

28
29 connection.start(); // 连接
30

1.  // 消费消息
1.  Message message = topicSubscriber.receive();
1.  while (message!=null){
1.  TextMessage textMessage = (TextMessage) message;
1.  System.out.println("收到的持久化 topic："+textMessage.getText());
1.      message = topicSubscriber.receive(5000L); //持久化订阅的时间，不写就永久订阅！

37 }
38

1. session.close();
1. connection.close(); 41

42 }
43
44 }

**先启动订阅在启动发布！**测试 OK，查看控制台
`Subscribers 订阅人
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834999379-4f82919f-63b1-4df6-8d94-73a071cb2ef6.jpeg#)
由于是持久化操作，我们这次先启动发布者，在启动订阅者，看是否可以接受到消息：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834999960-c892ab02-d735-4e0a-8bf3-abe3268edd1a.jpeg#)

#### 注意 ：

1、一定要先运行一次消费者，等于向 MQ 注册，代表我订阅了这个主题
2、然后再运行生产者发送信息，此时，消费者无论是否在线，都会接收到，不在线的话，下次连接的时 候，会把没有收过的消息都接收下来！

### 6、消息的可靠性-事务

为什么需要事务：一组操作要么同时成功，要么同时失败

参数说明

| 1   | // 3、创建会话 Session                            |                            |
| --- | ------------------------------------------------- | -------------------------- |
| 2   | // 参数 1：事务、参数 2：签收                     |                            |
| 3   | Session session = connection.createSession(false, | Session.AUTO_ACKNOWLEDGE); |

#### 事务偏生产者、签收偏消费者

Producer 提交时的事务：两个状态 true、false false：

| 1   | 只要执行 send，就进入到队列中。          |
| --- | ---------------------------------------- |
| 2   | 关闭事务，那第二个签收参数的设置需要有效 |

true：

| 1   | 先执行 send 再执行 commit，消息才被真正的提交到队列中。 |
| --- | ------------------------------------------------------- |
| 2   | 消息需要批量发送，需要缓冲区处理。                      |

代码测试
拷贝，队列消息生产者和 whlie 消费者的代码！ 消息生产者事务测试：

1. // 开启事务
1. Session session = connection.createSession(true, Session.AUTO_ACKNOWLEDGE);
1. try {
1. session.commit(); // 开启了事务就一定要 commit 提交
1. } catch (JMSException e) {
1. session.rollback(); // 出现异常，事务回滚
1. e.printStackTrace(); 8 }

消息消费者事务测试：只开启消费者的事务，设置为 true，测试。
问题：消息消费到了，但是控制台数量没增加，**消息可以重复消费**所以消费者要事务的话，也一定要提交！

### 7、消息的可靠性-签收

分析源码：查看签收的 4 种方式

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
public interface Session extends Runnable {
static final int AUTO_ACKNOWLEDGE = 1; // 自动签收 (默认)
static final int CLIENT_ACKNOWLEDGE = 2; // 客户端手动确认
// 需要显示调用 message.acknowledge(); 进行签收，否则也会导致重复消费
static final int DUPS_OK_ACKNOWLEDGE = 3; // 自动批量确认
static final int SESSION_TRANSACTED = 0;
// 事务提交并确认
}

#### 注意：事务开启和没开启是有区别的

在事务性会话中，当一个事务被成功提交则消息被自动签收。如果事务回滚，则消息会被再次传送。 在非事务性会话中，消息何时被确认取决于创建会话时的应答模式（ACKNOWLEDGE）

### 8、点对点总结

点对点模型是基于队列的，生产者发消息到队列，消费者从队列接收消息，队列的存在使得消息的异步 传输成为可能。和我们平时给朋友发短信类似。
1、如果在 Session 关闭时有部分消息已被接收到但还没有被签收，那当消费者下次连接到相同的队列时，这些消息还会被再次接收。
2、队列可以长久地保存消息直到消费者接收到消息。消费者不需要因为担心消息会丢失而时刻和队列保 持激活的连接状态，**充分体现了异步传输模式的优势。**

### 9、发布订阅总结

JMS Pub/Sub 模型定义了如何向一个内容节点发布和订阅消息，这些节点被称作 Topic。
主题可以被认为是消息的传输中介，发布者（publisher）发布消息到主题，订阅者（subscribe）从主 题订阅消息。
主题使得消息订阅者和消息发送者保持互相独立，不需要接触即可保证消息的传送。

非持久订阅
非持久订阅只有当客户端处于激活状态，也就是和 MQ 保持连接状态才能收到发送到某个主题的消息。 如果消费者处于离线状态，生产者发送的主题消息将会丢失作废，消费者永远不会收到。

#### 一句话：先要订阅注册才能接受到发布，只给订阅者发布。

持久订阅
客户端首先向 MQ 注册一个自己的身份 ID 识别号，当这个客户端处于离线时，生产者会为这个 ID 保存所 有发送到主题的消息，当客户再次连接 到 MQ 时会根据消费者的 ID 得到所有当自己处于离线时发送到主题的消息。
非持久订阅状态下，不能恢复或重新派送一个未签收的消息。持久订阅才能恢复或重新派送一个未签收的消息。

#### 当所有的消息必须被接收，则使用持久订阅，如果允许消息丢失，则使用非持久订阅

**10、Broker**

什么是 Broker
相当于 ActiveMQ 服务器实例
说白了，Broker 其实就是实现了用代码的形式启动 ActiveMQ 将 MQ 嵌入到 Java 代码中，以便随时用随时启动，在用的时候再去启动，这样能节省了资源，也保证了可靠性。
在 Linux 中，我们的 ActiveMQ 启动的时候去读取了配置文件 ActiveMQ.xml，我们可以选择配置不同的文件去启动。

嵌入式 Broker
官方文档：[http://activemq.apache.org/how-do-i-embed-a-broker-inside-a-connection.html](http://activemq.apache.org/how-do-i-embed-a-broker-inside-a-connection.html)
[api 文档：http://activemq.apache.org/maven/apidocs/org/apache/activemq/broker/BrokerService. html](http://activemq.apache.org/maven/apidocs/org/apache/activemq/broker/BrokerService.html)
用 ActiveMQ Broker 作为独立的消息服务器来构建 Java 应用。
ActiveMQ 也支持在 vm 中通信基于嵌入式的 broker，能够无缝的集成其他 java 应用。
1、导入 pom 依赖

1. <!-- [https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-) databind -->
1. <dependency>
1. <groupId>com.fasterxml.jackson.core</groupId>
1. <artifactId>jackson-databind</artifactId>
1. <version>2.10.0</version>
1. </dependency>

2、EmbedBroker.java

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
package com.kuang.activemq.embed;
import org.apache.activemq.broker.BrokerService;

/\*\*

- EmbedBroker 嵌入式代理
- @author 狂神说 Java

\*/
public class EmbedBroker {
public static void main(String[] args) throws Exception {

BrokerService brokerService = new BrokerService();
// jmx 是 java 语言的新框架
// 它允许你把所有资源(包括软硬件)封装成 java 对象,然后暴露在分布式环境中。brokerService.setUseJmx(true); brokerService.addConnector("tcp://localhost:61616");

17
18
19
brokerService.start();
}
}

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835000441-57ec0991-0374-42ce-bcc6-e3c9c17a0f17.jpeg#)3、使用生产者消费者测试一下，看是否可以发布到此地址！

# 3、整合 Springboot

## 、队列-发送者

1、新建一个 springboot 工程
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835000723-63599a95-2f19-4c21-823d-6dfae852805e.png#)
2、导入 pom 依赖

1
2
3
4
5
6
7
8
9
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-activemq</artifactId>
</dependency>

3、配置 application.yml

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
server: port: 6666
spring: activemq:
broker-url: tcp://192.168.0.110:61616 # 自己的 MQ 服务器地址
user: admin password: admin
jms:
pub-sub-domain: false

# false = Queue

true = Topic

# 队列名称

myqueue: springboot-activemq-queue

4、ActiveMQ 配置 Bean，注意包

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
package com.kuang.config;
import org.apache.activemq.command.ActiveMQQueue;
import org.springframework.beans.factory.annotation.Value; import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration; import org.springframework.jms.annotation.EnableJms;

import javax.jms.Queue;
/\*\*

- @author 狂神说 Java
  \*/ @Configuration
  @EnableJms // 声明对 JMS 注解的支持。
  public class ActiveMQConfig { @Value("${myqueue}") private String myqueue;
  @Bean
  public Queue queue(){
  return new ActiveMQQueue(myqueue);
  }
  }

5、编写队列的消息生成者

1 package com.kuang.queue; 2

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
import org.springframework.beans.factory.annotation.Autowired; import org.springframework.jms.core.JmsMessagingTemplate;
import org.springframework.stereotype.Component;
import javax.jms.Queue;
@Component
public class QueueProduce {
@Autowired
private JmsMessagingTemplate template;

@Autowired
private Queue queue;

public void produceMsg(){ template.convertAndSend(queue,"produceMsg");
}
}

6、Controller 测试类

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
package com.kuang.controller;
import com.kuang.queue.QueueProduce;
import org.springframework.beans.factory.annotation.Autowired; import org.springframework.web.bind.annotation.RequestMapping; import org.springframework.web.bind.annotation.RestController;
@RestController
public class ActiveMQController {
@Autowired
private QueueProduce queueProduce;

// 理论每请求一次，都会发送一条消息到队列@RequestMapping("/queue/produce") public void queue_send(){
queueProduce.produceMsg();
System.out.println("消息发送成功！");
}
}

7、启动主启动类测试！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835001073-54d6ec22-561a-4b6a-9271-dd0c2e232dfe.jpeg#)

新要求每隔 3 秒钟往 MQ 推送消息，定时发送
1、在队列发送类中新增方法

1. // 带定时功能的
1. @Scheduled(fixedDelay = 3000)
1. public void produceMsgScheduled(){
1. template.convertAndSend(queue,"produceMsgScheduled");
1. System.out.println("系统定时投递 produceMsgScheduled"); 6 }

2、在主启动类上也要增加一个对应的支持注解

1
2
3
4
5
6
7
8
9
@SpringBootApplication @EnableScheduling // 增加支持
public class SpringbootActivemqApplication {
public static void main(String[] args) { SpringApplication.run(SpringbootActivemqApplication.class, args);
}
}

3、启动主启动类进行测试，观察是否能够定时自动发送！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835001522-8d2f4df3-6106-408b-bb7b-36ca79ea98f2.jpeg#)

## 、队列-消费者

1、新增消息消费者方法类

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
package com.kuang.queue;
import org.springframework.context.annotation.Configuration; import org.springframework.jms.annotation.JmsListener;

import javax.jms.JMSException; import javax.jms.TextMessage;
@Configuration
public class QueueConsumer {
@JmsListener(destination = "${myqueue}")
public void receive(TextMessage textMessage) throws JMSException {
System.out.println("消费者收到消息："+textMessage.getText());
}
}

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835001937-59ba4985-fa88-4ad9-a933-cbf9a60d7c48.jpeg#)2、启动测试，手动发送一条消息，看看是否能够接受到消息 !

## 、主题-生产者

1、修改 jms.pub-sub-domain 为 true ， 然后新增一个主题名称

1 server:
2 port: 6666
3 spring:

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
activemq:
broker-url: tcp://192.168.0.110:61616 # 自己的 MQ 服务器地址
user: admin password: admin
jms:
pub-sub-domain: true

# false = Queue

true = Topic

# 队列名称

myqueue: springboot-activemq-queue

# 主题名称

mytopic: springboot-activemq-topic

2、修改 ActiveMQ 的配置类

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
package com.kuang.config;
import org.apache.activemq.command.ActiveMQTopic;
import org.springframework.beans.factory.annotation.Value; import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration; import org.springframework.jms.annotation.EnableJms;
import javax.jms.Topic;
/\*\*

- @author 狂神说 Java
  \*/ @Configuration
  @EnableJms // 声明对 JMS 注解的支持。
  public class ActiveMQConfig { @Value("${myqueue}") private String myqueue;
// 读取配置文件
@Value("${mytopic}") private String mytopic;

// 修改返回的 bean 为 topic @Bean
public Topic topic(){
return new ActiveMQTopic(mytopic);
}
}

3、新建一个主题发布者类

| 1   | package com.kuang.topic;                                       |
| --- | -------------------------------------------------------------- |
| 2   |                                                                |
| 3   | import org.springframework.beans.factory.annotation.Autowired; |
| 4   | import org.springframework.jms.core.JmsMessagingTemplate;      |
| 5   | import org.springframework.scheduling.annotation.Scheduled;    |
| 6   | import org.springframework.stereotype.Component;               |
| 7   |                                                                |
| 8   | import javax.jms.Topic;                                        |
| 9   |                                                                |
| 10  | @Component                                                     |
| 11  | public class TopicProduce {                                    |

| 12 |

} | @Autowired
private JmsMessagingTemplate jmsMessagingTemplate;

@Autowired
private Topic topic;

@Scheduled(fixedDelay = 3000) public void produceTopic(){
jmsMessagingTemplate.convertAndSend(topic,"=>produceTopic");
System.out.println("消息推送完毕");
} |
| --- | --- | --- |
| 13 | | |
| 14 | | |
| 15 | | |
| 16 | | |
| 17 | | |
| 18 | | |
| 19 | | |
| 20 | | |
| 21 | | |
| 22 | | |
| 23 | | |
| 24 | | |

4、主题发布需要有消费者等待，所以我们等消费者写完一起测试！

## 、主题-消费者

1、编写 topic 的 consumer

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
package com.kuang.queue;
import org.springframework.context.annotation.Configuration; import org.springframework.jms.annotation.JmsListener;

import javax.jms.JMSException; import javax.jms.TextMessage;
@Configuration
public class QueueConsumer {
@JmsListener(destination = "${mytopic}")
public void receive(TextMessage textMessage) throws JMSException {
System.out.println("消费者收到主题发布消息："+textMessage.getText());
}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835002440-3f1baf67-7ea0-4586-9799-89368838bd9b.png#)2、启动项目测试，测试通过！

# 4、ActiveMQ 的传输协议

## 、前言

问题 1：默认的 61616 端口如何修改？
问题 2：你生产上的链接协议如何配置的？使用 Tcp 吗？
官网地址：[http://activemq.apache.org/conﬁguring-version-5-transports.html](http://activemq.apache.org/configuring-version-5-transports.html)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835002997-564b6e8a-f5d8-465f-b4f0-11763b762a03.jpeg#)

是什么
ActiveMQ 支持的 client-broker 通讯协议有：TCP、NIO、UDP、SSL、Http(s)、VM。
其中配置 Transport Connector 的文件在 activeMQ 安装目录的 conf/activemq.xml 中的 <
transportConnectors> 标签之类。
1、使用 进入 ActiveMQ， 退出
docker exec -it activemq /bin/bash
exit
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835003509-19e1885f-0acd-456c-9727-4f79ef0655db.jpeg#)
2、打开 activemq.xml。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835003996-87878f46-6612-4558-af2c-0fc63381d87a.jpeg#)

在配置文件给出的配置信息中，URL 描述信息的头部都是采用协议名称，例如： 描述 amqp 协议的监听端口时，采用的 URL 描述格式为 “amqp://. ”
描述 Stomp 协议的监听端口时，采用的 URL 描述格式为 “stomp://. ”
唯独在进行 openwire 协议描述时，URL，头却采用的 “tcp://. ”

#### 这是因为 ActiveMQ 中默认的消息协议就是 openwire

在控制台中，点击连接查看，可以看到支持的协议。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835004652-51b6cdb4-fa2b-490e-9a28-1fe1ac90d8cd.jpeg#)

## 、不同种类分析

Transmission Control Protocol （TCP） 默认的

1、这是默认的 Broker 配置，TCP 的 Client 监听端口 61616
2、在网络传输数据前，必须要序列化数据，消息是通过一个叫 wire protocol 的来序列化成字节流。默认情况下 ActiveMQ 把 wire protocol 叫做 OpenWire，它的目的是促使网络上的效率和数据快速交互。
3、TCP 连接的 URL 形式如：tcp://hostname:port?key=value&key=value，后面的参数是可选。
4、TCP 传输的优点：
TCP 协议传输可靠性高，稳定性强 高效性：字节流方式传递，效率很高
有效性、可用性：应用广泛，支持任何平台
[5、关于 Transport 协议的可配置参数可以参考官网：http://activemq.apache.org/tcp-transport-refer ence](http://activemq.apache.org/tcp-transport-reference)

New I/O API Protocol （NIO）
1、NIO 协议和 TCP 协议类似，但 NIO 更侧重于底层的访问操作。它允许开发人员对同一资源可有更多的 client 调用和服务端有更多的负载。
2、适合使用 NIO 协议的场景：
可能有大量的 Client 去连接到 Broker 上，一般情况下，大量的 Client 去连接 Broker 是被操作系统的线程所限制的。因此，NIO 的实现比 TCP 需要更少的线程去运行，所以建议使用 NIO 协议。
可能对于 Broker 有一个很迟钝的网络传输，NIO 比 TCP 提供更好的性能。
3、NIO 连接的 URL 形式：nio://hostname:port?key=value
4、Transport Connector 配置示例，参考官网：[http://activemq.apache.org/nio-transport-reference](http://activemq.apache.org/nio-transport-reference)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835005565-81c5f389-4db9-4bce-95f6-196d6d16a548.png#)

AMQP 协议
即 Advanced Message Queuing Protocol，一个提供统一消息服务的应用层标准高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计，基于此协议的客户端与消息中间件可传递消
息。并不受客户端、中间件不同的产品，不同的开发语言等条件限制。

Stomp 协议
STOMP，Streaming Text Orrientated Message Protocol，是流文本定向消息协议，是一种为 MOM
（Message Proented Middleware，面向消息的中间件）设计的简单文本协议。

Secure Sockets Layer Protocol （SSL）

安全套接字层协议。连接的 URL 形式： ssl://hostname:port?key=value

mqtt 协议
MQTT （Message Queuing Telemetry Transport，消息队列遥测传输）是 IBM 开发的一个即时通讯协议，有可能成为物联网的重要组成部分。该协议支持所有平台，几乎可以把所有联网物品和外部连接起 来，被用来当做传感器和致动器（比如通过 Twitter 让房屋联网）的通信协议。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835006098-20933a02-bd41-403c-8843-ea9773dbe11f.jpeg#)

## 、测试 NIO 协议

1、停止 ActiveMQ
2、拷贝 activemq.xml 为 activemq1.xml
3、修改 activemq.xml

1. <transportConnectors>
1. <transportConnector name="nio" uri="nio://0.0.0.0:61618?trace=true"/>
1. </transportConnectors>

#### 一定要开放防火墙端口

如果你不特别指定 ActiveMQ 的网络监听端口，那么这些端口都将使用 BIO 网络 IO 模型。 所以为了首先提高单节点的网络吞吐性能，我们需要明确指定 Active 的网络 IO 模型。
如下所示：URL 格式头是 nio 开投诉，表示这个端口使用以 TCP 协议为基础的 NIOI 网络 IO 模型

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835006747-e2816b52-bb01-401d-bc4b-9d181a15bdad.jpeg#)

4、重启 activeMQ，查看控制台
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835007212-887c3862-7981-4afb-8a29-62df52579409.jpeg#)
5、使用 Java 程序连接测试，需要修改链接的 url 为

| 1   | public | static | final | String | ACTIVEMQ_URL | =   | "nio://192.168.0.110:61618"; |
| --- | ------ | ------ | ----- | ------ | ------------ | --- | ---------------------------- |

问题：由于 Docker 没有开启这个端口映射，所以我们需要重新生成一下容器镜像； 停止 activeMQ
删除容器

启 动 docker run -d --name activemq -p 61616:61616 -p 61618:61618 -p 8161:8161
webcenter/activemq
修改配置文件，再次测试！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835007751-a16c62cc-f27e-4474-a4c0-92edbe76b53d.jpeg#)

## 、NIO 增强

上述的 NIO 性能不错了，如何进一步优化？
问题：URL 格式头以 “nio” 开头，表示这个端口使用 Tcp 协议为基础的 NIO 网络 IO 模型。但是这样的设置方式，只能使用这个端口支持 Openwire 协议。
那么我们怎么既让这个端口支持 NIO 网络 IO 模型，又让它支持多个协议呢？

解决 官方文档：[http://activemq.apache.org/auto](http://activemq.apache.org/auto)
1、使用 auto 关键字。
2、使用 "+" 符号来为端口设置多种特性
3、如果我们既需要某一个端口支持 NIO 网络 IO 模型，又需要它支持多个协议。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835008292-c3e594a8-7fa2-4913-a637-e24c6b71b214.jpeg#)

# 5、消息持久化

为了测试方便，我们在 Windows 也安装一个 ActiveMQ；
1、下载并解压 ActiveMQ
2、进入 bin 目录。打开 cmd 启动
activemq-admin.bat start
3、http://localhost:8161/admin/ 测试控制台

## 、什么是持久化机制

#### 一句话：MQ 服务器宕机了，消息不会丢失的机制

为了避免意外宕机以后丢失信息，需要做到重启后可以恢复消息队列，消息系统一般都会采用持久化机 制。
ActiveMQ 的消息持久化机制有 JDBC、AMQ、KahaDB 和 LevelDB，无论使用哪种持久化方式，消息的存储逻辑都是一致的。
就是在发送者将消息发送出去后，消息中心首先将消息存储到本地数据文件、内存数据库或者远程数据 库等。再试图将消息发送给接收者，成功则将消息从存储中删除，失败则继续尝试发送。
消息中心启动以后首先要检查指定的存储位置。如果有未发送成功的消息，则需要把消息发出去。

AMQ Message Store（了解）
基于文件的存储方式，是以前的默认消息存储，现在不用了

AMQ 是一种文件存储形式，它具有写入速度快和容易恢复的特点。消息存储在一个个文件中，文件的默认大小为 32M，当一个存储文件中的消息已经被全部消费，那么这个文件将被标识为可删除，在下一个 清除阶段，这个文件被删除。AMQ 适用于 ActiveMQ5.3 之前的版本。

## 、KahaDB(默认的)

KahaDB：这个 5.4 版本之后出现的**默认的持久化方式**，与 AMQ 很相似，不同的是只为 Destination 创建 一个索引。
KahaDb 恢复时间远远小于其前身 AMQ 并且使用更少的数据文件，所以可以完全代替 AMQ。（AMQ 适 用于 ActiveMQ5.3 之前的版本。）KahaDB 的持久化机制同样是基于日志文件，索引和缓存。

#### 配置方式：

在 配置文件中配置，更加详细参数请参考：[https://activemq.apache.org/kahadb](https://activemq.apache.org/kahadb)。
activemq.xml

1. <broker brokerName="broker">
1. <persistenceAdapter>
1. <kahaDB directory="数据存储目录" journalMaxFileLength="32mb"/>
1. </persistenceAdapter>
1. </broker>

默认的数据存储位置在，data 目录下的 kahadb 文件夹！
${activemq.data}/kahadb

**KahaDB 主要特性**
1、日志形式存储消息；
2、消息索引以 B-Tree 结构存储，可以快速更新；
3、完全支持 JMS 事务；
4、支持多种恢复机制；

说明
KahaDB 是目前默认的存储方式，可用于任何场景，提高了性能和恢复能力。

#### 消息存储使用一个事务日志和仅仅用一个索引文件来存储它所有的地址。

KahaDB 是一个专门针对消息持久化的解决方案，它对典型的消息使用模式进行了优化。数据被追加到 data logs 中。当不再需要 log 文件中的数据的时候，log 文件会被丢弃。

kahaDB 的存储原理
kahadb 在消息保存目录中只有 4 类文件和一个 lock，跟 ActiveMQ 的其他几种文件存储引擎相比这就非常简洁了；

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835008704-15beb37e-7372-42a1-8892-e277e17fd2ea.jpeg#)

#### db-[number].log

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835009083-9fe04338-425f-46b0-bdf5-03722d7b8e28.jpeg#)KahaDB 存储消息到预定义大小的数据记录文件中，文件命名为 db-[number].log 。当数据文件已满时，一个新的文件会随之创建，number 数值也会随之递增，它随着消息数量的增多，如每 32M 一个文件，文件名按照数字进行编号，如 db-1.log、db-2.log、... 。当不再有引用到数据文件中的任何消息时，文件会被删除或归档。

#### db.data

该文件包含了持久化的 BTree 索引，索引了消息数据记录中的消息，它是消息的索引文件，本质上是 B-
Tree（B 树），使用 B-Tree 作为索引指向 db-[number].log 里面存储的消息。

#### db.free

当前 db.data 文件里哪些页面是空闲的，文件具体内容是所有空闲页的 ID。

#### db.redo

用来进行消息恢复，如果 KahaDB 消息存储在强制退出后启动，用于恢复 BTree 索引。

#### lock

lock 文件锁，表示当前获得 kahadb 读写权限的 broker。

#### 四类文件+一把锁 ==> KahaDB

1.  **、了解 LevelDB**

这种文件系统是从 ActiveMQ 5.8 之后引进的，它和 KahaDB 非常相似，也是基于文件的本地数据库存储形式，但是它提供比 KahaDB 更快的持久性。
但它不使用自定义 B-Tree 实现来索引预写日志，而是使用基于 LevelDB 的索引。
题外话：为什么 LeavelDB 更快，并且 5.8 以后就支持，为什么还是默认 KahaDB 引擎，因为 activemq
官网本身没有定论，LeavelDB 之后又出了可复制的 LeavelDB 比 LeavelDB 更性能更优越，但需要基于
ZooKeeper 所以这些官方还没有定论，仍旧使用 KahaDB。

默认配置如下：

1. <persistenceAdapter>
1. <levelDB directory="activemq-data">
1. </persistenceAdapter>

## 、配置 JDBC

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835009412-8d766d7e-d547-47b2-8126-d68e1a558278.jpeg#)
MQ + MySQL 模型

ActiveMQ 配置 JDBC 持久化操作
官网：[https://dns.xuexi.icu/](https://dns.xuexi.icu/) [https://activemq.apache.org/persistence](https://activemq.apache.org/persistence)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835009880-8cf96453-9835-4108-8cec-810191857e06.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835010346-d7127f8b-2b29-411c-82bb-a3673be2bb27.png#)1、添加 Mysql 数据库的驱动包到 lib 文件夹

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835010974-3d99e3d8-dcbd-41c5-9b10-bdd1f269957f.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835011518-efe97dfb-fbb3-4b56-bc49-efc101fd732d.jpeg#)2、修改 activemq.xml 配置文件

1.  <persistenceAdapter>
1.      <jdbcPersistenceAdapter dataSource="#mysql-ds" createTablesOnStartup="true"/>
1.  </persistenceAdapter>

3、上面指向了 mysql-ds 的数据源，所以我们需要配置一个数据源！

1.  <bean id="mysql-ds" class="org.apache.commons.dbcp2.BasicDataSource" destroy- method="close">
1.  <property name="driverClassName" value="com.mysql.jdbc.Driver" />
1.      <property name="url" value="jdbc:mysql://192.168.0.106:3306/activemq? relaxAutoCommit=true" />
1.  <property name="username" value="root" />
1.  <property name="password" value="123456" />
1.  <property name="maxTotal" value="200" />
1.  <property name="poolPreparedStatements" value="true" />
1.  </bean>

注意 bean 存放的位置，不要放错了

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835011908-6a1db9f7-a420-499d-8949-8980aaebbd58.png#)

4、创建一个 activemq 的数据库

| 1   | -- 创建数据库                                                          |
| --- | ---------------------------------------------------------------------- |
| 2   | CREATE DATABASE `activemq` CHARACTER SET utf8 COLLATE utf8_general_ci; |

ActiveMQ 重新启动后会自动在 mysql 的 activemq 数据库下创建三张表：activemq_msgs 、
activemq_acks、activemq_lock
activemq_acks：用于存储订阅关系。如果是持久化 Topic，订阅者和服务器的订阅关系在这个表保 存
activemq_lock：在集群环境中才有用，只有一个 Broker 可以获得消息，称为 Master Broker
activemq_msgs：用于存储消息，Queue 和 Topic 都存储在这个表中
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835012399-139025b8-1d2f-4f8f-9d23-fc44f2929ef9.png#)

## 、测试 JDBC

队列
1、新建一个 JDBC 包

2、新建一个队列生产者测试类，运行后查看数据库

1 package com.kuang.activemq.jdbc; 2
3 import org.apache.activemq.ActiveMQConnectionFactory; 4
5 import javax.jms._; 6
7 /\*\*
8 _ @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com) 9 \*/
10 public class JmsProduce { 11
12 public static final String ACTIVEMQ_URL = "tcp://127.0.0.1:61616"; 13
14 public static void main(String[] args) throws JMSException { 15

1.      ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
1.  Connection connection = factory.createConnection();
1.  connection.start(); 19
1.      Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
1.  Queue queue = session.createQueue("jdbc-queue01");
1.  MessageProducer producer = session.createProducer(queue); 23
1.  // 一定要开启持久化
1.  producer.setDeliveryMode(DeliveryMode.PERSISTENT); 26

27 for (int i = 1; i <= 3; i++) {

1.      TextMessage textMessage = session.createTextMessage("msg=>" + i);
1.  producer.send(textMessage); 30 }

31

1. producer.close();
1. session.close();
1. connection.close(); 35

36 System.out.println("消息发布到 MQ 完成");
37
38 }
39
40 }
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835013022-b10a95da-2e36-42d0-996a-3a70d8e71c0c.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835013426-63f6a27d-8c05-4e27-92b0-61a4ab3220c9.png#)

3、新建一个队列消费者测试类，运行后查看数据库

1 package com.kuang.activemq.jdbc; 2
3 import org.apache.activemq.ActiveMQConnectionFactory; 4
5 import javax.jms._; 6
7 /\*\*
8 _ @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com) 9 \*/
10 public class JmsConsumer { 11
12 public static final String ACTIVEMQ_URL = "tcp://127.0.0.1:61616"; 13

1.  public static void main(String[] args) throws JMSException {
1.      ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
1.  Connection connection = factory.createConnection();
1.  connection.start(); 18
1.      Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
1.  Queue queue = session.createQueue("jdbc-queue01");
1.  MessageConsumer consumer = session.createConsumer(queue); 22
1.  while (true){
1.  TextMessage receive = (TextMessage) consumer.receive(4000L);
1.  if (receive!=null){
1.  System.out.println("消息接收者收到消息:"+receive.getText());
    | 27 |

} |

} | }else {
break;
}
}

| // 9、关闭资源 consumer.close(); session.close(); connection.close(); |
| --------------------------------------------------------------------- | --- | --- | --- |
| 28                                                                    |     |     |     |
| 29                                                                    |     |     |     |
| 30                                                                    |     |     |     |
| 31                                                                    |     |     |     |
| 32                                                                    |     |     |     |
| 33                                                                    |     |     |     |
| 34                                                                    |     |     |     |
| 35                                                                    |     |     |     |
| 36                                                                    |     |     |     |
| 37                                                                    |     |     |     |
| 38                                                                    |     |     |     |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835014016-e10918df-3b5c-43d7-a821-c1272b79886d.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835014448-1b9003bf-e4b6-49ae-be02-07634cd585ef.png#)

#### 点到点队列小结：

在点对点类型中，当 DeliveryMode 设置为 NON_PERSISTENCE 时，消息被保存在内存中；
当 DeliveryMode 设置为 PERSISTENCE 时，消息被保存在 Broker 的相应的文件或者数据库中。而且点对点类型中消息一旦被 Consumer 消费就从 Broker 中删除

Topic 主题测试
1、编写消息生产者

| 1   | package com.kuang.activemq.jdbc;                      |
| --- | ----------------------------------------------------- |
| 2   |                                                       |
| 3   | import org.apache.activemq.ActiveMQConnectionFactory; |
| 4   |                                                       |
| 5   | import javax.jms.\*;                                  |

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
/\*\*

- @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com)
- 发布

\*/
public class JmsProduceTopic {
public static final String ACTIVEMQ_URL = "tcp://127.0.0.1:61616";

public static void main(String[] args) throws JMSException {
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
ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);
Connection connection = factory.createConnection(); Session session = connection.createSession(false,
Session.AUTO_ACKNOWLEDGE);
Topic topic = session.createTopic("topic-kuangshen"); MessageProducer producer = session.createProducer(topic);

// 一 定 要 设 置 完 持 久 化 在 连 接 connection producer.setDeliveryMode(DeliveryMode.PERSISTENT); connection.start();
29
30
31
32
33
34
35
36
37
38
for (int i = 1; i <= 3; i++) {
TextMessage textMessage = session.createTextMessage("topic-
msg=>" + i);
producer.send(textMessage);
}

producer.close(); session.close(); connection.close();

System.out.println("topic 消息发布到 MQ 完成");
}
}

2、编写消费者

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
package com.kuang.activemq.jdbc;
import org.apache.activemq.ActiveMQConnectionFactory;

import javax.jms.\*;
import java.io.IOException;

/\*\*

- @author 秦疆 [24736743@qq.com](mailto:24736743@qq.com)
- 订阅即消费者

\*/
public class JmsConsumerTopic {

public static final String ACTIVEMQ_URL = "tcp://127.0.0.1:61616";

public static void main(String[] args) throws JMSException, IOException
{
17 ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(ACTIVEMQ_URL);

18
19
20
21
22
23
Connection connection = factory.createConnection();

// 设 置 id， 好 比 你 的 微 信 id; connection.setClientID("qinjiang");
24
25
26
27
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
Topic topic = session.createTopic("topic-kuangshen");
// 创建主题持久用户(参数二：订阅者)
TopicSubscriber topicSubscriber = session.createDurableSubscriber(topic, "狂神说");
28
29
30
31
32
33
34
35
36
connection.start(); // 连接
37
38
39
40
41
42
43
// 消费消息
Message message = topicSubscriber.receive(); while (message!=null){
TextMessage textMessage = (TextMessage) message;
System.out.println("收到的持久化 topic："+textMessage.getText());
//message = topicSubscriber.receive(5000L); //持久化订阅的时间，不写
就永久订阅！
message = topicSubscriber.receive();
}

session.close(); connection.close();
}
}

3、启动测试，先启动消费者，在启动生产者，一定要订阅成功才可以！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835015009-1507d93b-8965-4c13-9f0d-3ccde61ebed7.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835015614-aae91678-b96b-4149-9946-38ba3a1090c2.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835016031-b0dd2de9-92c7-4c52-80e2-49e010ec8fac.png#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835016256-a2d021d2-8412-4e9d-b48d-c951443dc672.png#)

小结

#### Queue

在没有消费者消费的情况下会将消息保存到 activemq_msgs 表中，只要有任意一个消费者已经消费过了，消费之后这些消息将会立即被删除。

#### Topic

一般是先启动消费订阅然后再启动生产，消息会被保留在 activemq_msgs 表中，订阅关系在 acks 表中。

#### 注意

1、数据库的 jar 包和数据库连接池的 jar 包一定要导入 lic 目录下。
2、在 createTableOnStartup 属性设置了 true 的，在第一次生成完表之后，需要改为 ﬂase；
3、操作系统机器名不能存在 ，否则会报错：BeanFactory not initialized or already closed；
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835016551-41071883-4931-4f76-9649-3a42657f1991.png#)

-

## 、Journal

说明
这种方式克服了 JDBC Store 的不足，JDBC 每次消息过来，都需要去写库和读库。ActiveMQ Journal，使用高速缓存写入技术，大大提高了性能。

当消费者的消费速度能够及时跟上生产者消息的生产速度时，Journal 文件能够大大减少需要写入到 DB 中的消息。
举个例子，生产者生产了 1000 条消息，这 1000 条消息会保存到 journal 文件，如果消费者的消费速度很快的情况下，在 journal 文件还没有同步到 DB 之前，消费者已经消费了 90%以上的信息，那么这个时候只需要同步剩余的 10%的信息到 DB。如果消费者的消费速度很慢，这个时候 Journal 文件可以使消息以批量方式写到 DB。

配置
修改 activemq.xml 配置文件
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835017090-9929b4fb-4e8f-4e00-bb09-7d5c169844d6.jpeg#)
配置完成后，重新启动 activemq 就可以了！

| 1 | <persistenceFactory>
<journalPersistenceAdapterFactory

</persistenceFactory> |

| journalLogFiles="4" journalLogFileSize="32768" useJournal="true" useQuickJournal="true" dataSource="#mysql-ds" dataDirectory="activemq-data" /> |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | --- | --- |
| 2                                                                                                                                               |     |     |
| 3                                                                                                                                               |     |     |
| 4                                                                                                                                               |     |     |
| 5                                                                                                                                               |     |     |
| 6                                                                                                                                               |     |     |
| 7                                                                                                                                               |     |     |
| 8                                                                                                                                               |     |     |
| 9                                                                                                                                               |     |     |

小结

#### 持久化消息主要是指：

MQ 所在的服务器 down 了消息不会丢失的机制；

#### 持久化机制演化过程：

从最初的 AMQ Message Store 方案到 ActiveMQ V4 版本中推出的 High performance journal（高性能事务支持）附件，并且同步推出了关于关系型数据库的存储方案。ActiveMQ 5.3 版本中又推出了对
KahaDB 的支持（V5.4 版本后称为 ActiveMQ 默认的持久化方案），后来 ActiveMQ V5.8 版本支持 LevelDB，到现在，V5.9 + 版本提供了标准的 Zookeeper + LevelDB 集群化方案。我们重点介绍了
KahaDB、LevelDB 和 mysql 数据库这三种持久化存储方案。

#### ActiveMQ 的消息持久化机制有：

AMQ 基于日志文件
KahaDB 基于日志文件，从 ActiveMQ 5.4 开始默认的持久化插件

JDBC 基于第三方数据库
LevelDB 基于文件的本地数据库存储，从 ActiveMQ5.8 版本之后又推出了 LevelDB 的持久化引擎，性能高于 KahaDB。
Replicated LevelDB Store 从 ActiveMQ5.9 提供了基于 LevelDB 和 Zookeeper 的数据复制方式，用于
Master-slave 方式的首选数据复制方案。

#### 无论使用哪种持久化方式，消息的存储逻辑都是一致的：

就是在发送者将消息发送出去后，消息中心首先将消息存储到本地数据文件、内存数据库。或者远程数 据库等，然后试图将消息发送给接收者，发送成功则将消息从存储中删除，失败则继续尝试。消息中心 启动以后首先要检查指定的存储位置。如果有未发送成功的消息，则需要把消息发送出去。

# 6、多节点集群

问题：引入消息队列之后该如何保证其高可用性？
答案：基于 Zookeeper 和 LevelDB 搭建 ActiveMQ 集群，集群仅提供主备方式的高可用集群功能，避免单 点故障。

## 6.1、三种集群方式对比

官网地址：[http://activemq.apache.org/masterslave.html](http://activemq.apache.org/masterslave.html)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835017444-80208f2d-02a0-4682-b715-60b73ccbe831.jpeg#)
1、基于 sharedFileSystem 共享文件系统（KahaDB 默认）
2、基于 JDBC
3、基于可复制的 LevelDB

#### 说明：

5.6 版本之后推出了 LevelDB 的持久化引擎，它使用了自定义的索引代替常用的 Btree 索引，其持久化性能高于 KahaDB，虽然默认的持久化方式还是 KahaDB，但是 LevelDB 可能会是趋势。
在 5.9 版本还提供了基于 LevelDB 和 Zookeeper 的数据复制方式，作为 Master-Slave 方式的首选数据复制方案。

## 6.2、搭建集群测试

说明

从 ActiveMQ 5.9 开始，ActiveMQ 的集群实现方式取消了传统的 Master-Slave 方式。
增加了基于 Zookeeper + LevelDB 的 Master-Slave 实现方式，从 5.9 版本后也是官网的推荐。
基于 Zookeeper 和 LevelDB 搭建 ActiveMQ 集群。集群仅提供主备方式的高可用集群功能，避免单点故障。
**官网集群原理图**：[http://activemq.apache.org/replicated-leveldb-store](http://activemq.apache.org/replicated-leveldb-store)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835017691-5a4c24f7-5fe6-479b-8f5c-35898ba2c2e5.jpeg#)
.

#### 原理说明：

使用 Zookeeper 集群注册所有的 ActiveMQ Broker，但只有其中的一个 Broker 可以提供服务，它将被视为 Master。其他的 Broker 处于待机状态被视为 Slave。
如果 Master 因故障而不能提供服务，Zookeeper 会从 Slave 中选举出一个 Broker 充当 Master。
Slave 连接 Master 并同步他们的存储状态，Slave 不接受客户端连接。所有的存储操作都会被复制到连接至 Master 的 Slaves。
如果 Master 宕机，得到了最新更新的 Slave 会成为 Master。故障节点在恢复后会重新加入到集群中并连接 Master 进入 Slaves 模式。
所有需要同步的消息操作，都将等待存储状态被复制到其他法定节点的操作完成才能完成。
所以，如果你配置了 replicas=3，那么法定大小是（3/2）+1 = 2。 Master 将会存储并更新然后等待
（2-1）= 1 个 Slave 存储和更新完成，才汇报 success。至于为什么是 2-1，Zookeeper 讲解过自行复习。
有一个 node 要作为观察者存在。当一个新的 Master 被选中，你需要至少保障一个法定 node 在线以能够找到拥有最新状态的 node。这个 node 才可以成为新的 Master 。
因此，推荐运行至少 3 个 replica nodes 以防止一个 node 失败后服务中断。

搭建 Zookeeper 集群

| **--** | **服务端口** | **集群通信端口** |
| ------ | ------------ | ---------------- |
| zk1    | 2181         | 9000:7000        |
| zk2    | 2182         | 9001:7001        |
| zk3    | 2183         | 9002:7002        |

1、Zookeeper 下载：[http://archive.apache.org/dist/zookeeper/zookeeper-3.5.7/](http://archive.apache.org/dist/zookeeper/zookeeper-3.5.7/)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835018151-c8bf4157-0ab1-46f8-a927-e664dd3025ac.png#)2、解压 zk 包，进入 zookeeper-3.5.7\conf 目录，修改 zoo_sample.cfg 文件为 zoo.cfg.如果没有特殊需 求，不需要修改配置文件，直接使用默认配置文件即可.

#### 各个参数的意义：

：这个时间是作为 Zookeeper 服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个 tickTime 时间就会发送一个心跳 。
tickTime
：顾名思义就是 Zookeeper 保存数据的目录，默认情况下，Zookeeper 将写数据的日志文
dataDir
件也保存在这个目录里.
：这个端口就是客户端连接 Zookeeper 服务器的端口，Zookeeper 会监听这个端口， 接受客户端的访问请求。
clientPort
: 集群中的 follower 服务器(F)与 leader 服务器(L)之间初始连接时能容忍的最多心跳数
initLimit
（tickTime 的数量）
: 集群中的 follower 服务器与 leader 服务器之间请求和应答之间能容忍的最多心跳数
syncLimit
（tickTime 的数量）。
启动 zookeeper，windows 下执行 bin >zkServer.cmd；linux 下 ookeeper-3.4.6\bin >zkServer.sh start
启动，确保不报错！

3、在 zookeeper1 的目录下新建一个 data 文件夹，里面新建一个没有后缀的 myid 文件，写入数字 1
4、修改 zookeeper1 的配置文件。增加集群配置

| 1   | # The number of milliseconds of each tick |
| --- | ----------------------------------------- |
| 2   | tickTime=2000                             |
| 3   | # The number of ticks that the initial    |
| 4   | # synchronization phase can take          |
| 5   | initLimit=10                              |

| 6   | # The number of ticks that can pass between                                                 |
| --- | ------------------------------------------------------------------------------------------- |
| 7   | # sending a request and getting an acknowledgement                                          |
| 8   | syncLimit=5                                                                                 |
| 9   | # the directory where the snapshot is stored.                                               |
| 10  | # do not use /tmp for storage, /tmp here is just                                            |
| 11  | # example sakes.                                                                            |
| 12  | # 数据目录切换到自己定义的的目录                                                            |
| 13  | dataDir=D:/Environment/zookeeper/apache-zookeeper-1/data                                    |
| 14  | # the port at which the clients will connect                                                |
| 15  | clientPort=2181                                                                             |
| 16  | # 设置集群配置，格式： server.A = B:C:D                                                     |
| 17  | # A:是一个数字，表示第几号服务器                                                            |
| 18  | # B:服务器 IP 地址                                                                          |
| 19  | # C:是一个端口号，用来集群成员的信息交换,表示这个服务器与集群中的 leader 服务器交换信息的端 |
|     | 口                                                                                          |
| 20  | # D:是在 leader 挂掉时专门用来进行选举 leader 所用的端口                                    |
| 21  |                                                                                             |
| 22  | server.1=127.0.0.1:9000:7000                                                                |
| 23  | server.2=127.0.0.1:9001:7001                                                                |
| 24  | server.3=127.0.0.1:9002:7002                                                                |

5、将 zookeeper1 复制 3 份，改名为 zookeeper2、zookeeper3
6、修改每个目录下的 data 文件中的 myid 文件，分别为 2， 3
7、修改其与两个 zookeeper 节点的配置文件，主要是端口号，data 目录，以及集群配置，同上！

| 1   | # 数据目录切换到自己定义的的目录                         |
| --- | -------------------------------------------------------- |
| 2   | dataDir=D:/Environment/zookeeper/apache-zookeeper-2/data |
| 3   | clientPort=2182                                          |
| 4   | server.1=127.0.0.1:9000:7000                             |
| 5   | server.2=127.0.0.1:9001:7001                             |
| 6   | server.3=127.0.0.1:9002:7002                             |

| 1   | # 数据目录切换到自己定义的的目录                         |
| --- | -------------------------------------------------------- |
| 2   | dataDir=D:/Environment/zookeeper/apache-zookeeper-3/data |
| 3   | clientPort=2183                                          |
| 4   | server.1=127.0.0.1:9000:7000                             |
| 5   | server.2=127.0.0.1:9001:7001                             |
| 6   | server.3=127.0.0.1:9002:7002                             |

8、启动顺序为 server1、server2、server3。在启动 server1，server2 时 zk 会报错，当所有节点全部启动时错误会消失
9、可以通过 Zk 提供的简易客户端来进行验证，双击下 bin/zkCli.cmd 来启动 Zk 简易客户端，然后通过使
用 命名（列出 Zk 指定节点下的所有子节点）来验证 Zk 已经启动完成。
ls /
10、为了方便启动，我们还可以编写一个 bat 脚本文件一次性启动

| 1   | start | apache-zookeeper-1\bin\zkServer.cmd |
| --- | ----- | ----------------------------------- |
| 2   | start | apache-zookeeper-2\bin\zkServer.cmd |
| 3   | start | apache-zookeeper-3\bin\zkServer.cmd |

搭建 activemq 集群
配置参考官方：[http://activemq.apache.org/replicated-leveldb-store](http://activemq.apache.org/replicated-leveldb-store)

| **--** | **服务端口** | **jetty 控制台端口** |
| ------ | ------------ | -------------------- |
| node1  | 61616        | 8161                 |
| node2  | 61617        | 8162                 |
| node3  | 61618        | 8163                 |

1、下载并解压 ActiveMQ，修改文件名为 apache-activemq-1，复制 3 份，1 2 3
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835018515-784a02af-7054-4673-8f5c-25f8988ea7e9.png#)2、修改 3 个节点的配置，保证 BrokerName 的名称一致，默认是 localhost。
3、3 个节点的持久化配置、然后端口需要修改 61616 端口为 61617 和 61618
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835018822-e84a45b1-a157-4f14-9cd1-bd10af26e385.png#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835019169-6f0e3f85-e1d9-49ca-b7e4-2e6369369803.png#)
1
2
3
4
5
6
<persistenceAdapter>
<replicatedLevelDB
directory="${activemq.data}/leveldb" replicas="3"
bind="tcp://0.0.0.0:0"
zkAddress="127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183"

1. hostname="broker-ms"
1. zkPath="/activemq/leveldb-stores"
1. sync="local_disk"

10 />
11 </persistenceAdapter>

4、jetty 控制台端口号修改，修改 jetty.xml 中的端口配置
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835019431-cd411d57-0a47-4c71-889f-bbd5265a7740.png#)

5、为了方便启动，我们可以编写一个启动脚本来启动 3 个 activemq

| 1   | start | apache-activemq-1\bin\activemq.bat | start |
| --- | ----- | ---------------------------------- | ----- |
| 2   | start | apache-activemq-2\bin\activemq.bat | start |
| 3   | start | apache-activemq-3\bin\activemq.bat | start |

启动测试集群
1、启动 zk 集群 2、启动 ActiveMQ 集群
3、进入 zk 客户端，查看

| 1   | ls /                                      |
| --- | ----------------------------------------- |
| 2   | = [activemq, zookeeper]                   |
| 3   | ls /activemq/leveldb-stores               |
| 4   | = [00000000000, 00000000001, 00000000002] |
| 5   | get /activemq/leveldb-stores/00000000000  |
| 6   | get /activemq/leveldb-stores/00000000001  |
| 7   | get /activemq/leveldb-stores/00000000002  |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835019716-2da61186-f9fd-4291-b789-dde66444af2e.png#)
4、连接测试发现，只有 Master 节点的 ActiveMQ 才能连接通，其与两个结点无法通过

#### 说明：elected 不为 null 就是 Master 节点

代码执行测试
一定要先启动集群环境
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835020208-dd4d0554-e7e1-45aa-b169-b64cf7594eb7.jpeg#)

#### 注意：URL 一定要改为 支持故障转移传输的 URL

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835020438-a1c52049-74e2-49d8-895a-61aca2fa06e2.png#)1、消息生产者
2、消息消费者

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625835020612-ef815234-b789-4a69-a1b4-a8bb7909ceab.png#)
3、停止一个 activemq 的主节点，它会自动切换到另一个或者的节点

说明及总结
ActiveMQ 的客户端只能访问 Master 的 Broker，其他处于 Slave 的 Broker 不能访问，所以客户端连接的
Broker 应该使用 failover 协议，即故障转移协议；
当一个 ActiveMQ 节点挂掉或者一个 Zookeeper 节点挂掉，ActiveMQ 服务依然正常运转，如果仅剩一个
ActiveMQ 节点，由于不能选举 Master，所以 ActiveMQ 不能正常运行；
同样的，如果 Zookeeper 仅剩一个节点活动，不管 ActiveMQ 各节点存活，ActiveMQ 也不能正常提供服 务。
ActiveMQ 集群的高可用依赖于 Zookeeper 集群的高可用。

# 7、提高

常见面试题：引入消息队列之后该如何保证其高可用性
zookeeper + replicated-leveldb-store 的主从集群；

## 、异步投递

说明
官网：[http://activemq.apache.org/async-sends](http://activemq.apache.org/async-sends)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835020974-6b66ccd9-0079-49be-8794-0631b7fde42a.jpeg#)
对于一个 Slow Consumer，使用同步发送消息可能出现 Producer 堵塞等情况，慢消费者适合使用异步发送。

什么是异步投递

ActiveMQ 支持同步，异步两种发送的模式将消息发送到 broker，模式的选择对发送延时有巨大的影响。producer 能达到怎样的产出率 （产出率=发送数据总量/时间）主要受发送延时的影响，使用异步发送可以显著的提高发送的性能。
ActiveMQ 默认使用异步发送的模式：除非明确指定使用同步发送的方式或者在未使用事务的前提下发送持久化的消息，这两种情况都是同步发送的。
如果你**没有使用事务且发送的是持久化的消息**，每一次发送都是同步发送的且会阻塞 producer ，直到 broker 返回一个确认，表示消息已经被安全的持久化到磁盘。确认机制提供了消息安全的保障。但同时会阻塞客户端，带来了很大的延时。
很多高性能的应用，允许在失败的情况下有少量的数据丢失。如果你的应用满足这个特点，你可以使用 异步发送来提高生产率，即使发送的是持久化的消息。
异步发送
它可以最大化 producer 端的发送效率。我们通常在发送消息量比较密集的情况下使用异步发送，它可以很大的提升 Producer 性能；不过这也带来了额外的问题。就是需要消耗较多的 Client 端内存同时也会导致 broker 端性能消耗增加；
此外它不能有效的确保消息的发送成功。在失的可能。
useAsyncSend=true
的情况下，客户端需要容忍消息丢

如何配置
1、使用连接 URI 配置异步发送

1 cf = new ActiveMQConnectionFactory("tcp://locahost:61616? jms.useAsyncSend=true");

2、在 ConnectionFactory 级别配置异步发送

1 ((ActiveMQConnectionFactory)connectionFactory).setUseAsyncSend(true);

3、在连接级别配置异步发送

1 ((ActiveMQConnection)connection).setUseAsyncSend(true);

面试题：异步发送如何确认发送成功？
异步发送丢失消息的场景是：生产者设置 UseAsyncSend=true，使用 producer.send(msg) 持续发送消息。
由于消息不阻塞，生产者会认为所有 send 的消息均被发送至 MQ。
如果 MQ 突然宕机，此时生产者端内存中尚未被发送至 MQ 的消息都会丢失。所以，正确的异步发送方法是需要接收回调的
同步发送和异步发送的区别就在于此，同步发送等 send 不阻塞了就表示一定发送成功了，异步发送需要接收回执并由客户端再判断一次是否发送成功。

代码测试说明：

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
// 修改 MessageProducer 为 ActiveMQMessageProducer 细粒度操作
ActiveMQMessageProducer producer = (ActiveMQMessageProducer) session.createProducer(queue); producer.setDeliveryMode(DeliveryMode.NON_PERSISTENT);
for (int i = 1; i <= 3; i++) {
final TextMessage textMessage = session.createTextMessage("msg=>" + i);
// 给消息设置一个编号 textMessage.setJMSMessageID(UUID.randomUUID().toString()+"<= myid"); String jmsMessageID = textMessage.getJMSMessageID();
// 通过生产者发布，发送的时候带上异步回调
producer.send(textMessage, new AsyncCallback() { public void onSuccess() {
System.out.println(jmsMessageID + "发送 OK");
}
public void onException(JMSException e) { System.out.println(jmsMessageID + "发送失败");
}
});
}

## 、定时延时投递

说明
官网：[http://activemq.apache.org/delay-and-schedule-message-delivery.html](http://activemq.apache.org/delay-and-schedule-message-delivery.html)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835021455-397d3a5b-b8fe-4211-8ec9-734f9c88c2d9.jpeg#)

#### 需要修改 xml 配置:将 broker schedulerSupport 属性设置为 true

为了增强 Java JMS 客户端的性能，在调度的属性名称的接口。
org.apache.activemq.ScheduledMessage
例如，要安排在 60 秒内发送邮件-您需要设置*AMQ_SCHEDULED_DELAY*属性：

上有一个带有用于

| 1   | MessageProducer producer = session.createProducer(destination);      |
| --- | -------------------------------------------------------------------- |
| 2   | TextMessage message = session.createTextMessage("test msg");         |
| 3   | long time = 60 \* 1000;                                              |
| 4   | message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_DELAY, time); |
| 5   | producer.send(message);                                              |

您可以将消息设置为等待初始延迟，然后重复发送 10 次，每次重新发送之间等待 10 秒：

| 1   | MessageProducer producer = session.createProducer(destination);         |
| --- | ----------------------------------------------------------------------- |
| 2   | TextMessage message = session.createTextMessage("test msg");            |
| 3   | long delay = 30 \* 1000;                                                |
| 4   | long period = 10 \* 1000;                                               |
| 5   | int repeat = 9;                                                         |
| 6   | message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_DELAY, delay);   |
| 7   | message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_PERIOD, period); |
| 8   | message.setIntProperty(ScheduledMessage.AMQ_SCHEDULED_REPEAT, repeat);  |
| 9   | producer.send(message);                                                 |

您还可以使用[CRON](http://en.wikipedia.org/wiki/Cron)安排邮件的时间，例如，如果希望将邮件安排为每小时发送一次，则需要将 CRON 条
目设置为 --例如
0 \* \* \* \*

| 1   | MessageProducer producer = session.createProducer(destination);                  |
| --- | -------------------------------------------------------------------------------- |
| 2   | TextMessage message = session.createTextMessage("test msg");                     |
| 3   | message.setStringProperty(ScheduledMessage.AMQ_SCHEDULED_CRON, "0 \* \* \* \*"); |
| 4   | producer.send(message);                                                          |

CRON 调度优先于使用消息延迟-但是，如果为 CRON 条目设置了重复和延时，则 ActiveMQ 调度程序将在 每次 CRON 条目触发时调度消息的传递。用一个例子更容易解释。假设您希望传递一条消息 10 次，每条 消息之间有一秒钟的延迟-并且您希望每小时发生一次-您可以这样做：

| 1   | MessageProducer producer = session.createProducer(destination);                  |
| --- | -------------------------------------------------------------------------------- |
| 2   | TextMessage message = session.createTextMessage("test msg");                     |
| 3   | message.setStringProperty(ScheduledMessage.AMQ_SCHEDULED_CRON, "0 \* \* \* \*"); |
| 4   | message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_DELAY, 1000);             |
| 5   | message.setLongProperty(ScheduledMessage.AMQ_SCHEDULED_PERIOD, 1000);            |
| 6   | message.setIntProperty(ScheduledMessage.AMQ_SCHEDULED_REPEAT, 9);                |
| 7   | producer.send(message);                                                          |

## 、消息重发

常见面试题

#### 具体哪些情况会引起消息重发？

1、Client 用了 transactions 且在 session 中调用了 rollback()
2、Client 用了 transactions 且在 调用 commit() 之前关闭或没有 commit() 3、Client 在 Client_Ackonwledge 的传递模式下，在 session 中调用了 recover() **请说说消息重发时间间隔和重发次数吗？**
间隔：1， 次数 ：6

#### 有毒消息 Poison Ack 谈谈你的理解？

ActiveMQConnection
一个消息被 redelivedred 超过默认的最大重发次数（默认 6 次）时，消费端会给 MQ 发送一个 “ poison
Ack” 表示这个消息有毒，告诉 broker 不要再发了。这个时候 broker 会把这个消息放到 DLQ （死信队列）

官网说明
官网：[http://activemq.apache.org/redelivery-policy](http://activemq.apache.org/redelivery-policy)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835021859-87455993-b990-4f04-a59d-ded561c22497.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835022486-d62e4163-f8d6-4395-b6e2-33996e602a6a.jpeg#)属性说明：

代码测试
1、生成者正常发送消息
2、消费者需要开启提交事务，但是不要 commit(); 则会产生重复接收问题。6 次之后则接收不到消息；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835022813-5970debc-7d22-41b1-bce8-3b3dae6cb8b5.jpeg#)

自定义修改消息重发策略
从 ActiveMQ v5.7.0 开始，您现在可以基于每个目标配置[RedeliveryPolicy](http://svn.apache.org/viewvc/activemq/trunk/activemq-client/src/main/java/org/apache/activemq/RedeliveryPolicy.java?view=markup)。 现在， 工厂类公开了[RedeliveryPolicyMap](http://svn.apache.org/viewvc/activemq/trunk/activemq-client/src/main/java/org/apache/activemq/broker/region/policy/RedeliveryPolicyMap.java?view=markup)属性，该属性允许使用命名目标或目标通配符来分配 RedeliveryPolicy。下面的代码片段显示了如何为主题和队列配置不同的[RedeliveryPolicy](http://svn.apache.org/viewvc/activemq/trunk/activemq-client/src/main/java/org/apache/activemq/RedeliveryPolicy.java?view=markup)。

| 1   | ActiveMQConnection | connection | ... | //  | Create | a   | connection |
| --- | ------------------ | ---------- | --- | --- | ------ | --- | ---------- |

| 2   |                                                                |
| --- | -------------------------------------------------------------- |
| 3   | RedeliveryPolicy queuePolicy = new RedeliveryPolicy();         |
| 4   | queuePolicy.setInitialRedeliveryDelay(0);                      |
| 5   | queuePolicy.setRedeliveryDelay(1000);                          |
| 6   | queuePolicy.setUseExponentialBackOff(false);                   |
| 7   | queuePolicy.setMaximumRedeliveries(2);                         |
| 8   |                                                                |
| 9   | RedeliveryPolicy topicPolicy = new RedeliveryPolicy();         |
| 10  | topicPolicy.setInitialRedeliveryDelay(0);                      |
| 11  | topicPolicy.setRedeliveryDelay(1000);                          |
| 12  | topicPolicy.setUseExponentialBackOff(false);                   |
| 13  | topicPolicy.setMaximumRedeliveries(3);                         |
| 14  |                                                                |
| 15  | // Receive a message with the JMS API                          |
| 16  | RedeliveryPolicyMap map = connection.getRedeliveryPolicyMap(); |
| 17  | map.put(new ActiveMQTopic(">"), topicPolicy);                  |
| 18  | map.put(new ActiveMQQueue(">"), queuePolicy);                  |

## 、死信队列

什么是死信队列
ActiveMQ 中引入了 “死信队列” （Dead Letter Queue）的概念。
即一条消息在被重发了多次后（默认是 6 次），将会被 ActiveMQ 移入 “死信队列”。开发人员可以在这个
Queue 中查看处理出错的消息，进行人工干预；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835023082-45e2d10e-21e5-460b-ae5f-90e4df8e397c.jpeg#)

#### 死信队列就是用来处理失败的消息

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625835023424-849b565d-71c8-4050-9504-581ca2bc4f1c.jpeg#)
一般生产环境中，在使用 MQ 的时候设计两个队列：一个是核心业务队列，一个是死信队列；
核心业务队列，就是比如上图专门用来让订单系统发送订单消息的，然后另外一个死信队列就是用 来处理异常情况的。

假如第三方物流系统故障了，此时无法请求。那么仓储系统每次消费到一条订单消息，尝试通知发 货和配送都会遇到对方的接口报错。此时仓储系统就可以把这条消息拒绝访问或者标志为处理失 败。一旦标志这条消息处理失败了后，MQ 就会把这条消息转入提前设置好的一个死信队列中。然 后你会看到的就是，在第三方物流系统故障期间，所有订单消息全部处理失败，全部会转入死信队 列。然后你的仓储系统得专门有一个后台线程，监控第三方物流系统是否正常。能否请求的，不停 的监视。一旦发现对方恢复正常，这个后台线程就从死信队列消费出来处理失败的订单，重新执行 发货和配送的通知逻辑。

死信队列的配置介绍

#### SharedDeadLetterStrategy

将所有的 DeadLetter 保存在一个共享的队列中，这就是 ActiveMQ Broker 端默认的策略。共享队列默认为 “ActiveMQ.DLQ”，可以通过 “deadLetterQueue” 属性来设定。

1. <deadLetterStrategy>
1. <sharedDeadLetterStrategy deadLetterQueue="DLQ-QUEUE"/>
1. </deadLetterStrategy>

#### IndividualDeadLetterStrategy

把 DeadLetter 放入各自的死信通道中，
对于 Queue 而言，死信通道的前缀默认为 “ActiveMQ.DLQ.Queue.” ； 对于 Topic 而言，死信通道的前缀默认为 “ActiveMQ.DLQ.Topic.” ；
比如队列 Order，那么它对应的死信通道为 “ActiveMQ.DLQ.Queue.Order”。我们使用 “queuePreﬁx” “topicPreﬁx” 来指定上述前缀。
默认情况下，无论是 Topic 还是 Queue，broker 将使用 Queue 来保存 DeadLeader，即死信通道常常为 Queue，不过开发者也可以指定为 Topic。

1.  <policyEntry queue="order">
1.  <deadLetterStartegy>
1.      <individualDeadLetterStrategy queuePrefix="DLQ." useQueueForQueueMessages="false"/>
1.  </deadLetterStrategy>
1.  </policyEntry>

将队列 Order 中出现的 DeadLetter 保存在 DLQ.Order，不过此时 DLQ.Order 为 Topic。
属性 “useQueueForTopicMessages”，此值表示是否将 Topic 的 DeadLetter 保存在 Queue 中，默认为
true；
