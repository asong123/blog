---
title: JAVA注解与反射
urlname: lurog0
date: '2021-07-12 12:52:26 +0800'
tags: []
categories: []
abbrlink: 59568
---

# 注解(Annotation)

## 注解入门

Annotation 是 JDK5.0 开始引入的技术
**Annotation 的作用：**
不是程序本身，可以对程序作出解释
**可以被其它程序（比如编译器）读取。**
**Annotation 的格式:**
注解是以"**@注释名**"在代码中存在，还可以添加一些参数值，例如：@SuppressWarnings(value="unchecked").
**Annotation 在哪里使用？**
可以附加在 package,class,method,field 等上面，相当于给他们添加了额外的辅助信息，我们还可以通过反射机制编程实现对这些元数据的访问
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596694049798-9de9a8c2-0453-4160-a9cf-9974786f3873.png#height=224&id=DiLOP&margin=%5Bobject%20Object%5D&name=image.png&originHeight=299&originWidth=641&originalType=binary∶=1&size=24928&status=done&style=none&width=481)

## 内置注解

@Override                             重写注解
@Deprecated                      不推荐使用的注解，存在更好的方式
@SuppressWarnings("all")      镇压警告注解

```java
package 注解与反射;

import java.util.ArrayList;
import java.util.List;

public class Test01 extends Object {
    @Override
    public String toString() {
        return super.toString();
    }

    @Deprecated
    public static void test01(){
        System.out.println("Deprecated");
    }

    @SuppressWarnings("all")
    public static void test02(){
        List list = new ArrayList();
    }

    public static void main(String[] args) {
        test01();
        test02();
    }
}
```

## 元注解

元注解的作用是负责注解其它注解，java 定义了 4 个标准的**meta-annotation**类型，他们被用来提供对其他 annotation 类型作说明。
这些类型和他们所支持的类在 java.lang.annotation 包中可以找到。

- @Target()      用于描述注解的使用范围
- @Retention()     表示需要什么级别保存该注释信息，用于描述注解的生命周期
  - SOURCE < CLASS < **RENTIME**
- @Documented     说明该注解将被包含在 javadoc 中
- @Inherited            说明子类可以继承父类中的注解

```java
package 注解与反射;
import java.awt.*;
import java.lang.annotation.*;

//测试元注解
@MyAnnotation
public class Test02 {

    @MyAnnotation
    public static void  test(){

    }
    public static void main(String[] args) {
        test();
    }
}

@Inherited                                                 //表示子类可以继承父类的注解
@Documented                                               //表示是否将我们的注解生成在Javadoc中
@Retention(value = RetentionPolicy.RUNTIME)               //表示我们的注解在什么阶段有效
@Target(value = {ElementType.METHOD,ElementType.TYPE})    //表示注解可以用在什么地方
 @interface MyAnnotation{

 }
```

## 自定义注解

**@interface**自定义注解，自动继承了 Java.lang.annotation.Annotation 接口

```java
package 注解与反射;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

//自定义注解
public class Test03 {
    @MyAnnotation2(age=18)
    public static void test(){

    }

    //@MyAnnotation3(value = "Java")
    @MyAnnotation3( "Java")
    public static void test2(){
    }
}

@Target({ElementType.TYPE,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation2{
    //注解的参数 参数类型 参数名() 不是方法
    String name() default "";    //默认为空，不设默认值，一定要给注解赋值
    int age();
    int id() default -1;         //默认值为-1表示不存在
    String[] schools() default {"清华大学","北京大学"};
}

@Target({ElementType.METHOD,ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation3{
    String value();
}
```

# 反射(Reflection)

## Java 反射机制概述

Java 的反射机制是指在程序运行时可以判断任意一个对象的所属类、可以构造任意一个类的对象、可以判断任意一个类所具有的成员变量以及方法、可以调用任意一个类的成员变量和方法。反射机制被视为动态语言的关键。

