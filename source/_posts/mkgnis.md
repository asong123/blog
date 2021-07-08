---
title: Spring5学习笔记
author: 阿松
date: '2021-07-05 15:26:08 +0800'
top: true
coverImg: https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625470123698-d2c29622-abbf-4042-b3a6-46d13f848425.jpeg#clientId=u144c994e-9948-4&from=paste&height=220&id=u1a5ee6b2&margin=%5Bobject%20Object%5D&name=v2-c7d94fabd9e1251acb3bba73458e3813_720w.jpg&originHeight=330&originWidth=780&originalType=binary%E2%88%B6=1&size=17036&status=done&style=none&taskId=u06eb739c-b8a1-480f-bd25-9b74cdab4ad&width=521
img: https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471469101-9ac80fb7-a32a-41fa-8ed3-85dc36791cd4.png#clientId=ue6922a46-94b3-4&from=paste&height=375&id=u553607f9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=750&originWidth=1449&originalType=binary%E2%88%B6=1&size=270746&status=done&style=none&taskId=ufd3f5eae-1736-41c9-8255-72f6add5160&width=724.5
hide: false
cover: true
toc: false
mathjax: false
summary: Spring ——> 春天，为开源软件带来了春天
categories: Spring
tags:
  - Spring
  - java
  - 后端
