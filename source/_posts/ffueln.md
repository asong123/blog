---
title: ZooKeeper整理
urlname: ffueln
date: '2021-07-12 14:31:49 +0800'
tags: []
categories: []
---

# 一、ZooKeeper 简介

ZooKeeper 一个开源的，用于分布式中一致性处理的框架。分布式应用程序可以基于 ZooKeeper 实现诸如数据发布/订阅、负载均衡、命名服务、分布式协调/通知、集群管理、Master 选举、配置维护、名字服务、分布式同步、分布式锁和分布式队列等功能。

- ZooKeeper 是一个拥有文件系统特点的数据库。
- ZooKeeper 是一个解决了数据一致性问题的分布式数据库。
- ZooKeeper 是一个具有发布和订阅功能的分布式数据库。

ZooKeeper 可以保证如下分布式一致性特性：

1. 顺序一致性

从同一个客户端发起的事务请求，最终将会严格按照其发起的顺序被应用到 ZooKeeper 中去。

2. 原子性

所有的事务请求的处理结果在整个集群中所有的机器上的应用情况是一致的，也就是说要么都应用，要么都没应用。

3. 单一视图

无论客户端连接的是哪个 ZooKeeper 服务器，其所看到的服务端的数据模型都是一致的。

4. 可靠性

一旦服务端成功的应用了一个事务请求，并完成对客户端的响应，那么该事务所引起的服务端状态的变化将会被一直保留下来，除非有另一个事务又对其进行了变更。

5. 实时性

ZooKeeper 仅仅保证在一定的时间内，客户端最终一定能够从服务端读取到最新的数据状态，而不是第一时间。

### 一致性

- 什么是一致性？

分布式系统中数据和状态保持同步和一致。如何保证在分布式环境下数据的最终一致，这个就是 ZooKeeper 需要解决的问题。

- 常见一致性问题解决方式
  - 查询重试补偿：对于分布式应用中不确定的情况，先使用查询接口查询到当前状态，如果当前状态不一致则采用补偿接口对状态进行重试推进，或者回滚接口对业务做回滚。
  - 定时任务推送：对于上面的情况，有可能一次推送搞不定，于是需要 2 次，3 次推送。
  - TCC（Try-Confirm-Cancel）：实际上就是**两阶段协议**，第二阶段的可以实现提交操作或是逆操作。

**ZooKeeper 保证是最终一致性！保证的 CP！**

# 二、ZooKeeper 基本概念

## 1、集群角色

在 ZooKeeper 中，有三个角色：Leader、Follower、Observer。

- Leader

在一个 ZooKeeper 集群中，同一个时刻只有一个 Leader，其他都是 Follower 或 Observer。Leader 作为整个 ZooKeeper 集群的主节点，负责响应所有对 ZooKeeper 状态变更的请求。它会将每个状态更新请求进行排序和编号，以便保证整个集群内部消息处理的 FIFO，写操作都走 Leader。

- Follower

Follower 除了响应本服务器上的读请求外，Follower 还要处理 Leader 的提议，并在 Leader 提交该提议时在本地也进行提交。 另外需要注意的是，Leader 和 Follower 构成 ZooKeeper 集群的法定人数，也就是说，只有他们才参与新 Leader 的选举、响应 Leader 的提议。

- Observer

如果 ZooKeeper 集群的读取负载很高，或者客户端多到跨机房，可以设置一些 Observer 服务器，以提高读取的吞吐量。Observer 和 Follower 比较相似，只有一些小区别：首先 Observer 不属于法定人数，即不参加选举也不响应提议；其次是 Observer 不需要将事务持久化到磁盘，一旦 Observer 被重启，需要从 Leader 重新同步整个名字空间。

## 2、节点读写服务分工

- ZooKeeper 集群的所有机器通过一个 Leader 选举过程来选定一台被称为 Leader 的机器，Leader 服务器为客户端提供读和写服务。
- Follower 和 Observer 都能提供读服务，不能提供写服务。两者唯一的区别在于，Observer 机器不参与 Leader 选举过程，也不参与写操作的『过半写成功』策略，因此 Observer 可以在不影响写性能的情况下提升集群的读性能。
  > ZooKeeper 写数据时，经过二阶段提交，当 follower 节点过多时，写性能会有所下降！

## 3、Session

Session 是指客户端会话，在讲解客户端会话之前，我们先来了解下客户端连接。在 ZooKeeper 中，一个客户端连接是指客户端和 ZooKeeper 服务器之间的 TCP 长连接。
ZooKeeper 对外的服务端口默认是 2181，客户端启动时，首先会与服务器建立一个 TCP 连接，从第一次连接建立开始，客户端会话的生命周期也开始了，通过这个连接，客户端能够通过心跳检测和服务器保持有效的会话，也能够向 ZooKeeper 服务器发送请求并接受响应，同时还能通过该连接接收来自服务器的 Watch 事件通知。
Session 的 SessionTimeout 值用来设置一个客户端会话的超时时间。当由于服务器压力太大、网络故障或是客户端主动断开连接等各种原因导致客户端连接断开时，只要在 SessionTimeout 规定的时间内能够重新连接上集群中任意一台服务器，那么之前创建的会话仍然有效。

## 4、数据节点

ZooKeeper 的结构其实就是一个树形结构，Leader 就相当于其中的根结点，其它节点就相当于  Follower 节点，每个节点都保留自己的内容。
ZooKeeper 的节点分 2 大类、四种类型：持久、临时、持久有序、临时有序。

- 持久（PERSISTENT）节点：

所谓持久节点是指一旦这个树形结构上被创建了，除非主动进行对树节点的移除操作，否则这个节点将一直保存在 ZooKeeper 上。

- 持久顺序节点(PERSISTENT_SEQUENTIAL)：

ZooKeeper 在创建节点的过程中，会自动的为给定节点名加上一个数字后缀，作为一个新的完整的节点名，后缀的上限为整型的最大值。

- 临时（EPHEMERAL）节点：

临时节点的生命周期跟客户端会话绑定，一旦客户端会话失效，那么这个客户端创建的所有临时节点都会被移除。临时节点只能作为叶子节点，不能基于临时节点创建子节点。

- 临时有序节点(EPHEMERAL_SEQUENTIAL)

## 5、状态信息

