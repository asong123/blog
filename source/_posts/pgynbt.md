---
title: 33、JVM探究
urlname: pgynbt
date: '2021-07-09 20:45:55 +0800'
tags: []
categories: []
---

# 前言

**先给大家看几道面试题？**
1、请你谈谈你对 JVM 的理解？Java8 的虚拟机有什么更新？
2、什么是 OOM？什么是 StackOverFlowError？有哪些方法分析？ 3、JVM 的常用参数调优你知道哪些？
4、内存快照抓取和 MAT 分析 DUMP 文件知道吗？
5、堆里面的分区：Eden，Survival from to，老年代，各自的特点？
6、GC 的三种收集方法：标记清除，标记整理，复制算法的原理与特点，分别用在什么地方？

唠叨几句
每一个学习 JVM 的人，都渴望成功。每一个 Java 开发人员的终极目标都是在日常生活中深入理解 JVM 的运 行原理。JVM 和平时的应用框架明显的区别，应用框架学习之后，可以直接拿来写项目了，就可以运行 起来看到 helloworld。然而对于 JVM，是一个特别枯燥的事情，还看不到直接的效果，必须要写笔记， 因为一扭头就会忘记。
JVM 是一个令人望而却步的领域，因为它博大精深，涉及到的内容与知识点非常之多。虽然 Java 开发者 每天都在使用 JVM，但对其有所研究并且研究深入的人却少之又少。然而，JVM 的重要性却又是不言而喻 的。基于 JVM 的各种动态与静态语言生态圈已经异常繁荣了，对 JVM 的运行机制有一定的了解不但可以提 升我们的竞争力，还可以让我们在面对问题时能够沉着应对，加速问题的解决速度；同时还能够增强我 们的自信心，让我们更加游刃有余。
而且，如果我们想要进阶到技术专家或者更高等级，就必须要学习 JVM；

# JVM 的位置

首先我们来看看 JVM 在我们整个系统的位置：

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834763982-57093583-51cb-4362-97e6-aabbf723b4be.png#)
所以要理解一个问题：JVM 是运行在操作系统之上的，它与硬件没有直接的交互 假如你的电脑刚买来就有 Java 环境，那么说明这个电脑已经被人用过了！2333

# JVM 体系结构图

如果你不能够闭着眼睛画出 JVM 的体系结构图，说明你还没有入门 JVM：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834764402-199af639-3f74-4406-9296-e23b193ef9e4.png#)

### 课堂要求：在现场自己画出来这个图，能记下来多少记多少！

分析：这个区域一定不会有垃圾回收
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834764754-98d0ead6-4b6a-46b6-b1dd-f0086fe82f5f.png#)
所谓 JVM 的调优，其实就是在调这个区域，而且 99%情况下都在调堆 !

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834764977-06d1a8f8-58f5-4d28-88f3-d6d3075b0b60.png#)

在整个 JVM 的学习过程当中，希望大家可以在大脑中一直留着这幅图的印象！

# 类加载器 ClassLoader

我们先来看看一个类加载到 JVM 的一个基本结构：

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834765463-5a9ab874-0f95-4127-9454-edcdce3ff4f5.png#)
在如下几种情况下，Java 虚拟机将结束生命周期：

1. 执行了 System.exit()方法
1. 程序正常执行结束
1. 程序在执行过程中遇到了异常或者错误而异常终止
1. 由于操作系统出现错误而导致 Java 虚拟机进行终止

类的加载、连接与初始化
在 Java 代码中，**Class**的**加载、连接与初始化**过程都是在程序运行期间完成的。Runtime！ 加载： 查找并加载类的二进制数据
连接
验证：确保被加载的类的正确性
准备：为类的静态变量分配内存，并将其初始化为默认值解析：把类中的符号引用转换为直接引用

在编译的时候一个每个 java 类都会被编译成一个 class 文件，但在编译的时候虚拟机并不知道 所引用类的地址，多以就用符号引用来代替，而在这个解析阶段就是为了把这个符号引用转化 成为真正的地址的阶段。
1

初始化：为类的静态变量赋予正确的初始值

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834766042-00c52566-8053-4548-a711-d21465c1667d.jpeg#)

从代码来理解：

| 1   | class Test{                                                     |
| --- | --------------------------------------------------------------- |
| 2   | public static int a = 1;                                        |
| 3   | }                                                               |
| 4   | //我们程序中给定的是 public static int a = 1;                   |
| 5   | //但是在加载过程中的步骤如下：                                  |
| 6   |                                                                 |
| 7   | 1. 加载阶段                                                     |
| 8   | 编译文件为 .class 文件，然后通过类加载，加载到 JVM              |
| 9   |                                                                 |
| 10  | 2. 连接阶段                                                     |
| 11  | 第一步（验证）：确保 Class 类文件没问题                         |
| 12  | 第二步（准备）：先初始化为 a=0。（因为你 int 类型的初始值为 0） |
| 13  | 第三步（解析）：将引用转换为直接引用                            |
| 14  |                                                                 |
| 15  | 3. 初始化阶段：                                                 |
| 16  | 通过此解析阶段，把 1 赋值为变量 a                               |

类的加载
类的加载指的是将类的.class 文件中二进制数据读入到内存中，将其放在运行时数据区内的方法区内，然
后再内存中创建一个 对象用来封装类在方法区内的数据结构。
java.lang.Class

1. //对于静态字段来说，只有直接定义了该字段的类才会被初始化；
1. //当一个类在初始化时，要求其父类全部都已经初始化完毕了；
1. //所有 Java 虚拟机实现必须在每个类或者接口被 Java 程序“首次主动使用”时才初始化他们
1. public class MyTest1 {
1. public static void main (String[] args){
1. System.out.println(MyChild1.str2);

| 7   | }                                         |
| --- | ----------------------------------------- |
| 8   | }                                         |
| 9   |                                           |
| 10  | class MyParent1{                          |
| 11  | public static String str = "hello world"; |
| 12  | static {                                  |
| 13  | System.out.println("MyParent1 static");   |
| 14  | }                                         |
| 15  | }                                         |
| 16  |                                           |
| 17  | class MyChild1 extends MyParent1{         |
| 18  | public static String str2 = "welcome";    |
| 19  | static{                                   |
| 20  | System.out.println("MyChild1 static");    |
| 21  | }                                         |
| 22  | }                                         |
| 23  | // 输出结果：                             |
| 24  | MyParent1 static block                    |
| 25  | MyChild1 static block                     |
| 26  | welcome                                   |

查看类的加载信息，并打印出来：

| 1   | jvm 参数介绍：                                           |
| --- | -------------------------------------------------------- |
| 2   | -XX:+TraceClassLoading,用于追踪类的加载信息并打印出来。  |
| 3   |                                                          |
| 4   | 所有的参数都是：                                         |
| 5   | -XX:+<option> ， 表示开启 option 选项                    |
| 6   | -XX:-<option> ， 表示关闭 option 选项                    |
| 7   | -XX:+<option>=<value> 表示将 option 选项的值设置为 value |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834766540-8b38280a-9936-4c4d-b943-c85aeb895563.png#)
常量池的概念

我们先来看一道题：

| 1   | public class MyTest2{                           |
| --- | ----------------------------------------------- |
| 2   | public static void main(String[] args){         |
| 3   | System.out.println(MyParent2.str);              |
| 4   | }                                               |
| 5   | }                                               |
| 6   |                                                 |
| 7   | class MyParent2{                                |
| 8   | public static final String str = "hello world"; |
| 9   | static {                                        |

| 10  | System.out.println("Myparent2 static block");// 这一行能输出吗？不会                        |
| --- | ------------------------------------------------------------------------------------------- |
| 11  | }                                                                                           |
| 12  | }                                                                                           |
| 13  | /\*                                                                                         |
| 14  | 常量在编译阶段会存入到调用这个常量的方法所在的类的常量池中                                  |
| 15  | 本质上，调用类并没有直接用用到定义常量的类，因此并不会触发定义常量的类的初始化。            |
| 16  | 注意：这里指的是将常量存放到了 MyTest2 的常量池中，之后 MyTest2 与 MyParent2 就没有任何关系 |
|     | 了。                                                                                        |
| 17  | \*/                                                                                         |

再看一道题，类的初始化规则：

| 1   | /\*                                                                                          |
| --- | -------------------------------------------------------------------------------------------- |
| 2   | \* 当一个常量的值并非编译期间可以确定的，那么其值就不会被放到调用类的常量池中，              |
| 3   | 这是在程序运行时，会导致主动使用这个常量所在的类，显然就会导致这个类被初始化。               |
| 4   | \*/                                                                                          |
| 5   | public class MyTest3{                                                                        |
| 6   | public static void main(String[] args){                                                      |
| 7   | System.out.println(MyParent3.str);                                                           |
| 8   | }                                                                                            |
| 9   | }                                                                                            |
| 10  |                                                                                              |
| 11  | class MyParent3{                                                                             |
| 12  | public static final String str = UUID.randomUUID().toString();                               |
| 13  |                                                                                              |
| 14  | static {                                                                                     |
| 15  | System.out.println("Myparent3 static block"); // 这一行能输出吗？会                          |
| 16  | }                                                                                            |
| 17  | }                                                                                            |
| 18  |                                                                                              |
| 19  | 为什么第二个例子不会输出，第三个例子就输出了呢？                                             |
| 20  | 因为第三个例子的值，是只有当运行期才会被确定的值。而第二个例子的值，是编译时就能被确定的值。 |

ClassLoader 分类
有两种类型的类加载器
1、Java 虚拟机自带的加载器
根类加载器（BootStrap）(BootClassLoader) sun.boot.class.path （加载系统的包，包含 jdk 核心库里的类）
扩展类加载器（Extension）（ExtClassLoader） java.ext.dirs（加载扩展 jar 包中的类）
系统（应用）类加载器（System）(AppClassLoader) java.class.path（加载你编写的类，编译后的类）
2、用户自定义的类加载器
Java.long.ClassLoader 的子类（继承），用户可以定制类的加载方式

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834767048-930fa76d-00be-40ab-8e73-6222f3419676.png#)

### 代码测试