> 动态语言 vs 静态语言
> 1、动态语言
> 是一类在运行时可以改变其结构的语言：例如新的函数、对象、甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化。通俗点说就是在运行时代码可以根据某些条件改变自身结构。主要动态语言： Object-C、 C#、 JavaScript、 PHP、 Python、 Erlang。2、静态语言与动态语言相对应的， 运行时结构不可变的语言就是静态语言。如 Java、 C、C++。//Java 不是动态语言， 但 Java 可以称之为“准动态语言” 。 即 Java 有一定的动态性， 我们可以利用反射机制、 字节码操作获得类似动态语言的特性。Java 的动态性让编程的时候更加灵活！

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596698326001-102b75bc-456d-4430-966d-b87988400dda.png#height=118&id=GAuyU&name=image.png&originHeight=194&originWidth=1220&originalType=binary∶=1&size=114371&status=done&style=none&width=743)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596698392127-cda63049-90e7-4b8f-8dca-4f6b31c21e2a.png#height=100&id=JqlyB&name=image.png&originHeight=166&originWidth=1213&originalType=binary∶=1&size=121289&status=done&style=none&width=730)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596698470496-528f361f-b34c-477d-977e-9e90cc6e71ad.png#height=98&id=iADAG&margin=%5Bobject%20Object%5D&name=image.png&originHeight=139&originWidth=1002&originalType=binary∶=1&size=120456&status=done&style=none&width=707)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596698506408-0a03ba76-5eb8-446b-9bc2-82e58f56294e.png#height=258&id=gFJxs&name=image.png&originHeight=507&originWidth=831&originalType=binary∶=1&size=133849&status=done&style=none&width=423)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596699527238-98d47d79-f11a-43fb-9f78-8807c3d54028.png#height=220&id=jlXUG&margin=%5Bobject%20Object%5D&name=image.png&originHeight=440&originWidth=703&originalType=binary∶=1&size=212256&status=done&style=none&width=351.5)

```java
package AnnotationReflection.Reflection;

//测试什么是反射
public class Test01 {
    public static void main(String[] args) throws ClassNotFoundException {
        //通过反射获取类的Class对象
        Class c1 = Class.forName("AnnotationReflection.Reflection.User");
        System.out.println(c1);

        Class c2 = Class.forName("AnnotationReflection.Reflection.User");
        Class c3 = Class.forName("AnnotationReflection.Reflection.User");
        Class c4 = Class.forName("AnnotationReflection.Reflection.User");
        System.out.println(c2.hashCode());
        System.out.println(c3.hashCode());
        System.out.println(c4.hashCode());





    }
}
class User{
    private String name;
    private int id;
    private int age;

    public User() {
    }

    public User(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", id=" + id +
                ", age=" + age +
                '}';
    }
}
```

## 理解 Class 类并获取 Class 实例

### Class 类

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596699164002-a2d7c83c-d666-494a-968f-cc6fb743fbca.png#height=353&id=It5on&margin=%5Bobject%20Object%5D&name=image.png&originHeight=706&originWidth=1503&originalType=binary∶=1&size=400927&status=done&style=none&width=751.5)

### Class 类的常用方法

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596699488681-b8a997b8-1f8c-40d6-ba6d-d409b7037864.png#height=272&id=gSDEE&margin=%5Bobject%20Object%5D&name=image.png&originHeight=544&originWidth=1175&originalType=binary∶=1&size=472067&status=done&style=none&width=587.5)

### 获取 Class 类的实例

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596699680012-1e441bd2-0855-4555-95ed-67894ed8e5b2.png#height=264&id=STm2v&margin=%5Bobject%20Object%5D&name=image.png&originHeight=528&originWidth=1221&originalType=binary∶=1&size=257785&status=done&style=none&width=610.5)

