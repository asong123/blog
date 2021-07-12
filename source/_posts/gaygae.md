---
title: JAVA-多线程
urlname: gaygae
date: '2021-07-12 12:54:13 +0800'
tags: []
categories: []
---

> 参考内容：
> 狂神说 Java(多线程学习) [https://www.bilibili.com/video/BV1V4411p7EF?p=2](https://www.bilibili.com/video/BV1V4411p7EF?p=2)

# 线程简介

**任务**
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1595923250271-68f1c040-1f33-4a72-b887-031103ab4509.png#height=251&id=Ah16T&name=image.png&originHeight=502&originWidth=1646&originalType=binary∶=1&size=691434&status=done&style=none&width=823)
**进程**
**进程（Process）**是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是操作系统结构的基础，每个应用程序就是一个进程。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1595923507047-40c6ded5-ddc5-4307-818e-38d43dce4692.png#height=52&id=gETe2&name=image.png&originHeight=52&originWidth=438&originalType=binary∶=1&size=9476&status=done&style=none&width=438)
**线程**
**线程（thread）**是操作系统能够进行运算调度的最小单位。**它被包含在进程之中**，是进程中的实际运作单位。一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程，每条线程并行执行不同的任务。
**​**

**多线程**
**多线程（multithreading）**是指从软件或者硬件上实现多个线程并发执行的技术。
    ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596004649956-d9da2843-428d-4585-b62f-198ad2f78112.png#height=368&id=e7jXL&name=image.png&originHeight=735&originWidth=1181&originalType=binary∶=1&size=286013&status=done&style=none&width=590.5)

# 线程实现

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596004592765-8fed54cc-486c-4373-be27-111634f26e26.png#height=328&id=gQY58&margin=%5Bobject%20Object%5D&name=image.png&originHeight=365&originWidth=814&originalType=binary∶=1&size=39059&status=done&style=none&width=731)

## Thread 类创建线程

### 创建线程方式一

1. 继承 Thread 类
2. 重写 run 方法
3. 调用 start 开启线程

```java
package Thread.demo01创建线程;
/*
创建线程方式一：
    1. 继承Thread类
    2. 重写run方法
    3. 调用start开启线程
 */

public class TestThread1 extends Thread{
    @Override
    public void run() {
        // run方法线程体
        for (int i = 0; i <20 ; i++) {
            System.out.println("我在看代码------"+i);
        }
    }

    public static void main(String[] args) {       // 主线程
        //创建一个线程对象
        TestThread1 tt = new TestThread1();
        // 调用start()方法
        tt.start();

        for (int i = 0; i < 200; i++) {
            System.out.println("我在学习多线程-------"+i);
        }

    }
}
```

### 案例：实现多线程下载图片

需要用到第三方工具包 commons.io-2.6.jar，下载地址：
链接: [https://pan.baidu.com/s/1hwOa9DiY4bjTZ2AV1n6JpA](https://pan.baidu.com/s/1hwOa9DiY4bjTZ2AV1n6JpA) 提取码: s1ag
下载好以后将其添加到 lib 文件夹下，记得**Add to Build path**

```java
package Thread.demo01创建线程;

import org.apache.commons.io.FileUtils;
import java.io.File;
import java.io.IOException;
import java.net.URL;

public class PictureDownloadThread extends Thread{
    String url;
    String name;

    public PictureDownloadThread(String url,String name){
        this.url = url;
        this.name = name;
    }

    @Override
    public void run() {
        WebDownloader webDownloader = new WebDownloader();
        webDownloader.downloader(url,name);
        System.out.println("下载了文件名为："+name);
    }

    public static void main(String[] args) {
        PictureDownloadThread pt1 = new PictureDownloadThread("https://pic.cnblogs.com/avatar/1418974/20181015113902.png","Kuang头.png");
        PictureDownloadThread pt2= new PictureDownloadThread("https://t7.baidu.com/it/u=3616242789,1098670747&fm=79&app=86&size=h300&n=0&g=4n&f=jpeg?sec=1596611654&t=6e68bfae4f52559d8856b922d491307a","girl.png");
        PictureDownloadThread pt3 = new PictureDownloadThread("https://t7.baidu.com/it/u=3204887199,3790688592&fm=79&app=86&size=h300&n=0&g=4n&f=jpeg?sec=1596611654&t=58b381c7ecb89fc6eb26d71cdde4e72a","flower.png");

        pt1.start();
        pt2.start();
        pt3.start();
    }
}

class WebDownloader{
    //下载方法
    public void downloader(String url, String name){
        try {
            FileUtils.copyURLToFile(new URL(url), new File(name));
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("IO异常，downloader方法出现问题");
        }
    }
}
```

练习结果：
    ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596007172207-78d180b6-b800-43bf-8c55-58f54ba86eb4.png#height=57&id=qYeIM&name=image.png&originHeight=113&originWidth=366&originalType=binary∶=1&size=11691&status=done&style=none&width=183)
    ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596007193368-7a5b8c99-f730-4e94-ab0f-dc29fed9a5d9.png#height=306&id=io10H&name=image.png&originHeight=611&originWidth=853&originalType=binary∶=1&size=84458&status=done&style=none&width=426.5)

## 实现 Runable 接口（推荐）

### 创建线程方式二

1. 实现 runable 接口
1. 重写 run 方法
1. 执行线程需要丢入 runable 接口的实现类,调用 start()

```java
package Thread.demo01创建线程;
/**
 * 创建线程方式二：
 *  1.实现runable接口
 *  2.重写run方法
 *  3.执行线程需要丢入runable接口的实现类,调用start()
 */
public class CreateThread2 implements Runnable{
    @Override
    public void run() {
        // run方法线程体
        for (int i = 0; i <20 ; i++) {
            System.out.println("我在看代码------"+i);
        }
    }

    public static void main(String[] args) {    // 主线程
        //创建Runable接口的实现类
        CreateThread2 thread2 = new CreateThread2();
        Thread thread = new Thread(thread2);
        // 调用start()方法
        thread.start();

        for (int i = 0; i < 200; i++) {
            System.out.println("我在学习多线程-------"+i);
        }
    }
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596008506597-8d0d158b-2a3b-420d-bc6b-96a72d389d03.png#height=444&id=VThkZ&name=image.png&originHeight=888&originWidth=1654&originalType=binary∶=1&size=611970&status=done&style=none&width=827)

### 案例：实现多个线程购**买火车票**

```java
package Thread.demo01创建线程;
/**
 * 多个线程同时操作同一个对象
 * 买火车票
 */
public class TestThread implements Runnable {
    private int ticktnumbers = 10;
    @Override
    public void run() {
        while (true){
            if(ticktnumbers <=0){
                break;
            }
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+ "拿到了" + ticktnumbers-- + "票");
        }
    }
    public static void main(String[] args) {
        TestThread testThread = new TestThread();
        new Thread(testThread,"小明").start();
        new Thread(testThread,"老师").start();
        new Thread(testThread,"黄牛").start();
    }
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596009385934-1d695b31-14ce-4448-9f05-f9c751e54aef.png#height=406&id=Tg77k&name=image.png&originHeight=510&originWidth=274&originalType=binary∶=1&size=50670&status=done&style=none&width=218)
**发现问题：**多个线程操作了同一个资源的情况下，**线程不安全了**，数据紊乱，线程并发问题，需要实现线程同步。

### 案例：龟兔赛跑

```java
package Thread.demo01创建线程;
import javax.swing.*;

public class Race implements Runnable{
    // 胜利者
    private static String winner;

    @Override
    public void run() {
        for (int i = 0; i <= 100 ; i++) {
            //模拟兔子休息
            if( Thread.currentThread().getName().equals("兔子") && i%10==0 ){
                try {
                    Thread.sleep(2);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

            boolean flag = gameOver(i);
            if(flag){   //比赛结束，停止程序
                break;
            }
            System.out.println(Thread.currentThread().getName()+"跑了：" + i +" 步");
        }
    }
    // 判断比赛是否完成
    private boolean gameOver(int steps){
        if (winner!=null){  //已经存在胜利者
            return true;
        }else{
            if(steps >= 100){
                winner = Thread.currentThread().getName();
                System.out.println("winner is: " + winner);
                return true;
            }
        }
        return  false;
    }
    public static void main(String[] args) {
        Race race = new Race();
        new Thread(race,"乌龟").start();
        new Thread(race,"兔子").start();

    }

}

```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596010331448-8f27960c-f22a-4788-88c3-3600ae642b8f.png#height=305&id=U0wfM&margin=%5Bobject%20Object%5D&name=image.png&originHeight=609&originWidth=218&originalType=binary∶=1&size=26490&status=done&style=none&width=109)

## 实现 Callable 接口

### 创建线程方式三

- 实现 Callable 接口 ，需要返回值类型
- 重写 call 方法
- 创建目标对象  `CreateThreadCallable ct1 = new CreateThreadCallable();`
- 创建执行服务： ` ExecutorService ser = Executors.newFixedThreadPool(``**3**``); // 3表示线程数 `
- 提交执行：      `Future<Boolean> result1 = ser.submit(ct1);`
- 获取结果：   ` boolean rs1 = ``result1 ``.get();`
- 关闭服务：      ` ser.shutdown();`

```java
package Thread.demo01创建线程;
import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;
import java.util.concurrent.*;

/**
 * 线程创建方式三：实现Callable接口
 *
 */
public class CreateThreadCallable implements Callable<Boolean> {
    String url;
    String name;

    public CreateThreadCallable(String url,String name){
        this.url = url;
        this.name = name;
    }
    @Override
    public Boolean call() throws Exception {
        WebDownloader2 webDownloader = new WebDownloader2();
        webDownloader.downloader(url,name);
        System.out.println("下载了文件名为："+name);
        return true;
    }

    public static void main(String[] args) {
        CreateThreadCallable ct1 = new CreateThreadCallable("https://pic.cnblogs.com/avatar/1418974/20181015113902.png","Kuang.png");
        CreateThreadCallable ct2= new CreateThreadCallable("https://t7.baidu.com/it/u=3616242789,1098670747&fm=79&app=86&size=h300&n=0&g=4n&f=jpeg?sec=1596611654&t=6e68bfae4f52559d8856b922d491307a","girl.png");
        CreateThreadCallable ct3 = new CreateThreadCallable("https://t7.baidu.com/it/u=3204887199,3790688592&fm=79&app=86&size=h300&n=0&g=4n&f=jpeg?sec=1596611654&t=58b381c7ecb89fc6eb26d71cdde4e72a","flower.png");

        //创建执行服务
        ExecutorService ser = Executors.newFixedThreadPool(3);
        // 提交执行
        Future<Boolean> r1 = ser.submit(ct1);
        Future<Boolean> r2 = ser.submit(ct2);
        Future<Boolean> r3 = ser.submit(ct3);
        //获取结果
        try {
            boolean rs1 = r1.get();
            boolean rs2 = r2.get();
            boolean rs3 = r3.get();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
        //关闭服务
        ser.shutdown();
    }
}

class WebDownloader2{
    //下载方法
    public void downloader(String url, String name){
        try {
            FileUtils.copyURLToFile(new URL(url), new File(name));
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("IO异常，downloader方法出现问题");
        }
    }
}
```

# 静态代理

## 静态代理举例

**静态代理模式：**
真实对象和代理对象都实现同一个接口
代理对象要代理真实角色
**静态代理的好处：**

1.  代理对象可以做很多真实对象做不了的事情
2.  真实对象可专注做自己的事情

```java
package Thread.demo02静态代理;
// 人间四大喜事: 久旱逢甘霖,他乡遇故知,洞房花烛夜,金榜题名时
interface Marry{
    void happyMarry();
}

// 真实角色
class You implements Marry{
    @Override
    public void happyMarry() {
        System.out.println("Nick要结婚了，超开心...");
    }
}

// 代理角色
class WeddingCompany implements Marry{
    private Marry target;

    public WeddingCompany(Marry target) {
        this.target = target;       //真实目标角色
    }

    @Override
    public void happyMarry() {
        System.out.println("结婚之前，布置现场");
        this.target.happyMarry();   //真实对象
        System.out.println("结婚之后，收尾款");
    }
}
public class StaticProxy{
    public static void main(String[] args) {
        // 真实对象掉方法
        You you = new You();
        you.happyMarry();

        //通过代理帮你调方法
        WeddingCompany weddingCompany = new WeddingCompany(new You());
        weddingCompany.happyMarry();

        // lambda表达式
        /*
        new Thread(()-> System.out.println("我爱你")).start();
        new Thread( new Runnable() {
            @Override
            public void run() {
                System.out.println("我爱你");
            }
        }).start();
        // Thread就是静态代理     Runnable是真实的对象
        new WeddingCompany(new You()).happyMarry();*/
    }
}
```

## 线程中的静态代理分析

**1.Thread 类实现了 Runable 接口，即 Thread 类相当于上文中的"婚庆公司"**
![](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596013133508-57e15579-38ba-40ac-9561-0affa2a704c7.png#height=164&id=TNgu0&originHeight=164&originWidth=824&originalType=binary∶=1&size=0&status=done&style=none&width=824)

** 2.我们写的类也是实现了 Runnable 接口，即我们写的类相当于上文中的"结婚人 You"**
![](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596013133480-9f283a88-3b2d-4673-b090-66dfc4fe36c3.png#height=188&id=r8n1K&originHeight=188&originWidth=771&originalType=binary∶=1&size=0&status=done&style=none&width=771)

**3.在实现了 Runnable 接口后通过代理类 Thread 对象完成线程的启动**
A.在代理类 Thread 对象的创建中，声明了我们所写的实际对象，eg:"myRunnable"。
B.然后由 Thread 类协助我们完成这一系列的操作。
C.看似简单的 start()背后，代理类 Thread 还帮助我们做了很多事。
​

# Lambda 表达式

**Lambda 表达式**，也可称为闭包。Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）。使用 Lambda 表达式可以使代码变的更加简洁紧凑。Lambda 表达式可以替代以前广泛使用的内部匿名类，各种回调，比如事件响应器、传入 Thread 类的 Runnable 等。
**​**

**lambda 表达式的语法格式如下：**

```
(parameters) -> expression
或
(parameters) ->{ statements; }
```

**Lambda 表达式的特征:**

- 类型声明（可选）：可以不需要声明参数类型，编译器会识别参数值。
- 参数圆括号（可选）：在单个参数时可以不使用括号，多个参数时必须使用。
- 大括号和 return 关键字（可选）：如果只有一个表达式，则可以省略大括号和 return 关键字，编译器会自动的返回值；相对的，在使用大括号的情况下，则必须指明返回值。

```java
package Thread.demo03Lambda表达式;

interface ILove{
    void love(int a);   // 接口只有一个方法，这个接口叫做函数式接口
}
class Love implements ILove{
    @Override
    public void love(int a) {
        System.out.println("I Love you： " + a);
    }
}

public class TestLambda2 {

    static  class Love2 implements ILove{
        @Override
        public void love(int a) {
            System.out.println("I Love you： " + a);
        }
    }

    public static void main(String[] args) {

        //局部内部类
        class Love3 implements ILove{
            @Override
            public void love(int a) {
                System.out.println("I Love you： " + a);
            }
        }
        //外部类实现接口
        ILove love = new Love();
        love.love(1);

        //静态内部类
        ILove love2 = new Love2();
        love.love(2);

        //局部内部类
        ILove love3 = new Love3();
        love3.love(3);

        // 匿名内部类
        ILove love4 = new ILove() {
            @Override
            public void love(int a) {
                System.out.println("I Love you： " + a);
            }
        };
        love4.love(4);

        //Lambda表达式
        ILove love5 = (int a)->{
            System.out.println("I Love you： " + a);
        };
        love5.love(5);

        //去掉参数类型
        ILove love6 = (a)->{
            System.out.println("I Love you： " + a);
        };
        love6.love(6);

        ILove love7 = a->{
            System.out.println("I Love you： " + a);
        };
        love7.love(7);

        //  lambda表达只有一行代码，还可继续简化成下面的情况
        ILove love8 = a->System.out.println("I Love you： " + a);
        love8.love(7);
    }
}
```

# 线程状态

## 线程的生命周期

一个线程的生命周期,线程是一个动态执行的过程，它也有一个从产生到死亡的过程。下图显示了一个线程完整的生命周期。
                ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596027546556-8d3fab8f-2ba4-4fb4-88f7-206f70b63f4f.png#height=426&id=OcSeJ&name=image.png&originHeight=698&originWidth=1029&originalType=binary∶=1&size=170348&status=done&style=none&width=628)

## 描述线程状态的方法

下表列出了 Thread 类的一些重要方法：

| **序号**                                                                                                 | **方法描述**                                    |
| -------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| 1                                                                                                        | **public final void setPriority(int priority)** |
| 更改线程的优先级。                                                                                       |
| 2                                                                                                        | **public static void sleep(long millisec)**     |
| 在指定的毫秒数内让当前正在执行的线程休眠（暂停执行），此操作受到系统计时器和调度程序精度和准确性的影响。 |
| 3                                                                                                        | **public final void join(long millisec)**       |
| 等待该线程终止的时间最长为  millis  毫秒。                                                               |
| 4                                                                                                        | **public static void yield()**                  |
| 暂停当前正在执行的线程对象，并执行其他线程。                                                             |
| 5                                                                                                        | **public final void setDaemon(boolean on)**     |
| 将该线程标记为守护线程或用户线程。                                                                       |
| 6                                                                                                        | **public final void join(long millisec)**       |
| 等待该线程终止的时间最长为  millis  毫秒。                                                               |
| 7                                                                                                        | **public void interrupt()**                     |
| 中断线程。                                                                                               |
| 8                                                                                                        | **public final boolean isAlive()**              |
| 测试线程是否处于活动状态。                                                                               |

### 线程停止

```java
package Thread.demo04线程状态;
/* 测试停止线程
    1. 建议线程正常停止-->利用次数，不建议死循环
    2. 建议使用标志位--->设置一个标志位
    3. 不要使用stop或者destroy等过时或者JDK不建议使用的方法
 */
public class StopTest implements Runnable{
    // 1. 设置标志位
    private boolean flag = true;
    @Override
    public void run() {
        int i = 0;
        while(flag){
            System.out.println("run.........Thread"+ i++);
        }
    }
    // 2.设置一个公开的方法停止线程，转换标志位
    public void stop(){
        this.flag = false;
    }
    public static void main(String[] args) {
        StopTest stopTest = new StopTest();
        new Thread(stopTest).start();

        for (int i = 0; i <1000; i++) {
            System.out.println("main"+i);
            if(i==900){
                stopTest.stop();
                System.out.println("线程该停止了");
            }
        }
    }
}
```

### 线程休眠

- sleep(时间）指定当前线程阻塞的毫秒数；
- sleep 存在异常 InterruptedException；
- sleep 时间达到后线程进入就绪状态；
- sleep 可以模拟网络延时；
- **每一个对象都有一个锁，sleep 不会释放锁。**

```java
package Thread.demo04线程状态;
// 模拟倒计时

import java.text.SimpleDateFormat;
import java.util.Date;

public class TestSleep2 {
    public static  void tenDown(){
        int num = 10;
        while(true){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(num--);
            if(num<0){
                break;
            }
        }
    }

    public static void main(String[] args) {
        // TestSleep2.tenDown();
        // 获取系统当前时间
        Date startTime = new Date(System.currentTimeMillis());
        while(true){
            try {
                Thread.sleep(1000);
                System.out.println(new SimpleDateFormat("HH:mm:ss").format(startTime));
                startTime = new Date(System.currentTimeMillis());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// 输出结果
21:36:41
21:36:42
21:36:43
21:36:44
21:36:45
21:36:46
21:36:47
21:36:48
21:36:49
21:36:50
21:36:51
```

### 线程礼让（yield）

```java
package Thread.demo04线程状态;
/*
    礼让不一定成功，看CPU心情
 */

class MyYield implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+ "线程开始执行。");
        Thread.yield();
        System.out.println(Thread.currentThread().getName()+"线程停止执行。");
    }
}
public class TestYield {
    public static void main(String[] args) {
        MyYield myYield = new MyYield();
        new Thread(myYield,"A").start();
        new Thread(myYield,"B").start();
    }

}

// 礼让成功
A线程开始执行。
B线程开始执行。
B线程停止执行。
A线程停止执行。

//礼让不一定成
B线程开始执行。
A线程开始执行。
B线程停止执行。
A线程停止执行。
```

### Join 合并线程

```java
package Thread.demo04线程状态;

// 线程插队
public class TestJoin implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 10 ; i++) {
            System.out.println("VIP来了....." + i);
        }
    }
    public static void main(String[] args) throws InterruptedException {
        TestJoin testJoin = new TestJoin();
        Thread thread = new Thread(testJoin);
        thread.start();

        for (int i = 0; i < 10 ; i++) {
            if(i==2){
                thread.join();	//开始插队
            }
            System.out.println("main"  + i );
        }
    }
}

/* 运行结果
	要等到插队线程运行完了之后，才运行主线程。
VIP 来了.....0
main0
VIP 来了.....1
main1
VIP 来了.....2
VIP 来了.....3
VIP 来了.....4
VIP 来了.....5
VIP 来了.....6
VIP 来了.....7
VIP 来了.....8
VIP 来了.....9
main2
main3
main4
main5
main6
main7
main8
main9

*/
```

### 线程观测

```java
package Thread.demo04线程状态;
/* 观察，测试线程的状态
	Thread.State
 */
public class TestState {
    public static void main(String[] args) throws InterruptedException {

        Thread thread = new Thread(()->{
            for (int i = 0; i < 5; i++) {
                try {
                    Thread.sleep(1000);   // 阻塞
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
            System.out.println("/////");
        });

        // 观测状态
        Thread.State state = thread.getState();
        System.out.println(state);  // New

        // 观察启动后
        thread.start();         // 启动线程
        state = thread.getState();
        System.out.println(state);  //Run

        // 只要线程不终止，就一直输出状态
        while(state != Thread.State.TERMINATED){
            Thread.sleep(100);
            state = thread.getState();      // 更新线程状态
            System.out.println(state);
        }
        // thread.start(); 线程只能启动一次，一旦中断就不能在启动了

    }
}

```

### 线程优先级

> 在操作系统中，线程可以划分优先级，Cpu 优先执行优先级高的线程的任务。在 java 中线程优先级分为 1~10，如果小于 1 或者大于 10，则 jdk 报 illegalArgumentException()异常。设置线程优先级使用 setPriority()方法。
> 1、线程优先级具有继承性。a 线程启动 b 线程，b 线程的优先级和 a 线程的优先级是一样的。
> 2、线程具有规则性。高优先级的线程总是大部分先执行完，并不是高优先级的完全先执行完。线程的优先级和执行顺序无关。线程的优先级具有一定的规则性，cpu**尽量**将执行资源让给优先级比较高的线程。

```java
package Thread.demo04线程状态;
class MyPriorxity implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"-->"+Thread.currentThread().getPriority());
    }
}
public class TestPriority {
    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName()+"-->"+Thread.currentThread().getPriority());
        MyPriorxity myPriorxity = new MyPriorxity();
        Thread t1 = new Thread(myPriorxity);
        Thread t2 = new Thread(myPriorxity);
        Thread t3 = new Thread(myPriorxity);
        Thread t4 = new Thread(myPriorxity);
        Thread t5 = new Thread(myPriorxity);
        Thread t6 = new Thread(myPriorxity);

        t1.setPriority(6);	//先设置优先级，后启动
        t1.start();
        t2.setPriority(10);
        t2.start();
        t3.setPriority(4);
        t3.start();
        t4.setPriority(5);
        t4.start();
        t5.setPriority(2);
        t5.start();
        t6.setPriority(3);
        t6.start();
    }
}

// 输出结果
main-->5
Thread-1-->10
Thread-2-->4
Thread-0-->6
Thread-3-->5
Thread-5-->3
Thread-4-->2
```

### 守护线程

> 线程分为**用户线程**和**守护线程**
> 虚拟机必须确保用户线程执行完毕
> **虚拟机不必等待守护线程执行完毕**
> 如：后台记录操作日志，监控内存，垃圾回收等待。

```java
package Thread.demo04线程状态;
// 测试守护线程


class You implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i <36500 ; i++) {
            System.out.println("你一生都开心的活着");
        }
        System.out.println("----good bye----");
    }
}

class God implements Runnable{

    @Override
    public void run() {
        while(true){
            System.out.println("上帝保佑着你");
        }
    }
}
public class TestDaemon {

    public static void main(String[] args) {
        You you = new You();
        God god = new God();
        Thread godthread = new Thread(god,"God");
        godthread.setDaemon(true);		//将上帝线程设置为守护线程
        godthread.start();
        Thread youthread = new Thread(you,"You");
        youthread.start();

    }
}

```

# 线程同步

## 队列和锁

> **理解：多个线程操作同一个资源，才会存在线程同步，也即是：并发。** > **实例：上万人同时抢票、银行取钱解决：排队、对象等待池、 队列（FIFO）+锁、synchronized 加锁带来的问题： 一个线程持有锁，会导致其它需要该锁的线程挂起 在线程竞争下，加锁会导致较多的上下文比较和调度延时，引起性能问题**

> ** 优先级高的线程等待一个优先级低的线程释放锁，会导致性能倒置。**

**​**

## 线程存在的三种不安全

### 不安全的买票

```java
package Thread.demo05线程同步;

class BuyTicket implements Runnable {
    private int ticktnumbers = 10;
    boolean flag = true; // 外部停止方式
    @Override
    public void run() {
        while (flag) {
            try {
                buy();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    public void buy() throws InterruptedException {
        if (ticktnumbers <= 0) {
            flag = false;
        }
        Thread.sleep(1000);
        System.out.println(Thread.currentThread().getName() + "买了第" + ticktnumbers-- + "张票。");
    }
}

public class UnSafeBuyTicket{
    public static void main(String[] args) {
        BuyTicket ticket = new BuyTicket();
        new Thread(ticket,"小明").start();
        new Thread(ticket,"老师").start();
        new Thread(ticket,"黄牛").start();
    }
}

//结果
小明买了第8张票。
老师买了第10张票。
黄牛买了第9张票。
小明买了第5张票。
黄牛买了第7张票。
老师买了第6张票。
老师买了第3张票。
黄牛买了第4张票。
小明买了第2张票。
老师买了第1张票。
黄牛买了第0张票。
小明买了第-1张票。
老师买了第-2张票。
Process finished with exit code 0
```

### 不安全的取钱

```java
package Thread.demo05线程同步;

import oop.demo05.A;

class Account{
    int money;  //余额
    String name;    //卡名

    public Account(int money, String name) {
        this.money = money;
        this.name = name;
    }
}

// 银行：模拟取款
class Drawer extends  Thread{
    Account account;
    int drawingMoney;
    int nowMoney;

    public Drawer(Account account, int drawingMoney, String name){
        super(name);
        this.account = account;
        this.drawingMoney = drawingMoney;
    }

    @Override
    public void run() {
        if(account.money - drawingMoney < 0){
            System.out.println(Thread.currentThread().getName()+"正在取钱不够了，取不了");
        }
        try {
            Thread.sleep(1000);     // 放大问题的发生性
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 卡内余额 = 卡里的钱 - 取出的钱
        account.money = account.money - drawingMoney;
        // 手里的钱 =  手里的钱 + 取出的钱
        nowMoney = nowMoney + drawingMoney;

        System.out.println(account.name + "余额为：" +account.money);
        System.out.println(this.getName()+"手里的钱："+nowMoney);
    }
}

public class UnSafeBank {
    public static void main(String[] args) {
        Account account = new Account(100000,"结婚基金");
        Drawer you = new Drawer(account,50000,"你");
        Drawer wife = new Drawer(account,100000,"妻子");
        you.start();
        wife.start();

    }
}
// 结果
妻子正在取钱不够了，取不了
结婚基金余额为：50000
结婚基金余额为：-50000
你手里的钱：50000
妻子手里的钱：100000
```

### 不安全的集合

```java
package Thread.demo05线程同步;

import java.util.ArrayList;
import java.util.List;

// 集合线程不安全
public class UnSafeList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(()->{
                list.add(Thread.currentThread().getName());
                //同一时间会存在线程往列表同一位置添加数据，被覆盖掉一些
            }).start();
        }
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //System.out.println(list);
        System.out.println(list.size());
    }
}
// 输出结果不满10000
9992
```

## 同步（synchronized）

### 同步方法

> public synchronized void method(int args){}  
> synchronized 方法控制对象的访问，每个对象都有一把锁，每个 synchronized 方法都必须获得调用该方法的对象的锁才能执行，否则线程会阻塞，方法一旦执行，就独占该锁，直到方法返回才释放锁，后面阻塞的线程才能获得这个锁，才能执行。
> 缺陷：若将一个方法申明为 synchronized 将会大大影响效率。

### 同步块

> synchronized(Obj){ }
> Obj 称之为 同步监视器
>
> - Obj 可以是任何对象，但是推荐使用共享资源作为同步监视器
> - 同步方法中无需指定同步监视器，因为同步方法的同步监视器就是 this，就是 this 对象本身或者 class
> - 同步监视器的执行过程
>   - 第一个线程访问、锁定同步监视器，执行其中的代码。
>   - 第二个线程访问、发现同步监视器被锁定，无法访问。
>   - 第一个线程访问完毕，解锁同步监视器。
>   - 第二个线程访问，发现同步监视器没有锁，然后锁定并访问。

### 安全买票

```java
package Thread.demo05线程同步;

class BuyTicket implements Runnable {
    private int ticktnumbers = 10;
    boolean flag = true; // 外部停止方式
    @Override
    public void run() {
        while (flag) {
            try {
                buy();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    // synchronized同步方法： 锁的是this
    public synchronized void buy() throws InterruptedException {
        if (ticktnumbers <= 0) {
            flag = false;
            return;
        }
        Thread.sleep(1000);
        System.out.println(Thread.currentThread().getName() + "买了第" + ticktnumbers-- + "张票。");
    }
}

public class UnSafeBuyTicket{
    public static void main(String[] args) {
        BuyTicket ticket = new BuyTicket();
        new Thread(ticket,"小明").start();
        new Thread(ticket,"老师").start();
        new Thread(ticket,"黄牛").start();
    }
}
//结果
小明买了第10张票。
黄牛买了第9张票。
老师买了第8张票。
小明买了第7张票。
黄牛买了第6张票。
老师买了第5张票。
小明买了第4张票。
黄牛买了第3张票。
老师买了第2张票。
老师买了第1张票。

Process finished with exit code 0

```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596079989659-b960e55d-5b55-4bee-a647-757b085fc01f.png#height=241&id=W1wCG&name=image.png&originHeight=482&originWidth=1178&originalType=binary∶=1&size=30266&status=done&style=none&width=589)

### 安全的银行

```java
package Thread.demo05线程同步;

import oop.demo05.A;

class Account{
    int money;  //余额
    String name;    //卡名

    public Account(int money, String name) {
        this.money = money;
        this.name = name;
    }
}

// 银行：模拟取款
class Drawer extends  Thread{
    Account account;
    int drawingMoney;
    int nowMoney;

    public Drawer(Account account, int drawingMoney, String name){
        super(name);
        this.account = account;
        this.drawingMoney = drawingMoney;
    }

    @Override
    public  void run() {

        // 取钱的方法run加synchronized锁的是this,此处即为Drawer的实例对象
        //所以不能在run上加synchronized，而应该给同步资源account加锁，因为增删改的对象是account
        synchronized (account){
            if(account.money - drawingMoney < 0){
                System.out.println(Thread.currentThread().getName()+"正在取钱不够了，取不了");
                return ;
            }
            try {
                Thread.sleep(1000);     // 放大问题的发生性
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            // 卡内余额 = 卡里的钱 - 取出的钱
            account.money = account.money - drawingMoney;
            // 手里的钱 =  手里的钱 + 取出的钱
            nowMoney = nowMoney + drawingMoney;

            System.out.println(account.name + "余额为：" +account.money);
            System.out.println(this.getName()+"手里的钱："+nowMoney);
        }
    }
}

public class UnSafeBank {
    public static void main(String[] args) {
        Account account = new Account(200000,"结婚基金");
        Drawer you = new Drawer(account,50000,"你");
        Drawer wife = new Drawer(account,100000,"妻子");
        you.start();
        wife.start();

    }
}

//
结婚基金余额为：150000
你手里的钱：50000
结婚基金余额为：50000
妻子手里的钱：100000

```

### 安全的列表

```java
package Thread.demo05线程同步;

import java.util.ArrayList;
import java.util.List;

// 集合线程不安全
public class UnSafeList {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(()->{
                synchronized (list){
                    list.add(Thread.currentThread().getName());
                    //同一时间存在线程往列表同一位置添加数据，被覆盖掉一些
                }
            }).start();
        }
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //System.out.println(list);
        System.out.println(list.size());
    }
}
//输出结果
10000
```

# 死锁

**死锁：**多个线程互相抱着对方需要的资源，然后形成僵持。

## 产生死锁的原因

（1） 因为系统资源不足。
（2） 进程运行推进的顺序不合适。
（3） 资源分配不当等。
如果系统资源充足，进程的资源请求都能够得到满足，死锁出现的可能性就很低，否则就会因争夺有限的资源而陷入死锁。其次，进程运行推进顺序与速度不同，也可能产生死锁。

## 产生死锁的四个必要条件

（1）  互斥条件：一个资源每次只能被一个进程使用。
（2）  请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
（3）  不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。
（4）  循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。
这四个条件是**死锁的必要条件**，只要系统发生死锁，这些条件必然成立，而只要上述条件之一不满足，就不会发生死锁。

## 死锁的解除与预防

理解了死锁的原因，尤其是产生死锁的四个必要条件，就可以最大可能地避免、预防和解除死锁。所以，在系统设计、进程调度等方面注意如何不让这四个必要条件成立，如何确定资源的合理分配算法，避免进程永久占据系统资源。此外，也要防止进程在处于等待状态的情况下占用资源。因此，对资源的分配要给予合理的规划。

## 死锁实例

```java
package Thread.demo06死锁;

import com.sun.jdi.event.ThreadStartEvent;

//口红
class Lipstick{

}

//镜子
class  Mirror{

}

//化妆
class Makeup extends Thread{
    static Lipstick lipstick = new Lipstick();
    static Mirror mirror = new Mirror();
    int choice;
    String girlName;

    public Makeup(int choice,String girlName){
        this.choice = choice;
        this.girlName = girlName;
    }
    //化妆： 互相持有对方的锁
    private void makeup() throws InterruptedException {
       if(choice==1){
           synchronized (lipstick){
               System.out.println(this.girlName+"获得口红的锁。");
               Thread.sleep(1000);
               synchronized (mirror){
                   System.out.println(this.girlName+"1s钟后获得镜子的锁。");
               }
           }
       }else{
           synchronized (mirror){
               System.out.println(this.girlName+"获得镜子的锁。");
               Thread.sleep(2000);
               synchronized (lipstick){
                   System.out.println(this.girlName+"1s钟后获得口红的锁。");
               }
           }
       }
    }

    @Override
    public void run() {
        //化妆
        try {
            makeup();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class DeadLock {
    public static void main(String[] args) {
        Makeup g1 = new Makeup(1,"灰姑凉");
        Makeup g2= new Makeup(2,"白雪公主");
        g1.start();
        g2.start();
    }
}

```

**出现死锁：**

#         ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596086802294-4946dc86-1f5e-4603-9f37-985822dc6ca1.png#height=75&id=xdfWD&name=image.png&originHeight=88&originWidth=312&originalType=binary∶=1&size=7931&status=done&style=none&width=267)

**解决：不让锁中抱对方的锁**

```java

    private void makeup() throws InterruptedException {
       if(choice==1){
           synchronized (lipstick){
               System.out.println(this.girlName+"获得口红的锁。");
               Thread.sleep(1000);
           }
           synchronized (mirror){		//将对方的锁拿出来
                   System.out.println(this.girlName+"1s钟后获得镜子的锁。");
           }
       }else{
           synchronized (mirror){
               System.out.println(this.girlName+"获得镜子的锁。");
               Thread.sleep(2000);
           }
           synchronized (lipstick){	 //将对方的锁拿出来
                   System.out.println(this.girlName+"1s钟后获得口红的锁。");
           }
       }
    }
```

**解除死锁：**
\*\*            \*\*![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596086973726-87680d19-ea7f-4a36-9fa9-3529b71e8bf6.png#height=153&id=S12TD&name=image.png&originHeight=188&originWidth=354&originalType=binary∶=1&size=20690&status=done&style=none&width=289)

# Lock 锁

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596088604964-bcd04ca7-1989-4228-847f-a80a3c69a109.png#height=666&id=K3p1P&name=image.png&originHeight=666&originWidth=1528&originalType=binary∶=1&size=575624&status=done&style=none&width=1528)

```java
package Thread.demo07Lock锁;

import java.util.concurrent.locks.ReentrantLock;

//测试Lock锁
class TestLooK2 implements Runnable{
    private int ticktnumbers = 10;
    boolean flag = true; // 外部停止方式

    // 定义lock锁：显示的锁
    // ReentrantLock 可重入锁
    private ReentrantLock lock = new ReentrantLock();
    @Override
    public void run() {
        while (flag) {
            try {
                buy();
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    public  void buy(){
        try{
            lock.lock();    //加锁
            if (ticktnumbers <= 0) {
                flag = false;
                return;
            }
            System.out.println(Thread.currentThread().getName() + "买了第" + ticktnumbers-- + "张票。");
        }finally {
            lock.unlock();  //解锁
        }
    }
}
public class TestLock {
    public static void main(String[] args) {
        TestLooK2 testLooK2 = new TestLooK2();
        new Thread(testLooK2).start();
        new Thread(testLooK2).start();
        new Thread(testLooK2).start();
    }
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596089471599-41cabf93-7799-481d-a8c5-0a5f3ac83937.png#height=396&id=g3rhx&name=image.png&originHeight=791&originWidth=1561&originalType=binary∶=1&size=342311&status=done&style=none&width=780.5)

# 线程通信

## 线程通信的方法

> - wait() 表示线程一直等待，直到其他线程通知，与 sleep 不同，会释放锁
> - wait(long timeout) 指定等待的毫秒数
> - notify() 唤醒一个处于等待状态的线程
> - notifyAll() 唤醒同一个对象上所有调用 wait()方法的线程，优先级别高的线程先调度

**注：上述方法都是 Object 类的方法，都只能在同步方法或者同步代码块中使用，否则会抛出 IIIegalMonitorSateException 异常**

## 生产者消费者问题

生产者消费者问题（英语：Producer-consumer problem），是一个多线程同步问题的经典案例。该问题描述了两个共享固定大小缓冲区的线程——即所谓的“生产者”和“消费者”，在实际运行时会发生的问题。生产者的主要作用是生成一定量的数据放到缓冲区中，然后重复此过程。与此同时，消费者也在缓冲区消耗这些数据。该问题的关键就是要保证生产者不会在缓冲区满时加入数据，消费者也不会在缓冲区中空时消耗数据。

### 解决方式 1-管程法

生产者消费者模型是一个并发协作的模型：
**生产者：**负责生产数据的模块（可能是方法、对象、线程、进程）
**消费者：**负责处理数据的模块（可能是方法、对象、线程、进程）
**缓冲区（仓库）：**消费者和生产者之间通信的桥梁，生产者将生产好的产品放入缓冲区，消费者从缓冲区中取出产品。
        ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596089765275-fe93afd4-3613-4808-8d5b-359c7e3eea0d.png#height=306&id=gYKb9&name=image.png&originHeight=612&originWidth=1188&originalType=binary∶=1&size=68668&status=done&style=stroke&width=594)

```java
package Thread.demo08线程通信;
//测试：生产者消费者模型-->管程法
//生产者、消费者、缓冲区、产品
class Producer extends Thread{
    SynContainer container;

    public Producer(SynContainer container){
        this.container = container;
    }

    //生产
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            container.push(new Product(i));
            System.out.println("生产了"+i+"个产品。");
        }
    }
}
class Consumer extends Thread{
    SynContainer container;

    public Consumer(SynContainer container){
        this.container = container;
    }

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("消费了--->"+container.pop().id+"个产品。");
        }
    }
}
class Product{
    int id;
    public Product(int id) {
        this.id = id;
    }
}
// 缓冲区
class SynContainer{

    Product[] products = new Product[10];//需要容器大小
    int count = 0;    //容器计数器

    public synchronized void push(Product product){//生产者放入产品
        if(count==products.length){ //如果缓冲区满了，就需要等待消费者消费
            try {
                this.wait();//生产者等待下一次生产
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        //如果没有满，就继续生产产品
        products[count] = product;
        count++;
        this.notifyAll();// 可以通知消费者消费了
    }

    //消费者消费产品
    public synchronized Product pop(){
        if(count==0){
            //等待生产者生产，消费者等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        count--;
        Product product = products[count];
        //消费了之后，就可以通知生产者继续生产
        this.notifyAll();
        return product;
    }
}

public class TestPC {
    public static void main(String[] args) {
        SynContainer container = new SynContainer();
        new Producer(container).start();
        new Consumer(container).start();
    }
}
```

### 解决方式 2-信号灯法

```java
package Thread.demo08线程通信;

//测试：生产者消费者模型-->标志法

public class TestPC2 {
    public static void main(String[] args) {
        TV tv = new TV();
        new Player(tv).start();
        new Watcher(tv).start();
    }

}

//生产者--演员
class Player extends Thread{
    TV tv;

    public Player(TV tv) {
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            if(i%2==0){
                this.tv.play("快乐大本营");
            }else{
                this.tv.play("抖音纪录片");
            }
        }
    }
}
//消费者-观众
class Watcher extends Thread{
    TV tv;

    public Watcher(TV tv) {
        this.tv = tv;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            this.tv.watch();
        }
    }
}
//产品--节目
class TV{
    String voice;
    boolean flag = true;
    //表演
    public synchronized void play(String voice){
        if(!flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("演员表演了："+ voice);
        this.notifyAll();       //通知观众观看
        this.voice = voice;
        this.flag = !this.flag;
    }

    //观看
    public synchronized void watch() {
        if(flag){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("观众观看了："+voice);
        this.notifyAll();
        this.flag = !this.flag;
    }

}
//
演员表演了：快乐大本营
观众观看了：快乐大本营
演员表演了：抖音纪录片
观众观看了：抖音纪录片
演员表演了：快乐大本营
观众观看了：快乐大本营
演员表演了：抖音纪录片
观众观看了：抖音纪录片
演员表演了：快乐大本营
观众观看了：快乐大本营
演员表演了：抖音纪录片
观众观看了：抖音纪录片
演员表演了：快乐大本营
观众观看了：快乐大本营
演员表演了：抖音纪录片
观众观看了：抖音纪录片
演员表演了：快乐大本营
观众观看了：快乐大本营
演员表演了：抖音纪录片
观众观看了：抖音纪录片
Process finished with exit code 0
```

# 线程池

## ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596093590281-6d82df1b-1396-4a89-bf60-532b62968536.png#height=393&id=eKKty&name=image.png&originHeight=785&originWidth=1607&originalType=binary∶=1&size=567245&status=done&style=stroke&width=803.5)

```java
package Thread.demo09线程池;


import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

//测试线程池
public class TestPool {
    public static void main(String[] args) {
        // 1.创建服务，创建线程池
        // newFixedThreadPool(10); 参数是线程池的大小
        ExecutorService service = Executors.newFixedThreadPool(10);

        // 2. 执行
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());
        service.execute(new MyThread());

        //3. 关闭连接
        service.shutdown();
    }
}

class MyThread implements Runnable{

    @Override
    public void run() {
      System.out.println(Thread.currentThread().getName());
    }
}
// 输出
pool-1-thread-1
pool-1-thread-2
pool-1-thread-3
pool-1-thread-5
pool-1-thread-4
```

# 总结

![](https://cdn.nlark.com/yuque/0/2020/png/1281683/1601637738635-5b14133e-5fdb-490f-9a4f-9acd710e2445.png)