abbrlink: 59063
---
![v2-c7d94fabd9e1251acb3bba73458e3813_720w.jpg](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625470123698-d2c29622-abbf-4042-b3a6-46d13f848425.jpeg#clientId=u144c994e-9948-4&from=paste&height=220&id=u1a5ee6b2&margin=%5Bobject%20Object%5D&name=v2-c7d94fabd9e1251acb3bba73458e3813_720w.jpg&originHeight=330&originWidth=780&originalType=binary∶=1&size=17036&status=done&style=none&taskId=u06eb739c-b8a1-480f-bd25-9b74cdab4ad&width=521)

# Spring5

## 1.、Spring

### 1.1、简介

- Spring ——> 春天，为开源软件带来了春天
- 2002，首次推出了 Spring 框架的雏形：interface21 框架！
- Spring 框架以 interface21 框架为基础，经过重新设计，并不断丰富其内涵，于 2004 年 3 月 24 日发布了 1.0 正式版
- Spring 的理念：使用现有的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架！
- SSH：Struct2 + Spring + Hibernate（全自动持久化框架）！
- SSM：SpringMVC + Spring + MyBatis（半自动持久化框架，可自定义性质更强）！

spring 官网： [https://spring.io/projects/spring-framework#overview](https://spring.io/projects/spring-framework#overview)

官方下载： [https://repo.spring.io/release/org/springframework/spring/](https://repo.spring.io/release/org/springframework/spring/)

GitHub： [https://github.com/spring-projects/spring-framework](https://github.com/spring-projects/spring-framework)

Spring Web MVC： [spring-webmvc 最新版](https://mvnrepository.com/artifact/org.springframework/spring-webmvc/5.2.7.RELEASE)

Spring Web MVC 和 Spring-JDBC 的 pom 配置文件：

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.7.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.7.RELEASE</version>
</dependency>
```

### 1.2 优点

- Spring 是一个开源的免费的框架（容器）！
- Spring 是一个轻量级的、非入侵式的框架！
- 控制反转（IoC），面向切面编程（AOP）
- 支持事务的处理，对框架整合的支持！（几乎市面上所有热门框架都能整合进去）！

=== 总结一句话：Spring 就是一个轻量级的控制反转（IoC）和面向切面编程（AOP）的框架！ ===

### 1.3 组成

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471469101-9ac80fb7-a32a-41fa-8ed3-85dc36791cd4.png#clientId=ue6922a46-94b3-4&from=paste&height=375&id=u553607f9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=750&originWidth=1449&originalType=binary∶=1&size=270746&status=done&style=none&taskId=ufd3f5eae-1736-41c9-8255-72f6add5160&width=724.5)

### 1.4、扩展

现代化的 java 开发 -> 基于 Spring 的开发！

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471489413-df72e28a-2468-4e34-9a71-1fb0436b433f.png#clientId=ue6922a46-94b3-4&from=paste&height=284&id=u0f064c2e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=567&originWidth=1445&originalType=binary∶=1&size=664695&status=done&style=none&taskId=u4176e656-ae09-409b-8999-5ad3ed67e44&width=722.5)

- Spring Boot

  - 一个快速开发的脚手架
  - 基于 SpringBoot 可以快速开发单个微服务
  - 约定大于配置！

- Spring Cloud
  - SpringCloud 是基于 SpringBoot 实现的！

因为现在大多数公司都在使用 SpringBoot 进行快速开发，学习 SpringBoot 的前提，需要完全掌握 Spring 及 SpringMVC！承上启下的作用！

## 2、IoC（控制反转）理论推导

**传统**的调用

1.  UserDao

```java
package dao;
public interface UserDao {
	void getUser();
}
```

2.  UserDaoImp

```java
package dao;
public class UserDaoImpl implements UserDao{
	public void getUser() {
		System.out.println("默认获取用户数据");
	}
}
```

3.  UserSevice

```java
package Service;
public interface UserService {
	void getUser();
}
```

4.  UserServiceImp

```java
package Service;
import dao.UserDao;
import dao.UserDaoImpl;

public class UserServiceImpl implements UserService{
		UserDao userDao = new UserDaoImpl();
		public void getUser(){
			userDao.getUser();
		}
}
```

测试

```java
package holle0;
import Service.UserService;
import Service.UserServiceImpl;

public class MyTest0 {
	public static void main(String[] args) {
		// 用户实际调用的是业务层，dao层他们不需要接触
		UserService userService = new UserServiceImpl();
		userService.getUser();
	}
}
```

在我们之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改原代码！如果程序代码量十分大，修改一次的成本代价十分昂贵！

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471525605-dfe1b994-8532-43a8-8fd3-c810b4a9ad52.png#clientId=ue6922a46-94b3-4&from=paste&height=547&id=u64f67cf9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1094&originWidth=887&originalType=binary∶=1&size=114582&status=done&style=none&taskId=uf1e21627-30d9-4320-8487-04c4391dad0&width=443.5)
**改良：**我们使用一个 Set 接口实现。已经发生了革命性的变化！

```java
//在Service层的实现类(UserServiceImpl)增加一个Set()方法
//利用set动态实现值的注入！
//DAO层并不写死固定调用哪一个UserDao的实现类
//而是通过Service层调用方法设置实现类！
private UserDao userDao;
public void setUserDao(UserDao userDao){
    this.userDao = userDao;
}
```

set() 方法实际上是动态改变了 UserDao userDao 的 初始化部分（**new UserDaoImpl()**）

测试中加上

```java
((UserServiceImpl)userService).setUserDao(new UserDaoImpl());
```

- 之前，程序是主动创建对象！**控制权在程序猿手上**！
- 使用了 set 注入后，程序不再具有主动性，而是变成了被动的接受对象！（**主动权在客户手上**）

本质上解决了问题，程序员不用再去管理对象的创建

系统的耦合性大大降低，可以更专注在业务的实现上

这是 IoC（控制反转）的原型，反转(理解)：主动权交给了用户

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471548672-390be083-84e3-4789-b6a8-61b4d2bb6bae.png#clientId=ue6922a46-94b3-4&from=paste&height=544&id=u68f4844a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1087&originWidth=986&originalType=binary∶=1&size=121755&status=done&style=none&taskId=u875cbec4-c3b4-47b5-9578-c161d15fdb5&width=493)

### IoC 本质

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471698388-279f845b-d9fd-4c1c-aa04-9178f7d2e879.png#clientId=u65c68b31-85a1-4&from=paste&height=97&id=ubbca603d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=194&originWidth=1433&originalType=binary∶=1&size=450005&status=done&style=none&taskId=u65cdfe48-8874-4914-ae85-e831f02b4c9&width=716.5)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471724309-5072877d-ff16-4f20-aaae-47fcd9335689.png#clientId=u65c68b31-85a1-4&from=paste&height=216&id=udae5b455&margin=%5Bobject%20Object%5D&name=image.png&originHeight=431&originWidth=1427&originalType=binary∶=1&size=373766&status=done&style=none&taskId=ucefa5faf-a866-4671-b89c-558a37831fe&width=713.5)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471762487-5054c138-9bdc-4529-963b-70a06aa8918c.png#clientId=u65c68b31-85a1-4&from=paste&height=643&id=u82ca1a76&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1285&originWidth=1447&originalType=binary∶=1&size=537847&status=done&style=none&taskId=ub21bca2a-1dd7-4539-b6bb-139c888d2f2&width=723.5)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471787767-b51f0885-276f-4a0a-8883-835ff617f080.png#clientId=u65c68b31-85a1-4&from=paste&height=108&id=u1868c399&margin=%5Bobject%20Object%5D&name=image.png&originHeight=216&originWidth=1444&originalType=binary∶=1&size=476060&status=done&style=none&taskId=ud4a40ca1-31a7-4ffb-a4ee-9f5fa582c1e&width=722)

## 3、HolleSpring

在父模块中导入 jar 包

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-webmvc</artifactId>
	<version>5.2.7.RELEASE</version>
</dependency>
```

pojo 的 Hello.java

```java
package pojo;

public class Hello {

	private String str;

	public String getStr() {
		return str;
	}

	public void setStr(String str) {
		this.str = str;
	}

	@Override
	public String toString() {
		return "Holle [str=" + str + "]";
	}
}
```

在 resource 里面的 xml 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--在Spring中创建对象，在Spring这些都称为bean
    	类型 变量名 = new 类型();
    	Holle holle = new Holle();

    	bean = 对象(holle)
    	id = 变量名(holle)
    	class = new的对象(new Holle();)
    	property 相当于给对象中的属性设值,让str="Spring"
    -->

    <bean id="hello" class="pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>
</beans>
```

测试类 MyTest

```java
package holle1;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import pojo.Hello;

public class MyTest {

	public static void main(String[] args) {
		//获取Spring的上下文对象
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		//我们的对象下能在都在spring·中管理了，我们要使用，直接取出来就可以了
		Hello holle = (Hello) context.getBean("hello");
		System.out.println(holle.toString());
	}

}
```

核心用 set 注入，所以必须要有下面的 se()方法

```java
//Hello类
public void setStr(String str) {
		this.str = str;
	}
```

**思考：**

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471815422-ae6ce201-a69e-466e-bd47-7f699c396eea.png#clientId=u65c68b31-85a1-4&from=paste&height=428&id=ue193d403&margin=%5Bobject%20Object%5D&name=image.png&originHeight=856&originWidth=1431&originalType=binary∶=1&size=1137501&status=done&style=none&taskId=u9f574879-54f0-4587-a3d6-b3938d958c7&width=715.5)
IoC：对象由 Spring 来创建，管理，装配！

**弹幕评论里面的理解：**

原来这套程序是：你写好菜单买好菜，客人来了自己把菜炒好招待，就相当于你请人吃饭
现在这套程序是：你告诉楼下餐厅，你要哪些菜，客人来的时候，餐厅把做好的你需要的菜送上来
IoC：炒菜这件事，不再由你自己来做，而是委托给了第三方\_\_餐厅来做

此时的区别就是，如果我还需要做其他的菜，我不需要自己搞菜谱买材料再做好，而是告诉餐厅，我要什么菜，什么时候要，你做好送来

.

在前面第一个 module 试试引入 Spring

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userDaomSql" class="dao.UserDaoMysqlImpl"></bean>

    <bean id="userServiceImpl" class="service.UserServiceImp">
        <!--ref引用spring中已经创建很好的对象-->
        <!--value是一个具体的值,基本数据类型-->
        <property name="userDao" ref="userDaomSql"/>
    </bean>

</beans>
```

第一个 module 改良后测试

```java
package holle0;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import service.UserServiceImpl;

public class MyTest0 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		UserServiceImpl userServiceImpl = (UserServiceImpl) context.getBean("userServiceImpl");
		userServiceImpl.getUser();
	}
}
```

**总结：**

所有的类都要装配的 beans.xml 里面；

所有的 bean 都要通过容器去取；

容器里面取得的 bean，拿出来就是一个对象，用对象调用方法即可；

## 4、IoC 创建对象的方式

1. 使用无参构造创建对象，默认。
2. 使用有参构造（如下）

下标赋值

index 指的是有参构造中参数的下标，下标从 0 开始;

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="pojo.User">
        <constructor-arg index="0" value="chen"/>
    </bean>
</beans>
```

类型赋值（不建议使用）

```xml
<bean id="user" class="pojo.User">
    <constructor-arg type="java.lang.String" value="kuang"/>
</bean>
```

直接通过参数名（掌握）

```xml
<bean id="user" class="pojo.User">
    <constructor-arg name="name" value="kuang"></constructor-arg>
</bean>
<!-- 比如参数名是name，则有name="具体值" -->
```

注册 bean 之后就对象的初始化了（**类似 new 类名()**）

弹幕评论：

name 方式还需要无参构造和 set 方法,index 和 type 只需要有参构造

就算是 new 两个对象，也是只有一个实例（**单例模式：全局唯一**）

```java
User user = (User) context.getBean("user");
User user2 = (User) context.getBean("user");
system.out.println(user == user2)//结果为true
```

总结：在配置文件加载的时候，容器(< bean>)中管理的对象就已经初始化了

## 5、Spring 配置

### 5.1、别名

```xml
<bean id="user" class="pojo.User">
    <constructor-arg name="name" value="chen"></constructor-arg>
</bean>

<alias name="user" alias="userLove"/>
<!-- 使用时
	User user2 = (User) context.getBean("userLove");
-->
```

### 5.2、Bean 的配置

```xml
<!--id：bean的唯一标识符，也就是相当于我们学的对象名
class：bean对象所对应的会限定名：包名+类型
name：也是别名，而且name可以同时取多个别名 -->
<bean id="user" class="pojo.User" name="u1 u2,u3;u4">
    <property name="name" value="chen"/>
</bean>
<!-- 使用时
	User user2 = (User) context.getBean("u1");
-->
```

### 5.3、import

import 一般用于团队开发使用，它可以将多个配置文件，导入合并为一个

假设，现在项目中有多个人开发，这三个人复制不同的类开发，不同的类需要注册在不同的 bean 中，我们可以利
用 import 将所有人的 beans.xml 合并为一个总的！

- 张三(beans.xm1)
- 李四(beans2.xm1)
- 王五(beans3.xm1)
- applicationContext.xml

```xml
<import resource="beans.xm1"/>
<import resource="beans2.xml"/>
<import resource="beans3.xm1"/>
```

**使用的时候，直接使用总的配置就可以了**

弹幕评论：

按照在总的 xml 中的导入顺序来进行创建，后导入的会重写先导入的，最终实例化的对象会是后导入 xml 中的那个

## 6、依赖注入（DI）

### 6.1、构造器注入

第 4 点有提到

### 6.2、set 方式注入【重点】

依赖注入：set 注入！

- 依赖：bean 对象的创建依赖于容器
- 注入：bean 对象中的所有属性，由容器来注入

【环境搭建】

1.  复杂类型
    Address 类
2.  真实测试对象
    Student 类
3.  beans.xml
4.  测试
    MyTest3

Student 类

```java
package pojo;

import java.util.*;
@Get
@Set
public class Student {
//别忘了写get和set方法（用lombok注解也行）
    private String name;
    private Address address;

    private String[] books;
    private List<String> hobbies;

    private Map<String, String> card;
    private Set<String> game;

    private Properties infor;
    private String wife;

    @Override
    public String toString() {
        return "Student{" +"\n"+
                "name='" + name + '\'' +"\n"+
                ", address=" + address.toString() +"\n"+
                ", books=" + Arrays.toString(books) +"\n"+
                ", hobbies=" + hobbies +"\n"+
                ", card=" + card +"\n"+
                ", game=" + game +"\n"+
                ", infor=" + infor +"\n"+
                ", wife='" + wife + '\'' +"\n"+
                '}';
    }
}
```

Address 类

```java
package pojo;

public class Address {

    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}
```

beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="address" class="pojo.Address">
		<property name="address" value="address你好" />
	</bean>

	<bean id="student" class="pojo.Student">
		<!--第一种，普通值注入 -->
		<property name="name" value="name你好" />
		<!--第二种，ref注入 -->
		<property name="address" ref="address" />

		<!--数组注入 -->
		<property name="books">
			<array>
				<value>三国</value>
				<value>西游</value>
				<value>水浒</value>
			</array>
		</property>

		<!--list列表注入 -->
		<property name="hobbies">
			<list>
				<value>唱</value>
				<value>跳</value>
				<value>rap</value>
				<value>篮球</value>
			</list>
		</property>

		<!--map键值对注入 -->
		<property name="card">
			<map>
				<entry key="username" value="root" />
				<entry key="password" value="root" />
			</map>
		</property>

		<!--set(可去重)注入 -->
		<property name="game">
			<set>
				<value>wangzhe</value>
				<value>lol</value>
				<value>galname</value>
			</set>
		</property>

		<!--空指针null注入 -->
		<property name="wife">
			<null></null>
		</property>

		<!--properties常量注入 -->
		<property name="infor">
			<props>
				<prop key="id">20200802</prop>
				<prop key="name">cbh</prop>
			</props>
		</property>
	</bean>
</beans>
```

MyTest3

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import pojo.Student;

public class MyTest3 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		Student stu = (Student) context.getBean("student");
		System.out.println(stu.toString());
	}
}
```

### 6.3、拓展注入

官方文档位置

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471878381-057df3da-65e9-41cf-8804-c9d387f0fcf3.png#clientId=u65c68b31-85a1-4&from=paste&height=328&id=u58d619ea&margin=%5Bobject%20Object%5D&name=image.png&originHeight=656&originWidth=1427&originalType=binary∶=1&size=326070&status=done&style=none&taskId=uda825a33-9e6c-4d24-ac9a-4ca18afea94&width=713.5)

pojo 增加 User 类

```java
package pojo;

public class User {
    private String name;
    private int id;
	public User() {

	}
	public User(String name, int id) {
		super();
		this.name = name;
		this.id = id;
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
	@Override
	public String toString() {
		return "User [name=" + name + ", id=" + id + "]";
	}
}
```

注意： beans 里面加上这下面两行

使用 p 和 c 命名空间需要导入 xml 约束

xmlns:p=“http://www.springframework.org/schema/p”
xmlns:c=“http://www.springframework.org/schema/c”

```xml
?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入/set注入，可以直接注入属性的值-》property-->
    <bean id="user" class="pojo.User" p:name="cxk" p:id="20" >
    </bean>

    <!--c命名空间，通过构造器注入，需要写入有参和无参构造方法-》construct-args-->
    <bean id="user2" class="pojo.User" c:name="cbh" c:id="22"></bean>
</beans>
```

测试

```java
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
User user = context.getBean("user",User.class);//确定class对象，就不用再强转了
System.out.println(user.toString());
```

### 6.4、Bean 作用域

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471904621-7ed632de-12df-40e9-a3b9-ae6b4a446088.png#clientId=u65c68b31-85a1-4&from=paste&height=422&id=u6d12e90f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=843&originWidth=1424&originalType=binary∶=1&size=421622&status=done&style=none&taskId=u236a3f01-9665-409c-89c8-6110309854a&width=712)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471928095-937427a8-29ae-435a-b602-df741f768463.png#clientId=u65c68b31-85a1-4&from=paste&height=377&id=u79de32bf&margin=%5Bobject%20Object%5D&name=image.png&originHeight=753&originWidth=1427&originalType=binary∶=1&size=374384&status=done&style=none&taskId=ubdb4b1f9-53e8-4604-a7d3-3d0df1261d0&width=713.5)

1.  单例模式（默认）

```xml
<bean id="user2" class="pojo.User" c:name="cxk" c:age="19" scope="singleton"></bean>
1
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625471985296-9123fd81-1e81-4c8e-beba-1596e15f352a.png#clientId=u65c68b31-85a1-4&from=paste&height=331&id=u44aa8f75&margin=%5Bobject%20Object%5D&name=image.png&originHeight=662&originWidth=1438&originalType=binary∶=1&size=310107&status=done&style=none&taskId=u3910b108-eea1-4fb9-bf65-538b0106af5&width=719)
弹幕评论：单例模式是把对象放在 pool 中，需要再取出来，使用的都是同一个对象实例

1.  原型模式: 每次从容器中 get 的时候，都产生一个新对象！

```xml
<bean id="user2" class="pojo.User" c:name="cxk" c:age="19" scope="prototype"></bean>
1
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472006517-1d85d479-1c63-4354-8b85-3b700f0423e2.png#clientId=u65c68b31-85a1-4&from=paste&height=330&id=uafc03b06&margin=%5Bobject%20Object%5D&name=image.png&originHeight=659&originWidth=1421&originalType=binary∶=1&size=290775&status=done&style=none&taskId=ua3770b8a-768b-48a4-8ed9-aba7346dfd7&width=710.5)

1. 其余的 request、session、application 这些只能在 web 开放中使用！

## 7、Bean 的自动装配

- 自动装配是 Spring 满足 bean 依赖的一种方式
- Spring 会在上下文自动寻找，并自动给 bean 装配属性

在 Spring 中有三种装配的方式

1.  在 xml 中显示配置
2.  在 java 中显示配置
3.  隐式的自动装配 bean 【重要】
4.  环境搭建：一个人有两个宠物
5.  byType 自动装配：byType 会自动查找，和自己对象 set 方法参数的类型相同的 bean
    保证所有的 class 唯一(类为全局唯一)
6.  byName 自动装配：byName 会自动查找，和自己对象 set 对应的值对应的 id
    保证所有 id 唯一，并且和 set 注入的值一致

```xml
<!-- 找不到id和多个相同class -->
<bean id="cat1" class="pojo.Cat"/>
<bean id="cat2" class="pojo.Cat"/>
<!-- 找不到 id=cat，且有两个Cat -->
```

### 7.1 测试：自动装配

pojo 的 Cat 类

```java
public class Cat {
    public void shut(){
        System.out.println("miao");
    }
}
```

pojo 的 Dog 类

```java
public class Dog {

    public void shut(){
        System.out.println("wow");
    }

}
```

pojo 的 People 类

```java
package pojo;
public class People {

    private Cat cat;
    private Dog dog;
    private String name;

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "People{" +
                "cat=" + cat +
                ", dog=" + dog +
                ", name='" + name + '\'' +
                '}';
    }
}
```

xml 配置 -> byType 自动装配

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="cat" class="pojo.Cat"/>
    <bean id="dog" class="pojo.Dog"/>

    <!--byType会在容器自动查找，和自己对象属性相同的bean
		例如，Dog dog; 那么就会查找pojo的Dog类，再进行自动装配
	-->
    <bean id="people" class="pojo.People" autowire="byType">
        <property name="name" value="cbh"></property>
    </bean>

</beans>
```

xml 配置 -> byName 自动装配

```xml
<bean id="cat" class="pojo.Cat"/>
<bean id="dog" class="pojo.Dog"/>
<!--byname会在容器自动查找，和自己对象set方法的set后面的值对应的id
  例如:setDog()，取set后面的字符作为id，则要id = dog 才可以进行自动装配

 -->
<bean id="people" class="pojo.People" autowire="byName">
	<property name="name" value="cbh"></property>
</bean>
```

弹幕评论：byName 只能取到小写，大写取不到

### 7.2、使用注解实现自动装配

jdk1.5 支持的注解，spring2.5 支持的注解

The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML.（翻译：基于注释的配置的引入提出了一个问题，即这种方法是否比 XML“更好”）

1. 导入 context 约束

**xmlns:context="**[**http://www.springframework.org/schema/context**](http://www.springframework.org/schema/context)**"**

1. 配置注解的支持：< context:annotation-config/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
</beans>
```

#### 7.2.1、[@Autowired ](/Autowired)

**默认是 byType 方式，如果匹配不上，就会 byName**

在属性上个使用，也可以在 set 上使用

我们可以不用编写 set 方法了，前提是自动装配的属性在 Spring 容器里，且要符合 ByName 自动装配

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

> [@Nullable ](/Nullable) 字段标记了这个注解，说明该字段可以为空
>
> public name([@Nullable ](/Nullable) String name){
>
> }

```java
//源码
public @interface Autowired {
	boolean required() default true;
}
```

如果定义了 Autowire 的 require 属性为 false，说明这个对象可以为 null，否则不允许为空（false 表示找不到装配，不抛出异常）

#### 7.2.2、@Autowired+[@Qualifier ](/Qualifier)

**@Autowired 不能唯一装配时，需要@Autowired+**[**@Qualifier **](/Qualifier)\*\* \*\*

如果[@Autowired 自动装配环境比较复杂。自动装配无法通过一个注解完成的时候，可以使用@Qualifier(value ](/Autowired自动装配环境比较复杂。自动装配无法通过一个注解完成的时候，可以使用@Qualifier(value) = “dog”)去配合使用，指定一个唯一的 id 对象

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog")
    private Dog dog;
    private String name;
}
```

弹幕评论：

如果 xml 文件中同一个对象被多个 bean 使用，Autowired 无法按类型找到，可以用@Qualifier 指定 id 查找

#### 7.2.3、[@Resource ](/Resource)

**默认是 byName 方式，如果匹配不上，就会 byType**

```java
public class People {
    Resource(name="cat")
    private Cat cat;
    Resource(name="dog")
    private Dog dog;
    private String name;
}
```

弹幕评论：

Autowired 是 byType，@Autowired+[@Qualifier ](/Qualifier) = byType || byName

Autowired 是先 byteType,如果唯一則注入，否则 byName 查找。resource 是先 byname,不符合再继续 byType

#### 区别：

@Resource 和@Autowired 的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired 通过 byType 的方式实现，而且必须要求这个对象存在！【常用】
- @Resource 默认通过 byname 的方式实现，如果找不到名字，则通过 byType 实现！如果两个都找不到的情况下，就报错！【常用】
- 执行顺序不同：@Autowired 通过 byType 的方式实现。@Resource 默认通过 byname 的方式实现

## 8、使用注解开发

在 spring4 之后，使用注解开发，必须要保证 aop 包的导入
![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472040975-16f26b51-4e5d-402d-9197-1f4070053ed9.png#clientId=u65c68b31-85a1-4&from=paste&height=274&id=ua663f32d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=548&originWidth=846&originalType=binary∶=1&size=196088&status=done&style=none&taskId=u32f1f35d-08fe-466a-ac85-5abaf88a5a3&width=423)
使用注解需要导入 contex 的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

### 8.1、bean

弹幕评论：
有了< context:component-scan>，另一个< context:annotation-config/>标签可以移除掉，因为已经被包含进去了。

```xml
<!--指定要扫描的包，这个包下面的注解才会生效
	别只扫一个com.kuang.pojo包-->
<context:component-scan base-package="com.kuang"/>
<context:annotation-config/>
```

```java
//@Component 组件
//等价于<bean id="user" classs"pojo.User"/>
@Component
public class User {
     public String name ="秦疆";
}
```

### 8.2、属性如何注入[@value ](/value)

```java
@Component
public class User {
    //相当于<property name="name" value="kuangshen"/>
    @value("kuangshen")
    public String name;

    //也可以放在set方法上面
    //@value("kuangshen")
    public void setName(String name) {
        this.name = name;
    }
}
```

### 8.3、衍生的注解

@Component 有几个衍生注解，会按照 web 开发中，mvc 架构中分层。

- dao （@Repository）
- service（@Service）
- controller（@Controller）

**这四个注解的功能是一样的，都是代表将某个类注册到容器中**

### 8.4、自动装配置

@Autowired：默认是 byType 方式，如果匹配不上，就会 byName

@Nullable：字段标记了这个注解，说明该字段可以为空

@Resource：默认是 byName 方式，如果匹配不上，就会 byType

### 8.5、作用域[@scope ](/scope)

```java
//原型模式prototype，单例模式singleton
//scope("prototype")相当于<bean scope="prototype"></bean>
@Component
@scope("prototype")
public class User {

    //相当于<property name="name" value="kuangshen"/>
    @value("kuangshen")
    public String name;

    //也可以放在set方法上面
    @value("kuangshen")
    public void setName(String name) {
        this.name = name;
    }
}
```

### 8.6、小结

**xml 与注解：**

- xml 更加万能，维护简单，适用于任何场合
- 注解，不是自己的类使用不了，维护复杂

**最佳实践：**

- xml 用来管理 bean
- 注解只用来完成属性的注入
- 要开启注解支持

## 9、使用 Java 的方式配置 Spring

不使用 Spring 的 xml 配置，完全交给 java 来做！

Spring 的一个子项目，在 spring4 之后，，，它成为了核心功能

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472064164-cf739a67-0d9a-4f49-b36f-8d78ccaf6136.png#clientId=u65c68b31-85a1-4&from=paste&height=344&id=u974c124e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=687&originWidth=1424&originalType=binary∶=1&size=1679930&status=done&style=none&taskId=ue3516ca5-3e68-458a-bd30-916ccabb60a&width=712)
**实体类：pojo 的 User.java**

```java
//这里这个注解的意思,就是说明这个类被Spring接管了,注册到了容器中
@component
public class User {
    private String name;

    public String getName() {
    	return name;
    }
    //属性注入值
    @value("QINJIANG')
    public void setName(String name) {
    	this.name = name;
    }
    @Override
    public String toString() {
        return "user{" +
        "name='" + name + '\''+
        '}';
    }
}
```

弹幕评论：要么使用@Bean，要么使用@Component 和 ComponentScan，两种效果一样

**配置文件：config 中的 kuang.java**

@Import(KuangConfig2.class)，用@import 来包含 KuangConfig2.java

```java
//这个也会Spring容器托管,注册到容器中,因为他本米就是一个@Component
// @Configuration表这是一个配置类,就像我们之前看的beans.xml，类似于<beans>标签
@Configuration
@componentScan("com.Kuang.pojo") //开启扫描
//@Import(KuangConfig2.class)
public class KuangConfig {
    //注册一个bean , 就相当于我们之前写的一个bean 标签
    //这个方法的名字,就相当于bean 标签中的 id 属性 ->getUser
    //这个方法的返同值,就相当于bean 标签中的class 属性 ->User

    //@Bean
    public User getUser(){
    	return new User(); //就是返回要注入到bean的对象!
    }
}
```

弹幕评论：ComponentScan、@Component("pojo”) 这两个注解配合使用

**测试类**

```java
public class MyTest {
    public static void main(String[ ] args) {
    //如果完全使用了配置类方式去做,我们就只能通过 Annotationconfig 上下文来获取容器,通过配置类的class对象加载!
    ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.Class); //class对象
    User getUser =(User)context.getBean( "getUser"); //方法名getUser
    System.out.Println(getUser.getName());
    }
}
```

**会创建两个相同对象问题的说明：**

**弹幕总结 - -> @Bean 是相当于< bean>标签创建的对象，而我们之前学的@Component 是通过 spring 自动创建的这个被注解声明的对象，所以这里相当于有两个 User 对象被创建了。一个是 bean 标签创建的（@Bean），一个是通过扫描然后使用@Component，spring 自动创建的 User 对象，所以这里去掉@Bean 这些东西，然后开启扫描。之后在 User 头上用@Component 即可达到 spring 自动创建 User 对象了**

```java
//这个也会Spring容器托管,注册到容器中,因为他本米就是一个@Component
// @Configuration表这是一个配置类,就像我们之前看的beans.xml，类似于<beans>标签
@Configuration
@componentScan("com.Kuang.pojo") //开启扫描
//@Import(KuangConfig2.class)
public class KuangConfig {
    //注册一个bean , 就相当于我们之前写的一个bean 标签
    //这个方法的名字,就相当于bean 标签中的 id 属性 ->getUser
    //这个方法的返同值,就相当于bean 标签中的class 属性 ->User

    //@Bean
    public User getUser(){
    	return new User(); //就是返回要注入到bean的对象!
    }
}
```

弹幕评论：ComponentScan、@Component("pojo”) 这两个注解配合使用

**测试类**

```java
public class MyTest {
    public static void main(String[ ] args) {
    //如果完全使用了配置类方式去做,我们就只能通过 Annotationconfig 上下文来获取容器,通过配置类的class对象加载!
    ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.Class); //class对象
    User getUser =(User)context.getBean( "getUser"); //方法名getUser
    System.out.Println(getUser.getName());
    }
}
```

**会创建两个相同对象问题的说明：**

**弹幕总结 - -> @Bean 是相当于< bean>标签创建的对象，而我们之前学的@Component 是通过 spring 自动创建的这个被注解声明的对象，所以这里相当于有两个 User 对象被创建了。一个是 bean 标签创建的（@Bean），一个是通过扫描然后使用@Component，spring 自动创建的 User 对象，所以这里去掉@Bean 这些东西，然后开启扫描。之后在 User 头上用@Component 即可达到 spring 自动创建 User 对象了**

## 10、动态代理

代理模式是 SpringAOP 的底层

分类：动态代理和静态代理

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472093138-32a4cc48-0e0c-4470-8d41-95edc040a9f5.png#clientId=u65c68b31-85a1-4&from=paste&height=318&id=u06036644&margin=%5Bobject%20Object%5D&name=image.png&originHeight=635&originWidth=1426&originalType=binary∶=1&size=295439&status=done&style=none&taskId=u3a4682f7-93df-4327-89d8-5588d7a3f33&width=713)

### 10.1、静态代理

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472110260-68d6d574-e8a2-4ada-974a-93238575f044.png#clientId=u65c68b31-85a1-4&from=paste&height=213&id=ued37f954&margin=%5Bobject%20Object%5D&name=image.png&originHeight=426&originWidth=1458&originalType=binary∶=1&size=333570&status=done&style=none&taskId=ufa2f2bd3-104b-4298-ac4b-082af9a2452&width=729)
代码步骤：

1、接口

```java
package pojo;
public interface Host {
	public void rent();
}
```

2、真实角色

```java
package pojo;
public class HostMaster implements Host{

	public void rent() {
		System.out.println("房东要出租房子");
	}
}
```

3、代理角色

```java
package pojo;
public class Proxy {

	public Host host;

	public Proxy() {

	}

	public Proxy(Host host) {
		super();
		this.host = host;
	}

	public void rent() {
		seeHouse();
		host.rent();
		fee();
		sign();
	}
	//看房
	public void seeHouse() {
		System.out.println("看房子");
	}
	//收费
	public void fee() {
		System.out.println("收中介费");
	}
	//合同
	public void sign() {
		System.out.println("签合同");
	}
}
```

4、客户端访问代理角色

```java
package holle4_proxy;

import pojo.Host;
import pojo.HostMaster;
import pojo.Proxy;

public class My {

	public static void main(String[] args) {
		//房东要出租房子
		Host host = new HostMaster();
		//中介帮房东出租房子，但也收取一定费用（增加一些房东不做的操作）
		Proxy proxy = new Proxy(host);
		//看不到房东，但通过代理，还是租到了房子
		proxy.rent();

	}
}
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472182662-cd60ab25-c271-4316-af34-9722171677ff.png#clientId=u65c68b31-85a1-4&from=paste&height=276&id=u44fe5565&margin=%5Bobject%20Object%5D&name=image.png&originHeight=552&originWidth=1432&originalType=binary∶=1&size=318739&status=done&style=none&taskId=u4c988078-ba16-4d13-ad27-de269300307&width=716)
代码翻倍：几十个真实角色就得写几十个代理

AOP 横向开发

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472202392-f8398270-e31d-4f1d-962d-c24024ab0170.png#clientId=u65c68b31-85a1-4&from=paste&height=616&id=u6f0ff715&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1231&originWidth=1429&originalType=binary∶=1&size=535042&status=done&style=none&taskId=uee994fb2-a8b0-4b92-9319-ac8f359e328&width=714.5)

### 10.2、动态代理

动态代理和静态角色一样，动态代理底层是反射机制

动态代理类是动态生成的，不是我们直接写好的！

动态代理(两大类)：基于接口，基于类

- 基于接口：JDK 的动态代理【使用 ing】
- 基于类：cglib
- java 字节码实现：javasisit

了解两个类
1、Proxy：代理
2、InvocationHandler：调用处理程序
![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472250406-dbe28260-99f3-47cd-bcb3-dd256a81ddfe.png#clientId=u65c68b31-85a1-4&from=paste&height=75&id=uec06fc4f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=150&originWidth=1437&originalType=binary∶=1&size=119714&status=done&style=none&taskId=u1b8a68e3-05e6-4b8b-b247-e42a373ad8b&width=718.5)

实例：

接口 Host.java

```java
//接口
package pojo2;
public interface Host {
	public void rent();

}
```

接口 Host 实现类 HostMaster.java

```java
//接口实现类
package pojo2;
public class HostMaster implements Host{
	public void rent() {
		System.out.println("房东要租房子");
	}
}
```

代理角色的处理程序类 ProxyInvocationHandler.java

```java
package pojo2;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

///用这个类，自动生成代理
public class ProxyInvocationHandler implements InvocationHandler {

	// Foo f =(Foo) Proxy.NewProxyInstance(Foo. Class.GetClassLoader(),
	// new Class<?>[] { Foo.Class },
	// handler);

	// 被代理的接口
	public HostMaster hostMaster ;

	public void setHostMaster(HostMaster hostMaster) {
		this.hostMaster = hostMaster;
	}

	// 得到生成的代理类
	public Object getProxy() {
		// newProxyInstance() -> 生成代理对象，就不用再写具体的代理类了
		// this.getClass().getClassLoader() -> 找到加载类的位置
		// hostMaster.getClass().getInterfaces() -> 代理的具体接口
		// this -> 代表了接口InvocationHandler的实现类ProxyInvocationHandler
		return Proxy.newProxyInstance(this.getClass().getClassLoader(), hostMaster.getClass().getInterfaces(), this);


	// 处理代理实例并返回结果
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		seeHouse();
		// 动态代理的本质，就是使用反射机制实现的
        // invoke()执行它真正要执行的方法
		Object result = method.invoke(hostMaster, args);
		fee();
		return result;
	}

	public void seeHouse() {
		System.out.println("看房子");
	}

	public void fee() {
		System.out.println("收中介费");
	}

}
```

用户类 My2.java

```java
package holle4_proxy;

import pojo2.Host;
import pojo2.Host2;
import pojo2.HostMaster;
import pojo2.ProxyInvocationHandler;

public class My2 {

	public static void main(String[] args) {

		//真实角色
		HostMaster hostMaster = new HostMaster();

		//代理角色，现在没有；用代理角色的处理程序来实现Host接口的调用
		ProxyInvocationHandler pih = new ProxyInvocationHandler();

        //pih -> HostMaster接口类 -> Host接口
		pih.setHostMaster(hostMaster);

		//获取newProxyInstance动态生成代理类
		Host proxy = (Host) pih.getProxy();

		proxy.rent();

	}
}
```

弹幕评论：
什么时候调用 invoke 方法的?
代理实例调用方法时 invoke 方法就会被调用，可以 debug 试试

改为**万能代理类**

```java
///用这个类，自动生代理
public class ProxyInvocationHandler implements InvocationHandler {

	// 被代理的接口
	public Object target;

	public void setTarget(Object target) {
		this.target = target;
	}

	// 得到生成的代理类 -> 固定的代码
	public Object getProxy() {
		return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
	}

	// 处理代理实例并返回结果
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		// 动态代理的本质，就是使用反射机制实现的
		// invoke()执行它真正要执行的方法
		Object result = method.invoke(target, args);
		return result;
	}

}
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472286319-fedd39e9-373b-4ad8-9f64-4b8ee3c604ca.png#clientId=u65c68b31-85a1-4&from=paste&height=297&id=u9fe42ff0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=593&originWidth=1465&originalType=binary∶=1&size=416618&status=done&style=none&taskId=ua22f25c6-84a0-4f29-9431-610048ecc8e&width=732.5)

## 11、AOP

### 11.1、什么是 AOP

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472302779-a8366011-5841-48c9-a6c7-eff3e7546103.png#clientId=u65c68b31-85a1-4&from=paste&height=97&id=u04219bd7&margin=%5Bobject%20Object%5D&name=image.png&originHeight=194&originWidth=1456&originalType=binary∶=1&size=457846&status=done&style=none&taskId=ua7ef26da-9f3b-497b-b0a7-659aab41f56&width=728)

### 11.2、AOP 在 Spring 中的使用

提供声明式事务，允许用户自定义切面

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等…
- 切面(Aspect)：横切关注点 被模块化的特殊对象。即，它是一个类。（Log 类）
- 通知(Advice)：切面必须要完成的工作。即，它是类中的一个方法。（Log 类中的方法）
- 目标(Target)：被通知对象。（生成的代理类)
- 代理(Proxy)：向目标对象应用通知之后创建的对象。（生成的代理类）
- 切入点(PointCut)：切面通知执行的”地点”的定义。（最后两点：在哪个地方执行，比如：method.invoke()）
- 连接点(JointPoint)：与切入点匹配的执行点。

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472322962-506e3170-dbca-45d9-ac0b-0e3eea0c8a9e.png#clientId=u65c68b31-85a1-4&from=paste&height=443&id=uf65ee572&margin=%5Bobject%20Object%5D&name=image.png&originHeight=886&originWidth=1451&originalType=binary∶=1&size=606566&status=done&style=none&taskId=ud3471f4c-c60b-46b1-b070-7126b300d2b&width=725.5)
SpringAOP 中，通过 Advice 定义横切逻辑，Spring 中支持 5 种类型的 Advice:

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472344670-8b22f0b8-df31-4b7a-bdb5-98af8fdf68cc.png#clientId=u65c68b31-85a1-4&from=paste&height=233&id=u99ab396b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=466&originWidth=1409&originalType=binary∶=1&size=265012&status=done&style=none&taskId=u47ab38d8-661a-472d-8686-7cd6f379dbd&width=704.5)
**即 AOP 在不改变原有代码的情况下，去增加新的功能。**（代理）

### 11.3、使用 Spring 实现 AOP

导入 jar 包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

#### 11.3.1、方法一：使用原生 spring 接口

springAPI 接口实现

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userservice" class="service.UserServiceImpl"/>
    <bean id="log" class="log.Log"/>
    <bean id="afterLog" class="log.AfterLog"/>
	<!--方式一，使用原生Spring API接口-->
    <!--配置aop,还需要导入aop约束-->
    <aop:config>
        <!--切入点：expression:表达式，execution（要执行的位置）-->
        <aop:pointcut id="pointcut" expression="execution(* service.UserServiceImpl.*(..))"/>
        <!--UserServiceImpl.*(..) -》 UserServiceImpl类下的所以方法(参数)-->
        <!--执行环绕增加-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
        <!-- 环绕,在id="pointcut"的前后切入 -->
    </aop:config>

</beans>
```

execution(返回类型，类名，方法名(参数)) -> execution(_ com.service._,\*(…))

UserService.java

```java
package service;
public interface UserService {
	    public void add() ;
	    public void delete() ;
	    public void query() ;
	    public void update();
}
```

UserService 的实现类 UserServiceImp.java

```java
package service;

public class UserServiceImpl implements UserService {

    public void add() {
        System.out.println("add增");
    }
    public void delete() {
        System.out.println("delete删");
    }
    public void update() {
        System.out.println("update改");
    }
    public void query() {
        System.out.println("query查");
    }
}
```

前置 Log.java

```java
package log;
import org.springframework.aop.MethodBeforeAdvice;
import java.lang.reflect.Method;

public class Log implements MethodBeforeAdvice {
    //method：要执行的目标对象的方法
    //args：参数
    //target：目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
    }
}
```

后置 AfterLog.java

```java
package log;
import java.lang.reflect.Method;
import org.springframework.aop.AfterReturningAdvice;

public class AfterLog implements AfterReturningAdvice {
    //returnVaule: 返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
    	System.out.println("执行了"+method.getName()+"方法，返回值是"+returnValue);
    }
}
```

测试类 MyTest5

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import service.UserService;

public class MyTest5 {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
```

#### 11.3.2、方法二：自定义类实现 AOP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:aop="http://www.springframework.org/schema/aop"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
   	https://www.springframework.org/schema/beans/spring-beans.xsd
   	http://www.springframework.org/schema/aop
   	https://www.springframework.org/schema/aop/spring-aop.xsd">

   <!--注册bean-->
   <bean id="userservice" class="service.UserServiceImpl"/>
   <bean id="log" class="log.Log"/>
   <bean id="afterLog" class="log.AfterLog"/>
   <!-- 方式二，自定义 -->
   <bean id="diy" class="diy.DiyPointcut"/>
   <aop:config>
       <!--自定义切面-->
       <aop:aspect ref="diy">
           <!--切入点-->
           <aop:pointcut id="point" expression="execution(* service.UserServiceImpl.*(..))"/>
           <aop:before method="before" pointcut-ref="point"/>
           <aop:after method="after" pointcut-ref="point"/>
       </aop:aspect>
   </aop:config>

</beans>
```

```java
package diy;
public class DiyPointcut {

    public void before(){
        System.out.println("插入到前面");
    }

    public void after(){
        System.out.println("插入到后面");
    }
}
```

```java
//测试
public class MyTest5 {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
```

#### 11.3.3、方法三：使用注解实现

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 注册 -->
    <bean id="userservice" class="service.UserServiceImpl"/>
    <!--方式三，使用注解实现-->
    <bean id="diyAnnotation" class="diy.DiyAnnotation"></bean>

    <!-- 开启自动代理
		实现方式：默认JDK (proxy-targer-class="fasle")
    			 cgbin (proxy-targer-class="true")-->
	<aop:aspectj-autoproxy/>

</beans>
```

DiyAnnotation.java

```java
package diy;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect  //标注这个类是一个切面
public class DiyAnnotation {

    @Before("execution(* service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("=====方法执行前=====");
    }

    @After("execution(* service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("=====方法执行后=====");
    }

    //在环绕增强中，我们可以给地暖管一个参数，代表我们要获取切入的点
    @Around("execution(* service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前");

        Object proceed = joinPoint.proceed();

        System.out.println("环绕后");
    }
}
```

测试

```java
public class MyTest5 {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
```

输出结果：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472366377-cc3b2d27-7a9d-4c51-81f4-ea9986f2dc37.png#clientId=u65c68b31-85a1-4&from=paste&height=152&id=ub2d25d62&margin=%5Bobject%20Object%5D&name=image.png&originHeight=304&originWidth=530&originalType=binary∶=1&size=16982&status=done&style=none&taskId=uc5e52ebc-7c61-4cc3-936b-fd7f9c8de83&width=265)

## 12、整合 mybatis

mybatis-spring 官网：[https://mybatis.org/spring/zh/](https://mybatis.org/spring/zh/)

**mybatis 的配置流程：**

1. 编写实体类
2. 编写核心配置文件
3. 编写接口
4. 编写 Mapper.xmi
5. 测试

### 12.1、mybatis-spring-方式一

1. 编写数据源配置
2. sqISessionFactory
3. sqISessionTemplate（相当于 sqISession）
4. 需要给接口加实现类【new】
5. 将自己写的实现类，注入到 Spring 中
6. 测试！

先导入 jar 包

```xml
<dependencies>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.4</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.12</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
    </dependency>

</dependencies>

<!--在build中配置resources，来防止资源导出失败的问题-->
<!-- Maven解决静态资源过滤问题 -->
<build>
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
</resources>
</build>
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625472386121-cbb94361-c6f6-47ba-868e-39521696a0a2.png#clientId=u65c68b31-85a1-4&from=paste&height=536&id=ud6ca78bf&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1071&originWidth=727&originalType=binary∶=1&size=184771&status=done&style=none&taskId=ude8d9f5a-d54e-4e62-b99f-e34a08fb5cd&width=363.5)
**编写顺序：**
**User -> UserMapper -> UserMapper.xml -> spring-dao.xml -> UserServiceImpl -> applicationContext.xml -> MyTest6**

**代码步骤：**

pojo 实体类 User

```java
package pojo;
import lombok.Data;
@Data
public class User {
	private int id;
	private String name;
	private String pwd;
}
```

mapper 目录下的 UserMapper、UserMapperImpl、UserMapper.xml

接口 UserMapper

```java
package mapper;
import java.util.List;
import pojo.User;
public interface UserMapper {
	public List<User> getUser();
}
```

UserMapperImpl

```java
package mapper;
import java.util.List;
import org.mybatis.spring.SqlSessionTemplate;
import pojo.User;

public class UserMapperImpl implements UserMapper{

	//我们的所有操作，在原来都使用sqlSession来执行，现在都使用SqlSessionTemplate；
	private SqlSessionTemplate sqlSessionTemplate;

	public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
		this.sqlSessionTemplate = sqlSessionTemplate;
	}

	public List<User> getUser() {
		UserMapper mapper = sqlSessionTemplate.getMapper(UserMapper.class);
		return mapper.getUser();
	}
}
```

UserMapper.xml （狂神给面子才留下来的）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 绑定接口 -->
<mapper namespace="mapper.UserMapper">
	<select id="getUser" resultType="pojo.User">
		select * from mybatis.mybatis
	</select>
</mapper>
```

resource 目录下的 mybatis-config.xml、spring-dao.xml、applicationContext.xml

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!--开启日志-->
	<settings>
		<setting name="logImpl" value="STDOUT_LOGGING" />
	</settings>

	<!--可以给实体类起别名 -->
	<typeAliases>
		<package name="pojo" />
	</typeAliases>

</configuration>
```

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

	<!--DataSource:使用Spring的数帮源替换Mybatis的配置 其他数据源：c3p0、dbcp、druid
		这使用Spring提供的JDBC: org.springframework.jdbc.datasource -->
	<!--data source -->
	<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai"/>
		<property name="username" value="root" />
		<property name="password" value="root" />
	</bean>

	<!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>

	<!-- sqlSessionTemplate 就是之前使用的：sqlsession -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    	<!-- 只能使用构造器注入sqlSessionFactory 原因：它没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

	<!-- 导入spring-dao.xml -->
	<import resource="spring-dao.xml"/>

    <bean id="userMapper" class="mapper.UserMapperImpl">
        <property name="sqlSessionTemplate" ref="sqlSession"></property>
    </bean>

</beans>
```

测试类

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import mapper.UserMapper;
import pojo.User;
public class MyTest6 {
	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

		UserMapper userMapper = (UserMapper) context.getBean("userMapper");

		for (User user : userMapper.getUser()) {
			System.out.println(user);
		}
	}
}
```

### 12.2、mybatis-spring-方式二

UserServiceImpl2

```java
package mapper;
import pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.mybatis.spring.support.SqlSessionDaoSupport;
import java.util.List;
//继承SqlSessionDaoSupport 类
public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {
    public List<User> getUser() {
        SqlSession sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.getUser();
        //或者一句话：return getSqlSession().getMapper(UserMapper.class).getUser();
    }
}
```

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

	<!--DataSource:使用Spring的数帮源替换Mybatis的配置 c3p0 dbcp druid
		这使用Spring提供的JDBC: org.springframework.jdbc.datasource -->
	<!--data source -->
	<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai"/>
		<property name="username" value="root" />
		<property name="password" value="root" />
	</bean>

	<!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>

	<!-- 方法二：SqlSessionTemplate 可以不写了-->

</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

	<import resource="spring-dao.xml" />

	<!-- 方法二 -->
	<bean id="userMapper2" class="mapper.UserMapperImpl2">
		<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
	</bean>
</beans>
```

测试

```java
public class MyTest6 {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		UserMapper userMapper = (UserMapper) context.getBean("userMapper2");
		for (User user : userMapper.getUser()) {
			System.out.println(user);
		}
	}
}
```

## 13. 声明式事务

- 把一组业务当成一个业务来做；要么都成功，要么都失败！
- 事务在项目开发中，十分的重要，涉及到数据的一致性问题
- 确保完整性和一致性

事务的 ACID 原则：
1、原子性
2、隔离性
3、一致性
4、持久性

ACID 参考文章：[https://www.cnblogs.com/malaikuangren/archive/2012/04/06/2434760.html](https://www.cnblogs.com/malaikuangren/archive/2012/04/06/2434760.html)

Spring 中的事务管理

- 声明式事务：AOP
- 编程式事务：需要再代码中，进行事务管理

**声明式事务**

先导入 jar 包

```xml
<dependencies>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.4</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.12</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
    </dependency>

</dependencies>

<!--在build中配置resources，来防止资源导出失败的问题-->
<!-- Maven解决静态资源过滤问题 -->
<build>
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
</resources>
</build>
```

**代码步骤：**

pojo 实体类 User

```java
package pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
	private int id;
	private String name;
	private String pwd;
}
```

mapper 目录下的 UserMapper、UserMapperImpl、UserMapper.xml

接口 UserMapper

```java
package mapper;
import java.util.List;
import org.apache.ibatis.annotations.Param;
import pojo.User;

public interface UserMapper {
	public List<User> getUser();

	public int insertUser(User user);

	public int delUser(@Param("id") int id);
}
```

UserMapperImpl

```java
package mapper;

import pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.mybatis.spring.support.SqlSessionDaoSupport;
import java.util.List;

public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {
    public List<User> getUser() {
    	User user = new User(5,"你好","ok");
    	insertUser(user);
    	delUser(5);
        SqlSession sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.getUser();
        //或者return  getSqlSession().getMapper(UserMapper.class).getUser();
    }
    //插入
	public int insertUser(User user) {
		return getSqlSession().getMapper(UserMapper.class).insertUser(user);
	}
	//删除
	public int delUser(int id) {
		return getSqlSession().getMapper(UserMapper.class).delUser(id);
	}
}
```

UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 绑定接口 -->
<mapper namespace="mapper.UserMapper">
	<select id="getUser" resultType="pojo.User">
		select * from mybatis.mybatis
	</select>

	<insert id="insertUser"  parameterType="pojo.User" >
		insert into  mybatis.mybatis (id,name,pwd) values (#{id},#{name},#{pwd})
	</insert>

	<delete id="delUser" parameterType="_int">
		deleteAAAAA from mybatis.mybatis where id = #{id}
		<!-- deleteAAAAA是故意写错的 -->
	</delete>

</mapper>
```

resource 目录下的 mybatis-config.xml、spring-dao.xml、applicationContext.xml

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- configuration -->
<configuration>

	<!--开启日志-->
	<settings>
		<setting name="logImpl" value="STDOUT_LOGGING" />
	</settings>

	<!--可以给实体类起别名-->
	<typeAliases>
		<package name="pojo" />
	</typeAliases>

</configuration>
```

spring-dao.xml（已导入约束）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

	<!--data source -->
	<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai"/>
		<property name="username" value="root" />
		<property name="password" value="root" />
	</bean>

	<!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:mapper/*.xml"/>
    </bean>

	<!--声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="datasource" />
    </bean>

    <!--结合aop实现事务织入-->
    <!--配置事务的通知类-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给哪些方法配置事务-->
        <!--新东西：配置事务的传播特性 propagation-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <!-- *号包含上面4个方法：
            <tx:method name="*" propagation="REQUIRED"/> -->
        </tx:attributes>
    </tx:advice>

    <!--配置事务切入-->
    <aop:config>
        <aop:pointcut id="txpointcut" expression="execution(* mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txpointcut"/>
    </aop:config>

</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">

	<import resource="spring-dao.xml" />

	<bean id="userMapper" class="mapper.UserMapperImpl">
		<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
	</bean>
</beans>
```

测试类

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import mapper.UserMapper;import pojo.User;
public class MyTest7 {
	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

		UserMapper userMapper = (UserMapper) context.getBean("userMapper");

		for (User user : userMapper.getUser()) {
			System.out.println(user);
		}
	}
}
```

**思考：**
为什么需要事务？

- 如果不配置事务，可能存在数据提交不一致的情况下；
- 如果不在 spring 中去配置声明式事务，我们就需要在代码中手动配置事务！
- 事务在项目的开发中非常重要，涉及到数据的一致性和完整性问题！