```java
package AnnotationReflection.Reflection;
// 测试Class类的创建方式
public class Test02 {
    public static void main(String[] args) throws ClassNotFoundException {
        Person person = new Student();
        Person person1 = new Teacher();
        System.out.println(person.name);

        //方式一：通过对象获得
        Class c1 = person.getClass();
        System.out.println(c1.hashCode());
        //方式二：通过forName
        Class c2 = Class.forName("AnnotationReflection.Reflection.Student");
        System.out.println(c2.hashCode());
        //方式三： 通过类名.class
        Class c3 = Student.class;
        System.out.println(c3.hashCode());

        // 方式四：基本内置类型的包装类都有一个Type属性(作为了解)
        // 这里的对象就和上面的不一样了，这里是 Integer，上面是 Student
        // public static final Class<Integer>  TYPE = (Class<Integer>) Class.getPrimitiveClass("int");
        Class c4 = Integer.TYPE;
        System.out.println(c4);

        // 获得父类类型
        // Student 的一个父类类型(通过获得这个类,在通过这个类的Class对象去获得其它属性(例如：父类))
        Class c5 = c1.getSuperclass();
        System.out.println(c5);
    }
}

class Person{

    public String name;
    public Person() {
    }
    public Person(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "Person [name=" + name + "]";
    }
}

class Student extends Person{
    public Student() {
        this.name = "学生";
    }
}
class Teacher extends Person{
    public Teacher() {
        this.name = "老师";
    }
}
```

### 那些类型可以有 Class 对象

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596700350433-39523592-c6f1-4d71-a36d-956ff064c870.png#height=198&id=qm3ht&name=image.png&originHeight=395&originWidth=1061&originalType=binary∶=1&size=108818&status=done&style=none&width=530.5)

```java
package AnnotationReflection.Reflection;

import java.lang.annotation.ElementType;

public class Test03 {
    public static void main(String[] args) {
        Class c1 = Object.class;	    // 类
        Class c2 = Comparable.class;	// 接口
        Class c3 = String[].class;	    // 一维数组
        Class c4 = int[][].class;	    // 二维数组
        Class c5 = Override.class;	    // 注解
        Class c6 = ElementType.class;	// 枚举
        Class c7 = Integer.class;	    // 基本数据类型
        Class c8 = void.class;	        // 空类型
        Class c9 = Class.class;	        // Class本身

        System.out.println(c1);
        System.out.println(c2);
        System.out.println(c3);
        System.out.println(c4);
        System.out.println(c5);
        System.out.println(c6);
        System.out.println(c7);
        System.out.println(c8);
        System.out.println(c9);

        // 只要元素类型与维度一样，就是同一个Class
        // 同一个元素同一个类只有一个Class对象
        // 一个类只有一个Class对象
        int[] a = new int[10];
        int[] b = new int[100];
        System.out.println(a.getClass().hashCode());
        System.out.println(b.getClass().hashCode());
    }
}
//输出结果
class java.lang.Object
interface java.lang.Comparable
class [Ljava.lang.String;
class [[I
interface java.lang.Override
class java.lang.annotation.ElementType
class java.lang.Integer
void
class java.lang.Class
381259350
381259350
```

## 类的加载与 ClassLoader

### Java 内存分析

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596700646580-dfc700e8-9dd3-4f96-a1f2-221b90161375.png#height=298&id=a7vDN&margin=%5Bobject%20Object%5D&name=image.png&originHeight=515&originWidth=1164&originalType=binary∶=1&size=330974&status=done&style=none&width=674)

### 类加载过程

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596700708152-d6f415ee-b593-4bac-9cbe-a61f75d18adb.png#height=330&id=Bir8b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=533&originWidth=1079&originalType=binary∶=1&size=480334&status=done&style=none&width=669)

### 类的加载与 ClassLoader 的理解

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596700763111-27716c4e-1571-4bd3-9cbe-7b35c74ef09e.png#height=253&id=Cop1W&margin=%5Bobject%20Object%5D&name=image.png&originHeight=472&originWidth=1173&originalType=binary∶=1&size=332686&status=done&style=none&width=629)

