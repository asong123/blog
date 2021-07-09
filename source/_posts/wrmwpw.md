---
title: 34、JUC精讲
urlname: wrmwpw
date: '2021-07-09 20:46:46 +0800'
tags: []
categories: []
---

# 1、什么是 JUC

java.util.concurrent 包是在并发编程中使用的工具类，有以下三个包：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834810049-339d6c13-dea3-49a7-b170-e66d82260b52.jpeg#)

# 2、进程和线程回顾

进程 / 线程是什么？
进程：进程是一个具有一定独立功能的程序关于某个数据集合的一次运行活动。它是操作系统动态执行 的基本单元，在传统的操作系统中，进程既是基本的分配单元，也是基本的执行单元。
线程：通常在一个进程中可以包含若干个线程，当然一个进程中至少有一个线程，不然没有存在的意 义，线程可以利用进程所有拥有的资源。在引入线程的操作系统中，通常都是把进程作为分配资源的基 本单位，而把线程作为独立运行和独立调度的基本单位，由于线程比进程小，基本上不拥有系统资源， 故对它的调度所付出的开销就会小得多，能更高效的提高系统多个程序间并发执行的程度。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834810616-2cdc6e1f-68de-4c0b-a669-36c646030609.jpeg#)

### 白话：

进程：就是操作系统中运行的一个程序，QQ.exe, music.exe， word.exe ，这就是多个进程

线程：每个进程中都存在一个或者多个线程，比如用 word 写文章时，就会有一个线程默默帮你定时自动保存。
学习一些枯燥的概念的时候，一定要与生活中的例子结合起来，这样记忆才是最方便的。

并发 / 并行是什么？
做并发编程之前，必须首先理解什么是并发，什么是并行。
**并发和并行**是两个非常容易混淆的概念。它们都可以表示两个或多个任务一起执行，但是偏重点有点不 同。并发偏重于多个任务交替执行，而多个任务之间有可能还是串行的。并发是逻辑上的同时发生
（simultaneous），而并行是物理上的同时发生。然而并行的偏重点在于”同时执行”。
严格意义上来说，并行的多个任务是真实的同时执行，而对于并发来说，这个过程只是交替的，一会运 行任务一,一会儿又运行任务二，系统会不停地在两者间切换。但对于外部观察者来说，即使多个任务是 串行并发的，也会造成是多个任务并行执行的错觉。
实际上，如果系统内只有一个 CPU，而现在而使用多线程或者多线程任务，那么真实环境中这些任务不可能真实并行的，毕竟一个 CPU 一次只能执行一条指令，这种情况下多线程或者多线程任务就是并发 的，而不是并行，操作系统会不停的切换任务。真正的并发也只能够出现在拥有多个 CPU 的系统中（多核 CPU）。
**并发的动机**：在计算能力恒定的情况下处理更多的任务, 就像我们的大脑, 计算能力相对恒定, 要在一天中处理更多的问题, 我们就必须具备多任务的能力. 现实工作中有很多事情可能会中断你的当前任务, 处理这种多任务的能力就是你的并发能力。
**并行的动机**：用更多的 CPU 核心更快的完成任务. 就像一个团队, 一个脑袋不够用了, 一个团队来一起处理一个任务。
例子：
你吃饭吃到一半，电话来了，你一直到吃完了以后才去接，这就说明你不支持并发也不支持并行。
你吃饭吃到一半，电话来了，你停了下来接了电话，接完后继续吃饭，这说明你支持并发。 （不一定是同时的）
你吃饭吃到一半，电话来了，你一边打电话一边吃饭，这说明你支持并行。
**所以并发编程的目标是充分的利用处理器的每一个核，以达到最高的处理性能。**

线程的状态
Java 的线程有 6 种状态：可以分析源码：

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
public enum State {
//线程刚创建 NEW,
//在 JVM 中正在运行的线程
RUNNABLE,

//线程处于阻塞状态，等待监视锁，可以重新进行同步代码块中执行 BLOCKED,

//等待状态

| 12  |     | WAITING,                                                 |
| --- | --- | -------------------------------------------------------- |
| 13  |     |                                                          |
| 14  |     | //调用 sleep() join() wait()方法可能导致线程处于等待状态 |
| 15  |     | TIMED_WAITING,                                           |
| 16  |     |                                                          |
| 17  |     | //线程执行完毕，已经退出                                 |
| 18  |     | TERMINATED;                                              |
| 19  | }   |                                                          |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834811011-012264d0-d6fc-40ba-988a-d4974e58975f.jpeg#)

wait / sleep 的区别

### 1、来自不同的类

这两个方法来自不同的类分别是，sleep 来自 Thread 类，和 wait 来自 Object 类。
sleep 是 Thread 的静态类方法，谁调用的谁去睡觉，即使在 a 线程里调用了 b 的 sleep 方法，实际上还是 a
去睡觉，要让 b 线程睡觉要在 b 的代码中调用 sleep。

### 2、有没有释放锁(释放资源)

最主要是 sleep 方法没有释放锁，而 wait 方法释放了锁，使得其他线程可以使用同步控制块或者方法。
sleep 是线程被调用时，占着 cpu 去睡觉，其他线程不能占用 cpu，os 认为该线程正在工作，不会让出系统资源，wait 是进入等待池等待，让出系统资源，其他线程可以占用 cpu。
sleep(100L)是占用 cpu，线程休眠 100 毫秒，其他进程不能再占用 cpu 资源，wait（100L）是进入等待池中等待，交出 cpu 等系统资源供其他进程使用，在这 100 毫秒中，该线程可以被其他线程 notify，但不同的是其他在等待池中的线程不被 notify 不会出来，但这个线程在等待 100 毫秒后会自动进入就绪队列等待系统分配资源，换句话说，sleep（100）在 100 毫秒后肯定会运行，但 wait 在 100 毫秒后还有等待 os 调 用分配资源，所以 wait100 的停止运行时间是不确定的，但至少是 100 毫秒。

就是说 sleep 有时间限制的就像闹钟一样到时候就叫了，而 wait 是无限期的除非用户主动 notify。

### 3、使用范围不同

wait，notify 和 notifyAll 只能在同步控制方法或者同步控制块里面使用，而 sleep 可以在任何地方使用

synchronized(x){
//或者 wait() x.notify()
}
1
2
3
4

### 4、是否需要捕获异常

sleep 必须捕获异常，而 wait，notify 和 notifyAll 不需要捕获异常。

# 3、Lock 锁

传统的 synchronized
1 /\*

1. - 题目：三个售票员 卖出 30 张票
1. - 多线程编程的企业级套路：
1. - 1. 在高内聚低耦合的前提下， 线程 操作(对外暴露的调用方法) 资源类

5 \*/

1.  public class SaleTicketTest1 {
1.      public static void main(String[] args) throws Exception { // main一切程序的入口

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
35
Ticket ticket = new Ticket();
new Thread(new Runnable() { @Override
public void run() {
for (int i = 1; i <=40; i++) { ticket.saleTicket();
}
}
}, "A").start();

new Thread(new Runnable() { @Override
public void run() {
for (int i = 1; i <=40; i++) { ticket.saleTicket();
}
}
}, "B").start();

new Thread(new Runnable() { @Override
public void run() {
for (int i = 1; i <=40; i++) { ticket.saleTicket();
}
}

| 36  | }, "C").start();                                                  |
| --- | ----------------------------------------------------------------- |
| 37  | }                                                                 |
| 38  | }                                                                 |
| 39  |                                                                   |
| 40  | class Ticket{ // 资源类                                           |
| 41  | private int number = 30;                                          |
| 42  |                                                                   |
| 43  | public synchronized void saleTicket(){                            |
| 44  | if (number>0){                                                    |
| 45  | System.out.println(Thread.currentThread().getName() + "卖出第 " + |
|     | (number--) + "票,还剩下:"+number);                                |
| 46  | }                                                                 |
| 47  | }                                                                 |
| 48  | }                                                                 |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834812220-d132017c-bda0-4f57-97b8-90fcff219329.jpeg#)
使用 juc.locks 包下的类操作 Lock 锁 + Lambda 表达式