ZooKeeper 的每个节点除了存储数据内容之外，还存储了节点本身的一些状态信息。用 get 命名可以同时获取某个节点的内容和状态信息。
在 ZooKeeper 中，version 属性是用来实现乐观锁机制中的『写入校验』（保证分布式数据原子性操作）。
![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimages-cdn.shimo.im%2FwaQfFoCnBcEJcHgU%2Fimage.png!thumbnail&sign=022d4188d146b94f5296ee502fc1c7a260cb94ae1b098ec898f02946752bf134#align=left&display=inline&height=432&id=ta2i9&margin=%5Bobject%20Object%5D&originHeight=432&originWidth=962&status=done&style=none&width=962)

Znode  结构：Stat：状态信息、版本、权限相关：

| 状态属性       | 说明                                                                                                                                  |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| czxid          | 节点创建时的 zxid                                                                                                                     |
| mzxid          | 节点最新一次更新发生时的 zxid                                                                                                         |
| ctime          | 节点创建时的时间戳                                                                                                                    |
| mtime          | 节点最新一次更新发生时的时间戳                                                                                                        |
| dataVersion    | 节点数据的更新次数                                                                                                                    |
| cversion       | 其子节点的更新次数                                                                                                                    |
| aclVersion     | 节点 ACL(授权信息)的更新次数                                                                                                          |
| ephemeralOwner | 如果该节点为 ephemeral 节点,  ephemeralOwner 值表示与该节点绑定的 session id.   如果该节点不是 ephemeral 节点,  ephemeralOwner 值为 0 |
| dataLength     | 节点数据的字节数                                                                                                                      |
| numChildren    | 子节点个数                                                                                                                            |

## 6、事务操作

在 ZooKeeper 中，能改变 ZooKeeper 服务器状态的操作称为事务操作。一般包括数据节点的创建与删除、数据内容的更新和客户端会话创建与失效等操作。对应每一个事务请求，ZooKeeper 都分配一个全局唯一的事务 ID，用 ZXID 表示，通常是一个 64 位的数字。每个 ZXID 对应一次更新操作，从这些 ZXID 中可以间接地识别出 ZooKeeper 处理这些事务操作请求的顺序。

## 7、Watcher（事件监听器）

Watcher 是 ZooKeeper 中一个很重要的特性。ZooKeeper 允许用户在指定节点上注册一些 Watcher，并且在一些特定事件触发的时候，ZooKeeper 服务端会将事件通知到感兴趣的客户端上去。该机制是 ZooKeeper 实现分布式协调服务的重要特性。

## 8、ZAB 协议

ZAB：ZooKeeper Atomic Broadcast，支持崩溃恢复的原子广播协议。
ZooKeeper 的核心是原子广播，这个机制保证了各个 server 之间的同步。实现这个机制的协议叫做 Zab 协议。Zab 协议有两种模式，它们分别是**恢复模式**和**广播模式**。当服务启动或者在 Leader 崩溃后，Zab 就进入了恢复模式，当 Leader 被选举出来，且过半数 Server 的完成了和 Leader 的状态同步以后，恢复模式就结束了。状态同步保证了 Leader 和 Server 具有相同的系统状态。一旦 Leader 已经和多数的 Follower 进行了状态同步后，他就可以开始广播消息了，即进入广播状态。这时候当一个 Server 加入 ZooKeeper 服务中，它会在恢复模式下启动，发现 Leader，并和 Leader 进行状态同步。待到同步结束，它也参与消息广播。
Zookeeper 服务一直维持在 Broadcast 状态，直到 Leader 崩溃了或者 Leader 失去了大部分的 Followers 支持。广播模式需要保证 proposal 被按顺序处理，因此 zk 采用了递增的事务 id 号(zxid)来保证。所有的提议(proposal)都在被提出的时候加上了 zxid。实现中 zxid 是一个 64 为的数字，它高 32 位是 epoch 用来标识 Leader 关系是否改变，每次一个 Leader 被选出来，它都会有一个新的 epoch。低 32 位是个递增计数。当 Leader 崩溃或者 Leader 失去大多数的 Follower，这时候 zk 进入恢复模式，恢复模式需要重新选举出一个新的 Leader，让所有的 server 都恢复到一个正确的状态。

- 实现了一种主备模式的系统架构来保持集群中各副本之间的数据一致性；
- ZK 使用**单一的主进程**来接收并处理客户端所有事务请求，并采用 ZAB 原子广播协议以事务 Proposal 的形式广播到所有的副本进程；
- 事务变更可能存在顺序依赖，ZAB 协议还能够**保证一个全局的变更序列顺序**的被应用；
- 主进程随时有可能出现崩溃退出或重启，因此 ZAB 协议还必须保证出现以上情况时，依旧能够正常工作。

# 三、ZooKeeper 特性

- 同一时刻多台机器创建同一个节点，只有一个会争抢成功。利用这个特性可以做分布式锁。
- 临时节点的生命周期与会话一致，会话关闭则临时节点删除。这个特性经常用来做心跳，动态监控，负载等动作。
- 顺序节点保证节点名全局唯一。这个特性可以用来生成分布式环境下的全局自增长 id。
- ZooKeeper 是一个由多个 server 组成的集群，一个 leader，多个 follower，每个 server 保存一份数据副本。分布式读 follower，写由 leader 实施；更新请求转发，由 leader 实施。
- 更新请求顺序进行，来自同一个 client 的更新请求按其发送顺序依次执行。
- 数据更新原子性，一次数据更新要么成功，要么失败。
- 全局数据一致，全局唯一数据视图，client 无论连接到哪个 server，数据视图都是一致的实时性，在一定事件范围内，client 能读到最新数据。

# 四、ZooKeeper 典型应用

ZooKeeper 是一个高可用的分布式数据管理与协调框架。基于对 ZAB 算法的实现，该框架能够很好地保证分布式环境中数据的一致性。也是基于这样的特性，使得 ZooKeeper 成为了解决分布式一致性问题的利器。

## 1、数据发布与订阅（配置中心）

数据发布与订阅，即所谓的配置中心，顾名思义就是发布者将数据发布到 ZooKeeper 节点上，供订阅者进行数据订阅，进而达到动态获取数据的目的，实现配置信息的集中式管理和动态更新。
对于数据量比较小、数据内容在运行时动态变化、集群中各机器共享，配置一致这样的全局配置信息就可以发布到 ZooKeeper 上，让客户端（集群的机器）去订阅该消息。
发布/订阅系统一般有两种设计模式，分别是推（Push）和拉（Pull）模式。

- 推模式：服务端主动将数据更新发送给所有订阅的客户端。
- 拉模式：客户端主动发起请求来获取最新数据，通常客户端都采用定时轮询拉取的方式。

ZooKeeper 采用的是推拉相结合的方式：客户端向服务端注册自己需要关注的节点，一旦该节点的数据发生变更，那么服务端就会向相应的客户端发送 Watcher 事件通知，客户端接收到这个消息通知后，**需要主动到服务端获取最新的数据**。

## 2、命名服务

命名服务也是分布式系统中比较常见的一类场景。在分布式系统中，通过使用命名服务，客户端应用能够根据指定名字来获取资源或服务的地址，提供者等信息。被命名的实体通常可以是集群中的机器，提供的服务，远程对象等等——这些我们都可以统称他们为名字。
其中较为常见的就是一些分布式服务框架（如 RPC）中的服务地址列表。通过在 ZooKeeper 里创建顺序节点，能够很容易创建一个全局唯一的路径，这个路径就可以作为一个名字。ZooKeeper 的命名服务即生成全局唯一的 ID。

## 3、分布式协调服务/通知

ZooKeeper 中特有 Watcher 注册与异步通知机制，能够很好的实现分布式环境下不同机器，甚至不同系统之间的通知与协调，从而实现对数据变更的实时处理。使用方法通常是不同的客户端如果机器节点发生了变化，那么所有订阅的客户端都能够接收到相应的 Watcher 通知，并做出相应的处理。
ZooKeeper 的分布式协调/通知，是一种通用的分布式系统机器间的通信方式。

## 4、Master 选举

Master 选举可以说是 ZooKeeper 最典型的应用场景了。比如 HDFS 中 Active NameNode 的选举、YARN 中 Active ResourceManager 的选举和 HBase 中 Active HMaster 的选举等。
针对 Master 选举的需求，通常情况下，我们可以选择常见的关系型数据库中的主键特性来
实现：希望成为 Master 的机器都向数据库中插入一条相同主键 ID 的记录，数据库会帮我们进行主键冲突检查，也就是说，只有一台机器能插入成功。那么，我们就认为向数据库中成功插入数据的客户端机器成为 Master。
依靠关系型数据库的主键特性确实能够很好地保证在集群中选举出唯一的一个 Master。但是，如果当前选举出的 Master 挂了，那么该如何处理？谁来告诉我 Master 挂了呢？显然，关系型数据库无法通知我们这个事件。但是，ZooKeeper 可以做到！
利用 ZooKeepr 的强一致性，能够很好地保证在分布式高并发情况下节点的创建一定能够保证全局唯一性，即 ZooKeeper 将会保证客户端无法创建一个已经存在的数据单元节点。也就是说，如果同时有多个客户端请求创建同一个临时节点，那么最终一定只有一个客户端请求能够创建成功。利用这个特性，就能很容易地在分布式环境中进行 Master 选举了。成功创建该节点的客户端所在的机器就成为了 Master。同时，其他没有成功创建该节点的客户端，都会在该节点上注册一个子节点变更的 Watcher，用于监控当前 Master 机器是否存活，一旦发现当前的 Master 挂了，那么其他客户端将会重新进行 Master 选举。这样就实现了 Master 的动态选举。

## 5、分布式锁

分布式锁是控制分布式系统之间同步访问共享资源的一种方式  ，分布式锁又分为排他锁和共享锁两种。ZK 节点不能重复，且 ZK 所有的写操作都是走 Leader 节点，为单线程操作，故 ZK 能用来实现分布式锁！

- 排它锁

ZooKeeper 如何实现排它锁？

- 定义锁  ：ZooKeeper 上的一个 机器节点可以表示一个锁。
- 获得锁

把 ZooKeeper 上的一个节点看作是一个锁，获得锁就通过创建临时节点的方式来实现。ZooKeeper 会保证在所有客户端中，最终只有一个客户端能够创建成功，那么就可以认为该客户端获得了锁。同时，所有没有获取到锁的客户端就需要到 /exclusive_lock  节点上注册一个子节点变更的 Watcher 监听，以便实时监听到 lock 节点的变更情况。

- 释放锁

因为锁是一个临时节点，释放锁有两种方式：

1.  当前获得锁的客户端机器发生宕机或重启，那么该临时节点就会被删除，释放锁；
1.  正常执行完业务逻辑后，客户端就会主动将自己创建的临时节点删除，释放锁。

无论在什么情况下移除了 lock 节点，ZooKeeper 都会通知所有在 /exclusive_lock 节点上注册了节点变更 Watcher 监听的客户端。这些客户端在接收到通知后，再次重新发起分布式锁获取，即重复『获取锁』过程。

- 共享锁

共享锁在同一个进程中很容易实现，但是在跨进程或者在不同 Server 之间就不好实现了。Zookeeper 却很容易实现这个功能，实现方式也是需要获得锁的 Server 创建一个 EPHEMERAL_SEQUENTIAL 目录节点，然后调用 getChildren 方法获取当前的目录节点列表中最小的目录节点是不是就是自己创建的目录节点，如果正是自己创建的，那么它就获得了这个锁，如果不是那么它就调用 exists(String path, boolean watch) 方法并监控 Zookeeper 上目录节点列表的变化，一直到自己创建的节点是列表中最小编号的目录节点，从而获得锁，释放锁很简单，只要删除前面它自己所创建的目录节点就行了。
使用 zookeeper 实现分布式锁的算法流程，假设锁空间的根节点为 /lock：

- 客户端连接 zookeeper，并在/lock 下创建  **临时的**  且  **有序的**  子节点，第一个客户端对应的子节点为 /lock/lock-0000000000，第二个为 /lock/lock-0000000001，以此类推。
- 客户端获取 /lock 下的子节点列表，判断自己创建的子节点是否为当前子节点列表中  **序号最小**的子节点，如果是则认为获得锁，~~否则监听/lock 的子节点变更消息~~， **监听刚好在自己之前一位的子节点删除消息** ，获得子节点变更通知后重复此步骤直至获得锁；
- 执行业务代码；
- 完成业务流程后，删除对应的子节点释放锁。
  > PS：
  > 1、临时节点能够保证在故障的情况下锁也能被释放
  > 2、zookeeper 提供的 API 中设置监听器的操作与读操作是原子执行的，也就是说在读子节点列表时同时设置监听器，保证不会丢失事件。

**ZooKeeper 在高并发下实现分布式锁，性能较低！ZK 集群时，所有的写操作都是要走 Leader，每次写操作 Leader 都要与 Follower 进行通信！**

## 6、分布式队列

队列方面，简单地讲有两种，一种是常规的先进先出队列，另一种是要等到队列成员聚齐之后的才统一按序执行。对于第一种先进先出队列，和分布式锁服务中的控制时序场景基本原理一致，这里不再赘述。
第二种队列其实是在 FIFO 队列的基础上作了一个增强。通常可以在 /queue 这个 znode 下预先建立一个 /queue/num 节点，并且赋值为 n（或者直接给/queue 赋值 n），表示队列大小，之后每次有队列成员加入后，就判断下是否已经到达队列大小，决定是否可以开始执行了。这种用法的典型场景是，分布式环境中，一个大任务 Task A，需要在很多子任务完成（或条件就绪）情况下才能进行。这个时候，凡是其中一个子任务完成（就绪），那么就去 /taskList 下建立自己的临时时序节点（CreateMode.EPHEMERAL_SEQUENTIAL），当 /taskList 发现自己下面的子节点满足指定个数，就可以进行下一步按序进行处理了。

# 五、ZooKeeper 部署

配置文件路径：%zk_home%/conf/，默认使用 zoo.cfg 文件。
假设有三台互相联网的 Linux 机器，它们的 IP 分别为 IP1，IP2，IP3。

```java
# ZK 最小时间单元，单位毫秒。很多运行时间间隔都是用 tickTime 的倍数来表示。
tickTime=2000
initLimit=10
syncLimit=5
# 服务器对外服务端口
clientPort=2181
# 快照存放路径
dataDir=/tmp/zookeeper/2181
# server.id=ip:port1:port2
# id: 对应 myid
# port1: follower与leader 数据同步端口
# port1: 选举时投票通信端口
server.1=IP1:2888:3888
server.2=IP2:2889:3889
server.3=IP3:2890:3890
```

server.id = host:port:port
其中 id 称为 Server ID，范围 1~255，用来标识机器在集群中的机器序号。同时在每台 ZK 机器上，都要在数据目录（dataDir: /tmp/zookeeper/2181）创建一个 myid 文件，该文件只包含一行内容，即 Server ID。

启动集群：

```
%zkhome%/bin/zkServer.sh start ../conf/zoo_2181.cfg
%zkhome%/bin/zkServer.sh start ../conf/zoo_2182.cfg
%zkhome%/bin/zkServer.sh start ../conf/zoo_2183.cfg
```

查看集群节点状态:
![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fuploader.shimo.im%2Ff%2FYaS6n5BGpHMUjdcy.png!thumbnail&sign=17efe0f18602fa1b4193812d4deb0b882f27d3fc0577925fed9c41a512556541#align=left&display=inline&height=294&id=OOWaA&margin=%5Bobject%20Object%5D&originHeight=294&originWidth=928&status=done&style=none&width=928)

# 六、ZooKeeper 客户端脚本

## 1、连接

```bash
$sh zkCli.sh
```

看到如下信息时，表示已经连接上 ZK 服务器：
![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimages-cdn.shimo.im%2FMHxcQBbrSEQCpG7u%2Fimage.png!thumbnail&sign=b446a226bc4cc40f8b2fad026c6a147a01bf7633d3734e066f96278260bbcd09#align=left&display=inline&height=70&id=JZjiU&margin=%5Bobject%20Object%5D&originHeight=70&originWidth=723&status=done&style=none&width=723)
注意：上面的命令没有显式的指定 ZK 服务器地址，默认连接本地的 ZK 服务器，如需指定服务器地址，使用如下命令：

```bash
$sh zkCli.sh -server ip：port
```

## 2、创建

使用 create 命令可以创建一个 ZK 节点，用法如下：

```bash
$sh create [-s] [-e] path data acl
```

参数说明：

- -s 或 -e 指定节点属性，顺序或者临时节点，默认情况下创建的是持久节点。
- acl：权限控制，缺省情况下不做任何控制。

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimages-cdn.shimo.im%2FddQcCwI6qsgXvR6S%2Fimage.png!thumbnail&sign=f97a2f589f32bc87d47f377490f73ebe5a5371de41babe46f9dd5afe481f0bde#align=left&display=inline&height=152&id=Niex0&margin=%5Bobject%20Object%5D&originHeight=152&originWidth=765&status=done&style=none&width=765)

## 3、读取

与读取相关的命令包含 ls 和 get。

- ls：列出指定节点下的第一级的所有子节点。

```bash
$sh ls path [watch]
```

- get：获取指定节点的数据内容和属性信息。

```bash
$sh get path [watch]
```

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimages-cdn.shimo.im%2FFdd3YnG4o1MzQD8v%2Fimage.png!thumbnail&sign=465fa9f7a241d140cbe54010a3a4f9f5ee4e7b44a6db9927c2906e518e8388bb#align=left&display=inline&height=314&id=z4Ujh&margin=%5Bobject%20Object%5D&originHeight=314&originWidth=672&status=done&style=none&width=672)
第一行是节点的数据内容
cZxid：该节点的事务 ID
mZxid：最后一次更新该节点的事务 ID

## 4、更新

使用 set 命令可以更新指定节点的数据内容，用法如下：

```bash
$sh set path data [version]
```

version：用于指定本次更新操作是基于 ZNode 的哪个数据版本进行的。

## 5、删除

使用 delete 命令可以删除指定节点（无法删除包含子节点的节点），用法如下：

```bash
$sh delete path [version]
```

# 七、ZooKeeper 选举

## 1、Leader 选举时机

- 集群规模至少 2 台机器；
- 服务器集群初始化；
- Leader 服务器宕机；
  > 集群初始化时，每个节点都是投给自己，因为 ZXID 是一样的，因此 myid（服务 id）最大的最终就称为了 Leader。
  > 宕机后的选举，则会先比较 ZXID

![image.png](https://cdn.nlark.com/yuque/0/2021/png/441009/1615346074849-54d39a85-8742-4f0a-8ed5-14d167d01617.png#align=left&display=inline&height=464&id=jOZSn&margin=%5Bobject%20Object%5D&name=image.png&originHeight=464&originWidth=840&size=385197&status=done&style=stroke&width=840)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/441009/1615346046385-0d82073a-0bde-4bc2-85a6-a8e20a0a9c80.png#align=left&display=inline&height=438&id=Xgodg&margin=%5Bobject%20Object%5D&name=image.png&originHeight=438&originWidth=862&size=302979&status=done&style=stroke&width=862)

## 2、投票

- 数据结构：

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fuploader.shimo.im%2Ff%2F85sWqL2WTD0X04ui.png!thumbnail&sign=cd1c5994bc1e21bbe80dd30433b88aa5f31b5ebbdc80e4efc476c3cc2b137bff#align=left&display=inline&height=225&id=UgWY1&margin=%5Bobject%20Object%5D&originHeight=225&originWidth=1034&status=done&style=none&width=1034)

- 投票：

集群初始化时，每个 Server 都会将自己作为 Leader 服务器来投票，每个服务器都会接收来自其他服务器的投票。

- 处理投票

服务器接收到其他服务器的投票后，会进行 PK，规则如下：

- 优先检查 ZXID。ZXID 比较大的服务器优先作为 Leader；
- 如果 ZXID 相同，那么就比较 myid，大的作为 Leader。

如果自己的票 PK 失败，服务器就会更新自己的投票，并重新将投票发出去。

# 八、ZooKeeper 开源客户端

## 1、Curator

Curator 是 Netflix 公司开源的一套 ZooKeeper 客户端框架，解决了很多 ZooKeeper 客户端非常底层的细节开发工作，包括连接重连、反复注册 Watcher 和等。
Curator 包含了几个包：

- **curator-framework：**对 ZooKeeper 的底层 api 的一些封装
- **curator-client：**提供一些客户端的操作，例如重试策略等
- **curator-recipes：**封装了一些高级特性，如：Cache 事件监听、选举、分布式锁、分布式计数器、分布式 Barrier 等。
  > Maven 依赖(使用 curator 的版本：2.12.0，对应 Zookeeper 的版本为：3.4.x，如果跨版本会有兼容性问题，很有可能导致节点操作失败)。

```xml
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>2.11.1</version>
</dependency>
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>2.11.1</version>
</dependency>
```

### 1.1 创建会话

```java
/**
 * 使用静态工程方法创建客户端.
 */
public CuratorFramework getClient() {
    return CuratorFrameworkFactory.newClient(ZK_ADDRESS, new RetryNTimes(10, 5000));
}
/**
 * 使用 Fluent 风格的 Api 创建会话
 */
public CuratorFramework getClientByFluent() {
    RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000, 3);
    return CuratorFrameworkFactory.builder()
            .connectString(ZK_ADDRESS)
            .sessionTimeoutMs(5000)
            .connectionTimeoutMs(5000)
            .retryPolicy(retryPolicy)
            .build();
}
```

newClient 静态工厂方法包含四个主要参数：

| 参数名              | 说明                                                                                                                                                                                                |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| connectionString    | 服务器列表，格式 host1:port1,host2:port2,...                                                                                                                                                        |
| retryPolicy         | 重试策略,内建有四种重试策略,也可以自行实现 RetryPolicy 接口  ExponentialBackoffRetry：衰减重试  RetryNTimes：指定最大重试次数  RetryOneTime：仅重试一次  RetryUnitilElapsed：一直重试直到规定的时间 |
| sessionTimeoutMs    | 会话超时时间，单位毫秒，默认 60000ms                                                                                                                                                                |
| connectionTimeoutMs | 连接创建超时时间，单位毫秒，默认 60000ms                                                                                                                                                            |

> 为了实现 ZK 的业务线隔离，往往会为每个业务线分配一个独立的命名空间，即指定一个 ZK 的根路径。Curator 也支持创建含命名空间的 Client，后续所有的节点操作都是基于该相对目录进行的。

```java
public CuratorFramework getClientByFluent() {
    RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000, 3);
    return CuratorFrameworkFactory.builder()
            .connectString(ZK_ADDRESS)
            .sessionTimeoutMs(5000)
            .connectionTimeoutMs(5000)
            .retryPolicy(retryPolicy)
            .namespace("base")
            .build();
```

### 1.2 CRUD 操作

- 获取节点数据

```java
// 读取一个节点的数据内容
byte[] data = CLIENT.getData().forPath(path);
// 读取一个节点的数据内容,并获取 Stat
Stat stat = new Stat();
data = CLIENT.getData().storingStatIn(stat).forPath(path);
```

- 查询节点

```java
// 节点不存在返回 null
Stat stat =  CLIENT.checkExists().forPath(path);
```

- 更新节点

```java
// 更新节点数据
CLIENT.setData().forPath(ZK_PATH, bytes);
// 更新节点的数据内容,强制指定版本进行更新
CLIENT.setData().withVersion(stat.getVersion()).forPath(ZK_PATH, bytes);
```

- 创建节点

Curator 默认创建的是持久节点，内容默认为空。

```java
// 创建一个初始内容为空的节点
CLIENT.create().forPath(path);
// 创建一个初始内容不为空的节点
CLIENT.create().forPath(path,bytes);
// 创建一个初始内容为空的临时节点
// CreateMode: PERSISTENT、PERSISTENT_SEQUENTIAL、
//             EPHEMERAL、EPHEMERAL_SEQUENTIAL
CLIENT.create().withMode(CreateMode.EPHEMERAL).forPath(path);
// 递归创建节点
CLIENT.create().creatingParentsIfNeeded().withMode(CreateMode.EPHEMERAL).forPath("/path/path2",bytes);
// 异步创建节点
// 注意:如果自己指定了线程池,那么相应的操作就会在线程池中执行,如果没有指定,
// 那么就会使用 Zookeeper 的 EventThread 线程对事件进行串行处理
CLIENT.create().inBackground((client1, event) -> {
    // TODO callback function
}, Executors.newFixedThreadPool(10)).forPath("/path", bytes);
```

- 删除节点

```java
// 删除一个叶子节点
CLIENT.delete().forPath(path);
// 删除一个节点并递归删除子节点
CLIENT.delete().deletingChildrenIfNeeded().forPath(path);
// 删除指定版本的节点
CLIENT.delete().withVersion(1).forPath(path);
// 删除一个节点:保证删除(只要客户端会话有效 guaranteed 会持续进行删除,直至删除成功)
CLIENT.delete().guaranteed().forPath(path);
```

### 1.3 事件监听

Curator 的监听采用 3 种 Watcher 机制来监听节点变化：

- Node Cache：监视一个数据节点本身的变化，并将节点数据更新至本地；

```java
/**
 * 监听节点数据的变化
 * NodeCache 构造器说明：
 * clien：curator 客户端实例
 * path：要监听的数据节点的路径
 * dataIsCompressed：是否进行数据压缩，默认为false
 */
public static void testNodeCache() throws Exception {
    String path = "/watch";
    final NodeCache nodeCache = new NodeCache(CLIENT, path, false);
    // nodeCache = new NodeCache(CLIENT, path);
    nodeCache.start();
    nodeCache.getListenable().addListener(() -> {
        System.out.println("Node Data change...");
        System.out.println("New Data:" + Arrays.toString(nodeCache.getCurrentData().getData()));
    });
}
```

- Path Cache：监视一个路径下子节点的建立，删除，以及节点数据更新，使用的事件监听类：PathChildrenCacheListener；（无法监听二级及以下子节点）

```java
/**
 * 监听子节点数据变化
 * PathChildrenCache 构造函数参数说明：
 * client：curator 客户端实例
 * path：要监听的数据节点的路径
 * cacheData：是否缓存数据
 * dataIsCompressed：数据是否压缩
 * threadFactory： 构造线程池
 * executorService： 构造线程池
 */
public static void testPathChildrenCache() throws Exception  {
    String path = "/pathChildrenCache";
    PathChildrenCache cache = new PathChildrenCache(CLIENT, path, true);
    cache.start();
    cache.getListenable().addListener((client, event) -> {
        // 新增子节点(CHILD_ADD) 子节点数据更新(CHILD_UPDATED)
        // 子节点删除(CHILD_REMOVED)
        System.out.println("事件类型:" + event.getType());
    });
}
```

- Tree Cache：Path cache 和 Node Cache 的结合体，监视路径下创建，更新，删除事件，并缓存路径下所有孩子节点的数据。

### 1.4 Master 选举

Master 选举：
分布式系统中，对于一个业务，仅需从集群中选举一台机器进行处理即可，类似的问题统称为 Master 选举。
ZooKeeper 实现 Master 的大体思路如下：
选择一个根节点，例如 /master_select，多台机器同时向该节点创建一个子节点 /master_select/lock，利用 ZooKeeper 的特性，只有一台能够创建成功，成功的那台机器就称为了 Master。

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fuploader.shimo.im%2Ff%2F7xN9EmAiRaoD8XMs.png!thumbnail&sign=ad4116c80413e227ab218eb5aa6f33633681a90678ee764d9204b7e5fc86c23f#align=left&display=inline&height=462&id=JNiaF&margin=%5Bobject%20Object%5D&originHeight=462&originWidth=1509&status=done&style=none&width=1509)

```java
public static void testMasterSelect() throws InterruptedException {
    // 实例化 Selector 需要一个监听器:LeaderSelectorListenerAdapter
    LeaderSelector selector = new LeaderSelector(CLIENT, MASTER_PATH, new LeaderSelectorListenerAdapter() {
        @Override
        public void takeLeadership(CuratorFramework client) throws Exception {
            System.out.println("成为 Master 角色!");
            // doSomeThing
            Thread.sleep(10000);
            System.out.println("完成 Master 操作,释放 Master 权利!");
        }
    });
    selector.autoRequeue();
    selector.start();
    Thread.sleep(Integer.MAX_VALUE);
}
```

### 1.5 分布式锁

```java
/**
 * 分布式锁
 */
public static void testLock(){
    final InterProcessMutex lock = new InterProcessMutex(CLIENT,LOCK_PATH);
    final CountDownLatch latch = new CountDownLatch(1);
    for(int i = 0;i < 30;i++){
        new Thread(new Runnable() {
            @Override
            public void run() {
                try{
                    latch.await();
                    lock.acquire();
                }catch (Exception e){
                    System.out.println("Error");
                }
                SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss||SSS");
                String orderNo = sdf.format(new Date());
                System.out.println("生成的订单号:" + orderNo);
                try {
                    lock.release();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
    latch.countDown();
}
```

# 九、ZooKeeper 技术内幕

## 1、通信协议

ZooKeeper 基于 TCP/IP 协议，实现了自己的通信协议，对于请求主要包含请求头、请求体，对于响应主要包含响应头、响应体。
![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fuploader.shimo.im%2Ff%2F3JW6yL8sico2DEY8.png!thumbnail&sign=a6ea0fbacc9cccc37b8cfb22f17bbb2b1a04603b592b3ab2bf8c3ab7a14276e4#align=left&display=inline&height=124&id=vZ284&margin=%5Bobject%20Object%5D&originHeight=124&originWidth=382&status=done&style=none&width=382)

- 请求头

根据协议规定除非是“会话创建”请求，其他所有的客户端请求都会带上请求头。

```java
public class RequestHeader implements Record {
    // 记录请求发起的先后序号
    private int xid;
    // 请求的操作类型 OpCode
    private int type;
}
```

- 请求体

请求的主体内容，包含了请求的所有的操作内容，不同的请求类型，请求体的结构不同。

- ConnectRequest：会话创建

```java
public class ConnectRequest implements Record {
  private int protocolVersion;
  private long lastZxidSeen;
  private int timeOut;
  private long sessionId;
  private byte[] passwd;
}
```

- GetDataRequest：获取节点数据

```java
public class GetDataRequest implements Record {
  private String path;
  private boolean watch;
}
```

- SetDataRequest：更新节点数据

```java
public class SetDataRequest implements Record {
  private String path;
  private byte[] data;
  private int version;
}
```

- 响应头

```java
public class ReplyHeader implements Record {
  // 与请求的 xid 一致
  private int xid;
  // 事务ID
  private long zxid;
  private int err;
}
```

- 响应体

协议的响应体是指响应的主体部分，包含了响应的所有数据。不同的响应类型，响应体的结构有所不同。

## 2、数据结构

- DataNode

```java
public class DataNode implements Record {
    DataNode parent;
    byte data[];
    Long acl;
    public StatPersisted stat;
    private Set<String> children = null;
    private static final Set<String> EMPTY_SET=Collections.emptySet();
}
```

- DataTree

```java
public class DataTree {
    private final ConcurrentHashMap<String, DataNode> nodes =
        new ConcurrentHashMap<String, DataNode>();
    // 临时节点
    private final Map<Long, HashSet<String>> ephemerals =
    new ConcurrentHashMap<Long, HashSet<String>>();
    private final WatchManager dataWatches = new WatchManager();
    private final WatchManager childWatches = new WatchManager();
}
```

- ZKDatabase

```java
public class ZKDatabase {
    protected DataTree dataTree;
    protected ConcurrentHashMap<Long, Integer> sessionsWithTimeouts;
    protected FileTxnSnapLog snapLog;
    protected long minCommittedLog, maxCommittedLog;
    public static final int commitLogCount = 500;
    protected static int commitLogBuffer = 700;
    protected LinkedList<Proposal> committedLog = new LinkedList<Proposal>();
    protected ReentrantReadWriteLock logLock = new ReentrantReadWriteLock();
    volatile private boolean initialized = false;
}
```

## 3、Watcher

- watcher 机制概述

客户端向服务端注册 Watcher 的同时，会将 Watcher 对象存储在客户端的 WatcherManager 中。当服务端触发 Watcher 事件时，会向客户端发送通知，客户端线程从 WatcherManager 中取出对应的 Watcher 对象来执行回调逻辑。
![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fuploader.shimo.im%2Ff%2FP9NSdoGwFF0yERg5.png!thumbnail&sign=be7867766bc84adbd7742245763314f2346072c29dcc8540319a055e8be53fd4#align=left&display=inline&height=296&id=Wqwvz&margin=%5Bobject%20Object%5D&originHeight=296&originWidth=980&status=done&style=none&width=980)

- 相关接口
  - Watcher 接口

客户端回调处理逻辑接口，需实现 process() 回调方法。
![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fuploader.shimo.im%2Ff%2FlRphq0Lrqsk4T6aN.png!thumbnail&sign=74a6c084331832b2d8ebb3fdedcb29f178f1c760be3eb69f34029d03ac647c3f#align=left&display=inline&height=190&id=WSUfR&margin=%5Bobject%20Object%5D&originHeight=190&originWidth=456&status=done&style=none&width=456)
![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fuploader.shimo.im%2Ff%2Fl3oCyE4G6EUDyifD.png!thumbnail&sign=7c29e33be6a999ac469a5ba0f6f5e439890e1f0c03f9038fc996285ce5a752b6#align=left&display=inline&height=786&id=FL1Rr&margin=%5Bobject%20Object%5D&originHeight=786&originWidth=965&status=done&style=none&width=965)

> NodeDataChanged：节点数据内容主要是基于 dataVersion。只要是执行了更新操作，不管数据是否有变换，都会触发 Watcher 事件。

- WatcherEvent

WatcherEvent  & WatchedEvent ：服务端事件的封装。WatcherEvent   实现了序列化，看用于网络传输。

```java
public class WatcherEvent implements Record {
  private int type;
  private int state;
  private String path;
}
public class WatchedEvent {
    final private KeeperState keeperState;
    final private EventType eventType;
    private String path;
}
```

- WatchRegistration

Watcher 注册信息类，保存数据节点与 Watcher 的对应关系。三个具体实现类。

```java
abstract class WatchRegistration {
    private Watcher watcher;
    private String clientPath;
}
```

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fuploader.shimo.im%2Ff%2FpsgcmEmCYp00Omki.png!thumbnail&sign=202cb3e94d161cc83e181ab6d34fb6931aa3939e73e385c56b07f87288d895ad#align=left&display=inline&height=137&id=BZgOS&margin=%5Bobject%20Object%5D&originHeight=137&originWidth=890&status=done&style=none&width=890)

- ZKWatchManager

客户端 Watcher 管理类，主要存储不同类型 Watcher。

```java
private static class ZKWatchManager implements ClientWatchManager {
    private final Map<String, Set<Watcher>> dataWatches =
        new HashMap<String, Set<Watcher>>();
    private final Map<String, Set<Watcher>> existWatches =
        new HashMap<String, Set<Watcher>>();
    private final Map<String, Set<Watcher>> childWatches =
        new HashMap<String, Set<Watcher>>();
}
```

- WathcerManager

ZooKeeper 服务端 Watcher 的管理者。内部管理 watchTable 和 watch2Paths 两个存储结构，同时还负责触发 Watcher 并移除已经触发的 Watcher。

```java
public class WatchManager {
    // 数据节点 --> Wathcer
    private final HashMap<String, HashSet<Watcher>> watchTable =
        new HashMap<String, HashSet<Watcher>>();
    // Watcher --> Paths
    private final HashMap<Watcher, HashSet<String>> watch2Paths =
        new HashMap<Watcher, HashSet<String>>();
}
```

- ServerCnxn

ZooKeeper 客户端与服务端之间的连接接口，代表一个客户端和服务端的连接！同时还是一个 Watcher 对象！服务端在存储 Watcher 列表时，实际保存的就是这个连接对象！
默认实现为 NIOServerCnxn，3.4.0 版本后引入了 Netty 实现 NettyServerCnxn。

```java
/**
 * Interface to a Server connection - represents a connection from a client
 * to the server.
 */
public abstract class ServerCnxn implements Stats, Watcher {}
```

- ClientCnxn

Client Socket I/O 管理类。

```java
/**
 * This class manages the socket i/o for the client.
 * ClientCnxn maintains a list of available servers to connect to
 * and "transparently" switches servers it is
 * connected to as needed.
 */
public class ClientCnxn {
    private final LinkedList<Packet> pendingQueue = new LinkedList<Packet>();
    private final LinkedList<Packet> outgoingQueue = new LinkedList<Packet>();
    ...
}
```

- 工作机制

ZooKeeper 的 Watcher 机制，总的来说可以概括为三个过程：客户端注册 Watcher，服务端处理 Watcher 和客户端回调 Watcher。

- 客户端注册 Watcher

```java
// 注册方式 构造器注册：
public ZooKeeper(String connectString, int sessionTimeout, Watcher watcher){...}
// getData getChildren  exist 三个接口注册：
public Stat exists(final String path, Watcher watcher);
public byte[] getData(final String path, Watcher watcher, Stat stat);
public List<String> getChildren(final String path, Watcher watcher);
```

> 客户端的 Watcher 实例不会随着客户端的请求而发送到服务端！Packet.createBB() 在序列化时只会将 requestHeader 和 request 两个属性进行实例化！客户端实际只会告诉服务端是否有 Watcher！

- 服务端处理 Watcher

服务端处理 Watcher 时序图：

```java
// org.apache.zookeeper.server.FinalRequestProcessor#processRequest
case OpCode.getData: {
    ....
    byte b[] = zks.getZKDatabase().getData(getDataRequest.getPath(), stat, getDataRequest.getWatch() ? cnxn : null);
    rsp = new GetDataResponse(b, stat);
    break;
}
// org.apache.zookeeper.server.WatchManager#addWatch
// AddWatcher 时，实际添加的都是 ServerCnxn，也就是服务端保存的 Watcher 信息
// 其实就是每个客户端连接
public synchronized void addWatch(String path, Watcher watcher) {
    HashSet<Watcher> list = watchTable.get(path);
    ...
    list.add(watcher);
    HashSet<String> paths = watch2Paths.get(watcher);
    ...
    paths.add(path);
}
```

- Watcher 触发

```java
public Set<Watcher> triggerWatch(String path, EventType type, Set<Watcher> supress) {
    WatchedEvent e = new WatchedEvent(type,
            KeeperState.SyncConnected, path);
    HashSet<Watcher> watchers;
    synchronized (this) {
        watchers = watchTable.remove(path);
        if (watchers == null || watchers.isEmpty()) {
            if (LOG.isTraceEnabled()) {
                ZooTrace.logTraceMessage(LOG,
                        ZooTrace.EVENT_DELIVERY_TRACE_MASK,
                        "No watchers for " + path);
            }
            return null;
        }
        for (Watcher w : watchers) {
            HashSet<String> paths = watch2Paths.get(w);
            if (paths != null) {
                paths.remove(path);
            }
        }
    }
    for (Watcher w : watchers) {
        if (supress != null && supress.contains(w)) {
            continue;
        }
        w.process(e);
    }
    return watchers;
}
```

Watcher 触发的基本逻辑如下：

1. 封装服务端事件 WatchedEvent；
1. 查询 Watcher 并移除；
1. 调用 process 方法来触发 Watcher。

前面已经提到 ZooKeeper 会把当前请求 ServerCnxn 当做一个 Watcher   进行存储，因此实际调用的 process 方法，事实上就是 ServerCnxn 的 process 方法！

```java
public void process(WatchedEvent event) {
    // 请求头标记为 -1 ，表示当前是一个通知
    ReplyHeader h = new ReplyHeader(-1, -1L, 0);
    // 包装成 watcherEnvent，以便网络传输。
    WatcherEvent e = event.getWrapper();
    try {
        // 向客户端发送通知
        sendResponse(h, e, "notification");
    } catch (IOException e1) {
        close();
    }
}
```

总结：本质上服务端不会处理客户端的 Watcher 的真正逻辑，而是借助于当前客户端 TCP 连接 ServerCnxn 来实现客户端的 WatchedEvent 的传递，真正的客户端回调和业务逻辑都在客户端执行。

- 客户端回调 Watcher

服务端通过使用 ServerCnxn 对应的 TCP 连接向客户端发送一个 WatcherEvent 事件，客户端具体是如何处理这个事件的呢？
对于服务端的一个响应，客户端都是由 SendThread.readResponse 来统一接收处理的，然后再交由 EventThread.queueEvent 来处理回调！

```java
// 1. org.apache.zookeeper.ClientCnxn.SendThread#readResponse
void readResponse(ByteBuffer incomingBuffer) throws IOException {
    ...
    // -1 means notification
    if (replyHdr.getXid() == -1) {
        // 1. 反序列化
        WatcherEvent event = new WatcherEvent();
        event.deserialize(bbia, "response");
        // 2.路径转换:将服务端的完整路径调整为客户端的相对路径
        if (chrootPath != null) {
            String serverPath = event.getPath();
            if(serverPath.compareTo(chrootPath)==0)
                event.setPath("/");
            else if (serverPath.length() > chrootPath.length())
                event.setPath
                      (serverPath.substring(chrootPath.length()));
        }
        // 3. 还原 WatchedEvent
        WatchedEvent we = new WatchedEvent(event);
        // 4. 回调 Watcher
        eventThread.queueEvent( we );
        return;
    }
}
// 2. org.apache.zookeeper.ClientCnxn.EventThread#queueEvent
public void queueEvent(WatchedEvent event) {
    sessionState = event.getState();
    // 根据事件取出相关的 Watcher
    WatcherSetEventPair pair = new WatcherSetEventPair(
            watcher.materialize(event.getState(), event.getType(),
                    event.getPath()),
                    event);
    // 放入队列，等待处理
    // LinkedBlockingQueue<Object> waitingEvents
    waitingEvents.add(pair);
}
   // 根据事件取出相关的 Watcher
    /*
     * @see org.apache.zookeeper.ClientWatchManager#materialize
     */
    @Override
    public Set<Watcher> materialize(Watcher.Event.KeeperState state,
                                    Watcher.Event.EventType type,
                                    String clientPath) {
        Set<Watcher> result = new HashSet<Watcher>();
        switch (type) {
        case None:
            result.add(defaultWatcher);
            boolean clear = ClientCnxn.getDisableAutoResetWatch() &&
                    state != Watcher.Event.KeeperState.SyncConnected;
            synchronized(dataWatches) {
                for(Set<Watcher> ws: dataWatches.values()) {
                    result.addAll(ws);
                }
                if (clear) {
                    dataWatches.clear();
                }
            }
            synchronized(existWatches) {
                for(Set<Watcher> ws: existWatches.values()) {
                    result.addAll(ws);
                }
                if (clear) {
                    existWatches.clear();
                }
            }
            synchronized(childWatches) {
                for(Set<Watcher> ws: childWatches.values()) {
                    result.addAll(ws);
                }
                if (clear) {
                    childWatches.clear();
                }
            }
            return result;
        case NodeDataChanged:
        case NodeCreated:
            synchronized (dataWatches) {
                addTo(dataWatches.remove(clientPath), result);
            }
            synchronized (existWatches) {
                addTo(existWatches.remove(clientPath), result);
            }
            break;
        case NodeChildrenChanged:
            synchronized (childWatches) {
                addTo(childWatches.remove(clientPath), result);
            }
            break;
        case NodeDeleted:
            synchronized (dataWatches) {
                addTo(dataWatches.remove(clientPath), result);
            }
            // XXX This shouldn't be needed, but just in case
            synchronized (existWatches) {
                Set<Watcher> list = existWatches.remove(clientPath);
                if (list != null) {
                    addTo(list, result);
                }
            }
            synchronized (childWatches) {
                addTo(childWatches.remove(clientPath), result);
            }
            break;
        default:
            throw new RuntimeException(msg);
        }
        return result;
    }
}
// EventThread.run 方法持续消费
public void run() {
   try {
      isRunning = true;
      while (true) {
         Object event = waitingEvents.take();
         if (event == eventOfDeath) {
            wasKilled = true;
         } else {
            processEvent(event);
         }
         if (wasKilled)
            synchronized (waitingEvents) {
               if (waitingEvents.isEmpty()) {
                  isRunning = false;
                  break;
               }
            }
      }
   } catch (InterruptedException e) {
      LOG.error("Event thread exiting due to interruption", e);
   }
}
// 执行 Watcher 的 process
private void processEvent(Object event) {
   try {
       if (event instanceof WatcherSetEventPair) {
           // each watcher will process the event
           WatcherSetEventPair pair = (WatcherSetEventPair) event;
           // 串行处理
           for (Watcher watcher : pair.watchers) {
               try {
                   // 实际的 watcher
                   watcher.process(pair.event);
               } catch (Throwable t) {
                   LOG.error("Error while calling watcher ", t);
               }
           }
       } else {
         ...
       }
}
```

- Watcher 特性总结
  - 一次性

无论是在服务端还是在客户端，一旦一个 Watcher 被触发，ZooKeeper 都会将其从存储中移除！

- 客户端串行

客户端 Watcher 的回调过程是个串行同步的过程，保证了顺序性！

- 轻量级

WatchedEvent 是 ZooKeeper 整个 Watcher 通知机制的最小通知单元。只包含事件类型、通知状态和节点路径。不包含原始数据和新数据。
另外客户端向服务端注册 Watcher 时，并不会把客户端真实的 Watcher 对象传给服务端，而只是用个 boolean 变量进行标记，服务端存储的仅仅只是当前连接的 ServerCnxn。

## 4、客户端

ZooKeeper 的客户端主要由以下几个核心组价组成：

- ZooKeeper 实例：客户端入口。
- ClientWatcherManager：客户端 Watcher 管理器。
- HostProvider：客户端地址列表管理器。
- ClientCnxn：客户端核心线程，内部又包含 SendThread、EventThread 两个线程。
  - SendThread：I/O 线程，主要负责 ZooKeeper 客户端和服务端之间的网络 I/O 通信。
  - EventThread：事件线程，主要负责对服务端的事件进行处理