```java
package AnnotationReflection.Reflection;

public class Test05 {
    public static void main(String[] args) {
    A a = new A();
    System.out.println(A.m);

		/*
		 * 1、加载到内存，会产生一个类对应的 Class对象
		 * 2、链接 ，链接结束后 m = 0（刚开始赋值默认值为0）
		 * 3、初始化(调用 clinit 方法，并进行合并)
		 * 		通过<clinit>(){
		 * 				System.out.println("A类静态代码块初始化");
						m = 300;
						m = 100;
		 * 			}方法初始化(拿代码进来(上面的三行代码拿进来))
		 *
		 * 		此时 m = 100
		 */
}
}

class A{
    static {
        System.out.println("A类静态代码块初始化");
        m = 300;
    }

    static int m = 100;
    public A(){
        System.out.println("A类的无参构造初始化");
    }
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596702208148-815cb7bc-7e3c-4d24-b2bb-3e54df09d11e.png#height=321&id=fvvBF&name=image.png&originHeight=520&originWidth=1207&originalType=binary∶=1&size=167420&status=done&style=none&width=746)
1、刚开始加载类时（类的数据、静态变量、静态方法、常量池、代码）
2、类加载完成立马产生一个 Class 对象（生成一个 Java.lang.Class 对象 代表 Test05 这个类、生成一个 Java.lang.Class 对象 代表 A 这个类），在加载的时候就形成了这两个对象，这两个 Class 对象就包含了这个类所有的东西
3、下面开始准备执行 main（）方法了，**此时首先 m 默认为 0（m = 0）**（**这里匹配链接阶段的准备阶段：正式为类变量(static)分配内存并设置类变量默认初始值的阶段，这些内存都将在方法区中进行分配**）
**链接阶段的 m 为 0（m = 0）**
4、new A（）在堆内存中，这个动作会产生一个 A 类新的对象（**这个对象会去找它自己(A 类)的那个 Class 类，无论创建多少个 A 类的对象，它的 Class 类只有一个**），它会指向 A 类的 Class，这时就能拿到 A 类的所有东西（它会去找 A 类的 Class（在堆内存指向），因为 Class 拥有 A 类的所有数据，然后通过这些数据，就可以给 A 类显示赋值了，然后初始化，此时初始化时会执行一个方法()方法，它会把静态代码块的初始值合并了）
5、合并静态代码块（合并 m = 300 和 m = 100），这两句话相当于重新赋值给 m（**第一次给 m 赋值为 300 第二次赋值为 100，并把前面的值被覆盖了，于是上面的** **A.m 打印出来的为 100**，它是在初始化的时候执行的（把静态代码块合并起来），通过 A 类的具体对象给它赋值，赋值完通过 初始化，这次就有一个初始值，就可以打印出来了）

### 什么时候类会初始化

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596705080253-72a23ded-dba1-4666-b569-c9dc87904065.png#height=250&id=qBFNz&margin=%5Bobject%20Object%5D&name=image.png&originHeight=499&originWidth=1211&originalType=binary∶=1&size=237407&status=done&style=none&width=605.5)

#### 类的主动引用

如果在 main（）方法中去 new 一个子类的对象（子类继承了父类），那么 JVM 会自动初始化父类
由于这里是 new 子类(Son 类)，父类没有被初始化，所以 JVM 会先自动初始化父类

```java
package AnnotationReflection.Reflection;

/**
 * 测试类什么时候会初始化
 */
public class Test05 {
    static {
        System.out.println("Main类被加载");
    }
    public static void main(String[] args) throws ClassNotFoundException {
    //        Son son = new Son();
    //        //反射也会产生主动引用
    //        Class.forName("AnnotationReflection.Reflection.Son");
        //不会产生类的引用方法
        System.out.println(Son.b);
        Son[] sons = new Son[10];

    }
}
class Father{
    static {
        System.out.println("父类被加载");
    }
}
class Son extends Father{
    static int b = 2;
    static {
        System.out.println("子类被加载");
    }
    static int m = 100;
    static final int M = 1;
}

//结果
Main类被加载
父类被加载
子类被加载
```

#### 类主动引用（反射形式）

```java
// 测试类什么时候会初始化
public class Test05 {

	static {
		System.out.println("Main类被加载");
	}

	public static void main(String[] args) throws ClassNotFoundException {
		// 1、主动引用
		// 由于这里是new子类(Son类)，父类没有被初始化，所以JVM会自动初始化父类先
//		Son son = new Son();

		// 反射也会产生主动引用
		Class.forName("com.lwm.reflection.Son");
	}
}

class Father{

	static int b = 2;

	// 静态代码块
	static {
		System.out.println("父类被加载");
	}
}

class Son extends Father{

