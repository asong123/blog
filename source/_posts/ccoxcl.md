---
title: 13、JavaSE：Gui编程
urlname: ccoxcl
date: '2021-07-09 20:38:03 +0800'
tags: []
categories: []
---

GUI 编程
Swing 和 AWT 是 java 开发 GUI 常用的技术 . 但是由于外观不太美观, 组件数量偏少, 并且运行需要 JRE 环境
(动不动就上百 M 的 JRE 包 ), 所以没有流行起来. 但是 ,建议学还是需要简单的学习和了解的.

1. 组件( JTable,JList 等)很多都是 MVC 的经典示范. 学习也可以了解 mvc 架构的
1. 工作时,也有可能遇见需要维护 N 年前 awt/swing 写的软件 ,虽然可能性很小
1. 可以写一些自己使用用的软件. 还是相当的方便.

艺多不压身
**学习了 swing 还有必要学习 awt 吗？**
swing 是建立在 awt 基础上的。还是有必要学习一下的.原因如下:
:知识的关联性 . 比如 布局 , 颜色, 字体,事件机制 等 这些都是 awt 里的内容. 但在 swing 里也经常使
用到.
学习成本低, 因为 awt 和 swing 在编码上区别不大, 写法基本一致, 组件使用上也差不多,(只需要记住少数有区别的地方就可以了)
使用场景存在不同. awt 消耗资源少, 运行速度快. 适合嵌入式等. swing 跨平台,组件丰富.
虽然现在用 Java 做 cs 的很少，但是对于我们学习 Java 基础来说，我觉得这个还是很好的资源，我们可以 利用它把以前的所有知识贯穿起来，做一些小应用，游戏，等都可以，可以将自己的一些小想法，做成 工具分享出来！
**AWT**

**一、AWT 介绍**

AWT（Abstract Window Toolkit）包括了很多类和接口，用于 Java Application 的 GUI（Graphics User Interface 图形用户界面）编程。
GUI 的各种元素（如：窗口，按钮，文本框等）由 Java 类来实现。 使用 AWT 所涉及的类一般在 Java.AWT 包及其子包中。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834287929-6215165d-5fe4-4abe-a432-c561e64a9be6.jpeg#)Container 和 Component 是 AWT 中的两个核心类。
所有的可以显示出来的图形元素都称为 Component，Component 代表了所有的可见的图形元素，
Component 里面有一种比较特殊的图形元素叫 Container，Container(容器)在图形界面里面是一种可以 容纳其它 Component 元素的一种容器，Container 本身也是一种 Component 的，Container 里面也可以容纳别的 Container。
Container 里面又分为 Window 和 Pannel，Window 是可以独立显示出来的，平时我们看到的各种各样的 应用程序的窗口都可以称为 Window，Window 作为一个应用程序窗口独立显示出来，Pannel 也可以容纳其它的图形元素，但一般看不见 Pannel，Pannel 不能作为应用程序的独立窗口显示出来，Pannel 要想 显示出来就必须得把自己装入到 Window 里面才能显示出来。
Pannel 应用比较典型的就是 Applet( JAVA 的页面小应用程序)，现在基本上已经不用了，A JAX 和
JAVASCRIPT 完全取代了它的应用。
Window 本身又可以分为 Frame 和 Dialog，Frame 就是我们平时看到的一般的窗口，而 Dialog 则是那 些需要用户进行了某些操作(如点击某个下拉菜单的项)才出现的对话框，这种对话框就是 Dialog。

**二、组件和容器(Component 和 Container)**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834288508-bab78a02-c6ea-47bf-8687-9c4f82471d47.jpeg#)

1.  **Frame**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834288881-5b76eeac-2748-4810-b0f3-b327191eecf8.jpeg#)
【Frame 范例】

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
34
35
package com.kuang;
import java.awt.\*;
//学习 JAVA 的 GUI 编程编写的第一个图形界面窗口
public class TestFrame {
public static void main(String[] args) {
//这里只是在内存里面创建了一个窗口对象 还不能真正显示出来然我们看到
Frame frame = new Frame("我的第一个 JAVA 图形界面窗口");

//设置窗体的背景颜色 frame.setBackground(Color.blue);

//设置窗体是否可见
//要想看到在内存里面创建出来的窗口对象
//必须调用 setVisble()方法
//并且把参数 true 传入才能看得见窗体
//如果传入的参数是 false
//那么窗体也是看不见的 frame.setVisible(true);

//设置窗体的初始大小 frame.setSize(400,400);

//设置窗体出现时的位置，如果不设置则默认在左上角(0,0)位置显示 frame.setLocation(200,200);

// 设置窗体能否被改变大小
// 设置为 false 后表示不能改变窗体的显示大小
// 这里将窗体显示的大小设置为 200X200
// 那么窗体的显示只能是这个大小了，不能再使用鼠标拖大或者缩小
frame.setResizable(false);
}
}

运行结果：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834289134-61256c58-f079-4059-93cf-c64ca87a4d90.png#)

【发现问题：关闭不掉，解决方法：停止 Java 程序的运行】
【演示二：展示多个窗口】

1 package com.kuang; 2
3 import java.awt.\*; 4

1. public class TestMultiFrame {
1. public static void main(String[] args) { 7
1. MyFrame f1 = new MyFrame(100,100,200,200,Color.blue);
1. MyFrame f2 = new MyFrame(300,100,200,200,Color.yellow);
1. MyFrame f3 = new MyFrame(100,300,200,200,Color.red);
1. MyFrame f4 = new MyFrame(300,300,200,200,Color.MAGENTA); 12

13 }
14 }
15

1. //自定义一个类 MyFrame，并且从 Frame 类继承
1. //这样 MyFrame 类就拥有了 Frame 类的一切属性和方法
1. //并且 MyFrame 类还可以自定义属性和方法
1. //因此使用从 Frame 类继承而来的自定义类来创建图形窗口比直接使用 Frame 类来创建图形窗口要灵活
1. //所以一般使用从 Frame 类继承而来的自定义类创建图形窗口界面比较好，
1. //不推荐直接使用 Frame 类来创建图形窗口界面
1. class MyFrame extends Frame{ 23
1. //定义一个静态成员变量 id，用来记录创建出来的窗口的数目
1. static int id = 0; 26
1. //自定义构成方法，在构造方法体内使用 super 调用父类 Frame 的构造方法
1. public MyFrame(int x,int y,int w,int h,Color color){
1. super("MyFrame"+(++id));
1. /_使用从父类 Frame 继承而来的方法设置窗体的相关属性_/
   | 31 | | | setBackground(color); |
   | --- | --- | --- | --- |
   | 32 | | | setLayout(null); |
   | 33 | | | setBounds(x,y,w,h); |
   | 34 | | | setVisible(true); |
   | 35 | | } | |
   | 36 | | | |
   | 37 | } | | |

运行结果：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834289592-9e6764b5-6b58-459e-80de-bd33231d5313.png#)

1.  **Panel**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834290117-7056533e-85d4-4fdb-9631-b8e422aa2618.jpeg#)
【演示】

1 package com.kuang;

| 2   |        |                                |
| --- | ------ | ------------------------------ |
| 3   | import | java.awt.\*;                   |
| 4   | import | java.awt.event.WindowEvent;    |
| 5   | import | java.awt.event.WindowListener; |
| 6   |        |                                |
| 7   | public | class TestPanel {              |

| 8
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
51
52
53
54
55
56
57
58
59 | public static void main(String[] args) {
Frame frame = new Frame("JAVA Frame With Panel"); Panel panel = new Panel(null); frame.setLayout(null);

//这里设置的坐标(300,300)是相对于整个屏幕的 frame.setBounds(300,300,500,500);

//设置背景颜色时使用三基色(红，绿，蓝)的比例来调配背景色 frame.setBackground(new Color(0,0,102));

//这里设置的坐标(50,50)是相对于 Frame 窗体的 panel.setBounds(50,50,400,400); panel.setBackground(new Color(204,204,255));

//把 Panel 容器装入到 Frame 容器中，使其能在 Frame 窗口中显示出来 frame.add(panel);

frame.setVisible(true);

//解决关闭问题
frame.addWindowListener(new WindowListener() { @Override
public void windowOpened(WindowEvent e) {

}

@Override
public void windowClosing(WindowEvent e) { System.exit(0);
}

@Override
public void windowClosed(WindowEvent e) {

}

@Override
public void windowIconified(WindowEvent e) {

}

@Override
public void windowDeiconified(WindowEvent e) {

}

@Override
public void windowActivated(WindowEvent e) {

} | |

| 60 |

} |

} |

}); | @Override
public void windowDeactivated(WindowEvent e) {

| }   |
| --- | --- | --- | --- | --- |
| 61  |     |     |     |     |
| 62  |     |     |     |     |
| 63  |     |     |     |     |
| 64  |     |     |     |     |
| 65  |     |     |     |     |
| 66  |     |     |     |     |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834290601-600b88c4-50ea-4dbe-b08d-827ff6a6adc7.png#)结果如下：

**三、布局管理器**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834291091-fa49600a-ff2e-439e-8ff6-264f287e8e7b.jpeg#)

1.  ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834291671-015e9691-365d-4f3f-9be5-118ca4c4ef0c.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834292128-6160ebe0-3b8c-422f-b333-c92228bf96cd.jpeg#)**第一种布局管理器——FlowLayout**

【演示】

1
2
3
4
5
6
package com.kuang;
import java.awt.\*;

public class TestFlowLayout {
public static void main(String[] args) {

| 7   |     |     | Frame frame = new Frame("FlowLayout");                                   |
| --- | --- | --- | ------------------------------------------------------------------------ |
| 8   |     |     |                                                                          |
| 9   |     |     | //使用 Button 类创建按钮                                                 |
| 10  |     |     | // 按钮类的其中一个构造方法：Button(String label) label 为按钮显示的文本 |
| 11  |     |     | Button button1 = new Button("button1");                                  |
| 12  |     |     | Button button2 = new Button("button2");                                  |
| 13  |     |     | Button button3 = new Button("button3");                                  |
| 14  |     |     |                                                                          |
| 15  |     |     | // setLayout 方法的定义：public void setLayout(LayoutManager mgr)        |
| 16  |     |     | // 使用流水(Flow)线般的布局                                              |
| 17  |     |     | frame.setLayout(new FlowLayout());                                       |
| 18  |     |     | // 使用了布局管理器 FlowLayout，这里的布局采用默认的水平居中模式         |
| 19  |     |     |                                                                          |
| 20  |     |     | // frame.setLayout(new FlowLayout(FlowLayout.LEFT));                     |
| 21  |     |     | // 这里在布局的时候使用了 FlowLayout.LEFT 常量，这样就将按钮设置为左对齐 |
| 22  |     |     |                                                                          |
| 23  |     |     | // frame.setLayout(new FlowLayout(FlowLayout.RIGHT));                    |
| 24  |     |     | //这里在布局的时候使用了 FlowLayout.RIGHT 常量，这样就将按钮设置为右对齐 |
| 25  |     |     |                                                                          |
| 26  |     |     |                                                                          |
| 27  |     |     | frame.setSize(200,200);                                                  |
| 28  |     |     |                                                                          |
| 29  |     |     | frame.add(button1); // 把创建出来的按钮放置到 Frame 窗体中               |
| 30  |     |     | frame.add(button2); // 这里并没有设置按钮的大小与位置                    |
| 31  |     |     | frame.add(button3); // 设置按钮的大小与位置都是由布局管理器来做的        |
| 32  |     |     |                                                                          |
| 33  |     |     | frame.setVisible(true);                                                  |
| 34  |     |     |                                                                          |
| 35  |     | }   |                                                                          |
| 36  | }   |     |                                                                          |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834292645-ae4c1527-16a5-43bb-8b12-6bd1ff97cbe5.png#)运行结果：