| 1   | public class ClassLoaderDemo01 {                                            |
| --- | --------------------------------------------------------------------------- |
| 2   | public static void main(String[] args) {                                    |
| 3   | Object object = new Object();                                               |
| 4   | ClassLoaderDemo01 demo01 = new ClassLoaderDemo01();                         |
| 5   |                                                                             |
| 6   | System.out.println(object.getClass().getClassLoader());                     |
| 7   | System.out.println(demo01.getClass().getClassLoader());                     |
| 8   | System.out.println(demo01.getClass().getClassLoader().getParent());         |
| 9   |                                                                             |
|     | System.out.println(demo01.getClass().getClassLoader().getParent().getParent |
|     | ());                                                                        |
| 10  |                                                                             |
| 11  | /\*                                                                         |
| 12  | 结果：                                                                      |
| 13  | null                                                                        |
| 14  | sun.misc.Launcher$AppClassLoader@18b4aac2                                   |
| 15  | sun.misc.Launcher$ExtClassLoader@1b6d3586                                   |
| 16  | null                                                                        |
| 17  | \*\*/                                                                       |
| 18  | }                                                                           |
| 19  | }                                                                           |

双亲委派机制
双亲委派机制的工作原理：一层一层的 让父类去加载，最顶层父类不能加载往下数，依次类推。

1. 类加载器收到类加载的请求；
1. 把这个请求委托给父加载器去完成，一直向上委托，直到启动类加载器；
1. 启动器加载器检查能不能加载（使用 ﬁndClass()方法），能就加载（结束）；否则，抛出异常，通 知子加载器进行加载。
1. 重复步骤三；

代码测试：

| 1   | package java.lang;                       |
| --- | ---------------------------------------- |
| 2   |                                          |
| 3   | public class String {                    |
| 4   | public static void main(String[] args) { |
| 5   | System.out.println(1);                   |
| 6   | }                                        |
| 7   | }                                        |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834767477-82e36ce2-42db-4c9e-9b2f-bc59e43cb3c3.png#)
大家所熟知的 String 类，直接告诉大家，String 默认情况下是启动类加载器进行加载的。假设我也自定义一个 String 。现在你会发现自定义的 String 可以正常编译，但是永远无法被加载运行。
这是因为申请自定义 String 加载时，总是启动类加载器，而不是自定义加载器，也不会是其他的加载器。
双亲委派机制可以确保 Java 核心类库所提供的类，不会被自定义的类所替代。

# Native 方法

编写一个多线程类启动

1
2
3
4
5
public static void main(String[] args) {
new Thread(()->{
},"your thread name").start();
}

点进去看 start 方法的源码

| 1   | public synchronized void start() {       |
| --- | ---------------------------------------- |
| 2   |                                          |
| 3   | if (threadStatus != 0)                   |
| 4   | throw new IllegalThreadStateException(); |
| 5   |                                          |
| 6   | group.add(this);                         |
| 7   |                                          |
| 8   | boolean started = false;                 |
| 9   | try {                                    |
| 10  | start0(); //调用了一个 start0 方法       |
| 11  | started = true;                          |
| 12  | } finally {                              |
| 13  | try {                                    |
| 14  | if (!started) {                          |
| 15  | group.threadStartFailed(this);           |

| 16  | }                                                                             |
| --- | ----------------------------------------------------------------------------- |
| 17  | } catch (Throwable ignore) {                                                  |
| 18  | }                                                                             |
| 19  | }                                                                             |
| 20  | }                                                                             |
| 21  |                                                                               |
| 22  | //这个 Thread 是一个类，这个方法定义在这里是不是很诡异！看这个关键字 native； |
| 23  | private native void start0();                                                 |

凡是带了 native 关键字的，说明 java 的作用范围达不到，去调用底层 C 语言的库！
**JNI：Java Native Interface （Java 本地方法接口） **凡是带了 native 关键字的方法就会进入本地方法栈； **Native Method Stack **本地方法栈
本地接口的作用是融合不同的编程语言为 Java 所用，它的初衷是融合 C/C++程序，Java 在诞生的时候是 C/C++横行的时候，想要立足，必须有调用 C、C++的程序，于是就在内存中专门开辟了一块区域处理标 记为 native 的代码，它的具体做法是 在 Native Method Stack 中登记 native 方法，在 ( Execution Engine ) 执行引擎执行的时候加载 Native Libraies。
目前该方法使用的越来越少了，除非是与硬件有关的应用，比如通过 Java 程序驱动打印机或者 Java 系统 管理生产设备，在企业级应用中已经比较少见。因为现在的异构领域间通信很发达，比如可以使用 Socket 通信，也可以使用 Web Service 等等，不多做介绍！

# 程序计数器

程序计数器：Program Counter Register
每个线程都有一个程序计数器，是线程私有的。
程序计数器是一块较小的内存空间，它的作用可以看作是当前线程所执行的字节码的行号指示器。在虚 拟机的概念模型里字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指 令，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。是一个非常小 的内存空间，几乎可以忽略不计

