---
title: 8、JavaSE：常用类
urlname: ggwy9l
date: '2021-07-09 20:36:18 +0800'
tags: []
categories: []
---

# 前言

ok,各位同学们,那么从今天开始呢，我们就正式开始迈入 Java 常用类的学习了.之前呢，我们已经学习了面向对象相关的知识，也算正式踏入了 Java 开发的大门了，那么后面的课程都属于是高级部分学习了！
我们会学习：常用类，集合框架，IO 流，多线程，网络编程，注解和反射，以及 GUI 编程！ 其实，Java 的学习，除了思想，本质还是在学习一个个类的使用！
我们来看下，这一章会学习哪些内容呢？
首先，我们要学习 Math 类，这里面有许多关于数学操作的方法，然后我们会学习时间和日期类，使用 Java 来获得时间相关的对象，然后会给大家讲解 String 类，这个我们从学 java 的第一天就接触的类，我们 其实并未真正的了解，包括它的一些扩展 StringBuilder 和 StringBuﬀer，接下来就是我们的包装类了， 之前学习的 8 大基本数据类型都有其对应的包装类，还有自动装箱和拆箱的原理，我们需要知道。然后就 是所有类的老祖宗类：Object 的探究及分析，最后给大家讲讲 File 类，为之后会学习的 IO 流打个基础！
这些就是我们这个阶段会学习的一些东西了！是一个很重要的阶段，以后开发中，这些都是天天用的东 西，不能不会！

# Object 类