	static {
		System.out.println("子类被加载");
		m = 300;
	}

	static int m = 100;
	static final int M = 1;
}

```

通过反射也是会产生类的主动引用，它会把所有东西加载进来(**Main 类被加载、父类被加载、子类被加载**)，这样会极大的消耗资源

#### 类的被动引用

通过子类去调用父类的静态方法或者静态变量，不会对子类产生任何影响，子类不会被加载

```java
// 测试类什么时候会初始化
public class Test05 {

	static {
		System.out.println("Main类被加载");
	}

	public static void main(String[] args) throws ClassNotFoundException {
		// 不会产生类的引用的方法

		// 通过子类去调用父类的静态方法或者静态变量，不会对子类产生任何影响，子类不会被加载
		System.out.println(Son.b);
	}
}

class Father{

	static int b = 2;
	// 静态代码块
	static {
		System.out.println("父类被加载");
	}
}

class Son extends Father{

	static {
		System.out.println("子类被加载");
		m = 300;
	}

	static int m = 100;
	static final int M = 1;
}

//输出结果
Main类被加载
父类被加载
2
```

#### 被动引用（通过数组形式）

数组占了一个空间，开辟了 5 个空间（如果没被加载说明什么都没干（此时只有 main 类被加载））

```java
// 测试类什么时候会初始化
public class Test05 {

	static {
		System.out.println("Main类被加载");
	}

	public static void main(String[] args) throws ClassNotFoundException {
		// 不会产生类的引用的方法

        // 通过一个数组
		// 数组占了一个空间，开辟了5个空间（如果没被加载说明什么都没干（此时只有main类被加载））
		Son[] array = new Son[5];
	}
}

class Father{

	static int b = 2;

	// 静态代码块
	static {
		System.out.println("父类被加载");
	}
}

class Son extends Father{

	static {
		System.out.println("子类被加载");
		m = 300;
	}

	static int m = 100;
	static final int M = 1;
}

```

##### 为什么会加载 Main 类呢？

因为当虚拟机启动，就会先初始化 main 方法所在的类，然后才执行 **Son[] array = new Son[5];这行代码，这时没有任何类被加载**，这行代码只是一个数组，它只是一个名字和一片空间而已

#### 被动引用（通过调用常量形式）

```java
// 测试类什么时候会初始化
public class Test05 {

	static {
		System.out.println("Main类被加载");
	}

	public static void main(String[] args) throws ClassNotFoundException {
		// 不会产生类的引用的方法

		// 调用子类的常量（常量并不会引起父类和子类的初始化）
		System.out.println(Son.M);
	}
}

class Father{

	static int b = 2;

	// 静态代码块
	static {
		System.out.println("父类被加载");
	}
}

class Son extends Father{

	static {
		System.out.println("子类被加载");
		m = 300;
	}

	static int m = 100;
	static final int M = 1;
}
//
Main类被加载
1

```

所有的常量和类的静态变量都是在链接阶段就被赋了一个值，在链接阶段就做了，初始化的时候就已经存在了

### 类加载器

```java
package AnnotationReflection.Reflection;

public class Test06 {
    public static void main(String[] args) throws ClassNotFoundException {
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);

        ClassLoader parent = systemClassLoader.getParent();
        System.out.println(parent);

        ClassLoader parent1 = parent.getParent();
        System.out.println(parent1);

        //测试当前类是哪个加载器加载的
        ClassLoader classLoader = Class.forName("AnnotationReflection.Reflection.Test06").getClassLoader();
        System.out.println(classLoader);

        //测试JDK内置的类是谁加载的
        ClassLoader classLoader1 = Class.forName("java.lang.Object").getClassLoader();
        System.out.println(classLoader1);

        //如何获取系统类加载器可以加载的路径
        System.out.println(System.getProperty("java.class.path"));

