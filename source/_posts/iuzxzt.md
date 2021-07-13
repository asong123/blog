---
title: 消息队列总结
urlname: iuzxzt
date: '2021-07-12 14:16:41 +0800'
author: 阿松
top: true
hide: false
cover: false
toc: false
mathjax: false
summary: 消息队列总结
categories: java
tags:
  - java
  - 后端
  - 消息队列
abbrlink: 64131
---

# 一、rocketmq

## 1.1 RocketMQ 特点

> 灵活可扩展性

RocketMQ 天然支持集群，其核心四组件（Name Server、Broker、Producer、Consumer）每一个都可以在没有单点故障的情况下进行水平扩展。

> 海量消息堆积能力

RocketMQ 采用零拷贝原理实现超大的消息的堆积能力，据说单机已可以支持亿级消息堆积，而且在堆积了这么多消息后依然保持写入低延迟。

> 支持顺序消息

可以保证消息消费者按照消息发送的顺序对消息进行消费。顺序消息分为全局有序和局部有序，一般推荐使用局部有序，即生产者通过将某一类消息按顺序发送至同一个队列来实现。

> 多种消息过滤方式

消息过滤分为在服务器端过滤和在消费端过滤。服务器端过滤时可以按照消息消费者的要求做过滤，优点是减少不必要消息传输，缺点是增加了消息服务器的负担，实现相对复杂。消费端过滤则完全由具体应用自定义实现，这种方式更加灵活，缺点是很多无用的消息会传输给消息消费者。

> 支持事务消息

RocketMQ 除了支持普通消息，顺序消息之外还支持事务消息，这个特性对于分布式事务来说提供了又一种解决思路。

> 回溯消费

回溯消费是指消费者已经消费成功的消息，由于业务上需求需要重新消费，RocketMQ 支持按照时间回溯消费，时间维度精确到毫秒，可以向前回溯，也可以向后回溯。

## 1.2 RocketMQ 的架构