1.  public class SaleTicketTest2 {
1.      public static void main(String[] args) throws Exception { // main一切程序的入口

3
4 Ticket ticket = new Ticket(); 5
6

7
8
new Thread(()->{for (int i = 1; i <=40; i++) ticket.saleTicket();}, "A").start();
new Thread(()->{for (int i = 1; i <=40; i++) ticket.saleTicket();}, "B").start();
new Thread(()->{for (int i = 1; i <=40; i++) ticket.saleTicket();},
"C").start();
9
10 }

| 11  | }                                                             |
| --- | ------------------------------------------------------------- |
| 12  |                                                               |
| 13  | class Ticket{ // 资源类                                       |
| 14  |                                                               |
| 15  | private Lock lock = new ReentrantLock();                      |
| 16  |                                                               |
| 17  | private int number = 30;                                      |
| 18  |                                                               |
| 19  | public void saleTicket(){                                     |
| 20  |                                                               |
| 21  | lock.lock();                                                  |
| 22  | try {                                                         |
| 23  | if (number>0){                                                |
| 24  | System.out.println(Thread.currentThread().getName() + "卖出第 |
|     | " + (number--) + "票,还剩下:"+number);                        |
| 25  | }                                                             |
| 26  | } catch (Exception e) {                                       |
| 27  | e.printStackTrace();                                          |
| 28  | } finally {                                                   |
| 29  | lock.unlock();                                                |
| 30  | }                                                             |
| 31  | }                                                             |
| 32  |                                                               |
| 33  | }                                                             |

synchronized 和 lock 区别

1. 首先 synchronized 是 java 内置关键字，在 jvm 层面，Lock 是个 java 类；
1. synchronized 无法判断是否获取锁的状态，Lock 可以判断是否获取到锁；
1. synchronized 会自动释放锁(a 线程执行完同步代码会释放锁 ；b 线程执行过程中发生异常会释放锁)，Lock 需在 ﬁnally 中手工释放锁（unlock()方法释放锁），否则容易造成线程死锁；
1. 用 synchronized 关键字的两个线程 1 和线程 2，如果当前线程 1 获得锁，线程 2 线程等待。如果线程 1 阻塞，线程 2 则会一直等待下去，而 Lock 锁就不一定会等待下去，如果尝试获取不到锁，线程可以不用一直等待就结束了；
1. synchronized 的锁可重入、不可中断、非公平，而 Lock 锁可重入、可判断、可公平（两者皆可）
1. ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834812821-f10b90d9-d4c1-4418-a53a-c0ad7d90bbc1.jpeg#)Lock 锁适合大量同步的代码的同步问题，synchronized 锁适合代码少量的同步问题。

# 4、生产者和消费者

线程间的通信 , 线程之间要协调和调度

生产者和消费者 synchroinzed 版

| 1   | package com.kuang;                                                 |
| --- | ------------------------------------------------------------------ |
| 2   |                                                                    |
| 3   | /\*\*                                                              |
| 4   | \* 题目：现在两个线程，可以操作初始值为 0 的一个变量               |
| 5   | \* 实现一个线程对该变量 + 1，一个线程对该变量 -1                   |
| 6   | \* 实现交替 10 次                                                  |
| 7   | \*                                                                 |
| 8   | \* 诀窍：                                                          |
| 9   | \* 1. 高内聚低耦合的前提下，线程操作资源类                         |
| 10  | \* 2. 判断 、干活、通知                                            |
| 11  | \*/                                                                |
| 12  | public class B {                                                   |
| 13  | public static void main(String[] args) throws Exception {          |
| 14  | Data data = new Data();                                            |
| 15  |                                                                    |
| 16  | new Thread(()->{                                                   |
| 17  | for (int i = 1; i <= 10; i++) {                                    |
| 18  | try {                                                              |
| 19  | data.increment();                                                  |
| 20  | } catch (InterruptedException e) {                                 |
| 21  | e.printStackTrace();                                               |
| 22  | }                                                                  |
| 23  | }                                                                  |
| 24  | },"A").start();                                                    |
| 25  |                                                                    |
| 26  | new Thread(()->{                                                   |
| 27  | for (int i = 1; i <= 10; i++) {                                    |
| 28  | try {                                                              |
| 29  | data.decrement();                                                  |
| 30  | } catch (InterruptedException e) {                                 |
| 31  | e.printStackTrace();                                               |
| 32  | }                                                                  |
| 33  | }                                                                  |
| 34  | },"B").start();                                                    |
| 35  | }                                                                  |
| 36  | }                                                                  |
| 37  |                                                                    |
| 38  | class Data{ // 资源类                                              |
| 39  |                                                                    |
| 40  | private int number = 0;                                            |
| 41  |                                                                    |
| 42  | public synchronized void increment() throws InterruptedException { |
| 43  | // 判断该不该这个线程做                                            |
| 44  | if (number!=0){                                                    |
| 45  | this.wait();                                                       |
| 46  | }                                                                  |
| 47  | // 干活                                                            |
| 48  | number++;                                                          |
| 49  | System.out.println(Thread.currentThread().getName()+"\t"+number);  |
| 50  | // 通知                                                            |

| 51  |     | this.notifyAll();                                                  |
| --- | --- | ------------------------------------------------------------------ |
| 52  |     | }                                                                  |
| 53  |     |                                                                    |
| 54  |     | public synchronized void decrement() throws InterruptedException { |
| 55  |     | // 判断该不该这个线程做                                            |
| 56  |     | if (number==0){                                                    |
| 57  |     | this.wait();                                                       |
| 58  |     | }                                                                  |
| 59  |     | // 干活                                                            |
| 60  |     | number--;                                                          |
| 61  |     | System.out.println(Thread.currentThread().getName()+"\t"+number);  |
| 62  |     | // 通知                                                            |
| 63  |     | this.notifyAll();                                                  |
| 64  |     | }                                                                  |
| 65  |     |                                                                    |
| 66  | }   |                                                                    |

问题升级：防止虚假唤醒，4 个线程，两个加，两个减
【重点】if 和 while
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834813342-50e4b4d7-aae1-4ade-9937-e291aed960df.jpeg#)

| 1   | package com.kuang;                                                                  |
| --- | ----------------------------------------------------------------------------------- |
| 2   |                                                                                     |
| 3   | /\*\*                                                                               |
| 4   | \* 题目：现在四个线程，可以操作初始值为 0 的一个变量                                |
| 5   | \* 实现两个线程对该变量 + 1，两个线程对该变量 -1                                    |
| 6   | \* 实现交替 10 次                                                                   |
| 7   | \*                                                                                  |
| 8   | \* 诀窍：                                                                           |
| 9   | \* 1. 高内聚低耦合的前提下，线程操作资源类                                          |
| 10  | \* 2. 判断 、干活、通知                                                             |
| 11  | \* 3. 多线程交互中，必须要防止多线程的虚假唤醒，也即（判断不能用 if，只能用 while） |
| 12  | \*/                                                                                 |
| 13  | public class B {                                                                    |
| 14  | public static void main(String[] args) throws Exception {                           |
| 15  | Data data = new Data();                                                             |
| 16  |                                                                                     |
| 17  | new Thread(()->{                                                                    |
| 18  | for (int i = 1; i <= 10; i++) {                                                     |

| 19  | try {                                                              |
| --- | ------------------------------------------------------------------ |
| 20  | data.increment();                                                  |
| 21  | } catch (InterruptedException e) {                                 |
| 22  | e.printStackTrace();                                               |
| 23  | }                                                                  |
| 24  | }                                                                  |
| 25  | },"A").start();                                                    |
| 26  |                                                                    |
| 27  | new Thread(()->{                                                   |
| 28  | for (int i = 1; i <= 10; i++) {                                    |
| 29  | try {                                                              |
| 30  | data.decrement();                                                  |
| 31  | } catch (InterruptedException e) {                                 |
| 32  | e.printStackTrace();                                               |
| 33  | }                                                                  |
| 34  | }                                                                  |
| 35  | },"B").start();                                                    |
| 36  |                                                                    |
| 37  | new Thread(()->{                                                   |
| 38  | for (int i = 1; i <= 10; i++) {                                    |
| 39  | try {                                                              |
| 40  | data.increment();                                                  |
| 41  | } catch (InterruptedException e) {                                 |
| 42  | e.printStackTrace();                                               |
| 43  | }                                                                  |
| 44  | }                                                                  |
| 45  | },"C").start();                                                    |
| 46  |                                                                    |
| 47  | new Thread(()->{                                                   |
| 48  | for (int i = 1; i <= 10; i++) {                                    |
| 49  | try {                                                              |
| 50  | data.decrement();                                                  |
| 51  | } catch (InterruptedException e) {                                 |
| 52  | e.printStackTrace();                                               |
| 53  | }                                                                  |
| 54  | }                                                                  |
| 55  | },"D").start();                                                    |
| 56  | }                                                                  |
| 57  | }                                                                  |
| 58  |                                                                    |
| 59  | class Data{ // 资源类                                              |
| 60  |                                                                    |
| 61  | private int number = 0;                                            |
| 62  |                                                                    |
| 63  | public synchronized void increment() throws InterruptedException { |
| 64  | // 判断该不该这个线程做                                            |
| 65  | while (number!=0){                                                 |
| 66  | this.wait();                                                       |
| 67  | }                                                                  |
| 68  | // 干活                                                            |
| 69  | number++;                                                          |
| 70  | System.out.println(Thread.currentThread().getName()+"\t"+number);  |
| 71  | // 通知                                                            |
| 72  | this.notifyAll();                                                  |
| 73  | }                                                                  |
| 74  |                                                                    |
| 75  | public synchronized void decrement() throws InterruptedException { |
| 76  | // 判断该不该这个线程做                                            |

| 77  |     |     | while (number==0){                                                |
| --- | --- | --- | ----------------------------------------------------------------- |
| 78  |     |     | this.wait();                                                      |
| 79  |     |     | }                                                                 |
| 80  |     |     | // 干活                                                           |
| 81  |     |     | number--;                                                         |
| 82  |     |     | System.out.println(Thread.currentThread().getName()+"\t"+number); |
| 83  |     |     | // 通知                                                           |
| 84  |     |     | this.notifyAll();                                                 |
| 85  |     | }   |                                                                   |
| 86  |     |     |                                                                   |
| 87  | }   |     |                                                                   |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834813859-31739837-45fd-47d9-af44-4f2977eed5d6.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834814452-06df97f6-0bdb-4a18-affb-fb7ed639e865.jpeg#)
新版生产者和消费者写法

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834814963-55442190-6f59-4b36-bed7-32f242562e92.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834815282-13aaa2f8-59c3-4fb5-9f39-58d749dfdecb.png#)

闲聊常见笔试题：手写单例模式、手写冒泡排序、手写生产者消费者代码测试：

| 1   | package com.kuang;                                                                  |
| --- | ----------------------------------------------------------------------------------- |
| 2   |                                                                                     |
| 3   | import java.util.concurrent.locks.Condition;                                        |
| 4   | import java.util.concurrent.locks.Lock;                                             |
| 5   | import java.util.concurrent.locks.ReentrantLock;                                    |
| 6   |                                                                                     |
| 7   | /\*\*                                                                               |
| 8   | \* 题目：现在四个线程，可以操作初始值为 0 的一个变量                                |
| 9   | \* 实现两个线程对该变量 + 1，两个线程对该变量 -1                                    |
| 10  | \* 实现交替 10 次                                                                   |
| 11  | \* <p>                                                                              |
| 12  | \* 诀窍：                                                                           |
| 13  | \* 1. 高内聚低耦合的前提下，线程操作资源类                                          |
| 14  | \* 2. 判断 、干活、通知                                                             |
| 15  | \* 3. 多线程交互中，必须要防止多线程的虚假唤醒，也即（判断不能用 if，只能用 while） |
| 16  | \*/                                                                                 |
| 17  | public class B {                                                                    |
| 18  | public static void main(String[] args) throws Exception {                           |
| 19  | Data data = new Data();                                                             |
| 20  |                                                                                     |
| 21  | new Thread(() -> {                                                                  |
| 22  | for (int i = 1; i <= 10; i++) {                                                     |
| 23  | try {                                                                               |
| 24  | data.increment();                                                                   |
| 25  | } catch (InterruptedException e) {                                                  |
| 26  | e.printStackTrace();                                                                |

| 27  | }                                                            |
| --- | ------------------------------------------------------------ |
| 28  | }                                                            |
| 29  | }, "A").start();                                             |
| 30  |                                                              |
| 31  | new Thread(() -> {                                           |
| 32  | for (int i = 1; i <= 10; i++) {                              |
| 33  | try {                                                        |
| 34  | data.decrement();                                            |
| 35  | } catch (InterruptedException e) {                           |
| 36  | e.printStackTrace();                                         |
| 37  | }                                                            |
| 38  | }                                                            |
| 39  | }, "B").start();                                             |
| 40  |                                                              |
| 41  | new Thread(() -> {                                           |
| 42  | for (int i = 1; i <= 10; i++) {                              |
| 43  | try {                                                        |
| 44  | data.increment();                                            |
| 45  | } catch (InterruptedException e) {                           |
| 46  | e.printStackTrace();                                         |
| 47  | }                                                            |
| 48  | }                                                            |
| 49  | }, "C").start();                                             |
| 50  |                                                              |
| 51  | new Thread(() -> {                                           |
| 52  | for (int i = 1; i <= 10; i++) {                              |
| 53  | try {                                                        |
| 54  | data.decrement();                                            |
| 55  | } catch (InterruptedException e) {                           |
| 56  | e.printStackTrace();                                         |
| 57  | }                                                            |
| 58  | }                                                            |
| 59  | }, "D").start();                                             |
| 60  | }                                                            |
| 61  | }                                                            |
| 62  |                                                              |
| 63  | class Data { // 资源类                                       |
| 64  |                                                              |
| 65  | private int number = 0;                                      |
| 66  | private Lock lock = new ReentrantLock();                     |
| 67  | private Condition condition = lock.newCondition();           |
| 68  |                                                              |
| 69  | public void increment() throws InterruptedException {        |
| 70  |                                                              |
| 71  | lock.lock();                                                 |
| 72  | try {                                                        |
| 73  | // 判断该不该这个线程做                                      |
| 74  | while (number != 0) {                                        |
| 75  | condition.await();                                           |
| 76  | }                                                            |
| 77  | // 干活                                                      |
| 78  | number++;                                                    |
| 79  | System.out.println(Thread.currentThread().getName() + "\t" + |
|     | number);                                                     |
| 80  | // 通知                                                      |
| 81  | condition.signalAll();                                       |
| 82  | } catch (Exception e) {                                      |
| 83  | e.printStackTrace();                                         |

| 84  | } finally {                                                  |
| --- | ------------------------------------------------------------ |
| 85  | lock.unlock();                                               |
| 86  | }                                                            |
| 87  |                                                              |
| 88  | }                                                            |
| 89  |                                                              |
| 90  | public void decrement() throws InterruptedException {        |
| 91  | lock.lock();                                                 |
| 92  | try {                                                        |
| 93  | // 判断该不该这个线程做                                      |
| 94  | while (number == 0) {                                        |
| 95  | condition.await();                                           |
| 96  | }                                                            |
| 97  | // 干活                                                      |
| 98  | number--;                                                    |
| 99  | System.out.println(Thread.currentThread().getName() + "\t" + |
|     | number);                                                     |
| 100 | // 通知                                                      |
| 101 | condition.signalAll();                                       |
| 102 | } catch (Exception e) {                                      |
| 103 | e.printStackTrace();                                         |
| 104 | } finally {                                                  |
| 105 | lock.unlock();                                               |
| 106 | }                                                            |
| 107 | }                                                            |
| 108 |                                                              |
| 109 | }                                                            |

精确通知顺序访问

| 1   | package com.kuang;                                      |
| --- | ------------------------------------------------------- |
| 2   |                                                         |
| 3   | import java.util.concurrent.locks.Condition;            |
| 4   | import java.util.concurrent.locks.Lock;                 |
| 5   | import java.util.concurrent.locks.ReentrantLock;        |
| 6   |                                                         |
| 7   | /\*\*                                                   |
| 8   | \* 题目：多线程之间按顺序调用，实现 A->B->C             |
| 9   | \* 三个线程启动，要求如下：                             |
| 10  | \* AA 打印 5 次，BB 打印 10 次。CC 打印 15 次，依次循环 |
| 11  | \*                                                      |
| 12  | \* 重点：标志位                                         |
| 13  | \*/                                                     |
| 14  | public class C {                                        |
| 15  | public static void main(String[] args) {                |
| 16  | Resources resources = new Resources();                  |
| 17  |                                                         |
| 18  | new Thread(()->{                                        |
| 19  | for (int i = 1; i <=10; i++) {                          |
| 20  | resources.print5();                                     |
| 21  | }                                                       |
| 22  | },"AA").start();                                        |
| 23  |                                                         |
| 24  | new Thread(()->{                                        |
| 25  | for (int i = 1; i <=10; i++) {                          |

| 26  | resources.print10();                                         |
| --- | ------------------------------------------------------------ |
| 27  | }                                                            |
| 28  | },"BB").start();                                             |
| 29  |                                                              |
| 30  | new Thread(()->{                                             |
| 31  | for (int i = 1; i <=10; i++) {                               |
| 32  | resources.print15();                                         |
| 33  | }                                                            |
| 34  | },"CC").start();                                             |
| 35  | }                                                            |
| 36  | }                                                            |
| 37  |                                                              |
| 38  | class Resources{ // 资源类                                   |
| 39  |                                                              |
| 40  | private int number = 1; // 1A 2B 3C                          |
| 41  | private Lock lock = new ReentrantLock();                     |
| 42  | private Condition condition1 = lock.newCondition();          |
| 43  | private Condition condition2 = lock.newCondition();          |
| 44  | private Condition condition3 = lock.newCondition();          |
| 45  |                                                              |
| 46  | public void print5(){                                        |
| 47  | lock.lock();                                                 |
| 48  | try {                                                        |
| 49  | // 判断                                                      |
| 50  | while (number!=1){                                           |
| 51  | condition1.await();                                          |
| 52  | }                                                            |
| 53  | // 干活                                                      |
| 54  | for (int i = 1; i <= 5; i++) {                               |
| 55  |                                                              |
|     | System.out.println(Thread.currentThread().getName()+'\t'+i); |
| 56  | }                                                            |
| 57  | // 通知,指定的干活！                                         |
| 58  | number = 2;                                                  |
| 59  | condition2.signal();                                         |
| 60  |                                                              |
| 61  | } catch (Exception e) {                                      |
| 62  | e.printStackTrace();                                         |
| 63  | } finally {                                                  |
| 64  | lock.unlock();                                               |
| 65  | }                                                            |
| 66  | }                                                            |
| 67  |                                                              |
| 68  | public void print10(){                                       |
| 69  | lock.lock();                                                 |
| 70  | try {                                                        |
| 71  | // 判断                                                      |
| 72  | while (number!=2){                                           |
| 73  | condition2.await();                                          |
| 74  | }                                                            |
| 75  | // 干活                                                      |
| 76  | for (int i = 1; i <= 10; i++) {                              |
| 77  |                                                              |
|     | System.out.println(Thread.currentThread().getName()+'\t'+i); |
| 78  | }                                                            |
| 79  | // 通知,指定的干活！                                         |
| 80  | number = 3;                                                  |
| 81  | condition3.signal();                                         |

| 82  |                                                              |
| --- | ------------------------------------------------------------ |
| 83  | } catch (Exception e) {                                      |
| 84  | e.printStackTrace();                                         |
| 85  | } finally {                                                  |
| 86  | lock.unlock();                                               |
| 87  | }                                                            |
| 88  | }                                                            |
| 89  |                                                              |
| 90  | public void print15(){                                       |
| 91  | lock.lock();                                                 |
| 92  | try {                                                        |
| 93  | // 判断                                                      |
| 94  | while (number!=3){                                           |
| 95  | condition3.await();                                          |
| 96  | }                                                            |
| 97  | // 干活                                                      |
| 98  | for (int i = 1; i <= 15; i++) {                              |
| 99  |                                                              |
|     | System.out.println(Thread.currentThread().getName()+'\t'+i); |
| 100 | }                                                            |
| 101 | // 通知,指定的干活！                                         |
| 102 | number = 1;                                                  |
| 103 | condition1.signal();                                         |
| 104 |                                                              |
| 105 | } catch (Exception e) {                                      |
| 106 | e.printStackTrace();                                         |
| 107 | } finally {                                                  |
| 108 | lock.unlock();                                               |
| 109 | }                                                            |
| 110 | }                                                            |
| 111 |                                                              |
| 112 | }                                                            |

# 5、8 锁的现象

### 1、标准访问，请问先打印邮件还是短信？

| 1   | package com.kuang;                                                   |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | /\*\*                                                                |
| 4   | \* 多线程的 8 锁                                                     |
| 5   | \* 1、标准访问，请问先打印邮件还是短信？                             |
| 6   | \*/                                                                  |
| 7   | public class Lock8 {                                                 |
| 8   | public static void main(String[] args) throws InterruptedException { |
| 9   | Phone phone = new Phone();                                           |
| 10  |                                                                      |
| 11  | new Thread(()->{                                                     |
| 12  | try {                                                                |
| 13  | phone.sendEmail();                                                   |
| 14  | } catch (Exception e) {                                              |
| 15  | e.printStackTrace();                                                 |
| 16  | }                                                                    |
| 17  | },"A").start();                                                      |
| 18  |                                                                      |

| 19  | Thread.sleep(200);                                     |
| --- | ------------------------------------------------------ |
| 20  |                                                        |
| 21  | new Thread(()->{                                       |
| 22  | try {                                                  |
| 23  | phone.sendSMS();                                       |
| 24  | } catch (Exception e) {                                |
| 25  | e.printStackTrace();                                   |
| 26  | }                                                      |
| 27  | },"B").start();                                        |
| 28  | }                                                      |
| 29  | }                                                      |
| 30  |                                                        |
| 31  |                                                        |
| 32  | class Phone{                                           |
| 33  | public synchronized void sendEmail() throws Exception{ |
| 34  | System.out.println("sendEmail");                       |
| 35  | }                                                      |
| 36  | public synchronized void sendSMS() throws Exception{   |
| 37  | System.out.println("sendSMS");                         |
| 38  | }                                                      |
| 39  | }                                                      |

结论：被 synchronized 修饰的方法，锁的对象是方法的调用者。因为两个方法的调用者是同一个，所以两个方法用的是同一个锁，先调用方法的先执行。

### 2、邮件方法暂停 4 秒钟，请问先打印邮件还是短信？

| 1   | package com.kuang;                                                   |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | import java.util.concurrent.TimeUnit;                                |
| 4   |                                                                      |
| 5   | /\*\*                                                                |
| 6   | \* 多线程的 8 锁                                                     |
| 7   | \* 1、标准访问，请问先打印邮件还是短信？                             |
| 8   | \* 2、邮件方法暂停 4 秒钟，请问先打印邮件还是短信？                  |
| 9   | \*/                                                                  |
| 10  | public class Lock8 {                                                 |
| 11  | public static void main(String[] args) throws InterruptedException { |
| 12  | Phone phone = new Phone();                                           |
| 13  |                                                                      |
| 14  | new Thread(()->{                                                     |
| 15  | try {                                                                |
| 16  | phone.sendEmail();                                                   |
| 17  | } catch (Exception e) {                                              |
| 18  | e.printStackTrace();                                                 |
| 19  | }                                                                    |
| 20  | },"A").start();                                                      |
| 21  |                                                                      |
| 22  | Thread.sleep(200);                                                   |
| 23  |                                                                      |
| 24  | new Thread(()->{                                                     |
| 25  | try {                                                                |
| 26  | phone.sendSMS();                                                     |
| 27  | } catch (Exception e) {                                              |
| 28  | e.printStackTrace();                                                 |
| 29  | }                                                                    |

| 30  | },"B").start();                                        |
| --- | ------------------------------------------------------ |
| 31  | }                                                      |
| 32  | }                                                      |
| 33  |                                                        |
| 34  |                                                        |
| 35  | class Phone{                                           |
| 36  | public synchronized void sendEmail() throws Exception{ |
| 37  | try {                                                  |
| 38  | TimeUnit.SECONDS.sleep(4);                             |
| 39  | } catch (InterruptedException e) {                     |
| 40  | e.printStackTrace();                                   |
| 41  | }                                                      |
| 42  | System.out.println("sendEmail");                       |
| 43  | }                                                      |
| 44  | public synchronized void sendSMS() throws Exception{   |
| 45  | System.out.println("sendSMS");                         |
| 46  | }                                                      |
| 47  | }                                                      |

结论：被 synchronized 修饰的方法，锁的对象是方法的调用者。因为两个方法的调用者是同一个，所以两个方法用的是同一个锁，先调用方法的先执行，第二个方法只有在第一个方法执行完释放锁之后才能 执行。

### 3、新增一个普通方法 hello()没有同步,请问先打印邮件还是 hello？

| 1   | package com.kuang;                                                   |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | import java.util.concurrent.TimeUnit;                                |
| 4   |                                                                      |
| 5   | /\*\*                                                                |
| 6   | \* 多线程的 8 锁                                                     |
| 7   | \* 1、标准访问，请问先打印邮件还是短信？                             |
| 8   | \* 2、邮件方法暂停 4 秒钟，请问先打印邮件还是短信？                  |
| 9   | \* 3、新增一个普通方法 hello()没有同步,请问先打印邮件还是 hello？    |
| 10  | \*/                                                                  |
| 11  | public class Lock8 {                                                 |
| 12  | public static void main(String[] args) throws InterruptedException { |
| 13  | Phone phone = new Phone();                                           |
| 14  |                                                                      |
| 15  | new Thread(()->{                                                     |
| 16  | try {                                                                |
| 17  | phone.sendEmail();                                                   |
| 18  | } catch (Exception e) {                                              |
| 19  | e.printStackTrace();                                                 |
| 20  | }                                                                    |
| 21  | },"A").start();                                                      |
| 22  |                                                                      |
| 23  | Thread.sleep(200);                                                   |
| 24  |                                                                      |
| 25  | new Thread(()->{                                                     |
| 26  | try {                                                                |
| 27  | // phone.sendSMS();                                                  |
| 28  | phone.hello();                                                       |
| 29  | } catch (Exception e) {                                              |
| 30  | e.printStackTrace();                                                 |
| 31  | }                                                                    |

| 32  | },"B").start();                                        |
| --- | ------------------------------------------------------ |
| 33  | }                                                      |
| 34  | }                                                      |
| 35  |                                                        |
| 36  | class Phone{                                           |
| 37  | public synchronized void sendEmail() throws Exception{ |
| 38  | try {                                                  |
| 39  | TimeUnit.SECONDS.sleep(4);                             |
| 40  | } catch (InterruptedException e) {                     |
| 41  | e.printStackTrace();                                   |
| 42  | }                                                      |
| 43  | System.out.println("sendEmail");                       |
| 44  | }                                                      |
| 45  | public synchronized void sendSMS() throws Exception{   |
| 46  | System.out.println("sendSMS");                         |
| 47  | }                                                      |
| 48  |                                                        |
| 49  | public void hello(){                                   |
| 50  | System.out.println("Hello");                           |
| 51  | }                                                      |
| 52  | }                                                      |

结论：新增的方法没有被 synchronized 修饰，不是同步方法，不受锁的影响，所以不需要等待。其他线程共用了一把锁，所以还需要等待。

### 4、两部手机、请问先打印邮件还是短信？

| 1   | package com.kuang;                                                   |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | import java.util.concurrent.TimeUnit;                                |
| 4   |                                                                      |
| 5   | /\*\*                                                                |
| 6   | \* 多线程的 8 锁                                                     |
| 7   | \* 1、标准访问，请问先打印邮件还是短信？                             |
| 8   | \* 2、邮件方法暂停 4 秒钟，请问先打印邮件还是短信？                  |
| 9   | \* 3、新增一个普通方法 hello()没有同步,请问先打印邮件还是 hello？    |
| 10  | \* 4、两部手机、请问先打印邮件还是短信？                             |
| 11  | \*/                                                                  |
| 12  | public class Lock8 {                                                 |
| 13  | public static void main(String[] args) throws InterruptedException { |
| 14  | Phone phone = new Phone();                                           |
| 15  | Phone phone2 = new Phone();                                          |
| 16  |                                                                      |
| 17  | new Thread(()->{                                                     |
| 18  | try {                                                                |
| 19  | phone.sendEmail();                                                   |
| 20  | } catch (Exception e) {                                              |
| 21  | e.printStackTrace();                                                 |
| 22  | }                                                                    |
| 23  | },"A").start();                                                      |
| 24  |                                                                      |
| 25  | Thread.sleep(200);                                                   |
| 26  |                                                                      |
| 27  | new Thread(()->{                                                     |
| 28  | try {                                                                |
| 29  | // phone.sendSMS();                                                  |

| 30  | // phone.hello();                                      |
| --- | ------------------------------------------------------ |
| 31  | phone2.sendSMS();                                      |
| 32  | } catch (Exception e) {                                |
| 33  | e.printStackTrace();                                   |
| 34  | }                                                      |
| 35  | },"B").start();                                        |
| 36  | }                                                      |
| 37  | }                                                      |
| 38  |                                                        |
| 39  |                                                        |
| 40  | class Phone{                                           |
| 41  | public synchronized void sendEmail() throws Exception{ |
| 42  | try {                                                  |
| 43  | TimeUnit.SECONDS.sleep(4);                             |
| 44  | } catch (InterruptedException e) {                     |
| 45  | e.printStackTrace();                                   |
| 46  | }                                                      |
| 47  | System.out.println("sendEmail");                       |
| 48  | }                                                      |
| 49  | public synchronized void sendSMS() throws Exception{   |
| 50  | System.out.println("sendSMS");                         |
| 51  | }                                                      |
| 52  | }                                                      |

结论：被 synchronized 修饰的方法，锁的对象是方法的调用者。因为用了两个对象调用各自的方法，所以两个方法的调用者不是同一个，所以两个方法用的不是同一个锁，后调用的方法不需要等待先调用的 方法。

### 5、两个静态同步方法，同一部手机，请问先打印邮件还是短信？

| 1   | package com.kuang;                                                   |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | import java.util.concurrent.TimeUnit;                                |
| 4   |                                                                      |
| 5   | /\*\*                                                                |
| 6   | \* 多线程的 8 锁                                                     |
| 7   | \* 1、标准访问，请问先打印邮件还是短信？                             |
| 8   | \* 2、邮件方法暂停 4 秒钟，请问先打印邮件还是短信？                  |
| 9   | \* 3、新增一个普通方法 hello()没有同步,请问先打印邮件还是 hello？    |
| 10  | \* 4、两部手机、请问先打印邮件还是短信？                             |
| 11  | \* 5、两个静态同步方法，同一部手机，请问先打印邮件还是短信？         |
| 12  | \*/                                                                  |
| 13  | public class Lock8 {                                                 |
| 14  | public static void main(String[] args) throws InterruptedException { |
| 15  | Phone phone = new Phone();                                           |
| 16  |                                                                      |
| 17  | new Thread(()->{                                                     |
| 18  | try {                                                                |
| 19  | phone.sendEmail();                                                   |
| 20  | } catch (Exception e) {                                              |
| 21  | e.printStackTrace();                                                 |
| 22  | }                                                                    |
| 23  | },"A").start();                                                      |
| 24  |                                                                      |
| 25  | Thread.sleep(200);                                                   |
| 26  |                                                                      |

| 27  | new Thread(()->{                                              |
| --- | ------------------------------------------------------------- |
| 28  | try {                                                         |
| 29  | phone.sendSMS();                                              |
| 30  | } catch (Exception e) {                                       |
| 31  | e.printStackTrace();                                          |
| 32  | }                                                             |
| 33  | },"B").start();                                               |
| 34  | }                                                             |
| 35  | }                                                             |
| 36  |                                                               |
| 37  |                                                               |
| 38  | class Phone{                                                  |
| 39  | public static synchronized void sendEmail() throws Exception{ |
| 40  | try {                                                         |
| 41  | TimeUnit.SECONDS.sleep(4);                                    |
| 42  | } catch (InterruptedException e) {                            |
| 43  | e.printStackTrace();                                          |
| 44  | }                                                             |
| 45  | System.out.println("sendEmail");                              |
| 46  | }                                                             |
| 47  | public static synchronized void sendSMS() throws Exception{   |
| 48  | System.out.println("sendSMS");                                |
| 49  | }                                                             |
| 50  | }                                                             |

结论：被 synchronized 和 static 修饰的方法，锁的对象是类的 class 对象。因为两个同步方法都被 static 修饰了，所以两个方法用的是同一个锁，后调用的方法需要等待先调用的方法。

### 6、两个静态同步方法，2 部手机，请问先打印邮件还是短信？

| 1   | package com.kuang;                                                   |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | import java.util.concurrent.TimeUnit;                                |
| 4   |                                                                      |
| 5   | /\*\*                                                                |
| 6   | \* 多线程的 8 锁                                                     |
| 7   | \* 1、标准访问，请问先打印邮件还是短信？                             |
| 8   | \* 2、邮件方法暂停 4 秒钟，请问先打印邮件还是短信？                  |
| 9   | \* 3、新增一个普通方法 hello()没有同步,请问先打印邮件还是 hello？    |
| 10  | \* 4、两部手机、请问先打印邮件还是短信？                             |
| 11  | \* 5、两个静态同步方法，同一部手机，请问先打印邮件还是短信？         |
| 12  | \* 6、两个静态同步方法，2 部手机，请问先打印邮件还是短信？           |
| 13  | \*/                                                                  |
| 14  | public class Lock8 {                                                 |
| 15  | public static void main(String[] args) throws InterruptedException { |
| 16  | Phone phone = new Phone();                                           |
| 17  | Phone phone2 = new Phone();                                          |
| 18  |                                                                      |
| 19  | new Thread(()->{                                                     |
| 20  | try {                                                                |
| 21  | phone.sendEmail();                                                   |
| 22  | } catch (Exception e) {                                              |
| 23  | e.printStackTrace();                                                 |
| 24  | }                                                                    |
| 25  | },"A").start();                                                      |
| 26  |                                                                      |

| 27  | Thread.sleep(200);                                            |
| --- | ------------------------------------------------------------- |
| 28  |                                                               |
| 29  | new Thread(()->{                                              |
| 30  | try {                                                         |
| 31  | phone2.sendSMS();                                             |
| 32  | } catch (Exception e) {                                       |
| 33  | e.printStackTrace();                                          |
| 34  | }                                                             |
| 35  | },"B").start();                                               |
| 36  | }                                                             |
| 37  | }                                                             |
| 38  |                                                               |
| 39  | class Phone{                                                  |
| 40  | public static synchronized void sendEmail() throws Exception{ |
| 41  | try {                                                         |
| 42  | TimeUnit.SECONDS.sleep(4);                                    |
| 43  | } catch (InterruptedException e) {                            |
| 44  | e.printStackTrace();                                          |
| 45  | }                                                             |
| 46  | System.out.println("sendEmail");                              |
| 47  | }                                                             |
| 48  | public static synchronized void sendSMS() throws Exception{   |
| 49  | System.out.println("sendSMS");                                |
| 50  | }                                                             |
| 51  | }                                                             |

结论：被 synchronized 和 static 修饰的方法，锁的对象是类的 class 对象。因为两个同步方法都被 static 修饰了，即便用了两个不同的对象调用方法，两个方法用的还是同一个锁，后调用的方法需要等待先调用 的方法。

### 7、一个普通同步方法，一个静态同步方法，同一部手机，请问先打印邮件还是短信？

| 1   | package com.kuang;                                                             |
| --- | ------------------------------------------------------------------------------ |
| 2   |                                                                                |
| 3   | import java.util.concurrent.TimeUnit;                                          |
| 4   |                                                                                |
| 5   | /\*\*                                                                          |
| 6   | \* 多线程的 8 锁                                                               |
| 7   | \* 1、标准访问，请问先打印邮件还是短信？                                       |
| 8   | \* 2、邮件方法暂停 4 秒钟，请问先打印邮件还是短信？                            |
| 9   | \* 3、新增一个普通方法 hello()没有同步,请问先打印邮件还是 hello？              |
| 10  | \* 4、两部手机、请问先打印邮件还是短信？                                       |
| 11  | \* 5、两个静态同步方法，同一部手机，请问先打印邮件还是短信？                   |
| 12  | \* 6、两个静态同步方法，2 部手机，请问先打印邮件还是短信？                     |
| 13  | \* 7、一个普通同步方法，一个静态同步方法，同一部手机，请问先打印邮件还是短信？ |
| 14  | \*/                                                                            |
| 15  | public class Lock8 {                                                           |
| 16  | public static void main(String[] args) throws InterruptedException {           |
| 17  | Phone phone = new Phone();                                                     |
| 18  |                                                                                |
| 19  | new Thread(()->{                                                               |
| 20  | try {                                                                          |
| 21  | phone.sendEmail();                                                             |
| 22  | } catch (Exception e) {                                                        |
| 23  | e.printStackTrace();                                                           |
| 24  | }                                                                              |

| 25  | },"A").start();                                               |
| --- | ------------------------------------------------------------- |
| 26  |                                                               |
| 27  | Thread.sleep(200);                                            |
| 28  |                                                               |
| 29  | new Thread(()->{                                              |
| 30  | try {                                                         |
| 31  | phone.sendSMS();                                              |
| 32  | } catch (Exception e) {                                       |
| 33  | e.printStackTrace();                                          |
| 34  | }                                                             |
| 35  | },"B").start();                                               |
| 36  | }                                                             |
| 37  | }                                                             |
| 38  |                                                               |
| 39  |                                                               |
| 40  | class Phone{                                                  |
| 41  | public static synchronized void sendEmail() throws Exception{ |
| 42  | try {                                                         |
| 43  | TimeUnit.SECONDS.sleep(4);                                    |
| 44  | } catch (InterruptedException e) {                            |
| 45  | e.printStackTrace();                                          |
| 46  | }                                                             |
| 47  | System.out.println("sendEmail");                              |
| 48  | }                                                             |
| 49  | public synchronized void sendSMS() throws Exception{          |
| 50  | System.out.println("sendSMS");                                |
| 51  | }                                                             |
| 52  | }                                                             |

结论：被 synchronized 和 static 修饰的方法，锁的对象是类的 class 对象。仅仅被 synchronized 修饰的方法，锁的对象是方法的调用者。因为两个方法锁的对象不是同一个，所以两个方法用的不是同一个锁， 后调用的方法不需要等待先调用的方法。

### 8、一个普通同步方法，一个静态同步方法，2 部手机，请问先打印邮件还是短信？

| 1   | package com.kuang;                                                             |
| --- | ------------------------------------------------------------------------------ |
| 2   |                                                                                |
| 3   | import java.util.concurrent.TimeUnit;                                          |
| 4   |                                                                                |
| 5   | /\*\*                                                                          |
| 6   | \* 多线程的 8 锁                                                               |
| 7   | \* 1、标准访问，请问先打印邮件还是短信？                                       |
| 8   | \* 2、邮件方法暂停 4 秒钟，请问先打印邮件还是短信？                            |
| 9   | \* 3、新增一个普通方法 hello()没有同步,请问先打印邮件还是 hello？              |
| 10  | \* 4、两部手机、请问先打印邮件还是短信？                                       |
| 11  | \* 5、两个静态同步方法，同一部手机，请问先打印邮件还是短信？                   |
| 12  | \* 6、两个静态同步方法，2 部手机，请问先打印邮件还是短信？                     |
| 13  | \* 7、一个普通同步方法，一个静态同步方法，同一部手机，请问先打印邮件还是短信？ |
| 14  | \* 8、一个普通同步方法，一个静态同步方法，2 部手机，请问先打印邮件还是短信？   |
| 15  | \*/                                                                            |
| 16  | public class Lock8 {                                                           |
| 17  | public static void main(String[] args) throws InterruptedException {           |
| 18  | Phone phone = new Phone();                                                     |
| 19  | Phone phone2 = new Phone();                                                    |
| 20  |                                                                                |
| 21  | new Thread(()->{                                                               |

| 22  | try {                                                         |
| --- | ------------------------------------------------------------- |
| 23  | phone.sendEmail();                                            |
| 24  | } catch (Exception e) {                                       |
| 25  | e.printStackTrace();                                          |
| 26  | }                                                             |
| 27  | },"A").start();                                               |
| 28  |                                                               |
| 29  | Thread.sleep(200);                                            |
| 30  |                                                               |
| 31  | new Thread(()->{                                              |
| 32  | try {                                                         |
| 33  | phone2.sendSMS();                                             |
| 34  | } catch (Exception e) {                                       |
| 35  | e.printStackTrace();                                          |
| 36  | }                                                             |
| 37  | },"B").start();                                               |
| 38  | }                                                             |
| 39  | }                                                             |
| 40  |                                                               |
| 41  |                                                               |
| 42  | class Phone{                                                  |
| 43  | public static synchronized void sendEmail() throws Exception{ |
| 44  | try {                                                         |
| 45  | TimeUnit.SECONDS.sleep(4);                                    |
| 46  | } catch (InterruptedException e) {                            |
| 47  | e.printStackTrace();                                          |
| 48  | }                                                             |
| 49  | System.out.println("sendEmail");                              |
| 50  | }                                                             |
| 51  | public synchronized void sendSMS() throws Exception{          |
| 52  | System.out.println("sendSMS");                                |
| 53  | }                                                             |
| 54  | }                                                             |

结论：被 synchronized 和 static 修饰的方法，锁的对象是类的 class 对象。仅仅被 synchronized 修饰的方法，锁的对象是方法的调用者。即便是用同一个对象调用两个方法，锁的对象也不是同一个，所以两个 方法用的不是同一个锁，后调用的方法不需要等待先调用的方法。

小结
new this 具体的一个手机
static class 唯一的一个模板
一个对象里面如果有多个 synchronized 方法，某个时刻内，只要一个线程去调用其中一个 synchronized 方法了，其他的线程都要等待，换句话说，在某个时刻内，只能有唯一一个线程去访问这些 synchronized 方法，锁的是当前对象 this，被锁定后，其他的线程都不能进入到当前对象的其他的 synchronized 方法
加个普通方法后发现和同步锁无关，换成两个对象后，不是同一把锁，情况立刻变化
都换成静态同步方法后，情况又变化了。所有的非静态的同步方法用的都是同一把锁 实例对象本身
synchronized 实现同步的基础：java 中的每一个对象都可以作为锁

具体的表现为以下三种形式：
对于普通同步方法，锁的是当前实例对象
对于静态同步方法，锁的是当前的 Class 对象。
对于同步方法块，锁是 synchronized 括号里面的配置对象
当一个线程试图访问同步代码块时，他首先必须得到锁，退出或者是抛出异常时必须释放锁，也就是说 如果一个实例对象的非静态同步方法获取锁后，该实例对象的其他非静态同步方法必须等待获取锁的方 法释放锁后才能获取锁，可以是别的实例对象非非静态同步方法因为跟该实例对象的非静态同步方法用 的是不同的锁，所以必须等待该实例对象已经获取锁的非静态同步方法释放锁就可以获取他们自己的 锁。
所有的静态同步方法用的也是同一把锁 类对象本身
这两把锁的是两个不同的对象，所以静态的同步方法与非静态的同步方法之间是不会有竞争条件的，但 是一旦一个静态同步方法获取锁后，其他的静态同步方法都必须等待该方法释放锁后才能获取锁，而不 是同一个实例对象的静态同步方法之间，还是不同的实例对象的静态同步方法之间，只要他们用一个的 是同一个类的实例对象。

# 6、集合类不安全

list 不安全
单线程下：

| 1   | package com.kuang;                              |
| --- | ----------------------------------------------- |
| 2   |                                                 |
| 3   | import java.util.ArrayList;                     |
| 4   | import java.util.List;                          |
| 5   | import java.util.UUID;                          |
| 6   |                                                 |
| 7   | // 单线程十分安全                               |
| 8   | public class UnSafeList {                       |
| 9   | public static void main(String[] args) {        |
| 10  | List<String> list = Arrays.asList("a","b","c"); |
| 11  | list.forEach(System.out::println);              |
| 12  | }                                               |
| 13  | }                                               |

多线程下：

| 1   | package com.kuang;                       |
| --- | ---------------------------------------- |
| 2   |                                          |
| 3   | import java.util.ArrayList;              |
| 4   | import java.util.List;                   |
| 5   | import java.util.UUID;                   |
| 6   |                                          |
| 7   | public class UnSafeList {                |
| 8   | public static void main(String[] args) { |
| 9   | List<String> list = new ArrayList<>();   |
| 10  |                                          |
| 11  | // 对比 3 个线程 和 30 个线程，看区别    |
| 12  | for (int i = 1; i <= 30; i++) {          |
| 13  | new Thread(()->{                         |

| 14  |     |     |     | list.add(UUID.randomUUID().toString().substring(0,8)); |
| --- | --- | --- | --- | ------------------------------------------------------ |
| 15  |     |     |     | System.out.println(list);                              |
| 16  |     |     |     | },String.valueOf(i)).start();                          |
| 17  |     |     | }   |                                                        |
| 18  |     |     |     |                                                        |
| 19  |     | }   |     |                                                        |
| 20  | }   |     |     |                                                        |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834815857-030b2480-d4c3-4b6d-ae19-ce57dd4123a8.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834817612-f51b6363-71e4-462a-9cb5-e922c269baed.jpeg#)

| 1   | package com.kuang;                                                     |
| --- | ---------------------------------------------------------------------- |
| 2   |                                                                        |
| 3   | import java.lang.reflect.Array;                                        |
| 4   | import java.util.\*;                                                   |
| 5   | import java.util.concurrent.CopyOnWriteArrayList;                      |
| 6   |                                                                        |
| 7   | /\*\*                                                                  |
| 8   | \* 1 故障现象：ConcurrentModificationException                         |
| 9   | \* 2 导致原因：add 方法没有加锁                                        |
| 10  | \* 3 解决方案：换一个集合类                                            |
| 11  | \* 1、List<String> list = new Vector<>(); JDK1.0 就存在了！            |
| 12  | \* 2、List<String> list = Collections.synchronizedList(new ArrayList<> |
|     | ());                                                                   |
| 13  | \* 3、List<String> list = new CopyOnWriteArrayList<>();                |
| 14  | \*/                                                                    |
| 15  | public class UnSafeList {                                              |
| 16  | public static void main(String[] args) {                               |
| 17  | List<String> list = new CopyOnWriteArrayList<>();                      |
| 18  |                                                                        |
| 19  | for (int i = 1; i <= 30; i++) {                                        |

| 20  |     |     |     | new Thread(()->{                                       |
| --- | --- | --- | --- | ------------------------------------------------------ |
| 21  |     |     |     | list.add(UUID.randomUUID().toString().substring(0,8)); |
| 22  |     |     |     | System.out.println(list);                              |
| 23  |     |     |     | },String.valueOf(i)).start();                          |
| 24  |     |     | }   |                                                        |
| 25  |     |     |     |                                                        |
| 26  |     | }   |     |                                                        |
| 27  | }   |     |     |                                                        |

### 写入时复制（CopyOnWrite）思想

写入时复制（CopyOnWrite，简称 COW）思想是计算机程序设计领域中的一种优化策略。其核心思想是，如果有多个调用者（Callers）同时要求相同的资源（如内存或者是磁盘上的数据存储），他们会共同获取相同的指针指向相同的资源，直到某个调用者视图修改资源内容时，系统才会真正复制一份专用 副本（private copy）给该调用者，而其他调用者所见到的最初的资源仍然保持不变。这过程对其他的调用者都是透明的（transparently）。此做法主要的优点是如果调用者没有修改资源，就不会有副本
（private copy）被创建，因此多个调用者只是读取操作时可以共享同一份资源。
读写分离，写时复制出一个新的数组，完成插入、修改或者移除操作后将新数组赋值给 array

### CopyOnWriteArrayList 为什么并发安全且性能比 Vector 好

我知道 Vector 是增删改查方法都加了 synchronized，保证同步，但是每个方法执行的时候都要去获得
锁，性能就会大大下降，而 CopyOnWriteArrayList 只是在增删改上加锁，但是读不加锁，在读方面的性能就好于 Vector，CopyOnWriteArrayList 支持读多写少的并发情况。

set 不安全
同理

| 1   | package com.kuang;                                                    |
| --- | --------------------------------------------------------------------- |
| 2   |                                                                       |
| 3   | import java.lang.reflect.Array;                                       |
| 4   | import java.util.\*;                                                  |
| 5   | import java.util.concurrent.CopyOnWriteArrayList;                     |
| 6   | import java.util.concurrent.CopyOnWriteArraySet;                      |
| 7   |                                                                       |
| 8   | /\*\*                                                                 |
| 9   | \* 1 故障现象：ConcurrentModificationException                        |
| 10  | \* 2 导致原因：add 方法没有加锁                                       |
| 11  | \* 3 解决方案：换一个集合类                                           |
| 12  | \* 1、Set<String> set = new HashSet<>(); 默认                         |
| 13  | \* 2、Set<String> set = Collections.synchronizedSet(new HashSet<>()); |
| 14  | \* 3、Set<String> set = new CopyOnWriteArraySet();                    |
| 15  | \* 4 优化建议：（同样的错误，不出现第 2 次）                          |
| 16  | \*                                                                    |
| 17  | \*/                                                                   |
| 18  | public class UnSafeList {                                             |
| 19  | public static void main(String[] args) {                              |
| 20  | Set<String> set = new CopyOnWriteArraySet();                          |
| 21  |                                                                       |
| 22  | for (int i = 1; i <= 30; i++) {                                       |
| 23  | new Thread(()->{                                                      |
| 24  | set.add(UUID.randomUUID().toString().substring(0,8));                 |
| 25  | System.out.println(set);                                              |
| 26  | },String.valueOf(i)).start();                                         |

27
28
29
30
}
}
}

hashset 底层就是 hashMap

| 1   | Set<String> set = new HashSet<>();                             |
| --- | -------------------------------------------------------------- |
| 2   | // 点进去                                                      |
| 3   | public HashSet() {                                             |
| 4   | map = new HashMap<>();                                         |
| 5   | }                                                              |
| 6   |                                                                |
| 7   | // add 方法 就是 map 的 put 方法                               |
| 8   | public boolean add(E e) {                                      |
| 9   | return map.put(e, PRESENT)==null;                              |
| 10  | }                                                              |
| 11  |                                                                |
| 12  | //private static final Object PRESENT = new Object(); 不变得值 |

map 不安全
hashMap 底层是数组+链表+红黑树

| 1   | Map<String,String> map = new HashMap<>();        |
| --- | ------------------------------------------------ |
| 2   | // 等价于                                        |
| 3   | Map<String,String> map = new HashMap<>(16,0.75); |
| 4   | // 工作中，常常会自己根据业务来写参数，提高效率  |

map 不安全测试：

| 1   | public static void main(String[] args) {                                    |
| --- | --------------------------------------------------------------------------- |
| 2   | Map<String,String> map = new HashMap<>();                                   |
| 3   | // 人生如程序，不是选择就是循环，时常的自我总结十分的重要                   |
| 4   | for (int i = 1; i <= 30; i++) {                                             |
| 5   | new Thread(()->{                                                            |
| 6   |                                                                             |
|     | map.put(Thread.currentThread().getName(),UUID.randomUUID().toString().subst |
|     | ring(0,8));                                                                 |
| 7   | System.out.println(map);                                                    |
| 8   | },String.valueOf(i)).start();                                               |
| 9   | }                                                                           |
| 10  | }                                                                           |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834818128-9c4d2724-37fe-4468-a803-ec19c609457c.jpeg#)
注意名字：

| 1   | public class UnSafeList {                                                   |
| --- | --------------------------------------------------------------------------- |
| 2   | public static void main(String[] args) {                                    |
| 3   | // Map<String,String> map = new HashMap<>();                                |
| 4   | Map<String,String> map = new ConcurrentHashMap<>();                         |
| 5   | for (int i = 1; i <= 30; i++) {                                             |
| 6   | new Thread(()->{                                                            |
| 7   |                                                                             |
|     | map.put(Thread.currentThread().getName(),UUID.randomUUID().toString().subst |
|     | ring(0,8));                                                                 |
| 8   | System.out.println(map);                                                    |
| 9   | },String.valueOf(i)).start();                                               |
| 10  | }                                                                           |
| 11  | }                                                                           |
| 12  | }                                                                           |

# 7、Callable

多线程中，第 3 种获得多线程的方式，Callable。它与 Runnable 有什么区别呢？ 是否有返回值
是否抛异常
方法不一样，一个是 call，一个是 run
基础入门

| 1   | package com.kuang;                                                |
| --- | ----------------------------------------------------------------- |
| 2   |                                                                   |
| 3   | import java.util.concurrent.Callable;                             |
| 4   | import java.util.concurrent.ExecutionException;                   |
| 5   | import java.util.concurrent.FutureTask;                           |
| 6   |                                                                   |
| 7   | public class CallableDemo {                                       |
| 8   | public static void main(String[] args) throws ExecutionException, |
|     | InterruptedException {                                            |
| 9   | MyThread myThread = new MyThread();                               |
| 10  | FutureTask futureTask = new FutureTask(myThread); // 适配类       |
| 11  |                                                                   |
| 12  | Thread t1 = new Thread(futureTask,"A"); // 调用执行               |
| 13  | t1.start();                                                       |
| 14  |                                                                   |
| 15  | Integer result = (Integer) futureTask.get(); // 获取返回值        |
| 16  | System.out.println(result);                                       |
| 17  | }                                                                 |
| 18  | }                                                                 |
| 19  |                                                                   |
| 20  | class MyThread implements Callable<Integer>{                      |
| 21  | @Override                                                         |
| 22  | public Integer call() throws Exception {                          |
| 23  | System.out.println("call 被调用");                                |
| 24  | return 1024;                                                      |
| 25  | }                                                                 |
| 26  |                                                                   |
| 27  | }                                                                 |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834818581-934b50e0-c7c8-4545-8ef5-ff3643a800f7.jpeg#)

Callable 细节
练武不练功，到头一场空，天下武功没有高低之分，只有习武的人有强弱之别

| 1   | package com.kuang;                                                        |
| --- | ------------------------------------------------------------------------- |
| 2   |                                                                           |
| 3   | import java.util.concurrent.Callable;                                     |
| 4   | import java.util.concurrent.ExecutionException;                           |
| 5   | import java.util.concurrent.FutureTask;                                   |
| 6   | import java.util.concurrent.TimeUnit;                                     |
| 7   |                                                                           |
| 8   | /\*\*                                                                     |
| 9   | \* 1、get 方法获得返回结果! 一般放在最后一行！否则可能会阻塞              |
| 10  | \*/                                                                       |
| 11  | public class CallableDemo {                                               |
| 12  | public static void main(String[] args) throws ExecutionException,         |
|     | InterruptedException {                                                    |
| 13  | MyThread myThread = new MyThread();                                       |
| 14  | FutureTask futureTask = new FutureTask(myThread); // 适配类               |
| 15  |                                                                           |
| 16  | new Thread(futureTask,"A").start(); // 调用执行                           |
| 17  | new Thread(futureTask,"B").start(); // 第二次调用执行，会有结果缓存，不用 |
|     | 再次计算                                                                  |
| 18  |                                                                           |
| 19  | System.out.println(Thread.currentThread().getName()+" OK");               |
| 20  |                                                                           |
| 21  | Integer result = (Integer) futureTask.get(); // 获取返回值                |
| 22  | System.out.println(result);                                               |
| 23  |                                                                           |
| 24  | }                                                                         |
| 25  | }                                                                         |
| 26  |                                                                           |
| 27  | class MyThread implements Callable<Integer>{                              |
| 28  | @Override                                                                 |
| 29  | public Integer call() throws Exception {                                  |
| 30  | System.out.println("call 被调用");                                        |
| 31  | try {                                                                     |
| 32  | TimeUnit.SECONDS.sleep(3);                                                |
| 33  | } catch (InterruptedException e) {                                        |

| 34  |     |     | e.printStackTrace(); |
| --- | --- | --- | -------------------- |
| 35  |     |     | }                    |
| 36  |     |     | return 1024;         |
| 37  |     | }   |                      |
| 38  |     |     |                      |
| 39  | }   |     |                      |

# 8、常用辅助类

## 、CountDownLatch

| 1   | package com.kuang;                                                   |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | import java.util.concurrent.CountDownLatch;                          |
| 4   |                                                                      |
| 5   | public class CountDownLatchDemo {                                    |
| 6   |                                                                      |
| 7   | public static void main(String[] args) throws InterruptedException { |
| 8   | // 计数器                                                            |
| 9   | CountDownLatch countDownLatch = new CountDownLatch(6);               |
| 10  |                                                                      |
| 11  | for (int i = 1; i <= 6; i++) {                                       |
| 12  | new Thread(()->{                                                     |
| 13  | System.out.println(Thread.currentThread().getName()+"                |
|     | Start");                                                             |
| 14  | countDownLatch.countDown(); // 计数器-1                              |
| 15  | },String.valueOf(i)).start();                                        |
| 16  | }                                                                    |
| 17  | //阻塞等待计数器归零                                                 |
| 18  | countDownLatch.await();                                              |
| 19  | System.out.println(Thread.currentThread().getName()+" End");         |
| 20  | }                                                                    |
| 21  |                                                                      |
| 22  | /\*\*                                                                |
| 23  | \* 顺序不一定，结果诡异，达不到预期的最后 End                        |
| 24  | \*/                                                                  |
| 25  | public void test1(){                                                 |
| 26  | for (int i = 1; i <= 6; i++) {                                       |
| 27  | new Thread(()->{                                                     |
| 28  | System.out.println(Thread.currentThread().getName()+"                |
|     | Start");                                                             |
| 29  | },String.valueOf(i)).start();                                        |
| 30  | }                                                                    |
| 31  | System.out.println(Thread.currentThread().getName()+" End");         |
| 32  | }                                                                    |
| 33  | }                                                                    |

### 原理：

CountDownLatch 主要有两个方法，当一个或多个线程调用 await 方法时，这些线程会阻塞其他线程调用 CountDown 方法会将计数器减 1（调用 CountDown 方法的线程不会阻塞）
当计数器变为 0 时，await 方法阻塞的线程会被唤醒，继续执行

## 、CyclicBarrier

翻译：CyclicBarrier 篱栅
作用：和上面的减法相反，这里是加法，好比集齐 7 个龙珠召唤神龙，或者人到齐了再开会！

| 1   | package com.kuang;                                          |
| --- | ----------------------------------------------------------- |
| 2   |                                                             |
| 3   | import java.util.concurrent.BrokenBarrierException;         |
| 4   | import java.util.concurrent.CyclicBarrier;                  |
| 5   |                                                             |
| 6   | public class CyclicBarrierDemo {                            |
| 7   | public static void main(String[] args) {                    |
| 8   | // CyclicBarrier(int parties, Runnable barrierAction)       |
| 9   | CyclicBarrier cyclicBarrier = new CyclicBarrier(7,()->{     |
| 10  | System.out.println("召唤神龙成功");                         |
| 11  | });                                                         |
| 12  |                                                             |
| 13  | for (int i = 1; i <= 7; i++) {                              |
| 14  | final int tempInt = i;                                      |
| 15  | new Thread(()->{                                            |
| 16  | System.out.println(Thread.currentThread().getName()+"收集了 |
|     | 第"+ tempInt +"颗龙珠");                                    |
| 17  | try {                                                       |
| 18  | cyclicBarrier.await(); // 等待                              |
| 19  | } catch (InterruptedException e) {                          |
| 20  | e.printStackTrace();                                        |
| 21  | } catch (BrokenBarrierException e) {                        |
| 22  | e.printStackTrace();                                        |
| 23  | }                                                           |
| 24  | }).start();                                                 |
| 25  | }                                                           |
| 26  |                                                             |
| 27  | }                                                           |
| 28  | }                                                           |

## 、Semaphore

翻译：Semaphore 信号量;信号灯;信号作用：抢车位

| 1   | package com.kuang;                       |
| --- | ---------------------------------------- |
| 2   |                                          |
| 3   | import java.util.concurrent.Semaphore;   |
| 4   | import java.util.concurrent.TimeUnit;    |
| 5   |                                          |
| 6   | /\*\*                                    |
| 7   | \* 信号灯                                |
| 8   | \*/                                      |
| 9   | public class SemaphoreDemo {             |
| 10  | public static void main(String[] args) { |
| 11  | // 模拟资源类，有 3 个空车位             |
| 12  | Semaphore semaphore = new Semaphore(3);  |

| 13  |                                                          |
| --- | -------------------------------------------------------- |
| 14  | for (int i = 1; i <= 6; i++) { // 模拟 6 个车            |
| 15  | new Thread(()->{                                         |
| 16  | try {                                                    |
| 17  | semaphore.acquire(); // acquire 得到                     |
| 18  | System.out.println(Thread.currentThread().getName()+" 抢 |
|     | 到了车位");                                              |
| 19  | TimeUnit.SECONDS.sleep(3); // 停 3 秒钟                  |
| 20  | System.out.println(Thread.currentThread().getName()+" 离 |
|     | 开了车位");                                              |
| 21  | } catch (InterruptedException e) {                       |
| 22  | e.printStackTrace();                                     |
| 23  | } finally {                                              |
| 24  | semaphore.release(); // 释放这个位置                     |
| 25  | }                                                        |
| 26  | },String.valueOf(i)).start();                            |
| 27  | }                                                        |
| 28  |                                                          |
| 29  | }                                                        |
| 30  | }                                                        |

### 原理：

在信号量上我们定义两种操作：
acquire（获取）
当一个线程调用 acquire 操作时，他要么通过成功获取信号量（信号量-1） 要么一直等下去，直到有线程释放信号量，或超时
release （释放）
实际上会将信号量的值 + 1，然后唤醒等待的线程。
（release）
信号量主要用于两个目的：一个是用于多个共享资源的互斥使用，另一个用于并发线程数的控制。

# 9、读写锁

练武不练功，到老一场空！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834819203-6f4b4f0e-faa0-4563-ba8c-93bd589e06ad.jpeg#)
独占锁（写锁）：指该锁一次只能被一个线程锁持有。对于 ReentranrLock 和 Synchronized 而言都是独占锁。
共享锁（读锁）：该锁可被多个线程所持有。
对于 ReentrantReadWriteLock 其读锁时共享锁，写锁是独占锁，读锁的共享锁可保证并发读是非常高效 的。

测试：

| 1   | package com.kuang;                                                                          |
| --- | ------------------------------------------------------------------------------------------- |
| 2   |                                                                                             |
| 3   |                                                                                             |
| 4   | import java.util.HashMap;                                                                   |
| 5   | import java.util.Map;                                                                       |
| 6   | import java.util.concurrent.locks.ReadWriteLock;                                            |
| 7   | import java.util.concurrent.locks.ReentrantReadWriteLock;                                   |
| 8   |                                                                                             |
| 9   | /\*\*                                                                                       |
| 10  | \* 多个线程同时读一个资源类没有任何问题，所以为了满足并发量，读取共享资源应该可以同时进行。 |
| 11  | \* 但是，如果有一个线程想去写共享资源，就不应该再有其他线程可以对该资源进行读或写。         |
| 12  | \* 1. 读-读 可以共存                                                                        |
| 13  | \* 2. 读-写 不能共存                                                                        |
| 14  | \* 3. 写-写 不能共存                                                                        |
| 15  | \*/                                                                                         |
| 16  | public class ReadWriteLockDemo {                                                            |
| 17  | public static void main(String[] args) {                                                    |
| 18  | MyCacheLock myCache = new MyCacheLock();                                                    |
| 19  | // 写                                                                                       |
| 20  | for (int i = 1; i <= 5; i++) {                                                              |
| 21  | final int tempInt = i;                                                                      |
| 22  | new Thread(()->{                                                                            |
| 23  | myCache.put(tempInt+"",tempInt+"");                                                         |
| 24  | },String.valueOf(i)).start();                                                               |
| 25  | }                                                                                           |

| 26  |                                                                        |
| --- | ---------------------------------------------------------------------- |
| 27  | // 读                                                                  |
| 28  | for (int i = 1; i <= 5; i++) {                                         |
| 29  | final int tempInt = i;                                                 |
| 30  | new Thread(()->{                                                       |
| 31  | myCache.get(tempInt+"");                                               |
| 32  | },String.valueOf(i)).start();                                          |
| 33  | }                                                                      |
| 34  | }                                                                      |
| 35  | }                                                                      |
| 36  |                                                                        |
| 37  | // 测试发现问题: 写入的时候，还没写入完成，会存在其他的写入！造成问题  |
| 38  | class MyCache{                                                         |
| 39  | private volatile Map<String,Object> map = new HashMap<>();             |
| 40  |                                                                        |
| 41  | public void put(String key,Object value){                              |
| 42  | System.out.println(Thread.currentThread().getName()+" 写入"+key);      |
| 43  | map.put(key,value);                                                    |
| 44  | System.out.println(Thread.currentThread().getName()+" 写入成功!");     |
| 45  | }                                                                      |
| 46  |                                                                        |
| 47  | public void get(String key){                                           |
| 48  | System.out.println(Thread.currentThread().getName()+" 读取"+key);      |
| 49  | Object result = map.get(key);                                          |
| 50  | System.out.println(Thread.currentThread().getName()+" 读取结           |
|     | 果："+result);                                                         |
| 51  | }                                                                      |
| 52  | }                                                                      |
| 53  |                                                                        |
| 54  | // 加锁                                                                |
| 55  | class MyCacheLock{                                                     |
| 56  | private volatile Map<String,Object> map = new HashMap<>();             |
| 57  | private ReadWriteLock readWriteLock = new ReentrantReadWriteLock(); // |
|     | 读写锁                                                                 |
| 58  |                                                                        |
| 59  | public void put(String key,Object value){                              |
| 60  | // 写锁                                                                |
| 61  | readWriteLock.writeLock().lock();                                      |
| 62  | try {                                                                  |
| 63  | System.out.println(Thread.currentThread().getName()+" 写入"+key);      |
| 64  | map.put(key,value);                                                    |
| 65  | System.out.println(Thread.currentThread().getName()+" 写入成           |
|     | 功!");                                                                 |
| 66  | } catch (Exception e) {                                                |
| 67  | e.printStackTrace();                                                   |
| 68  | } finally {                                                            |
| 69  | readWriteLock.writeLock().unlock();                                    |
| 70  | }                                                                      |
| 71  | }                                                                      |
| 72  |                                                                        |
| 73  | public void get(String key){                                           |
| 74  | // 读锁                                                                |
| 75  | readWriteLock.readLock().lock();                                       |
| 76  | try {                                                                  |
| 77  | System.out.println(Thread.currentThread().getName()+" 读取"+key);      |
| 78  | Object result = map.get(key);                                          |
| 79  | System.out.println(Thread.currentThread().getName()+" 读取结           |
|     | 果："+result);                                                         |

| 80 |

} |

} | } catch (Exception e) { e.printStackTrace();
} finally {
readWriteLock.readLock().unlock();
} |
| --- | --- | --- | --- |
| 81 | | | |
| 82 | | | |
| 83 | | | |
| 84 | | | |
| 85 | | | |
| 86 | | | |

# 10、阻塞队列

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834819939-49dffc28-39f5-4b9d-a555-5f13e7d24a74.jpeg#)
阻塞队列

### 阻塞：必须要阻塞、不得不阻塞

阻塞队列是一个队列，在数据结构中起的作用如下图：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834820468-7e2928fb-f4f9-484d-b79c-8dcaf10ba074.png#)

当队列是空的，从队列中获取元素的操作将会被阻塞。当队列是满的，从队列中添加元素的操作将会被阻塞。
试图从空的队列中获取元素的线程将会被阻塞，直到其他线程往空的队列插入新的元素。

试图向已满的队列中添加新元素的线程将会被阻塞，直到其他线程从队列中移除一个或多个元素或者完 全清空，使队列变得空闲起来并后续新增。

### 阻塞队列的用处：

在多线程领域：所谓阻塞，在某些情况下会挂起线程（即阻塞），一旦条件满足，被挂起的线程又会自 动被唤起。
为什么需要 BlockingQueue？
好处是我们不需要关心什么时候需要阻塞线程，什么时候需要唤醒线程，因为这一切 BlockingQueue 都给你一手包办了。
在 concurrent 包发布以前，在多线程环境下，我们每个程序员都必须自己去控制这些细节，尤其还要兼顾效率和线程安全，而这会给我们的程序带来不小的复杂度。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834820991-61c4f9a3-864c-4c47-8e5e-df0b1c10f2dc.jpeg#)
接口架构图

### ArrayBlockingQueue： 由 数 组 结 构 组 成 的 有 界 阻 塞 队 列 。 LinkedBlockingQueue：由链表结构组成的有界（默认值为：integer.MAX_VALUE）阻塞队列。

PriorityBlockingQueue：支持优先级排序的无界阻塞队列 DelayQueue：使用优先级队列实现的延迟无界阻塞队列。**SynchronousQueue：不存储元素的阻塞队列，也即单个元素的队列。**LinkedTransferQueue：由链表组成的无界阻塞队列。LinkedBlockingDeque：由链表组成的双向阻塞队列。

API 的使用
常用 API：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834821236-7b074c58-906f-49d7-a85a-6bc6b30f5dc1.jpeg#)
**尽量按组匹配使用**
解释：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834821562-08373340-d058-4fd2-bd24-ac9320bbff21.jpeg#)

### 代码测试（抛出异常）：

| 1   | package com.kuang;                                              |
| --- | --------------------------------------------------------------- |
| 2   |                                                                 |
| 3   | import java.util.concurrent.ArrayBlockingQueue;                 |
| 4   |                                                                 |
| 5   | public class BlockingQueueDemo {                                |
| 6   | public static void main(String[] args) {                        |
| 7   | // 队列大小                                                     |
| 8   | ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3); |
| 9   |                                                                 |
| 10  | System.out.println(blockingQueue.add("a"));                     |
| 11  | System.out.println(blockingQueue.add("b"));                     |
| 12  | System.out.println(blockingQueue.add("c"));                     |
| 13  | System.out.println(blockingQueue.add("d")); //                  |
|     | java.lang.IllegalStateException: Queue full                     |
| 14  |                                                                 |
| 15  | }                                                               |
| 16  | }                                                               |

| 1   | package com.kuang;                                                 |
| --- | ------------------------------------------------------------------ |
| 2   |                                                                    |
| 3   | import java.util.concurrent.ArrayBlockingQueue;                    |
| 4   |                                                                    |
| 5   | public class BlockingQueueDemo {                                   |
| 6   | public static void main(String[] args) {                           |
| 7   | // 队列大小                                                        |
| 8   | ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);    |
| 9   |                                                                    |
| 10  | System.out.println(blockingQueue.add("a"));                        |
| 11  | System.out.println(blockingQueue.add("b"));                        |
| 12  | System.out.println(blockingQueue.add("c"));                        |
| 13  |                                                                    |
| 14  | System.out.println(blockingQueue.element()); // 检测队列队首元素！ |
| 15  |                                                                    |
| 16  | // public E remove() 返回值 E，就是移除的值                        |
| 17  | System.out.println(blockingQueue.remove()); //a                    |
| 18  | System.out.println(blockingQueue.remove()); //b                    |
| 19  | System.out.println(blockingQueue.remove()); //c                    |
| 20  |                                                                    |
| 21  | System.out.println(blockingQueue.remove()); //                     |
|     | java.util.NoSuchElementException                                   |
| 22  |                                                                    |
| 23  | }                                                                  |
| 24  | }                                                                  |

**代码测试（返回特殊值）：**

| 1 | public class BlockingQueueDemo { public static void main(String[]
// 队列大小 |
args) |
{ |
| --- | --- | --- | --- |
| 2 | | | |
| 3 | | | |

| 4   |     |     | ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3); |
| --- | --- | --- | --------------------------------------------------------------- |
| 5   |     |     |                                                                 |
| 6   |     |     | System.out.println(blockingQueue.offer("a")); // true           |
| 7   |     |     | System.out.println(blockingQueue.offer("b")); // true           |
| 8   |     |     | System.out.println(blockingQueue.offer("c")); // true           |
| 9   |     |     | //System.out.println(blockingQueue.offer("d")); // false        |
| 10  |     |     |                                                                 |
| 11  |     |     | System.out.println(blockingQueue.peek()); // 检测队列队首元素！ |
| 12  |     |     |                                                                 |
| 13  |     |     | // public E poll()                                              |
| 14  |     |     | System.out.println(blockingQueue.poll()); // a                  |
| 15  |     |     | System.out.println(blockingQueue.poll()); // b                  |
| 16  |     |     | System.out.println(blockingQueue.poll()); // c                  |
| 17  |     |     | System.out.println(blockingQueue.poll()); // null               |
| 18  |     |     |                                                                 |
| 19  |     | }   |                                                                 |
| 20  | }   |     |                                                                 |

### 代码测试（一直阻塞）：

| 1   | public class BlockingQueueDemo {                                     |
| --- | -------------------------------------------------------------------- |
| 2   | public static void main(String[] args) throws InterruptedException { |
| 3   | // 队列大小                                                          |
| 4   | ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);      |
| 5   |                                                                      |
| 6   | // 一直阻塞                                                          |
| 7   | blockingQueue.put("a");                                              |
| 8   | blockingQueue.put("b");                                              |
| 9   | blockingQueue.put("c");                                              |
| 10  | // blockingQueue.put("d");                                           |
| 11  |                                                                      |
| 12  | System.out.println(blockingQueue.take()); // a                       |
| 13  | System.out.println(blockingQueue.take()); // b                       |
| 14  | System.out.println(blockingQueue.take()); // c                       |
| 15  | System.out.println(blockingQueue.take()); // 阻塞不停止等待          |
| 16  | }                                                                    |
| 17  | }                                                                    |

**代码测试（超时退出）：**

| 1   | public class BlockingQueueDemo {                                     |
| --- | -------------------------------------------------------------------- |
| 2   | public static void main(String[] args) throws InterruptedException { |
| 3   | // 队列大小                                                          |
| 4   | ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);      |
| 5   |                                                                      |
| 6   | // 一直阻塞                                                          |
| 7   | blockingQueue.offer("a");                                            |
| 8   | blockingQueue.offer("b");                                            |
| 9   | blockingQueue.offer("c");                                            |
| 10  | blockingQueue.offer("d",3L,TimeUnit.SECONDS); // 等待 3 秒超时退出   |
| 11  |                                                                      |
| 12  | System.out.println(blockingQueue.poll()); // a                       |
| 13  | System.out.println(blockingQueue.poll()); // b                       |
| 14  | System.out.println(blockingQueue.poll()); // c                       |
| 15  | System.out.println(blockingQueue.poll(3L,TimeUnit.SECONDS)); // 阻塞 |
|     | 不停止等待                                                           |
| 16  | }                                                                    |
| 17  | }                                                                    |

SynchronousQueue 同步队列
SynchronousQueue 没有容量。
与其他的 BlockingQueue 不同，SynchronousQueue 是一个不存储元素的 BlockingQueue 。每一个 put 操作必须要等待一个 take 操作，否则不能继续添加元素，反之亦然。

| 1   | package com.kuang;                                                         |
| --- | -------------------------------------------------------------------------- |
| 2   |                                                                            |
| 3   | import jdk.nashorn.internal.ir.Block;                                      |
| 4   |                                                                            |
| 5   | import java.util.concurrent.BlockingQueue;                                 |
| 6   | import java.util.concurrent.SynchronousQueue;                              |
| 7   | import java.util.concurrent.TimeUnit;                                      |
| 8   |                                                                            |
| 9   | public class SynchronousQueueDemo {                                        |
| 10  | public static void main(String[] args) {                                   |
| 11  | BlockingQueue<String> blockingQueue = new SynchronousQueue<>();            |
| 12  |                                                                            |
| 13  | new Thread(()->{                                                           |
| 14  | try {                                                                      |
| 15  | System.out.println(Thread.currentThread().getName()+" put                  |
|     | 1");                                                                       |
| 16  | blockingQueue.put("1");                                                    |
| 17  | System.out.println(Thread.currentThread().getName()+" put                  |
|     | 2");                                                                       |
| 18  | blockingQueue.put("2");                                                    |
| 19  | System.out.println(Thread.currentThread().getName()+" put                  |
|     | 3");                                                                       |
| 20  | blockingQueue.put("3");                                                    |
| 21  | } catch (InterruptedException e) {                                         |
| 22  | e.printStackTrace();                                                       |
| 23  | }                                                                          |
| 24  | },"T1").start();                                                           |
| 25  |                                                                            |
| 26  | new Thread(()->{                                                           |
| 27  | try {                                                                      |
| 28  | TimeUnit.SECONDS.sleep(3);                                                 |
| 29  |                                                                            |
|     | System.out.println(Thread.currentThread().getName()+blockingQueue.take()); |
| 30  | TimeUnit.SECONDS.sleep(3);                                                 |
| 31  |                                                                            |
|     | System.out.println(Thread.currentThread().getName()+blockingQueue.take()); |
| 32  | TimeUnit.SECONDS.sleep(3);                                                 |
| 33  |                                                                            |
|     | System.out.println(Thread.currentThread().getName()+blockingQueue.take()); |
| 34  | } catch (InterruptedException e) {                                         |
| 35  | e.printStackTrace();                                                       |
| 36  | }                                                                          |
| 37  | },"T1").start();                                                           |
| 38  | }                                                                          |
| 39  | }                                                                          |

# 11、线程池

池化技术
程序的运行，其本质上，是对系统资源(CPU、内存、磁盘、网络等等)的使用。如何高效的使用这些资源是我们编程优化演进的一个方向。今天说的线程池就是一种对 CPU 利用的优化手段。
通过学习线程池原理，明白所有池化技术的基本设计思路。遇到其他相似问题可以解决。

### 池化技术

前面提到一个名词——池化技术，那么到底什么是池化技术呢 ?
池化技术简单点来说，就是提前保存大量的资源，以备不时之需。在机器资源有限的情况下，使用池化 技术可以大大的提高资源的利用率，提升性能等。
在编程领域，比较典型的池化技术有：

### 线程池、连接池、内存池、对象池等。

主要来介绍一下其中比较简单的线程池的实现原理，希望读者们可以举一反三，通过对线程池的理解， 学习并掌握所有编程中池化技术的底层原理。
我们通过创建一个线程对象，并且实现 Runnable 接口就可以实现一个简单的线程。可以利用上多核
CPU。当一个任务结束，当前线程就接收。
但很多时候，我们不止会执行一个任务。如果每次都是如此的创建线程->执行任务->销毁线程，会造成 很大的性能开销。
那能否一个线程创建后，执行完一个任务后，又去执行另一个任务，而不是销毁。这就是线程池。
这也就是池化技术的思想，通过预先创建好多个线程，放在池中，这样可以在需要使用线程的时候直接 获取，避免多次重复创建、销毁带来的开销。

为什么使用线程池
10 年前单核 CPU 电脑，假的多线程，像马戏团小丑玩多个球 ，CPU 需要来回切换。现在是多核电脑，多个线程各自跑在独立的 CPU 上，不用切换效率高。

### 线程池的优势：

线程池做的工作主要是：控制运行的线程数量，处理过程中将任务放入队列，然后在线程创建后启动这 些任务，如果线程数量超过了最大数量，超出数量的线程排队等候，等其他线程执行完毕，再从队列中 取出任务来执行。
它的主要特点为：线程复用，控制最大并发数，管理线程。
第一：降低资源消耗，通过重复利用已创建的线程降低线程创建和销毁造成的消耗。 第二：提高响应速度。当任务到达时，任务可以不需要等待线程创建就能立即执行。
第三：提高线程的可管理性，线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系 统的稳定性，使用线程池可以进行统一分配，调优和监控。

线程池的三大方法
Java 中的线程池是通过 Executor 框架实现的，该框架中用到了 Executor ，Executors，
ExecutorService，ThreadPoolExecutor 这几个类。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834822072-eebdb6f7-1e73-4e55-a1bb-60c0a3846910.jpeg#)

### 三大方法说明：

Executors.newFixedThreadPool(int)
执行长期任务性能好，创建一个线程池，一池有 N 个固定的线程，有固定线程数的线程。

| 1   | public class MyThreadPoolDemo {                                      |
| --- | -------------------------------------------------------------------- |
| 2   | public static void main(String[] args) {                             |
| 3   | // 池子大小 5                                                        |
| 4   | ExecutorService threadPool =                                         |
|     | Executors.newFixedThreadPool(5);                                     |
| 5   |                                                                      |
| 6   | try {                                                                |
| 7   | // 模拟有 10 个顾客过来银行办理业务，池子中只有 5 个工作人员受理业务 |
| 8   | for (int i = 1; i <= 10; i++) {                                      |
| 9   | threadPool.execute(()->{                                             |
| 10  |                                                                      |
|     | System.out.println(Thread.currentThread().getName()+" 办理业务");    |
| 11  | });                                                                  |
| 12  | }                                                                    |
| 13  | } catch (Exception e) {                                              |
| 14  | e.printStackTrace();                                                 |
| 15  | } finally {                                                          |
| 16  | threadPool.shutdown(); // 用完记得关闭                               |
| 17  | }                                                                    |
| 18  |                                                                      |
| 19  | }                                                                    |
| 20  | }                                                                    |

Executors.newSingleThreadExecutor()

只有一个线程

| 1   | public class MyThreadPoolDemo {                                      |
| --- | -------------------------------------------------------------------- |
| 2   | public static void main(String[] args) {                             |
| 3   | // 有且只有一个固定的线程                                            |
| 4   | ExecutorService threadPool =                                         |
|     | Executors.newSingleThreadExecutor();                                 |
| 5   |                                                                      |
| 6   | try {                                                                |
| 7   | // 模拟有 10 个顾客过来银行办理业务，池子中只有 1 个工作人员受理业务 |
| 8   | for (int i = 1; i <= 10; i++) {                                      |
| 9   | threadPool.execute(()->{                                             |
| 10  |                                                                      |
|     | System.out.println(Thread.currentThread().getName()+" 办理业务");    |
| 11  | });                                                                  |
| 12  | }                                                                    |
| 13  | } catch (Exception e) {                                              |
| 14  | e.printStackTrace();                                                 |
| 15  | } finally {                                                          |
| 16  | threadPool.shutdown(); // 用完记得关闭                               |
| 17  | }                                                                    |
| 18  |                                                                      |
| 19  | }                                                                    |
| 20  | }                                                                    |

Executors.newCachedThreadPool();
执行很多短期异步任务，线程池根据需要创建新线程，但在先构建的线程可用时将重用他们。 可扩容，遇强则强

| 1   | public class MyThreadPoolDemo {                                      |
| --- | -------------------------------------------------------------------- |
| 2   | public static void main(String[] args) {                             |
| 3   | // 一池 N 线程，可扩容伸缩                                           |
| 4   | ExecutorService threadPool =                                         |
|     | Executors.newCachedThreadPool();                                     |
| 5   |                                                                      |
| 6   | try {                                                                |
| 7   | // 模拟有 10 个顾客过来银行办理业务，池子中只有 N 个工作人员受理业务 |
| 8   | for (int i = 1; i <= 10; i++) {                                      |
| 9   | // 模拟延时看效果                                                    |
| 10  | // try {                                                             |
| 11  | // TimeUnit.SECONDS.sleep(1);                                        |
| 12  | // } catch (InterruptedException e) {                                |
| 13  | // e.printStackTrace();                                              |
| 14  | // }                                                                 |
| 15  | threadPool.execute(()->{                                             |
| 16  |                                                                      |
|     | System.out.println(Thread.currentThread().getName()+" 办理业务");    |
| 17  | });                                                                  |
| 18  | }                                                                    |
| 19  | } catch (Exception e) {                                              |
| 20  | e.printStackTrace();                                                 |
| 21  | } finally {                                                          |
| 22  | threadPool.shutdown(); // 用完记得关闭                               |
| 23  | }                                                                    |
| 24  |                                                                      |
| 25  | }                                                                    |
| 26  | }                                                                    |

ThreadPoolExecutor 七大参数
操作：查看三大方法的底层源码，发现本质都是调用了
new ThreadPoolExecutor ( 7 大参数 )

| 1   | // 源码 |                                                   |                                       |
| --- | ------- | ------------------------------------------------- | ------------------------------------- | --- | --------------------- | --- | ---------------- |
| 2   | public  |                                                   | ThreadPoolExecutor(int corePoolSize,  |
| 3   |         |                                                   | int maximumPoolSize,                  |
| 4   |         |                                                   | long keepAliveTime,                   |
| 5   |         |                                                   | TimeUnit unit,                        |
| 6   |         |                                                   | BlockingQueue<Runnable> workQueue,    |
| 7   |         |                                                   | ThreadFactory threadFactory,          |
| 8   |         |                                                   | RejectedExecutionHandler handler) {   |
| 9   | if      |                                                   | (corePoolSize < 0                     |     |                       |
| 10  |         |                                                   | maximumPoolSize <= 0                  |     |                       |
| 11  |         |                                                   | maximumPoolSize < corePoolSize        |     |                       |
| 12  |         |                                                   | keepAliveTime < 0)                    |
| 13  |         |                                                   | throw new IllegalArgumentException(); |
| 14  | if      |                                                   | (workQueue == null                    |     | threadFactory == null |     | handler == null) |
| 15  |         |                                                   | throw new NullPointerException();     |
| 16  |         | this.acc = System.getSecurityManager() == null ?  |                                       |
| 17  |         | null :                                            |                                       |
| 18  |         | AccessController.getContext();                    |                                       |
| 19  |         | this.corePoolSize = corePoolSize;                 |                                       |
| 20  |         | this.maximumPoolSize = maximumPoolSize;           |                                       |
| 21  |         | this.workQueue = workQueue;                       |                                       |
| 22  |         | this.keepAliveTime = unit.toNanos(keepAliveTime); |                                       |
| 23  |         | this.threadFactory = threadFactory;               |                                       |
| 24  |         | this.handler = handler;                           |                                       |
| 25  | }       |                                                   |                                       |

### 参数理解：

：核心线程数。在创建了线程池后，线程中没有任何线程，等到有任务到来时才创建
corePollSize
线程去执行任务。默认情况下，在创建了线程池后，线程池中的线程数为 0，当有任务来之后，就会创建 一个线程去执行任务，当线程池中的线程数目达到 corePoolSize 后，就会把到达的任务放到缓存队列当中。
：最大线程数。表明线程中最多能够创建的线程数量，此值必须大于等于 1。
maximumPoolSize
：空闲的线程保留的时间。
keepAliveTime
：空闲线程的保留时间单位。
TimeUnit

| 1   | TimeUnit.DAYS;         | //天   |
| --- | ---------------------- | ------ |
| 2   | TimeUnit.HOURS;        | //小时 |
| 3   | TimeUnit.MINUTES;      | //分钟 |
| 4   | TimeUnit.SECONDS;      | //秒   |
| 5   | TimeUnit.MILLISECONDS; | //毫秒 |
| 6   | TimeUnit.MICROSECONDS; | //微妙 |
| 7   | TimeUnit.NANOSECONDS;  | //纳秒 |

：阻塞队列，存储等待执行的任务。参数有 ArrayBlockingQueue、
BlockingQueue< Runnable>
LinkedBlockingQueue、SynchronousQueue 可选。
：线程工厂，用来创建线程，一般默认即可
ThreadFactory

：队列已满，而且任务量大于最大线程的异常处理策略。有以下取值
RejectedExecutionHandler

| 1   | ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出 RejectedExecutionException 异常。    |
| --- | ---------------------------------------------------------------------------------- |
| 2   | ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。                   |
| 3   | ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务 |
|     | （重复此过程）                                                                     |
| 4   | ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务                          |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834822719-c9253707-7484-48e9-9faf-74fdd242d478.jpeg#)
ThreadPoolExecutor 底层工作原理

### 举例：8 个人进银行办理业务

1、1~2 人被受理（核心大小 core）
2、3~5 人进入队列（Queue）
3、6~8 人到最大线程池（扩容大小 max）
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834823144-72f9805d-8614-48d7-a65a-fc587c13e217.jpeg#)4、再有人进来就要被拒绝策略接受了

1. 在创建了线程池后，开始等待请求。
1. 当调用 execute()方法添加一个请求任务时，线程池会做出如下判断：
   1. 如果正在运行的线程数量小于 corePoolSize，那么马上创建线程运行这个任务：
   1. 如果正在运行的线程数量大于或等于 corePoolSize，那么将这个任务放入队列：
   1. 如果这个时候队列满了且正在运行的线程数量还小于 maximumPoolSize，那么还是要创建非 核心线程立刻运行这个任务;
   1. 如果队列满了且正在运行的线程数量大于或等于 1Size，那么线程池会启动饱和拒绝策略来执行。
1. 当一个线程完成任务时，它会从队列中取下一个任务来执行。
1. 当一个线程无事可做超过一定的时间(keepA1iveTime)时，线程会判断： 如果当前运行的线程数大于 coreP 佣 1Size，那么这个线程就被停掉。

所以线程池的所有任务完成后，它最终会收缩到 corePoolSize 的大小。

线程池用哪个？生产中如何设置合理参数

### 在工作中单一的/固定数的/可变的三种创建线程池的方法哪个用的多? 坑

答案是一个都不用，我们工作中只能使用自定义的；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834823696-a73979e5-9daf-4bd3-969c-599618d09ce7.jpeg#)Executors 中 JDK 已经给你提供了，为什么不用？
代码测试

### 线程池的拒绝策略：

| 1   | RejectedExecutionHandler rejected = null;                                          |
| --- | ---------------------------------------------------------------------------------- |
| 2   | rejected = new ThreadPoolExecutor.AbortPolicy();//默认，队列满了丢任务，抛出异常   |
| 3   | rejected = new ThreadPoolExecutor.DiscardPolicy();//队列满了丢任务，不抛出异常【如 |
|     | 果允许任务丢失这是最好的】                                                         |
| 4   | rejected = new ThreadPoolExecutor.DiscardOldestPolicy();//将最早进入队列的任务     |
|     | 删，之后再尝试加入队列                                                             |
| 5   | rejected = new ThreadPoolExecutor.CallerRunsPolicy();//如果添加到线程池失败，那么  |
|     | 主线程会自己去执行该任务，回退                                                     |

测试代码

public class MyThreadPoolDemo {
public static void main(String[] args) {

// 获得 CPU 的内核数
System.out.println(Runtime.getRuntime().availableProcessors());

// 自定义 ThreadPoolExecutor
1
2
3
4
5
6
7

| 8   | ExecutorService threadPool = new ThreadPoolExecutor(      |
| --- | --------------------------------------------------------- |
| 9   | 2,                                                        |
| 10  | 5,                                                        |
| 11  | 2L,                                                       |
| 12  | TimeUnit.SECONDS,                                         |
| 13  | new LinkedBlockingDeque<>(3),                             |
| 14  | Executors.defaultThreadFactory(),                         |
| 15  | new ThreadPoolExecutor.DiscardPolicy());                  |
| 16  |                                                           |
| 17  | try {                                                     |
| 18  | // 模拟有 6,7,8,9,10 个顾客过来银行办理业务，观察结果情况 |
| 19  | // 最大容量为：maximumPoolSize + workQueue = 最大容量数   |
| 20  | for (int i = 1; i <= 19; i++) {                           |
| 21  | threadPool.execute(()->{                                  |
| 22  | System.out.println(Thread.currentThread().getName()+" 办  |
|     | 理业务");                                                 |
| 23  | });                                                       |
| 24  | }                                                         |
| 25  | } catch (Exception e) {                                   |
| 26  | e.printStackTrace();                                      |
| 27  | } finally {                                               |
| 28  | threadPool.shutdown(); // 用完记得关闭                    |
| 29  | }                                                         |
| 30  |                                                           |
| 31  | }                                                         |
| 32  | }                                                         |

### 思考题：线程是否越多越好？

一个计算为主的程序（专业一点称为**CPU 密集型程序**）。多线程跑的时候，可以充分利用起所有的 cpu 核心，比如说 4 个核心的 cpu,开 4 个线程的时候，可以同时跑 4 个线程的运算任务，此时是最大效率。
但是如果线程远远超出 cpu 核心数量 反而会使得任务效率下降，因为频繁的切换线程也是要消耗时间的。
因此对于 cpu 密集型的任务来说，线程数等于 cpu 数是最好的了。
如果是一个磁盘或网络为主的程序（**IO 密集型**）。一个线程处在 IO 等待的时候，另一个线程还可以在 CPU 里面跑，有时候 CPU 闲着没事干，所有的线程都在等着 IO，这时候他们就是同时的了，而单线程的话此时还是在一个一个等待的。我们都知道 IO 的速度比起 CPU 来是慢到令人发指的。所以开多线程，比方说多线程网络传输，多线程往不同的目录写文件，等等。
此时 线程数等于 IO 任务数是最佳的。

# 12、四大函数式接口

java.util.function , Java 内置核心四大函数式接口，可以使用 lambda 表达式

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834824259-8817f152-dc05-488f-9a7a-92846b422a20.jpeg#)

函数型接口，有一个输入，有一个输出

| 1   | public static void main(String[] args) {                              |     |
| --- | --------------------------------------------------------------------- | --- |
| 2   | // 函数式接口，可以改为 lambda 表达式                                 |     |
| 3   | //Function<String,Integer> function = new Function<String, Integer>() | {   |
| 4   | // @Override                                                          |     |
| 5   | // public Integer apply(String s) {                                   |     |
| 6   | // return 1024;                                                       |     |
| 7   | // }                                                                  |     |
| 8   | //};                                                                  |     |
| 9   |                                                                       |     |
| 10  | // 简写                                                               |     |
| 11  | Function<String,Integer> function = s->{return s.length();};          |     |
| 12  | System.out.println(function.apply("abc"));                            |     |
| 13  |                                                                       |     |
| 14  | }                                                                     |     |

断定型接口，有一个输入参数，返回只有布尔值。

| 1   | public static void main(String[] args) {                  |
| --- | --------------------------------------------------------- |
| 2   |                                                           |
| 3   | //Predicate<String> predicate = new Predicate<String>() { |
| 4   | // @Override                                              |
| 5   | // public boolean test(String s) {                        |
| 6   | // return false;                                          |
| 7   | // }                                                      |
| 8   | //};                                                      |
| 9   |                                                           |
| 10  | // 简写                                                   |
| 11  | Predicate<String> predicate = s -> {return s.isEmpty();}; |
| 12  | System.out.println(predicate.test("abc"));                |
| 13  |                                                           |
| 14  | }                                                         |

消费型接口，有一个输入参数，没有返回值

| 1   | public static void main(String[] args) {                    |     |
| --- | ----------------------------------------------------------- | --- |
| 2   |                                                             |     |
| 3   | // Consumer<String> consumer = new Consumer<String>()       | {   |
| 4   | // @Override                                                |     |
| 5   | // public void accept(String s) {                           |     |
| 6   | //                                                          |     |
| 7   | // }                                                        |     |
| 8   | // };                                                       |     |
| 9   |                                                             |     |
| 10  | // 简写                                                     |     |
| 11  | Consumer<String> consumer = s -> { System.out.println(s);}; |     |
| 12  | consumer.accept("abc");                                     |     |
| 13  |                                                             |     |
| 14  | }                                                           |     |

供给型接口，没有输入参数，只有返回参数

| 1   | public static void main(String[] args) {              |     |
| --- | ----------------------------------------------------- | --- |
| 2   |                                                       |     |
| 3   | // Supplier<String> supplier = new Supplier<String>() | {   |
| 4   | // @Override                                          |     |
| 5   | // public String get() {                              |     |
| 6   | // return null;                                       |     |
| 7   | // }                                                  |     |
| 8   | // };                                                 |     |
| 9   | Supplier<String> supplier = ()->{return "abc";};      |     |
| 10  | System.out.println(supplier.get());                   |     |
| 11  |                                                       |     |
| 12  | }                                                     |     |

# 13、Stream 流式计算

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834824831-cec63f73-3840-483c-83a9-077b57c3e124.jpeg#)

### 链式编程、流式计算、lambda 表达式，现在的 Java 程序员必会！

流（Stream）到底是什么呢？
是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。

### “集合讲的是数据，流讲的是计算！”

特点：
Stream 自己不会存储元素。
Stream 不会改变源对象，相反，他们会返回一个持有结果的新 Stream。
Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834825299-9253f9ed-a903-4512-aecc-656a065ce74b.jpeg#)
代码验证

### User 实体类

public class User { private int id;
private String userName; private int age;
//get、set、有参/无参构造器、toString
}
1
2
3
4
5
6

**Stream 算法题**

1. import java.util.Arrays;
1. import java.util.List; 3

4 /\*

1. - 题目：请按照给出数据，找出同时满足以下条件的用户
1. - 也即以下条件：
1. - 1、全部满足偶数 ID
1. - 2、年龄大于 24
1. - 3、用户名转为大写
1. - 4、用户名字母倒排序
1. - 5、只输出一个用户名字 limit 12 \*\*/
1. public class StreamDemo {
1. public static void main(String[] args) { 15
   | 16 | User | u1 | = | new | User(11, | "a", | 23); |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | 17 | User | u2 | = | new | User(12, | "b", | 24); |
   | 18 | User | u3 | = | new | User(13, | "c", | 22); |
   | 19 | User | u4 | = | new | User(14, | "d", | 28); |
   | 20 | User | u5 | = | new | User(16, | "e", | 26); |
   | 21 | | | | | | | |

22 List<User> list = Arrays.asList(u1, u2, u3, u4, u5); 23
24 /\*

1.  - 1.  首先我们需要将 list 转化为 stream 流
1.      * 2. 然后将用户过滤出来，这里用到一个函数式接口Predicate<? super T>，我们可以使用lambda表达式简化
1.      * 3. 这里面传递的参数，就是Stream流的泛型类型，也就是User，所以，这里可以直接返回用户id为偶数的用户信息;
1.  - 4.  通过 forEach 进行遍历，直接简化输出 System.out::println ，等价于

System.out.println(u); 29 \*\*/
30

1.      //list.stream().filter(u -> {return u.getId()%2==0;}).forEach(System.out::println);
1.  //list.stream().filter(u -> {return u.getId()%2==0;})
1.      //.filter(u -> {return u.getAge()>24;}).forEach(System.out::println);

34
35 //sorted() 自然排序，正排序 D->E 36

1. list.stream()
1. .filter(u -> {return u.getId()%2==0;})
1. .filter(u -> {return u.getAge()>24;})
1. .map(u -> {return u.getUserName().toUpperCase();})
1. //.sorted() //默认正排序 自己用 compareTo 比较
1. .sorted((o1,o2)->{return o2.compareTo(o1);})
1. .limit(1)
   | 44 | .forEach(System.out::println); |
   | --- | --- |
   | 45 | |
   | 46 | /* |
   | 47 | map 解释 |
   | 48 | List<Integer> list2 = Arrays.asList(1,2,3); |
   | 49 | list2 = list2.stream().map(x -> {return |
   | | x*2;}).collect(Collectors.toList()); |
   | 50 | |
   | 51 | for (Integer element : list2) { |
   | 52 | System.out.println(element); |
   | 53 | } |
   | 54 | \*/ |
   | 55 | |
   | 56 | } |
   | 57 | } |

# 14、分支合并

什么是 ForkJoin
从 JDK1.7 开始，Java 提供 Fork/Join 框架用于并行执行任务，它的思想就是讲一个大任务分割成若干小任 务，最终汇总每个小任务的结果得到这个大任务的结果。
这种思想和 MapReduce 很像（input --> split --> map --> reduce --> output） 主要有两步：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834825778-31ab66d7-d32f-4eb2-a152-bdd153ff5c3e.png#)第一、任务切分； 第二、结果合并
它的模型大致是这样的：线程池中的每个线程都有自己的工作队列（PS：这一点和 ThreadPoolExecutor 不同，ThreadPoolExecutor 是所有线程公用一个工作队列，所有线程都从这个工作队列中取任务），当 自己队列中的任务都完成以后，会从其它线程的工作队列中偷一个任务执行，这样可以充分利用资源。

工作窃取
另外，forkjoin 有一个工作窃取的概念。简单理解，就是一个工作线程下会维护一个包含多个子任务的双端队列。而对于每个工作线程来说，会从头部到尾部依次执行任务。这时，总会有一些线程执行的速度 较快，很快就把所有任务消耗完了。那这个时候怎么办呢，总不能空等着吧，多浪费资源啊。
工作窃取（work-stealing）算法是指某个线程从其他队列里窃取任务来执行。工作窃取的运行流程图如下：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834826334-cc16473b-d2df-42d5-a3fe-6a22b033a7ab.jpeg#)
那么为什么需要使用工作窃取算法呢？
假如我们需要做一个比较大的任务，我们可以把这个任务分割为若干互不依赖的子任务，为了减少线程 间的竞争，于是把这些子任务分别放到不同的队列里，并为每个队列创建一个单独的线程来执行队列里 的任务，线程和队列一一对应，比如 A 线程负责处理 A 队列里的任务。但是有的线程会先把自己队列里的 任务干完，而其他线程对应的队列里还有任务等待处理。干完活的线程与其等着，不如去帮其他线程干 活，于是它就去其他线程的队列里窃取一个任务来执行。而在这时它们会访问同一个队列，所以为了减 少窃取任务线程和被窃取任务线程之间的竞争，通常会使用双端队列，被窃取任务线程永远从双端队列 的头部拿任务执行，而窃取任务的线程永远从双端队列的尾部拿任务执行。
工作窃取算法的优点是充分利用线程进行并行计算，并减少了线程间的竞争，其缺点是在某些情况下还 是存在竞争，比如双端队列里只有一个任务时。并且消耗了更多的系统资源，比如创建多个线程和多个 双端队列。
于是，先做完任务的工作线程会从其他未完成任务的线程尾部依次获取任务去执行。这样就可以充分利 用 CPU 的资源。这个非常好理解，就比如有个妹子程序员做任务比较慢，那么其他猿就可以帮她分担一些任务，这简直是双赢的局面啊，妹子开心了，你也开心了。

核心类

### ForkJoinPool

WorkQueue 是一个 ForkJoinPool 中的内部类，它是线程池中线程的工作队列的一个封装，支持任务窃 取。

什么叫线程的任务窃取呢？就是说你和你的一个伙伴一起吃水果，你的那份吃完了，他那份没吃完，那 你就偷偷的拿了他的一些水果吃了。存在执行 2 个任务的子线程，这里要讲成存在 A,B 两个个 WorkQueue 在执行任务，A 的任务执行完了，B 的任务没执行完，那么 A 的 WorkQueue 就从 B 的 WorkQueue 的 ForkJoinTask 数组中拿走了一部分尾部的任务来执行，可以合理的提高运行和计算效率。
每个线程都有一个 WorkQueue，而 WorkQueue 中有执行任务的线程（ForkJoinWorkerThread owner），还有这个线程需要处理的任务（ForkJoinTask<?>[] array）。那么这个新提交的任务就是加到 array 中。

### ForkJoinTask

ForkJoinTask 代表运行在 ForkJoinPool 中的任务。主要方法：
fork() 在当前线程运行的线程池中安排一个异步执行。简单的理解就是再创建一个子任务。
join() 当任务完成的时候返回计算结果。
invoke() 开始执行任务，如果必要，等待计算完成。
子类： **Recursive ：递归**
RecursiveAction 一个递归无结果的 ForkJoinTask（没有返回值）
RecursiveTask 一个递归有结果的 ForkJoinTask（有返回值）

代码验证
核心代码

| 1   | package FORKJOIN;                                       |
| --- | ------------------------------------------------------- |
| 2   |                                                         |
| 3   | import java.util.concurrent.RecursiveTask;              |
| 4   |                                                         |
| 5   | public class ForkJoinWork extends RecursiveTask<Long> { |
| 6   |                                                         |
| 7   | private Long start;//起始值                             |
| 8   | private Long end;//结束值                               |
| 9   | public static final Long critical = 10000L;//临界值     |
| 10  |                                                         |
| 11  | public ForkJoinWork(Long start, Long end) {             |
| 12  | this.start = start;                                     |
| 13  | this.end = end;                                         |
| 14  | }                                                       |
| 15  |                                                         |
| 16  | @Override                                               |
| 17  | protected Long compute() {                              |
| 18  | //判断是否是拆分完毕                                    |
| 19  | Long lenth = end - start;                               |
| 20  | if(lenth<=critical){                                    |
| 21  | //如果拆分完毕就相加                                    |
| 22  | Long sum = 0L;                                          |
| 23  | for (Long i = start;i<=end;i++){                        |
| 24  | sum += i;                                               |
| 25  | }                                                       |
| 26  | return sum;                                             |
| 27  | }else {                                                 |

| 28  |     |     |     | //没有拆分完毕就开始拆分                             |
| --- | --- | --- | --- | ---------------------------------------------------- |
| 29  |     |     |     | Long middle = (end + start)/2;//计算的两个值的中间值 |
| 30  |     |     |     | ForkJoinWork right = new ForkJoinWork(start,middle); |
| 31  |     |     |     | right.fork();//拆分，并压入线程队列                  |
| 32  |     |     |     | ForkJoinWork left = new ForkJoinWork(middle+1,end);  |
| 33  |     |     |     | left.fork();//拆分，并压入线程队列                   |
| 34  |     |     |     |                                                      |
| 35  |     |     |     | //合并                                               |
| 36  |     |     |     | return right.join() + left.join();                   |
| 37  |     |     | }   |                                                      |
| 38  |     | }   |     |                                                      |
| 39  | }   |     |     |                                                      |

三种测试

| 1   | package com.kuang;                                                                     |
| --- | -------------------------------------------------------------------------------------- |
| 2   |                                                                                        |
| 3   | import java.util.concurrent.ExecutionException;                                        |
| 4   | import java.util.concurrent.ForkJoinPool;                                              |
| 5   | import java.util.concurrent.ForkJoinTask;                                              |
| 6   | import java.util.stream.LongStream;                                                    |
| 7   |                                                                                        |
| 8   | public class ForkJoinWorkDemo {                                                        |
| 9   |                                                                                        |
| 10  | public static void main(String[] args) throws ExecutionException,                      |
|     | InterruptedException {                                                                 |
| 11  | test(); //15756 // 14414 // 203                                                        |
| 12  | }                                                                                      |
| 13  |                                                                                        |
| 14  | // forkjoin 这个框架针对的是大任务执行，效率才会明显的看出来有提升，于是我把总数调大到 |
|     | 20 亿。                                                                                |
| 15  | public static void test() throws ExecutionException,                                   |
|     | InterruptedException {                                                                 |
| 16  | //ForkJoin 实现                                                                        |
| 17  | long l = System.currentTimeMillis();                                                   |
| 18  |                                                                                        |
| 19  | ForkJoinPool forkJoinPool = new ForkJoinPool();//实现 ForkJoin 就必须有                |
|     | ForkJoinPool 的支持                                                                    |
| 20  | ForkJoinTask<Long> task = new ForkJoinWork(0L,2000000000L);//参数为起                  |
|     | 始值与结束值                                                                           |
| 21  | ForkJoinTask<Long> result = forkJoinPool.submit(task);                                 |
| 22  | Long aLong = result.get();                                                             |
| 23  |                                                                                        |
| 24  | long l1 = System.currentTimeMillis();                                                  |
| 25  | System.out.println("invoke = " + aLong +" time: " + (l1-l));                           |
| 26  | }                                                                                      |
| 27  |                                                                                        |
| 28  | public static void test2(){                                                            |
| 29  | //普通线程实现                                                                         |
| 30  | Long x = 0L;                                                                           |
| 31  | Long y = 2000000000L;                                                                  |
| 32  | long l = System.currentTimeMillis();                                                   |
| 33  | for (Long i = 0L; i <= y; i++) {                                                       |
| 34  | x+=i;                                                                                  |
| 35  | }                                                                                      |
| 36  | long l1 = System.currentTimeMillis();                                                  |
| 37  | System.out.println("invoke = " + x+" time: " + (l1-l));                                |
| 38  |                                                                                        |

| 39  | }                                                            |
| --- | ------------------------------------------------------------ |
| 40  |                                                              |
| 41  | public static void test3(){                                  |
| 42  | //Java 8 并行流的实现                                        |
| 43  | long l = System.currentTimeMillis();                         |
| 44  | long reduce = LongStream.rangeClosed(0,                      |
|     | 2000000000L).parallel().reduce(0, Long::sum);                |
| 45  | long l1 = System.currentTimeMillis();                        |
| 46  | System.out.println("invoke = " + reduce+" time: " + (l1-l)); |
| 47  |                                                              |
| 48  | }                                                            |
| 49  |                                                              |
| 50  | }                                                            |

打个比方，假设一个酒店有 400 个房间，一共有 4 名清洁工，每个工人每天可以打扫 100 个房间，这样，4 个工人满负荷工作时，400 个房间全部打扫完正好需要 1 天。
Fork/Join 的工作模式就像这样：首先，工人甲被分配了 400 个房间的任务，他一看任务太多了自己一个人不行，所以先把 400 个房间拆成两个 200，然后叫来乙，把其中一个 200 分给乙。
紧接着，甲和乙再发现 200 也是个大任务，于是甲继续把 200 分成两个 100，并把其中一个 100 分给丙， 类似的，乙会把其中一个 100 分给丁，这样，最终 4 个人每人分到 100 个房间，并发执行正好是 1 天。

# 15、异步回调

概述
Future 设计的初衷：对将来某个时刻会发生的结果进行建模。
当我们需要调用一个函数方法时。如果这个函数执行很慢,那么我们就要进行等待。但有时候,我们可能并 不急着要结果。
因此,我们可以让被调用者立即返回,让他在后台慢慢处理这个请求。对于调用者来说,则可以先处理一些 其他任务,在真正需要数据的场合再去尝试获取需要的数据。
它建模了一种异步计算，返回一个执行运算结果的引用，当运算结束后，这个引用被返回给调用方。在 Future 中出发那些潜在耗时的操作把调用线程解放出来，让它能继续执行其他有价值的工作，不再需要等待耗时的操作完成。
**Future 的优点：比更底层的 Thread 更易用。**要使用 Future，通常只需要将耗时的操作封装在一个
Callable 对象中，再将它提交给 ExecutorService。
为了让程序更加高效，让 CPU 最大效率的工作，我们会采用异步编程。首先想到的是开启一个新的线程去做某项工作。再进一步，为了让新线程可以返回一个值，告诉主线程事情做完了，于是乎 Future 粉墨登场。然而 Future 提供的方式是主线程主动问询新线程，要是有个回调函数就爽了。所以，为了满足 Future 的某些遗憾，强大的 CompletableFuture 随着 Java8 一起来了。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834826846-c84a7338-96f5-4b4c-9242-32d2c7944745.jpeg#)

实例

| 1   | package com.kuang;                                                |
| --- | ----------------------------------------------------------------- |
| 2   |                                                                   |
| 3   | import java.util.concurrent.CompletableFuture;                    |
| 4   | import java.util.concurrent.TimeUnit;                             |
| 5   |                                                                   |
| 6   | public class CompletableFutureDemo {                              |
| 7   | public static void main(String[] args) throws Exception {         |
| 8   | //没有返回值的 runAsync 异步调用                                  |
| 9   | CompletableFuture<Void> completableFuture =                       |
|     | CompletableFuture.runAsync(() -> {                                |
| 10  | try {                                                             |
| 11  | TimeUnit.SECONDS.sleep(1);                                        |
| 12  | } catch (InterruptedException e) {                                |
| 13  | e.printStackTrace();                                              |
| 14  | }                                                                 |
| 15  | System.out.println(Thread.currentThread().getName() + "没有返回， |
|     | update mysql ok");                                                |
| 16  | });                                                               |
| 17  | System.out.println("111111"); // 先执行                           |
| 18  | completableFuture.get();                                          |
| 19  |                                                                   |
| 20  | //有返回值的 供给型参数接口                                       |
| 21  | CompletableFuture<Integer> completableFuture2 =                   |
|     | CompletableFuture.supplyAsync(() -> {                             |
| 22  | System.out.println(Thread.currentThread().getName() +             |
|     | "completableFuture2");                                            |
| 23  | int i = 10/0;                                                     |
| 24  | return 1024;                                                      |
| 25  | });                                                               |
| 26  |                                                                   |

| 27  | System.out.println(completableFuture2.whenComplete((t, u) -> { //编译 |
| --- | --------------------------------------------------------------------- |
|     | 完成，正常结束输出                                                    |
| 28  | System.out.println("===t:" + t); //正常结果                           |
| 29  | System.out.println("===u:" + u); //报错的信息                         |
| 30  | }).exceptionally(e -> { //结果异常，非正常结束                        |
| 31  | System.out.println("=======exception:" + e.getMessage());             |
| 32  | return 555;                                                           |
| 33  | }).get());                                                            |
| 34  |                                                                       |
| 35  | }                                                                     |
| 36  | }                                                                     |

# 16、JMM

问题：请你谈谈你对 volatile 的理解
volitile 是 Java 虚拟机提供的轻量级的同步机制，三大特性：
1、保证可见性
2、不保证原子性
3、禁止指令重排

什么是 JMM
JMM 本身是一种抽象的概念，并不真实存在，它描述的是一组规则或者规范~

### JMM 关于同步的规定：

1、线程解锁前，必须把共享变量的值刷新回主内存
2、线程加锁前，必须读取主内存的最新值到自己的工作内存
3、加锁解锁是同一把锁

JMM 即为 JAVA 内存模型（java memory model）。因为在**不同的硬件生产商和不同的操作系统**下，内存的访问逻辑有一定的差异，结果就是当你的代码在某个系统环境下运行良好，并且线程安全，但是换 了个系统就出现各种问题。Java 内存模型，就是为了屏蔽系统和硬件的差异，让一套代码在不同平台下能到达相同的访问结果。JMM 从 java 5 开始的 JSR-133 发布后，已经成熟和完善起来。
JMM 规定了内存主要划分为**主内存**和**工作内存**两种。此处的主内存和工作内存跟 JVM 内存划分（堆、 栈、方法区）是在不同的层次上进行的，如果非要对应起来，主内存对应的是 Java 堆中的对象实例部 分，工作内存对应的是栈中的部分区域，从更底层的来说，主内存对应的是硬件的物理内存，工作内存 对应的是寄存器和高速缓存。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834827416-7d360169-9597-449e-9ed3-4f3572316db0.jpeg#)
JVM 在设计时候考虑到，如果 JAVA 线程每次读取和写入变量都直接操作主内存，对性能影响比较大，所 以每条线程拥有各自的工作内存，工作内存中的变量是主内存中的一份拷贝，线程对变量的读取和写 入，直接在工作内存中操作，而不能直接去操作主内存中的变量。但是这样就会出现一个问题，当一个 线程修改了自己工作内存中变量，对其他线程是不可见的，会导致线程不安全的问题。因为 JMM 制定了一套标准来保证开发者在编写多线程程序的时候，能够控制什么时候内存会被同步给其他线程。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834827905-d3b98f25-2bae-4635-9b5e-2510701808c3.png#)
JMM 的内存模型

线程 A 感知不到线程 B 操作了值的变化！如何能够保证线程间可以同步感知这个问题呢？只需要使用 Volatile 关键字即可！volatile 保证线程间变量的可见性，简单地说就是当线程 A 对变量 X 进行了修改后， 在线程 A 后面执行的其他线程能看到变量 X 的变动，更详细地说是要符合以下两个规则 ：
线程对变量进行修改之后，要立刻回写到主内存。
线程对变量读取的时候，要从主内存中读，而不是缓存。

各线程的工作内存间彼此独立，互不可见，在线程启动的时候，虚拟机为每个内存分配一块工作内存， 不仅包含了线程内部定义的局部变量，也包含了线程所需要使用的共享变量（非线程内构造的对象）的 副本，即，为了提高执行效率。

### 内存交互操作

内存交互操作有 8 种，虚拟机实现必须保证每一个操作都是原子的，不可在分的（对于 double 和 long 类型的变量来说，load、store、read 和 write 操作在某些平台上允许例外）
lock （锁定）：作用于主内存的变量，把一个变量标识为线程独占状态
unlock （解锁）：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定
read （读取）：作用于主内存变量，它把一个变量的值从主内存传输到线程的工作内存中，以便 随后的 load 动作使用
load （载入）：作用于工作内存的变量，它把 read 操作从主存中变量放入工作内存中
use （使用）：作用于工作内存中的变量，它把工作内存中的变量传输给执行引擎，每当虚拟机 遇到一个需要使用到变量的值，就会使用到这个指令
assign （赋值）：作用于工作内存中的变量，它把一个从执行引擎中接受到的值放入工作内存的变量副本中
store （存储）：作用于主内存中的变量，它把一个从工作内存中一个变量的值传送到主内存 中，以便后续的 write 使用
write （写入）：作用于主内存中的变量，它把 store 操作从工作内存中得到的变量的值放入主内存的变量中

### JMM 对这八种指令的使用，制定了如下规则：

不允许 read 和 load、store 和 write 操作之一单独出现。即使用了 read 必须 load，使用了 store 必须
write
不允许线程丢弃他最近的 assign 操作，即工作变量的数据改变了之后，必须告知主存不允许一个线程将没有 assign 的数据从工作内存同步回主内存
一个新的变量必须在主内存中诞生，不允许工作内存直接使用一个未被初始化的变量。就是怼变量 实施 use、store 操作之前，必须经过 assign 和 load 操作
一个变量同一时间只有一个线程能对其进行 lock。多次 lock 后，必须执行相同次数的 unlock 才能解 锁
如果对一个变量进行 lock 操作，会清空所有工作内存中此变量的值，在执行引擎使用这个变量前， 必须重新 load 或 assign 操作初始化变量的值
如果一个变量没有被 lock，就不能对其进行 unlock 操作。也不能 unlock 一个被其他线程锁住的变量 对一个变量进行 unlock 操作之前，必须把此变量同步回主内存

JMM 对这八种操作规则和对**volatile 的一些特殊规则**就能确定哪里操作是线程安全，哪些操作是线程不 安全的了。但是这些规则实在复杂，很难在实践中直接分析。所以一般我们也不会通过上述规则进行分 析。更多的时候，使用 java 的 happen-before 规则来进行分析。
happens-before 字面翻译过来就是先行发生，A happens-before B 就是 A 先行发生于 B？
不准确！在 Java 内存模型中，happens-before 应该翻译成：前一个操作的结果可以被后续的操作获取。讲白点就是前面一个操作把变量 a 赋值为 1，那后面一个操作肯定能知道 a 已经变成了 1。
我们再来看看为什么需要这几条规则？
因为我们现在电脑都是多 CPU,并且都有缓存，导致多线程直接的可见性问题。详情可以看我之前的文章面试官：你知道并发 Bug 的源头是什么吗？

所以为了解决多线程的可见性问题，就搞出了 happens-before 原则，让线程之间遵守这些原则。编译器还会优化我们的语句，所以等于是给了编译器优化的约束。不能让它优化的不知道东南西北了！

# 17、volatile

### volatile 是不错的机制，但是也不能保证原子性。

代码验证可见性

| 1   | //Volatile 用来保证数据的同步，也就是可见性                               |
| --- | ------------------------------------------------------------------------- |
| 2   | public class JMMVolatileDemo01 {                                          |
| 3   | // volatile 不加 volatile 没有可见性                                      |
| 4   | // 不加 volatile 就会死循环，这里给大家将主要是为了面试，可以避免指令重排 |
| 5   | private volatile static int num = 0;                                      |
| 6   |                                                                           |
| 7   | public static void main(String[] args) throws InterruptedException {      |
| 8   | new Thread(()->{                                                          |
| 9   | while (num==0){ //此处不要编写代码,让计算机忙的不可开交                   |
| 10  |                                                                           |
| 11  | }                                                                         |
| 12  | }).start();                                                               |
| 13  |                                                                           |
| 14  | Thread.sleep(1000);                                                       |
| 15  | num = 1;                                                                  |
| 16  | System.out.println(num);                                                  |
| 17  | }                                                                         |
| 18  | }                                                                         |

验证 volatile 不保证原子性
原子性理解：
不可分割，完整性，也就是某个线程正在做某个具体的业务的时候，中间不可以被加塞或者被分割，需 要整体完整，要么同时成功，要么同时失败。

| 1   | public class JMMVolatileDemo02 {                                     |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | private volatile static int num = 0;                                 |
| 4   |                                                                      |
| 5   | public static void add(){                                            |
| 6   | num++;                                                               |
| 7   | }                                                                    |
| 8   |                                                                      |
| 9   | // 结果应该是 num 为 2 万，测试看结果                                |
| 10  | public static void main(String[] args) throws InterruptedException { |
| 11  | for (int i = 1; i <= 20; i++) {                                      |
| 12  | new Thread(()->{                                                     |
| 13  | for (int j = 1; j <= 1000; j++) {                                    |
| 14  | add();                                                               |
| 15  | }                                                                    |
| 16  | },String.valueOf(i)).start();                                        |
| 17  | }                                                                    |

| 18  |     |     | // 需要等待上面 20 个线程都全部计算完毕，看最终结果                |
| --- | --- | --- | ------------------------------------------------------------------ |
| 19  |     |     | while (Thread.activeCount()>2){ // 默认一个 main 线程 一个 gc 线程 |
| 20  |     |     | Thread.yield();                                                    |
| 21  |     |     | }                                                                  |
| 22  |     |     |                                                                    |
| 23  |     |     | System.out.println(Thread.currentThread().getName()+" "+num);      |
| 24  |     | }   |                                                                    |
| 25  |     |     |                                                                    |
| 26  | }   |     |                                                                    |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834828334-80dd42b7-06b5-42da-861b-bfa8f5714dbd.jpeg#)因为我们的 add 方法没有加锁，但是加了 volatile ，说明 volatile 不能保证原子性；画图解释，数值被覆盖
javap -c JMMVolatileDemo02.class
命令行查看底层字节码代码实现：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834828961-24df0fce-2a44-4dba-80bd-b0065c83b58e.jpeg#)
num++ 在多线程下是非线程安全的，如何不加 synchronized 解决？ 查看原子包下的类，分析方法！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834829504-b22c5354-13d9-4f11-8aea-76e6f862eaab.jpeg#)

测试：

| 1   | public class JMMVolatileDemo02 {                                     |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | private volatile static AtomicInteger num = new AtomicInteger();     |
| 4   |                                                                      |
| 5   | public static void add(){                                            |
| 6   | num.getAndIncrement(); // 等价 num++                                 |
| 7   | }                                                                    |
| 8   |                                                                      |
| 9   | // 结果应该是 num 为 2 万，测试看结果                                |
| 10  | public static void main(String[] args) throws InterruptedException { |
| 11  | for (int i = 1; i <= 20; i++) {                                      |
| 12  | new Thread(()->{                                                     |
| 13  | for (int j = 1; j <= 1000; j++) {                                    |
| 14  | add();                                                               |
| 15  | }                                                                    |
| 16  | },String.valueOf(i)).start();                                        |
| 17  | }                                                                    |
| 18  | // 需要等待上面 20 个线程都全部计算完毕，看最终结果                  |
| 19  | while (Thread.activeCount()>2){ // 默认一个 main 线程 一个 gc 线程   |
| 20  | Thread.yield();                                                      |
| 21  | }                                                                    |
| 22  |                                                                      |
| 23  | System.out.println(Thread.currentThread().getName()+" "+num);        |
| 24  | }                                                                    |
| 25  |                                                                      |
| 26  | }                                                                    |

你的同学在学习，你的对手在磨刀，你的闺蜜在减肥，隔壁老王在练腰

指令重排讲解
计算机在执行程序时，为了提高性能，编译器和处理器的常常会对指令做重排，一般分以下 3 种：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834829917-0711f3e1-41fd-4823-b83c-55232bb28993.jpeg#)

单线程环境里面确保程序最终执行结果和代码顺序执行的结果一致。处理器在进行重排序时必须要考虑指令之间的数据依赖性。
多线程环境中线程交替执行，由于编译器优化重排的存在，两个线程中使用的变量能否保证一致性是无 法确定的，结果无法预测。

### 重排理解测试 1：

| 1   | public class TestHappensBefore {                          |
| --- | --------------------------------------------------------- |
| 2   | public static void main(String[] args) {                  |
| 3   | int x = 11; // 语句 1                                     |
| 4   | int y = 12; // 语句 2                                     |
| 5   | x = x + 5; // 语句 3                                      |
| 6   | y = x \* x; // 语句 4                                     |
| 7   | }                                                         |
| 8   | // 指令顺序预测: 1234 2134 1324                           |
| 9   | // 问题：请问语句 4 可以重排后变成第一条吗？ 答案：不可以 |
| 10  |                                                           |
| 11  | }                                                         |

**重排理解测试 2：**
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834830569-62723774-a2e3-41b6-a557-b4702f49aae2.png#)
案例：

| 1   | // 多线程环境中线程交替执行，由于编译器优化重排的存在             |
| --- | ----------------------------------------------------------------- |
| 2   | // 两个线程中使用的变量能否保证一致性是无法确定的，结果无法预测。 |
| 3   | public class TestHappensBefore {                                  |
| 4   |                                                                   |
| 5   | int a = 0;                                                        |
| 6   | boolean flag = false;                                             |
| 7   |                                                                   |
| 8   | public void m1(){                                                 |
| 9   | a = 1; // 语句 1                                                  |
| 10  | flag = true; // 语句 2                                            |
| 11  | }                                                                 |
| 12  |                                                                   |
| 13  | public void m2(){                                                 |
| 14  | if (flag){                                                        |
| 15  | a = a + 5; // 语句 3                                              |

| 16 |

} |

} |
} | System.out.println("m2=>"+a); |
| --- | --- | --- | --- | --- |
| 17 | | | | |
| 18 | | | | |
| 19 | | | | |
| 20 | | | | |

### 指令重排小结：

volatile 实现了禁止指令重排优化，从而避免 多线程环境下程序出现乱序执行的现象。
先了解一个概念，内存屏障（Memory Barrier）又称内存栅栏，是一个 CPU 指令，它的作用有两个：
1、保证特定操作的执行顺序。
2、保证某些变量的内存可见性（利用该特性实现 volatile 的内存可见性）。
由于编译器和处理器都能执行指令重排优化。如果在指令间插入一条 Memory Barrier 则会告诉编译器和 CPU，不管什么指令都不能和这条 Memory Barrier 指令重排序，也就是说，通过插入内存屏障禁止在内存屏障前后的指令执行重排序优化。内存屏障另外一个作用是强制刷出各种 CPU 的缓存数据，因此任何 CPU 上的线程都能读取到这些数据的最新版本。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834831074-47884045-41c7-423d-8afc-b2fb6acd917c.jpeg#)

### 经过，可见性，原子性，指令重排的话，线程安全性获得保证：

工作内存与主内存同步延迟现象导致的可见性问题，可以使用 synchronized 或 volatile 关键字解决，它们都可以使一个线程修改后的变量立即对其他线程可见。
对于指令重排导致的可见性问题 和 有序性问题，可以利用 volatile 关键字解决，因为 volatile 的另外一个作用就是禁止重排序优化。

# 18、深入单例模式

单例模式可以说只要是一个合格的开发都会写，但是如果要深究，小小的单例模式可以牵扯到很多东 西，比如 多线程是否安全，是否懒加载，性能等等。还有你知道几种单例模式的写法呢？如何防止反射破坏单例模式？今天，我们来探究单例模式。
关于单例模式的概念，在这里就不在阐述了，相信每个小伙伴都了如指掌。我们直接进入正题：

## 1、饿汉式

| 1   | public class Hungry {                              |
| --- | -------------------------------------------------- |
| 2   | private Hungry() {                                 |
| 3   | }                                                  |
| 4   | private final static Hungry hungry = new Hungry(); |
| 5   |                                                    |
| 6   | public static Hungry getInstance() {               |
| 7   | return hungry;                                     |
| 8   | }                                                  |
| 9   | }                                                  |

饿汉式是最简单的单例模式的写法，保证了线程的安全，在很长的时间里，我都是饿汉模式来完成单例 的，因为够简单，后来才知道饿汉式会有一点小问题，看下面的代码：

| 1   | public class Hungry {                              |
| --- | -------------------------------------------------- |
| 2   | private byte[] data1 = new byte[1024];             |
| 3   | private byte[] data2 = new byte[1024];             |
| 4   | private byte[] data3 = new byte[1024];             |
| 5   | private byte[] data4 = new byte[1024];             |
| 6   |                                                    |
| 7   | private Hungry() {                                 |
| 8   | }                                                  |
| 9   |                                                    |
| 10  | private final static Hungry hungry = new Hungry(); |
| 11  |                                                    |
| 12  | public static Hungry getInstance() {               |
| 13  | return hungry;                                     |
| 14  | }                                                  |
| 15  | }                                                  |

在 Hungry 类中，我定义了四个 byte 数组，当代码一运行，这四个数组就被初始化，并且放入内存了，如果长时间没有用到 getInstance 方法，不需要 Hungry 类的对象，这不是一种浪费吗？我希望的是 只有用到了 getInstance 方法，才会去初始化单例类，才会加载单例类中的数据。所以就有了 第二种单例模
式：懒汉式。

## 2、懒汉式

正常的 懒汉式单例：

| 1   | public class LazyMan {                                        |
| --- | ------------------------------------------------------------- |
| 2   | private LazyMan() {                                           |
| 3   | System.out.println(Thread.currentThread().getName()+"Start"); |
| 4   | }                                                             |
| 5   |                                                               |
| 6   | private static LazyMan lazyMan;                               |
| 7   |                                                               |
| 8   | public static LazyMan getInstance() {                         |
| 9   | if (lazyMan == null) {                                        |
| 10  | lazyMan = new LazyMan();                                      |
| 11  | }                                                             |
| 12  | return lazyMan;                                               |
| 13  | }                                                             |
| 14  |                                                               |
| 15  | // 测试并发环境，发现单例失效                                 |
| 16  | public static void main(String[] args) {                      |
| 17  | for (int i = 0; i < 10; i++) {                                |
| 18  | new Thread(()->{                                              |
| 19  | LazyMan.getInstance();                                        |

20
21
22
23
}).start();
}
}
}

多加一层检测可以避免问题，也就是 DCL 懒汉式！

| 1   | public class LazyMan {                |
| --- | ------------------------------------- |
| 2   | private LazyMan() {                   |
| 3   | }                                     |
| 4   |                                       |
| 5   | private static LazyMan lazyMan;       |
| 6   |                                       |
| 7   | public static LazyMan getInstance() { |
| 8   | if (lazyMan == null) {                |
| 9   | synchronized (LazyMan.class) {        |
| 10  | if (lazyMan == null) {                |
| 11  | lazyMan = new LazyMan();              |
| 12  | }                                     |
| 13  | }                                     |
| 14  | }                                     |
| 15  | return lazyMan;                       |
| 16  | }                                     |
| 17  | }                                     |

DCL 懒汉式的单例，保证了线程的安全性，又符合了懒加载，只有在用到的时候，才会去初始化，调用效率也比较高，但是这种写法在极端情况还是可能会有一定的问题。因为

| 1   | lazyMan | =   | new | LazyMan(); |
| --- | ------- | --- | --- | ---------- |

不是原子性操作，至少会经过三个步骤：

1. 分配对象内存空间
1. 执行构造方法初始化对象
1. 设置 instance 指向刚分配的内存地址，此时 instance ！=null；

由于指令重排，导致 A 线程执行 lazyMan = new LazyMan();的时候，可能先执行了第三步（还没执行第二步），此时线程 B 又进来了，发现 lazyMan 已经不为空了，直接返回了 lazyMan，并且后面使用了返回 的 lazyMan，由于线程 A 还没有执行第二步，导致此时 lazyMan 还不完整，可能会有一些意想不到的错误，所以就有了下面一种单例模式。
这种单例模式只是在上面 DCL 单例模式增加一个 volatile 关键字来避免指令重排：

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
public class LazyMan { private LazyMan() {
}
private volatile static LazyMan lazyMan;

public static LazyMan getInstance() { if (lazyMan == null) {
synchronized (LazyMan.class) { if (lazyMan == null) {
lazyMan = new LazyMan();
}
}
}
return lazyMan;

}
}
16
17

## 3、静态内部类

还有这种方式是第一种饿汉式的改进版本，同样也是在类中定义 static 变量的对象，并且直接初始化，不过是移到了静态内部类中，十分巧妙。既保证了线程的安全性，同时又满足了懒加载。

| 1   | public class Holder {                              |
| --- | -------------------------------------------------- |
| 2   | private Holder() {                                 |
| 3   | }                                                  |
| 4   |                                                    |
| 5   | public static Holder getInstance() {               |
| 6   | return InnerClass.holder;                          |
| 7   | }                                                  |
| 8   |                                                    |
| 9   | private static class InnerClass {                  |
| 10  | private static final Holder holder = new Holder(); |
| 11  | }                                                  |
| 12  | }                                                  |

## 4、万恶的反射

万恶的反射登场了，反射是一个比较霸道的东西，无视 private 修饰的构造方法，可以直接在外面
newInstance，破坏我们辛辛苦苦写的单例模式。

| 1
2
3
4 | public static void main(String[] args) { try {
LazyMan lazyMan1 = LazyMan.getInstance(); Constructor<LazyMan> declaredConstructor =
LazyMan.class.getDeclaredConstructor(null); | |
| --- | --- | --- |
| 5 | | declaredConstructor.setAccessible(true); |
| 6 | | LazyMan lazyMan2 = declaredConstructor.newInstance(); |
| 7 | | System.out.println(lazyMan1.hashCode()); |
| 8 | | System.out.println(lazyMan2.hashCode()); |
| 9 | | System.out.println(lazyMan1 == lazyMan2); |
| 10 | | } catch (Exception e) { |
| 11 | | e.printStackTrace(); |
| 12 | | } |
| 13 | } | |

我们分别打印出 lazyMan1，lazyMan2 的 hashcode，lazyMan1 是否相等 lazyMan2，结果显而易见，**不相等；**
那么，怎么解决这种问题呢？

| 1   | public class LazyMan {                                    |
| --- | --------------------------------------------------------- |
| 2   | private LazyMan() {                                       |
| 3   | synchronized (LazyMan.class) {                            |
| 4   | if (lazyMan != null) {                                    |
| 5   | throw new RuntimeException("不要试图用反射破坏单例模式"); |
| 6   | }                                                         |
| 7   | }                                                         |
| 8   | }                                                         |
| 9   |                                                           |
| 10  | private volatile static LazyMan lazyMan;                  |
| 11  |                                                           |

| 12 |

} | public static LazyMan getInstance() { if (lazyMan == null) {
synchronized (LazyMan.class) { if (lazyMan == null) {
lazyMan = new LazyMan();
}
}
}
return lazyMan;
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

在私有的构造函数中做一个判断，如果 lazyMan 不为空，说明 lazyMan 已经被创建过了，如果正常调用
getInstance 方法，是不会出现这种事情的，所以直接抛出异常！
但是这种写法还是有问题：
上面我们是先正常的调用了 getInstance 方法，创建了 LazyMan 对象，所以第二次用反射创建对象，私有 构造函数里面的判断起作用了，反射破坏单例模式失败。但是如果破坏者干脆不先调用 getInstance 方 法，一上来就直接用反射创建对象，我们的判断就不生效了：

| 1
2
3 | public static void main(String[] args) { try {
Constructor<LazyMan> declaredConstructor =
LazyMan.class.getDeclaredConstructor(null); | |
| --- | --- | --- |
| 4 | | declaredConstructor.setAccessible(true); |
| 5 | | LazyMan lazyMan1 = declaredConstructor.newInstance(); |
| 6 | | LazyMan lazyMan2 = declaredConstructor.newInstance(); |
| 7 | | System.out.println(lazyMan1.hashCode()); |
| 8 | | System.out.println(lazyMan2.hashCode()); |
| 9 | | } catch (Exception e) { |
| 10 | | e.printStackTrace(); |
| 11 | | } |
| 12 | } | |

那么如何防止这种反射破坏呢？

public class LazyMan {
private static boolean flag = false; private LazyMan() {
synchronized (LazyMan.class) { if (flag == false) {
flag = true;
} else {
throw new RuntimeException("不要试图用反射破坏单例模式");
}
}
}
private volatile static LazyMan lazyMan; public static LazyMan getInstance() {
if (lazyMan == null) { synchronized (LazyMan.class) {
if (lazyMan == null) { lazyMan = new LazyMan();
}
}
}
return lazyMan;
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

}
}
22
23

在这里，我定义了一个 boolean 变量 ﬂag，初始值是 false，私有构造函数里面做了一个判断，如果 ﬂag=false，就把 ﬂag 改为 true，但是如果 ﬂag 等于 true，就说明有问题了，因为正常的调用是不会第二 次跑到私有构造方法的，所以抛出异常。
看起来很美好，但是还是不能完全防止反射破坏单例模式，因为可以利用反射修改 ﬂag 的值。

| 1   | class Demo02{                                         |
| --- | ----------------------------------------------------- |
| 2   | public static void main(String[] args) {              |
| 3   | try {                                                 |
| 4   | // 通过反射创建对象                                   |
| 5   | Constructor<LazyMan> declaredConstructor =            |
|     | LazyMan.class.getDeclaredConstructor(null);           |
| 6   | Field field = LazyMan.class.getDeclaredField("flag"); |
| 7   | field.setAccessible(true);                            |
| 8   |                                                       |
| 9   | // 通过反射实例化对象                                 |
| 10  | declaredConstructor.setAccessible(true);              |
| 11  | LazyMan lazyMan1 = declaredConstructor.newInstance(); |
| 12  | System.out.println(field.get(lazyMan1));              |
| 13  | System.out.println(lazyMan1.hashCode());              |
| 14  |                                                       |
| 15  | //通过反射，修改字段的值！                            |
| 16  | field.set(lazyMan1,false);                            |
| 17  | LazyMan lazyMan2 = declaredConstructor.newInstance(); |
| 18  | System.out.println(field.get(lazyMan2));              |
| 19  | System.out.println(lazyMan2.hashCode());              |
| 20  |                                                       |
| 21  | } catch (Exception e) {                               |
| 22  | e.printStackTrace();                                  |
| 23  | }                                                     |
| 24  | }                                                     |
| 25  | }                                                     |

并没有一个很好的方案去避免反射破坏单例模式，所以轮到我们的枚举登场了。

## 5、枚举

枚举类型是 Java 5 中新增特性的一部分，它是一种特殊的数据类型，之所以特殊是因为它既是一种类(class)类型却又比类类型多了些特殊的约束，但是这些约束的存在也造就了枚举类型的简洁性、安全性以及便捷性。

| 1   | public enum EnumSingleton {                                |
| --- | ---------------------------------------------------------- |
| 2   | INSTANCE;                                                  |
| 3   | public EnumSingleton getInstance(){                        |
| 4   | return INSTANCE;                                           |
| 5   | }                                                          |
| 6   | }                                                          |
| 7   |                                                            |
| 8   | class Demo04{                                              |
| 9   | public static void main(String[] args) {                   |
| 10  | EnumSingleton singleton1=EnumSingleton.INSTANCE;           |
| 11  | EnumSingleton singleton2=EnumSingleton.INSTANCE;           |
| 12  | System.out.println("正常情况下，实例化两个实例是否相同："+ |
|     | (singleton1==singleton2));                                 |

}
}
13
14

枚举是目前最推荐的单例模式的写法，因为足够简单，不需要开发自己保证线程的安全，同时又可以有 效的防止反射破坏我们的单例模式，我们可以看下 newInstance 的源码：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834831450-a2a7bda3-0d30-4387-9c5f-f47f3f00227f.png#)img
重点就是红框中圈出来的部分，如果枚举去 newInstance 就直接抛出异常了。

### 反编译查看下枚举的源码

| 1   | javap -p EnumSingleton.class                                            |
| --- | ----------------------------------------------------------------------- |
| 2   |                                                                         |
| 3   | Compiled from "EnumSingleton.java"                                      |
| 4   | public final class 单例模式.EnumSingleton extends java.lang.Enum<单例模 |
|     | 式.EnumSingleton> {                                                     |
| 5   | public static final 单例模式.EnumSingleton INSTANCE;                    |
| 6   | private static final 单例模式.EnumSingleton[] $VALUES;                  |
| 7   | public static 单例模式.EnumSingleton[] values();                        |
| 8   | public static 单例模式.EnumSingleton valueOf(java.lang.String);         |
| 9   | private 单例模式.EnumSingleton();                                       |
| 10  | public 单例模式.EnumSingleton getInstance();                            |
| 11  | static {};                                                              |
| 12  | }                                                                       |

这个看的不清楚，我们可以下 jad 进行反编译，我们的素材中也都有！

| 1   | jad -sjava EnumSingleton.class                               |
| --- | ------------------------------------------------------------ |
| 2   | # 会生成一个 java 文件                                       |
| 3   | Parsing EnumSingleton.class... Generating EnumSingleton.java |

我们点开里面的源码

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
// Decompiled by Jad v1.5.8g. Copyright 2001 Pavel Kouznetsov.
// Jad home page: [http://www.kpdus.com/jad.html](http://www.kpdus.com/jad.html)
// Decompiler options: packimports(3)
// Source File Name:
EnumSingleton.java
package 53554F8B6A215F0F;
public final class EnumSingleton extends Enum
{

public static EnumSingleton[] values()
{
return (EnumSingleton[])$VALUES.clone();
}
20
21
22
23
public static EnumSingleton valueOf(String name)
{
return (EnumSingleton)Enum.valueOf(53554F8B6A215F0F/EnumSingleton,
name);
}
private EnumSingleton(String s, int i)
{

| 24  |     | super(s, i);                                  |
| --- | --- | --------------------------------------------- |
| 25  |     | }                                             |
| 26  |     |                                               |
| 27  |     | public EnumSingleton getInstance()            |
| 28  |     | {                                             |
| 29  |     | return INSTANCE;                              |
| 30  |     | }                                             |
| 31  |     |                                               |
| 32  |     | public static final EnumSingleton INSTANCE;   |
| 33  |     | private static final EnumSingleton $VALUES[]; |
| 34  |     |                                               |
| 35  |     | static                                        |
| 36  |     | {                                             |
| 37  |     | INSTANCE = new EnumSingleton("INSTANCE", 0);  |
| 38  |     | $VALUES = (new EnumSingleton[] {              |
| 39  |     | INSTANCE                                      |
| 40  |     | });                                           |
| 41  |     | }                                             |
| 42  | }   |                                               |

再次尝试破坏看一下！

| 1   | package 单例模式;                                                        |
| --- | ------------------------------------------------------------------------ |
| 2   |                                                                          |
| 3   | import java.lang.reflect.Constructor;                                    |
| 4   |                                                                          |
| 5   | public enum EnumSingleton {                                              |
| 6   | INSTANCE;                                                                |
| 7   | public EnumSingleton getInstance(){                                      |
| 8   | return INSTANCE;                                                         |
| 9   | }                                                                        |
| 10  | }                                                                        |
| 11  |                                                                          |
| 12  | class Demo04{                                                            |
| 13  | public static void main(String[] args) throws Exception {                |
| 14  | EnumSingleton singleton1=EnumSingleton.INSTANCE;                         |
| 15  | EnumSingleton singleton2=EnumSingleton.INSTANCE;                         |
| 16  | System.out.println("正常情况下，实例化两个实例是否相同："+               |
|     | (singleton1==singleton2));                                               |
| 17  | //Constructor<EnumSingleton> constructor =                               |
|     | EnumSingleton.class.getDeclaredConstructor(); //自身的类没有无参构造方法 |
| 18  | Constructor<EnumSingleton> constructor =                                 |
|     | EnumSingleton.class.getDeclaredConstructor(String.class,int.class);      |
| 19  | constructor.setAccessible(true);                                         |
| 20  | EnumSingleton enumSingleton = constructor.newInstance();                 |
| 21  | }                                                                        |
| 22  | }                                                                        |

试图破坏，真的破坏不了！
假如有人问你单例模式，再也不用害怕了。

# 19、深入理解 CAS

CAS : 比较并交换

前言：互联网缩招之下，初级程序员大量过剩，高级程序员重金难求，除非你不吃这碗饭，否则就要逼 自己提升！
用代码理解下什么是 CAS：

| 1   | package com.kuang;                                                     |
| --- | ---------------------------------------------------------------------- |
| 2   |                                                                        |
| 3   | import java.util.concurrent.atomic.AtomicInteger;                      |
| 4   |                                                                        |
| 5   | /\*\*                                                                  |
| 6   | \* CAS : 比较并交换 compareAndSet                                      |
| 7   | \*                                                                     |
| 8   | \* 参数：期望值，更新值                                                |
| 9   | \* public final boolean compareAndSet(int expect, int update) {        |
| 10  | \* return unsafe.compareAndSwapInt(this, valueOffset, expect, update); |
| 11  | \* }                                                                   |
| 12  | \* @author 狂神说 Java [24736743@qq.com](mailto:24736743@qq.com)       |
| 13  | \*/                                                                    |
| 14  | public class CASDemo {                                                 |
| 15  | public static void main(String[] args) {                               |
| 16  | AtomicInteger atomicInteger = new AtomicInteger(5);                    |
| 17  |                                                                        |
| 18  | // main do somethings...                                               |
| 19  |                                                                        |
| 20  | // 期望的是 5，后面改为 2020 ， 所以结果为 true，2020                  |
| 21  | System.out.println(atomicInteger.compareAndSet(5,                      |
|     | 2020)+"=>"+atomicInteger.get());                                       |
| 22  |                                                                        |
| 23  | // 期望的是 5，后面改为 1024 ， 所以结果为 false，2020                 |
| 24  | System.out.println(atomicInteger.compareAndSet(5,                      |
|     | 1024)+"=>"+atomicInteger.get());                                       |
| 25  |                                                                        |
| 26  | }                                                                      |
| 27  | }                                                                      |

一句话：真实值和期望值相同，就修改成功，真实值和期望值不同，就修改失败！

CAS 底层原理？如果知道，谈谈你对 UnSafe 的理解？
这里的自增 + 1 怎么实现的！

| 1   | atomicInteger.getAndIncrement(); // 分析源码，如何实现的 | i++ | 安全的问题 |
| --- | -------------------------------------------------------- | --- | ---------- |
| 2   |                                                          |     |            |
| 3   | public final int getAndIncrement() { // 继续走源码       |     |            |
| 4   | // this 当前对象                                         |     |            |
| 5   | // valueOffset 内存偏移量，内存地址                      |     |            |
| 6   | // 1.固定写死                                            |     |            |
| 7   | return unsafe.getAndAddInt(this, valueOffset, 1);        |     |            |
| 8   | }                                                        |     |            |

发现到了 字节码文件，我们已经无法在这里操作了！
atomicInteger.getAndIncrement();

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834831740-c12f19ed-4b1b-4341-9dfa-538d2f5d45f3.png#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834831985-2ae3e81a-891b-4068-94ec-f08c6f781bd2.png#)需要去到 JDK 安装目录下的 rt.jar 包下寻找了！而且这个类中的方法大部分都是 native 的方法了！ 问题：这个 UnSafe 类到底是什么？ 可以看到 AtomicInteger 源码中也是它！
1

### 1、UnSafe

UnSafe 是 CAS 的核心类，由于 Java 方法无法直接访问底层系统，需要通过本地（native）方法来访问， UnSafe 相当于一个后门，基于该类可以直接操作特定内存的数据，Unsafe 类存在于 sun.misc 包中，其内部方法操作可以像 C 的指针一样直接操作内存，因为 Java 中 CAS 操作的执行依赖于 Unsafe 类的方法。
注意：Unsafe 类中的所有方法都是 Native 修饰的，也就是说 Unsafe 类中的方法都直接调用操作系统底层 资源执行相应任务

### 2、变量 valueOﬀset

表示该变量值在内存中的偏移地址，因为 Unsafe 就是根据内存偏移地址获取数据的。

### 3、变量 value 用 volatile 修饰，保证了多线程之间的内存可见性

最后解释 CAS 是什么
CAS 的全称为 Compare-And-Swap，它是一条 CPU 并发原语。
它的功能是判断内存某个位置的值是否为预期值，如果是则更改为新的值，这个过程是原子的。
CAS 并发原语体现在 JAVA 语言中就是 sun.misc.Unsafe 类中的各个方法。调用 UnSafe 类中的 CAS 方法， JVM 会帮我们实现出 CAS 汇编指令。这是一种完全依赖于硬件的功能，通过它实现了原子操作。再次强调，由于 CAS 是一种系统原语，原语属于操作系统用于范畴，是由若干条指令组成的，用于完成某个功 能的一个过程，并且原语的执行必须是连续的，在执行过程中不允许被中断，也就是说 CAS 是一条 CPU 的原子指令，不会造成所谓的数据不一致问题。
分析源码：

| 1   | public final int getAndAddInt(Object var1, long var2, int var4) {         |
| --- | ------------------------------------------------------------------------- |
| 2   | int var5;                                                                 |
| 3   | do {                                                                      |
| 4   | // 获取传入对象的地址                                                     |
| 5   | var5 = this.getIntVolatile(var1, var2);                                   |
| 6   | // 比较并交换，如果 var1，var2 还是原来的 var5，就执行内存偏移+1； var5 + |
|     | var4                                                                      |
| 7   | } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));          |
| 8   |                                                                           |
| 9   | return var5;                                                              |
| 10  | }                                                                         |

汇编层面理解
Unsafe 类中的 compareAndSwapint，是一个本地方法，该方法的实现位于 unsafe.cpp 中；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834832612-08c83ab7-60f1-4ebe-b830-8d18c43a79a0.jpeg#)

总结

### CAS（CompareAndSwap）

比较当前工作内存中的值和主内存中的值，如果相同则执行规定操作，否则继续比较直到主内存和工作 内存中的值一致为止。

### CAS 应用

CAS 有 3 个操作数，内存值 V，旧的预期值 A，要修改的更新值 B。且仅当预期值 A 和 内存值 V 相同时， 将内存值 V 修改为 B，否则什么都不做。

### CAS 的缺点

1、循环时间长开销很大。
可以看到源码中存在 一个 do...while 操作，如果 CAS 失败就会一直进行尝试。
2、只能保证一个共享变量的原子操作。
当对一个共享变量执行操作时，我们可以使用循环 CAS 的方式来保证原子操作。但是：
对多个共享变量操作时，循环 CAS 就无法保证操作的原子性，这时候就可以用锁来保证原子性。
3、引出来 ABA 问题？？？

# 20、原子引用

原子类 AtomicInteger 的 ABA 问题谈谈？原子更新引用知道吗？
CAS => UnSafe => CAS 底层思想 => ABA => 原子引用更新 => 如何规避 ABA 问题

### ABA 问题怎么产生的？

CAS 会导致 “ABA 问题”。狸猫换太子
CAS 算法实现一个重要前提：需要取出内存中某时刻的数据并在当下时刻比较并交换，那么在这个时间差内会导致数据的变化
比如说一个线程 one 从内存位置 V 中取出 A，这个时候另一个线程 two 也从内存中取出 A，并且线程 two 进行了一些操作将值变成了 B，然后线程 two 又将 V 位置的数据变成 A，这时候线程 one 进行 CAS 操作发现内存中仍然是 A，然后线程 one 操作成功。
尽管线程 one 的 CAS 操作成功，但是不代表这个过程就是没有问题的。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834832958-f45a3c44-9fd2-4e40-8d76-e2696abb3fc6.png#)
原子引用 AtomicReference

| 1   | package com.kuang;                                               |
| --- | ---------------------------------------------------------------- |
| 2   |                                                                  |
| 3   | import java.util.concurrent.atomic.AtomicReference;              |
| 4   |                                                                  |
| 5   | public class AtomicReferenceDemo {                               |
| 6   | public static void main(String[] args) {                         |
| 7   | User zhangsan = new User("zhangsan", 22);                        |
| 8   | User lisi = new User("lisi", 25);                                |
| 9   |                                                                  |
| 10  | AtomicReference<User> atomicReference = new AtomicReference<>(); |
| 11  | atomicReference.set(zhangsan); // 设置                           |
| 12  |                                                                  |
| 13  | System.out.print(atomicReference.compareAndSet(zhangsan,lisi));  |
| 14  | System.out.println(atomicReference.get().toString());            |
| 15  |                                                                  |
| 16  | System.out.print(atomicReference.compareAndSet(zhangsan,lisi));  |
| 17  | System.out.println(atomicReference.get().toString());            |
| 18  | }                                                                |
| 19  | }                                                                |

| 20  |                                         |
| --- | --------------------------------------- |
| 21  | class User{                             |
| 22  | String username;                        |
| 23  | int age;                                |
| 24  |                                         |
| 25  | public User(String username, int age) { |
| 26  | this.username = username;               |
| 27  | this.age = age;                         |
| 28  | }                                       |
| 29  |                                         |
| 30  | @Override                               |
| 31  | public String toString() {              |
| 32  | return "User{" +                        |
| 33  | "username='" + username + '\'' +        |
| 34  | ", age=" + age +                        |
| 35  | '}';                                    |
| 36  | }                                       |
| 37  | }                                       |

### 要解决 ABA 问题，我们就需要加一个版本号

版本号原子引用，类似乐观锁

| T1  | 100 | 1   |                 |     |
| --- | --- | --- | --------------- | --- |
| T2  | 100 | 1   | => 101 2 => 100 | 3   |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834833625-29462352-c7c2-40cd-814e-d8307ee4ea7a.jpeg#)
演示 ABA 问题：

| 1   | /\*\*                                                                   |
| --- | ----------------------------------------------------------------------- |
| 2   | \* ABA 问题的解决 AtomicStampedReference                                |
| 3   | \*/                                                                     |
| 4   | public class ABADemo {                                                  |
| 5   |                                                                         |
| 6   | static AtomicReference<Integer> atomicReference = new AtomicReference<> |
|     | (100);                                                                  |
| 7   |                                                                         |
| 8   | public static void main(String[] args) {                                |

| 9   | new Thread(()->{                                                 |
| --- | ---------------------------------------------------------------- |
| 10  | atomicReference.compareAndSet(100,101);                          |
| 11  | atomicReference.compareAndSet(101,100);                          |
| 12  | },"T1").start();                                                 |
| 13  |                                                                  |
| 14  | new Thread(()->{                                                 |
| 15  | // 暂停一秒钟，保证上面线程先执行                                |
| 16  | try {                                                            |
| 17  | TimeUnit.SECONDS.sleep(1);                                       |
| 18  | } catch (InterruptedException e) {                               |
| 19  | e.printStackTrace();                                             |
| 20  | }                                                                |
| 21  | System.out.println(atomicReference.compareAndSet(100, 2019)); // |
|     | 修改成功！                                                       |
| 22  | System.out.println(atomicReference.get());                       |
| 23  | },"T2").start();                                                 |
| 24  | }                                                                |
| 25  | }                                                                |

解决方案：

| 1   | package com.kuang;                                                  |
| --- | ------------------------------------------------------------------- |
| 2   |                                                                     |
| 3   | import java.util.concurrent.TimeUnit;                               |
| 4   | import java.util.concurrent.atomic.AtomicStampedReference;          |
| 5   |                                                                     |
| 6   | /\*\*                                                               |
| 7   | \* ABA 问题的解决 AtomicStampedReference                            |
| 8   | \* 注意变量版本号修改和获取问题。不要写错                           |
| 9   | \*/                                                                 |
| 10  | public class ABADemo {                                              |
| 11  |                                                                     |
| 12  | static AtomicStampedReference<Integer> atomicStampedReference = new |
|     | AtomicStampedReference<>(100,1);                                    |
| 13  |                                                                     |
| 14  | public static void main(String[] args) {                            |
| 15  | new Thread(()->{                                                    |
| 16  | int stamp = atomicStampedReference.getStamp(); // 获得版本号        |
| 17  | System.out.println("T1 stamp 01=>"+stamp);                          |
| 18  |                                                                     |
| 19  | // 暂停 2 秒钟，保证下面线程获得初始版本号                          |
| 20  | try {                                                               |
| 21  | TimeUnit.SECONDS.sleep(1);                                          |
| 22  | } catch (InterruptedException e) {                                  |
| 23  | e.printStackTrace();                                                |
| 24  | }                                                                   |
| 25  |                                                                     |
| 26  | atomicStampedReference.compareAndSet(100, 101,                      |
|     | atomicStampedReference.getStamp()                                   |
| 27  | ,                                                                   |
|     | atomicStampedReference.getStamp()+1);                               |
| 28  |                                                                     |
| 29  | System.out.println("T1 stamp                                        |
|     | 02=>"+atomicStampedReference.getStamp());                           |
| 30  |                                                                     |
| 31  | atomicStampedReference.compareAndSet(101, 100,                      |
|     | atomicStampedReference.getStamp()                                   |

| 32  | ,                                                                |
| --- | ---------------------------------------------------------------- |
|     | atomicStampedReference.getStamp()+1);                            |
| 33  |                                                                  |
| 34  | System.out.println("T1 stamp                                     |
|     | 03=>"+atomicStampedReference.getStamp());                        |
| 35  | },"T1").start();                                                 |
| 36  |                                                                  |
| 37  | new Thread(()->{                                                 |
| 38  |                                                                  |
| 39  | int stamp = atomicStampedReference.getStamp(); // 获得版本号     |
| 40  | System.out.println("T2 stamp 01=>"+stamp);                       |
| 41  | // 暂停 3 秒钟，保证上面线程先执行                               |
| 42  | try {                                                            |
| 43  | TimeUnit.SECONDS.sleep(3);                                       |
| 44  | } catch (InterruptedException e) {                               |
| 45  | e.printStackTrace();                                             |
| 46  | }                                                                |
| 47  |                                                                  |
| 48  | boolean result = atomicStampedReference.compareAndSet(100, 2019, |
|     | stamp, stamp + 1);                                               |
| 49  | System.out.println("T2 是否修改成功 =>"+ result);                |
| 50  | System.out.println("T2 最新 stamp                                |
|     | =>"+atomicStampedReference.getStamp());                          |
| 51  | System.out.println("T2 当前的最新值                              |
|     | =>"+atomicStampedReference.getReference());                      |
| 52  |                                                                  |
| 53  | },"T2").start();                                                 |
| 54  | }                                                                |
| 55  | }                                                                |

# 21、Java 锁

## 1、公平锁非公平锁

是什么
公平锁：是指多个线程按照申请锁的顺序来获取锁，类似排队打饭，先来后到。
非公平锁：是指多个线程获取锁的顺序并不是按照申请锁的顺序，有可能后申请的线程比现申请的线程 优先获取锁，在高并发的情况下，有可能会造成优先级反转或者饥饿现象。

// 无参
public ReentrantLock() { sync = new NonfairSync();
}
// 有参
public ReentrantLock(boolean fair) {
sync = fair ? new FairSync() : new NonfairSync();
}
1
2
3
4
5
6
7
8

两者区别
并发包中的 ReentrantLock 的创建可以指定构造函数 的 boolean 类型来得到公平锁或者非公平锁，默认是非公平锁！
公平锁：就是很公平，在并发环境中，每个线程在获取到锁时会先查看此锁维护的等待队列，如果为 空，或者当前线程是等待队列的第一个，就占有锁，否则就会加入到等待队列中，以后会按照 FIFO 的规则从队列中取到自己。
非公平锁：非公平锁比较粗鲁，上来就直接尝试占有锁，如果尝试失败，就会采用类似公平锁那种方 式。
Java ReentrantLock 而言，通过构造函数指定该锁是否是公平锁，默认是非公平锁。非公平锁的优点在于吞吐量比公平锁大。
对于 Synchronized 而言，也是一种非公平锁。

## 2、可重入锁

是什么
可重入锁（也叫递归锁）
指的是同一线程外层函数获得锁之后，内层递归函数仍然能获取该锁的代码，在同一个线程在外层方法 获取锁的时候，在进入内层方法会自动获取锁。
也就是说，线程可以进入任何一个它已经拥有的锁，所同步着的代码块。 好比家里进入大门之后，就可以进入里面的房间了；
ReentrantLock、Synchronized 就是一个典型的可重入锁；

### 可重入锁最大的作用就是避免死锁

测试一：Synchronized

| 1   | package com.kuang;                                                    |
| --- | --------------------------------------------------------------------- |
| 2   |                                                                       |
| 3   | /\*\*                                                                 |
| 4   | \* 可重入锁（也叫递归锁）                                             |
| 5   | \* 指的是同一线程外层函数获得锁之后，内层递归函数仍然能获取该锁的代码 |
| 6   | \* 在同一个线程在外层方法获取锁的时候，在进入内层方法会自动获取锁。   |
| 7   | \*/                                                                   |
| 8   | public class ReentrantLockDemo {                                      |
| 9   | public static void main(String[] args) throws Exception {             |
| 10  | Phone phone = new Phone();                                            |
| 11  |                                                                       |
| 12  | // T1 线程在外层获取锁时，也会自动获取里面的锁                        |
| 13  | new Thread(()->{                                                      |
| 14  | phone.sendSMS();                                                      |
| 15  | },"T1").start();                                                      |
| 16  |                                                                       |
| 17  | new Thread(()->{                                                      |
| 18  | phone.sendSMS();                                                      |
| 19  | },"T2").start();                                                      |
| 20  |                                                                       |
| 21  | }                                                                     |

| 22  | }                                                                  |
| --- | ------------------------------------------------------------------ |
| 23  |                                                                    |
| 24  | class Phone{                                                       |
| 25  | public synchronized void sendSMS(){                                |
| 26  | System.out.println(Thread.currentThread().getName()+" sendSMS");   |
| 27  | sendEmail();                                                       |
| 28  | }                                                                  |
| 29  |                                                                    |
| 30  | public synchronized void sendEmail(){                              |
| 31  | System.out.println(Thread.currentThread().getName()+" sendEmail"); |
| 32  | }                                                                  |
| 33  | }                                                                  |

测试二：ReentrantLock

| 1   | package com.kuang;                                                    |
| --- | --------------------------------------------------------------------- |
| 2   |                                                                       |
| 3   | import java.util.concurrent.locks.Lock;                               |
| 4   | import java.util.concurrent.locks.ReentrantLock;                      |
| 5   |                                                                       |
| 6   | /\*\*                                                                 |
| 7   | \* 可重入锁（也叫递归锁）                                             |
| 8   | \* 指的是同一线程外层函数获得锁之后，内层递归函数仍然能获取该锁的代码 |
| 9   | \* 在同一个线程在外层方法获取锁的时候，在进入内层方法会自动获取锁。   |
| 10  | \*/                                                                   |
| 11  | public class ReentrantLockDemo {                                      |
| 12  | public static void main(String[] args) throws Exception {             |
| 13  | Phone phone = new Phone();                                            |
| 14  |                                                                       |
| 15  | // T1 线程在外层获取锁时，也会自动获取里面的锁                        |
| 16  | new Thread(phone,"T1").start();                                       |
| 17  |                                                                       |
| 18  | new Thread(phone,"T2").start();                                       |
| 19  |                                                                       |
| 20  | }                                                                     |
| 21  | }                                                                     |
| 22  |                                                                       |
| 23  | class Phone implements Runnable{                                      |
| 24  |                                                                       |
| 25  | Lock lock = new ReentrantLock();                                      |
| 26  |                                                                       |
| 27  | @Override                                                             |
| 28  | public void run() {                                                   |
| 29  | get();                                                                |
| 30  | }                                                                     |
| 31  | public void get(){                                                    |
| 32  | lock.lock();                                                          |
| 33  | // lock.lock(); 锁必须匹配，如果两个锁，只有一个解锁就会失败          |
| 34  | try {                                                                 |
| 35  | System.out.println(Thread.currentThread().getName()+" get()");        |
| 36  | set();                                                                |
| 37  | } catch (Exception e) {                                               |
| 38  | e.printStackTrace();                                                  |
| 39  | } finally {                                                           |
| 40  | lock.unlock();                                                        |
| 41  | // lock.lock();                                                       |
| 42  | }                                                                     |
| 43  |                                                                       |

| 44  |     | }                                                              |
| --- | --- | -------------------------------------------------------------- |
| 45  |     | public void set(){                                             |
| 46  |     | lock.lock();                                                   |
| 47  |     | try {                                                          |
| 48  |     | System.out.println(Thread.currentThread().getName()+" set()"); |
| 49  |     | } catch (Exception e) {                                        |
| 50  |     | e.printStackTrace();                                           |
| 51  |     | } finally {                                                    |
| 52  |     | lock.unlock();                                                 |
| 53  |     | }                                                              |
| 54  |     |                                                                |
| 55  |     | }                                                              |
| 56  |     |                                                                |
| 57  | }   |                                                                |

## 3、自旋锁

自旋锁（spinlock）
是指尝试获取锁的线程不会立即阻塞，而是采用循环的方式去尝试获取锁，这样的好处是减少线程上下 文切换的消耗，缺点是循环会消耗 CPU。

| 1   | unsafe.getAndAddInt()                                                     |
| --- | ------------------------------------------------------------------------- |
| 2   | public final int getAndAddInt(Object var1, long var2, int var4) {         |
| 3   | int var5;                                                                 |
| 4   | do {                                                                      |
| 5   | // 获取传入对象的地址                                                     |
| 6   | var5 = this.getIntVolatile(var1, var2);                                   |
| 7   | // 比较并交换，如果 var1，var2 还是原来的 var5，就执行内存偏移+1； var5 + |
|     | var4                                                                      |
| 8   | } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));          |
| 9   |                                                                           |
| 10  | return var5;                                                              |
| 11  | }                                                                         |

测试代码：

| 1   | package com.kuang;                                                 |
| --- | ------------------------------------------------------------------ |
| 2   |                                                                    |
| 3   | import java.util.concurrent.TimeUnit;                              |
| 4   | import java.util.concurrent.atomic.AtomicReference;                |
| 5   |                                                                    |
| 6   | public class SpinLockDemo {                                        |
| 7   | // 原子引用线程, 没写参数，引用类型默认为 null                     |
| 8   | AtomicReference<Thread> atomicReference = new AtomicReference<>(); |
| 9   |                                                                    |
| 10  | //上锁                                                             |
| 11  | public void myLock(){                                              |
| 12  | Thread thread = Thread.currentThread();                            |
| 13  | System.out.println(Thread.currentThread().getName()+"==>mylock");  |
| 14  |                                                                    |
| 15  | // 自旋                                                            |
| 16  | while (!atomicReference.compareAndSet(null,thread)){               |
| 17  |                                                                    |
| 18  | }                                                                  |
| 19  | }                                                                  |

| 20  |     |                                                                     |
| --- | --- | ------------------------------------------------------------------- |
| 21  |     | //解锁                                                              |
| 22  |     | public void myUnlock(){                                             |
| 23  |     | Thread thread = Thread.currentThread();                             |
| 24  |     | atomicReference.compareAndSet(thread,null);                         |
| 25  |     | System.out.println(Thread.currentThread().getName()+"==>myUnlock"); |
| 26  |     | }                                                                   |
| 27  |     |                                                                     |
| 28  |     | // 测试                                                             |
| 29  |     | public static void main(String[] args) {                            |
| 30  |     |                                                                     |
| 31  |     | SpinLockDemo spinLockDemo = new SpinLockDemo();                     |
| 32  |     |                                                                     |
| 33  |     | new Thread(()->{                                                    |
| 34  |     | spinLockDemo.myLock();                                              |
| 35  |     | try {                                                               |
| 36  |     | TimeUnit.SECONDS.sleep(5);                                          |
| 37  |     | } catch (InterruptedException e) {                                  |
| 38  |     | e.printStackTrace();                                                |
| 39  |     | }                                                                   |
| 40  |     | spinLockDemo.myUnlock();                                            |
| 41  |     | },"T1").start();                                                    |
| 42  |     |                                                                     |
| 43  |     | try {                                                               |
| 44  |     | TimeUnit.SECONDS.sleep(1);                                          |
| 45  |     | } catch (InterruptedException e) {                                  |
| 46  |     | e.printStackTrace();                                                |
| 47  |     | }                                                                   |
| 48  |     |                                                                     |
| 49  |     | new Thread(()->{                                                    |
| 50  |     | spinLockDemo.myLock();                                              |
| 51  |     |                                                                     |
| 52  |     | try {                                                               |
| 53  |     | TimeUnit.SECONDS.sleep(1);                                          |
| 54  |     | } catch (InterruptedException e) {                                  |
| 55  |     | e.printStackTrace();                                                |
| 56  |     | }                                                                   |
| 57  |     |                                                                     |
| 58  |     | spinLockDemo.myUnlock();                                            |
| 59  |     | },"T2").start();                                                    |
| 60  |     |                                                                     |
| 61  |     | }                                                                   |
| 62  |     |                                                                     |
| 63  | }   |                                                                     |

## 4、死锁

死锁是什么
死锁是指两个或两个以上的进程在执行过程中，因争夺资源而造成的一种互相等待的现象，若无外力干 涉那它们都将无法推进下去，如果系统资源充足，进程的资源请求都能够得到满足，死锁出现的可能性 就很低，否者就会因为争夺有限的资源而陷入死锁。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834833997-598eb43d-3e2a-43ee-8ccd-3a22bc5eca17.jpeg#)

### 产生死锁主要原因：

1、系统资源不足
2、进程运行推进的顺序不合适
3、资源分配不当

测试

| 1   | import java.util.concurrent.TimeUnit;                                       |
| --- | --------------------------------------------------------------------------- |
| 2   |                                                                             |
| 3   | public class DeadLockDemo {                                                 |
| 4   | public static void main(String[] args) {                                    |
| 5   | String lockA = "lockA";                                                     |
| 6   | String lockB = "lockB";                                                     |
| 7   |                                                                             |
| 8   | new Thread(new HoldLockThread(lockA,lockB),"T1").start();                   |
| 9   | new Thread(new HoldLockThread(lockB,lockA),"T2").start();                   |
| 10  | }                                                                           |
| 11  | }                                                                           |
| 12  |                                                                             |
| 13  | class HoldLockThread implements Runnable{                                   |
| 14  |                                                                             |
| 15  | private String lockA;                                                       |
| 16  | private String lockB;                                                       |
| 17  |                                                                             |
| 18  | public HoldLockThread(String lockA, String lockB) {                         |
| 19  | this.lockA = lockA;                                                         |
| 20  | this.lockB = lockB;                                                         |
| 21  | }                                                                           |
| 22  |                                                                             |
| 23  | public void run() {                                                         |
| 24  | synchronized (lockA){                                                       |
| 25  |                                                                             |
|     | System.out.println(Thread.currentThread().getName()+"lock:"+lockA+"=>get"+l |
|     | ockB);                                                                      |
| 26  |                                                                             |

| 27  | try {                                                                       |
| --- | --------------------------------------------------------------------------- |
| 28  | TimeUnit.SECONDS.sleep(2);                                                  |
| 29  | } catch (InterruptedException e) {                                          |
| 30  | e.printStackTrace();                                                        |
| 31  | }                                                                           |
| 32  |                                                                             |
| 33  | synchronized (lockB){                                                       |
| 34  |                                                                             |
|     | System.out.println(Thread.currentThread().getName()+"lock:"+lockB+"=>get"+l |
|     | ockA);                                                                      |
| 35  | }                                                                           |
| 36  |                                                                             |
| 37  | }                                                                           |
| 38  | }                                                                           |
| 39  | }                                                                           |

解决

### 拓展 java 自带工具操作：

1、查看 JDK 目录的 bin 目录

2、使用
3、使用结果：
命令定位进程号
找到死锁查看
jps -l
jstack 进程号

| 1   | Java stack information for the threads listed above:        |
| --- | ----------------------------------------------------------- |
| 2   | ===================================================         |
| 3   | "T2":                                                       |
| 4   | at com.kuang.HoldLockThread.run(DeadLockDemo.java:43)       |
| 5   | - waiting to lock <0x00000000d5b87298> (a java.lang.String) |
| 6   | - locked <0x00000000d5b872d0> (a java.lang.String)          |
| 7   | at java.lang.Thread.run(Thread.java:748)                    |
| 8   | "T1":                                                       |
| 9   | at com.kuang.HoldLockThread.run(DeadLockDemo.java:43)       |
| 10  | - waiting to lock <0x00000000d5b872d0> (a java.lang.String) |
| 11  | - locked <0x00000000d5b87298> (a java.lang.String)          |
| 12  | at java.lang.Thread.run(Thread.java:748)                    |
| 13  |                                                             |
| 14  | Found 1 deadlock.                                           |

问 10 个人，9 个说看日志，还有一个分析堆栈信息，这一步，他就已经赢了！