public class Calc { public int calc(){
int a = 100; int b = 200; int c = 300;
return ( a + b ) \* c;
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

反编译： 反编译之后会有助记符。
Javap -c xx.class
ldc 表示将 int、ﬂoat 或是 String 类型的常量值从常量池中推送至栈顶。
bipush 表示将单字节（-128~127）的常量值推送至栈顶。sipush 表示将短整型(-32767~32768)的常量值推送至栈顶。istore_1 将一个数值从操作数栈存储到局部变量表
iadd 加
imul 乘
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834767718-f74a2a91-bdd0-4c88-be91-0116b482d30c.png#)

图中使用红框框起来的就是字节码指令的**偏移地址**，偏移地址对应的**bipush **等等是 jvm 中的操作指令, 这是入栈指令。 当执行到方法**calc()**时在当前的线程中会创建相应的程序计数器，在计数器中为存放执行地址 （红框中的）0 2 3…等等

# 方法区

Method Area 方法区 是 Java 虚拟机规范 中定义的运行时数据区域之一，它与堆(heap)一样在线程之间共享。
Java 虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做 Non-Heap（非堆），目的应该是与 Java 堆区分开来。
JDK7 之前（永久代）用于存储已被虚拟机加载的类信息、常量、字符串常量、类静态变量、即时编译器编译后的代码等数据。每当一个类初次被加载的时候，它的元数据都会被放到永久代中。永久代大小有 限制，如果加载的类太多，很可能导致永久代内存溢出，即
PermGen。
java.lang.OutOfMemoryError:
JDK8 彻底将永久代移除出 HotSpot JVM，将其原有的数据迁移至 Java Heap 或 Native Heap（Metaspace），取代它的是另一个内存区域被称为元空间（Metaspace）。
元空间（Metaspace）：元空间是方法区的在 HotSpot JVM 中的实现，方法区主要用于存储类信息、常量池、方法数据、方法代码、符号引用等。元空间的本质和永久代类似，都是对 JVM 规范中方法区的实现。不过元空间与永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用本地内存。
可以通过 和 配置内存大小。
-XX:MetaspaceSize
-XX:MaxMetaspaceSize
如果 Metaspace 的空间占用达到了设定的最大值，那么就会触发 GC 来收集死亡对象和类的加载器。

# 栈（Stack）

栈和队列
在计算机流传有一句废话： 程序 = 算法 + 数据结构但是对于大部分同学都是： 程序 = 框架 + 业务逻辑

栈：后进先出 / 先进后出
队列：先进先出（FIFO : First Input First Output）

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834768004-18aa1f4f-7231-4fab-b6d9-f4e155e04af9.png#)

Stack 栈是什么

### 栈管理程序运行

存储一些基本类型的值、对象的引用、方法等。

### 栈的优势是，存取速度比堆要快，仅次于寄存器，栈数据可以共享。

思考：为什么 main 方法最后执行！为什么一个 test() 方法执行完了，才会继续走 main 方法！

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834768258-db66e77a-7943-46c4-a1f5-ab07aa5eabc0.png#)

### 喝多了吐就是栈，吃多了拉就是队列

说明：
1、栈也叫栈内存，主管 Java 程序的运行，是在线程创建时创建，它的生命期是跟随线程的生命期，线程 结束栈内存也就释放。
2、**对于栈来说不存在垃圾回收问题**，只要线程一旦结束，该栈就 Over，生命周期和线程一致，是线程 私有的。
3、方法自己调自己就会导致栈溢出（递归死循环测试）

1. public class StackDemo {
1. public static void main(String[] args) { 3 a();

4 }
5 public static void a(){ 6 b();
7 }
8 public static void b(){ 9 a();
10 }
11 }

**栈运行原理**
Java 栈的组成元素—栈帧
栈帧是一种用于帮助虚拟机执行方法调用与方法执行的数据结构。他是独立于线程的，一个线程有自己 的一个栈帧。封装了方法的局部变量表、动态链接信息、方法的返回地址以及操作数栈等信息。
第一个方法从调用开始到执行完成，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程。

当一个方法 A 被调用时就产生了一个栈帧 F1，并被压入到栈中，A 方法又调用了 B 方法，于是产生了栈帧
F2 也被压入栈中，B 方法又调用了 C 方法，于是产生栈帧 F3 也被压入栈中 执行完毕后，先弹出 F3， 然后弹出 F2，在弹出 F1........
遵循 “先进后出” / "后进先出" 的原则。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834768579-21f38120-f950-4370-9201-ac24356a91e3.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834769074-a7cda4a6-b0a9-45f3-a402-9a1be01c2665.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834769639-ecde8b5b-18d9-493c-99a0-60ce20feb227.jpeg#)

### 什么是 HotSpot？

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834770090-470ba59b-9529-43e9-9919-9e89e5655b06.png#)
**了解三种 JVM：**
Sun 公司的 HotSpot BEA 公司的 JRockit IBM 公司的 J9VM

# 堆（Heap）

### Java7 之前

Heap 堆，一个 JVM 实例只存在一个堆内存，堆内存的大小是可以调节的，类加载器读取了类文件后，需要把类，方法，常变量放到堆内存中，保存所有引用类型的真实信息，以方便执行器执行，堆内存分为 三部分：
新生区 Young Generation Space Young/New 养 老 区 Tenure generation space Old/Tenure 永久区 Permanent Space Perm
堆内存逻辑上分为三部分：新生，养老，永久（元空间 : JDK8 以后名称）

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834770293-68fdf46b-2651-4a2b-9435-d9be28906b81.png#)
**GC 垃圾回收主要是在 新生区和养老区**，又分为 轻 GC 和 重 GC，如果内存不够，或者存在死循环，就会导致
java.lang.OutOfMemoryError: Java heap space

新生区
新生区是类诞生，成长，消亡的区域，一个类在这里产生，应用，最后被垃圾回收器收集，结束生命。
新生区又分为两部分：伊甸区（Eden Space）和幸存者区（Survivor Space），所有的类都是在伊甸区被 new 出来的，幸存区有两个：0 区 和 1 区，当伊甸园的空间用完时，程序又需要创建对象，JVM 的垃圾回收器将对伊甸园区进行垃圾回收（Minor GC）。将伊甸园中的剩余对象移动到幸存 0 区，若幸存 0 区也满了，再对该区进行垃圾回收，然后移动到 1 区，那如果 1 区也满了呢？（这里幸存 0 区和 1 区是一个互相 交替的过程）再移动到养老区，若养老区也满了，那么这个时候将产生 MajorGC（Full GC），进行养老区的内存清理，若养老区执行了 Full GC 后发现依然无法进行对象的保存，就会产生 OOM 异常“OutOfMemoryError ”。
如果出现 java.lang.OutOfMemoryError：java heap space 异常，说明 Java 虚拟机的堆内存不够，原因如下：
1、Java 虚拟机的堆内存设置不够，可以通过参数 -Xms（初始值大小），-Xmx（最大大小）来调整。
2、代码中创建了大量大对象，并且长时间不能被垃圾收集器收集（存在被引用）或者死循环

Sun HotSpot 内存管理
分代管理，不同的区域使用不同的算法：

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834770635-f764fff4-56ed-447d-9883-c26fd5f7fe95.png#)
Why？真相：经过研究，不同对象的生命周期不同，在 Java 中 98%的对象都是临时对象。

永久区（Perm）
永久存储区是一个常驻内存区域，用于存放 JDK 自身所携带的 Class，Interface 的元数据，也就是说它存储的是运行环境必须的类信息，被装载进此区域的数据是不会被垃圾回收器回收掉的，关闭 JVM 才会释 放此区域所占用的内存。
如果出现 java.lang.OutOfMemoryError：PermGen space，说明是 Java 虚拟机对永久代 Perm 内存设置不够。一般出现这种情况，都是程序启动需要加载大量的第三方 jar 包，例如：在一个 Tomcat 下部署 了太多的应用。或者大量动态反射生成的类不断被加载，最终导致 Perm 区被占满。

### 注意：

Jdk1.6 之前： 有永久代，常量池 1.6 在方法区
Jdk1.7： 有永久代，但是已经逐步 “去永久代”，常量池 1.7 在堆 Jdk1.8 及之后：无永久代，常量池 1.8 在元空间

### 熟悉三区结构后方可学习 JVM 垃圾回收机制

实际而言，方法区（Method Area）和堆一样，是各个线程共享的内存区域，它用于存储虚拟机加载的：类信息+普通常量+静态常量+编译器编译后的代码，**虽然 JVM 规范将方法区描述为堆的一个逻辑部 分，但它却还有一个别名，叫做 Non-Heap（非堆），目的就是要和堆分开。**
对于 HotSpot 虚拟机，很多开发者习惯将方法区称之为 “永久代（Parmanent Gen）”，但严格本质上说两者不同，或者说使用永久代实现方法区而已，永久代是方法区（相当于是一个接口 interface）的一个 实现，Jdk1.7 的版本中，已经将原本放在永久代的字符串常量池移走。
常量池（Constant Pool）是方法区的一部分，Class 文件除了有类的版本，字段，方法，接口描述信息外，还有一项信息就是常量池，这部分内容将在类加载后进入方法区的运行时常量池中存放！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834770909-c96b9927-7b24-4502-897c-fdcc02ce79f3.png#)

# 堆内存调优

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834771169-2f484f2d-66b3-4f5e-ac79-533ca007e917.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834771412-dbd9e5cd-9d7b-44bd-b127-03a25fea3dbd.jpeg#)了解完基本的堆的信息之后，我们就可以简单学习下关于堆内存调优的说明了！我们是基于 HotSpot 虚拟机的，JDK1.8；
先看下 JDK 1.7 的 和 1.8 的区别
JDK 1.8 的

使用 IDEA 调整堆内存大小测试

### 堆内存调优

：设置初始分配大小，默认为物理内存的 “1/64”
-Xms
：最大分配内存，默认为物理内存的 “1/4”
-Xmx
：输出详细的 GC 处理日志
-XX:+PrintGCDetails

### 代码测试

| 1   | public class Demo01 {                                     |
| --- | --------------------------------------------------------- |
| 2   | public static void main(String[] args) {                  |
| 3   | //返回 Java 虚拟机试图使用的最大内存量                    |
| 4   | long maxMemory = Runtime.getRuntime().maxMemory();        |
| 5   | //返回 Java 虚拟机中的内存总量                            |
| 6   | long totalMemory = Runtime.getRuntime().totalMemory();    |
| 7   |                                                           |
| 8   | System.out.println("MAX_MEMORY="+maxMemory+"(字节)、"     |
| 9   | +(maxMemory/(double)1024/1024)+"MB");                     |
| 10  | System.out.println("TOTAL_MEMORY="+totalMemory+"(字节)、" |
| 11  | +(totalMemory/(double)1024/1024)+"MB");                   |
| 12  | }                                                         |
| 13  | }                                                         |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834771705-973a02e8-7819-4011-8ea8-28eddb792ee5.jpeg#)**IDEA 中进行 JVM 调优参数设置，然后启动**
发现，默认的情况下分配的内存是总内存的 1/4，而初始化的内存为 1/64 ！

-Xms1024m -Xmx1024m -XX:+PrintGCDetails
1

VM 参数调优：把初始内存，和总内存都调为 1024M，运行，查看结果！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834772287-55e77a20-3efc-42f9-9579-a6cd670f6208.png#)

我们来大概计算分析一下！

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834772732-99100194-32d3-4e77-b8c8-5ad2c835cc36.png#)

再次证明：元空间并不在虚拟机中，而是使用本地内存。

测试二
代码：

public class Demo02 {
public static void main(String[] args) { String str = "kuangShenSayJava"; while (true){
str += str + new Random().nextInt(88888888)
+new Random().nextInt(999999999);
}
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

vm 参数：

-Xms8m -Xmx8m -XX:+PrintGCDetails
1

测试，查看结果！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834773128-e4a73b12-44c9-4c13-b72b-53324413f4a1.jpeg#)
这是一个 young 区域撑爆的 JAVA 内存日志，其中 PSYoungGen 表示 youngGen 分区的变化 1536k 表示 GC 之前的大小。
488k 表示 GC 之后的大小。
整个 Young 区域的大小从 1536K 到 624K , young 代的总大小为 7680K。

[Times: user=0.02 sys=0.00, real=0.01 secs]
user – 总计本次 GC 总线程所占用的总 CPU 时间

sys – OS 调用 or 等待系统时间
real – 应用暂停时间
如果 GC 线程是 Serial Garbage Collector 串行搜集器的方式的话（只有一条 GC 线程,）， real time 等于 user 和 system 时间之和。
通过日志发现 Young 的区域到最后 GC 之前后都是 0，old 区域 无法释放，最后报堆溢出错误。

# Dump 内存快照

在运行 java 程序的时候，有时候想测试运行时占用内存情况，这时候就需要使用测试工具查看了。在 eclipse 里面有 **Eclipse Memory Analyzer tool(MAT)**插件可以测试，而在 idea 中也有这么一个插件， 就是**JProﬁler**，一款性能瓶颈分析工具！
作用：
分析 Dump 文件，快速定位内存泄漏； 获得堆中对象的统计数据
获得对象相互引用的关系
采用树形展现对象间相互引用的情况
......
而且这个软件跨平台：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834773451-16adeab6-660e-435e-8186-a91b491be242.jpeg#)

安装 JProﬁler
1、IDEA 插件安装
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834773792-967a74e9-175a-4dc5-9df4-f2d442a566bc.png#)
2、安装 JProﬁler 监控软件
下载地址：[https://www.ej-technologies.com/download/jproﬁler/version_92](https://www.ej-technologies.com/download/jprofiler/version_92)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834774132-3dbcff10-643c-4a90-979b-097d71e9b8ee.jpeg#)

3、下载完双击运行，选择自定义目录安装，点击 Next
注意：安装路径，**建议选择一个文件名中没有中文，没有空格的路径 **，否则识别不了。然后一直点 Next
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834774439-db2a98be-5f40-47a9-9203-cf9a2ea81425.png#)
4、注册

| 1   | // 注册码仅供大家参考                         |
| --- | --------------------------------------------- |
| 2   | L-Larry_Lau@163.com#23874-hrwpdp1sh1wrn#0620  |
| 3   | L-Larry_Lau@163.com#36573-fdkscp15axjj6#25257 |
| 4   | L-Larry_Lau@163.com#5481-ucjn4a16rvd98#6038   |
| 5   | L-Larry_Lau@163.com#99016-hli5ay1ylizjj#27215 |
| 6   | L-Larry_Lau@163.com#40775-3wle0g1uin5c1#0674  |

5、配置 IDEA 运行环境

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834774863-6dd5f3c9-318f-4feb-8642-0e6138740b07.jpeg#)*Settings–Tools–JProﬂier–JProﬂier executable*选择 JProﬁle 安装可执行文件。（如果系统只装了一个版本， 启动 IDEA 时会默认选择）保存
6、选择你要分析的项目，点击 JProﬁler 图标启动， 启动完成会自动弹出 JProﬁler 窗口，在里面就可以监控自己的代码性能了。

代码测试

| 1   | public class Demo03 {                                  |
| --- | ------------------------------------------------------ |
| 2   |                                                        |
| 3   | byte[] byteArray = new byte[1*1024*1024]; //1M = 1024K |
| 4   |                                                        |
| 5   | public static void main(String[] args) {               |
| 6   | ArrayList<Demo03> list = new ArrayList<>();            |
| 7   | int count = 0;                                         |
| 8   | try {                                                  |
| 9   | while (true){                                          |
| 10  | list.add(new Demo03());                                |
| 11  | count = count + 1;                                     |
| 12  | }                                                      |
| 13  | }catch (Error e){                                      |
| 14  | System.out.println("count:"+count);                    |
| 15  | e.printStackTrace();                                   |
| 16  | }                                                      |
| 17  | }                                                      |
| 18  | }                                                      |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834775236-afe46d5f-4882-4fc1-b5b2-0b591f9b2a25.png#)vm 参数 ： 运行结果：
-Xms1m -Xmx8m -XX:+HeapDumpOnOutOfMemoryError
寻找文件：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834775534-50a7e9a2-43f4-45ea-a5a8-b73950169406.jpeg#)

使用 Jproﬁler 工具分析查看
双击这个文件默认使用 Jproﬁler 进行 Open
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834776003-f306a7a5-ade3-407a-8abc-c1a2a3590a09.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834776480-496f36e1-8c23-409d-9158-c12e3726fbc2.jpeg#)大的对象！

具体的 Jproﬁler 使用参考：[https://www.cnblogs.com/jpfss/p/8488111.html](https://www.cnblogs.com/jpfss/p/8488111.html)

# GC 详解

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834777003-315aeac2-ac62-47b4-9ae8-66831b4d62fb.jpeg#)
回顾一下 GC 的作用域

记住 GC 口诀：
次数频繁 Young 区，次数较少 Old 区，基本不动 Perm（永久区）区
分代收集算法

GC 算法总体概述
先看下一个对象的历程：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834777437-0679d6ba-1541-4c8b-9827-219aab0a6265.jpeg#)

### JVM 在进行 GC 时，并非每次都对上面三个内存区域一起回收的，大部分时候回收的都是指新生代

因此 GC 按照回收的区域又分了两种类型，一种是普通的 GC（minor GC），一种是全局 GC （major GC or Full GC）
普通 GC：只针对新生代区域的 GC
全局 GC：针对老年代的 GC，偶尔伴随对新生代的 GC 以及对永久代的 GC

GC 面试题
1、JVM 内存模型以及分区，需要详细到每个区放什么
2、堆里面的分区：Eden，Survival from to，老年代，各自的特点。
3、GC 的三种收集方法：标记清除，标记整理，复制算法的原理与特点，分别用在什么地方？
4、Minor GC 与 Full GC 分别在什么时候发生?
很多的问题其实很简单，只是大家没有去研究而已，下面我们来聊聊几种垃圾回收方法！

# GC 四大算法

引用计数法 说明：了解即可！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834777721-7cac83ed-5b4d-40e7-ab90-d7fea9baebdb.jpeg#)
每个对象有一个引用计数器，当对象被引用一次则计数器加 1，当对象引用失效一次，则计数器减 1，对 于计数器为 0 的对象意味着是垃圾对象，可以被 GC 回收。
目前虚拟机基本都是采用可达性算法，从 GC Roots 作为起点开始搜索，那么整个连通图中的对象边都是活对象，对于 GC Roots 无法到达的对象变成了垃圾回收对象，随时可被 GC 回收。

复制算法（Copying）

### 年轻代中使用的是 Minor GC，采用的就是复制算法（Copying） 什么是复制算法？

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834778061-b4764ad5-926a-4921-ac58-3f90d24037a3.jpeg#)
Minor GC 会把 Eden 中的所有活的对象都移到 Survivor 区域中，如果 Survivor 区中放不下，那么剩下的活的对象就被移动到 Old generation 中，也就是说，一旦收集后，Eden 就是变成空的了
当对象在 Eden（包括一个 Survivor 区域，这里假设是 From 区域）出生后，在经过一次 Minor GC 后，如果对象还存活，并且能够被另外一块 Survivor 区域所容纳 （上面已经假设为 from 区域，这里应为 to 区域，即 to 区域有足够的内存空间来存储 Eden 和 From 区域中存活的对象），则使用复制算法将这些仍然还活着的对象复制到另外一块 Survivor 区域（即 to 区域）中，然后清理所使用过的 Eden 以及 Survivor 区域（即 form 区域），并且将这些对象的年龄设置为 1，以后对象在 Survivor 区，每熬过一次 Minor

GC，就将这个对象的年龄 + 1，当这个对象的年龄达到某一个值的时候（默认是 15 岁，通过- XX:MaxTenuringThreshold 设定参数）这些对象就会成为老年代。

-XX:MaxTenuringThreshold 任期门槛=>设置对象在新生代中存活的次数
面试题：如何判断哪个是 to 区呢？一句话：**谁空谁是 to**

### 原理解释：

年轻代中的 GC，主要是复制算法（Copying）
HotSpot JVM 把年轻代分为了三部分：一个 Eden 区 和 2 个 Survivor 区（from 区 和 to 区）。默认比例为 8:1:1，一般情况下，新创建的对象都会被分配到 Eden 区（一些大对象特殊处理），这些对象经过第一次 Minor GC 后，如果仍然存活，将会被移到 Survivor 区，对象在 Survivor 中每熬过一次 Minor GC ， 年龄就会增加 1 岁，当它的年龄增加到一定程度时，就会被移动到年老代中，因为年轻代中的对象基本上 都是朝生夕死，所以在年轻代的垃圾回收算法使用的是复制算法！复制算法的思想就是将内存分为两
块，每次只用其中一块，当这一块内存用完，就将还活着的对象复制到另外一块上面。复制算法不会产 生内存碎片！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834778529-63647182-a85f-4724-9b42-75cce0988faf.jpeg#)
在 GC 开始的时候，对象只会在 Eden 区和名为 “From” 的 Survivor 区，Survivor 区“TO” 是空的，紧接着进行 GC，Eden 区中所有存活的对象都会被复制到 “To” ， 而在 “From” 区中，仍存活的对象会更具他们的年龄值来决定去向。年龄达到一定值 的对象会被移动到老年代中，没有达到阈值的对象会被复制到 “To 区域”，经过这次 GC 后，Eden 区和 From 区已经被清空，这个时候， “From” 和 “To” 会交换他们的角色， 也就是新的 “To“ 就是 GC 前的”From“ ， 新的 ”From“ 就是上次 GC 前的 ”To“。不管怎样，都会保证名为 To 的 Survicor 区域是空的。 Minor GC 会一直重复这样的过程。直到 To 区 被填满 ， ”To “ 区被填满之后，会将所有的对象移动到老年代中。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834779035-aa842424-4654-48b4-9c08-e34b24543faf.jpeg#)

因为 Eden 区对象一般存活率较低，一般的，使用两块 10%的内存作为空闲和活动区域，而另外 80%的内 存，则是用来给新建对象分配内存的。一旦发生 GC，将 10%的 from 活动区间与另外 80%中存活的 Eden 对象转移到 10%的 to 空闲区域，接下来，将之前的 90%的内存，全部释放，以此类推；
好处：没有内存碎片，坏处：浪费内存空间

### 劣势：

复制算法它的缺点也是相当明显的。
1、他浪费了一半的内存，这太要命了
2、如果对象的存活率很高，我们可以极端一点，假设是 100%存活，那么我们需要将所有对象都复制一 遍，并将所有引用地址重置一遍。复制这一工作所花费的时间，在对象存活率达到一定程度时，将会变 的不可忽视，所以从以上描述不难看出。复制算法要想使用，最起码对象的存活率要非常低才行，而且 最重要的是，我们必须要克服 50%的内存浪费。

标记清除（Mark-Sweep）
说明：老年代一般是由标记清除或者是标记清除与标记整理的混合实现

### 什么是标记清除？

回收时，对需要存活的对象进行标记； 回收不是绿色的对象

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834779526-63ee8ddf-778a-42bd-b68d-fac72afc11bb.jpeg#)

当堆中的有效内存空间被耗尽的时候，就会停止整个程序（也被称为 stop the world），然后进行两项工作，第一项则是标记，第二项则是清除。
标记：从引用根节点开始标记所有被引用的对象，标记的过程其实就是遍历所有的 GC Roots ，然后将所有 GC Roots 可达的对象，标记为存活的对象。
清除： 遍历整个堆，把未标记的对象清除。
缺点：这个算法需要暂停整个应用，会产生内存碎片。
用通俗的话解释一下 标记/清除算法，就是当程序运行期间，若可以使用的内存被耗尽的时候，GC 线程就会被触发并将程序暂停，随后将依旧存活的对象标记一遍，最终再将堆中所有没被标记的对象全部清 除掉，接下来便让程序恢复运行。

### 劣势：

1. 首先、它的缺点就是效率比较低（递归与全堆对象遍历），而且在进行 GC 的时候，需要停止应用 程序，这会导致用户体验非常差劲
1. 其次、主要的缺点则是这种方式清理出来的空闲内存是不连续的，这点不难理解，我们的死亡对象 都是随机的出现在内存的各个角落，现在把他们清除之后，内存的布局自然乱七八糟，而为了应付 这一点，JVM 就不得不维持一个内存空间的空闲列表，这又是一种开销。而且在分配数组对象的时 候，寻找连续的内存空间会不太好找。

标记压缩（Mark-Compact）
标记整理说明：老年代一般是由标记清除或者是标记清除与标记整理的混合实现。

### 什么是标记压缩？

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834780024-7a69af8c-7e78-4551-953c-8749c8ad2770.jpeg#)

在整理压缩阶段，不再对标记的对象作回收，而是通过所有存活对象都像一端移动，然后直接清除边界 以外的内存。可以看到，标记的存活对象将会被整理，按照内存地址依次排列，而未被标记的内存会被 清理掉，如此一来，当我们需要给新对象分配内存时，JVM 只需要持有一个内存的起始地址即可，这比 维护一个空闲列表显然少了许多开销。
标记、整理算法不仅可以弥补 标记、清除算法当中，内存区域分散的缺点，也消除了复制算法当中，内存减半的高额代价；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834780547-ba86d2a9-9b61-44de-baeb-e4db0b78fcb2.jpeg#)
标记清除压缩（Mark-Sweep-Compact）
小总结

内存效率：复制算法 > 标记清除算法 > 标记整理算法 （时间复杂度） 内存整齐度：复制算法 = 标记整理算法 > 标记清除算法

内存利用率：标记整理算法 = 标记清除算法 > 复制算法

可以看出，效率上来说，复制算法是当之无愧的老大，但是却浪费了太多内存，而为了尽量兼顾上面所 提到的三个指标，标记整理算法相对来说更平滑一些 ， 但是效率上依然不尽如人意，它比复制算法多了一个标记的阶段，又比标记清除多了一个整理内存的过程。

难道就没有一种最优算法吗？猜猜看，下面还有
答案 ： 无，没有最好的算法，只有最合适的算法 。 > 分代收集算法

**年轻代：**（Young Gen）
年轻代特点是区域相对老年代较小，对象存活低。
这种情况复制算法的回收整理，速度是最快的。复制算法的效率只和当前存活对象大小有关，因而很适 用于年轻代的回收。而复制算法内存利用率不高的问题，通过 hotspot 中的两个 survivor 的设计得到缓解。

**老年代：**（Tenure Gen）
老年代的特点是区域较大，对象存活率高！
这种情况，存在大量存活率高的对象，复制算法明显变得不合适。一般是由标记清除或者是标记清除与 标记整理的混合实现。Mark 阶段的开销与存活对象的数量成正比，这点来说，对于老年代，标记清除或 者标记整理有一些不符，但可以通过多核多线程利用，对并发，并行的形式提标记效率。Sweep 阶段的 开销与所管理里区域的大小相关，但 Sweep “就地处决” 的 特点，回收的过程没有对象的移动。使其相对其他有对象移动步骤的回收算法，仍然是是效率最好的，但是需要解决内存碎片的问题。

# 常见面试题

## 1、JVM 垃圾回收的时候如何确定垃圾？是否知道什么是 GC Roots

什么是垃圾：简单的说就是内存中已经不再被使用到的空间就是垃圾。

| 1   | Person | person | =   | null; |
| --- | ------ | ------ | --- | ----- |

要进行垃圾回收，如何判断一个对象是否可以被回收？
**方法一：引用计数法（了解即可）**
Java 中，引用和对象是有关联的，如果要操作对象则必须用引用进行。
因此，很显然一个简单的办法是通过引用计数来判断一个对象是否可以进行回收。简单说，给对象中添 加一个引用计数器，每当有一个地方引用它，计数器值加 1，每当有一个引用失效时，计数器减 1。
任何时刻计数器值为零的对象就是不可能再被使用的，那么这个对象就是可回收对象。
那么为什么主流的 Java 虚拟机里面没有选用这种算法呢？其中最主要的原因是它很难解决对象之间相互循环引用的问题。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834781074-d1761af9-3925-46ff-8141-a0ccb786aeca.jpeg#)

**方法二：可达性分析算法**
为了解决引用计数法的循环引用问题，Java 使用了可达性分析的方法。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834781349-79f28d7f-0321-4ba1-b9f3-d748f4a75d69.jpeg#)
所谓 或者说 的 “根集合" 就是一组必须活跃的引用。
基本思路就是通过一系列名为 的对象作为起始点，从这个被称为 GC Roots 的对象开始向
GC roots
tracing GC
GC Roots
下搜索，如果一个对象到 GC Roots 没有任何引用链相连时，则说明此对象不可用。也即给定一个集合的引用作为根出发，通过引用关系遍历对象图，能被遍历到的（可到达的）对象就被判定为存活，没有 被遍历到的就自然被判定为死亡。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834781691-73213727-f9e6-4069-ae26-21f565b6eb4c.jpeg#)

### Java 中可以作为 GC Roots 的对象：（共 4 种）

1、虚拟机栈（栈帧中的局部变量表）中引用的对象；

2、方法区中的类静态属性引用的对象。
3、方法区中常量引用的对象。
4、本地方法栈中 JNI （Native 方法）引用的对象。

| 1   | public class GCrootDemo{                                   |
| --- | ---------------------------------------------------------- |
| 2   | private byte[] byteArray = new byte[100*1024*1024];        |
| 3   |                                                            |
| 4   | //private static GCrootDemo2 t2;                           |
| 5   | //private static final GCrootDemo3 t3 = new GCrootDemo3(); |
| 6   |                                                            |
| 7   | public static void m1(){                                   |
| 8   | GCrootDemo t1 = new GCrootDemo();                          |
| 9   | System.gc();                                               |
| 10  | System.out.println("第一次 GC 完成");                      |
| 11  | }                                                          |
| 12  |                                                            |
| 13  | public static void main(String[] args){                    |
| 14  | m1();                                                      |
| 15  | }                                                          |
| 16  |                                                            |
| 17  | }                                                          |

## 2、你说你做过 JVM 调优和参数配置，请问如何盘点查看 JVM 系统默认值

### JVM 的参数类型有三种：标配参数、X 参数、XX 参数；

**标配参数：**在 JDK 各个版本之间很稳定，很少有大的变化；

| 1   | -version     |
| --- | ------------ |
| 2   | -help        |
| 3   | -showversion |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834782202-7751ee2b-cf98-4420-820f-4ad7adc02975.png#)

**X 参数（了解）**

| 1   | -Xint   | #   | 解释执行                   |
| --- | ------- | --- | -------------------------- |
| 2   | -Xcomp  | #   | 第一次使用就编译成本地代码 |
| 3   | -Xmixed | #   | 混合模式                   |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834782615-605216f5-9191-4c70-90b7-38727f043aea.jpeg#)

**XX 参数之 Boolean 类型 **公式： + 表示开启，- 表示关闭。
-XX: + 或者 - 某个属性值
我们来启动代码测试：

| 1   | package com.kuang.gc;                                                |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | public class GCDemo01 {                                              |
| 4   | public static void main(String[] args) throws InterruptedException { |
| 5   | System.out.println("Hello");                                         |
| 6   | Thread.sleep(Integer.MAX_VALUE);                                     |
| 7   | }                                                                    |
| 8   | }                                                                    |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834782915-3b6e4216-9ba2-4388-8562-33bb2ff22705.png#)如何查看一个正则运行中的 Java 程序，它的某个 JVM 参数是否开启？具体值是多少？ 得到当前正在运行的进程编号
jps -l
jinfo -flag 进程号
查看正则运行中的 Java 程序，它的某个 JVM 参数是否开启
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834783261-0fee0dfe-4d26-4bb0-b140-4a009ff53bf2.png#)
我们停止程序，然后增加 VM 参数：

-XX:+PrintGCDetails
1

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834783431-8dfb73a9-baf1-42bb-85e8-1ef6e80ff35c.png#)启动后再次测试，看是否开启！
-XX:+PrintFlagsFinal
小结：
1、是否打印 GC 收集细节：PrintGCDetails 2、是否使用串行垃圾回收器：UseSerialGC

**XX 参数之 KV 设值类型 **公式：
-XX: 属性 key=属性值 value
1、 元空间大小
2、 进老年区存活次数判定
-XX:MetaspaceSize=128m
-XX:MaxTenuringThreshold=15
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834783810-944abe1b-21d0-4c50-8abc-4aacc8243e3f.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834784332-752b67a3-c789-4f75-84d3-54fa3f1b101d.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834784898-952c2bd3-0c7b-4ac5-8682-ffc8faee79aa.jpeg#)

美团面试题：两个经典参数： ，请问这个怎么解释呢？考察你有没有去研究过！
1、-Xms 等价于 -XX:InitialHeapSize 初始堆大小
-Xms 和 -Xmx
2、-Xmx 等价于 -XX:MaxHeapSize 最大对大小
-XX:+PrintFlagsInitial

查看初始默认值
查看 Java 环境初始默认值
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834785438-5e1101a9-7906-45be-be79-5529bc295fa5.jpeg#)
java -XX:+PrintFlagsInitial
1

查看修改更新

| 1   | java -XX:+PrintFlagsFinal -version             |      |     |
| --- | ---------------------------------------------- | ---- | --- |
| 2   | // 具体执行 后面是要修改的参数， Test 要运行的 | Java | 类  |
| 3   | java -XX:+PrintFlagsFinal -Xss128K Test        |      |     |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834785839-2d143370-573f-4d65-b641-2a5d404d6b23.png#)
程序运行前打印出用户手动设置或者 JVM 自动设置
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834786341-872fb8d9-286d-44b8-9bab-a036199726e4.jpeg#)的 XX 选项
java -XX:+PrintCommandLineFlags -version

### 聊天鸡汤：

1、要想人前显贵，必定人后遭罪
2、今天的最好表现是对你们明天的最低要求
3、大家出去的时候以后一定要坐飞机，有的人还没坐过飞机，你以前出去，都是坐客车，坐火车出去 的，无论如何今年对自己好点，坐一次飞机，先投资自己的脑袋，在丰富自己的口袋。你去看看汽车 站，火车站，飞机站的旅客的区别，你看看你想过哪一种的生活，客车拥挤，你听过飞机卖站票的吗？ 你现在的努力，你现在的辛苦就是为了以后更好的生活！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834786895-8264a893-e0cb-4381-9917-3c8aad85bc70.jpeg#)
-XX:MaxHeapSize
-XX:TheadStackSize

## 3、你平时工作用过的 JVM 常用基本配置参数有哪些？

-Xms
-XX:InitialHeapSize
初始内存大小，默认为物理内存的 1/64，等价于

-Xmx
最大内存大小，默认为物理内存的 1/4，等价于

-Xss
设置单个线程栈的大小，一般默认为 512k ~ 1024k，等价于

-Xmn
设置年轻代大小，一般不用动

-XX:MetaspaceSize
设置元空间大小：
元空间的本质和永久代类似，都是对 JVM 规范中方法区的实现。不过元空间与永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用本地内存。因此，默认情况下，元空间的大小仅受本地内存限 制。
使用：
-XX:MetaspaceSize=512m

-XX:+PrintGCDetails
输出详细 GC 收集日志信息；输出参数说明如下：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834787447-db1ab2ee-e571-4d1e-a63a-f017001a6fae.jpeg#)
-XX:SurvivorRatio

设置新生代中 eden 和 s0/s1 空间的比例； 默认
假如
-XX:SurvivorRatio=8，Eden:S0:S1=8:1:1
-XX:SurvivorRatio=4，Eden:S0:S1=4:1:1
SurvivorRatio 值就是设置 eden 区的比例占多少，s0/s1 相同
-XX:NewRatio
配置年轻代与老年代在堆结构的占比；

| 默认 | -XX:NewRatio=2 | 新生代占 1，老年代 2，年轻代占整个堆的 1/3 |
| ---- | -------------- | ------------------------------------------ |
| 假如 | -XX:NewRatio=4 | 新生代占 1，老年代 4，年轻代占整个堆的 1/5 |

NewRatio 值就是这只老年代的占比，剩下的 1 给新生代
-XX:MaxTenuringThreshold
设置进入老年区的年龄限制，必须在 0~15 之间，默认 15；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834787948-49a8adf5-2620-44ef-9518-54c8d79ba156.jpeg#)

## 4、强引用、软引用、弱引用、虚引用分别是什么？

整体架构

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834788461-2ca8cc8a-1fec-48a8-a226-2ead2e3fbb5f.jpeg#)

强引用（默认支持模式）
当内存不足，JVM 开始垃圾回收，对于强引用的对象，就是出现了 OOM 也不会对该对象进行回收，死都 不收。
强引用是我们最常见的普通引用，只要还有强引用指向一个对象，就能表明对象还活着，垃圾收集器不 会碰这种对象。在 Java 中最常见的就是强引用，把一个对象赋值给一个引用变量，这个引用变量就是一个强引用，当一个对象被强引用时，它处在可达状态，它是不可能被垃圾回收机制回收的，即使该对象 以后永远都不会被用到，JVM 也不会回收。因此强引用是造成 Java 内存泄漏的主要原因之一。
对于一个普通的对象，如果没有其他的引用关系，只要超过了引用的作用域或者显示的将相应（强）引 用赋值为 null，一般认为就是可以被垃圾收集的了（当然具体回收时机还是要看垃圾收集策略）。

| 1   | Object obj1 = new Object(); // 这样定义的默认就是强引用 |
| --- | ------------------------------------------------------- |
| 2   | Object obj2 = obj1; // obj2 引用赋值                    |
| 3   | obj1 = null; // 置空                                    |
| 4   | System.gc();                                            |
| 5   | System.out.println(obj2); // 正常输出                   |

软引用（SoftReference）
软引用是一种相对强引用弱化了一些的引用，需要调用 java.lang.ref.SoftReference 类来实现，可以让对象豁免一些垃圾收集。
对于只有软引用的对象来说：
会

当系统内存充足时它
不会
被回收，当系统内存不足时它
被回收。

软引用通常在对应内存敏感的程序中，比如高速缓存就有用到软引用，内存够用的时候就保留，不够用 就回收！

| 1   | package com.kuang.gc;               |
| --- | ----------------------------------- |
| 2   |                                     |
| 3   | import java.lang.ref.SoftReference; |
| 4   |                                     |
| 5   | public class SoftReferenceDemo {    |
| 6   |                                     |

| 7   |     | public static void softRef_Memory_Enough(){                    |
| --- | --- | -------------------------------------------------------------- |
| 8   |     | Object o1 = new Object();                                      |
| 9   |     | SoftReference<Object> softReference = new SoftReference<>(o1); |
| 10  |     | System.out.println(o1);                                        |
| 11  |     | System.out.println(softReference.get());                       |
| 12  |     |                                                                |
| 13  |     | o1 = null;                                                     |
| 14  |     | System.gc();                                                   |
| 15  |     |                                                                |
| 16  |     | System.out.println(o1);                                        |
| 17  |     | System.out.println(softReference.get());                       |
| 18  |     | }                                                              |
| 19  |     |                                                                |
| 20  |     | /\*                                                            |
| 21  |     | JVM 配置，让它内存不够                                         |
| 22  |     | -Xms5m -Xmx5m -XX:+PrintGCDetails                              |
| 23  |     | \*/                                                            |
| 24  |     | public static void softRef_Memory_NotEnough(){                 |
| 25  |     | Object o1 = new Object();                                      |
| 26  |     | SoftReference<Object> softReference = new SoftReference<>(o1); |
| 27  |     | System.out.println(o1);                                        |
| 28  |     | System.out.println(softReference.get());                       |
| 29  |     |                                                                |
| 30  |     | o1 = null;                                                     |
| 31  |     | //System.gc();                                                 |
| 32  |     |                                                                |
| 33  |     | try {                                                          |
| 34  |     | byte[] bytes = new byte[30*1024*1024];                         |
| 35  |     | } catch (Exception e) {                                        |
| 36  |     | e.printStackTrace();                                           |
| 37  |     | } finally {                                                    |
| 38  |     | System.out.println(o1);                                        |
| 39  |     | System.out.println(softReference.get());                       |
| 40  |     | }                                                              |
| 41  |     | }                                                              |
| 42  |     |                                                                |
| 43  |     | public static void main(String[] args) {                       |
| 44  |     | softRef_Memory_NotEnough();                                    |
| 45  |     | }                                                              |
| 46  | }   |                                                                |

弱引用 WeakReference
弱引用需要用 java.lang.ref.WeakReference 类来实现，它比软引用的生存周期更短，对于只有弱引用的对象来说，只要垃圾回收机制一运行，不管 JVM 的内容空间是否足够，都会回收该对象占用的内存；

| 1   | public static void main(String[] args) {                       |
| --- | -------------------------------------------------------------- |
| 2   | Object o1 = new Object();                                      |
| 3   | WeakReference<Object> weakReference = new WeakReference<>(o1); |
| 4   | System.out.println(o1);                                        |
| 5   | System.out.println(weakReference.get());                       |
| 6   |                                                                |
| 7   | o1 = null;                                                     |
| 8   | System.gc();                                                   |
| 9   |                                                                |
| 10  | System.out.println(o1);                                        |
| 11  | System.out.println(weakReference.get());                       |
| 12  | }                                                              |

软引用和弱引用的适用场景
假如有一个应用需要读取大量的本地图片：
1、如果每次读取图片都要从硬盘读取则会严重影响性能；
2、如果一次性全部加载到内存中又可能造成内存溢出。此时适用软引用可以解决这类问题：
设计思路是：用一个 HashMap 来保存图片的路径和相应图片对象关联的软引用之间的映射关系，在内存 不足时，JVM 会自动回收这些缓存图片对象所占用的空间，从而有效地避免了 OOM 的问题。

Map<String,SoftReference<Bitmap>> imageCache
= new HashMap<String,SoftReference<Bitmap>>();
1
2

虚引用 PhantomReference
虚引用需要使用 java.lang.PhantomReference 类来实现。
顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。
如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收，它 不能单独使用也不能通过它访问对象，虚引用必须和引用队列（ReferenceQueue）联合使用。
虚引用的主要作用是跟踪对象被垃圾回收的状态。仅仅是提供了一种确保对象被 ﬁnalize 以后，做某些事情的机制。PhantomReference 的 get 方法总是返回 null，因此无法访问对应的引用对象。其意义在于说明一个对象已经进入 ﬁnalization 阶段，可以被 gc 回收，用来实现比 ﬁnalization 机制更灵活的回收操作。
换句话说，设置虚引用关联的唯一目的，就是在这个对象被收集器回收的时候收到一个系统通知或者后 续添加进一步的处理。
Java 技术允许使用 ﬁnalize() 方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。

| 1   | public static void main(String[] args) throws InterruptedException { |
| --- | -------------------------------------------------------------------- |
| 2   | Object o1 = new Object();                                            |
| 3   | ReferenceQueue<Object> referenceQueue = new ReferenceQueue<>();      |
| 4   | PhantomReference<Object> phantomReference = new PhantomReference<>   |
|     | (o1,referenceQueue);                                                 |
| 5   |                                                                      |
| 6   | System.out.println(o1); // java.lang.Object@1b6d3586                 |

| 7   |     | System.out.println(phantomReference.get()); // null                     |
| --- | --- | ----------------------------------------------------------------------- |
| 8   |     | System.out.println(referenceQueue.poll()); // null                      |
| 9   |     |                                                                         |
| 10  |     | System.out.println("===================");                              |
| 11  |     | o1 = null;                                                              |
| 12  |     | System.gc();                                                            |
| 13  |     | TimeUnit.SECONDS.sleep(1);                                              |
| 14  |     |                                                                         |
| 15  |     | System.out.println(o1); // null                                         |
| 16  |     | System.out.println(phantomReference.get()); // null                     |
| 17  |     | System.out.println(referenceQueue.poll()); // java.lang.Object@1b6d3586 |
| 18  |     |                                                                         |
| 19  | }   |                                                                         |

Java 提供了 4 中引用类型，在垃圾回收的时候，都有自己各自的特点。ReferenceQueue 是用来配合引用工作的，没有 ReferenceQueue 一样可以运行。
创建引用的时候可以指定关联的队列，当 GC 释放对象内存的时候，会将引用加入到引用队列，如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行 动。这相当于是一种通知机制。
当关联的引用队列中有数据的时候，意味着引用指向的堆内存中的对象被回收。通过这种方式，JVM 允许我们在对象被销毁后，做一些我们自己想做的事情。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834788712-8a509630-77cb-4495-a2a3-f0ed33be5fc2.jpeg#)
一幅图小总结

## 5、请你谈谈你对 OOM 的认识？

java.lang.StackOverﬂowError
public static void main(String[] args){ a();
}
public static void a(){ a();
}
1
2
3
4
5
6

java.lang.OutOfMemoryError：Java heap space
// -Xms10m -Xmx10m
public static void main(String[] args){ String str = "kuangshen"; while(true){
str += str + new Random().nextInt(11111111) + new Random().nextInt(11111111);
}
}
1
2
3
4
5

6
7

java.lang.OutOfMemoryError：GC overhead limit exceeded 超过 GC 开销限制
GC 回收时间过长时会抛出 OutOfMemoryError。过长的定义是，超过 98%的时间用来做 GC 并且回收了不到 2% 的堆内存，连续多次 GC 都只回收了不到 2% 的极端情况下才会抛出，假如不抛出 GC overhead limit exceeded 错误会发生什么情况呢？那就是 GC 清理的这么点内存很快会再次填满，迫使 GC 再次执行，这样就形成恶性循环，CPU 使用率一直是 100%，而 GC 却没有任何成果。

| 1   | // -Xms10m -Xmx10m -XX:MaxDirectMemorySize=5m -XX:+PrintGCDetails    |
| --- | -------------------------------------------------------------------- |
| 2   | public static void main(String[] args) throws InterruptedException { |
| 3   | int i = 0;                                                           |
| 4   | List<String> list = new ArrayList<>();                               |
| 5   |                                                                      |
| 6   | try {                                                                |
| 7   | while (true){                                                        |
| 8   | list.add(String.valueOf(++i).intern());                              |
| 9   | }                                                                    |
| 10  | } catch (Throwable e) {                                              |
| 11  | System.out.println("i=>"+i);                                         |
| 12  | e.printStackTrace();                                                 |
| 13  | throw e;                                                             |
| 14  | }                                                                    |
| 15  | // java.lang.OutOfMemoryError: GC overhead limit exceeded            |
| 16  | }                                                                    |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834789011-7e085afe-1117-4992-a95b-58e96751745d.png#)

java.lang.OutOfMemoryError：Direct buﬀer memory 直接缓冲存储
写 NIO 程序经常使用 ByteBuﬀer 来读取或者写入数据，这是一种基于通道（channel）与缓冲区
（Buﬀer）的 I/O 方式，它可以使用 Native 函数库直接分配堆外内存，然后通过一个存储在 Java 堆里面的 DirectByteBuﬀer 对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在 Java 堆和 Native 堆中来回复制数据。
第一种方式是分配 JVM 堆内存，属于 GC 管辖范围，由于需要
ByteBuffer.allocate(capability)

拷贝所以速度相对较慢。
ByteBuffer.allocateDirect(capability)

围，由于不需要拷贝所以速度相对较快。

第二种方式是分配 OS 本地内存，不属于 GC 管辖范

但如果不断分配本地内存，堆内存很少使用，那么 JVM 就不需要执行 GC，DirectByteBuﬀer 对象们就不会被回收，这时候堆内存充足，但本地内存可能已经使用光了，再次尝试分配本地内存就会出现 OutOfMemoryError，那程序就直接崩溃了。

| 1   | // -Xms10m -Xmx10m -XX:MaxDirectMemorySize=5m -XX:+PrintGCDetails    |
| --- | -------------------------------------------------------------------- |
| 2   | public static void main(String[] args) throws InterruptedException { |
| 3   | System                                                               |
| 4   | .out                                                                 |
| 5   | .println("配置的 maxDirectMemory："+                                 |
|     | (sun.misc.VM.maxDirectMemory()/(double)1014/1024)+"MB");             |
| 6   |                                                                      |
| 7   | TimeUnit.SECONDS.sleep(2);                                           |
| 8   | // -Xms10m -Xmx10m -XX:MaxDirectMemorySize=5m -XX:+PrintGCDetails    |
| 9   | // 我们配置的 5M，但是实际使用的 6M，故意搞破坏                      |
| 10  | ByteBuffer byteBuffer = ByteBuffer.allocateDirect(6 _ 1024 _ 1024);  |
| 11  | // java.lang.OutOfMemoryError: Direct buffer memory                  |
| 12  | }                                                                    |

java.lang.OutOfMemoryError：unable to create new native thread 无法创建新的本地线程
高并发请求服务器时，经常出现如下异常：java.lang.OutOfMemoryError：unable to create new native thread
准确的讲，该 native thread 异常与对应的平台有关；

### 导致原因：

1、你的应用创建了太多线程了，一个应用进程创建多个线程，超过系统承载极限
2、你的服务器并不允许你的应用程序创建这么多线程，Linux 系统默认允许单个进程可以创建的线程数是 1024 个，你的应用创建超过这个数量，就会报 java.lang.OutOfMemoryError：unable to create new native thread

### 解决办法：

1、想办法降低你的应用程序创建线程的数量，分析应用是否真的需要创建这么多线程，如果不是，改代 码将线程数降到最低！
2、对于有的应用，确实需要创建很多线程，远超过 Linux 系统的默认 1024 个线程的限制，可以通过修改 Linux 服务器配置，扩大 Linux 默认限制！

1. //在 Linux 虚拟机下操作，使用非 root 用户测试，因为 root 用户是无上限创建线程的。
1. public static void main(String[] args) { 3 for (int i = 1; ; i++) {
1. System.out.println("i=>"+i);
1. new Thread(()->{
1. try {
1. Thread.sleep(Integer.MAX_VALUE);
1. } catch (InterruptedException e) {
1. e.printStackTrace(); 10 }

11 },""+i).start();
12 }
13 }

java.lang.OutOfMemoryError：Metaspace
Java 8 及之后的版本使用 Metaspcae 来替代永久代。
Metaspace 是方法区在 HotSpot 中的实现，它与持久代最大的区别在于：Metaspace 并不在虚拟机内存中而是使用本地内存。
永久代（java8 后被原空间 Metaspace 取代了）存放了以下信息：
1、虚拟机加载的类信息
2、常量池
3、静态变量
4、即时编译后的代码
模拟 Metaspace 空间溢出，我们不断生成类往元空间灌，类占据的空间总是会超过 Metaspace 指定的空间大小的。

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
import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor; import org.springframework.cglib.proxy.MethodProxy;
import java.lang.reflect.Method;

// 注意要导入 spring 的核心包！
// -XX:MetaspaceSize=10m -XX:MaxMetaspaceSize=10m public class Test{

static class OOMTest{ }
public static void main(final String[] args) { int i = 0; //模拟计数器，来查看多少次以后发生异常 try{
while(true){ i++;
// Spring 的 cglib 动态代理技术
Enhancer enhancer = new Enhancer(); enhancer.setSuperclass(OOMTest.class); enhancer.setUseCache(false); enhancer.setCallback(new MethodInterceptor() {
public Object intercept(Object o, Method method,
Object[] objects, MethodProxy methodProxy) throws Throwable {

| 23 |

} |

} | return method.invoke(o,args);
}
});
enhancer.create();
}
}catch(Throwable e){ System.out.println("i=>"+i); e.printStackTrace();
} |
| --- | --- | --- | --- |
| 24 | | | |
| 25 | | | |
| 26 | | | |
| 27 | | | |
| 28 | | | |
| 29 | | | |
| 30 | | | |
| 31 | | | |
| 32 | | | |
| 33 | | | |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834789494-72659ac3-62f1-4bbd-8440-0c2b4c37936a.png#)

## 6、GC 垃圾回收算法和垃圾收集器的关系？分别是什么？

GC 算法（引用计数、复制、标记清除、标记整理）是内存回收的方法论，垃圾收集器就是算法的落地实 现；
因为目前为止还没有完美的收集器出现，更加没有万能的收集器，只是针对具体应用最合适的收集器， 进行分代收集；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834789985-58bb53a5-422f-437c-8a69-963c49648d9c.jpeg#)
4 种主要垃圾收集器

串行垃圾回收器(Serial)

它为单线程环境设计且只使用一个线程进行垃圾回收，会暂停所有的用户线程。所以不适合服务器环 境。

并行垃圾回收器(Parallel)
多个垃圾收集线程并行工作，此时用户线程是暂停的，适用于科学计算、大数据处理首台处理等弱交互 场景。

并发垃圾回收器(CMS)
用户线程和垃圾收集线程同时执行（不一定是并行，可能交替执行），不需要停顿用户线程，互联网公 司多用它，适用对响应时间有要求的场景。

G1 垃圾回收器
G1 垃圾回收器将堆内存分割成不同的区域然后并发的对其进行垃圾回收

## 7、谈谈垃圾收集器

怎么查看默认的垃圾收集器是哪个？
java -XX:+PrintCommandLineFlags -version
命令行输入：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834790537-0d201e86-3e5b-4cf9-944e-efd0694c5c1d.jpeg#)
默认的垃圾收集器有哪些?

java 的 gc 回收的类型主要有几种：
、 、 、
UseSerialGC 、
UseParallelOldGC 、 UseG1GC
UseParallelGC
UseConcMarkSweepGC
UseParNewGC

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834790795-b03fa6f1-0aad-48f0-9279-af39ec606ec2.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834791063-e0e86048-0ed1-4399-b649-d4ed834374a6.jpeg#)
垃圾收集器

垃圾收集器就来具体实现这些 GC 算法并实现内存回收。
不同厂商、不同版本的虚拟机实现差别很大，HotSpot 中包含的收集器如下图所示：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834791655-f1d80ad7-b49c-4b85-b664-59a9ebfc715a.jpeg#)

部分参数说明
DefNew => Default New Generation 【默认新一代】
Tenured => Old 【老年代】
ParNew => Parallel New Generation 【并行新一代】PSYoungGen => Parallel Scavenge 【并行清除年轻区】ParOldGen => Parallel Old Generation 【并行老年区】

Server / Client 模式分别是什么意思？
适用范围：只需要掌握 Server 模式即可，Client 模式基本不会用操作系统：
32 位 Window 操作系统，不论硬件如何都默认使用 Client 的 JVM 模式。
32 位其他操作系统，2G 内存同时有 2 个 cpu 以上用 Server 模式，低于该配置还是 Client 模式。
64 位都是 Server 模式。

新生代

### 串行 GC（Serial 收集器）/ （Serial Copying）

一句话：一个单线程的收集器，在进行垃圾收集时候，必须暂停其他所有的工作线程直到它收集结束。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834792176-559e7c9f-4334-49ff-9752-4595fe2a1a79.jpeg#)

串行收集器是最古老，最稳定以及效率高的收集器，只使用一个线程去回收但其在进行垃圾收集过程中 可能会产生较长的停顿，Stop-The-World，虽然在手机垃圾过程中需要暂停所有其他的工作线程，但是 它简单高效，对于限定单个 CPU 环境来说，没有线程交互的开销可以获得最高的单线程垃圾收集效率， 因此 Serial 垃圾收集器依然是 java 虚拟机运行在 Client 模式下默认的新生代垃圾收集器。
对应 JVM 参数是：
-XX:+UseSerialGC
开启后会使用：Serial（Young 区用）+ Tenured（Old 区用）的收集器组合
表示：新生代、老年代都会使用串行回收收集器，新生代使用复制算法，老年代使用标记-整理算法；
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834792374-a702a306-9238-43aa-87b1-bffdbee0a60e.png#)
-Xms10m -Xmx10m -XX:+PrintGCDetails -XX:+PrintCommandLineFlags -
XX:+UseSerialGC
1

### 并行 GC（ParNew）

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834792720-aa07e696-9909-470e-b9f6-4c0d04636132.jpeg#)一句话：使用多线程进行垃圾回收，在垃圾收集时，会 Stop-The-World 暂停其他所有的工作线程直到它收集结束。
ParNew 收集器其实就是 Serial 收集器新生代的并行多线程版本，最常见的应用场景是配合老年代的 CMG GC 工作，其余的行为和 Serial 收集器完全一样，ParNew 垃圾收集器在垃圾收集过程中同样也要暂停所有其他的工作线程。它是很多 java 虚拟机运行在 Server 模式下新生代的默认垃圾收集器。

常用对应 JVM 参数 ： 代。
-XX:+UseParNewGC
启用 ParNew 收集器，只影响新生代的收集，不影响老年

开启上述参数后，会使用：ParNew（Young 区用）+ Tenured 的收集器组合，新生代使用复制算法，老年代采用标记-整理算法。
但是 ParNew + Tenured 这样的搭配， Java8 已经不再被推荐；

### 说明：

| 1   | -XX:ParallelGCThreads=N 限制线程数量，默认开启和 CPU 数目相同的线程数。也可以通过 N 开启自 |
| --- | ------------------------------------------------------------------------------------------ |
|     | 定义的线程数                                                                               |
| 2   | cpu>8 N = 5/8                                                                              |
| 3   | cpu<8 N = 实际个数                                                                         |
| 4   |                                                                                            |
| 5   | -Xms10m -Xmx10m -XX:+PrintGCDetails -XX:+UseParNewGC                                       |

警告：
Java HotSpot(TM) 64-Bit Server VM warning: Using the ParNew young collector with the Serial old collector is deprecated and will likely be removed in a future release

### 并行回收 GC（Parallel）/（Parallel Scavenge）

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834793254-500ec2b0-8c03-40ea-9f7e-f4fa46017a33.jpeg#)
Parallel Scavenge 收集器类似 ParNew ，也是一个新生代垃圾收集器，使用复制算法，也是一个并行的多线程的垃圾收集器，俗称吞吐量优先收集器，一句话：串行收集器在新生代和老年代的并行化。
它重点关注的是：
可控制的吞吐量，比如程序运行 100 分钟，垃圾收集时间 1 分钟，吞吐量就是 99%。高吞吐量意味着高效利用 CPU 的时间，它多用于在后台运算而不需要太多交互的任务。
自适应调节策略也是 ParallelScavenge 收集器与 ParNew 收集器的一个重要区别。虚拟机会根据当前系统的运行情况收集性能监控信息，动态调整这些参数以提供最合适的停顿时间（-XX： MaxGCPauseMillis）或最大的吞吐量。
-XX：+UseParallelOldGC

常用 JVM 参数： 或
-XX：+UseParallelGC
ParallelScavenge 收集器。
（可互相激活）使用

开启该参数后：新生代使用复制算法，老年代使用标记-整理算法。

老年代

### 串行 GC（Serial Old）/ （Serial MSC）

Serial Old 是 Serial 垃圾收集器老年代版本，它同样是单个单线程的收集器，使用标记-整理算法，这个收集器也主要是运行在 Client 默认的 java 虚拟机默认的年老代垃圾收集器。
在 Server 模式下，主要有两个用途（了解，版本已经到 8 及以后）：
1、在 JDK 1.5 之前版本中与新生代的 Parallel Scavenge 收集器搭配使用。（Parallel Scavenge + Serial Old）
2、作为老年代版本中使用 CMS 收集器的后备垃圾收集方案。

### 并行 GC（Parallel Old）/ （Parallel MSC）

Parallel Old 收集器是 Parallel Scavenge 的老年代版本，使用多线程的标记-整理算法，Parallel Old 收集器在 JDK1.6 才开始提供。
在 JDK1.6 之前，新生代使用 Parallel Scavenge 收集器只能搭配老年代的 Serial Old 收集器，只能保证新生代的吞吐量优先，无法保证整体的吞吐量。在 JDK 1.6 之前（ Parallel Scavenge + Serial Old）。

Parallel Old 正是为了在年老代同样提供吞吐量优先的垃圾收集器，如果系统对吞吐量要求比较高， JDK1.8 后可以优先考虑新生代 Parallel Scavenge 和老年代 Parallel Old 收集器的搭配策略。在 JDK 1.8 及以后（ Parallel Scavenge + Parallel Old）
JVM 常用参数：
使用 Parallel Old 收集器，设置该参数后，新生代 Parallel Scavenge + 老
-XX:+UseParallelOldGC
年 代 Parallel Old

### 并发标记清除 GC（CMS）

CMS 收集器（Concurrent Mark Sweep：并发标记清除）是一种以获取最短回收停顿时间为目标的收集器。
适合应用在互联网站或者 B/S 系统的服务器上，这类应用尤其重视服务器的响应速度，希望系统停顿时间最短。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834793717-ef017cbf-b6bf-4a6d-a91e-67baa9ca1dd6.jpeg#)CMS 非常适合堆内存大、CPU 核数多的服务器端应用，也是 G1 出现之前大型应用的首选收集器。
Concurrent Mark Sweep 并发标记清除，并发收集低停顿，并发指的是与用户线程一起执行。

开启该收集器的 JVM 参数：
-XX:+UseConcMarkSweepGC
XX:+UseParNewGC 打开；
，开启该参数后会自动将 -

开启该参数后，使用 ParNew（Young 区用）+CMS（Old 区用）+ Serial Old 的收集器组合，Serial Old
将作为 CMS 出错的后备收集器。

| 1   | -Xms10m | -Xmx10m | -XX:+PrintGCDetails | -XX:UseConcMarkSweepGC |
| --- | ------- | ------- | ------------------- | ---------------------- |

优点：并发收集停顿低缺点：
1、并发执行，对 CPU 资源压力大

1. 由于并发进行，CMS 在收集与应用线程会同时增加对内存的占用，也就是说，CMS 必须要在老年代堆内存用 尽之前完成垃圾回收
1. 否则 CMS 回收失败时，将触发担保机制，串行老年代收集器将会以 STW 的方式进行一次 GC，从而造成较大停顿时间。

2、采用的标记清除算法会导致大量碎片

| 1   | 标记清除算法无法整理空间碎片，老年代空间会随着应用时长被逐步耗尽，最后将不得不通过担保机制对 |
| --- | -------------------------------------------------------------------------------------------- |
|     | 堆内存进行压缩。CMS 也提供了参数                                                             |
| 2   | -XX:CMSFullGCsBeForeCompaction(默认 0，即每次都进行内存整理) 来指定多少次 CMS 收集之         |
|     | 后，进行一次压缩的 Full GC.                                                                  |

GC 之如何选择垃圾收集器
1、单 CPU 或小内存，单机程序

-XX:+UseSerialGC
2、多 CPU，需要最大吞吐量，如后台计算型应用
或者
-XX:+UseParallelGC
-XX:+UseParakkekOldGC
3、多 CPU，追求低停顿时间，需要快速响应如互联网应用
或
-XX:+UseConcMarkSweepGC
-XX:+ParNewGC

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834794268-017dd96d-e3cb-4ec3-97d6-daf5e684cc69.jpeg#)

## 8、G1 垃圾收集器

以前收集器特点
1、年轻代和老年代是各自独立且连续的内存块；
2、年轻代收集使用单 eden + s0 + s1 进行复制算法；
3、老年代收集必须扫描整个老年代区域；
4、都是以尽可能少而快速地执行 GC 为设计原则。

G1 是什么？
G1（Garbage-First）收集器，是一款面向服务端应用的收集器；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834794783-87f255a1-8196-4b50-8205-fa7b95425f68.jpeg#)

从官网的描述中，我们知道 G1 是一种服务器端的垃圾收集器，应用在多处理器和大容量内存环境中， 在实现高吞吐量的同时，尽可能的满足垃圾收集暂停时间的要求。另外，它还具有以下特性：
1、像 CMS 收集器一样，能与应用程序线程并发执行。
2、整理空闲空间更快
3、需要更多的时间来预测 GC 停顿时间。
4、不希望牺牲大量的吞吐性能。 5、不需要更大的 Java Heap。
**G1 收集器的设计目标是取代 CMS 收集器**，它同 CMS 相比，在以下方面表现的更出色：
G1 是一个有整理内存过程的垃圾收集器，不会产生很多内存碎片。
G1 的 Stop The World 更可控，G1 在停顿时间上添加了预测机制，用户可以指定期望停顿时间。

CMS 垃圾收集器虽然减少了暂停应用程序的运行时间，但是它还存在着垃圾碎片问题。于是，为了去除内存碎片问题，同时又保留 CMS 垃圾收集器低暂停时间的优点，Java7 发布了一个新的垃圾收集器-G1 垃圾收集器。
G1 是在 2012 年才在 JDK1.7u4 中可用，oralce 官方计划在 jdk9 中将 G1 变成默认的垃圾收集器以替代 CMS。它是一款面向服务端应用的收集器，主要应用在多 CPU 和大内存服务器环境下，极大的减少垃圾 收集的停顿时间，全面提升服务器的性能，逐步替换 java8 以前的 CMS 收集器。
主要改变是 Eden，Survivor 和 Tenured 等内存区域不再是连续的了，而是变成了一个个大小一样的 region，每个 region 从 1M 到 32M 不等。一个 region 有可能属于 Eden，Survivor 或者 Tenured 内存区域。

特点
1、G1 能充分利用多 CPU、多核环境硬件优势，尽量缩短 STW。
2、G1 整体上采用标记-整理算法，局部是通过复制算法，不会产生内存碎片。
3、宏观上 G1 之中不再区分年轻代和老年代，把内存划分成多个独立的子区域（Region），可以近似理 解为一个围棋的棋盘。
4、G1 收集器里面将整个的内存区都混合在一起了，但其本身依然在小范围内要进行年轻代和老年代的 区分，保留了新生代和老年代，但他们不再是物理隔离的，而是一部分 Region 的集合且不需要 Region 是连续的。也就是说依然会采用不同的 GC 方式来处理不同的区域。
5、G1 虽然也是分代收集器，但整个内存分区不存在物理上的年轻代与老年代的区别，也不需要完全独 立的 survivor（to space）堆做复制准备。G1 只有逻辑上的分代概念，或者说每个分区都可能随 G1 的运行在不同代之间前后切换。

底层原理
Region 区域化垃圾收集器：最大好处是化整为零，避免全内存扫描，只需要按照区域来进行扫描即可。 区域化内存划片 Region，整体编为了一系列不连续的内存区域，避免了全内存区的 GC 操作。

核心思想是将整个堆内存区域分成大小相同的子区域（Region），在 JVM 启动时会自动设置这些子区域的大小，在堆的使用上，G1 并不要求对象的存储一定是物理上连续的，只要逻辑上连续即可。每个分区 也不会固定地为某个代服务，可以按需在年轻代和老年代之间切换。启动时可以通过参数 - XX:G1HeapRegionSize=n 可指定分区大小（1MB~32MB，且必须是 2 的幂），默认将整堆划分为 2048 个分区。
大小范围在 1MB ~ 32MB，最多能设置 2048 个区域，也即能够支持的最大内存为：32MB \* 2048 = 64G 内存！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834795306-083aa680-50b8-45cf-82a2-982ce7e41ed3.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834795775-f58d1394-9378-45ce-b289-8067f34f07b2.jpeg#)
回收步骤

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834796352-3fcd7f5a-36e7-4580-81e2-3d8f56bba5b5.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834796854-80db4351-a62c-46ee-a74d-42f40e7c683e.jpeg#)
常用配置参数

开发人员仅仅需要声明以下参数即可：
开启 G1=>设置最大内存=>设置最大停顿时间

-XX:+UseG1GC -Xmx32g -XX:MaxGCPauseMillis=100
1

最大 GC 停顿时间单位毫秒，JVM 将尽可能（不保证）停顿小于这个时
-XX:MaxGCPauseMillis=n
间。

和 CMS 相比的优势
1、G1 不会产生内存碎片
2、是可以精确控制停顿，该收集器是把整个堆（新生代、老年代）划分成多个固定大小的区域，每次根 据允许停顿的时间去收集垃圾最多的区域。