        //双亲委派机制：跟加载器和扩展加载器中的jar包有类，不会使用自己定义的
        // java.lang.String-->
    }
}
```

## 创建运行时类对象以及获取运行时类的完整结构

```java
package AnnotationReflection.Reflection;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class Test07 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, NoSuchMethodException {
        Class c1 = Class.forName("AnnotationReflection.Reflection.User");
        System.out.println(c1.getName());               //包名+类名
        System.out.println(c1.getSimpleName());         //仅类名

        User user = new User();
        c1 = user.getClass();
        System.out.println(c1.getName());               //包名+类名
        System.out.println(c1.getSimpleName());         //仅类名
        Field[] fields = c1.getFields();        //获得public属性
        fields = c1.getDeclaredFields();        //获得全部属性
        for(Field field: fields){
            System.out.println(field);
        }

        Field name = c1.getDeclaredField("name");
        System.out.println(name);

        Method[] methods = c1.getMethods(); // 全部方法
        for(Method method: methods){
            System.out.println(method);
        }
        System.out.println("===================");
        Method[] declaredMethods = c1.getDeclaredMethods(); //本类的所有方法：包括私有的方法
        for(Method declaredMethod: declaredMethods){
            System.out.println(declaredMethod);
        }
        System.out.println("===================");
        Method getName = c1.getMethod("getName", null);
        Method setName = c1.getMethod("setName",String.class);
        System.out.println(getName);
        System.out.println(setName);
        System.out.println("===================");
        Constructor constructor = c1.getConstructor();
        Constructor declaredConstructor = c1.getDeclaredConstructor(String.class,int.class,int.class);
        System.out.println(constructor);
        System.out.println(declaredConstructor);
        System.out.println("===================");

    }
}
```

## 调用运行时类的指定结构

### 动态创建对象执行方法

通过 c1.newInstance()动态创建对象执行方法。

```java
package AnnotationReflection.Reflection;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class Test09 {
    public static void main(String[] args) throws Exception {
        // 获得class对象
        Class c1 = Class.forName("AnnotationReflection.Reflection.User");

//        User user= (User)c1.newInstance();  //默认调用无参构造器
//        System.out.println(user);

        // 反射方式通过构造器创建对象
        Constructor constructor = c1.getDeclaredConstructor(String.class, int.class, int.class);
        User user1 = (User)constructor.newInstance("青梅",18,18);
        System.out.println(user1);

        //通过反射调用方法
        User user3 = (User)c1.newInstance();
        Method setName = c1.getDeclaredMethod("setName", String.class);
        setName.invoke(user3,"青梅");     //对象+方法参数值
        System.out.println(user3.getName());

        // 通过反射操作属性
        User user4 = (User)c1.newInstance();
        Field name = c1.getDeclaredField("name");
        name.setAccessible(true);       //不能直接操作私有属性，需要关闭程序的安全检测
        name.set(user4,"青梅2");
        System.out.println(user4.getName());

    }
}

```

### 性能比较

```java
package AnnotationReflection.Reflection;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

//分析性能问题
public class Test10 {
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException {
        test();
        test2();
        test3();
    }
    //普通方式调用
    public static void test(){
        User user = new User();
        long start = System.currentTimeMillis();
        for (int i = 0; i < 1000000000; i++) {
            user.getName();
        }
        long end = System.currentTimeMillis();
        System.out.println("普通方式10亿次:" + (end-start) + " ms");

    }
    //反射方式调用
    public static void test2() throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        User user = new User();
        Class c1 = user.getClass();
        Method getName = c1.getMethod("getName", null);
        long start = System.currentTimeMillis();
        for (int i = 0; i < 1000000000; i++) {
            getName.invoke(user,null);
        }
        long end = System.currentTimeMillis();
        System.out.println("反射方式10亿次:" + (end-start) + " ms");
    }
    //反射方式调用   关闭检测
    public static void test3() throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        User user = new User();
        Class c1 = user.getClass();
        Method getName = c1.getMethod("getName", null);
        getName.setAccessible(true);
        long start = System.currentTimeMillis();
        for (int i = 0; i < 1000000000; i++) {
            getName.invoke(user,null);
        }
        long end = System.currentTimeMillis();
        System.out.println("关闭检测后反射方式10亿次:" + (end-start) + "ms");
    }

}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596803093544-373965e3-a760-43d4-8c7f-8bc46ce6dfef.png#height=62&id=z7eDr&name=image.png&originHeight=124&originWidth=405&originalType=binary∶=1&size=15204&status=done&style=none&width=202.5)