1.  **第二种布局管理器——BorderLayout**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834292920-0512ff7d-8273-4544-b78d-68d92cb3322b.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834293234-8469a09f-a414-4be5-b81d-48dc1285d9b5.jpeg#)
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
package com.kuang;
import java.awt.\*;

public class TestBorderLayout {
public static void main(String[] args) {
Frame frame = new Frame("TestBorderLayout");

Button buttonEast = new Button("East"); Button buttonWest = new Button("West"); Button buttonSouth = new Button("South"); Button buttonNorth = new Button("North"); Button buttonCenter = new Button("Center");

//把按钮放置到 Frame 窗体时按照东西南北中五个方向排列好,推荐使用这种方式去排列窗体
元素
16
17
18
19
20
21
//这样容易检查出错误 因为这样写如果写错了编译器会提示出错
frame.add(buttonEast,BorderLayout.EAST); frame.add(buttonWest,BorderLayout.WEST); frame.add(buttonSouth,BorderLayout.SOUTH);
frame.add(buttonNorth,BorderLayout.NORTH);

22
23
24
frame.add(buttonCenter,BorderLayout.CENTER);
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
36
37
38
39
//也可以使用这样的方式排列按钮 在把按钮放置到 Frame 窗体时使用方向定位的字符串指定
按钮的放置位置
//这种使用方向定位的字符串指定按钮的放置方式不推荐使用 一旦写错了方向字符串就不好
检查出来
//因为即使是写错了仍然可以编译通过
/_ frame.add(buttonEast,"EAST"); frame.add(buttonWest,"West"); frame.add(buttonSouth,"South"); frame.add(buttonNorth,"North");
frame.add(buttonCenter,"Center");
_/

frame.setSize(200,200); frame.setVisible(true);

}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834293671-3726565b-a09a-4af2-a091-7522b8607b20.png#)运行结果：

1.  ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834294161-f8b3fbfb-cdea-4053-bacf-6228426e1685.jpeg#)**第三种布局管理器——GridLayout（表格布局管理器）**

【演示】

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
package com.kuang;
import java.awt.\*;

public class TestGridLayout {
public static void main(String[] args) { Frame frame = new Frame("TestGridLayout");

Button btn1 = new Button("btn1"); Button btn2 = new Button("btn2"); Button btn3 = new Button("btn3"); Button btn4 = new Button("btn4"); Button btn5 = new Button("btn5"); Button btn6 = new Button("bnt6");

// 把布局划分成 3 行 2 列的表格布局形式
frame.setLayout(new GridLayout(3,2));

frame.add(btn1); frame.add(btn2); frame.add(btn3); frame.add(btn4); frame.add(btn5); frame.add(btn6);

// Frame.pack()是 JAVA 语言的一个函数
// 这个函数的作用就是根据窗口里面的布局及组件的 preferredSize 来确定 frame 的最佳
大小。
28
29
30
31
32
frame.pack();
frame.setVisible(true);
}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834294441-b236016d-536e-4b55-8326-7187577cdb38.png#)运行结果：

1.  **布局练习**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834294758-8b6ab8e2-fe7b-4576-91e7-b2f323162664.jpeg#)
这几种布局管理器可以设置在 Frame 里面，也可以设置在 Panel 里面，而 Panel 本身也可以加入到 Frame 里面，因此通过 Frame 与 Panel 的嵌套就可以实现比较复杂的布局；
【演示】

1 package com.kuang; 2
3 import java.awt.\*; 4

1. public class TestTenButtons {
1. public static void main(String[] args) {
1. //这里主要是对显示窗体进行设置
1. Frame frame = new Frame("布局管理器的嵌套使用"); 9
1. //把整个窗体分成 2 行 1 列的表格布局
1. frame.setLayout(new GridLayout(2,1)); 12
1. frame.setLocation(300,400);
1. frame.setSize(400,300);
1. frame.setVisible(true);
1. frame.setBackground(new Color(204,204,255)); 17
1. //这里主要是对 Panel 进行布局的设置
1. Panel p1 = new Panel(new BorderLayout());
1. //p2 使用 2 行 1 列的表格布局
1. Panel p2 = new Panel(new GridLayout(2,1));
1. Panel p3 = new Panel(new BorderLayout());
1. //p4 使用 2 行 2 列的表格布局
1. Panel p4 = new Panel(new GridLayout(2,2)); 25
1. //这里主要是把按钮元素加入到 Panel 里面
1. p1.add(new Button("East(p1-东)"),BorderLayout.EAST);
1. p1.add(new Button("West(p1-西)"),BorderLayout.WEST); 29
1. p2.add(new Button("p2-Button1"));
1. p2.add(new Button("p2-Button2")); 32
1. //p1 里面嵌套 p2，把 p2 里面的按钮作为 p 的中间部分装入到 p1 里面
1. //把 p2 作为元素加入到 p1 里面
1. p1.add(p2,BorderLayout.CENTER); 36
1. p3.add(new Button("East(p3-东)"),BorderLayout.EAST);
1. p3.add(new Button("West(p3-西)"),BorderLayout.WEST);
   | 39 |

} |

} |
for(int i=0;i<4;i++){
p4.add(new Button("p4-Button"+i));
}

//p3 里面嵌套 p4，把 p4 里面的按钮作为 p 的中间部分装入到 p3 里面 p3.add(p4,BorderLayout.CENTER);

//把 Panel 装入 Frame 里面,以便于在 Frame 窗体中显示出来 frame.add(p1);
frame.add(p3); |
| --- | --- | --- | --- |
| 40 | | | |
| 41 | | | |
| 42 | | | |
| 43 | | | |
| 44 | | | |
| 45 | | | |
| 46 | | | |
| 47 | | | |
| 48 | | | |
| 49 | | | |
| 50 | | | |
| 51 | | | |
| 52 | | | |

运行结果 :
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834295262-f4128352-4eed-440c-8d23-3d5e3f70dab3.png#)

**四、布局管理器总结**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834295770-ded94458-0839-47c7-92b0-e7c9b85d2d70.jpeg#)

**五、事件监听**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834296326-d8115ea3-6606-4463-b570-69e065276140.jpeg#)

【**测试代码一**】

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
34
35
36
37
38
39
package com.kuang;
import java.awt._;
import java.awt.event._;
public class TestActionEvent {
public static void main(String[] args) {
Frame frame = new Frame("TestActionEvent");
Button button = new Button("Press Me");
// 创建一个监听对象
MyActionListener listener = new MyActionListener();

// 把监听加入到按钮里面，监听按钮的动作，
// 当按钮触发打击事件时，就会返回一个监听对象 e
// 然后就会自动执行 actionPerformed 方法
button.addActionListener(listener);

frame.add(button, BorderLayout.CENTER); frame.pack();

addWindowClosingEvent(frame);

frame.setVisible(true);
}
//点击窗体上的关闭按钮关闭窗体

private static void addWindowClosingEvent(Frame frame){ frame.addWindowListener(new WindowAdapter() {
@Override
public void windowClosing(WindowEvent e) { System.exit(0);
}
});
}

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
51
}
// 自定义 Monitor(监听)类实现事件监听接口 ActionListener
// 一个类要想成为监听类，那么必须实现 ActionListener 接口
class MyActionListener implements ActionListener{
//重写 ActionListener 接口里面的 actionPerformed(ActionEvent e)方法
@Override
public void actionPerformed(ActionEvent e) { System.out.println("A Button has been Pressed");
}
}

【**测试代码二**】

1 package com.kuang; 2

1. import java.awt.\*;
1. import java.awt.event.ActionEvent;
1. import java.awt.event.ActionListener; 6
1. public class TestActionEvent2 {
1. public static void main(String[] args) {
1. Frame frame = new Frame("TestActionEvent");
1. Button btn1 = new Button("start");
1. Button btn2 = new Button("stop"); 12
1. //创建监听对象
1. MyMonitor monitor = new MyMonitor(); 15
1. //一个监听对象同时监听两个按钮的动作
1. btn1.addActionListener(monitor);
1. btn2.addActionListener(monitor); 19
1. //设置 btn2 的执行单击命令后的返回信息
1. btn2.setActionCommand("GameOver"); 22
1. frame.add(btn1,BorderLayout.NORTH);
1. frame.add(btn2,BorderLayout.CENTER); 25
1. frame.pack();
1. frame.setVisible(true); 28

29 }
30 }
31
32 class MyMonitor implements ActionListener{ 33

1.  @Override
1.  public void actionPerformed(ActionEvent e) {
1.      //使用返回的监听对象e调用getActionCommand()方法获取两个按钮执行单击命令后的返  回信息
1.  //根据返回信息的不同区分开当前操作的是哪一个按钮,btn1 没有使用

setActionCommand()方法设置

1.  //则 btn1 返回的信息就是按钮上显示的文本
1.      System.out.println("a button has been pressed,"+"the relative info is:\n"

40 +e.getActionCommand());

41 }
42 }

# 六、TextField 事件监听

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834296872-cfd8cc3f-5d12-421f-b703-d84f9fa761c2.jpeg#)
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
package com.kuang;
import java.awt.\*;
import java.awt.event.ActionEvent; import java.awt.event.ActionListener;

public class TestTextField {
public static void main(String[] args) { new MyFrameTextField();
}
}
class MyFrameTextField extends Frame{ MyFrameTextField(){
TextField textField = new TextField();
add(textField); textField.addActionListener(new MyMonitor2());
//这个 setEchoChar()方法是设置文本框输入时显示的字符，这里设置为*，
//这样输入任何内容就都以*显示出来，不过打印出来时依然可以看到输入的内容 textField.setEchoChar('\*');
setVisible(true);
pack();
}
}
class MyMonitor2 implements ActionListener{
31
32
33
34
35
36
//接口里面的所有方法都是 public(公共的)
//所以从 API 文档复制 void actionPerformed(ActionEvent e)时 要在 void 前面加上
public
@Override
public void actionPerformed(ActionEvent e) {
//事件的相关信息都封装在了对象 e 里面，通过对象 e 的相关方法就可以获取事件的相关信息
//getSource()方法是拿到事件源，注意：拿到这个事件源的时候
//是把它当作 TextField 的父类来对待

| 37 |
对象

} |

} | //getSource()方法的定义是：“public Object getSource()”返回值是一个 Object

//所以要强制转换成 TextField 类型的对象
//在一个类里面想访问另外一个类的事件源对象可以通过 getSource()方法 TextField textField = (TextField) e.getSource();

// textField.getText()是取得文本框里面的内容 System.out.println(textField.getText());

// 把文本框里面的内容清空
textField.setText(""); |
| --- | --- | --- | --- |
|
38 | | | |
| 39 | | | |
| 40 | | | |
| 41 | | | |
| 42 | | | |
| 43 | | | |
| 44 | | | |
| 45 | | | |
| 46 | | | |
| 47 | | | |
| 48 | | | |

## 【使用 TextField 类实现简单的计算器】

1 package com.kuang2; 2

1. import java.awt.\*;
1. import java.awt.event.ActionEvent;
1. import java.awt.event.ActionListener; 6
1. public class TestMath {
1. public static void main(String[] args) {
1. new Calculator(); 10 }

11 }
12

1. //这里主要是完成计算器元素的布局
1. class Calculator extends Frame{
1. Calculator(){
1. //创建 3 个文本框，并指定其初始大小分别为 10 个字符和 15 个字符的大小 这里使用的是

TextField 类的另外一种构造方法 public TextField(int columns)

1. TextField num1 = new TextField(10);
1. TextField num2 = new TextField(10);
1. TextField num3 = new TextField(15); 20
1. //创建等号按钮
1. Button btnEqual = new Button("="); 23
1. //给等号按钮加上监听，让点击按钮后有响应事件发生
1. btnEqual.addActionListener(
1. new MyMonitor(num1, num2, num3) 27 );

28

1. //“+”是一个静态文本，所以使用 Label 类创建一个静态文本对象
1. Label lblPlus = new Label("+"); 31
1. //把 Frame 默认的 BorderLayout 布局改成 FlowLayout 布局
1. setLayout(new FlowLayout()); 34
1. add(num1);
1. add(lblPlus);
1. add(num2);
1. add(btnEqual);
1. add(num3);
1. pack();
1. setVisible(true);

42
43 }
44 }
45

1. class MyMonitor implements ActionListener{
1. //为了使对按钮的监听能够对文本框也起作用
1. //所以在自定义类 MyMonitor 里面定义三个 TextField 类型的对象 num1,num2,num3,
1. //并且定义了 MyMonitor 类的一个构造方法 这个构造方法带有三个 TextField 类型的参数，
1. //用于接收 从 TFFrame 类里面传递过来的三个 TextField 类型的参数
1. //然后把接收到的三个 TextField 类型的参数赋值给在本类中声明的 三个 TextField 类型的参数

num1,num2,num3

1. //然后再在 actionPerformed()方法里面处理 num1,num2,num3 53

54 TextField num1, num2, num3; 55

1. public MyMonitor(TextField num1, TextField num2, TextField num3) {
1. this.num1 = num1;
1. this.num2 = num2;
1. this.num3 = num3; 60 }

61

1.  //事件的相关信息都封装在了对象 e 里面，通过对象 e 的相关方法就可以获取事件的相关信息
1.  @Override
1.  public void actionPerformed(ActionEvent e) {
1.  // num 对象调用 getText()方法取得自己显示的文本字符串
1.  int n1 = Integer.parseInt(num1.getText());
1.  int n2 = Integer.parseInt(num2.getText()); 68
1.  //num3 对象调用 setText()方法设置自己的显示文本
1.  //字符串与任意类型的数据使用“+”连接时得到的一定是字符串，
1.      //这里使用一个空字符串与int类型的数连接，这样就可以直接把(n1+n2)得到的int类型的  数隐式地转换成字符串了，
1.  //这是一种把别的基础数据类型转换成字符串的一个小技巧。
1.  //也可以使用“String.valueOf((n1+n2))”把(n1+n2)的和转换成字符串 74 num3.setText("" + (n1 + n2));

75 //num3.setText(String.valueOf((n1+n2))); 76
77

1. //计算结束后清空 num1,num2 文本框里面的内容
1. num1.setText("");
1. num2.setText(""); 81 }

82 }

## 【JAVA 里面的经典用法：在一个类里面持有另外一个类的引用】

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
package com.kuang2;
import java.awt.\*;
import java.awt.event.ActionEvent; import java.awt.event.ActionListener;

public class TestMath1 {

public static void main(String[] args) { new Calculator2().launchFrame();
}

13 }
14

1. //做好计算器的窗体界面
1. class Calculator2 extends Frame {
1. //把设计计算器窗体的代码封装成一个方法
1. TextField num1, num2, num3; 19
1. public void launchFrame() {
1. num1 = new TextField(10);
1. num2 = new TextField(10);
1. num3 = new TextField(15);
1. Label lblPlus = new Label("+");
1. Button btnEqual = new Button("="); 26

27 btnEqual.addActionListener(new MyMonitorbtnEqual(this)); 28

1. setLayout(new FlowLayout());
1. add(num1);
1. add(lblPlus);
1. add(num2);
1. add(btnEqual);
1. add(num3);
1. pack();
1. setVisible(true); 37 }

38
39 }
40

1. //这里通过取得 Calculator2 类的引用，然后使用这个引用去访问 Calculator2 类里面的成员变量
1. //这种做法比上一种直接去访问 Calculator2 类里面的成员变量要好得多
1. //因为现在不需要知道 Calculator2 类里面有哪些成员变量了，
1. //现在要访问 Calculator2 类里面的成员变量，直接使用 Calculator2 类对象的引用去访问即可
1. //这个 Calculator2 类的对象好比是一个大管家， 而我告诉大管家，我要访问 Calculator2 类里面的那些成员变量，
1. //大管家的引用就会去帮我找，不再需要我自己去找了。
1. //这种在一个类里面持有另一个类的引用的用法是一种非常典型的用法
1. //使用获取到的引用就可以在一个类里面访问另一个类的所有成员了 49
1. class MyMonitorbtnEqual implements ActionListener {
1. Calculator2 calculator2 = null; 52
1. public MyMonitorbtnEqual(Calculator2 calculator2) {
1. this.calculator2 = calculator2; 55 }

56

1. @Override
1. public void actionPerformed(ActionEvent e) {
1. int n1 = Integer.parseInt(calculator2.num1.getText());
1. int n2 = Integer.parseInt(calculator2.num2.getText());
1. calculator2.num3.setText("" + (n1 + n2));
1. calculator2.num1.setText("");
1. calculator2.num2.setText(""); 64 }

65 }

结果：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834297240-e763e821-0592-435a-8491-46512a51c0bd.png#)

**七、内部类**

好处：
可以方便的访问包装类的成员
可以更清楚的组织逻辑，防止不应该被其他类 访问的类 进行访问何时使用：
该类不允许或不需要其它类进行访问时
【**内部类的使用范例**】

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
34
package com.kuang2;
import java.awt._;
import java.awt.event._;
public class TestMath3 {
public static void main(String args[]) { new MyMathFrame().launchFrame();
}
}
class MyMathFrame extends Frame { TextField num1, num2, num3;

public void launchFrame() { num1 = new TextField(10); num2 = new TextField(15); num3 = new TextField(15);
Label lblPlus = new Label("+"); Button btnEqual = new Button("=");
btnEqual.addActionListener(new MyMonitor()); setLayout(new FlowLayout());
add(num1); add(lblPlus); add(num2); add(btnEqual); add(num3); pack(); setVisible(true);
}

/\*

- 这个 MyMonitor 类是内部类，它在 MyFrame 类里面定义 MyFrame 类称为 MyMonitor 类的包
  装类
  35
  36
  37
  38
  39
  40
  _/
  /_

* 使用内部类的好处：
* 第一个巨大的好处就是可以畅通无阻地访问外部类(即内部类的包装类)的所有成员变量和方法
* 如这里的在 MyFrame 类(外部类)定义的三个成员变量 num1,num2,num3，
* 在 MyMonitor(内部类)里面就可以直接访问

| 41 |

} | _ 这相当于在创建外部类对象时内部类对象默认就拥有了一个外部类对象的引用
_/
private class MyMonitor implements ActionListener { public void actionPerformed(ActionEvent e) {
int n1 = Integer.parseInt(num1.getText()); int n2 = Integer.parseInt(num2.getText()); num3.setText("" + (n1 + n2));
num1.setText("");
num2.setText("");
}
} |
| --- | --- | --- |
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

内部类带来的巨大好处是：

1. 可以很方便地访问外部类定义的成员变量和方法
1. 当某一个类不需要其他类访问的时候就把这个类声明为内部类。

**八、Graphics 类**

每个 Component 都有一个 paint（Graphics g）用于实现绘图目的，每次重画该 Component 时都自动调用 paint 方法。
Graphics 类中提供了许多绘图方法，如：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834297493-10fb1f4a-920b-4f02-b1ff-79cdb57788d4.png#)

【测试代码】

1 package com.kuang3; 2
3 import java.awt.\*;

4

1.  public class TestPaint {
1.  public static void main(String[] args) {
1.  new MyPaint().launchFrame();
1.  //在 main()方法里面并没有显示调用 paint(Graphics g)方法
1.  //可是当创建出 Frame 窗体后却可以看到 Frame 窗体上画出了圆和矩形
1.  //这是因为 paint()方法是一个比较特殊的方法
1.  //在创建 Frame 窗体时会自动隐式调用
1.  //当我们把 Frame 窗体最小化又再次打开时
1.  //又会再次调用 paint()方法重新把圆和矩形在 Frame 窗体上画出来
1.  //即每次需要重画 Frame 窗体的时候就会自动调用 paint()方法 15 }

16 }
17

1. class MyPaint extends Frame{
1. public void launchFrame(){

20 setBounds(200,200,640,480);
21 setVisible(true); 22 }
23

1. public void paint(Graphics g){
1. //paint(Graphics g)方法有一个 Graphics 类型的参数 g
1. //我们可以把这个 g 当作是一个画家，这个画家手里拿着一只画笔
1. //我们通过设置画笔的颜色与形状来画出我们想要的各种各样的图像 28
1. /_设置画笔的颜色_/
1. g.setColor(Color.red);

31 g.fillOval(100,100,100,100);/_画一个实心椭圆_/
32 g.setColor(Color.green);
33 g.fillRect(150,200,200,200);/_画一个实心矩形_/
34

1. //这下面的两行代码是为了写程序的良好编程习惯而写的
1. //前面设置了画笔的颜色，现在就应该把画笔的初始颜色恢复过来
1. //就相当于是画家用完画笔之后把画笔上的颜色清理掉一样
1. Color c = g.getColor();
1. g.setColor(c); 40

41 System.out.println("gogoogo"); 42
43 }
44
45 }

# 九、鼠标事件适配器

抽象类 java.awt.event.MouseAdapter 实现了 MouseListener 接口，可以使用其子类作为
MouseEvent 的监听器，只要重写其相应的方法即可。 对于其他的监听器，也有对应的适配器。
适用适配器可以避免监听器定义没有必要的空方法。
【测试代码：画点】

1 package com.kuang3; 2
3 import java.awt.\*;

1. import java.awt.event.MouseAdapter;
1. import java.awt.event.MouseEvent;
1. import java.util.ArrayList;
1. import java.util.Iterator; 8

9 public class TestMouseAdapter {

1. public static void main(String[] args) {
1. new MyFrame("drawing ");

12 }
13 }
14

1. class MyFrame extends Frame{
1. ArrayList points = null;
1. MyFrame(String s){
1. super(s);
1. points = new ArrayList();
1. setLayout(null);

21 setBounds(200,200,400,300);

1. this.setBackground(new Color(204,204,255));
1. setVisible(true);
1. this.addMouseListener(new Monitor()); 25 }

26

1. public void paint(Graphics g){
1. Iterator i = points.iterator();
1. while (i.hasNext()){
1. Point p = (Point)i.next();
1. g.setColor(Color.BLUE);
1. g.fillOval(p.x,p.y,10,10); 33 }

34 }
35

1. public void addPoint(Point p){
1. points.add(p); 38 }

39

1. private class Monitor extends MouseAdapter{
1. @Override
1. public void mousePressed(MouseEvent e) {
1. MyFrame frame = (MyFrame) e.getSource();
1. frame.addPoint(new Point(e.getX(),e.getY()));
1. frame.repaint(); 46 }

47 }
48
49 }

# 十、window 事件

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834297999-4e65929b-e81f-4cd6-9a66-ddec17107429.jpeg#)

1 package com.kuang3; 2

1. import java.awt.\*;
1. import java.awt.event.\*; 5
1. public class TestWindowClose{
1. public static void main(String args[]){
1. new WindowFrame("关闭 WindowFrame"); 9 }

10 }
11

1. class WindowFrame extends Frame{
1. public WindowFrame(String s){
1. super(s);

15 setBounds(200,200,400,300);

1.  setLayout(null);
1.  setBackground(new Color(204,204,255));
1.  setVisible(true);
1.  this.addWindowListener(new WindowMonitor());
1.  /_监听本窗体的动作，把所有的动作信息封装成一个对象传递到监听类里面_/ 21
1.  this.addWindowListener(
1.  /\*在一个方法里面定义一个类，这个类称为局部类，也叫匿名的内部类，
1.  这里的{……代码……}里面的代码很像一个类的类体，只不过这个类没有名字，所以叫匿名类
1.      在这里是把这个匿名类当成WindowAdapter类来使用，语法上这样写的本质意义是相当于这  个匿名类
1.  从 WindowAdapter 类继承，现在 new 了一个匿名类的对象出来然后把这个对象当成

WindowAdapter 来使用

1. 这个匿名类出了()就没有人认识了\*/
1. new WindowAdapter(){
1. public void windowClosing(WindowEvent e){
1. setVisible(false);
1. System.exit(-1);

32 }
33 }
34 );
35 }
36

1. /_这里也是将监听类定义为内部类_/
1. class WindowMonitor extends WindowAdapter{
1. /\*WindowAdapter(Window 适配器)类实现了 WindowListener 监听接口
1. 重写了 WindowListener 接口里面的所有方法
   | 41 |

\*/

} |

} | 如果直接使用自定义 WindowMonitor 类直接去
实现 WindowListener 接口，那么就得要重写 WindowListener 接口 里面的所有方法，但现在只需要用到这些方法里面的其中一个方法
所以采用继承实现 WindowListener 监听接口的一个子类并重写这个子类里面需要用到的那个方法即可
这种做法比直接实现 WindowListener 监听接口要重写很多个用不到的方法要简洁方便得多

/_重写需要用到的 windowClosing(WindowEvent e)方法_/ public void windowClosing(WindowEvent e){
setVisible(false);/_将窗体设置为不显示，即可实现窗体关闭_/
System.exit(0);/_正常退出_/
} |
| --- | --- | --- | --- |
| 42 | | | |
| 43 | | | |
| 44 | | | |
| 45 | | | |
| 46 | | | |
|
47 | | | |
| 48 | | | |
| 49 | | | |
| 50 | | | |
| 51 | | | |
| 52 | | | |
| 53 | | | |

# 十一、键盘响应事件

【键盘响应事件——KeyEvent】

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
34
35
36
package com.kuang3;
import java.awt._; import java.awt.event._;

public class TestKeyEvent{
public static void main(String args[]){ new KeyFrame("键盘响应事件");
}
}

class KeyFrame extends Frame{ public KeyFrame(String s){
super(s); setBounds(200,200,400,300);
setLayout(null); setVisible(true); addKeyListener(new KeyMonitor());
}
/_把自定义的键盘的监听类定义为内部类
这个监听类从键盘适配器 KeyAdapter 类继承从 KeyAdapter 类继承也是为了可以简洁方便只需要重写需要用到的方法即可，这种做法比直接实现 KeyListener 接口要简单方便，如果
直接实现 KeyListener 接口就要把 KeyListener 接口里面的所有方法重写一遍，但真正用到的
只有一个方法，这样重写其他的方法但又用不到难免会做无用功_/
class KeyMonitor extends KeyAdapter{ public void keyPressed(KeyEvent e){
int keycode = e.getKeyCode();
/_使用 getKeyCode()方法获取按键的虚拟码_/
/\*如果获取到的键的虚拟码等于 up 键的虚拟码则表示当前按下的键是 up 键
KeyEvent.VK_UP 表示取得 up 键的虚拟码
键盘中的每一个键都对应有一个虚拟码

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
这些虚拟码在 KeyEvent 类里面都被定义为静态常量
所以可以使用“类名.静态常量名”的形式访问得到这些静态常量*/ if(keycode == KeyEvent.VK_UP){
System.out.println("你按的是 up 键");
}
}
}
}
/*键盘的处理事件是这样的：每一个键都对应着一个虚拟的码，
当按下某一个键时，系统就会去找这个键对应的虚拟的码，以此来确定当前按下的是那个键
\*/
**Swing**

Swing 是 GUI（图形用户界面）开发工具包，内容有很多，这里会分块编写，但在进阶篇中只编写
Swing 中的基本要素，包括容器、组件和布局等，更深入的内容这里就不介绍了。想深入学习的朋友们可查阅有关资料或图书，比如《*Java Swing*图形界面开发与案例详解》*——*清华大学出版社。
早期的 AWT（抽象窗口工具包）组件开发的图形用户界面，要依赖本地系统，当把 AWT 组件开发的应用程序移植到其他平台的系统上运行时，不能保证其外观风格，因此 AWT 是依赖于本地系统平台的。 而使用 Swing 开发的 Java 应用程序，其界面是不受本地系统平台限制的，也就是说 Swing 开发的 Java 应用 程序移植到其他系统平台上时，其界面外观是不会改变的。但要注意的是，虽然 Swing 提供的组件可以 方便开发 Java 应用程序，但是 Swing 并不能取代 AWT，在开发 Swing 程序时通常要借助与 AWT 的一些对象 来共同完成应用程序的设计。

**一、常用窗体**

Swing 窗体是 Swing 的一个组件，同时也是创建图形化用户界面的容器，可以将其它组件放置在窗体容器中。

1. **JFrame 框架窗体**

JFrame 窗体是一个容器，在 Swing 开发中我们经常要用到，它是 Swing 程序中各个组件的载体。语法格 式如下：

| 1   | JFrame | jf  | =   | new | JFrame(title); |
| --- | ------ | --- | --- | --- | -------------- |

当然，在开发中更常用的方式是通过继承 java.swing.JFrame 类创建一个窗体，可通过 this 关键字调用其 方法。
在 JFrame 对象创建完成后，需要调用 getContentPane()方法将窗体转换为容器，然后在容器中添加组件 或设置布局管理器，通常这个容器用来包含和显示组件。如果需要将组件添加至容器，可以使用来自
Container 类的 add()方法进行设置。至于 JPanel 容器会在后面提到。
【下面举一个 JFrame 窗体的例子】

| 1   | package com.kuang4; |                              |
| --- | ------------------- | ---------------------------- |
| 2   |                     |                              |
| 3   | import              | javax.swing.JFrame;          |
| 4   | import              | javax.swing.WindowConstants; |
| 5   |                     |                              |
| 6   | public              | class JFrameDemo {           |

| 7 |

} |
public void CreateJFrame() {
// 实例化一个 JFrame 对象
JFrame jf = new JFrame("这是一个 JFrame 窗体");
// 设置窗体可视
jf.setVisible(true);
// 设置窗体大小
jf.setSize(500, 350);
// 设置窗体关闭方式
jf.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}

public static void main(String[] args) {
new JFrameDemo().CreateJFrame(); // 调用 CreateJFrame()方法
} |
| --- | --- | --- |
| 8 | | |
| 9 | | |
| 10 | | |
| 11 | | |
| 12 | | |
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

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834298468-595a8b39-27e1-4df5-91dc-19a361dfe2a2.png#)结果：
这就是一个 500\*350 的窗体，用的是 setSize()方法；
标题为“这是一个 JFrame 窗体”，在实例化对象时就可以定义； 窗体关闭方式见窗体右上角为“EXIT_ON_CLOSE”；
窗体可视 setVisible()方法中的参数为“false”或不写 setVisible()方法时，此窗体不可见。
**常用的窗体关闭方式有四种：**
“DO_NOTHING_ON_CLOSE” ： 什 么 也 不 做 就 将 窗 体 关 闭 ； “DISPOSE_ON_CLOSE” ：任何注册监听程序对象后会自动隐藏并释放窗体； “HIDE_ON_CLOSE” ： 隐 藏 窗 口 的 默 认 窗 口 关 闭 ； “EXIT_ON_CLOSE”：退出应用程序默认窗口关闭。
【下面再举一个用继承 JFrame 的方式编写的代码，并加入 Container 容器及 JLabel 标签（后面会提到）， 来看一下具体的流程。】

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
34
35
36
37
38
39
40
41
package com.kuang4;
import java.awt.Color; import java.awt.Container;

import javax.swing.JFrame; import javax.swing.JLabel;
import javax.swing.SwingConstants; import javax.swing.WindowConstants;

public class JFrameDemo2 extends JFrame{
public void init() {
// 可视化
this.setVisible(true);
// 大小
this.setSize(500, 350);
// 标题
this.setTitle("西部开源");
// 关闭方式
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
// 创建一个 JLabel 标签
JLabel jl = new JLabel("欢迎来到西部开源学习，我是你们的 Java 老师，秦疆！");

// 使标签文字居中
jl.setHorizontalAlignment(SwingConstants.CENTER);

// 获取一个容器
Container container = this.getContentPane();
// 将标签添加至容器
container.add(jl);
// 设置容器背景颜色
container.setBackground(Color.YELLOW);
}
public static void main(String[] args) { new JFrameDemo2().init();
}
}

运行结果：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834298983-759d7dd8-059e-49c3-9dfc-a2089bad72a5.png#)

这里继承了 JFrame 类，所以方法中实现时用 this 关键字即可（或直接实现，不加 this）。

1. **JDialog 窗体**

JDialog 窗体是 Swing 组件中的对话框，继承了 AWT 组件中的 java.awt.Dialog 类。功能是从一个窗体中弹出另一个窗体。
【下面来看一个实例】

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
package com.kuang4;
import java.awt.Container;
import java.awt.event.ActionEvent; import java.awt.event.ActionListener;

import javax.swing.JButton; import javax.swing.JDialog; import javax.swing.JFrame; import javax.swing.JLabel;
import javax.swing.WindowConstants;

// 继承 JDialog 类
public class JDialogDemo extends JDialog {

// 实例化一个 JDialog 类对象，指定其父窗体、窗口标题和类型
public JDialogDemo() {
super(new MyJFrame(), "这是一个 JDialog 窗体", true); Container container = this.getContentPane(); container.add(new JLabel("秦老师带你学 Java")); this.setSize(500, 350);
}

public static void main(String[] args) { new JDialogDemo();
}

28
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
39
40
41
42
43
}
// 下面这部分内容包含监听器，可自行查阅资料
class MyJFrame extends JFrame { public MyJFrame() {
this.setVisible(true); this.setSize(700, 500);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
Container container = this.getContentPane(); container.setLayout(null);

JButton jb = new JButton("点击弹出对话框");
// 创建按钮
jb.setBounds(30, 30, 200, 50);
// 按钮位置及大小
jb.addActionListener(new ActionListener() {
// 监听器，用于监听
点击事件
44
45
46
47
48
49
50
51
@Override
public void actionPerformed(ActionEvent e) { new JDialogDemo().setVisible(true);
}
});
container.add(jb);
}
}

当我们点击按钮时，触发点击事件，创建一个 JDialog 的实例化对象，弹出一个窗口。这里出现了许多我们之前学过的知识，比如 super 关键字，在之前提到过，这里相当于使用了 JDialog(Frame f, String title, boolean model)形式的构造方法；监听器的实现就是一个匿名内部类，之前也提到过。

**二、标签组件**

在 Swing 中显示文本或提示信息的方法是使用标签，它支持文本字符串和图标。上面我们提到的
JLabel 就是这里的内容。

1. **标签**

标签由 JLabel 类定义，可以显示一行只读文本、一个图像或带图像的文本。
JLabel 类提供了许多构造方法，可查看 API 选择需要的使用，如显示只有文本的标签、只有图标的标签或包含文本与图标的标签等。因为上面已经出现过了，这里就不再举例了。常用语法格式如下，创建的是 一个不带图标和文本的 JLabel 对象：

| 1   | JLabel | jl  | =   | new | JLabel(); |
| --- | ------ | --- | --- | --- | --------- |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834299501-c6b8f329-85ed-4e66-b736-a257b4529901.png#)

1. **图标**

Swing 中的图标可以放置在按钮、标签等组件上，用于描述组件的用途。图标可以用 Java 支持的图片文 件类型进行创建，也可以使用 java.awt.Graphics 类提供的功能方法来创建。
在 Swing 中通过 Icon 接口来创建图标，可以在创建时给定图标的大小、颜色等特性。注意，Icon 是接口，在使用 Icon 接口的时候，必须实现 Icon 接口的三个方法：

| 1   | public | int getIconHeight()      |       |          |       |     |       |     |       |
| --- | ------ | ------------------------ | ----- | -------- | ----- | --- | ----- | --- | ----- |
| 2   | public | int getIconWidth()       |       |          |       |     |       |     |       |
| 3   | public | void paintIcon(Component | arg0, | Graphics | arg1, | int | arg2, | int | arg3) |

前两个方法用于获取图片的长宽，paintIcon()方法用于实现在指定坐标位置画图。下面看一个用 Icon 接口创建图标的实例：
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
package com.kuang4;
import java.awt.Component; import java.awt.Container; import java.awt.Graphics;

import javax.swing.Icon; import javax.swing.JFrame; import javax.swing.JLabel;
import javax.swing.SwingConstants;
import javax.swing.WindowConstants;
public class IconDemo extends JFrame implements Icon {
private int width;
private int height;
// 声明图标的宽
// 声明图标的长
public IconDemo() {}
// 定义无参构造方法
public IconDemo(int width, int height) { this.width = width;
this.height = height;
}
// 定义有参构造方法
@Override
public int getIconHeight() { return this.height;
}
// 实现 getIconHeight()方法
@Override
public int getIconWidth() {
// 实现 getIconWidth()方法

| 32 |

{

} | return this.width;
}

@Override
public void paintIcon(Component arg0, Graphics arg1, int arg2, int arg3)
// 实现 paintIcon()方法
arg1.fillOval(arg2, arg3, width, height); // 绘制一个圆形
}

public void init() { // 定义一个方法用于实现界面
IconDemo iconDemo = new IconDemo(15, 15); // 定义图标的长和宽
JLabel jb = new JLabel("icon 测试", iconDemo, SwingConstants.CENTER);
// 设置标签上的文字在标签正中间

Container container = getContentPane(); container.add(jb);

this.setVisible(true); this.setSize(500, 350);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}

public static void main(String[] args) { new IconDemo().init();
} |
| --- | --- | --- |
| 33 | | |
| 34 | | |
| 35 | | |
| 36 | | |
|
37 | | |
| 38 | | |
| 39 | | |
| 40 | | |
| 41 | | |
| 42 | | |
|
43 | | |
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

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834300030-7ab90013-3f50-40bf-a70f-5d784efc8fb0.png#)运行结果如下：
这样如果需要在窗体中使用图标，就可以用如下代码创建图标：

| 1   | IconDemo | iconDemo | =   | new | IconDemo(15, | 15); |
| --- | -------- | -------- | --- | --- | ------------ | ---- |

1. **图片图标**

Swing 中的图标除了可以绘制之外，还可以使用某个特定的图片创建。利用 javax.swing.ImageIcon 类根据现有图片创建图标。
下面看一个实例，我们先在包下放一个图片（注意放置位置，不同位置路径不同），如下：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834300499-a31a1518-f086-4496-8d07-feb741b64708.png#)
【下面是实现的代码】

| 1 | package com.kuang4;

import java.awt.Container; import java.net.URL;

import javax.swing.Icon; import javax.swing.ImageIcon; import javax.swing.JFrame; import javax.swing.JLabel;
import javax.swing.SwingConstants; import javax.swing.WindowConstants;

public class ImageIconDemo extends JFrame {

public ImageIconDemo() {
JLabel jl = new JLabel("这是一个 JFrame 窗体，旁边是一个图片"); URL url = ImageIconDemo.class.getResource("tx-old.jpg");
获得图片所在 URL
Icon icon = new ImageIcon(url); // 实例化 Icon 对象 jl.setIcon(icon); // 为标签设置图片 jl.setHorizontalAlignment(SwingConstants.CENTER); jl.setOpaque(true); // 设置标签为不透明状态

Container container = getContentPane(); container.add(jl);

setVisible(true); setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE); setSize(500, 350);
}

public static void main(String[] args) { new ImageIconDemo();
}

} |

| //  |
| --- | --- | --- |
| 2   |     |     |
| 3   |     |     |
| 4   |     |     |
| 5   |     |     |
| 6   |     |     |
| 7   |     |     |
| 8   |     |     |
| 9   |     |     |
| 10  |     |     |
| 11  |     |     |
| 12  |     |     |
| 13  |     |     |
| 14  |     |     |
| 15  |     |     |
| 16  |     |     |
| 17  |     |     |

|
18 | | |
| 19 | | |
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

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834300747-3453b17b-3cf5-4579-86e5-4abaebb4f095.jpeg#)
对于图片标签，我们经常将图片放置在标签上，用 JLabel 中的 setIcon()方法即可，当然也可以在初始化
JLabel 对象时为标签指定图标，这需要获取一个 Icon 实例。
而 getResource()方法可以获得资源文件的 URL 路径，这里的路径是相对于前面的那个类的，所以可将该 图片与该类放在同一个文件夹下；如果不在同一个文件夹下，需通过其它方法获取路径。

**三、布局管理器**

Swing 中，每个组件在容器中都有一个具体的位置和大小，在容器中摆放各自组件时很难判断其具体位置和大小，这里我们就要引入布局管理器了，它提供了基本的布局功能，可以有效的处理整个窗体的布 局。常用的布局管理器包括流布局管理器、边界布局管理器、网格布局管理器等。

1. **绝对布局**

绝对布局在上一篇的例子中已经出现过了，是硬性指定组件在容器中的位置和大小，可以使用绝对坐标 的方式来指定组件的位置。步骤如下：

1.  使用 Container.setLayout(null)方法取消布局管理器
1.  使用 Container.setBounds()方法设置每个组件的位置和大小

【举一个简单的例子】

| 1   | Container container = getContentPane(); // 创建容器  |
| --- | ---------------------------------------------------- |
| 2   | JButton jb = new JButton("按钮"); // 创建按钮        |
| 3   | jb.setBounds(10, 30, 100, 30); // 设置按钮位置和大小 |
| 4   | container.add(jb); // 将按钮添加到容器中             |

setBounds()方法中，前两个参数是位置的 xy 坐标，后两个参数是按钮的长和宽。

1. **流布局管理器**

流布局管理器是布局管理器中最基本的布局管理器，使用 FlowLayout 类，像“流”一样从左到右摆放 组件，直到占据了这一行的所有空间，再向下移动一行。组件在每一行的位置默认居中排列，要更改位 置可自行设置。
在 FlowLayout 的有参构造方法中，alignment 设置为 0 时，每一行的组件将被指定左对齐排列；当
alignment 被设置为 2 时，每一行的组件将被指定右对齐排列；而为 1 时是默认的居中排列。
下面举个例子，创建 10 个按钮并用流布局管理器排列。

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
package com.kuang5;
import java.awt.Container; import java.awt.FlowLayout;

import javax.swing.JButton; import javax.swing.JFrame;
import javax.swing.WindowConstants;

public class FlowLayoutDemo extends JFrame {
public FlowLayoutDemo() {
Container container = this.getContentPane();
// 设置流布局管理器，2 是右对齐，后两个参数分别为组件间的水平间隔和垂直间隔
this.setLayout(new FlowLayout(2, 10, 10));
// 循环添加按钮
for(int i=0; i<10; i++) { container.add(new JButton("按钮" + i));
}

this.setSize(300, 200); this.setVisible(true);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}
public static void main(String[] args) { new FlowLayoutDemo();
}
}

第一个参数为 2 是右对齐，每个按钮间的水平、垂直间隔都为 10。后两个图分别为参数为 1 居中排列和参数为 0 左对齐。运行结果如下：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834301034-2aeeaba5-6348-4f57-aafa-60ca740bccfc.png#)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834301233-a35ee729-204e-47e8-b18c-abf6d39d0ce9.png#)

1. **边界布局管理器**

在不指定窗体布局时，Swing 组件默认的布局管理器是边界布局管理器，使用的是 BorderLayout 类。在 上篇例子中，一个 JLabel 标签占据了整个空间，实质上是默认使用了边界布局管理器。边界布局管理器还可以容器分为东、南、西、北、中五个区域，可以将组件加入这五个区域中。
【演示】

| 1 | package com.kuang5;

import java.awt.BorderLayout; import java.awt.Container;

import javax.swing.JButton; import javax.swing.JFrame;
import javax.swing.WindowConstants;

public class BorderLayoutDemo extends JFrame {

private String[] border = {BorderLayout.CENTER, |

| BorderLayout.NORTH, |
| ------------------- | --- | --- |
| 2                   |     |     |
| 3                   |     |     |
| 4                   |     |     |
| 5                   |     |     |
| 6                   |     |     |
| 7                   |     |     |
| 8                   |     |     |
| 9                   |     |     |
| 10                  |     |     |
| 11                  |     |     |
| 12                  |     |     |

13
BorderLayout.SOUTH, BorderLayout.WEST, BorderLayout.EAST};
组用于存放组件摆放位置
// 此数
14
private String[] button = {"中", "北", "南", "西", "东"};
放按钮名称
// 此数组用于存
15
16
17
18
19
20
21
22
public BorderLayoutDemo() {
Container container = this.getContentPane();
this.setLayout(new BorderLayout());
// 设置容器为边界布局管理器
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
// 循环添加按钮
for(int i=0; i<button.length ; i++) {
container.add(border[i], new JButton(button[i])); // 左参数为设置布局，右参数为创建按钮
}

this.setVisible(true); this.setSize(300, 200);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}

public static void main(String[] args) { new BorderLayoutDemo();
}
}
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834301547-bf1b18b3-8ecb-4059-972a-e844a00c940a.png#)

1. **网格布局管理器**

网格布局管理器将容器划分为网格，组件按行按列排列，使用 GridLayout 类。在此布局管理器中，每个组件的大小都相同，且会填满整个网格，改变窗体大小，组件也会随之改变。
【演示】

| 1   | package com.kuang5;                 |     |
| --- | ----------------------------------- | --- |
| 2   |                                     |     |
| 3   | import java.awt.Container;          |     |
| 4   | import java.awt.GridLayout;         |     |
| 5   |                                     |     |
| 6   | import javax.swing.JButton;         |     |
| 7   | import javax.swing.JFrame;          |     |
| 8   | import javax.swing.WindowConstants; |     |
| 9   |                                     |     |
| 10  | class GirdLayoutDemo extends JFrame | {   |
| 11  |                                     |     |

12
13
14
public GirdLayoutDemo() {
Container container = this.getContentPane(); this.setLayout(new GridLayout(7, 3, 5, 5));
后两个参数为网格间的间距
// 前两个参数为 7 行 3 列，
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
for(int i=0; i<20; i++) { container.add(new JButton("按钮" + i));
}
this.setVisible(true); this.setSize(300, 300);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}
public static void main(String[] args) { new GirdLayoutDemo();
}
}
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834302075-8a7482ba-42fe-4eb7-b92d-f472c37f4249.jpeg#)

**四、面板**

面板也是一个容器，可作为容器容纳其他组件，但也必须被添加到其他容器中。Swing 中常用面板有
JPanel 面板和 JScrollPane 面板。

1. **JPanel**

JPanel 面板可以聚集一些组件来布局。继承自 java.awt.Container 类。
【演示】

| 1   | package com.kuang5; |                      |
| --- | ------------------- | -------------------- |
| 2   |                     |                      |
| 3   | import              | java.awt.Container;  |
| 4   | import              | java.awt.GridLayout; |
| 5   |                     |                      |
| 6   | import              | javax.swing.JButton; |

1.  import javax.swing.JFrame;
1.  import javax.swing.JPanel;
1.  import javax.swing.WindowConstants; 10

11 public class JPanelDemo extends JFrame { 12

1. public JPanelDemo() {
1. Container container = this.getContentPane();
1. container.setLayout(new GridLayout(2, 1, 10, 10)); // 整个容器为 2 行

1 列
16

1.      JPanel p1 = new JPanel(new GridLayout(1, 3));	// 初始化一个面板，设置1行3列的网格布局
1.      JPanel p2 = new JPanel(new GridLayout(1, 2));	// 初始化一个面板，设置1行2列的网格布局
1.      JPanel p3 = new JPanel(new GridLayout(2, 1));	// 初始化一个面板，设置2行1列的网格布局
1.      JPanel p4 = new JPanel(new GridLayout(3, 2));	// 初始化一个面板，设置3行2列的网格布局

21

1. p1.add(new JButton("1")); // 在 JPanel 面板中添加按钮
1. p1.add(new JButton("1")); // 在 JPanel 面板中添加按钮
1. p1.add(new JButton("1")); // 在 JPanel 面板中添加按钮
1. p2.add(new JButton("2")); // 在 JPanel 面板中添加按钮
1. p2.add(new JButton("2")); // 在 JPanel 面板中添加按钮
1. p3.add(new JButton("3")); // 在 JPanel 面板中添加按钮
1. p3.add(new JButton("3")); // 在 JPanel 面板中添加按钮
1. p4.add(new JButton("4")); // 在 JPanel 面板中添加按钮
1. p4.add(new JButton("4")); // 在 JPanel 面板中添加按钮
1. p4.add(new JButton("4")); // 在 JPanel 面板中添加按钮
1. p4.add(new JButton("4")); // 在 JPanel 面板中添加按钮
1. p4.add(new JButton("4")); // 在 JPanel 面板中添加按钮
1. p4.add(new JButton("4")); // 在 JPanel 面板中添加按钮

35

1. container.add(p1); // 在容器中添加面板
1. container.add(p2); // 在容器中添加面板
1. container.add(p3); // 在容器中添加面板
1. container.add(p4); // 在容器中添加面板

40

1. this.setVisible(true);
1. this.setSize(500, 350);
1. this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE); 44 }

45

1. public static void main(String[] args) {
1. new JPanelDemo(); 48 }

49
50 }

运行结果如下，可自行对比代码与结果理解 JPanel。其中，容器的 GridLayout 布局设置了横纵都为 10 的 间距，JPanel 的 GridLayout 布局没有设置网格间距。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834302598-c9b4a28e-4814-464d-aef9-1224ae25c1bc.png#)

1. **JScrollPane**

若遇到一个较小的容器窗体中显示一个较大部分内容的情况，可用 JScrollPane 面板。这是一个带滚 动条的面板，就像平时浏览网页，经常遇到的滚动条一样。
如果需要在 JScrollPane 面板中放置多个组件，需将这多个组件放置在 JPanel 面板上，然后将 JPanel 面板作为一个整体组件添加在 JScrollPane 面板上。
【演示】

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
package com.kuang5;
import java.awt.Container;

import javax.swing.JFrame; import javax.swing.JScrollPane; import javax.swing.JTextArea;
import javax.swing.WindowConstants;

public class JScrollPaneDemo extends JFrame {

public JScrollPaneDemo() {
Container container = this.getContentPane();

JTextArea tArea = new JTextArea(20, 50); tArea.setText("欢迎来到西部开源学 Java");
// 创建文本区域组件
JScrollPane sp = new JScrollPane(tArea);
container.add(sp);
this.setVisible(true); this.setSize(300, 150);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}
public static void main(String[] args) {
new JScrollPaneDemo();

28 }
29
30 }

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834302788-a0786eb1-78a0-4d6f-b7dd-d57e111acde5.png#)结果：
其中 JTextArea 是创建一个文本区域组件，大小为 20\*50，setText()方法是给该文本区域填值。这里在
new 一个 JScrollPane 时，就将文本区域组件添加到其上。

**五、按钮组件**

1. **提交按钮组件（JButton）**

JButton 在之前的例子中已经出现多次，是较为常用的组件，用于触发特定动作。可以在按钮上显示文本标签，还可以显示图标，如下：

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
package com.kuang5;
import javax.swing._; import java.awt._;

public class Demo extends JFrame {
public Demo(){
Container container = this.getContentPane();
Icon icon = new ImageIcon(Demo.class.getResource("tx-old.jpg"));
JButton jb = new JButton();

jb.setIcon(icon); // 设置图标
jb.setToolTipText("图片按钮"); // 设置按钮提示

container.add(jb);

this.setVisible(true); this.setSize(500, 350);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}
public static void main(String[] args) { new Demo();
}
}

1. **单选按钮组件（JRadioButton）**

默认情况下，单选按钮显示一个圆形图标，通常在其旁放置一些说明性文字。当用户选中某个单选按钮 后，按钮组中其它按钮将被自动取消，这时就需要按钮组（ButtonGroup）来将同组按钮放在一起，该按钮组中的按钮只能选择一个，而不在此按钮中的按钮不受影响。语法格式如下：

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
34
35
36
37
package com.kuang5;
import javax.swing._; import java.awt._;

public class Demo extends JFrame {
public Demo(){
Container container = this.getContentPane();
Icon icon = new ImageIcon(Demo.class.getResource("tx-old.jpg"));

//单选框
JRadioButton jr1 = new JRadioButton("JRadioButton1"); JRadioButton jr2 = new JRadioButton("JRadioButton2"); JRadioButton jr3 = new JRadioButton("JRadioButton3");

//按钮组，单选框只能选择一个
ButtonGroup group = new ButtonGroup(); group.add(jr1);
group.add(jr2); group.add(jr3);

container.add(jr1,BorderLayout.CENTER); container.add(jr2,BorderLayout.NORTH); container.add(jr3,BorderLayout.SOUTH);

this.setVisible(true); this.setSize(500, 350);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}
public static void main(String[] args) { new Demo();
}
}

1. **复选框组件（JCheckBox）**

复选框是一个方块图标，外加一段描述性文字，与单选按钮的区别就是可以多选。每一个复选框都提供
“选中”与“不选中”两种状态。语法格式如下：

| 1   | package | com.kuang5;        |        |     |
| --- | ------- | ------------------ | ------ | --- |
| 2   |         |                    |        |     |
| 3   | import  | javax.swing.\*;    |        |     |
| 4   | import  | java.awt.\*;       |        |     |
| 5   |         |                    |        |     |
| 6   | public  | class Demo extends | JFrame | {   |

| 7 |

} |
public Demo(){
Container container = this.getContentPane();

Icon icon = new ImageIcon(Demo.class.getResource("tx-old.jpg"));

//多选框
JCheckBox jrb = new JCheckBox("abc"); JCheckBox jrb2 = new JCheckBox("abc"); container.add(jrb); container.add(jrb2,BorderLayout.NORTH);

this.setVisible(true); this.setSize(500, 350);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}

public static void main(String[] args) { new Demo();
} |
| --- | --- | --- |
| 8 | | |
| 9 | | |
| 10 | | |
| 11 | | |
| 12 | | |
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
| 25 | | |
| 26 | | |
| 27 | | |
| 28 | | |

**六、列表组件**

1. **下拉列表（JComboBox）**

下拉列表框使用 JComboBox 类对象来表示，如下方代码：

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
package com.kuang5;
import javax.swing._; import java.awt._;

public class Demo extends JFrame {

public Demo(){
Container container = this.getContentPane();

Icon icon = new ImageIcon(Demo.class.getResource("tx-old.jpg"));

JComboBox status = new JComboBox(); status.addItem(null); status.addItem(" 正 在 上 映 "); status.addItem(" 即 将 上 映 "); status.addItem("下架");

container.add(status);

this.setVisible(true); this.setSize(500, 350);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);

25
26
27
28
29
30
}

public static void main(String[] args) { new Demo();
}
}