大家都知道 Object 是所有类的父类，任何类都默认继承 Object。
理论上 Object 类是所有类的父类，即直接或间接的继承 java.lang.Object 类。由于所有的类都继承在 Object 类，因此省略了 extends Object 关键字。
该类中主要有以下方法: toString() getClass() equals()
clone() ﬁnalize(
其中 toString(),getClass(),equals 是其中最重要的方法。
【演示：查看 Object 类源码】

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834179913-c811c902-4828-4b85-8747-c60f6ed60705.png#)

**注意： Object 类中的 getClass(),notify(),notifyAll(),wait()等方法被定义为 ﬁnal 类型，因此不能重写。**
**1、clone() 方法**
[详 解 文 章 ：https://blog.csdn.net/zhangjg_blog/article/details/18369201#0-qzone-1-28144-d020d2 d2a4e8d1a374a433f596ad1440](https://blog.csdn.net/zhangjg_blog/article/details/18369201#0-qzone-1-28144-d020d2d2a4e8d1a374a433f596ad1440)

| 1   | protected | native | Object | clone() | throws | CloneNotSupportedException; |
| --- | --------- | ------ | ------ | ------- | ------ | --------------------------- |

clone 顾名思义就是复制， 在 Java 语言中， clone 方法被对象调用，所以会复制对象。所谓的复制对象， 首先要分配一个和源对象同样大小的空间，在这个空间中创建一个新的对象。那么在 java 语言中，有几 种方式可以创建对象呢？
使用 new 操作符创建一个对象使用 clone 方法复制一个对象
那么这两种方式有什么相同和不同呢？ new 操作符的本意是分配内存。程序执行到 new 操作符时， 首先去看 new 操作符后面的类型，因为知道了类型，才能知道要分配多大的内存空间。分配完内存之后， 再调用构造函数，填充对象的各个域，这一步叫做对象的初始化，构造方法返回后，一个对象创建完
毕，可以把他的引用（地址）发布到外部，在外部就可以使用这个引用操纵这个对象。而 clone 在第一步 是和 new 相似的， 都是分配内存，调用 clone 方法时，分配的内存和源对象（即调用 clone 方法的对象） 相同，然后再使用原对象中对应的各个域，填充新对象的域， 填充完成之后，clone 方法返回，一个新的相同的对象被创建，同样可以把这个新对象的引用发布到外部。

#### clone 与 copy 的区别

假设现在有一个 Employee 对象，Employee tobby =new Employee(“CMTobby”,5000)
通常我们会有这样的赋值 Employee cindyelf=tobby，这个时候只是简单了 copy 了一下 reference， cindyelf 和 tobby 都指向内存中同一个 object，这样 cindyelf 或者 tobby 的一个操作都可能影响到对方。打 个比方，如果我们通过 cindyelf.raiseSalary()方法改变了 salary 域的值，那么 tobby 通过 getSalary()方法 得到的就是修改之后的 salary 域的值，显然这不是我们愿意看到的。我们希望得到 tobby 的一个精确拷 贝，同时两者互不影响，这时候， 我们就可以使用 Clone 来满足我们的需求。Employee cindy=tobby.clone()，这时会生成一个新的 Employee 对象，并且和 tobby 具有相同的属性值和方法。

#### Shallow Clone 与 Deep Clone

主要是 JAVA 里除了 8 种基本类型传参数是值传递，其他的类对象传参数都是引用，我们有时候不希望在 方法里将参数改变，这是就需要在类中复写 clone 方法（实现深复制）。
Clone 是如何完成的呢？Object 在对某个对象实施 Clone 时对其是一无所知的，它仅仅是简单地执行域 对域的 copy，这就是 Shallow Clone。这样，问题就来了咯。
以 Employee 为例，它里面有一个域 hireDay 不是基本数据类型的变量，而是一个 reference 变量，经过
Clone 之后就会产生一个新的 Date 型的 reference，
它和原始对象中对应的域指向同一个 Date 对象，这样克隆类就和原始类共享了一部分信息，而这样显然 是不利的，过程下图所示：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834180300-39e83fed-5b23-4743-bc53-ae71cd12e885.jpeg#)
这个时候我们就需要进行 deep Clone 了，对那些非基本类型的域进行特殊的处理，例如本例中的 hireDay。我们可以重新定义 Clone 方法，对 hireDay 做特殊处理，如下代码所示：

class Employee implements Cloneable {
public Object clone() throws CloneNotSupportedException { Employee cloned = (Employee) super.clone(); cloned.hireDay = (Date) hireDay.clone()
return cloned;
}
}
1
2
3
4
5
6
7

#### clone 方法的保护机制

在 Object 中 Clone()是被声明为 protected 的，这样做是有一定的道理的，以 Employee 类为例，通过声明 为 protected，就可以保证只有 Employee 类里面才能“克隆”Employee 对象.

#### clone 方法的使用

什么时候使用 shallow Clone，什么时候使用 deep Clone，这个主要看具体对象的域是什么性质的，基本型别还是 reference variable
调用 Clone()方法的对象所属的类(Class)必须 implements Clonable 接口，否则在调用 Clone 方法的时候会抛出 CloneNotSupportedException

## 2、toString()方法

public String toString() {
return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
1
2
3

Object 类的 toString 方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at 标记符“@” 和此对象[哈希码](https://baike.baidu.com/item/%E5%93%88%E5%B8%8C%E7%A0%81/5035512?fr=aladdin)的无符号十六进制表示组成。
该方法用得比较多，**一般子类都有覆盖。**
我们推荐在学习阶段所有有属性的类都加上 toString() 方法！

public static void main(String[] args){ Object o1 = new Object(); System.out.println(o1.toString());
}
1
2
3
4

## 3、getClass()方法

| 1   | public | final | native | Class<?> | getClass(); |
| --- | ------ | ----- | ------ | -------- | ----------- |

返回次 Object 的运行时类类型。
不可重写，要调用的话，一般和 getName()联合使用，如 getClass().getName();

1. public static void main(String[] args) {
1. Object o = new Object();
1. System.out.println(o.getClass());
1. //class java.lang.Object 5 }

## 4、ﬁnalize()方法

| 1   | protected | void | finalize() | throws | Throwable | {   | }   |
| --- | --------- | ---- | ---------- | ------ | --------- | --- | --- |

该方法用于释放资源。因为无法确定该方法什么时候被调用，很少使用。
Java 允许在类中定义一个名为 ﬁnalize()的方法。它的工作原理是：一旦垃圾回收器准备好释放对象占用 的存储空间，将首先调用其 ﬁnalize()方法。并且在下一次垃圾回收动作发生时，才会真正回收对象占用 的内存。
关于垃圾回收，有三点需要记住：
1、对象可能不被垃圾回收。只要程序没有濒临存储空间用完的那一刻，对象占用的空间就总也得不 到释放。
2、垃圾回收并不等于“析构”。
【科普：析构函数(destructor) 与构造函数相反，当对象结束其生命周期，如对象所在的函数已调用完毕时，系统自动执行析构函数。析构函数往往用来做“清理善后” 的工作（例如在建立对象时用 new 开辟了一片内存空间，delete 会自动调用析构函数后释放内存）。】
3、垃圾回收只与内存有关。使用垃圾回收的唯一原因是为了回收程序不再使用的内存。

#### ﬁnalize()的用途：

无论对象是如何创建的，垃圾回收器都会负责释放对象占据的所有内存。
这就将对 ﬁnalize()的需求限制到一种特殊情况，即通过某种创建对象方式以外的方式为对象分配了存储 空间。不过这种情况一般发生在使用“本地方法”的情况下，本地方法是一种在 Java 中调用非 Java 代码的方式。

## 5、equals()方法

public boolean equals(Object obj) { return (this == obj);
}
1
2
3

Object 中的 equals 方法是直接判断 this 和 obj 本身的值是否相等，即用来判断调用 equals 的对象和形参 obj 所引用的对象是否是同一对象，
所谓同一对象就是指内存中同一块存储单元，如果 this 和 obj 指向的 hi 同一块内存对象，则返回 true,如果
this 和 obj 指向的不是同一块内存，则返回 false。
注意：即便是内容完全相等的两块不同的内存对象，也返回 false。
如果是同一块内存，则 object 中的 equals 方法返回 true,如果是不同的内存，则返回 false
如果希望不同内存但相同内容的两个对象 equals 时返回 true,则我们需要重写父类的 equal 方法
String 类已经重写了 object 中的 equals 方法（这样就是比较内容是否相等了）
【演示：查看 String 类源码 equals 方法】

public boolean equals(Object anObject) { if (this == anObject) {
return true;
}
if (anObject instanceof String) {
String anotherString = (String)anObject; int n = value.length;
if (n == anotherString.value.length) { char v1[] = value;
char v2[] = anotherString.value; int i = 0;
while (n-- != 0) {
if (v1[i] != v2[i]) return false;
i++;
}
return true;
}
}
return false;
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
17
18
19
20
21

## 6、hashCode()方法

| 1   | public | native | int | hashCode(); |
| --- | ------ | ------ | --- | ----------- |

返回该对象的哈希码值。
该方法用于哈希查找，可以减少在查找中使用 equals 的次数，重写了 equals 方法一般都要重写
hashCode 方法。这个方法在一些具有哈希功能的 Collection 中用到。
一般必须满足 obj1.equals(obj2)==true。可以推出 obj1.hash Code() == obj2.hashCode()，但是 hashCode 相等不一定就满足 equals。不过为了提高效率，应该尽量使上面两个条件接近等价。

## wait()方法

1 public final void wait() throws InterruptedException { 2 wait(0);
3 }
狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 4   | public final native void wait(long timeout) throws InterruptedException;    |
| --- | --------------------------------------------------------------------------- | --- | ----------------- |
| 5   | public final void wait(long timeout, int nanos) throws InterruptedException |
|     | {                                                                           |
| 6   | if (timeout < 0) {                                                          |
| 7   | throw new IllegalArgumentException("timeout value is negative");            |
| 8   | }                                                                           |
| 9   |                                                                             |
| 10  | if (nanos < 0                                                               |     | nanos > 999999) { |
| 11  | throw new IllegalArgumentException(                                         |
| 12  | "nanosecond timeout value out of range");                                   |
| 13  | }                                                                           |
| 14  |                                                                             |
| 15  | if (nanos > 0) {                                                            |
| 16  | timeout++;                                                                  |
| 17  | }                                                                           |
| 18  |                                                                             |
| 19  | wait(timeout);                                                              |
| 20  | }                                                                           |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834180692-a6e98f49-66a0-40d1-9ab9-908a4843695f.png#)可以看到有三种重载，wait 什么意思呢？
方法中的异常：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834180922-e83a7edd-753c-4895-b800-9a5a1ac19082.png#)
wait 方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait()方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，
如果在规定时间内没有获得锁就返回。
调用该方法后当前线程进入睡眠状态，直到以下事件发生。

1. 其他线程调用了该对象的 notify 方法。
1. 其他线程调用了该对象的 notifyAll 方法。
1. 其他线程调用了 interrupt 中断该线程。
1. 时间间隔到了。

此时该线程就可以被调度了，如果是被中断的话就抛出一个 InterruptedException 异常。

## notify()方法

| 1   | public | final | native | void | notify(); |
| --- | ------ | ----- | ------ | ---- | --------- |

该方法唤醒在该对象上等待的某个线程。

| 1   | public | final | native | void | notifyAll(); |
| --- | ------ | ----- | ------ | ---- | ------------ |

该方法唤醒在该对象上等待的所有线程。

# 包装类

## 1、包装类介绍

虽然 Java 语言是典型的面向对象编程语言，但其中的八种基本数据类型并不支持面向对象编程，基本类型的数据不具备“对象”的特性——不携带属性、没有方法可调用。 沿用它们只是为了迎合人类根深蒂固的习惯，并的确能简单、有效地进行常规数据处理。
这种借助于非面向对象技术的做法有时也会带来不便，比如引用类型数据均继承了 Object 类的特性，要转换为 String 类型（经常有这种需要）时只要简单调用 Object 类中定义的 toString()即可，而基本数据类型转换为 String 类型则要麻烦得多。为解决此类问题 ，Java 为每种基本数据类型分别设计了对应的类，称之为包装类(Wrapper Classes)，也有教材称为外覆类或数据类型类。

| **基本数据类型** | **对应的包装类** |
| ---------------- | ---------------- |
| byte             | Byte             |
| short            | Short            |
| int              | Integer          |
| long             | Long             |
| char             | Character        |
| ﬂoat             | Float            |
| double           | Double           |
| boolean          | Boolean          |

每个包装类的对象可以封装一个相应的基本类型的数据，并提供了其它一些有用的方法。包装类对象一 经创建，其内容（所封装的基本类型数据值）不可改变。
基本类型和对应的包装类可以相互装换：
由基本类型向对应的包装类转换称为装箱，例如把 int 包装成 Integer 类的对象； 包装类向对应的基本类型转换称为拆箱，例如把 Integer 类的对象重新简化为 int。

## 2、包装类的应用

【1、 实现 int 和 Integer 的相互转换】
可以通过 Integer 类的构造方法将 int 装箱，通过 Integer 类的 intValue 方法将 Integer 拆箱。

public static void main(String[] args) { int m = 500;
Integer obj = new Integer(m); // 手动装箱
int n = obj.intValue(); // 手动拆箱 System.out.println("n = " + n); Integer obj1 = new Integer(500);
System.out.println("obj 等价于 obj1？" + obj.equals(obj1));
}
1
2
3
4
5
6
7
8

【2、 将字符串转换为整数】
Integer 类有一个静态的 paseInt() 方法，可以将字符串转换为整数，语法为：

| 1   | parseInt(String | s,  | int | radix); |
| --- | --------------- | --- | --- | ------- |

s 为要转换的字符串，radix 为进制，可选，默认为十进制。下面的代码将会告诉你什么样的字符串可以转换为整数：

| 1   | public static void main(String[] args) {              |
| --- | ----------------------------------------------------- |
| 2   | String[] str = {"123", "123abc", "abc123", "abcxyz"}; |
| 3   | for(String str1 : str){                               |
| 4   | try{                                                  |
| 5   | int m = Integer.parseInt(str1, 10);                   |
| 6   | System.out.println(str1 + " 可以转换为整数 " + m);    |
| 7   | }catch(Exception e){                                  |
| 8   | System.out.println(str1 + " 无法转换为整数");         |
| 9   | }                                                     |
| 10  | }                                                     |
| 11  | }                                                     |
| 12  |                                                       |
| 13  | //结果                                                |
| 14  | 123 可以转换为整数 123                                |
| 15  | 123abc 无法转换为整数                                 |
| 16  | abc123 无法转换为整数                                 |
| 17  | abcxyz 无法转换为整数                                 |

【 3、将整数转换为字符串】
Integer 类有一个静态的 toString() 方法，可以将整数转换为字符串。或者直接在整数后面加空字符串即可！

| 1   | public static void main(String[] args) { |                                 |
| --- | ---------------------------------------- | ------------------------------- |
| 2   |                                          | int m = 500;                    |
| 3   |                                          | String s = Integer.toString(m); |
| 4   |                                          | String s2 = m+"";               |
| 5   |                                          | System.out.println("s = " + s); |
| 6   | }                                        |                                 |

## 3、自动拆箱和装箱

上面的例子都需要手动实例化一个包装类，称为手动拆箱装箱。Java 1.5(5.0) 之前必须手动拆箱装箱。
Java 1.5 之后可以自动拆箱装箱，也就是在进行基本数据类型和对应的包装类转换时，系统将自动进行，这将大大方便程序员的代码书写。

| 1   | public static void main(String[] args) {                    |
| --- | ----------------------------------------------------------- |
| 2   | int m = 500;                                                |
| 3   | Integer obj = m; // 自动装箱                                |
| 4   | int n = obj; // 自动拆箱                                    |
| 5   | System.out.println("n = " + n);                             |
| 6   | Integer obj1 = 500;                                         |
| 7   | System.out.println("obj 等价于 obj1？" + obj.equals(obj1)); |
| 8   | }                                                           |
| 9   |                                                             |
| 10  | //结果：                                                    |
| 11  | // n = 500                                                  |
| 12  | // obj 等价于 obj1？true                                    |

自动装箱与拆箱的功能事实上是编译器来帮您的忙，编译器在编译时期依您所编写的语法，决定是否进 行装箱或拆箱动作。例如：

| 1   | Integer i = 100;                       |
| --- | -------------------------------------- |
| 2   | 相当于编译器自动为您作以下的语法编译： |
| 3   | Integer i = new Integer(100);          |

所以自动装箱与拆箱的功能是所谓的“编译器蜜糖”(Compiler Sugar)，虽然使用这个功能很方便，但在程序运行阶段您得了解 Java 的语义。例如下面的程序是可以通过编译的：

| 1   | Integer | i   | =   | null; |
| --- | ------- | --- | --- | ----- |
| 2   | int j = | i;  |     |       |

这样的语法在编译时期是合法的，但是在运行时期会有错误，因为这种写法相当于：

| 1   | Integer | i = null;     |
| --- | ------- | ------------- |
| 2   | int j = | i.intValue(); |

null 表示 i 没有参考至任何的对象实体，它可以合法地指定给对象参考名称。由于实际上 i 并没有参考至任何的对象，所以也就不可能操作 intValue()方法，这样上面的写法在运行时会出现 NullPointerException 错误。
自动拆箱装箱是常用的一个功能，需要重点掌握。
一般地，当需要使用数字的时候，我们通常使用内置数据类型，如：**byte、int、long、double **等。然而，在实际开发过程中，我们经常会遇到需要使用对象，而不是内置数据类型的情形。为了解决这个问 题，Java 语言为每一个内置数据类型提供了对应的包装类。
所有的包装类**（Integer、Long、Byte、Double、Float、Short）**都是**抽象类 Number **的子类。

# Math 类

Java 的 Math 包含了用于执行基本数学运算的属性和方法，如初等指数、对数、平方根和三角函数。
Math 的方法都被定义为 static 形式，通过 Math 类可以在主函数中直接调用。
【演示：查看 Math 类的源码】

public final class Math{
//数学方法
}
1
2
3

【常用值与函数】
Math.PI 记录的圆周率
Math.E 记录 e 的常量
Math 中还有一些类似的常量，都是一些工程数学常用量。
Math.abs 求绝对值
Math.sin 正弦函数 Math.asin 反正弦函数
Math.cos 余弦函数 Math.acos 反余弦函数
Math.tan 正切函数 Math.atan 反正切函数 Math.atan2 商的反正切函数 Math.toDegrees 弧度转化为角度 Math.toRadians 角度转化为弧度 Math.ceil 得到不小于某数的最大整数
Math.ﬂoor 得到不大于某数的最大整数
Math.IEEEremainder 求余 Math.max 求两数中最大 Math.min 求两数中最小 Math.sqrt 求开方
Math.pow 求某数的任意次方, 抛出 ArithmeticException 处理溢出异常

Math.exp 求 e 的任意次方 Math.log10 以 10 为底的对数 Math.log 自然对数
Math.rint 求距离某数最近的整数（可能比某数大，也可能比它小） Math.round 同上，返回 int 型或者 long 型（上一个函数返回 double 型） Math.random 返回 0，1 之间的一个随机数

| 1   | public static void main(String[] args) | {     |
| --- | -------------------------------------- | ----- |
| 2   | /\*\*                                  |       |
| 3   | \*Math.sqrt()//计算平方根              |       |
| 4   | \*Math.cbrt()//计算立方根              |       |
| 5   | \*Math.pow(a, b)//计算 a 的 b 次方     |       |
| 6   | \*Math.max( , );//计算最大值           |       |
| 7   | \*Math.min( , );//计算最小值           |       |
| 8   | \*/                                    |       |
| 9   | System.out.println(Math.sqrt(16));     | //4.0 |
| 10  | System.out.println(Math.cbrt(8));      | //2.0 |
| 11  | System.out.println(Math.pow(3,2));     | //9.0 |

1.  System.out.println(Math.max(2.3,4.5));//4.5
1.  System.out.println(Math.min(2.3,4.5));//2.3 14 /\*\*

15 _ abs 求绝对值
16 _/

1. System.out.println(Math.abs(-10.4)); //10.4
1. System.out.println(Math.abs(10.1)); //10.1 19 /\*\*

20 _ ceil 天花板的意思，就是返回大的值
21 _/

1. System.out.println(Math.ceil(-10.1)); //-10.0
1. System.out.println(Math.ceil(10.7)); //11.0
1. System.out.println(Math.ceil(-0.7)); //-0.0
1. System.out.println(Math.ceil(0.0)); //0.0
1. System.out.println(Math.ceil(-0.0)); //-0.0
1. System.out.println(Math.ceil(-1.7)); //-1.0 28 /\*\*

29 _ floor 地板的意思，就是返回小的值
30 _/

1. System.out.println(Math.floor(-10.1)); //-11.0
1. System.out.println(Math.floor(10.7)); //10.0
1. System.out.println(Math.floor(-0.7)); //-1.0
1. System.out.println(Math.floor(0.0)); //0.0
1. System.out.println(Math.floor(-0.0)); //-0.0 36 /\*\*

37 _ random 取得一个大于或者等于 0.0 小于不等于 1.0 的随机数 [0,1)
38 _/

1. System.out.println(Math.random()); //小于 1 大于 0 的 double 类型的数
1. System.out.println(Math.random()+1);//大于 1 小于 2 的 double 类型的数 41 /\*\*
1. - rint 四舍五入，返回 double 值
1. - 注意.5 的时候会取偶数 异常的尴尬=。= 44 \*/
1. System.out.println(Math.rint(10.1)); //10.0
1. System.out.println(Math.rint(10.7)); //11.0
1. System.out.println(Math.rint(11.5)); //12.0
1. System.out.println(Math.rint(10.5)); //10.0
1. System.out.println(Math.rint(10.51)); //11.0
1. System.out.println(Math.rint(-10.5)); //-10.0

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 51  |     | System.out.println(Math.rint(-11.5)); //-12.0                 |
| --- | --- | ------------------------------------------------------------- |
| 52  |     | System.out.println(Math.rint(-10.51)); //-11.0                |
| 53  |     | System.out.println(Math.rint(-10.6)); //-11.0                 |
| 54  |     | System.out.println(Math.rint(-10.2)); //-10.0                 |
| 55  |     | /\*\*                                                         |
| 56  |     | \* round 四舍五入，float 时返回 int 值，double 时返回 long 值 |
| 57  |     | \*/                                                           |
| 58  |     | System.out.println(Math.round(10.1)); //10                    |
| 59  |     | System.out.println(Math.round(10.7)); //11                    |
| 60  |     | System.out.println(Math.round(10.5)); //11                    |
| 61  |     | System.out.println(Math.round(10.51)); //11                   |
| 62  |     | System.out.println(Math.round(-10.5)); //-10                  |
| 63  |     | System.out.println(Math.round(-10.51)); //-11                 |
| 64  |     | System.out.println(Math.round(-10.6)); //-11                  |
| 65  |     | System.out.println(Math.round(-10.2)); //-10                  |
| 66  |     |                                                               |
| 67  | }   |                                                               |

# Random 类

Java 中存在着两种 Random 函数：

#### 一、java.lang.Math.Random;

调用这个 Math.Random()函数能够返回带正号的 double 值，该值大于等于 0.0 且小于 1.0，即取值范围是
[0.0,1.0)的左闭右开区间，返回值是一个伪随机选择的数，在该范围内（近似）均匀分布。例子如下：

public static void main(String[] args) {
// 结 果 是 个 double 类 型 的 值 ， 区 间 为 [0.0,1.0） System.out.println("Math.random()=" + Math.random()); int num = (int) (Math.random() * 3);
// 注意不要写成(int)Math.random()*3，这个结果为 0 或 1，因为先执行了强制转换
System.out.println("num=" + num);
}
//结果
//Math.random()=0.44938147153848396
//num=1
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

#### 二、java.util.Random

下面是 Random()的两种构造方法：
Random()：创建一个新的随机数生成器。
Random(long seed)：使用单个 long 种子创建一个新的随机数生成器。
你在创建一个 Random 对象的时候可以给定任意一个合法的种子数，种子数只是随机算法的起源数字， 和生成的随机数的区间没有任何关系。
如下面的 Java 代码：
【演示一】
在没带参数构造函数生成的 Random 对象的种子缺省是当前系统时间的毫秒数。
rand.nextInt(100)中的 100 是随机数的上限,产生的随机数为 0-100 的整数,不包括 100。

1. public static void main(String[] args) {
1. Random rand =new Random();
1. int i=rand.nextInt(100);
1. System.out.println(i); 5 }

【演示二】
对于种子相同的 Random 对象，生成的随机数序列是一样的。

| 1   | public static void main(String[] args) { |                                                                            |     |
| --- | ---------------------------------------- | -------------------------------------------------------------------------- | --- |
| 2   |                                          | Random ran1 = new Random(25);                                              |     |
| 3   |                                          | System.out.println("使用种子为 25 的 Random 对象生成[0,100)内随机整数序列: | "); |
| 4   |                                          | for (int i = 0; i < 10; i++) {                                             |     |
| 5   |                                          | System.out.print(ran1.nextInt(100) + " ");                                 |     |
| 6   |                                          | }                                                                          |     |
| 7   |                                          | System.out.println();                                                      |     |
| 8   | }                                        |                                                                            |     |

【方法摘要】

1. protected int next(int bits)：生成下一个伪随机数。
1. boolean nextBoolean()：返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的 boolean 值。
1. void nextBytes(byte[] bytes)：生成随机字节并将其置于用户提供的 byte 数组中。
1. double nextDouble()：返回下一个伪随机数，它是取自此随机数生成器序列的、在 0.0 和 1.0 之间均匀分布的 double 值。
1. ﬂoat nextFloat()：返回下一个伪随机数，它是取自此随机数生成器序列的、在 0.0 和 1.0 之间均匀分布 ﬂoat 值。
1. double nextGaussian()：返回下一个伪随机数，它是取自此随机数生成器序列的、呈高斯（“正态”）分布的 double 值，其平均值是 0.0 标准差是 1.0。
1. int nextInt()：返回下一个伪随机数，它是此随机数生成器的序列中均匀分布的 int 值。
1. int nextInt(int n)：返回一个伪随机数，它是取自此随机数生成器序列的、在（包括和指定值（不包括）之间均匀分布的 int 值。
1. long nextLong()：返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的 long 值。
1. void setSeed(long seed)：使用单个 long 种子设置此随机数生成器的种子。

【例子】

1. 生成[0,1.0)区间的小数：double d1 = r.nextDouble();
1. 生成[0,5.0)区间的小数：double d2 = r.nextDouble() \* 5;
1. 生成[1,2.5)区间的小数：double d3 = r.nextDouble() \* 1.5 + 1;
1. 生成[0,10)区间的整数：int n2 = r.nextInt(10);

# 日期时间类

## 1、Date 类

java.util 包提供了 Date 类来封装当前的日期和时间。
Date 类提供两个构造函数来实例化 Date 对象。
第一个构造函数使用当前日期和时间来初始化对象。

Date()
1

第二个构造函数接收一个参数，该参数是从 1970 年 1 月 1 日起的毫秒数。

Date(long millisec)
1

Date 对象创建以后，可以调用下面的方法。

| **序号** | **方法和描述** |
| -------- | -------------- |

|
1 | **boolean after(Date date) **若当调用此方法的 Date 对象在指定日期之后返回 true,否则返回
false。 |
|
2 | **boolean before(Date date) **若当调用此方法的 Date 对象在指定日期之前返回 true,否则返回 false。 |
| 3 | **Object clone( ) **返回此对象的副本。 |
|
4 | **int compareTo(Date date) **比较当调用此方法的 Date 对象和指定日期。两者相等时候返回
0。调用对象在指定日期之前则返回负数。调用对象在指定日期之后则返回正数。 |
|
5 | **int compareTo(Object obj) **若 obj 是 Date 类型则操作等同于 compareTo(Date) 。否则它抛出 ClassCastException。 |
|
6 | **boolean equals(Object date) **当调用此方法的 Date 对象和指定日期相等时候返回 true,否则返回 false。 |
|
7 | **long getTime( ) **返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。 |
| 8 | **int hashCode( ) **返回此对象的哈希码值。 |
|
9 | **void setTime(long time) **用自 1970 年 1 月 1 日 00:00:00 GMT 以后 time 毫秒数设置时间和日期。 |
|
10 | **String toString( ) **把此 Date 对象转换为以下形式的 String： dow mon dd hh:mm:ss zzz yyyy 其中： dow 是一周中的某一天 (Sun, Mon, Tue, Wed, Thu, Fri, Sat)。 |

【演示：获取当前日期时间】
Java 中获取当前日期和时间很简单，使用 Date 对象的 toString() 方法来打印当前日期和时间如下所示：

| 1   | public static void main(String args[]) { |
| --- | ---------------------------------------- |
| 2   | // 初始化 Date 对象                      |
| 3   | Date date = new Date();                  |
| 4   |                                          |
| 5   | // 使用 toString() 函数显示日期时间      |
| 6   | System.out.println(date.toString());     |
| 7   | //Sat Apr 27 15:09:43 CST 2019           |
| 8   | }                                        |

【演示：日期比较】
使用 getTime() 方法获取两个日期（自 1970 年 1 月 1 日经历的毫秒数值），然后比较这两个值。

| 1   | public static void main(String[] args) { |
| --- | ---------------------------------------- |
| 2   | // 初始化 Date 对象                      |
| 3   | Date date = new Date();                  |
| 4   |                                          |
| 5   | long time = date.getTime();              |
| 6   | long time2 = date.getTime();             |
| 7   | System.out.println(time==time2);         |
| 8   | }                                        |

使用方法 before()，after() 和 equals()。例如，一个月的 12 号比 18 号早，则 new Date(99, 2, 12).before(new Date (99, 2, 18)) 返回 true。

public static void main(String[] args) {
boolean before = new Date(97, 01, 05).before(new Date(99, 11, 16)); System.out.println(before);
}
1
2
3
4

## 2、SimpleDateFormat

【演示：使用 SimpleDateFormat 格式化日期】
SimpleDateFormat 是一个以语言环境敏感的方式来格式化和分析日期的类。SimpleDateFormat 允许你选择任何用户自定义日期时间格式来运行。例如：

1. public static void main(String args[]) {
1. Date dNow = new Date( );
1. SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd hh:mm:ss");
1. System.out.println("当前时间为: " + ft.format(dNow)); 5 }

其中 yyyy 是完整的公元年，MM 是月份，dd 是日期，HH:mm:ss 是时、分、秒。
**注意**:有的格式大写，有的格式小写，例如 MM 是月份，mm 是分；HH 是 24 小时制，而 hh 是 12 小时制。
时间模式字符串用来指定时间格式。在此模式中，所有的 ASCII 字母被保留为模式字母，定义如下：

| **字母** | **描述**                 | **示例**                |
| -------- | ------------------------ | ----------------------- |
| G        | 纪元标记                 | AD                      |
| y        | 四位年份                 | 2001                    |
| M        | 月份                     | July or 07              |
| d        | 一个月的日期             | 10                      |
| h        | A.M./P.M. (1~12)格式小时 | 12                      |
| H        | 一天中的小时 (0~23)      | 22                      |
| m        | 分钟数                   | 30                      |
| s        | 秒数                     | 55                      |
| S        | 毫秒数                   | 234                     |
| E        | 星期几                   | Tuesday                 |
| D        | 一年中的日子             | 360                     |
| F        | 一个月中第几周的周几     | 2 (second Wed. in July) |
| w        | 一年中第几周             | 40                      |
| W        | 一个月中第几周           | 1                       |
| a        | A.M./P.M. 标记           | PM                      |
| k        | 一天中的小时(1~24)       | 24                      |
| K        | A.M./P.M. (0~11)格式小时 | 10                      |
| z        | 时区                     | Eastern Standard Time   |
| '        | 文字定界符               | Delimiter               |
| "        | 单引号                   | `                       |

【演示：使用 printf 格式化日期】
[printf 方法使用说明](https://www.runoob.com/w3cnote/java-printf-formate-demo.html)
printf 方法可以很轻松地格式化时间和日期。使用两个字母格式，它以 **%t **开头并且以下面表格中的一个字母结尾。

| 1   | public static void main(String args[]) {                      |
| --- | ------------------------------------------------------------- |
| 2   | // 初始化 Date 对象                                           |
| 3   | Date date = new Date();                                       |
| 4   |                                                               |
| 5   | //c 的使用                                                    |
| 6   | System.out.printf("全部日期和时间信息：%tc%n",date);          |
| 7   | //f 的使用                                                    |
| 8   | System.out.printf("年-月-日格式：%tF%n",date);                |
| 9   | //d 的使用                                                    |
| 10  | System.out.printf("月/日/年格式：%tD%n",date);                |
| 11  | //r 的使用                                                    |
| 12  | System.out.printf("HH:MM:SS PM 格式（12 时制）：%tr%n",date); |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 13  | //t 的使用                                                 |
| --- | ---------------------------------------------------------- |
| 14  | System.out.printf("HH:MM:SS 格式（24 时制）：%tT%n",date); |
| 15  | //R 的使用                                                 |
| 16  | System.out.printf("HH:MM 格式（24 时制）：%tR",date);      |
| 17  | }                                                          |
| 18  |                                                            |
| 19  | //结果:                                                    |
| 20  | 全部日期和时间信息：星期六 四月 27 15:23:45 CST 2019       |
| 21  | 年-月-日格式：2019-04-27                                   |
| 22  | 月/日/年格式：04/27/19                                     |
| 23  | HH:MM:SS PM 格式（12 时制）：03:23:45 下午                 |
| 24  | HH:MM:SS 格式（24 时制）：15:23:45                         |
| 25  | HH:MM 格式（24 时制）：15:23                               |

【时间休眠：休眠(sleep)】
sleep()使当前线程进入停滞状态（阻塞当前线程），让出 CPU 的使用、目的是不让当前线程独自霸占该 进程所获的 CPU 资源，以留一定时间给其他线程执行的机会。
你可以让程序休眠一毫秒的时间或者到您的计算机的寿命长的任意段时间。例如，下面的程序会休眠 3 秒：

1. public static void main(String args[]) {
1. try {
1. System.out.println(new Date( ) + "\n");
1. Thread.sleep(1000\*3); // 休眠 3 秒
1. System.out.println(new Date( ) + "\n");
1. } catch (Exception e) {
1. System.out.println("Got an exception!"); 8 }

9 }

## 3、Calendar 类

我们现在已经能够格式化并创建一个日期对象了，但是我们如何才能设置和获取日期数据的特定部分
呢，比如说小时，日，或者分钟? 我们又如何在日期的这些部分加上或者减去值呢? 答案是使用 Calendar 类。Date 中有很多方法都已经废弃了！
Calendar 类的功能要比 Date 类强大很多，而且在实现方式上也比 Date 类要复杂一些。
Calendar 类是一个抽象类，在实际使用时实现特定的子类的对象，创建对象的过程对程序员来说是透明 的，只需要使用 getInstance 方法创建即可。

#### 创建一个代表系统当前日期的 Calendar 对象

public static void main(String args[]) {
Calendar c = Calendar.getInstance();//默认是当前日期 System.out.println(c);
}
//输出 java.util.GregorianCalendar[time=1556350818634,areFieldsSet=true,areAllFields Set=true,lenient=true,zone=sun.util.calendar.ZoneInfo[id="Asia/Shanghai",offs et=28800000,dstSavings=0,useDaylight=false,transitions=29,lastRule=null],firs tDayOfWeek=1,minimalDaysInFirstWeek=1,ERA=1,YEAR=2019,MONTH=3,WEEK_OF_YEAR=17
,WEEK_OF_MONTH=4,DAY_OF_MONTH=27,DAY_OF_YEAR=117,DAY_OF_WEEK=7,DAY_OF_WEEK_IN
\_MONTH=4,AM_PM=1,HOUR=3,HOUR_OF_DAY=15,MINUTE=40,SECOND=18,MILLISECOND=634,ZO NE_OFFSET=28800000,DST_OFFSET=0]
1
2
3
4
5
6

**创建一个指定日期的 Calendar 对象**
使用 Calendar 类代表特定的时间，需要首先创建一个 Calendar 的对象，然后再设定该对象中的年月日参 数来完成。

| 1   | //创建一个代表 2019 年 4 月 27 日的 Calendar 对象 |
| --- | ------------------------------------------------- |
| 2   | Calendar c1 = Calendar.getInstance();             |
| 3   | c1.set(2019, 4 - 1, 27);                          |

#### Calendar 类对象字段类型

Calendar 类中用以下这些常量表示不同的意义，jdk 内的很多类其实都是采用的这种思想

| **常量**              | **描述**                       |
| --------------------- | ------------------------------ |
| Calendar.YEAR         | 年份                           |
| Calendar.MONTH        | 月份                           |
| Calendar.DATE         | 日期                           |
| Calendar.DAY_OF_MONTH | 日期，和上面的字段意义完全相同 |
| Calendar.HOUR         | 12 小时制的小时                |
| Calendar.HOUR_OF_DAY  | 24 小时制的小时                |
| Calendar.MINUTE       | 分钟                           |
| Calendar.SECOND       | 秒                             |
| Calendar.DAY_OF_WEEK  | 星期几                         |

| 1   | // 获得年份                                                                                     |
| --- | ----------------------------------------------------------------------------------------------- |
| 2   | int year = c1.get(Calendar.YEAR);                                                               |
| 3   | // 获得月份                                                                                     |
| 4   | int month = c1.get(Calendar.MONTH) + 1;                                                         |
| 5   | // 获得日期                                                                                     |
| 6   | int date = c1.get(Calendar.DATE);                                                               |
| 7   | // 获得小时                                                                                     |
| 8   | int hour = c1.get(Calendar.HOUR_OF_DAY);                                                        |
| 9   | // 获得分钟                                                                                     |
| 10  | int minute = c1.get(Calendar.MINUTE);                                                           |
| 11  | // 获得秒                                                                                       |
| 12  | int second = c1.get(Calendar.SECOND);                                                           |
| 13  | // 获得星期几（注意（这个与 Date 类是不同的）：1 代表星期日、2 代表星期 1、3 代表星期二，以此类 |
|     | 推）                                                                                            |
| 14  | int day = c1.get(Calendar.DAY_OF_WEEK);                                                         |

【演示：设置完整日期】

| 1   | c1.set(2009, | 6   | -   | 1,  | 12);//把 Calendar 对象 c1 的年月日分别设这为：2009、6、12 |
| --- | ------------ | --- | --- | --- | --------------------------------------------------------- |

【演示：设置某个字段】

| 1   | c1.set(Calendar.DATE,10);         |
| --- | --------------------------------- |
| 2   | c1.set(Calendar.YEAR,2008);       |
| 3   | //其他字段属性 set 的意义以此类推 |

【add 设置】

| 1   | //把 c1 对象的日期加上 10，也就是 c1 也就表示为 10 天后的日期，其它所有的数值会被重新计算 |
| --- | ----------------------------------------------------------------------------------------- |
| 2   | c1.add(Calendar.DATE, 10);                                                                |
| 3   | //把 c1 对象的日期减去 10，也就是 c1 也就表示为 10 天前的日期，其它所有的数值会被重新计算 |
| 4   | c1.add(Calendar.DATE, -10);                                                               |

【演示：GregorianCalendar】

| 1   | public static void main(String args[]) {                    |
| --- | ----------------------------------------------------------- |
| 2   | String months[] = {                                         |
| 3   | "Jan", "Feb", "Mar", "Apr",                                 |
| 4   | "May", "Jun", "Jul", "Aug",                                 |
| 5   | "Sep", "Oct", "Nov", "Dec"};                                |
| 6   |                                                             |
| 7   | int year;                                                   |
| 8   | // 初始化 Gregorian 日历                                    |
| 9   | // 使用当前时间和日期                                       |
| 10  | // 默认为本地时间和时区                                     |
| 11  | GregorianCalendar gcalendar = new GregorianCalendar();      |
| 12  | // 显示当前时间和日期的信息                                 |
| 13  | System.out.print("Date: ");                                 |
| 14  | System.out.print(months[gcalendar.get(Calendar.MONTH)]);    |
| 15  | System.out.print(" " + gcalendar.get(Calendar.DATE) + " "); |
| 16  | System.out.println(year = gcalendar.get(Calendar.YEAR));    |
| 17  | System.out.print("Time: ");                                 |
| 18  | System.out.print(gcalendar.get(Calendar.HOUR) + ":");       |
| 19  | System.out.print(gcalendar.get(Calendar.MINUTE) + ":");     |
| 20  | System.out.println(gcalendar.get(Calendar.SECOND));         |
| 21  |                                                             |
| 22  | // 测试当前年份是否为闰年                                   |
| 23  | if(gcalendar.isLeapYear(year)) {                            |
| 24  | System.out.println("当前年份是闰年");                       |
| 25  | }                                                           |
| 26  | else {                                                      |
| 27  | System.out.println("当前年份不是闰年");                     |
| 28  | }                                                           |
| 29  | }                                                           |
| 30  | //输出：                                                    |
| 31  | Date: Apr 27 2019                                           |
| 32  | Time: 3:56:20                                               |
| 33  | 当前年份不是闰年                                            |

#### 注意：Calender 的月份是从 0 开始的，但日期和年份是从 1 开始的

【演示】

1. public static void main(String[] args) {
1. Calendar c1 = Calendar.getInstance(); 3 c1.set(2017, 1, 1);

4 System.out.println(c1.get(Calendar.YEAR)
5 +"-"+c1.get(Calendar.MONTH)
6 +"-"+c1.get(Calendar.DATE)); 7 c1.set(2017, 1, 0);
8 System.out.println(c1.get(Calendar.YEAR)
9 +"-"+c1.get(Calendar.MONTH)
10 +"-"+c1.get(Calendar.DATE)); 11 }
狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 12  | //输出    |
| --- | --------- |
| 13  | 2017-1-1  |
| 14  | 2017-0-31 |

可见，将日期设为 0 以后，月份变成了上个月，但月份可以为 0，把月份改为 2 试试：

public static void main(String[] args) { Calendar c1 = Calendar.getInstance(); c1.set(2017, 2, 1);
System.out.println(c1.get(Calendar.YEAR)
+"-"+c1.get(Calendar.MONTH)
+"-"+c1.get(Calendar.DATE));
c1.set(2017, 2, 0);
System.out.println(c1.get(Calendar.YEAR)
+"-"+c1.get(Calendar.MONTH)
+"-"+c1.get(Calendar.DATE));
}
//输出 2017-2-1
2017-1-28
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

可以看到上个月的最后一天是 28 号，所以 Calendar.MONTH 为 1 的时候是 2 月 。
【作业：在控制台输出 windows 日历效果】

# String 类

## 1、String 概述

在 API 中是这样描述：
String 类代表字符串。Java 程序中的所有字符串字面值（如 "abc" ）都作为此类的实例实现。 字符串是常量；它们的值在创建之后不能更改。字符串缓冲区支持可变的字符串。因为 String 对象是不可变的， 所以可以共享。
【演示：查看 String 源码】

| 1   | public final class String                                           |
| --- | ------------------------------------------------------------------- |
| 2   | implements java.io.Serializable, Comparable<String>, CharSequence { |
| 3   |                                                                     |
| 4   | }                                                                   |

【String 的成员变量】

| 1   | //String 的属性值                 |
| --- | --------------------------------- |
| 2   | private final char value[];       |
| 3   |                                   |
| 4   | //数组被使用的开始位置            |
| 5   | private final int offset;         |
| 6   |                                   |
| 7   | //String 中元素的个数             |
| 8   | private final int count;          |
| 9   |                                   |
| 10  | //String 类型的 hash 值           |
| 11  | private int hash; // Default to 0 |
| 12  |                                   |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 13 | private

private
new | static final long serialVersionUID = -6849794470754667710L;

| static final ObjectStreamField[] serialPersistentFields = ObjectStreamField[0]; |
| ------------------------------------------------------------------------------- | --- | --- |
| 14                                                                              |     |     |
| 15                                                                              |     |     |
| 16                                                                              |     |     |

从源码看出 String 底层使用一个字符数组来维护的。
成员变量可以知道 String 类的值是 ﬁnal 类型的，不能被改变的，所以只要一个值改变就会生成一个新的
String 类型对象，存储 String 数据也不一定从数组的第 0 个元素开始的，而是从 oﬀset 所指的元素开始。
【String 的构造方法】

| 1   | String()                                                                             |
| --- | ------------------------------------------------------------------------------------ |
| 2   | //初始化一个新创建的 String 对象，使其表示一个空字符序列。                           |
| 3   | String(byte[] bytes)                                                                 |
| 4   | //通过使用平台的默认字符集解码指定的 byte 数组，构造一个新的 String。                |
| 5   | String(byte[] bytes, Charset charset)                                                |
| 6   | //通过使用指定的 charset 解码指定的 byte 数组，构造一个新的 String。                 |
| 7   | String(byte[] bytes, int offset, int length)                                         |
| 8   | //通过使用平台的默认字符集解码指定的 byte 子数组，构造一个新的 String。              |
| 9   | String(byte[] bytes, int offset, int length, Charset charset)                        |
| 10  | //通过使用指定的 charset 解码指定的 byte 子数组，构造一个新的 String。               |
| 11  | String(byte[] bytes, int offset, int length, String charsetName)                     |
| 12  | //通过使用指定的字符集解码指定的 byte 子数组，构造一个新的 String。                  |
| 13  | String(byte[] bytes, String charsetName)                                             |
| 14  | //通过使用指定的 charset 解码指定的 byte 数组，构造一个新的 String。                 |
| 15  | String(char[] value)                                                                 |
| 16  | //分配一个新的 String，使其表示字符数组参数中当前包含的字符序列。                    |
| 17  | String(char[] value, int offset, int count)                                          |
| 18  | //分配一个新的 String，它包含取自字符数组参数一个子数组的字符。                      |
| 19  | String(int[] codePoints, int offset, int count)                                      |
| 20  | //分配一个新的 String，它包含 Unicode 代码点数组参数一个子数组的字符。               |
| 21  | String(String original)                                                              |
| 22  | //初始化一个新创建的 String 对象，使其表示一个与参数相同的字符序列；换句话说，新创建 |
|     | 的字符串是该参数字符串的副本。                                                       |
| 23  | String(StringBuffer buffer)                                                          |
| 24  | //分配一个新的字符串，它包含字符串缓冲区参数中当前包含的字符序列。                   |
| 25  | String(StringBuilder builder)                                                        |
| 26  | //分配一个新的字符串，它包含字符串生成器参数中当前包含的字符序列。                   |

## 2、创建字符串对象方式

直接赋值方式创建对象是在方法区的常量池

String str="hello";//直接赋值的方式
1

通过构造方法创建字符串对象是在堆内存

String str=new String("hello");//实例化的方式
1

【两种实例化方式的比较】

1. 编写代码比较

1. public static void main(String[] args) {
1. String str1 = "Lance";
1. String str2 = new String("Lance");
1. String str3 = str2; //引用传递，str3 直接指向 st2 的堆内存地址

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 5 |

} | String str4 = "Lance";
/\*\*

- ==:

* 基本数据类型：比较的是基本数据类型的值是否相同
* 引用数据类型：比较的是引用数据类型的地址值是否相同
* 所以在这里的话：String 类对象==比较，比较的是地址，而不是内容

| \*/ System.out.println(str1==str2);//false System.out.println(str1==str3);//false System.out.println(str3==str2);//true System.out.println(str1==str4);//true |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- | --- |
| 6                                                                                                                                                             |     |     |
| 7                                                                                                                                                             |     |     |
| 8                                                                                                                                                             |     |     |
| 9                                                                                                                                                             |     |     |
| 10                                                                                                                                                            |     |     |
| 11                                                                                                                                                            |     |     |
| 12                                                                                                                                                            |     |     |
| 13                                                                                                                                                            |     |     |
| 14                                                                                                                                                            |     |     |
| 15                                                                                                                                                            |     |     |
| 16                                                                                                                                                            |     |     |

1. 内存图分析

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834181226-ff88ada3-5dba-4bf5-a2ea-7636fe7e67d3.png#)[

可能这里还是不够明显，构造方法实例化方式的内存图：String str = new String("Hello");
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834181602-7eb959cb-d929-4a29-bd4e-8bfa86385ad0.jpeg#)首先：

[

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834181919-2401676a-a32f-4aff-9500-03caed7ca8ef.jpeg#)当我们再一次的 new 一个 String 对象时：

[

【字符串常量池】
在字符串中，如果采用直接赋值的方式（String str="Lance"）进行对象的实例化，则会将匿名对象“Lance”放入对象池，每当下一次对不同的对象进行直接赋值的时候会直接利用池中原有的匿名对象，我 们可以用对象手工入池；

1. public static void main(String args[]){
1. String str =new String("Lance").intern();//对匿名对象"hello"进行手工入池操作
1. String str1="Lance";
1. System.out.println(str==str1);//true 5 }

【两种实例化方式的区别】

1. 直接赋值（String str = "hello"）：只开辟一块堆内存空间，并且会自动入池，不会产生垃圾。
1. 构造方法（String str= new String("hello");）:会开辟两块堆内存空间，其中一块堆内存会变成垃圾被系统回收，而且不能够自动入池，需要通过 public String intern();方法进行手工入池。
1. 在开发的过程中不会采用构造方法进行字符串的实例化。

【避免空指向】
首先了解： == 和 public boolean equals()比较字符串的区别
==在对字符串比较的时候，对比的是内存地址，而 equals 比较的是字符串内容，在开发的过程中， equals()通过接受参数，可以避免空指向。

String str = null; if(str.equals("hello")){//此时会出现空指向异常
...
}
if("hello".equals(str)){//此时 equals 会处理 null 值，可以避免空指向异常
...
}
1
2
3
4
5
6
7

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834182205-2d4be391-aaad-4750-9dde-8c794355fcdd.jpeg#)【String 类对象一旦声明则不可以改变；而改变的只是地址，原来的字符串还是存在的，并且产生垃 圾】

[

## 3、String 常用的方法

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834182477-a710310e-efcf-4693-9bd8-e84bacec7154.jpeg#)[

### 1、String 的判断

【常用方法】

| 1   | boolean | equals(Object obj)：比较字符串的内容是否相同                       |
| --- | ------- | ------------------------------------------------------------------ |
| 2   | boolean | equalsIgnoreCase(String str)： 比较字符串的内容是否相同,忽略大小写 |
| 3   | boolean | startsWith(String str)： 判断字符串对象是否以指定的 str 开头       |
| 4   | boolean | endsWith(String str)： 判断字符串对象是否以指定的 str 结尾         |

【演示】

| 1   | public static void main(String[] args) {                                    |
| --- | --------------------------------------------------------------------------- |
| 2   | // 创建字符串对象                                                           |
| 3   | String s1 = "hello";                                                        |
| 4   | String s2 = "hello";                                                        |
| 5   | String s3 = "Hello";                                                        |
| 6   |                                                                             |
| 7   | // boolean equals(Object obj):比较字符串的内容是否相同                      |
| 8   | System.out.println(s1.equals(s2)); //true                                   |
| 9   | System.out.println(s1.equals(s3)); //false                                  |
| 10  | System.out.println(" ");                                                    |
| 11  |                                                                             |
| 12  | // boolean equalsIgnoreCase(String str):比较字符串的内容是否相同,忽略大小写 |
| 13  | System.out.println(s1.equalsIgnoreCase(s2)); //true                         |
| 14  | System.out.println(s1.equalsIgnoreCase(s3)); //true                         |
| 15  | System.out.println(" ");                                                    |
| 16  |                                                                             |
| 17  | // boolean startsWith(String str):判断字符串对象是否以指定的 str 开头       |
| 18  | System.out.println(s1.startsWith("he")); //true                             |
| 19  | System.out.println(s1.startsWith("ll")); //false                            |
| 20  | }                                                                           |

### 2、String 的截取

【常用方法】

int length():获取字符串的长度，其实也就是字符个数 char charAt(int index):获取指定索引处的字符
int indexOf(String str):获取 str 在字符串对象中第一次出现的索引 String substring(int start):从 start 开始截取字符串
String substring(int start,int end):从 start 开始，到 end 结束截取字符串。包括 start， 不包括 end
1
2
3
4
5

【演示】

| 1   | public static void main(String args[]) {                                    |
| --- | --------------------------------------------------------------------------- |
| 2   | // 创建字符串对象                                                           |
| 3   | String s = "helloworld";                                                    |
| 4   |                                                                             |
| 5   | // int length():获取字符串的长度，其实也就是字符个数                        |
| 6   | System.out.println(s.length()); //10                                        |
| 7   | System.out.println(" ");                                                    |
| 8   |                                                                             |
| 9   | // char charAt(int index):获取指定索引处的字符                              |
| 10  | System.out.println(s.charAt(0)); //h                                        |
| 11  | System.out.println(s.charAt(1)); //e                                        |
| 12  | System.out.println(" ");                                                    |
| 13  |                                                                             |
| 14  | // int indexOf(String str):获取 str 在字符串对象中第一次出现的索引          |
| 15  | System.out.println(s.indexOf("l")); //2                                     |
| 16  | System.out.println(s.indexOf("owo")); //4                                   |
| 17  | System.out.println(s.indexOf("ak")); //-1                                   |
| 18  | System.out.println(" ");                                                    |
| 19  |                                                                             |
| 20  | // String substring(int start):从 start 开始截取字符串                      |
| 21  | System.out.println(s.substring(0)); //helloworld                            |
| 22  | System.out.println(s.substring(5)); //world                                 |
| 23  | System.out.println(" ");                                                    |
| 24  |                                                                             |
| 25  | // String substring(int start,int end):从 start 开始，到 end 结束截取字符串 |
| 26  | System.out.println(s.substring(0, s.length())); //helloworld                |
| 27  | System.out.println(s.substring(3, 8)); //lowor                              |
| 28  | }                                                                           |

### 3、String 的转换

【常用方法】

| 1   | char[] toCharArray()：把字符串转换为字符数组   |
| --- | ---------------------------------------------- |
| 2   | String toLowerCase()：把字符串转换为小写字符串 |
| 3   | String toUpperCase()：把字符串转换为大写字符串 |

【演示】

| 1 | public
// | static void
创建字符串对象 | main(String | args[]) | { |
| --- | --- | --- | --- | --- | --- |
| 2 | | | | | |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 3   |     | String s = "abcde";                              |
| --- | --- | ------------------------------------------------ |
| 4   |     |                                                  |
| 5   |     | // char[] toCharArray():把字符串转换为字符数组   |
| 6   |     | char[] chs = s.toCharArray();                    |
| 7   |     | for (int x = 0; x < chs.length; x++) {           |
| 8   |     | System.out.println(chs[x]);                      |
| 9   |     | }                                                |
| 10  |     |                                                  |
| 11  |     | System.out.println(" ");                         |
| 12  |     |                                                  |
| 13  |     | // String toLowerCase():把字符串转换为小写字符串 |
| 14  |     | System.out.println("HelloWorld".toLowerCase());  |
| 15  |     | // String toUpperCase():把字符串转换为大写字符串 |
| 16  |     | System.out.println("HelloWorld".toUpperCase());  |
| 17  | }   |                                                  |

### 4、其他方法

【常用方法】

| 1   | 去除字符串两端空格：String trim()                  |
| --- | -------------------------------------------------- |
| 2   | 按照指定符号分割字符串：String[] split(String str) |

【演示】

| 1   | public static void main(String args[]) {       |
| --- | ---------------------------------------------- |
| 2   | // 创建字符串对象                              |
| 3   | String s1 = "helloworld";                      |
| 4   | String s2 = " helloworld ";                    |
| 5   | String s3 = " hello world ";                   |
| 6   | System.out.println("---" + s1 + "---");        |
| 7   | System.out.println("---" + s1.trim() + "---"); |
| 8   | System.out.println("---" + s2 + "---");        |
| 9   | System.out.println("---" + s2.trim() + "---"); |
| 10  | System.out.println("---" + s3 + "---");        |
| 11  | System.out.println("---" + s3.trim() + "---"); |
| 12  | System.out.println(" ");                       |
| 13  |                                                |
| 14  | // String[] split(String str)                  |
| 15  | // 创建字符串对象                              |
| 16  | String s4 = "aa,bb,cc";                        |
| 17  | String[] strArray = s4.split(",");             |
| 18  | for (int x = 0; x < strArray.length; x++) {    |
| 19  | System.out.println(strArray[x]);               |
| 20  | }                                              |
| 21  | }                                              |

## 4、String 的不可变性

当我们去阅读源代码的时候，会发现有这样的一句话：

| 1   | Strings | are | constant; | their | values | cannot | be  | changed | after | they | are | created. |
| --- | ------- | --- | --------- | ----- | ------ | ------ | --- | ------- | ----- | ---- | --- | -------- |

意思就是说：String 是个常量，从一出生就注定不可变。

我想大家应该就知道为什么 String 不可变了，String 类被 ﬁnal 修饰，官方注释说明创建后不能被改变，但 是为什么 String 要使用 ﬁnal 修饰呢？
【了解一个经典的面试题】

| 1
2
3
4 | public static void main(String[] args) { String a = "abc";
String b = "abc";
String c = new String("abc"); | |
| --- | --- | --- |
| 5 | | System.out.println(a==b); //true |
| 6 | | System.out.println(a.equals(b)); //true |
| 7 | | System.out.println(a==c); //false |
| 8 | | System.out.println(a.equals(c)); //true |
| 9 | } | |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834182830-ac7be430-ae3d-4a59-9f71-2c4308883f83.png#)内存图分析：

[

【分析】
因为 String 太过常用，JAVA 类库的设计者在实现时做了个小小的变化，即采用了享元模式,每当生成一个 新内容的字符串时，他们都被添加到一个共享池中，当第二次再次生成同样内容的字符串实例时，就共 享此对象，而不是创建一个新对象，但是这样的做法仅仅适合于通过=符号进行的初始化。
需要说明一点的是，在 object 中，equals()是用来比较内存地址的，但是 String 重写了 equals()方 法，用来比较内容的，即使是不同地址，只要内容一致，也会返回 true，这也就是为什么 a.equals(c)返 回 true 的原因了。
【String 不可变的好处】
可以实现多个变量引用堆内存中的同一个字符串实例，避免创建的开销。我们的程序中大量使用了 String 字符串，有可能是出于安全性考虑。
大家都知道 HashMap 中 key 为 String 类型，如果可变将变的多么可怕。
当我们在传参的时候，使用不可变类不需要去考虑谁可能会修改其内部的值，如果使用可变类的 话，可能需要每次记得重新拷贝出里面的值，性能会有一定的损失。

## 5、字符串常量池

【字符串常量池概述】

1. 常量池表（Constant_Pool table）
   Class 文件中存储所有常量（包括字符串）的 table。这是 Class 文件中的内容，还不是运行时的内容，不 要理解它是个池子，其实就是 Class 文件中的字节码指令。
1. 运行时常量池（Runtime Constant Pool）

JVM 内存中方法区的一部分，这是运行时的内容。这部分内容（绝大部分）是随着 JVM 运行时候，从常 量池转化而来，每个 Class 对应一个运行时常量池。上一句中说绝大部分是因为：除了 Class 中常量池内容，还可能包括动态生成并加入这里的内容。

1. 字符串常量池（String Pool）
   这部分也在方法区中，但与 Runtime Constant Pool 不是一个概念，String Pool 是 JVM 实例全局共享的，全局只有一个。JVM 规范要求进入这里的 String 实例叫“被驻留的 interned string”，各个 JVM 可以有不同的实现，HotSpot 是设置了一个哈希表 StringTable 来引用堆中的字符串实例，被引用就是被驻留。
   【亨元模式】
   其实字符串常量池这个问题涉及到一个设计模式，叫“享元模式”，顾名思义 - - - > 共享元素模式
   也就是说：一个系统中如果有多处用到了相同的一个元素，那么我们应该只存储一份此元素，而让所有 地方都引用这一个元素
   Java 中 String 部分就是根据享元模式设计的，而那个存储元素的地方就叫做“字符串常量池 - String Pool”
   【详细分析】

| 1   | int x  | =   | 10;        |
| --- | ------ | --- | ---------- |
| 2   | String | y   | = "hello"; |

1. 首先， 10 和 "hello" 会在经过 javac（或者其他编译器）编译过后变为 Class 文件中

的内容
constant_pool table

1. 当我们的程序运行时，也就是说 JVM 运行时，每个 Class 载到 JVM 内存中的方法区中各自 Class 的

Runtime Constant Pool。
constant_pool table
中的内容会被加

1. 一个没有被 String Pool 包含的 Runtime Constant Pool 中的字符串（这里是"hello"）会被加入到 String Pool 中（HosSpot 使用 hashtable 引用方式），步骤如下：
   1. 在 Java Heap（堆）中根据"hello"字面量 create 一个字符串对象
   1. 将字面量"hello"与字符串对象的引用在 hashtable 中关联起来键 - 值

形式是："hello" = 对象的引用地址。
另外来说，当一个新的字符串出现在 Runtime Constant Pool 中时怎么判断需不需要在 Java Heap 中创建新对象呢？
策略是这样：会先去根据 equals 来比较 Runtime Constant Pool 中的这个字符串是否和 String Pool 中某一个是相等的（也就是找是否已经存在），如果有那么就不创建，直接使用其引用；反之，就如同上面 的第三步。
如此，就实现了享元模式，提高的内存利用效率。举例：

| 1   | 使用 String s = new String("hello");会创建几个对象                                          |
| --- | ------------------------------------------------------------------------------------------- |
| 2   | 答：会创建 2 个对象                                                                         |
| 3   |                                                                                             |
| 4   | 首先，出现了字面量"hello"，那么去 String Pool 中查找是否有相同字符串存在，因为程序就这一行  |
|     | 代码所以肯定没有，那么就在 Java Heap 中用字面量"hello"首先创建 1 个 String 对象。           |
| 5   |                                                                                             |
| 6   | 接着，new String("hello")，关键字 new 又在 Java Heap 中创建了 1 个对象，然后调用接收 String |
|     | 参数的构造器进行了初始化。最终 s 的引用是这个 String 对象.                                  |

# StringBuilder 和 StringBuﬀer

## 1、概述

【演示：查看源码及 API 文档】

public final class StringBuilder extends AbstractStringBuilder
implements java.io.Serializable, CharSequence{

}
1
2
3
4
5

StringBuilder 是一个可变的字符序列。它继承于 AbstractStringBuilder，实现了 CharSequence 接口。StringBuﬀer 也是继承于 AbstractStringBuilder 的子类；但是，StringBuilder 和 StringBuﬀer 不同，前者是非线程安全的，后者是线程安全的。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834183138-e8f9867d-b065-4d1d-b2ce-7d058275861f.jpeg#)StringBuilder 和 CharSequence 之间的关系图如下：

[

【源码概览】

| 1   | package java.lang;                                                             |
| --- | ------------------------------------------------------------------------------ |
| 2   |                                                                                |
| 3   | public final class StringBuilder                                               |
| 4   | extends AbstractStringBuilder                                                  |
| 5   | implements java.io.Serializable, CharSequence {                                |
| 6   |                                                                                |
| 7   | static final long serialVersionUID = 4383685877147921099L;                     |
| 8   |                                                                                |
| 9   | // 构造函数。默认的字符数组大小是 16。                                         |
| 10  | public StringBuilder() {                                                       |
| 11  | super(16);                                                                     |
| 12  | }                                                                              |
| 13  |                                                                                |
| 14  | // 构造函数。指定 StringBuilder 的字符数组大小是 capacity。                    |
| 15  | public StringBuilder(int capacity) {                                           |
| 16  | super(capacity);                                                               |
| 17  | }                                                                              |
| 18  |                                                                                |
| 19  | // 构造函数。指定字符数组大小=str 长度+15，且将 str 的值赋值到当前字符数组中。 |
| 20  | public StringBuilder(String str) {                                             |
| 21  | super(str.length() + 16);                                                      |
| 22  | append(str);                                                                   |
| 23  | }                                                                              |
| 24  |                                                                                |
| 25  | // 构造函数。指定字符数组大小=seq 长度+15，且将 seq 的值赋值到当前字符数组中。 |
| 26  | public StringBuilder(CharSequence seq) {                                       |
| 27  | this(seq.length() + 16);                                                       |
| 28  | append(seq);                                                                   |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 29  | }                                                                              |
| --- | ------------------------------------------------------------------------------ |
| 30  |                                                                                |
| 31  | // 追加“对象 obj 对应的字符串”。String.valueOf(obj)实际上是调用 obj.toString() |
| 32  | public StringBuilder append(Object obj) {                                      |
| 33  | return append(String.valueOf(obj));                                            |
| 34  | }                                                                              |
| 35  |                                                                                |
| 36  | // 追加“str”。                                                                 |
| 37  | public StringBuilder append(String str) {                                      |
| 38  | super.append(str);                                                             |
| 39  | return this;                                                                   |
| 40  | }                                                                              |
| 41  |                                                                                |
| 42  | // 追加“sb 的内容”。                                                           |
| 43  | private StringBuilder append(StringBuilder sb) {                               |
| 44  | if (sb == null)                                                                |
| 45  | return append("null");                                                         |
| 46  | int len = sb.length();                                                         |
| 47  | int newcount = count + len;                                                    |
| 48  | if (newcount > value.length)                                                   |
| 49  | expandCapacity(newcount);                                                      |
| 50  | sb.getChars(0, len, value, count);                                             |
| 51  | count = newcount;                                                              |
| 52  | return this;                                                                   |
| 53  | }                                                                              |
| 54  |                                                                                |
| 55  | // 追加“sb 的内容”。                                                           |
| 56  | public StringBuilder append(StringBuffer sb) {                                 |
| 57  | super.append(sb);                                                              |
| 58  | return this;                                                                   |
| 59  | }                                                                              |
| 60  |                                                                                |
| 61  | // 追加“s 的内容”。                                                            |
| 62  | public StringBuilder append(CharSequence s) {                                  |
| 63  | if (s == null)                                                                 |
| 64  | s = "null";                                                                    |
| 65  | if (s instanceof String)                                                       |
| 66  | return this.append((String)s);                                                 |
| 67  | if (s instanceof StringBuffer)                                                 |
| 68  | return this.append((StringBuffer)s);                                           |
| 69  | if (s instanceof StringBuilder)                                                |
| 70  | return this.append((StringBuilder)s);                                          |
| 71  | return this.append(s, 0, s.length());                                          |
| 72  | }                                                                              |
| 73  |                                                                                |
| 74  | // 追加“s 从 start(包括)到 end(不包括)的内容”。                                |
| 75  | public StringBuilder append(CharSequence s, int start, int end) {              |
| 76  | super.append(s, start, end);                                                   |
| 77  | return this;                                                                   |
| 78  | }                                                                              |
| 79  |                                                                                |
| 80  | // 追加“str 字符数组对应的字符串”                                              |
| 81  | public StringBuilder append(char[] str) {                                      |
| 82  | super.append(str);                                                             |
| 83  | return this;                                                                   |
| 84  | }                                                                              |
| 85  |                                                                                |
| 86  | // 追加“str 从 offset 开始的内容，内容长度是 len”                              |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 87  | public StringBuilder append(char[] str, int offset, int len) { |
| --- | -------------------------------------------------------------- |
| 88  | super.append(str, offset, len);                                |
| 89  | return this;                                                   |
| 90  | }                                                              |
| 91  |                                                                |
| 92  | // 追加“b 对应的字符串”                                        |
| 93  | public StringBuilder append(boolean b) {                       |
| 94  | super.append(b);                                               |
| 95  | return this;                                                   |
| 96  | }                                                              |
| 97  |                                                                |
| 98  | // 追加“c”                                                     |
| 99  | public StringBuilder append(char c) {                          |
| 100 | super.append(c);                                               |
| 101 | return this;                                                   |
| 102 | }                                                              |
| 103 |                                                                |
| 104 | // 追加“i”                                                     |
| 105 | public StringBuilder append(int i) {                           |
| 106 | super.append(i);                                               |
| 107 | return this;                                                   |
| 108 | }                                                              |
| 109 |                                                                |
| 110 | // 追加“lng”                                                   |
| 111 | public StringBuilder append(long lng) {                        |
| 112 | super.append(lng);                                             |
| 113 | return this;                                                   |
| 114 | }                                                              |
| 115 |                                                                |
| 116 | // 追加“f”                                                     |
| 117 | public StringBuilder append(float f) {                         |
| 118 | super.append(f);                                               |
| 119 | return this;                                                   |
| 120 | }                                                              |
| 121 |                                                                |
| 122 | // 追加“d”                                                     |
| 123 | public StringBuilder append(double d) {                        |
| 124 | super.append(d);                                               |
| 125 | return this;                                                   |
| 126 | }                                                              |
| 127 |                                                                |
| 128 | // 追加“codePoint”                                             |
| 129 | public StringBuilder appendCodePoint(int codePoint) {          |
| 130 | super.appendCodePoint(codePoint);                              |
| 131 | return this;                                                   |
| 132 | }                                                              |
| 133 |                                                                |
| 134 | // 删除“从 start(包括)到 end 的内容”                           |
| 135 | public StringBuilder delete(int start, int end) {              |
| 136 | super.delete(start, end);                                      |
| 137 | return this;                                                   |
| 138 | }                                                              |
| 139 |                                                                |
| 140 | // 删除“位置 index 的内容”                                     |
| 141 | public StringBuilder deleteCharAt(int index) {                 |
| 142 | super.deleteCharAt(index);                                     |
| 143 | return this;                                                   |
| 144 | }                                                              |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 145 |                                                                                      |
| --- | ------------------------------------------------------------------------------------ |
| 146 | // “用 str 替换 StringBuilder 中从 start(包括)到 end(不包括)的内容”                  |
| 147 | public StringBuilder replace(int start, int end, String str) {                       |
| 148 | super.replace(start, end, str);                                                      |
| 149 | return this;                                                                         |
| 150 | }                                                                                    |
| 151 |                                                                                      |
| 152 | // “在 StringBuilder 的位置 index 处插入‘str 中从 offset 开始的内容’，插入内容长度是 |
|     | len”                                                                                 |
| 153 | public StringBuilder insert(int index, char[] str, int offset,                       |
| 154 | int len)                                                                             |
| 155 | {                                                                                    |
| 156 | super.insert(index, str, offset, len);                                               |
| 157 | return this;                                                                         |
| 158 | }                                                                                    |
| 159 |                                                                                      |
| 160 | // “在 StringBuilder 的位置 offset 处插入 obj 对应的字符串”                          |
| 161 | public StringBuilder insert(int offset, Object obj) {                                |
| 162 | return insert(offset, String.valueOf(obj));                                          |
| 163 | }                                                                                    |
| 164 |                                                                                      |
| 165 | // “在 StringBuilder 的位置 offset 处插入 str”                                       |
| 166 | public StringBuilder insert(int offset, String str) {                                |
| 167 | super.insert(offset, str);                                                           |
| 168 | return this;                                                                         |
| 169 | }                                                                                    |
| 170 |                                                                                      |
| 171 | // “在 StringBuilder 的位置 offset 处插入 str”                                       |
| 172 | public StringBuilder insert(int offset, char[] str) {                                |
| 173 | super.insert(offset, str);                                                           |
| 174 | return this;                                                                         |
| 175 | }                                                                                    |
| 176 |                                                                                      |
| 177 | // “在 StringBuilder 的位置 dstOffset 处插入 s”                                      |
| 178 | public StringBuilder insert(int dstOffset, CharSequence s) {                         |
| 179 | if (s == null)                                                                       |
| 180 | s = "null";                                                                          |
| 181 | if (s instanceof String)                                                             |
| 182 | return this.insert(dstOffset, (String)s);                                            |
| 183 | return this.insert(dstOffset, s, 0, s.length());                                     |
| 184 | }                                                                                    |
| 185 |                                                                                      |
| 186 | // “在 StringBuilder 的位置 dstOffset 处插入's 中从 start 到 end 的内容'”            |
| 187 | public StringBuilder insert(int dstOffset, CharSequence s,                           |
| 188 | int start, int end)                                                                  |
| 189 | {                                                                                    |
| 190 | super.insert(dstOffset, s, start, end);                                              |
| 191 | return this;                                                                         |
| 192 | }                                                                                    |
| 193 |                                                                                      |
| 194 | // “在 StringBuilder 的位置 Offset 处插入 b”                                         |
| 195 | public StringBuilder insert(int offset, boolean b) {                                 |
| 196 | super.insert(offset, b);                                                             |
| 197 | return this;                                                                         |
| 198 | }                                                                                    |
| 199 |                                                                                      |
| 200 | // “在 StringBuilder 的位置 Offset 处插入 c”                                         |
| 201 | public StringBuilder insert(int offset, char c) {                                    |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 202 | super.insert(offset, c);                                             |
| --- | -------------------------------------------------------------------- |
| 203 | return this;                                                         |
| 204 | }                                                                    |
| 205 |                                                                      |
| 206 | // “在 StringBuilder 的位置 Offset 处插入 i”                         |
| 207 | public StringBuilder insert(int offset, int i) {                     |
| 208 | return insert(offset, String.valueOf(i));                            |
| 209 | }                                                                    |
| 210 |                                                                      |
| 211 | // “在 StringBuilder 的位置 Offset 处插入 l”                         |
| 212 | public StringBuilder insert(int offset, long l) {                    |
| 213 | return insert(offset, String.valueOf(l));                            |
| 214 | }                                                                    |
| 215 |                                                                      |
| 216 | // “在 StringBuilder 的位置 Offset 处插入 f”                         |
| 217 | public StringBuilder insert(int offset, float f) {                   |
| 218 | return insert(offset, String.valueOf(f));                            |
| 219 | }                                                                    |
| 220 |                                                                      |
| 221 | // “在 StringBuilder 的位置 Offset 处插入 d”                         |
| 222 | public StringBuilder insert(int offset, double d) {                  |
| 223 | return insert(offset, String.valueOf(d));                            |
| 224 | }                                                                    |
| 225 |                                                                      |
| 226 | // 返回“str”在 StringBuilder 的位置                                  |
| 227 | public int indexOf(String str) {                                     |
| 228 | return indexOf(str, 0);                                              |
| 229 | }                                                                    |
| 230 |                                                                      |
| 231 | // 从 fromIndex 开始查找，返回“str”在 StringBuilder 的位置           |
| 232 | public int indexOf(String str, int fromIndex) {                      |
| 233 | return String.indexOf(value, 0, count,                               |
| 234 | str.toCharArray(), 0, str.length(),                                  |
|     | fromIndex);                                                          |
| 235 | }                                                                    |
| 236 |                                                                      |
| 237 | // 从后向前查找，返回“str”在 StringBuilder 的位置                    |
| 238 | public int lastIndexOf(String str) {                                 |
| 239 | return lastIndexOf(str, count);                                      |
| 240 | }                                                                    |
| 241 |                                                                      |
| 242 | // 从 fromIndex 开始，从后向前查找，返回“str”在 StringBuilder 的位置 |
| 243 | public int lastIndexOf(String str, int fromIndex) {                  |
| 244 | return String.lastIndexOf(value, 0, count,                           |
| 245 | str.toCharArray(), 0, str.length(),                                  |
|     | fromIndex);                                                          |
| 246 | }                                                                    |
| 247 |                                                                      |
| 248 | // 反转 StringBuilder                                                |
| 249 | public StringBuilder reverse() {                                     |
| 250 | super.reverse();                                                     |
| 251 | return this;                                                         |
| 252 | }                                                                    |
| 253 |                                                                      |
| 254 | public String toString() {                                           |
| 255 | // Create a copy, don't share the array                              |
| 256 | return new String(value, 0, count);                                  |
| 257 | }                                                                    |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 258 |     |                                                        |
| --- | --- | ------------------------------------------------------ |
| 259 |     | // 序列化对应的写入函数                                |
| 260 |     | private void writeObject(java.io.ObjectOutputStream s) |
| 261 |     | throws java.io.IOException {                           |
| 262 |     | s.defaultWriteObject();                                |
| 263 |     | s.writeInt(count);                                     |
| 264 |     | s.writeObject(value);                                  |
| 265 |     | }                                                      |
| 266 |     |                                                        |
| 267 |     | // 序列化对应的读取函数                                |
| 268 |     | private void readObject(java.io.ObjectInputStream s)   |
| 269 |     | throws java.io.IOException, ClassNotFoundException {   |
| 270 |     | s.defaultReadObject();                                 |
| 271 |     | count = s.readInt();                                   |
| 272 |     | value = (char[]) s.readObject();                       |
| 273 |     | }                                                      |
| 274 | }   |                                                        |

## 2、常用方法

### 1、insert

| 1   | private static void testInsertAPIs(){                                                      |
| --- | ------------------------------------------------------------------------------------------ |
| 2   | System.out.println("---------- testInsertAPIs ");                                          |
| 3   |                                                                                            |
| 4   | StringBuilder sbuilder = new StringBuilder();                                              |
| 5   |                                                                                            |
| 6   | // 在位置 0 处插入字符数组                                                                 |
| 7   | sbuilder.insert(0, new char[]{'a', 'b', 'c', 'd', 'e'});                                   |
| 8   | // 在位置 0 处插入字符数组。0 表示字符数组起始位置，3 表示长度                             |
| 9   | sbuilder.insert(0, new char[]{'A', 'B', 'C', 'D', 'E'}, 0, 3);                             |
| 10  | // 在位置 0 处插入 float                                                                   |
| 11  | sbuilder.insert(0, 1.414f);                                                                |
| 12  | // 在位置 0 处插入 double                                                                  |
| 13  | sbuilder.insert(0, 3.14159d);                                                              |
| 14  | // 在位置 0 处插入 boolean                                                                 |
| 15  | sbuilder.insert(0, true);                                                                  |
| 16  | // 在位置 0 处插入 char                                                                    |
| 17  | sbuilder.insert(0, '\n');                                                                  |
| 18  | // 在位置 0 处插入 int                                                                     |
| 19  | sbuilder.insert(0, 100);                                                                   |
| 20  | // 在位置 0 处插入 long                                                                    |
| 21  | sbuilder.insert(0, 12345L);                                                                |
| 22  | // 在位置 0 处插入 StringBuilder 对象                                                      |
| 23  | sbuilder.insert(0, new StringBuilder("StringBuilder"));                                    |
| 24  | // 在位置 0 处插入 StringBuilder 对象。6 表示被在位置 0 处插入对象的起始位置(包括)，13 是  |
|     | 结束位置(不包括)                                                                           |
| 25  | sbuilder.insert(0, new StringBuilder("STRINGBUILDER"), 6, 13);                             |
| 26  | // 在位置 0 处插入 StringBuffer 对象。                                                     |
| 27  | sbuilder.insert(0, new StringBuffer("StringBuffer"));                                      |
| 28  | // 在位置 0 处插入 StringBuffer 对象。6 表示被在位置 0 处插入对象的起始位置(包括)，12 是结 |
|     | 束位置(不包括)                                                                             |
| 29  | sbuilder.insert(0, new StringBuffer("STRINGBUFFER"), 6, 12);                               |
| 30  | // 在位置 0 处插入 String 对象。                                                           |
| 31  | sbuilder.insert(0, "String");                                                              |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 32  | // 在位置 0 处插入 String 对象。1 表示被在位置 0 处插入对象的起始位置(包括)，6 是结束位置(不 |
| --- | -------------------------------------------------------------------------------------------- |
|     | 包括)                                                                                        |
| 33  | sbuilder.insert(0, "0123456789", 1, 6);                                                      |
| 34  | sbuilder.insert(0, '\n');                                                                    |
| 35  |                                                                                              |
| 36  | // 在位置 0 处插入 Object 对象。此处以 HashMap 为例                                          |
| 37  | HashMap map = new HashMap();                                                                 |
| 38  | map.put("1", "one");                                                                         |
| 39  | map.put("2", "two");                                                                         |
| 40  | map.put("3", "three");                                                                       |
| 41  |                                                                                              |
| 42  | sbuilder.insert(0, map);                                                                     |
| 43  |                                                                                              |
| 44  | System.out.printf("%s\n\n", sbuilder);                                                       |
| 45  | }                                                                                            |

### 2、append

| 1   | /\*\*                                                                               |
| --- | ----------------------------------------------------------------------------------- |
| 2   | \* StringBuilder 的 append()示例                                                    |
| 3   | \*/                                                                                 |
| 4   | private static void testAppendAPIs() {                                              |
| 5   |                                                                                     |
| 6   | System.out.println("------------------- testAppendAPIs -----------------            |
|     | --");                                                                               |
| 7   |                                                                                     |
| 8   | StringBuilder sbuilder = new StringBuilder();                                       |
| 9   |                                                                                     |
| 10  | // 追加字符数组                                                                     |
| 11  | sbuilder.append(new char[]{'a','b','c','d','e'});                                   |
| 12  | // 追加字符数组。0 表示字符数组起始位置，3 表示长度                                 |
| 13  | sbuilder.append(new char[]{'A','B','C','D','E'}, 0, 3);                             |
| 14  | // 追加 float                                                                       |
| 15  | sbuilder.append(1.414f);                                                            |
| 16  | // 追加 double                                                                      |
| 17  | sbuilder.append(3.14159d);                                                          |
| 18  | // 追加 boolean                                                                     |
| 19  | sbuilder.append(true);                                                              |
| 20  | // 追加 char                                                                        |
| 21  | sbuilder.append('\n');                                                              |
| 22  | // 追加 int                                                                         |
| 23  | sbuilder.append(100);                                                               |
| 24  | // 追加 long                                                                        |
| 25  | sbuilder.append(12345L);                                                            |
| 26  | // 追加 StringBuilder 对象                                                          |
| 27  | sbuilder.append(new StringBuilder("StringBuilder"));                                |
| 28  | // 追加 StringBuilder 对象。6 表示被追加对象的起始位置(包括)，13 是结束位置(不包括) |
| 29  | sbuilder.append(new StringBuilder("STRINGBUILDER"), 6, 13);                         |
| 30  | // 追加 StringBuffer 对象。                                                         |
| 31  | sbuilder.append(new StringBuffer("StringBuffer"));                                  |
| 32  | // 追加 StringBuffer 对象。6 表示被追加对象的起始位置(包括)，12 是结束位置(不包括)  |
| 33  | sbuilder.append(new StringBuffer("STRINGBUFFER"), 6, 12);                           |
| 34  | // 追加 String 对象。                                                               |
| 35  | sbuilder.append("String");                                                          |
| 36  | // 追加 String 对象。1 表示被追加对象的起始位置(包括)，6 是结束位置(不包括)         |
| 37  | sbuilder.append("0123456789", 1, 6);                                                |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 38  |     | sbuilder.append('\n');                                            |
| --- | --- | ----------------------------------------------------------------- |
| 39  |     |                                                                   |
| 40  |     | // 追加 Object 对象。此处以 HashMap 为例                          |
| 41  |     | HashMap map = new HashMap();                                      |
| 42  |     | map.put("1", "one");                                              |
| 43  |     | map.put("2", "two");                                              |
| 44  |     | map.put("3", "three");                                            |
| 45  |     | sbuilder.append(map);                                             |
| 46  |     | sbuilder.append('\n');                                            |
| 47  |     |                                                                   |
| 48  |     | // 追加 unicode 编码                                              |
| 49  |     | sbuilder.appendCodePoint(0x5b57); // 0x5b57 是“字”的 unicode 编码 |
| 50  |     | sbuilder.appendCodePoint(0x7b26); // 0x7b26 是“符”的 unicode 编码 |
| 51  |     | sbuilder.appendCodePoint(0x7f16); // 0x7f16 是“编”的 unicode 编码 |
| 52  |     | sbuilder.appendCodePoint(0x7801); // 0x7801 是“码”的 unicode 编码 |
| 53  |     |                                                                   |
| 54  |     | System.out.printf("%s\n\n", sbuilder);                            |
| 55  | }   |                                                                   |

### 3、replace

| 1   | /\*\*                                                                    |
| --- | ------------------------------------------------------------------------ |
| 2   | \* StringBuilder 的 replace()示例                                        |
| 3   | \*/                                                                      |
| 4   | private static void testReplaceAPIs() {                                  |
| 5   |                                                                          |
| 6   | System.out.println("------------------- testReplaceAPIs----------------- |
|     | --");                                                                    |
| 7   |                                                                          |
| 8   | StringBuilder sbuilder;                                                  |
| 9   |                                                                          |
| 10  | sbuilder = new StringBuilder("0123456789");                              |
| 11  | sbuilder.replace(0, 3, "ABCDE");                                         |
| 12  | System.out.printf("sbuilder=%s\n", sbuilder);                            |
| 13  |                                                                          |
| 14  | sbuilder = new StringBuilder("0123456789");                              |
| 15  | sbuilder.reverse();                                                      |
| 16  | System.out.printf("sbuilder=%s\n", sbuilder);                            |
| 17  |                                                                          |
| 18  | sbuilder = new StringBuilder("0123456789");                              |
| 19  | sbuilder.setCharAt(0, 'M');                                              |
| 20  | System.out.printf("sbuilder=%s\n", sbuilder);                            |
| 21  |                                                                          |
| 22  | System.out.println();                                                    |
| 23  | }                                                                        |

**4、delete**

1
2
3
private static void testDeleteAPIs() {
System.out.println("------------------- testDeleteAPIs -----------------
--");
4
5 StringBuilder sbuilder = new StringBuilder("0123456789");
狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 6   |                                                                                       |
| --- | ------------------------------------------------------------------------------------- |
| 7   | // 删除位置 0 的字符，剩余字符是“123456789”。                                         |
| 8   | sbuilder.deleteCharAt(0);                                                             |
| 9   | // 删除位置 3(包括)到位置 6(不包括)之间的字符，剩余字符是“123789”。                   |
| 10  | sbuilder.delete(3,6);                                                                 |
| 11  |                                                                                       |
| 12  | // 获取 sb 中从位置 1 开始的字符串                                                    |
| 13  | String str1 = sbuilder.substring(1);                                                  |
| 14  | // 获取 sb 中从位置 3(包括)到位置 5(不包括)之间的字符串                               |
| 15  | String str2 = sbuilder.substring(3, 5);                                               |
| 16  | // 获取 sb 中从位置 3(包括)到位置 5(不包括)之间的字符串，获取的对象是 CharSequence 对 |
|     | 象，此处转型为 String                                                                 |
| 17  | String str3 = (String)sbuilder.subSequence(3, 5);                                     |
| 18  |                                                                                       |
| 19  | System.out.printf("sbuilder=%s\nstr1=%s\nstr2=%s\nstr3=%s\n",                         |
| 20  | sbuilder, str1, str2, str3);                                                          |
| 21  | }                                                                                     |

### 5、index

| 1   | /\*\*                                                                    |
| --- | ------------------------------------------------------------------------ |
| 2   | \* StringBuilder 中 index 相关 API 演示                                  |
| 3   | \*/                                                                      |
| 4   | private static void testIndexAPIs() {                                    |
| 5   | System.out.println("-------------------------------- testIndexAPIs ----- |
|     | ");                                                                      |
| 6   |                                                                          |
| 7   | StringBuilder sbuilder = new StringBuilder("abcAbcABCabCaBcAbCaBCabc");  |
| 8   | System.out.printf("sbuilder=%s\n", sbuilder);                            |
| 9   |                                                                          |
| 10  | // 1. 从前往后，找出"bc"第一次出现的位置                                 |
| 11  | System.out.printf("%-30s = %d\n", "sbuilder.indexOf(\"bc\")",            |
|     | sbuilder.indexOf("bc"));                                                 |
| 12  |                                                                          |
| 13  | // 2. 从位置 5 开始，从前往后，找出"bc"第一次出现的位置                  |
| 14  | System.out.printf("%-30s = %d\n", "sbuilder.indexOf(\"bc\", 5)",         |
|     | sbuilder.indexOf("bc", 5));                                              |
| 15  |                                                                          |
| 16  | // 3. 从后往前，找出"bc"第一次出现的位置                                 |
| 17  | System.out.printf("%-30s = %d\n", "sbuilder.lastIndexOf(\"bc\")",        |
|     | sbuilder.lastIndexOf("bc"));                                             |
| 18  |                                                                          |
| 19  | // 4. 从位置 4 开始，从后往前，找出"bc"第一次出现的位置                  |
| 20  | System.out.printf("%-30s = %d\n", "sbuilder.lastIndexOf(\"bc\", 4)",     |
|     | sbuilder.lastIndexOf("bc", 4));                                          |
| 21  |                                                                          |
| 22  | System.out.println();                                                    |
| 23  | }                                                                        |

**6、其他 API**

1 /\*_
2 _ StringBuilder 的其它 API 示例
3 \*/
狂神社群笔记资料，禁止外传
狂神社群笔记资料，禁止外传，本人 QQ：24736743

| 4   | private static void testOtherAPIs() {                                              |
| --- | ---------------------------------------------------------------------------------- |
| 5   |                                                                                    |
| 6   | System.out.println("----------- testOtherAPIs ");                                  |
| 7   | StringBuilder sbuilder = new StringBuilder("0123456789");                          |
| 8   |                                                                                    |
| 9   | int cap = sbuilder.capacity();                                                     |
| 10  | System.out.printf("cap=%d\n", cap);                                                |
| 11  |                                                                                    |
| 12  | /\*                                                                                |
| 13  | capacity()返回的是字符串缓冲区的容量                                               |
| 14  | StringBuffer( ); 分配 16 个字符的缓冲区                                            |
| 15  | StringBuffer( int len ); 分配 len 个字符的缓冲区                                   |
| 16  | StringBuffer( String s ); 除了按照 s 的大小分配空间外,再分配 16 个 字符的缓冲区    |
| 17  | 你的 StringBuffer 是用字符构造的，"0123456789"的长度是 10 另外再分配 16 个字符，所 |
|     | 以一共是 26。                                                                      |
| 18  | \*/                                                                                |
| 19  |                                                                                    |
| 20  | char c = sbuilder.charAt(6);                                                       |
| 21  | System.out.printf("c=%c\n", c);                                                    |
| 22  |                                                                                    |
| 23  | char[] carr = new char[4];                                                         |
| 24  | sbuilder.getChars(3, 7, carr, 0);                                                  |
| 25  | for (int i=0; i<carr.length; i++){                                                 |
| 26  | System.out.printf("carr[%d]=%c ", i, carr[i]);                                     |
| 27  | }                                                                                  |
| 28  | System.out.println();                                                              |
| 29  | }                                                                                  |

## 3、StringBuﬀer

和 StringBulider 用法差不多，不过多介绍，主要看一下三者的区别

## 4、小结

【String、StringBuﬀer、StringBuilder 之间的区别】首先需要说明的是：
String 字符串常量
StringBuﬀer 字符串变量（线程安全）
StringBuilder 字符串变量（非线程安全）
在大多数情况下三者在执行速度方面的比较：StringBuilder > StringBuﬀer > String
解释：
String 类型和 StringBuﬀer 类型的主要性能区别其实在于 String 是不可变的对象, 因此在每次对 String 类型进行改变的时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象，所以经常改变内容的字符串最好不要用 String ，因为每次生成对象都会对系统性能产生影响，特别当内存中无引用对象多了以后， JVM 的 GC 就会开始工作，那速度是一定会相当慢的。
而如果是使用 StringBuﬀer 类则结果就不一样了，每次结果都会对 StringBuﬀer 对象本身进行操作，而不是生成新的对象，再改变对象引用。所以在一般情况下我们推荐使用 StringBuﬀer ，特别是字符串对象经常改变的情况下。
为什么是大多数情况呢？

在某些特别情况下， String 对象的字符串拼接其实是被 JVM 解释成了 StringBuﬀer 对象的拼接， 所以这些时候 String 对象的速度并不会比 StringBuﬀer 对象慢，而特别是以下的字符串对象生成中， String 效率是远要比 StringBuﬀer 快的：

| 1   | String S1 = “This | is only a” + “ simple” + | “   | test”;            |
| --- | ----------------- | ------------------------ | --- | ----------------- |
| 2   | StringBuffer Sb = | new StringBuilder(“This  | is  | only a”).append(“ |
|     | simple”).append(“ | test”);                  |     |                   |

你会很惊讶的发现，生成 String S1 对象的速度简直太快了，而这个时候 StringBuﬀer 居然速度上根本一点都不占优势。其实这是 JVM 的一个把戏，在 JVM 眼里，这个
String S1 = “This is only a” + “ simple” + “test”;
其实就是：String S1 = “This is only a simple test”;
所以当然不需要太多的时间了。但大家这里要注意的是，如果你的字符串是来自另外的 String 对象的话，速度就没那么快了，譬如：
String S2 = “This is only a”; String S3 = “ simple”; String S4 = “ test”;
大部分情况下 StringBuilder 的速度要大于 StringBuﬀer：
java.lang.StringBuilder 一个可变的字符序列是 5.0 新增的。（大多数情况下就是我们是在单线程下进行 的操作，所以大多数情况下是建议用 StringBuilder 而不用 StringBuﬀer 的）此类提供一个与 StringBuﬀer 兼容的 API，但不保证同步。该类被设计用作 StringBuﬀer 的一个简易替换，用在字符串缓冲区被单个线程使用的时候（这种情况很普遍）。如果可能，建议优先采用该类，因为在大多数实现中，它比 StringBuﬀer 要快。两者的方法基本相同。
对于三者使用的总结：
1） 如 果 要 操 作 少 量 的 数 据 用 = String 2）单线程操作字符串缓冲区 下操作大量数据 = StringBuilder 3）多线程操作字符串缓冲区 下操作大量数据 = StringBuﬀer

## 5、面试题的回答

#### StringBuilder 与 StringBuﬀer 的区别，StringBuilder 与 String 的区别。

1. StringBuilder 效率高，线程不安全，StringBuﬀer 效率低，线程安全。
1. String 是不可变字符串，StringBuilder 是可变字符串。为什么有这样的差异，可以深入源码去解析， 比如 String 类内的 priver ﬁnal char value[] 等方法的原因。
1. 如果是简单的声明一个字符串没有后续过多的操作，使用 String,StringBuilder 均可，若后续对字符穿 做频繁的添加，删除操作,或者是在循环当中动态的改变字符穿的长度应该用 StringBuilder。使用 String 会产生多余的字符串，占用内存空间。

# File 类

## 1、File 类的基本用法

1.  java.io.File 类：文件和目录路径名的抽象表示形式。File 类的常见构造方法：

public File(String pathname)
1
以 pathname 为路径创建 File 对象，如果 pathname 是相对路径，则默认的当前路径在系统属性 user.dir 中存储。

1. File 的静态属性 String separator 存储了当前系统的路径分隔符。
1. 通过 File 对象可以访问文件的属性。

| 1   | public | boolean canRead()     |
| --- | ------ | --------------------- |
| 2   | public | boolean exists()      |
| 3   | public | boolean isFile()      |
| 4   | public | long lastModified()   |
| 5   | public | String getName()      |
| 6   | public | boolean canWrite()    |
| 7   | public | boolean isDirectory() |
| 8   | public | boolean isHidden()    |
| 9   | public | long length()         |
| 10  | public | String getPath()      |

1. 通过 File 对象创建空文件或目录（在该对象所指的文件或目录不存在的情况下）。

| 1   | public | boolean | createNewFile()throws | IOException |
| --- | ------ | ------- | --------------------- | ----------- |
| 2   | public | boolean | delete()              |             |
| 3   | public | boolean | mkdir()， mkdirs()    |             |

1. 常见构造器，方法
   【演示】

| 1   | import java.io.File;                          |
| --- | --------------------------------------------- |
| 2   | import java.io.IOException;                   |
| 3   |                                               |
| 4   | public class TestFile {                       |
| 5   | /\*\*                                         |
| 6   | \* File 文件类 1.代表文件 2.代表目录          |
| 7   | \*/                                           |
| 8   | public static void main(String[] args) {      |
| 9   | File f = new File("d:/src3/TestObject.java"); |
| 10  | File f2 = new File("d:/src3");                |
| 11  | File f3 = new File(f2, "TestFile.java");      |
| 12  | File f4 = new File(f2, "TestFile666.java");   |
| 13  | File f5 = new File("d:/src3/aa/bb/cc/dd");    |
| 14  | //f5.mkdirs();                                |
| 15  | f5.delete();                                  |
| 16  |                                               |
| 17  | try {                                         |
| 18  | f4.createNewFile();                           |
| 19  | System.out.println("文件创建成功！");         |
| 20  | } catch (IOException e) {                     |
| 21  | e.printStackTrace();                          |
| 22  | }                                             |
| 23  | if (f.isFile()) {                             |
| 24  | System.out.println("是一个文件！");           |
| 25  | }                                             |
| 26  | if (f2.isDirectory()) {                       |
| 27  | System.out.println("是一个目录！");           |
| 28  | }                                             |
| 29  | if (f3.isFile()) {                            |
| 30  | System.out.println("是一个文件奥");           |
| 31  | }                                             |

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743

}
}
32
33

狂神社群笔记资料，禁止外传 狂神社群笔记资料，禁止外传，本人 QQ：24736743