### 获取泛型信息

```java
package AnnotationReflection.Reflection;

import java.lang.reflect.Method;
import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
import java.util.List;
import java.util.Map;

public class Test11 {
    public static void test01(Map<String,User> map, List<User> list){
        System.out.println("test01");
    }
    public static Map<String,User>  test02(){
        System.out.println("test01");
        return null;
    }

    public static void main(String[] args) throws NoSuchMethodException {
        Method method = Test11.class.getMethod("test01", Map.class, List.class);
        Type[] genericParameterTypes = method.getGenericParameterTypes();
        for(Type genericParameterType:genericParameterTypes){
            System.out.println("#"+ genericParameterType);
            if(genericParameterType instanceof ParameterizedType){
                Type[] actualTypeArguments = ((ParameterizedType) genericParameterType).getActualTypeArguments();
                for (Type actualTypeArgument:actualTypeArguments){
                    System.out.println(actualTypeArgument);
                }
            }
        }

        System.out.println("=================");
        Method method2 = Test11.class.getMethod("test02", null);
        Type genericReturnType = method2.getGenericReturnType();
        if(genericReturnType instanceof ParameterizedType){
            Type[] actualTypeArguments = ((ParameterizedType)genericReturnType  ).getActualTypeArguments();
            for (Type actualTypeArgument:actualTypeArguments){
                System.out.println(actualTypeArgument);
            }
        }

    }
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596804549571-9a471e62-c374-4e3a-b977-251079e665a5.png#height=141&id=ZgbGw&margin=%5Bobject%20Object%5D&name=image.png&originHeight=281&originWidth=918&originalType=binary∶=1&size=27991&status=done&style=none&width=459)

### 获取注解信息

```java
package AnnotationReflection.Reflection;

import java.lang.annotation.*;
import java.lang.reflect.Field;

//练习反射操作注解
public interface Test {


    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
        Class c1 = Class.forName("AnnotationReflection.Reflection.Student2");
        //通过反射获得注解
        Annotation[] annotations = c1.getAnnotations();
        for(Annotation annotation:annotations){
            System.out.println(annotation);
        }

        //获得注解的value的值
        TableStudent tableStudent = (TableStudent) c1.getAnnotation(TableStudent.class);
        String value = tableStudent.value();
        System.out.println(value);

        //获得类指定的注解
        System.out.println("----------------");
        Field f1 = c1.getDeclaredField("id");
        FieldStudent annotation1 = f1.getAnnotation(FieldStudent.class);
        System.out.println(annotation1.columnName());
        System.out.println(annotation1.type());
        System.out.println(annotation1.length());
        System.out.println("----------------");
        Field f2 = c1.getDeclaredField("age");
        FieldStudent annotation2 = f2.getAnnotation(FieldStudent.class);
        System.out.println(annotation2.columnName());
        System.out.println(annotation2.type());
        System.out.println(annotation2.length());
        System.out.println("----------------");
        Field f3 = c1.getDeclaredField("name");
        FieldStudent annotation3 = f3.getAnnotation(FieldStudent.class);
        System.out.println(annotation3.columnName());
        System.out.println(annotation3.type());
        System.out.println(annotation3.length());

    }
}

@TableStudent("db_student")
class Student2{
    @FieldStudent(columnName = "db_id",type = "int",length = 10)
    private int id;
    @FieldStudent(columnName = "db_age",type = "int",length = 10)
    private int age;
    @FieldStudent(columnName = "db_name",type = "varchar",length = 3)
    private String name;

    public Student2() {
    }

    public Student2(int id, int age, String name) {
        this.id = id;
        this.age = age;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

//类名的注解
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface TableStudent{
    String value();
}

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
//属性的注解
@interface FieldStudent{
    String columnName();
    String type();
    int length();
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596806422225-0806af70-384d-4bcd-8bbb-b6a8e92e72b0.png#height=241&id=xvMvO&name=image.png&originHeight=481&originWidth=921&originalType=binary∶=1&size=22642&status=done&style=none&width=460.5)