显示的样式如下：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834303113-381e60ce-2277-4c4b-86a6-701d0f639f96.png#)

1. **列表框（JList）**

列表框只是在窗体上占据固定的大小，如果要使列表框具有滚动效果，可以将列表框放入滚动面板中。 使用数组初始化列表框的参数如下。
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
package com.kuang5;
import javax.swing._; import java.awt._;

public class Demo extends JFrame {
public Demo(){
Container container = this.getContentPane();
Icon icon = new ImageIcon(Demo.class.getResource("tx-old.jpg"));

//使用数组初始化列表框的参数如下。String[] contents = {"1", "2", "3"}; JList jl = new JList(contents);

container.add(jl);

this.setVisible(true); this.setSize(500, 350);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}
public static void main(String[] args) { new Demo();
}
}

将 Vector 类型的数据作为初始化 JList 的参数如下。

| 1   | package com.kuang5;    |
| --- | ---------------------- |
| 2   |                        |
| 3   | import javax.swing.\*; |
| 4   | import java.awt.\*;    |

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
import java.util.Vector;
public class Demo extends JFrame {
public Demo(){
Container container = this.getContentPane();
Icon icon = new ImageIcon(Demo.class.getResource("tx-old.jpg"));

//将 Vector 类型的数据作为初始化 JList 的参数如下 Vector contents = new Vector();
JList jl = new JList(contents); contents.add("1");
contents.add("2");
contents.add("3");

container.add(jl);

this.setVisible(true); this.setSize(500, 350);
this.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
}
public static void main(String[] args) { new Demo();
}
}