![image.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1598606502022-6611f0f1-b252-461d-afeb-bb71a526d48e.png#height=336&id=xwFqu&margin=%5Bobject%20Object%5D&name=image.png&originHeight=431&originWidth=957&originalType=binary∶=1&size=68931&status=done&style=none&width=746)

RocketMQ 核心的四大组件：Name Server、Broker、Producer、Consumer ，每个组件都可以部署成集群模式进行水平扩展。

## 1.3 RocketMQ 基本概念

### 1.3.1 生产者

生产者（Producer）负责产生消息，生产者向消息服务器发送由业务应用程序系统生成的消息。 RocketMQ 提供了三种方式发送消息：同步、异步和单向。

#### 1.3.1.1 同步发送

同步发送指消息发送方发出数据后会在收到接收方发回响应之后才发下一个数据包。一般用于重要通知消息，例如重要通知邮件、营销短信。

#### 1.3.1.2 异步发送

异步发送指发送方发出数据后，不等接收方发回响应，接着发送下个数据包，一般用于可能链路耗时较长而对响应时间敏感的业务场景，例如用户视频上传后通知启动转码服务。

#### 1.3.1.3 单向发送

单向发送是指只负责发送消息而不等待服务器回应且没有回调函数触发，适用于某些耗时非常短但对可靠性要求并不高的场景，例如日志收集。
​

#### 1.3.1.4 生产者组

生产者组（Producer Group）是一类 Producer 的集合，这类 Producer 通常发送一类消息并且发送逻辑一致，所以将这些 Producer 分组在一起。从部署结构上看生产者通过 Producer Group 的名字来标记自己是一个集群。
​

### 1.3.2 消费者

消费者（Consumer）负责消费消息，消费者从消息服务器拉取信息并将其输入用户应用程序。站在用户应用的角度消费者有两种类型：拉取型消费者、推送型消费者。

#### 1.3.2.1 拉取型消费者

拉取型消费者（Pull Consumer）主动从消息服务器拉取信息，只要批量拉取到消息，用户应用就会启动消费过程，所以 Pull 称为主动消费型。

#### 1.3.2.2 推送型消费者

推送型消费者（Push Consumer）封装了消息的拉取、消费进度和其他的内部维护工作，将消息到达时执行的回调接口留给用户应用程序来实现。所以 Push 称为被动消费类型，但从实现上看还是从消息服务器中拉取消息，不同于 Pull 的是 Push 首先要注册消费监听器，当监听器处触发后才开始消费消息。

#### 1.3.2.3 消费者组

消费者组（Consumer Group）一类 Consumer 的集合名称，这类 Consumer 通常消费同一类消息并且消费逻辑一致，所以将这些 Consumer 分组在一起。消费者组与生产者组类似，都是将相同角色的分组在一起并命名，分组是个很精妙的概念设计，RocketMQ 正是通过这种分组机制，实现了天然的消息负载均衡。消费消息时通过 Consumer Group 实现了将消息分发到多个消费者服务器实例，比如某个 Topic 有 9 条消息，其中一个 Consumer Group 有 3 个实例（3 个进程或 3 台机器），那么每个实例将均摊 3 条消息，这也意味着我们可以很方便的通过加机器来实现水平扩展。
​

### 1.3.3 消息服务器（broker）

消息服务器（Broker）是消息存储中心，主要作用是接收来自 Producer 的消息并存储， Consumer 从这里取得消息。它还存储与消息相关的元数据，包括用户组、消费进度偏移量、队列信息等。从部署结构图中可以看出 Broker 有 Master 和 Slave 两种类型，Master 既可以写又可以读，Slave 不可以写只可以读。从物理结构上看 Broker 的集群部署方式有四种：单 Master 、多 Master 、多 Master 多 Slave（同步刷盘）、多 Master 多 Slave（异步刷盘）。

#### 1.3.3.1 单 Master

这种方式一旦 Broker 重启或宕机会导致整个服务不可用，这种方式风险较大，所以显然不建议线上环境使用。

#### 1.3.3.2 多 Master

所有消息服务器都是 Master ，没有 Slave 。这种方式优点是配置简单，单个 Master 宕机或重启维护对应用无影响。缺点是单台机器宕机期间，该机器上未被消费的消息在机器恢复之前不可订阅，消息实时性会受影响。

#### 1.3.3.3 多 Master 多 Slave（异步复制）

每个 Master 配置一个 Slave，所以有多对 Master-Slave，消息采用异步复制方式，主备之间有毫秒级消息延迟。这种方式优点是消息丢失的非常少，且消息实时性不会受影响，Master 宕机后消费者可以继续从 Slave 消费，中间的过程对用户应用程序透明，不需要人工干预，性能同多 Master 方式几乎一样。缺点是 Master 宕机时在磁盘损坏情况下会丢失极少量消息。

#### 1.3.3.4 多 Master 多 Slave（同步双写）

每个 Master 配置一个 Slave，所以有多对 Master-Slave ，消息采用同步双写方式，主备都写成功才返回成功。这种方式优点是数据与服务都没有单点问题，Master 宕机时消息无延迟，服务与数据的可用性非常高。缺点是性能相对异步复制方式略低，发送消息的延迟会略高。

### 1.3.4 名称服务器(NameServer)

名称服务器（NameServer）用来保存 Broker 相关元信息并给 Producer 和 Consumer 查找 Broker 信息。NameServer 被设计成几乎无状态的，可以横向扩展，节点之间相互之间无通信，通过部署多台机器来标记自己是一个伪集群。每个 Broker 在启动的时候会到 NameServer 注册，Producer 在发送消息前会根据 Topic 到 NameServer 获取到 Broker 的路由信息，Consumer 也会定时获取 Topic 的路由信息。所以从功能上看应该是和 ZooKeeper 差不多，据说 RocketMQ 的早期版本确实是使用的 ZooKeeper ，后来改为了自己实现的 NameServer

### 1.3.5 消息

消息（Message）就是要传输的信息。一条消息必须有一个主题（Topic），主题可以看做是你的信件要邮寄的地址。一条消息也可以拥有一个可选的标签（Tag）和额处的键值对，它们可以用于设置一个业务 key 并在 Broker 上查找此消息以便在开发期间查找问题。

#### 1.3.5.1 主题

主题（Topic）可以看做消息的规类，它是消息的第一级类型。比如一个电商系统可以分为：交易消息、物流消息等，一条消息必须有一个 Topic 。Topic 与生产者和消费者的关系非常松散，一个 Topic 可以有 0 个、1 个、多个生产者向其发送消息，一个生产者也可以同时向不同的 Topic 发送消息。一个 Topic 也可以被 0 个、1 个、多个消费者订阅。

#### 1.3.5.1 标签

标签（Tag）可以看作子主题，它是消息的第二级类型，用于为用户提供额外的灵活性。使用标签，同一业务模块不同目的的消息就可以用相同 Topic 而不同的 Tag 来标识。比如交易消息又可以分为：交易创建消息、交易完成消息等，一条消息可以没有 Tag 。标签有助于保持您的代码干净和连贯，并且还可以为 RocketMQ 提供的查询系统提供帮助。

#### 1.3.5.1 消息队列

消息队列（Message Queue），主题被划分为一个或多个子主题，即消息队列。一个 Topic 下可以设置多个消息队列，发送消息时执行该消息的 Topic ，RocketMQ 会轮询该 Topic 下的所有队列将消息发出去。下图 Broker 内部消息情况：
![image.jpeg](https://cdn.nlark.com/yuque/0/2020/jpeg/635741/1597734128477-83e3b220-0dae-4916-946f-5c0914499f59.jpeg#height=325&id=mP1GO&name=image.jpeg&originHeight=325&originWidth=580&originalType=binary∶=1&size=80818&status=done&style=none&width=580)

#### 1.3.5.1 消息消费模式

消息消费模式有两种：集群消费（Clustering）和广播消费（Broadcasting）。默认情况下就是集群消费，该模式下一个消费者集群共同消费一个主题的多个队列，一个队列只会被一个消费者消费，如果某个消费者挂掉，分组内其它消费者会接替挂掉的消费者继续消费。而广播消费消息会发给消费者组中的每一个消费者进行消费。

#### 1.3.5.1 消息顺序

消息顺序（Message Order）有两种：顺序消费（Orderly）和并行消费（Concurrently）。顺序消费表示消息消费的顺序同生产者为每个消息队列发送的顺序一致，所以如果正在处理全局顺序是强制性的场景，需要确保使用的主题只有一个消息队列。并行消费不再保证消息顺序，消费的最大并行数量受每个消费者客户端指定的线程池限制。

# 二、kafka

## 2.1 kafka 的体系架构

![clipboard.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1597736842152-ed127301-0f47-462d-9f98-7ebf19df6e06.png#height=424&id=hXEaR&margin=%5Bobject%20Object%5D&name=clipboard.png&originHeight=424&originWidth=866&originalType=binary∶=1&size=54523&status=done&style=none&width=866)
![clipboard.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1597736868217-b4e63174-ffae-4fbb-9363-c8e9f8c2d3b2.png#height=325&id=FqnsG&margin=%5Bobject%20Object%5D&name=clipboard.png&originHeight=325&originWidth=587&originalType=binary∶=1&size=82825&status=done&style=none&width=587)
![clipboard.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1597736881365-c68db380-a43f-4637-a11e-132154a676be.png#height=236&id=kUryv&margin=%5Bobject%20Object%5D&name=clipboard.png&originHeight=236&originWidth=459&originalType=binary∶=1&size=20061&status=done&style=none&width=459)
![121221343268.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1597736900364-e1aaf4f6-8edb-424a-85f5-7a0e4caa560f.png#height=159&id=bG62H&margin=%5Bobject%20Object%5D&name=121221343268.png&originHeight=159&originWidth=477&originalType=binary∶=1&size=35652&status=done&style=none&width=477)

## 2.2 **kafka 在 zk 中的存储结构**

![clipboard.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1597736948896-d596fb18-2f7e-4248-bdcd-d55eb129a31c.png#height=745&id=Bv5a0&margin=%5Bobject%20Object%5D&name=clipboard.png&originHeight=745&originWidth=1322&originalType=binary∶=1&size=77702&status=done&style=none&width=1322)

## 2.3 名词解释 && 基本概念

**Broker：**kafka 实例节点，一般一台主机一个 broker
**Topic：**主题（逻辑存储），一个主题包含一个或者多个分区，如果想全局消息有序，那么最多只能有一个分区。
**Partition：**分区（实际存储），包含.index 索引文件和.log 实际数据存储文件，分区上按照游标有序消费。
**消费组：**一个消费组有多个消费者，一个消息只能被一个消费组中的一个消费者消费，所以消费组的消费者的数量应该小于主题分区的数量（最好成整数倍关系，比如分区是 4,消费组中的消费者是 2），因为同一分区下的数据在同一时刻不能被同一个消费组中的不同消费者消费，否则会造成不同消费者抢占同一分区消息，这样子就会涉及到加锁的问题，得不偿失。
**re-blance（重平衡）：**每个 consumer 只负责调整自己所消费的 partition，任何 broker 或者 consumer 的增减都会触发所有的 consumer 的 rebalance，也就是重新为消费组中的 consumer 分配负责的分区。
**AR：**所有的副本（replicas）统称为 Assigned Replicas，即 AR。AR=ISR+OSR
**ISR：**ISR (In-Sync Replicas)，这个是指副本同步队列，它是 AR 的一个子集。follower 从 leader 同步数据有一些延迟 replica.lag.time.max.ms，如果副本的延迟超过了这个参数，那么就会移出 ISR 队列，放进 OSR 队列。同时在 ISR 中还有一个 HW 的概念，HW 俗称高水位，HighWatermark 的缩写，取一个 partition 对应的 ISR 中最小的 LEO 作为 HW。类似于木桶短板效应，consumer 只能消费到 HW。leader 宕机的时候是从 ISR 从选取出来一个作为 leader 的。这样子就保证了数据的高可用。
**OSR：**OSR（Outof-Sync Replicas），新加入的 follower 和数据复制延迟超时的会先存放在 OSR 中,等数据同步到一定程度可以移进 ISR.
**HW：**HW 俗称高水位，HighWatermark 的缩写，取一个 partition 对应的 ISR 中最小的 LEO 作为 HW，consumer 最多只能消费到 HW 所在的位置。
![clipboard.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1597737274415-0ba117da-ca25-40e5-8fe4-880a0cc053b4.png#height=547&id=M9N12&margin=%5Bobject%20Object%5D&name=clipboard.png&originHeight=547&originWidth=1000&originalType=binary∶=1&size=320817&status=done&style=none&width=1000)

## 2.4 核心参数配置

acks 确认参数（数的值可以看成是需要多少个分区（leader+副本）写入）：
1）acks=0： 设置为 0 表示 producer 不需要等待任何确认收到的信息。副本将立即加到 socket  buffer 并认为已经发送。没有任何保障可以保证此种情况下 server 已经成功接收数据，同时重试配置不会发生作用（因为客户端不知道是否失败）回馈的 offset 会总是设置为-1；
2）acks=1： 这意味着至少要等待 leader 已经成功将数据写入本地 log，但是并没有等待所有 follower 是否成功写入。这种情况下，如果 follower 没有成功备份数据，而此时 leader 又挂掉，则消息会丢失。
3）acks=all： 这意味着 leader 需要等待所有备份都成功写入日志，这种策略会保证只要有一个备份存活就不会丢失数据。这是最强的保证。
4）其他的设置，例如 acks=2 也是可以的，这将需要给定的 acks 数量，但是这种策略一般很少用。

retries 重试次数
默认是 0，设置重试次数大于 0 可以让客户端发送数据失败的时候重新发送任何数据。

auto.commit.enable 自动确认
默认为 true，可以改为 false 进行手动提交，防止重复消费（提交丢失，也就是提交的时候 broker 挂掉了，但此时业务系统已经消费了消息）

## 2.5 如何保证消息不丢失不重复？

**消息丢失：**
1）生产者阶段丢失：往 Broker 写入消息时，发生网络错误，消息不可达。
**问题出现原因：**若设置了 ack=0 或者 retries=0
**解决办法：**设置 ack=1 或者设置 ack=all**，**设置 retries > 1 进行消息不断的重新发送，这可能导致消息重发发送，解决办法参考消息重复-解决办法 1

2）Broker 阶段丢失：
2.1）分区 leader 收到消息，同步分区复制到新消息之前，分区 leader 崩溃。
**问题出现原因：**若设置了 ack=1 或者 unclean.leader.election.enable=true
**解决办法：**1）设置 ack=all,并且设置 min.insync.replicas > 1
2）设置 unclean.leader.election.enable = false
注：1）unclean.leader.election.enable 代表是否允许 ISR 以外的副本作为 leader,会导致数据 丢失，默认为 false。
2）min.insync.replicas 为最小成功副本数

3）消费者：读取到一批消息，消费者提交了偏移量却未能处理读到的消息（如数据需要的算力很大，处理时间会很长），这是消费者丢消息的主要原因。
**解决办法：**关闭 kafka 自动提交，auto.commit.enable=false，进行手动的提交消息，待业务处理完成后再进行手动的提交 offset,但是这样子也可能会带来消息重复的问题，手动提交时 kafka 节点挂了，offset 提交失败，但是此时消息已经进行消费。

**消息重复：**
1）生产者：往 Broker 写入消息时超时了，进行了重复消息发送。
**解决办法 1：**启动 kafka 的幂等性，无需修改代码，默认为关闭，需要修改配置文件:enable.idempotence=true 同时要求 ack=all 且 retries>1
幂等原理：
　　每个 producer 有一个 producer id，服务端会通过这个 id 关联记录每个 producer 的状态，每个 producer 的每条消息会带上一个递增的 sequence，服务端会记录每个 producer 对应的当前最大 sequence，producerId + sequence ，如果新的消息带上的 sequence 不大于当前的最大 sequence 就拒绝这条消息，如果消息落盘会同时更新最大 sequence，这个时候重发的消息会被服务端拒掉从而避免消息重复。该配置同样应用于 kafka 事务中。

**解决办法 2：**ack=0，不重试（看场景，需要高可靠性的不建议）
可能会丢消息，适用于吞吐量指标重要性高于数据丢失，例如：日志收集。

2）消费者：
问题一：offset 未提交。
**原因 1：**强行 kill 线程，导致消费后的数据，offset 没有提交（消费系统宕机、重启等）
**原因 2：**出现 re-blance 重平衡时，此时有一定几率 offset 没提交，会导致重平衡后重复消费。 
**解决办法 1：**如果消息消费是写在事务型方法里面，进行事物回滚消费消息失败即可。
**解决办法 2：**允许重复消费，对重复消费做等幂的处理。具体怎么处理可以看业务，例如对于存储到 redis 上的数据可以进行直接的覆盖，对于存储到数据库的数据可以考虑数据库的唯一索引，给每一条消息设置一个消息 ID，相同的消息 ID 一样。

问题二：消费者重新分配 partition 的时候，可能出现从头开始消费的情况，导致重复问题。
解决办法：kafka.consumer.auto.offset.reset=latest

总结：
对于消息丢失处理：设置 ack=all，retries 进行无限次重试，并设置最小同步副本数 min.insync.replicas > 1，设置 unclean.leader.election.enable = false，设置手动的消息提交 auto.commit.enable=false。
对于消息重复的处理：设置 enable.idempotence=true 让 kafka 处理生产端的消息重复，设置 kafka.consumer.auto.offset.reset=latest 防止重新分配 partition 的时候出现从头消费，同时业务层对消费的重复消息进行处理。

## 2.6 Kafka 如何进行选举？

leader 在 zk 上创建一个临时节点，所有 Follower 对此节点注册监听，当 leader 宕机时，此时 ISR 里的所有 Follower 都尝试创建该节点，而创建成功者（Zookeeper 保证只有一个能创建成功）即是新的 Leader，其它 Replica 即为 Follower。
实际上的实现思路也是这样，只是优化了下，多了个代理控制管理类（controller）。**引入的原因是，当 kafka 集群业务很多，partition 达到成千上万时，当 broker 宕机时，造成集群内大量的调整，会造成大量 Watch 事件被触发，Zookeeper 负载会过重**。zk 是不适合大量写操作的。
![clipboard.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1597737682294-4726e2ec-5a41-4179-adad-35897e73f392.png#height=506&id=iLQFU&margin=%5Bobject%20Object%5D&name=clipboard.png&originHeight=506&originWidth=1148&originalType=binary∶=1&size=120809&status=done&style=none&width=1148)
Kafaka 动态维护了一个同步状态的副本的集合（a set of in-sync replicas），简称 ISR，在这个集合中的节点都是和 leader 保持高度一致的，任何一条消息必须被这个集合中的每个节点读取并追加到日志中了，才回通知外部这个消息已经被提交了。因此这个集合中的任何一个节点随时都可以被选为 leader。ISR 在 ZooKeeper 中维护。ISR 中有 f+1 个节点，就可以允许在 f 个节点 down 掉的情况下不会丢失消息并正常提供服。ISR 的成员是动态的，如果一个节点被淘汰了，当它重新达到“同步中”的状态时，他可以重新加入 ISR。因此如果 leader 宕了，直接从 ISR 中选择一个 follower 就行。
那么如果所有节点都 down 掉了怎么办？Kafka 对于数据不会丢失的保证，是基于至少一个节点是存活的，一旦所有节点都 down 了，这个就不能保证了。
实际应用中，当所有的副本都 down 掉时，必须及时作出反应。可以有以下两种选择:
1）等待 ISR 中的任何一个节点恢复并担任 leader。
2）选择所有节点中（不只是 ISR）第一个恢复的节点作为 leader。（可能导致消息丢失）
​

​

# 三、消息队列的选型

## 3.1 消息队列方案对比图

![clipboard.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1597737836122-88161f22-c2b0-422b-83f3-69891ead113a.png#height=854&id=d3Jbt&margin=%5Bobject%20Object%5D&name=clipboard.png&originHeight=854&originWidth=934&originalType=binary∶=1&size=337300&status=done&style=none&width=934)
![clipboard.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1597737849661-b0f85b48-4f68-477a-a2a4-999bf6aa2106.png#height=833&id=gHcIc&margin=%5Bobject%20Object%5D&name=clipboard.png&originHeight=833&originWidth=835&originalType=binary∶=1&size=417124&status=done&style=none&width=835)

## 3.2 为什么要使用 kafka 做消息通信？

主要是从吞吐量和可靠性，易用性三个方面考虑。对于单机的吞吐量 kafka 可以达到十万的级别而 activemq 是万级，可靠性方面 activemq 是主从架构，而 kafka 是分布式架构，可靠性方面更加好，kafka 理论上不会有数据丢失，会有数据重复，对于像 kafka 这种社区活跃度也比较高，所以最终会选择它做消息系统。