**七、文本组件**

1. **文本框（JTextField）**

文本框用来显示或编辑一个单行文本，语法格式如下：

| 1   | JTextField jt = new JTextField("aaa"); // 创建一个文本框，值为 aaa              |
| --- | ------------------------------------------------------------------------------- |
| 2   | JTextField jt2 = new JTextField("aaa", 20); // 创建一个长度为 20 的文本框，值为 |
|     | aaa                                                                             |
| 3   | jt.setText(""); // 将文本框置空                                                 |

其余构造方法可参考 API 或源码。

1. **密码框（JPasswordField）**

密码框与文本框的定义与用法类似，但会使用户输入的字符串以某种符号进行加密。如下方代码：

| 1   | JPasswordField jp = new JPasswordField(); |
| --- | ----------------------------------------- |
| 2   | jp.setEchoChar('#'); // 设置回显符号      |

1. **文本域（JTextArea）**

文本域组件在上面的代码中已经出现了，如下方代码所示：

| 1   | JTextArea tArea = new JTextArea(20, 50);  | // 创建文本区域组件 |
| --- | ----------------------------------------- | ------------------- |
| 2   | tArea.setText("欢迎来到西部开源学 Java"); |                     |

**我们对 GUI 编程就讲到这里了，授人以鱼不如授人以渔，相信大家经过这一小段的学习已经能掌握看方 法和源码学习的能力了，之后我们会有一些小游戏专题来巩固我们 JavaSE 阶段的学习。**
**小游戏：2048**

思路：
使用了 4x4 的 GridLayout 作为布局，然后使用 16 个 JLabel 作为方块 ui。数据上则是使用一个长度为 16 的
int 数组储存方块的数值，通过监听上下左右的按键进行相应的数据处理，最后通过刷新函数将数据显示 出来并设置颜色。这里提一下胜负判定的实现，胜的判定很简单，就是玩家凑出了至少一个 2048 的方块即为胜利，而失败的判定思路略复杂，主要是通过模拟用户分别按下上、下、左、右键后，判断格子里 是否还有空位，如分别向四个方向移动后都无法产生空位，则判负。
【Game 类】

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
import javax.swing._; import java.awt._;
import java.awt.event.KeyEvent; import java.awt.event.KeyListener; import java.util.ArrayList;
import java.util.Arrays; import java.util.HashMap;
import java.util.List;
public class Game {
//用于储存颜色的实体类
private static class Color {
public Color(int fc, int bgc) { fontColor = fc;//字体颜色 bgColor = bgc;//背景颜色
}
public int fontColor;//字体颜色
public int bgColor;//背景颜色
}
JFrame mainFrame;//主窗口对象
JLabel[] jLabels;//方块，用 jlabel 代替 int[] datas = new int[]{0, 0, 0, 0,
0, 0, 0, 0,
0, 0, 0, 0,
0, 0, 0, 0};//每个方块上的数值
int[] temp = new int[4];//方块移动算法中抽离的的临时数组 int[] temp2 = new int[16];//用于检测方块是否有合并

List emptyBlocks = new ArrayList<Integer>(16);//在生成新方块时用到的临时
list，用以存放空方块
34

| 35 |

{{ | //存放颜色的 map
static HashMap<Integer, Color> colorMap = new HashMap<Integer, Color>()

put(0, new Color(0x776e65, 0xCDC1B4)); put(2, new Color(0x776e65, 0xeee4da)); put(4, new Color(0x776e65, 0xede0c8)); put(8, new Color(0xf9f6f2, 0xf2b179)); put(16, new Color(0xf9f6f2, 0xf59563)); put(32, new Color(0xf9f6f2, 0xf67c5f)); put(64, new Color(0xf9f6f2, 0xf65e3b)); put(128, new Color(0xf9f6f2, 0xedcf72)); put(256, new Color(0xf9f6f2, 0xedcc61)); put(512, new Color(0xf9f6f2, 0xe4c02a)); put(1024, new Color(0xf9f6f2, 0xe2ba13)); put(2048, new Color(0xf9f6f2, 0xecc400));
}};

public Game() { initGameFrame(); initGame(); refresh();
}

//开局时生成两个 2 的方块和一个 4 的方块 private void initGame() {
for (int i = 0; i < 2; i++) { generateBlock(datas, 2);
}
generateBlock(datas, 4);
}

//随机生成 4 或者 2 的方块
private void randomGenerate(int arr[]) { int ran = (int) (Math.random() \* 10); if (ran > 5) {
generateBlock(arr, 4);
} else {
generateBlock(arr, 2);
}

}

//随机生成新的方块，参数：要生成的方块数值
private void generateBlock(int arr[], int num) { emptyBlocks.clear();

for (int i = 0; i < 16; i++) { if (arr[i] == 0) {
emptyBlocks.add(i);
}
}
int len = emptyBlocks.size(); if (len == 0) {
return;
}
int pos = (int) (Math.random() \* 100) % len; arr[(int) emptyBlocks.get(pos)] = num; refresh(); |
| --- | --- | --- |
| 36 | | |
|
37 | | |
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
| 77 | | |
| 78 | | |
| 79 | | |
| 80 | | |
| 81 | | |
| 82 | | |
| 83 | | |
| 84 | | |
| 85 | | |
| 86 | | |
| 87 | | |
| 88 | | |
| 89 | | |
| 90 | | |
| 91 | | |

| 92  |      |                                                                      |     |     |
| --- | ---- | -------------------------------------------------------------------- | --- | --- |
| 93  |      | }                                                                    |     |     |
| 94  |      |                                                                      |     |     |
| 95  |      |                                                                      |     |     |
| 96  |      | //胜负判定并做终局处理                                               |     |     |
| 97  |      | private void judge(int arr[]) {                                      |     |     |
| 98  |      |                                                                      |     |     |
| 99  |      | if (isWin(arr)) {                                                    |     |     |
| 100 |      | JOptionPane.showMessageDialog(null, "恭喜，你已经成功凑出 2048 的方  |     |     |
|     | 块", | "你赢了", JOptionPane.PLAIN_MESSAGE);                                |     |     |
| 101 |      | System.exit(0);                                                      |     |     |
| 102 |      | }                                                                    |     |     |
| 103 |      | if (isEnd(arr)) {                                                    |     |     |
| 104 |      | int max = getMax(datas);                                             |     |     |
| 105 |      | JOptionPane.showMessageDialog(null, "抱歉，你没有凑出 2048 的方块,你 |     |     |

|
106
107
108
109
110
111
112
113
114
115
116
117
118
119
120
121
122 | 的最大方块是：" + max, "游戏结束", JOptionPane.PLAIN_MESSAGE); System.exit(0);
}

}

//判断玩家是否胜利，只要有一个方块大于等于 2048 即为胜利 private boolean isWin(int arr[]) {
for (int i : arr) { if (i >= 2048) {
return true;
}
}
return false;

}

//此函数用于判断游戏是否结束，如上下左右移后均无法产生空块，即代表方块已满，则返回 真，表示游戏结束 | | | |
| 123 | private boolean isEnd(int arr[]) { | | | |
| 124 | | | | |
| 125 | int[] tmp = new int[16]; | | | |
| 126 | int isend = 0; | | | |
| 127 | | | | |
| 128 | System.arraycopy(arr, 0, tmp, | | 0, | 16); |
| 129 | left(tmp); | | | |
| 130 | if (isNoBlank(tmp)) { | | | |
| 131 | isend++; | | | |
| 132 | } | | | |
| 133 | | | | |
| 134 | System.arraycopy(arr, 0, tmp, | | 0, | 16); |
| 135 | right(tmp); | | | |
| 136 | if (isNoBlank(tmp)) { | | | |
| 137 | isend++; | | | |
| 138 | } | | | |
| 139 | | | | |
| 140 | System.arraycopy(arr, 0, tmp, | | 0, | 16); |
| 141 | up(tmp); | | | |
| 142 | if (isNoBlank(tmp)) { | | | |
| 143 | isend++; | | | |
| 144 | } | | | |
| 145 | | | | |
| 146 | System.arraycopy(arr, 0, tmp, | | 0, | 16); |

1. down(tmp);
1. if (isNoBlank(tmp)) {
1. isend++;

150 }
151

1. if (isend == 4) {
1. return true;
1. } else {
1. return false;

156 }
157 }
158
159 //判断是否无空方块
160 private boolean isNoBlank(int arr[]) { 161
162 for (int i : arr) { 163 if (i == 0) {
164 return false;
165 }
166 }
167 return true;
168 }
169

1. //获取最大的方块数值
1. private int getMax(int arr[]) {
1. int max = arr[0];
1. for (int i : arr) {
1. if (i >= max) {
1. max = i;

176 }
177 }
178 return max;
179 }
180

1. //刷新每个方块显示的数据
1. private void refresh() {
1. JLabel j;

184 for (int i = 0; i < 16; i++) {
185 int arr = datas[i];
186 j = jLabels[i];
187 if (arr == 0) {
188 j.setText("");
189 } else if (arr >= 1024) {

1. j.setFont(new Font("Dialog", 1, 42));
1. j.setText(String.valueOf(datas[i]));
1. } else {
1. j.setFont(new Font("Dialog", 1, 50));
1. j.setText(String.valueOf(arr));

195 }
196

1. Color currColor = colorMap.get(arr);
1. j.setBackground(new java.awt.Color(currColor.bgColor));
1. j.setForeground(new java.awt.Color(currColor.fontColor));

200 }
201 }
202
203 //初始化游戏窗口，做一些繁杂的操作
204 private void initGameFrame() {

205

1.  //创建 JFrame 以及做一些设置
1.  mainFrame = new JFrame("2048 Game");
1.  mainFrame.setSize(500, 500);
1.  mainFrame.setResizable(false);//固定窗口尺寸
1.  mainFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
1.  mainFrame.setLocationRelativeTo(null); 212
1.  mainFrame.setLayout(new GridLayout(4, 4));
1.      mainFrame.getContentPane().setBackground(new java.awt.Color(0xCDC1B4));
1.  //添加按键监听
1.  mainFrame.addKeyListener(new KeyListener() {
1.  @Override
1.  public void keyTyped(KeyEvent keyEvent) {

219 }
220
221 @Override
222 public void keyPressed(KeyEvent keyEvent) { 223
224 System.arraycopy(datas, 0, temp2, 0, 16); 225

1. //根据按键的不同调用不同的处理函数
1. switch (keyEvent.getKeyCode()) {
1. case KeyEvent.VK_UP:
1. up(datas);
1. break;
1.

1. case KeyEvent.VK_DOWN:
1. down(datas);
1. break;
1.

1. case KeyEvent.VK_LEFT:
1. left(datas);
1. break;
1.

1. case KeyEvent.VK_RIGHT:
1. right(datas);
1. break;
1.

244 }
245
246

1. //判断移动后是否有方块合并，若有，生成新方块，若无，不产生新方块
1. if (!Arrays.equals(datas, temp2)) {
1. randomGenerate(datas);

250 }
251
252 refresh();
253 judge(datas);
254 }
255
256 @Override
257 public void keyReleased(KeyEvent keyEvent) {
258 }
259 });
260
261 //使用系统默认的 ui 风格

262 try {
263
UIManager.setLookAndFeel(UIManager.getSystemLookAndFeelClassName());
264 } catch (Exception e) {
265 JOptionPane.showMessageDialog(null, e.getMessage());
266 }
267

1.  //使用 16 个 JLabel 来显示 16 个方块
1.  jLabels = new JLabel[16];
1.  JLabel j; //引用复用，避免 for 里创建过多引用 271 for (int i = 0; i < 16; i++) {
1.  jLabels[i] = new JLabel("0", JLabel.CENTER);
1.  j = jLabels[i];
1.  j.setOpaque(true);
1.  // 设置边界，参数：上，左，下，右，边界颜色
1.      j.setBorder(BorderFactory.createMatteBorder(6, 6, 6, 6, new java.awt.Color(0xBBADA0)));
1.

1.  //j.setForeground(new java.awt.Color(0x776E65));
1.  j.setFont(new Font("Dialog", 1, 52));
1.  mainFrame.add(j);

281 }
282 mainFrame.setVisible(true);
283 }
284
285 private void left(int arr[]) {
286 moveLeft(arr); 287
288 combineLeft(arr); 289
290 moveLeft(arr);//合并完后会产生空位，所以要再次左移
291
292
293 }
294
295 //向左合并方块
296 private void combineLeft(int arr[]) { 297 for (int l = 0; l < 4; l++) {
298 //0 1 2
299 for (int i = 0; i < 3; i++) {
300 if ((arr[l * 4 + i] != 0 && arr[l * 4 + i + 1] != 0) && arr[l * 4 + i] == arr[l * 4 + i + 1]) {
301 arr[l * 4 + i] _= 2;
302 arr[l _ 4 + i + 1] = 0;
303 }
304 }
305 }
306 }
307
308 //方块左移，针对每一行利用临时数组实现左移
309 private void moveLeft(int arr[]) { 310 for (int l = 0; l < 4; l++) { 311
312
313 int z = 0, fz = 0;//z(零）;fz（非零）
314 for (int i = 0; i < 4; i++) {
315 if (arr[l * 4 + i] == 0) { 316 z++;

| 317 |     |     |     | } else {                   |     |
| --- | --- | --- | --- | -------------------------- | --- |
| 318 |     |     |     | temp[fz] = arr[l \* 4 +    | i]; |
| 319 |     |     |     | fz++;                      |     |
| 320 |     |     |     | }                          |     |
| 321 |     |     | }   |                            |     |
| 322 |     |     | for | (int i = fz; i < 4; i++) { |     |
| 323 |     |     |     | temp[i] = 0;               |     |
| 324 |     |     | }   |                            |     |
| 325 |     |     | for | (int j = 0; j < 4; j++) {  |     |
| 326 |     |     |     | arr[l * 4 + j] = temp[j];  |     |
| 327 |     |     | }   |                            |     |
| 328 |     | }   |     |                            |     |
| 329 | }   |     |     |                            |     |
| 330 |     |     |     |                            |     |

| 331
332
333
334
335
336
337
338
339
340
341
342
343

344
345
346
347
348
349
350 | private void right(int arr[]) {

moveRight(arr); combineRight(arr); moveRight(arr);

}

private void combineRight(int arr[]) { for (int l = 0; l < 4; l++) {
//3 2 1
for (int i = 3; i > 0; i--) {
if ((arr[l * 4 + i] != 0 && arr[l * 4 + i - 1] != 0) && arr[l * 4 + i] == arr[l * 4 + i - 1]) {
arr[l * 4 + i] _= 2; arr[l _ 4 + i - 1] = 0;
}
}
}
} | | | | |
| 351 | private | | void moveRight(int arr[]) { | | |
| 352 | | | | | |
| 353 | for | | (int l = 0; l < 4; l++) { | | |
| 354 | | | | | |
| 355 | | | int z = 3, fz = 3;//z(零）;fz（非零） | | |
| 356 | | | for (int i = 3; i >= 0; i--) { | | |
| 357 | | | if (arr[l * 4 + i] == 0) { | | |
| 358 | | | z--; | | |
| 359 | | | } else { | | |
| 360 | | | temp[fz] = arr[l * 4 + i]; | | |
| 361 | | | fz--; | | |
| 362 | | | } | | |
| 363 | | | } | | |
| 364 | | | for (int i = fz; i >= 0; i--) { | | |
| 365 | | | temp[i] = 0; | | |
| 366 | | | } | | |
| 367 | | | for (int j = 3; j >= 0; j--) { | | |
| 368 | | | arr[l * 4 + j] = temp[j]; | | |
| 369 | | | } | | |
| 370 | } | | | | |
| 371 | } | | | | |
| 372 | | | | | |
| 373 | | | | | |

1. private void up(int arr[]) {
1. moveUp(arr);
1. combineUp(arr);
1. moveUp(arr); 378

379 }
380
381 private void combineUp(int arr[]) { 382
383
384 for (int r = 0; r < 4; r++) {
385 for (int i = 0; i < 3; i++) {
386 if ((arr[r + 4 * i] != 0 && arr[r + 4 * (i + 1)] != 0) && arr[r + 4 * i] == arr[r + 4 * (i + 1)]) {
387 arr[r + 4 * i] _= 2;
388 arr[r + 4 _ (i + 1)] = 0;
389 }
390 }
391 }
392 }
393
394 private void moveUp(int arr[]) { 395
396 for (int r = 0; r < 4; r++) { 397
398 int z = 0, fz = 0;//z(零）;fz（非零）
399 for (int i = 0; i < 4; i++) {
400 if (arr[r + 4 * i] == 0) { 401 z++;
402 } else {
403 temp[fz] = arr[r + 4 * i];
404 fz++;
405 }
406 }
407 for (int i = fz; i < 4; i++) {
408 temp[i] = 0;
409 }
410 for (int j = 0; j < 4; j++) {
411 arr[r + 4 * j] = temp[j];
412 }
413 }
414 }
415
416
417 private void down(int arr[]) {
418 moveDown(arr);
419 combineDown(arr);
420 moveDown(arr);
421 }
422
423 private void combineDown(int arr[]) { 424 for (int r = 0; r < 4; r++) {
425 for (int i = 3; i > 0; i--) {
426 if ((arr[r + 4 * i] != 0 && arr[r + 4 * (i - 1)] != 0) &&

|     | arr[r | +   | 4   | \* i] | ==  | arr[r + | 4   | \*  | (i -  | 1)]) {     |
| --- | ----- | --- | --- | ----- | --- | ------- | --- | --- | ----- | ---------- |
| 427 |       |     |     |       |     | arr[r   | +   | 4   | \* i] | \*= 2;     |
| 428 |       |     |     |       |     | arr[r   | +   | 4   | \* (i | - 1)] = 0; |
| 429 |       |     |     |       | }   |         |     |     |       |            |

| 430 |

} | }
}
}

private void moveDown(int arr[]) { for (int r = 0; r < 4; r++) {

int z = 3, fz = 3;//z(零）;fz（非零） for (int i = 3; i >= 0; i--) {
if (arr[r + 4 * i] == 0) { z--;
} else {
temp[fz] = arr[r + 4 * i]; fz--;
}
}
for (int i = fz; i >= 0; i--) { temp[i] = 0;
}
for (int j = 3; j >= 0; j--) { arr[r + 4 * j] = temp[j];
}
}
} |
| --- | --- | --- |
| 431 | | |
| 432 | | |
| 433 | | |
| 434 | | |
| 435 | | |
| 436 | | |
| 437 | | |
| 438 | | |
| 439 | | |
| 440 | | |
| 441 | | |
| 442 | | |
| 443 | | |
| 444 | | |
| 445 | | |
| 446 | | |
| 447 | | |
| 448 | | |
| 449 | | |
| 450 | | |
| 451 | | |
| 452 | | |
| 453 | | |
| 454 | | |
| 455 | | |

【StartFrame 类】

1 package com.test2048; 2

1. import javax.swing.\*;
1. import java.awt.\*;
1. import java.awt.event.ActionEvent;
1. import java.awt.event.ActionListener; 7

8 public class StartFrame { 9

1.  JFrame mainFrame;
1.      final String gameRule = "2048游戏共有16个格子，开始时会随机生成两个数值为2的方块和一个数值为4的方块，\n" +
1.  "玩家可通过键盘上的上、下、左、右方向键来操控方块的滑动方向，\n" +
1.      "每按一次方向键，所有的方块会向一个方向靠拢，相同数值的方块将会相加并合并成 一个方块，\n" +
1.  "此外，每滑动一次将会随机生成一个数值为 2 或者 4 的方块，\n" +
1.      "玩家需要想办法在这16个格子里凑出2048数值的方块，若16个格子被填满且无法再 移动，\n" +
1.  "则游戏结束。"; 17
1.  public StartFrame() {
1.  initFrame(); 20 }

21

1. private void initFrame() {
1. mainFrame = new JFrame("2048 Game");
1. mainFrame.setSize(500, 500);
1. mainFrame.setResizable(false);//固定窗口尺寸
1. mainFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
1. mainFrame.setLocationRelativeTo(null);//窗口居中 28

29

1. JPanel jPanel = new JPanel();
1. //BoxLayout.Y_AXIS 是指定从上到下垂直布置组件。
1. jPanel.setLayout(new BoxLayout(jPanel, BoxLayout.Y_AXIS)); 33

34 jPanel.add(newLine(Box.createVerticalStrut(25)));//添加空白区域
35

1. JLabel jLabel = new JLabel("2048");
1. jLabel.setForeground(new Color(0x776e65));
1. jLabel.setFont(new Font("Dialog", 1, 92));
1. jPanel.add(newLine(jLabel)); 40

41 /\*

1. JLabel author = new JLabel("by xxx");
1. jPanel.add(newLine(author));

44 \*/
45
46
47 jPanel.add(newLine(Box.createVerticalStrut(50))); 48
49

1. JButton btn1 = new JButton("开始游戏");
1. btn1.addActionListener(new ActionListener() {
1. @Override
1. public void actionPerformed(ActionEvent actionEvent) {
1. new Game();
1. mainFrame.dispose(); 56 }

57 });
58 jPanel.add(newLine(btn1)); 59
60
61 jPanel.add(newLine(Box.createVerticalStrut(50))); 62
63

1.  JButton btn2 = new JButton("游戏规则");
1.  btn2.addActionListener(new ActionListener() {
1.  @Override
1.  public void actionPerformed(ActionEvent actionEvent) {
1.      JOptionPane.showMessageDialog(null, gameRule, "游戏规则", JOptionPane.PLAIN_MESSAGE);

69 }
70 });
71 jPanel.add(newLine(btn2)); 72
73
74 jPanel.add(newLine(Box.createVerticalStrut(50))); 75
76

1. JButton btn3 = new JButton("退出游戏");
1. btn3.addActionListener(new ActionListener() {
1. @Override
1. public void actionPerformed(ActionEvent actionEvent) {
1. System.exit(0);

82 }
83 });

| 84 |

} | jPanel.add(newLine(btn3));

mainFrame.add(jPanel);

mainFrame.setVisible(true);
}

//添加新一行垂直居中的控件，通过在控件两边填充 glue 对象实现 private JPanel newLine(Component c) {

JPanel jp = new JPanel();
jp.setLayout(new BoxLayout(jp, BoxLayout.X_AXIS)); jp.add(Box.createHorizontalGlue());
jp.add(c); jp.add(Box.createHorizontalGlue());
jp.setOpaque(false);//设置不透明

return jp;
} |
| --- | --- | --- |
| 85 | | |
| 86 | | |
| 87 | | |
| 88 | | |
| 89 | | |
| 90 | | |
| 91 | | |
| 92 | | |
| 93 | | |
| 94 | | |
| 95 | | |
| 96 | | |
| 97 | | |
| 98 | | |
| 99 | | |
| 100 | | |
| 101 | | |
| 102 | | |
| 103 | | |
| 104 | | |
| 105 | | |

【Main】

1
2
3
4
5
6
7
8
9
package com.test2048;
public class Main {
public static void main(String[] args) { new StartFrame();
}
}
