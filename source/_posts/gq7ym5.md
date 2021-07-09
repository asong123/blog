---
title: 22、SpringMVC
urlname: gq7ym5
date: '2021-07-09 20:42:19 +0800'
tags: []
categories: []
---

# 1、回顾 MVC

## 、什么是 MVC

MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，是一种软件设计规范。是将业务逻辑、数据、显示分离的方法来组织代码。
MVC 主要作用是**降低了视图与业务逻辑间的双向偶合**。
MVC 不是一种设计模式，**MVC 是一种架构模式**。当然不同的 MVC 存在差异。
**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或
JavaBean 组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据 Dao） 和 服务层
（行为 Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。
**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。
**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型 数据返回给视图，由视图负责展示。 也就是说控制器做了个调度员的工作。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834541755-71773e2f-6f19-4e6d-9dfe-d963bb5eaa0f.jpeg#)**最典型的 MVC 就是 JSP + servlet + javabean 的模式。**

1.  **、Model1 时代**

在 web 早期的开发中，通常采用的都是 Model1。
Model1 中，主要分为两层，视图层和模型层。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834542213-a1a33551-71aa-4e4b-9b87-b6840c2e3a9c.png#)
Model1 优点：架构简单，比较适合小型项目开发；
Model1 缺点：JSP 职责不单一，职责过重，不便于维护；

1.  **、Model2 时代**

Model2 把一个项目分成三部分，包括**视图、控制、模型。**
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834542609-3a96a144-81be-486f-9f6c-80d25c875ee3.png#)

1. 用户发请求
1. Servlet 接收请求数据，并调用对应的业务逻辑方法
1. 业务处理完毕，返回更新后的数据给 servlet
1. servlet 转向到 JSP，由 JSP 来渲染页面
1. 响应给前端更新后的页面

**职责分析：**
**Controller：控制器**

1. 取得表单数据
1. 调用业务逻辑
1. 转向指定的页面

**Model：模型**

1. 业务逻辑
1. 保存数据的状态

**View：视图**

1. 显示页面
   Model2 这样不仅提高的代码的复用率与项目的扩展性，且大大降低了项目的维护成本。Model 1 模式的实现比较简单，适用于快速开发小规模项目，Model1 中 JSP 页面身兼 View 和 Controller 两种角色，将控 制逻辑和表现逻辑混杂在一起，从而导致代码的重用性非常低，增加了应用的扩展性和维护的难度。
   Model2 消除了 Model1 的缺点。

   1. **、回顾 Servlet**

1. 新建一个 Maven 工程当做父工程！ pom 依赖！

1. <dependencies>
1. <dependency>
1. <groupId>junit</groupId>
1. <artifactId>junit</artifactId>
1. <version>4.12</version>
1. </dependency>
1. <dependency>
1. <groupId>org.springframework</groupId>
1. <artifactId>spring-webmvc</artifactId>
1. <version>5.1.9.RELEASE</version>
1. </dependency>
1. <dependency>
1. <groupId>javax.servlet</groupId>
1. <artifactId>servlet-api</artifactId>
1. <version>2.5</version>
1. </dependency>
1. <dependency>
1. <groupId>javax.servlet.jsp</groupId>
1. <artifactId>jsp-api</artifactId>
1. <version>2.2</version>
1. </dependency>
1. <dependency>
1. <groupId>javax.servlet</groupId>
1. <artifactId>jstl</artifactId>
1. <version>1.2</version>
1. </dependency>
1. </dependencies>

1. 建立一个 Moudle：springmvc-01-servlet ， 添加 Web app 的支持！
1. 导入 servlet 和 jsp 的 jar 依赖

1. <dependency>
1. <groupId>javax.servlet</groupId>
1. <artifactId>servlet-api</artifactId>
1. <version>2.5</version>
1. </dependency>
1. <dependency>
1. <groupId>javax.servlet.jsp</groupId>
1. <artifactId>jsp-api</artifactId>
1. <version>2.2</version>
1. </dependency>

1. 编写一个 Servlet 类，用来处理用户的请求

1 package com.kuang.servlet; 2

1.  //实现 Servlet 接口
1.  public class HelloServlet extends HttpServlet {
1.  @Override
1.      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
1.  //取得参数
1.  String method = req.getParameter("method");
1.  if (method.equals("add")){
1.  req.getSession().setAttribute("msg","执行了 add 方法"); 11 }
1.  if (method.equals("delete")){
1.  req.getSession().setAttribute("msg","执行了 delete 方法"); 14 }
1.  //业务逻辑
1.  //视图跳转
1.      req.getRequestDispatcher("/WEB- INF/jsp/hello.jsp").forward(req,resp);

18 }
19
20
21
22
23
24
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
doGet(req,resp);
}
}

1. 编写 Hello.jsp，在 WEB-INF 目录下新建一个 jsp 的文件夹，新建 hello.jsp

| 1
2
3
4
5
6
7
8
9 | <%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
<title>Kuangshen</title>
</head>
<body>
${msg}
</body>
</html> |
| --- | --- |

1. 在 web.xml 中注册 Servlet

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

<?xml version="1.0" encoding="UTF-8"?>

<web-app xmlns=["http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)" xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" xsi:schemaLocation="[http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)
[http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd](http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd)" version="4.0">
<servlet>
<servlet-name>HelloServlet</servlet-name>
<servlet-class>com.kuang.servlet.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>HelloServlet</servlet-name>
<url-pattern>/user</url-pattern>
</servlet-mapping>
</web-app>

1. 配置 Tomcat，并启动测试

localhost:8080/user?method=add localhost:8080/user?method=delete
**MVC 框架要做哪些事情**

1. 将 url 映射到 java 类或 java 类的方法 .
1. 封装用户提交的数据 .
1. 处理请求--调用相关的业务处理--封装响应数据 .
1. 将响应的数据进行渲染 . jsp / html 等表示层数据 .

**说明：**
常见的服务器端 MVC 框架有：Struts、Spring MVC、ASP.NET MVC、Zend Framework、JSF；常见前端 MVC 框架：vue、angularjs、react、backbone；由 MVC 演化出了另外一些模式如：MVP、MVVM 等等....

**2、什么是 SpringMVC**

1.  ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834542857-aafec431-61a3-4fa5-9869-976291033e05.jpeg#)**、概述**

Spring MVC 是 Spring Framework 的一部分，是基于 Java 实现 MVC 的轻量级 Web 框架。
[查看官方文档：https://docs.spring.io/spring/docs/5.2.0.RELEASE/spring-framework-reference/web. html#spring-web](https://docs.spring.io/spring/docs/5.2.0.RELEASE/spring-framework-reference/web.html#spring-web)

**我们为什么要学习 SpringMVC 呢?**
Spring MVC 的特点：

1. 轻量级，简单易学
1. 高效 , 基于请求响应的 MVC 框架
1. 与 Spring 兼容性好，无缝结合
1. 约定优于配置
1. 功能强大：RESTful、数据验证、格式化、本地化、主题等
1. 简洁灵活

Spring 的 web 框架围绕**DispatcherServlet **[ 调度 Servlet ] 设计。
DispatcherServlet 的作用是将请求分发到不同的处理器。从 Spring 2.5 开始，使用 Java 5 或者以上版本的用户可以采用基于注解形式进行开发，十分简洁；
正因为 SpringMVC 好 , 简单 , 便捷 , 易学 , 天生和 Spring 无缝集成(使用 SpringIoC 和 Aop) , 使用约定优于配置 . 能够进行简单的 junit 测试 . 支持 Restful 风格 .异常处理 , 本地化 , 国际化 , 数据验证 , 类型转换 , 拦截器 等等 所以我们要学习 .
**最重要的一点还是用的人多 , 使用的公司多 .**

1.  **、中心控制器**

Spring 的 web 框架围绕 DispatcherServlet 设计。 DispatcherServlet 的作用是将请求分发到不同的处理器。从 Spring 2.5 开始，使用 Java 5 或者以上版本的用户可以采用基于注解的 controller 声明方式。
Spring MVC 框架像许多其他 MVC 框架一样, **以请求为驱动 **, **围绕一个中心 Servlet 分派请求及提供其他功能**，**DispatcherServlet 是一个实际的 Servlet (它继承自 HttpServlet 基类)**。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834543413-d3174059-8117-402e-bd3c-33d620fdc6f6.jpeg#)
SpringMVC 的原理如下图所示：
当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制 器，控制器处理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图 渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834543762-f4d7816f-1d27-437a-9947-ba35326579fe.jpeg#)

1.  **、SpringMVC 执行原理**

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834544270-59bae63e-fb0f-4137-b074-ab6b5e409294.jpeg#)

图为 SpringMVC 的一个较完整的流程图，实线表示 SpringMVC 框架提供的技术，不需要开发者实现，虚 线表示需要开发者实现。
**简要分析执行流程**

1. DispatcherServlet 表示前置控制器，是整个 SpringMVC 的控制中心。用户发出请求，

DispatcherServlet 接收请求并拦截请求。

我们假设请求的 url 为 : http://localhost:8080/SpringMVC/hello
**如上 url 拆分成三部分：**
http://localhost:8080 服务器域名
SpringMVC 部署在服务器上的 web 站点
hello 表示控制器
通过分析，如上 url 表示为：请求位于服务器 localhost:8080 上的 SpringMVC 站点的 hello 控制器。

1. HandlerMapping 为处理器映射。DispatcherServlet 调用 HandlerMapping,HandlerMapping 根据 请求 url 查找 Handler。
1. HandlerExecution 表示具体的 Handler,其主要作用是根据 url 查找控制器，如上 url 被查找控制器为：hello。
1. HandlerExecution 将解析后的信息传递给 DispatcherServlet,如解析控制器映射等。
1. HandlerAdapter 表示处理器适配器，其按照特定的规则去执行 Handler。
1. Handler 让具体的 Controller 执行。
1. Controller 将具体的执行信息返回给 HandlerAdapter,如 ModelAndView。
1. HandlerAdapter 将视图逻辑名或模型传递给 DispatcherServlet。
1. DispatcherServlet 调用视图解析器(ViewResolver)来解析 HandlerAdapter 传递的逻辑视图名。
1. 视图解析器将解析的逻辑视图名传给 DispatcherServlet。
1. DispatcherServlet 根据视图解析器解析的视图结果，调用具体的视图。
1. 最终视图呈现给用户。

在这里先听一遍原理，不理解没有关系，我们马上来写一个对应的代码实现大家就明白了，如果不明 白，那就写 10 遍，没有笨人，只有懒人！
**3、HelloSpring**

1.  **、配置版**
1.  新建一个 Moudle ， springmvc-02-hello ， 添加 web 的支持！
1.  确定导入了 SpringMVC 的依赖！
1.  配置 web.xml ， 注册 DispatcherServlet

    1.  <?xml version="1.0" encoding="UTF-8"?>
    1.  <web-app xmlns=["http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)"
    1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
    1.      xsi:schemaLocation=["http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)[ http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd](http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd)"
    1.  version="4.0">

6

1.  <!--1.注册DispatcherServlet-->
1.  <servlet>
1.  <servlet-name>springmvc</servlet-name>
1.      <servlet- class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
1.  <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
1.  <init-param>
1.  <param-name>contextConfigLocation</param-name>
1.  <param-value>classpath:springmvc-servlet.xml</param-value>
1.  </init-param>

16 <!--启动级别-1-->

1. <load-on-startup>1</load-on-startup>
1. </servlet> 19

20 <!--/ 匹配所有的请求；（不包括.jsp）-->
21 <!--/* 匹配所有的请求；（包括.jsp）-->

1. <servlet-mapping>
1. <servlet-name>springmvc</servlet-name>
1. <url-pattern>/</url-pattern>
1. </servlet-mapping> 26

27 </web-app>

1. 编写 SpringMVC 的 配置文件！名称：springmvc-servlet.xml : [servletname]-servlet.xml

说明，这里的名称要求是按照官方来的

| 1
2
3
4
5
6
7 | <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)" xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" xsi:schemaLocation="[http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
[http://www.springframework.org/schema/beans/spring-beans.xsd](http://www.springframework.org/schema/beans/spring-beans.xsd)">

| </beans> |
| -------- |

1. 添加 处理映射器
   | 1 | <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping" |
   | --- | --- |
   | /> | |

1. 添加 处理器适配器

| 1   | <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter |
| --- | ------------------------------------------------------------------------------- |
| "/> |                                                                                 |

1. 添加 视图解析器

| 1
2

3
4
5
6
7 | <!--视图解析器:DispatcherServlet给他的ModelAndView-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">

<!--前缀-->
<property name="prefix" value="/WEB-INF/jsp/"/>
<!--后缀-->
<property name="suffix" value=".jsp"/>
</bean> |
| --- | --- |

1. 编写我们要操作业务 Controller ，要么实现 Controller 接口，要么增加注解；需要返回一个

ModelAndView，装数据，封视图；

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
package com.kuang.controller;
import org.springframework.web.servlet.ModelAndView; import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse;

//注意：这里我们先导入 Controller 接口
public class HelloController implements Controller {
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
public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
//ModelAndView 模型和视图
ModelAndView mv = new ModelAndView();

//封装对象，放在 ModelAndView 中。Model mv.addObject("msg","HelloSpringMVC!");
//封装要跳转的视图，放在 ModelAndView 中 mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp return mv;
}
}

1. 将自己的类交给 SpringIOC 容器，注册 bean

| 1
2 | <!--Handler-->
<bean id="/hello" class="com.kuang.controller.HelloController"/> |
| --- | --- |

1. 写要跳转的 jsp 页面，显示 ModelandView 存放的数据，以及我们的正常页面；
| 1
2
3
4
5
6
7
8
9 | <%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
<title>Kuangshen</title>
</head>
<body>
${msg}
</body>
</html> |
| --- | --- |

1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834544621-612b4991-9015-4886-9cdb-89f681ecc626.png#)配置 Tomcat 启动测试！

**可能遇到的问题：访问出现 404，排查步骤：**

1. 查看控制台输出，看一下是不是缺少了什么 jar 包。
1. 如果 jar 包存在，显示无法输出，就在 IDEA 的项目发布中，添加 lib 依赖！
1. 重启 Tomcat 即可解决！

小结：看这个估计大部分同学都能理解其中的原理了，但是我们实际开发才不会这么写，不然就疯了， 还学这个玩意干嘛！我们来看个注解版实现，这才是 SpringMVC 的精髓，到底有多么简单，看这个图就知道了。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834544887-5bece7ad-84c9-4ab9-a5d2-17cffd2a9665.jpeg#)

1. **、注解版**
1. **新建一个 Moudle，springmvc-03-hello-annotation 。添加 web 支持！**

建立包结构 com.kuang.controller

1. 由于 Maven 可能存在资源过滤的问题，我们将配置完善

1. <build>
1. <resources>
1. <resource>
1. <directory>src/main/java</directory>
1. <includes>
1. <include>\*_/_.properties</include>
1. <include>\*_/_.xml</include>
1. </includes>
1. <filtering>false</filtering>
1. </resource>
1. <resource>
1. <directory>src/main/resources</directory>
1. <includes>
1. <include>\*_/_.properties</include>
1. <include>\*_/_.xml</include>
1. </includes>
1. <filtering>false</filtering>
1. </resource>
1. </resources>
1. </build>

1. 在 pom.xml 文件引入相关的依赖：主要有 Spring 框架核心库、Spring MVC、servlet , JSTL 等。我们在父依赖中已经引入了！
1. **配置 web.xml**

注意点：
注意 web.xml 版本问题，要最新版！ 注册 DispatcherServlet
关联 SpringMVC 的配置文件启动级别为 1
映射路径为 / 【不要用/\*，会 404】

1.  <?xml version="1.0" encoding="UTF-8"?>
1.  <web-app xmlns=["http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)"
1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1.      xsi:schemaLocation=["http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)[ http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd](http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd)"
1.  version="4.0">

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

<!--1.注册servlet-->
<servlet>
<servlet-name>SpringMVC</servlet-name>
<servlet- class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
<init-param>
<param-name>contextConfigLocation</param-name>
<param-value>classpath:springmvc-servlet.xml</param-value>
</init-param>
<!-- 启动顺序，数字越小，启动越早 -->
<load-on-startup>1</load-on-startup>
</servlet>
<!--所有请求都会被springmvc拦截 -->
<servlet-mapping>
<servlet-name>SpringMVC</servlet-name>

23
24
25
26
<url-pattern>/</url-pattern>
</servlet-mapping>
</web-app>

**/ 和 /\* 的区别：**
< url-pattern > / </ url-pattern > 不会匹配到.jsp， 只针对我们编写的请求； 即：.jsp 不会进入 spring 的 DispatcherServlet 类 。
< url-pattern > /_ </ url-pattern > 会匹配 _.jsp，
会出现返回 jsp 视图 时再次进入 spring 的 DispatcherServlet 类，导致找不到对应的 controller 所以报 404 错。

1. **添加 Spring MVC 配置文件**

让 IOC 的注解生效
静态资源过滤 ：HTML . JS . CSS . 图片 ， 视频 .....
MVC 的注解驱动配置视图解析器
在 resource 目录下添加 springmvc-servlet.xml 配置文件，配置的形式与 Spring 容器配置基本类似， 为了支持基于注解的 IOC，设置了自动扫描包的功能，具体配置信息如下：

1.  <?xml version="1.0" encoding="UTF-8"?>
1.  <beans xmlns=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)"
1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1.  xmlns:context=["http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)"
1.  xmlns:mvc=["http://www.springframework.org/schema/mvc](http://www.springframework.org/schema/mvc)"
1.  xsi:schemaLocation=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
1.  [http://www.springframework.org/schema/beans/spring-beans.xsd](http://www.springframework.org/schema/beans/spring-beans.xsd)
1.  [http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)
1.      [https://www.springframework.org/schema/context/spring-](http://www.springframework.org/schema/context/spring-) context.xsd
1.  [http://www.springframework.org/schema/mvc](http://www.springframework.org/schema/mvc)
1.  [https://www.springframework.org/schema/mvc/spring-mvc.xsd](http://www.springframework.org/schema/mvc/spring-mvc.xsd)"> 12
1.  <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
1.  <context:component-scan base-package="com.kuang.controller"/>
1.  <!-- 让Spring MVC不处理静态资源 -->
1.  <mvc:default-servlet-handler /> 17 <!--
1.  支持 mvc 注解驱动
1.  在 spring 中一般采用@RequestMapping 注解来完成映射关系
1.  要想使@RequestMapping 注解生效
1.  必须向上下文中注册 DefaultAnnotationHandlerMapping
1.  和一个 AnnotationMethodHandlerAdapter 实例
1.  这两个实例分别在类级别和方法级别处理。
1.  而 annotation-driven 配置帮助我们自动完成上述两个实例的注入。

25 -->
26 <mvc:annotation-driven /> 27
28 <!-- 视图解析器 -->

1.      <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver  "
1.  id="internalResourceViewResolver"> 31 <!-- 前缀 -->

32 <property name="prefix" value="/WEB-INF/jsp/" /> 33 <!-- 后缀 -->

34
35
36
37
<property name="suffix" value=".jsp" />
</bean>
</beans>

在视图解析器中我们把所有的视图都存放在/WEB-INF/目录下，这样可以保证视图安全，因为这个目录下的文件，客户端不能直接访问。

1. **创建 Controller**

编写一个 Java 控制类： com.kuang.controller.HelloController , 注意编码规范

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
package com.kuang.controller;
import org.springframework.stereotype.Controller; import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
@Controller @RequestMapping("/HelloController")
public class HelloController {
//真实访问地址 : 项目名/HelloController/hello
@RequestMapping("/hello")
public String sayHello(Model model){
//向模型中添加属性 msg 与值，可以在 JSP 页面中取出并渲染 model.addAttribute("msg","hello,SpringMVC");
//web-inf/jsp/hello.jsp return "hello";
}
}

@Controller 是为了让 Spring IOC 容器初始化时自动扫描到；
@RequestMapping 是为了映射请求路径，这里因为类与方法上都有映射所以访问时应该是/HelloController/hello；
方法中声明 Model 类型的参数是为了把 Action 中的数据带到视图中；
方法返回的结果是视图的名称 hello，加上配置文件中的前后缀变成 WEB-INF/jsp/**hello**.jsp。

1. **创建视图层**

在 WEB-INF/ jsp 目录中创建 hello.jsp ， 视图可以直接取出并展示从 Controller 带回的信息； 可以通过 EL 表示取出 Model 中存放的值，或者对象；

| 1
2
3
4
5
6
7
8
9 | <%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
<title>SpringMVC</title>
</head>
<body>
${msg}
</body>
</html> |
| --- | --- |

1. **配置 Tomcat 运行**

配置 Tomcat ， 开启服务器 ， 访问 对应的请求路径！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834545343-77df1a79-7254-43e8-82ab-1b3a0a4cead8.png#)

**OK，运行成功！**

1.  **、小结**

实现步骤其实非常的简单：

1. 新建一个 web 项目
1. 导入相关 jar 包
1. 编写 web.xml , 注册 DispatcherServlet
1. 编写 springmvc 配置文件
1. 接下来就是去创建对应的控制类 , controller
1. 最后完善前端视图和 controller 之间的对应
1. 测试运行调试.

使用 springMVC 必须配置的三大件：
**处理器映射器、处理器适配器、视图解析器**
通常，我们只需要**手动配置视图解析器**，而**处理器映射器**和**处理器适配器**只需要开启**注解驱动**即可，而 省去了大段的 xml 配置

# 4、Controller 及 RestFul

## 、控制器 Controller

控制器复杂提供访问应用程序的行为，通常通过接口定义或注解定义两种方法实现。 控制器负责解析用户的请求并将其转换为一个模型。
在 Spring MVC 中一个控制器类可以包含多个方法
在 Spring MVC 中，对于 Controller 的配置方式有很多种
我们来看看有哪些方式可以实现：

1.  **、实现 Controller 接口**

Controller 是一个接口，在 org.springframework.web.servlet.mvc 包下，接口中只有一个方法；

1.  //实现该接口的类获得控制器功能
1.  public interface Controller {
1.  //处理请求且返回一个模型与视图对象
1.      ModelAndView handleRequest(HttpServletRequest var1, HttpServletResponse var2) throws Exception;

5 }

**测试**

      1. 新建一个Moudle，springmvc-04-controller 。 将刚才的03 拷贝一份, 我们进行操作！ 删掉HelloController

mvc 的配置文件只留下 视图解析器！

      1. 编写一个Controller类，ControllerTest1

1. //定义控制器
1. //注意点：不要导错包，实现 Controller 接口，重写方法；
1. public class ControllerTest1 implements Controller { 4

5
6
7
8
9
10
11
12
public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
//返回一个模型视图对象
ModelAndView mv = new ModelAndView(); mv.addObject("msg","Test1Controller"); mv.setViewName("test");
return mv;
}
}

      1. 编写完毕后，去Spring配置文件中注册请求的bean；name对应请求路径，class对应处理请求的类

| 1   | <bean name="/t1" class="com.kuang.controller.ControllerTest1"/> |
| --- | --------------------------------------------------------------- |

      1. 编写前端test.jsp，注意在WEB-INF/jsp目录下编写，对应我们的视图解析器

| 1
2
3
4
5
6
7
8
9 | <%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
<title>Kuangshen</title>
</head>
<body>
${msg}
</body>
</html> |
| --- | --- |

      1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834545601-37590821-153b-49d0-a4ab-5069411916b7.png#)配置Tomcat运行测试，我这里没有项目发布名配置的就是一个 / ，所以请求不用加项目名，OK！

**说明：**
实现接口 Controller 定义控制器是较老的办法
缺点是：一个控制器中只有一个方法，如果要多个方法则需要定义多个 Controller；定义的方式比较麻烦；

1.  **、使用注解@Controller**

@Controller 注解类型用于声明 Spring 类的实例是一个控制器（在讲 IOC 时还提到了另外 3 个注 解）；
Spring 可以使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证 Spring 能找到你的控制器，需要在配置文件中声明组件扫描。

| 1
2 | <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
<context:component-scan base-package="com.kuang.controller"/> |
| --- | --- |

增加一个 ControllerTest2 类，使用注解实现；

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
//@Controller 注解的类会自动添加到 Spring 上下文中
@Controller
public class ControllerTest2{
//映射访问路径
@RequestMapping("/t2")
public String index(Model model){
//Spring MVC 会自动实例化一个 Model 对象用于向视图中传值 model.addAttribute("msg", "ControllerTest2");
//返回视图位置 return "test";
}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834545799-580b1d45-22f1-4d0e-824e-bc0322dbfc67.png#)运行 tomcat 测试

**可以发现，我们的两个请求都可以指向一个视图，但是页面结果的结果是不一样的，从这里可以看 出视图是被复用的，而控制器与视图之间是弱偶合关系。**

注解方式是平时使用的最多的方式！除了这两种之外还有其他的方式，大家想要自己研究的话，可以参 考我的博客：[https://www.cnblogs.com/hellokuangshen/p/11270742.html](https://www.cnblogs.com/hellokuangshen/p/11270742.html)

1.  **、RequestMapping**

**@RequestMapping**
@RequestMapping 注解用于映射 url 到控制器类或一个特定的处理程序方法。可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。
为了测试结论更加准确，我们可以加上一个项目名测试 myweb
只注解在方法上面

| 1
2
3
4
5
6
7 | @Controller
public class TestController { @RequestMapping("/h1") public String test(){
return "test";
}
} |
| --- | --- |

访问路径：http://localhost:8080 / 项目名 / h1
同时注解类与方法

| 1
2
3
4
5
6
7
8 | @Controller @RequestMapping("/admin") public class TestController {
@RequestMapping("/h1") public String test(){
return "test";
}
} |
| --- | --- |

访问路径：http://localhost:8080 / 项目名/ admin /h1 , 需要先指定类的路径再指定方法的路径；

1.  **、RestFul 风格**

**概念**
Restful 就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。
**功能**
资源：互联网所有的事物都可以被抽象为资源
资源操作：使用 POST、DELETE、PUT、GET，使用不同方法对资源进行操作。分别对应 添加、 删除、修改、查询。
**传统方式操作资源 **：通过不同的参数来实现不同的效果！方法单一，post 和 get
[http://127.0.0.1/item/queryItem.action?id=1 ](http://127.0.0.1/item/queryItem.action?id=1)查询,GET [http://127.0.0.1/item/saveItem.action ](http://127.0.0.1/item/saveItem.action)新增,POST
[http://127.0.0.1/item/updateItem.action ](http://127.0.0.1/item/updateItem.action)更新,POST
[http://127.0.0.1/item/deleteItem.action?id=1 ](http://127.0.0.1/item/deleteItem.action?id=1)删除,GET 或 POST
**使用 RESTful 操作资源 **： 可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但是功能可以不同！
[http://127.0.0.1/item/1 ](http://127.0.0.1/item/1)查询,GET
[http://127.0.0.1/item ](http://127.0.0.1/item)新增,POST
[http://127.0.0.1/item ](http://127.0.0.1/item)更新,PUT
[http://127.0.0.1/item/1 ](http://127.0.0.1/item/1)删除,DELETE

**学习测试**

1. 在新建一个类 RestFulController
   | 1
   2
   3 | @Controller
   public class RestFulController {
   } |
   | --- | --- |

1. 在 Spring MVC 中可以使用 @PathVariable 注解，让方法参数的值对应绑定到一个 URI 模板变量上。

1
2
3
4
5
6
@Controller
public class RestFulController {
//映射访问路径
@RequestMapping("/commit/{p1}/{p2}")
public String index(@PathVariable int p1, @PathVariable int p2, Model model){
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
int result = p1+p2;
//Spring MVC 会自动实例化一个 Model 对象用于向视图中传值 model.addAttribute("msg", "结果："+result);
//返回视图位置
return "test";
}
}

1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834546034-5728fcec-814a-4231-ac04-33b85e45698a.png#)我们来测试请求查看下

思考：使用路径变量的好处？ 使路径变得更加简洁；
获得参数更加方便，框架会自动进行类型转换。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834546402-b949429c-51c0-4475-b0f8-4902799e1650.png#)通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这 里访问是的路径是/commit/1/a，则路径与方法不匹配，而不会是参数转换失败。

1. 我们来修改下对应的参数类型，再次测试

1. //映射访问路径
1. @RequestMapping("/commit/{p1}/{p2}")
1. public String index(@PathVariable int p1, @PathVariable String p2, Model model){

4

1. String result = p1+p2;
1. //Spring MVC 会自动实例化一个 Model 对象用于向视图中传值
1. model.addAttribute("msg", "结果："+result);
1. //返回视图位置
1. return "test"; 10

11
}
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834546738-81242199-8845-4bef-9841-b1bee67a8f1a.png#)

**使用 method 属性指定请求类型**
用于约束请求的类型，可以收窄请求范围。指定请求谓词的类型如 GET, POST, HEAD, OPTIONS, PUT,
PATCH, DELETE, TRACE 等
我们来测试一下： 增加一个方法

| 1
2
3
4
5
6 | //映射访问路径,必须是 POST 请求
@RequestMapping(value = "/hello",method = {RequestMethod.POST}) public String index2(Model model){
model.addAttribute("msg", "hello!"); return "test";
} |
| --- | --- |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834546970-246f6c1a-6c9f-47d2-a717-9d82891f5383.png#)我们使用浏览器地址栏进行访问默认是 Get 请求，会报错 405：
如果将 POST 修改为 GET 则正常了；

| 1
2
3
4
5
6 | //映射访问路径,必须是 Get 请求
@RequestMapping(value = "/hello",method = {RequestMethod.GET}) public String index2(Model model){
model.addAttribute("msg", "hello!"); return "test";
} |
| --- | --- |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834547449-5aae13c7-5ba0-4e2c-8ff6-5362cf3c71ac.png#)

**小结：**
Spring MVC 的 @RequestMapping 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以及 PATCH。
**所有的地址栏请求默认都会是 HTTP GET 类型的。**
方法级别的注解变体有如下几个： 组合注解

| 1   | @GetMapping    |
| --- | -------------- |
| 2   | @PostMapping   |
| 3   | @PutMapping    |
| 4   | @DeleteMapping |
| 5   | @PatchMapping  |

@GetMapping 是一个组合注解
它所扮演的是 @RequestMapping(method =RequestMethod.GET) 的一个快捷方式。平时使用的会比较多！

1.  **、小黄鸭调试法**

场景一：我们都有过向别人（甚至可能向完全不会编程的人）提问及解释编程问题的经历，但是很多时 候就在我们解释的过程中自己却想到了问题的解决方案，然后对方却一脸茫然。
场景二：你的同行跑来问你一个问题，但是当他自己把问题说完，或说到一半的时候就想出答案走了， 留下一脸茫然的你。
其实上面两种场景现象就是所谓的小黄鸭调试法（Rubber Duck Debuging），又称橡皮鸭调试法，它是我们软件工程中最常使用调试方法之一。
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834547820-1e757a52-5058-4c80-8227-00393851450e.jpeg#)

此概念据说来自《程序员修炼之道》书中的一个故事，传说程序大师随身携带一只小黄鸭，在调试代码 的时候会在桌上放上这只小黄鸭，然后详细地向鸭子解释每行代码，然后很快就将问题定位修复了。

**5、结果跳转方式**

1.  **、ModelAndView**

设置 ModelAndView 对象 , 根据 view 的名称 , 和视图解析器跳到指定的页面 .
页面 : {视图解析器前缀} + viewName +{视图解析器后缀}

1 <!-- 视图解析器 -->

1. <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
1. id="internalResourceViewResolver"> 4 <!-- 前缀 -->

5 <property name="prefix" value="/WEB-INF/jsp/" /> 6 <!-- 后缀 -->

1. <property name="suffix" value=".jsp" />
1. </bean>

对应的 controller 类

1 public class ControllerTest1 implements Controller { 2
3
4
5
6
7
8
9
10
public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
//返回一个模型视图对象
ModelAndView mv = new ModelAndView(); mv.addObject("msg","ControllerTest1"); mv.setViewName("test");
return mv;
}
}

1.  **、ServletAPI**

通过设置 ServletAPI , 不需要视图解析器 .

1. 通过 HttpServletResponse 进行输出
1. 通过 HttpServletResponse 实现重定向
1. 通过 HttpServletResponse 实现转发

1. @Controller
1. public class ResultGo { 3

4
5
6
7
8
9
10
@RequestMapping("/result/t1")
public void test1(HttpServletRequest req, HttpServletResponse rsp) throws IOException {
rsp.getWriter().println("Hello,Spring BY servlet API");
}
11
12
13
14
15
@RequestMapping("/result/t2")
public void test2(HttpServletRequest req, HttpServletResponse rsp) throws IOException {
rsp.sendRedirect("/index.jsp");
}
16
17
18
19
20
21
@RequestMapping("/result/t3")
public void test3(HttpServletRequest req, HttpServletResponse rsp) throws Exception {
//转发
req.setAttribute("msg","/result/t3"); req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,rsp);
}
}

1.  **、SpringMVC**

**通过 SpringMVC 来实现转发和重定向 - 无需视图解析器；**
测试前，需要将视图解析器注释掉

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
@Controller
public class ResultSpringMVC { @RequestMapping("/rsm/t1") public String test1(){
//转发
return "/index.jsp";
}
@RequestMapping("/rsm/t2") public String test2(){
//转发二
return "forward:/index.jsp";
}

@RequestMapping("/rsm/t3")

16
17
18
19
20
public String test3(){
//重定向
return "redirect:/index.jsp";
}
}

**通过 SpringMVC 来实现转发和重定向 - 有视图解析器；**
重定向 , 不需要视图解析器 , 本质就是重新请求一个新地方嘛 , 所以注意路径问题. 可以重定向到另外一个请求实现 .
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
@Controller
public class ResultSpringMVC2 { @RequestMapping("/rsm2/t1") public String test1(){
//转发
return "test";
}
@RequestMapping("/rsm2/t2") public String test2(){
//重定向
return "redirect:/index.jsp";
//return "redirect:hello.do"; //hello.do 为另一个请求/
}
}

**6、数据处理**

1.  **、处理提交数据**

**1、提交的域名称和处理方法的参数名一致**
提交数据 : http://localhost:8080/hello?name=kuangshen
处理方法 :

1. @RequestMapping("/hello")
1. public String hello(String name){
1. System.out.println(name);
1. return "hello"; 5 }

后台输出 : kuangshen

**2、提交的域名称和处理方法的参数名不一致**
提交数据 : http://localhost:8080/hello?username=kuangshen
处理方法 :

1. //@RequestParam("username") : username 提交的域的名称 .
1. @RequestMapping("/hello")
1. public String hello(@RequestParam("username") String name){
1. System.out.println(name);
1. return "hello"; 6 }

后台输出 : kuangshen

**3、提交的是一个对象**
要求提交的表单域和对象的属性名一致 , 参数使用对象即可

1. 实体类

| 1
2
3
4
5
6
7
8 | public class User { private int id; private String name; private int age;
//构造
//get/set
//tostring()
} |
| --- | --- |

1. 提交数据 : http://localhost:8080/mvc04/user?name=kuangshen&id=1&age=15
1. 处理方法 :

| 1
2
3
4
5 | @RequestMapping("/user") public String user(User user){
System.out.println(user); return "hello";
} |
| --- | --- |

后台输出 : User { id=1, name='kuangshen', age=15 }
说明：如果使用对象的话，前端传递的参数名和对象名必须一致，否则就是 null。

1.  **、数据显示到前端**

**第一种 : 通过 ModelAndView**
我们前面一直都是如此 . 就不过多解释

1 public class ControllerTest1 implements Controller { 2
3
4
5
6
7
8
9
10
public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
//返回一个模型视图对象
ModelAndView mv = new ModelAndView(); mv.addObject("msg","ControllerTest1"); mv.setViewName("test");
return mv;
}
}

**第二种 : 通过 ModelMap**
ModelMap

1. @RequestMapping("/hello")
1. public String hello(@RequestParam("username") String name, ModelMap model){
1. //封装要显示到视图中的数据
1. //相当于 req.setAttribute("name",name);
1. model.addAttribute("name",name);
1. System.out.println(name);
1. return "hello"; 8 }

**第三种 : 通过 Model**
Model

1. @RequestMapping("/ct2/hello")
1. public String hello(@RequestParam("username") String name, Model model){
1. //封装要显示到视图中的数据
1. //相当于 req.setAttribute("name",name);
1. model.addAttribute("msg",name);
1. System.out.println(name);
1. return "test"; 8 }

   1. **、对比**

就对于新手而言简单来说使用区别就是：

| 1   | Model 只有寥寥几个方法只适合用于储存数据，简化了新手对于 Model 对象的操作和理解；     |
| --- | ------------------------------------------------------------------------------------- |
| 2   |                                                                                       |
| 3   | ModelMap 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特 |
|     | 性；                                                                                  |
| 4   |                                                                                       |
| 5   | ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。 |

当然更多的以后开发考虑的更多的是性能和优化，就不能单单仅限于此的了解。
**请使用 80%的时间打好扎实的基础，剩下 18%的时间研究框架，2%的时间去学点英文，框架的官方文档 永远是最好的教程。**

1.  **、乱码问题**

测试步骤：

1. 我们可以在首页编写一个提交的表单
| 1
2
3
4 | <form action="/e/t" method="post">
<input type="text" name="name">
<input type="submit">
</form> |
| --- | --- |

1. 后台编写对应的处理类

| 1
2
3
4
5
6
7
8 | @Controller
public class Encoding { @RequestMapping("/e/t")
public String test(Model model,String name){ model.addAttribute("msg",name); //获取表单提交的值 return "test"; //跳转到 test 页面显示输入的值
}
} |
| --- | --- |

1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834548079-18f371bd-2bec-4967-b38c-6bdca2c66c6b.png#)输入中文测试，发现乱码

不得不说，乱码问题是在我们开发中十分常见的问题，也是让我们程序猿比较头大的问题！
以前乱码问题通过过滤器解决 , 而 SpringMVC 给我们提供了一个过滤器 , 可以在 web.xml 中配置 .
修改了 xml 文件需要重启服务器！

1.  <filter>
1.  <filter-name>encoding</filter-name>
1.      <filter- class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
1.  <init-param>
1.  <param-name>encoding</param-name>
1.  <param-value>utf-8</param-value>
1.  </init-param>
1.  </filter>
1.  <filter-mapping>
1.  <filter-name>encoding</filter-name>
1.  <url-pattern>/\*</url-pattern>
1.  </filter-mapping>

但是我们发现 , 有些极端情况下.这个过滤器对 get 的支持不好 .
处理方法 :

1. 修改 tomcat 配置文件 ： 设置编码！

| 1
2
3 | <Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1" connectionTimeout="20000"
redirectPort="8443" /> |
| --- | --- |

1. 自定义过滤器

1 package com.kuang.filter; 2

1. import javax.servlet.\*;
1. import javax.servlet.http.HttpServletRequest;
1. import javax.servlet.http.HttpServletRequestWrapper;
1. import javax.servlet.http.HttpServletResponse;
1. import java.io.IOException;
1. import java.io.UnsupportedEncodingException;
1. import java.util.Map; 10

11 /\*_
12 _ 解决 get 和 post 请求 全部乱码的过滤器
13 \*/
14 public class GenericEncodingFilter implements Filter { 15

1. @Override
1. public void destroy() {

18 }
19

1.  @Override
1.      public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
1.  //处理 response 的字符编码
1.  HttpServletResponse myResponse=(HttpServletResponse) response;
1.  myResponse.setContentType("text/html;charset=UTF-8"); 25
1.  // 转型为与协议相关对象
1.      HttpServletRequest httpServletRequest = (HttpServletRequest) request;
1.  // 对 request 包装增强
1.      HttpServletRequest myrequest = new MyRequest(httpServletRequest);
1.  chain.doFilter(myrequest, response); 31 }

32

1.  @Override
1.      public void init(FilterConfig filterConfig) throws ServletException {

35 }
36
37 }
38

1. //自定义 request 对象，HttpServletRequest 的包装类
1. class MyRequest extends HttpServletRequestWrapper { 41
1. private HttpServletRequest request;
1. //是否编码的标记
1. private boolean hasEncode;
1. //定义一个可以传入 HttpServletRequest 对象的构造函数，以便对其进行装饰
1. public MyRequest(HttpServletRequest request) {
1. super(request);// super 必须写
1. this.request = request; 49 }

50

1.  // 对需要增强方法 进行覆盖
1.  @Override
1.  public Map getParameterMap() {
1.  // 先获得请求方式
1.  String method = request.getMethod();
1.  if (method.equalsIgnoreCase("post")) {
1.  // post 请求
1.  try {
1.  // 处理 post 乱码
1.  request.setCharacterEncoding("utf-8");
1.  return request.getParameterMap();
1.  } catch (UnsupportedEncodingException e) {
1.  e.printStackTrace(); 64 }
1.  } else if (method.equalsIgnoreCase("get")) {
1.  // get 请求
1.      Map<String, String[]> parameterMap = request.getParameterMap();
1.  if (!hasEncode) { // 确保 get 手动编码逻辑只运行一次
1.  for (String parameterName : parameterMap.keySet()) {
1.  String[] values = parameterMap.get(parameterName);
1.  if (values != null) {
1.  for (int i = 0; i < values.length; i++) {
1.  try {
1.  // 处理 get 乱码
1.  values[i] = new String(values[i]

76 .getBytes("ISO-8859-1"), "utf-
8");

1. } catch (UnsupportedEncodingException e) {
1. e.printStackTrace();

79 }
80 }
81 }
82 }
83 hasEncode = true; 84 }
85 return parameterMap; 86 }
87 return super.getParameterMap(); 88 }
89

1. //取一个值
1. @Override
1. public String getParameter(String name) {
1. Map<String, String[]> parameterMap = getParameterMap();
1. String[] values = parameterMap.get(name);
1. if (values == null) {
1. return null;

97 }
98 return values[0]; // 取回参数的第一个值
99 }
100

1. //取所有值
1. @Override
1. public String[] getParameterValues(String name) {
1. Map<String, String[]> parameterMap = getParameterMap();
1. String[] values = parameterMap.get(name);
1. return values;

107 }
108 }

这个也是我在网上找的一些大神写的，一般情况下，SpringMVC 默认的乱码处理就已经能够很好的解决了！
**然后在 web.xml 中配置这个过滤器即可！**
乱码问题，需要平时多注意，在尽可能能设置编码的地方，都设置为统一编码 UTF-8！

**7、整合 SSM**

1.  **、环境要求**

环境：
IDEA
MySQL 5.7.19
Tomcat 9
Maven 3.6
要求：
需要熟练掌握 MySQL 数据库，Spring，JavaWeb 及 MyBatis 知识，简单的前端知识；

1.  **、数据库环境**

创建一个存放书籍数据的数据库表

| 1   | CREATE DATABASE `ssmbuild`;                                          |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | USE `ssmbuild`;                                                      |
| 4   |                                                                      |
| 5   | DROP TABLE IF EXISTS `books`;                                        |
| 6   |                                                                      |
| 7   | CREATE TABLE `books` (                                               |
| 8   | `bookID` INT(10) NOT NULL AUTO_INCREMENT COMMENT '书 id',            |
| 9   | `bookName` VARCHAR(100) NOT NULL COMMENT '书名',                     |
| 10  | `bookCounts` INT(11) NOT NULL COMMENT '数量',                        |
| 11  | `detail` VARCHAR(200) NOT NULL COMMENT '描述',                       |
| 12  | KEY `bookID` (`bookID`)                                              |
| 13  | ) ENGINE=INNODB DEFAULT CHARSET=utf8                                 |
| 14  |                                                                      |
| 15  | INSERT INTO `books`(`bookID`,`bookName`,`bookCounts`,`detail`)VALUES |
| 16  | (1,'Java',1,'从入门到放弃'),                                         |
| 17  | (2,'MySQL',10,'从删库到跑路'),                                       |
| 18  | (3,'Linux',5,'从进门到进牢');                                        |

1. **、基本环境搭建**
1. 新建一 Maven 项目！ ssmbuild ， 添加 web 的支持
1. 导入相关的 pom 依赖！

1 <dependencies> 2 <!--Junit-->

1. <dependency>
1. <groupId>junit</groupId>
1. <artifactId>junit</artifactId>
1. <version>4.12</version>
1. </dependency>

8 <!--数据库驱动-->
9 <dependency>

1. <groupId>mysql</groupId>
1. <artifactId>mysql-connector-java</artifactId>
1. <version>5.1.47</version>
1. </dependency>

14 <!-- 数据库连接池 -->

1. <dependency>
1. <groupId>com.mchange</groupId>
1. <artifactId>c3p0</artifactId>
1. <version>0.9.5.2</version>
1. </dependency> 20
1. <!--Servlet - JSP -->
1. <dependency>
1. <groupId>javax.servlet</groupId>
1. <artifactId>servlet-api</artifactId>
1. <version>2.5</version>
1. </dependency>
1. <dependency>
1. <groupId>javax.servlet.jsp</groupId>
1. <artifactId>jsp-api</artifactId>
1. <version>2.2</version>
1. </dependency>
1. <dependency>
1. <groupId>javax.servlet</groupId>
1. <artifactId>jstl</artifactId>
1. <version>1.2</version>
1. </dependency> 37
1. <!--Mybatis-->
1. <dependency>
1. <groupId>org.mybatis</groupId>
1. <artifactId>mybatis</artifactId>
1. <version>3.5.2</version>
1. </dependency>
1. <dependency>
1. <groupId>org.mybatis</groupId>
1. <artifactId>mybatis-spring</artifactId>
1. <version>2.0.2</version>
1. </dependency> 49

50 <!--Spring-->

1. <dependency>
1. <groupId>org.springframework</groupId>
1. <artifactId>spring-webmvc</artifactId>
1. <version>5.1.9.RELEASE</version>
1. </dependency>
1. <dependency>
1. <groupId>org.springframework</groupId>
1. <artifactId>spring-jdbc</artifactId>
1. <version>5.1.9.RELEASE</version>
1. </dependency>

61 </dependencies>

1. Maven 资源过滤设置

1. <build>
1. <resources>
1. <resource>
1. <directory>src/main/java</directory>
1. <includes>
1. <include>\*_/_.properties</include>
1. <include>\*_/_.xml</include>
1. </includes>
1. <filtering>false</filtering>
1. </resource>
1. <resource>
1. <directory>src/main/resources</directory>
1. <includes>
1. <include>\*_/_.properties</include>
1. <include>\*_/_.xml</include>
1. </includes>
1. <filtering>false</filtering>
1. </resource>
1. </resources>
1. </build>

1. 建立基本结构和配置框架！ com.kuang.pojo com.kuang.dao

com.kuang.service com.kuang.controller mybatis-conﬁg.xml
1
2
3
4
5
6
7

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN" ["http://mybatis.org/dtd/mybatis-3-config.dtd](http://mybatis.org/dtd/mybatis-3-config.dtd)">
<configuration>
</configuration>

applicationContext.xml

1. <?xml version="1.0" encoding="UTF-8"?>
1. <beans xmlns=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)"
1. xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" 4

xsi:schemaLocation=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
5 [http://www.springframework.org/schema/beans/spring-](http://www.springframework.org/schema/beans/spring-) beans.xsd">
6
7 </beans>

1. **、Mybatis 层编写**
1. 数据库配置文件 **database.properties**

| 1   | jdbc.driver=com.mysql.jdbc.Driver                  |
| --- | -------------------------------------------------- |
| 2   | jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?     |
|     | useSSL=true&useUnicode=true&characterEncoding=utf8 |
| 3   | jdbc.username=root                                 |
| 4   | jdbc.password=123456                               |

1. IDEA 关联数据库
1. 编写 MyBatis 的核心配置文件

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

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN" ["http://mybatis.org/dtd/mybatis-3-config.dtd](http://mybatis.org/dtd/mybatis-3-config.dtd)">
<configuration>
<typeAliases>
<package name="com.kuang.pojo"/>
</typeAliases>
<mappers>
<mapper resource="com/kuang/dao/BookMapper.xml"/>
</mappers>

</configuration>

1. 编写数据库对应的实体类 com.kuang.pojo.Books

使用 lombok 插件！

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
package com.kuang.pojo;
import lombok.AllArgsConstructor; import lombok.Data;
import lombok.NoArgsConstructor;
@Data @AllArgsConstructor @NoArgsConstructor
public class Books {
private int bookID; private String bookName; private int bookCounts;
private String detail;
}

1. 编写 Dao 层的 Mapper 接口！

| 1   | package com.kuang.dao;       |
| --- | ---------------------------- |
| 2   |                              |
| 3   | import com.kuang.pojo.Books; |
| 4   | import java.util.List;       |
| 5   |                              |

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
public interface BookMapper {
//增加一个 Book
int addBook(Books book);

//根据 id 删除一个 Book
int deleteBookById(int id);

//更新 Book
int updateBook(Books books);

//根据 id 查询,返回一个 Book Books queryBookById(int id);

//查询全部 Book,返回 list 集合 List<Books> queryAllBook();
}

1. 编写接口对应的 Mapper.xml 文件。需要导入 MyBatis 的包；

   1. <?xml version="1.0" encoding="UTF-8" ?>
   1. <!DOCTYPE mapper
   1. PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
   1. ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)"> 5

6 <mapper namespace="com.kuang.dao.BookMapper"> 7

1. <!--增加一个Book-->
1. <insert id="addBook" parameterType="Books">
1. insert into ssmbuild.books(bookName,bookCounts,detail)
1. values (#{bookName}, #{bookCounts}, #{detail})
1. </insert> 13
1. <!--根据id删除一个Book-->
1. <delete id="deleteBookById" parameterType="int">
1. delete from ssmbuild.books where bookID=#{bookID}
1. </delete> 18

19 <!--更新Book-->

1. <update id="updateBook" parameterType="Books">
1. update ssmbuild.books
1. set bookName = #{bookName},bookCounts = #{bookCounts},detail = #

{detail}

1. where bookID = #{bookID}
1. </update> 25
1. <!--根据id查询,返回一个Book-->
1. <select id="queryBookById" resultType="Books">
1. select \* from ssmbuild.books
1. where bookID = #{bookID}
1. </select> 31
1. <!--查询全部Book-->
1. <select id="queryAllBook" resultType="Books">
1. SELECT \* from ssmbuild.books
1. </select> 36

37 </mapper>

1. 编写 Service 层的接口和实现类接口：

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
package com.kuang.service;
import com.kuang.pojo.Books;

import java.util.List;

//BookService:底下需要去实现,调用 dao 层 public interface BookService {
//增加一个 Book
int addBook(Books book);
//根据 id 删除一个 Book
int deleteBookById(int id);
//更新 Book
int updateBook(Books books);
//根据 id 查询,返回一个 Book Books queryBookById(int id);
//查询全部 Book,返回 list 集合 List<Books> queryAllBook();
}

实现类：

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
package com.kuang.service;
import com.kuang.dao.BookMapper; import com.kuang.pojo.Books; import java.util.List;

public class BookServiceImpl implements BookService {

//调用 dao 层的操作，设置一个 set 接口，方便 Spring 管理 private BookMapper bookMapper;

public void setBookMapper(BookMapper bookMapper) { this.bookMapper = bookMapper;
}

public int addBook(Books book) { return bookMapper.addBook(book);
}

public int deleteBookById(int id) { return bookMapper.deleteBookById(id);
}

public int updateBook(Books books) { return bookMapper.updateBook(books);
}

public Books queryBookById(int id) { return bookMapper.queryBookById(id);
}

31
32
33
34
35
public List<Books> queryAllBook() { return bookMapper.queryAllBook();
}
}

**OK，到此，底层需求操作编写完毕！**

1.  **、Spring 层**
1.  配置**Spring 整合 MyBatis**，我们这里数据源使用 c3p0 连接池；
1.  我们去编写 Spring 整合 Mybatis 的相关的配置文件； spring-dao.xml

    1.  <?xml version="1.0" encoding="UTF-8"?>
    1.  <beans xmlns=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)"
    1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
    1.  xmlns:context=["http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)"
    1.  xsi:schemaLocation=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
    1.  [http://www.springframework.org/schema/beans/spring-beans.xsd](http://www.springframework.org/schema/beans/spring-beans.xsd)
    1.  [http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)
    1.      [https://www.springframework.org/schema/context/spring-](http://www.springframework.org/schema/context/spring-) context.xsd">

9
10 <!-- 配置整合mybatis -->
11 <!-- 1.关联数据库文件 -->
12 <context:property-placeholder location="classpath:database.properties"/>
13
14 <!-- 2.数据库连接池 -->

1. <!--数据库连接池
1. dbcp 半自动化操作 不能自动连接
1. c3p0 自动化操作（自动的加载配置文件 并且设置到对象里面）

18 -->

1.      <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
1.  <!-- 配置连接池属性 -->
1.  <property name="driverClass" value="${jdbc.driver}"/>
1.  <property name="jdbcUrl" value="${jdbc.url}"/>
1.  <property name="user" value="${jdbc.username}"/>
1.  <property name="password" value="${jdbc.password}"/> 25
1.  <!-- c3p0连接池的私有属性 -->
1.  <property name="maxPoolSize" value="30"/>
1.  <property name="minPoolSize" value="10"/>
1.  <!-- 关闭连接后不自动commit -->
1.  <property name="autoCommitOnClose" value="false"/>
1.  <!-- 获取连接超时时间 -->
1.  <property name="checkoutTimeout" value="10000"/>
1.  <!-- 当获取连接失败重试次数 -->
1.  <property name="acquireRetryAttempts" value="2"/>
1.  </bean> 36
1.  <!-- 3.配置SqlSessionFactory对象 -->
1.      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">

1.  <!-- 注入数据库连接池 -->
1.  <property name="dataSource" ref="dataSource"/>
1.  <!-- 配置MyBaties全局配置文件:mybatis-config.xml -->
1.      <property name="configLocation" value="classpath:mybatis- config.xml"/>
1.  </bean> 44

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

<!-- 4.配置扫描Dao接口包，动态实现Dao接口注入到spring容器中 -->
<!--解释 ： [https://www.cnblogs.com/jpfss/p/7799806.html--](http://www.cnblogs.com/jpfss/p/7799806.html--)>
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<!-- 注入sqlSessionFactory -->
<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
<!-- 给出需要扫描Dao接口包 -->
<property name="basePackage" value="com.kuang.dao"/>
</bean>
</beans>

1.  **Spring 整合 service 层**

1.  <?xml version="1.0" encoding="UTF-8"?>
1.  <beans xmlns=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)"
1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1.  xmlns:context=["http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)"
1.  xsi:schemaLocation=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
1.  [http://www.springframework.org/schema/beans/spring-beans.xsd](http://www.springframework.org/schema/beans/spring-beans.xsd)
1.  [http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)
1.  [http://www.springframework.org/schema/context/spring-context.xsd](http://www.springframework.org/schema/context/spring-context.xsd)"> 9
1.  <!-- 扫描service相关的bean -->
1.  <context:component-scan base-package="com.kuang.service" /> 12
1.  <!--BookServiceImpl注入到IOC容器中-->
1.      <bean id="BookServiceImpl" class="com.kuang.service.BookServiceImpl">
1.  <property name="bookMapper" ref="bookMapper"/>
1.  </bean> 17
1.  <!-- 配置事务管理器 -->
1.      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"

>

1. <!-- 注入数据库连接池 -->
1. <property name="dataSource" ref="dataSource" />
1. </bean> 23

24 </beans>

Spring 层搞定！再次理解一下，Spring 就是一个大杂烩，一个容器！对吧！

1.  **、SpringMVC 层**
1.  **web.xml**
    1.  <?xml version="1.0" encoding="UTF-8"?>
    1.  <web-app xmlns=["http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)"
    1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
    1.      xsi:schemaLocation=["http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)[ http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd](http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd)"
    1.  version="4.0">

6

1.  <!--DispatcherServlet-->
1.  <servlet>
1.  <servlet-name>DispatcherServlet</servlet-name>
1.      <servlet- class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
1.  <init-param>
1.  <param-name>contextConfigLocation</param-name>
1.  <!--一定要注意:我们这里加载的是总的配置文件，之前被这里坑了！-->
1.  <param-value>classpath:applicationContext.xml</param-value>
1.  </init-param>
1.  <load-on-startup>1</load-on-startup>
1.  </servlet>
1.  <servlet-mapping>
1.  <servlet-name>DispatcherServlet</servlet-name>
1.  <url-pattern>/</url-pattern>
1.  </servlet-mapping> 22
1.  <!--encodingFilter-->
1.  <filter>
1.  <filter-name>encodingFilter</filter-name>
1.  <filter-class>
1.  org.springframework.web.filter.CharacterEncodingFilter
1.  </filter-class>
1.  <init-param>
1.  <param-name>encoding</param-name>
1.  <param-value>utf-8</param-value>
1.  </init-param>
1.  </filter>
1.  <filter-mapping>
1.  <filter-name>encodingFilter</filter-name>
1.  <url-pattern>/\*</url-pattern>
1.  </filter-mapping> 38
1.  <!--Session过期时间-->
1.  <session-config>
1.  <session-timeout>15</session-timeout>
1.  </session-config> 43

44 </web-app>

### spring-mvc.xml

1. <?xml version="1.0" encoding="UTF-8"?>
1. <beans xmlns=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)"
1. xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1. xmlns:context=["http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)"
1. xmlns:mvc=["http://www.springframework.org/schema/mvc](http://www.springframework.org/schema/mvc)"
1. xsi:schemaLocation=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
1. [http://www.springframework.org/schema/beans/spring-beans.xsd](http://www.springframework.org/schema/beans/spring-beans.xsd)
1. [http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)

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
[http://www.springframework.org/schema/context/spring-context.xsd](http://www.springframework.org/schema/context/spring-context.xsd) [http://www.springframework.org/schema/mvc](http://www.springframework.org/schema/mvc) https://[www.springframework.org/schema/mvc/spring-mvc.xsd](http://www.springframework.org/schema/mvc/spring-mvc.xsd)">

<!-- 配置SpringMVC -->
<!-- 1.开启SpringMVC注解驱动 -->

<mvc:annotation-driven />

<!-- 2.静态资源默认servlet配置-->

<mvc:default-servlet-handler/>
21
22
23
24
25
26
27
28
29

<!-- 3.配置jsp 显示ViewResolver视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver ">
<property name="viewClass" value="org.springframework.web.servlet.view.JstlView"  />
<property name="prefix" value="/WEB-INF/jsp/" />
<property name="suffix" value=".jsp" />
</bean>
<!-- 4.扫描web相关的bean -->
<context:component-scan base-package="com.kuang.controller" />

</beans>

1. **Spring 配置整合文件，applicationContext.xml**

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

<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)" xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" xsi:schemaLocation="[http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
[http://www.springframework.org/schema/beans/spring-beans.xsd](http://www.springframework.org/schema/beans/spring-beans.xsd)">
<import resource="spring-dao.xml"/>
<import resource="spring-service.xml"/>
<import resource="spring-mvc.xml"/>

</beans>

**配置文件，暂时结束！Controller 和 视图层编写**

1. BookController 类编写 ， 方法一：查询全部书籍

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
@Controller
@RequestMapping("/book") public class BookController {
@Autowired @Qualifier("BookServiceImpl") private BookService bookService;

@RequestMapping("/allBook") public String list(Model model) {
List<Books> list = bookService.queryAllBook(); model.addAttribute("list", list);
return "allBook";

14 }
15 }

1. 编写首页 **index.jsp**

1. <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
1. <!DOCTYPE HTML>
1. <html>
1. <head>
1. <title>首页</title>
1. <style type="text/css"> 7		a {
1. text-decoration: none;
1. color: black;
1. font-size: 18px; 11 }

12 h3 {

1. width: 180px;
1. height: 38px;
1. margin: 100px auto;
1. text-align: center;
1. line-height: 38px;
1. background: deepskyblue;
1. border-radius: 4px; 20 }
1. </style>
1. </head>
1. <body> 24

25
26
27
28
29

<h3>
<a href="${pageContext.request.contextPath}/book/allBook">点击进入列表页</a>
</h3>
</body>
</html>

1. 书籍列表页面 **allbook.jsp**

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
<%@ taglib prefix="c" uri=["http://java.sun.com/jsp/jstl/core](http://java.sun.com/jsp/jstl/core)" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
<title>书籍列表</title>
<meta name="viewport" content="width=device-width, initial- scale=1.0">
<!-- 引入 Bootstrap -->
<link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container">

<div class="row clearfix">
<div class="col-md-12 column">
<div class="page-header">

17 <h1>
18

<small>书籍列表 —— 显示所有书籍</small>

19 </h1>

1. </div>
1. </div>
1. </div> 23
1. <div class="row">
1. <div class="col-md-4 column">
1. <a class="btn btn-primary"

href="${pageContext.request.contextPath}/book/toAddBook">新增</a>

1. </div>
1. </div> 29
1. <div class="row clearfix">
1. <div class="col-md-12 column">
1. <table class="table table-hover table-striped">
1. <thead>
1. <tr>
1. <th>书籍编号</th>
1. <th>书籍名字</th>
1. <th>书籍数量</th>
1. <th>书籍详情</th>
1. <th>操作</th>

40 </tr>
41 </thead>
42

1.  <tbody>
1.      <c:forEach var="book" items="${requestScope.get('list')}">
1.  <tr>
1.  <td>${book.getBookID()}</td>
1.  <td>${book.getBookName()}</td>
1.  <td>${book.getBookCounts()}</td>
1.  <td>${book.getDetail()}</td>
1.  <td>
1.  <a

href="${pageContext.request.contextPath}/book/toUpdateBook?
id=${book.getBookID()}">更改</a> |

1. <a

href="${pageContext.request.contextPath}/book/del/${book.getBookID()}">
删除</a>
53 </td>
54 </tr>

1. </c:forEach>
1. </tbody>
1. </table>
1. </div>
1. </div>
1. </div>

1. BookController 类编写 ， 方法二：添加书籍

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
@RequestMapping("/toAddBook") public String toAddPaper() {
return "addBook";
}
@RequestMapping("/addBook")
public String addPaper(Books books) { System.out.println(books); bookService.addBook(books); return "redirect:/book/allBook";
}

1. 添加书籍页面：**addBook.jsp**

1. <%@ taglib prefix="c" uri=["http://java.sun.com/jsp/jstl/core](http://java.sun.com/jsp/jstl/core)" %>
1. <%@ page contentType="text/html;charset=UTF-8" language="java" %> 3

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

<html>
<head>
<title>新增书籍</title>
<meta name="viewport" content="width=device-width, initial- scale=1.0">
<!-- 引入 Bootstrap -->
<link href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
<div class="container">
24
25
26
27
28
29
30
<div class="row clearfix">
<div class="col-md-12 column">
<div class="page-header">
<h1>
<small>新增书籍</small>
</h1>
</div>
</div>
</div>
<form action="${pageContext.request.contextPath}/book/addBook" method="post">
书籍名称：<input type="text" name="bookName"><br><br><br>
书籍数量：<input type="text" name="bookCounts"><br><br><br>
书籍详情：<input type="text" name="detail"><br><br><br>
<input type="submit" value="添加">
</form>
</div>

1. BookController 类编写 ， 方法三：修改书籍

1. @RequestMapping("/toUpdateBook")
1. public String toUpdateBook(Model model, int id) {
1. Books books = bookService.queryBookById(id);
1. System.out.println(books);
1. model.addAttribute("book",books );
1. return "updateBook";

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
}
@RequestMapping("/updateBook")
public String updateBook(Model model, Books book) { System.out.println(book); bookService.updateBook(book);
Books books = bookService.queryBookById(book.getBookID()); model.addAttribute("books", books);
return "redirect:/book/allBook";
}

1.  修改书籍页面 **updateBook.jsp**

    1.  <%@ taglib prefix="c" uri=["http://java.sun.com/jsp/jstl/core](http://java.sun.com/jsp/jstl/core)" %>
    1.  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    1.  <html>
    1.  <head>
    1.  <title>修改信息</title>
    1.      <meta name="viewport" content="width=device-width, initial- scale=1.0">
    1.  <!-- 引入 Bootstrap -->
    1.  <link

href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

1.  </head>
1.  <body>
1.  <div class="container"> 12
1.  <div class="row clearfix">
1.  <div class="col-md-12 column">
1.  <div class="page-header"> 16		<h1>

17 <small>修改信息</small>
18 </h1>

1.  </div>
1.  </div>
1.  </div> 22
1.      <form action="${pageContext.request.contextPath}/book/updateBook" method="post">
1.  <input type="hidden" name="bookID" value="${book.getBookID()}"/>
1.      书籍名称：<input type="text" name="bookName" value="${book.getBookName()}"/>
1.      书籍数量：<input type="text" name="bookCounts" value="${book.getBookCounts()}"/>
1.      书籍详情：<input type="text" name="detail" value="${book.getDetail() }"/>
1.  <input type="submit" value="提交"/>
1.  </form> 30

31 </div>

1. BookController 类编写 ， 方法四：删除书籍
   | 1
   2
   3
   4
   5 | @RequestMapping("/del/{bookId}")
   public String deleteBook(@PathVariable("bookId") int id) { bookService.deleteBookById(id);
   return "redirect:/book/allBook";
   } |
   | --- | --- |

**配置 Tomcat，进行运行！**
到目前为止，这个 SSM 项目整合已经完全的 OK 了，可以直接运行进行测试！这个练习十分的重要，大家需要保证，不看任何东西，自己也可以完整的实现出来！
**项目结构图**
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834548487-a58b0736-cf0a-45a9-b03b-f9624524d28c.png#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834548728-fadf5edf-3c02-40d3-9332-007790aa511f.png#)

1.  **、小结及展望**

这个是同学们的第一个 SSM 整合案例，一定要烂熟于心！
SSM 框架的重要程度是不言而喻的，学到这里，大家已经可以进行基本网站的单独开发。但是这只是增删改查的基本操作。可以说学到这里，大家才算是真正的步入了后台开发的门。也就是能找一个后台相 关工作的底线。
或许很多人，工作就做这些事情，但是对于个人的提高来说，还远远不够！ 我们后面还要学习一些 SpringMVC 的知识！
Ajax 和 Json 文件上传和下载拦截器
SpringBoot、SpringCloud 开发！

1. **、实现查询书籍功能**
1. 前端页面增加一个输入框和查询按钮

| 1
2
3

4

5
6
7 | <div class="col-md-4 column"></div>

<div class="col-md-4 column">
<form class="form-inline" action="/book/queryBook" method="post" style="float: right">
<input type="text" name="queryBookName" class="form-control"
placeholder="输入查询书名" required>
<input type="submit" value="查询" class="btn btn-primary">
</form>
</div> |
| --- | --- |

1. 编写查询的 Controller

| 1
2
3
4
5
6 | @RequestMapping("/queryBook")
public String queryBook(String queryBookName,Model model){
System.out.println("要查询的书籍:"+queryBookName);
//业务逻辑还没有写 return "allBook";
} |
| --- | --- |

1. 由于底层没有实现，所以我们要将底层代码先搞定
1. Mapper 接口

| 1
2 | //根据 id 查询,返回一个 Book
Books queryBookByName(String bookName); |
| --- | --- |

1. Mapper.xml

| 1
2
3
4
5 | <!--根据书名查询,返回一个Book-->
<select id="queryBookByName" resultType="Books"> select \* from ssmbuild.books
where bookName = #{bookName}
</select> |
| --- | --- |

1. Service 接口
   | 1
   2 | //根据 id 查询,返回一个 Book
   Books queryBookByName(String bookName); |
   | --- | --- |

1. Service 实现类

| 1
2
3 | public Books queryBookByName(String bookName) { return bookMapper.queryBookByName(bookName);
} |
| --- | --- |

1. 完善 Controller

| 1
2
3
4
5
6
7
8
9 | @RequestMapping("/queryBook")
public String queryBook(String queryBookName,Model model){
System.out.println("要查询的书籍:"+queryBookName);
Books books = bookService.queryBookByName(queryBookName); List<Books> list = new ArrayList<Books>(); list.add(books);
model.addAttribute("list", list); return "allBook";
} |
| --- | --- |

1. 测试，查询功能 OK！
1. 无聊优化！我们发现查询的东西不存在的时候，查出来的页面是空的，我们可以提高一下用户的体 验性！
   1. 在前端添加一个展示全部书籍的按钮

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834549007-ee8f6ceb-ecfa-4046-97ab-c0d642c069ab.png#)

1.  并在后台增加一些判断性的代码！

1.  @RequestMapping("/queryBook")
1.  public String queryBook(String queryBookName,Model model){
1.  System.out.println("要查询的书籍:"+queryBookName);
1.  //如果查询的数据存在空格，则优化
1.      Books books = bookService.queryBookByName(queryBookName.trim());
1.  List<Books> list = new ArrayList<Books>();
1.  list.add(books);
1.  //如果没有查出来书籍，则返回全部书籍列表
1.  if (books==null){
1.  list = bookService.queryAllBook();
1.  model.addAttribute("error", "没有找到本书！"); 12 }
1.  model.addAttribute("list", list);
1.  return "allBook"; 15 }

    1. 将错误信息展示在前台页面！完整的查询栏代码

1.  <div class="row">
1.  <div class="col-md-4 column">
1.  <a class="btn btn-primary"

href="${pageContext.request.contextPath}/book/toAddBook">新增</a>

1. <a class="btn btn-primary"

href="${pageContext.request.contextPath}/book/allBook">显示全部书籍
</a>

1.  </div>
1.  <div class="col-md-8 column">
1.      <form class="form-inline" action="/book/queryBook" method="post" style="float: right">
1.  <span style="color:red;font-weight: bold">${error}

</span>

1.      <input type="text" name="queryBookName" class="form- control" placeholder="输入查询书名" required>
1.  <input type="submit" value="查询" class="btn btn-

primary">

1. </form>
1. </div>
1. </div>

**8、Json**

1.  **、什么是 JSON？**

JSON( JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。
采用完全独立于编程语言的**文本格式**来存储和表示数据。
简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。
在 JavaScript 语言中，一切都是对象。因此，任何 JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：
对象表示为键值对，数据由逗号分隔花括号保存对象
方括号保存数组
**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

| 1   | {"name": "QinJiang"} |
| --- | -------------------- |
| 2   | {"age": "3"}         |
| 3   | {"sex": "男"}        |

很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：
JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。

| 1

2 | var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个 |
| --- | --- |
| 字符串 | |

**JSON 和 JavaScript 对象互转**
要实现从 JSON 字符串转换为 JavaScript 对象，使用 JSON.parse() 方法：

| 1   | var obj  | = JSON.parse('{"a": "Hello", | "b": | "World"}'); |
| --- | -------- | ---------------------------- | ---- | ----------- |
| 2   | //结果是 | {a: 'Hello', b: 'World'}     |      |             |

要实现从 JavaScript 对象转换为 JSON 字符串，使用 JSON.stringify() 方法：

| 1
2 | var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}' |
| --- | --- |

**代码测试**

1. 新建一个 module ，springmvc-05-json ， 添加 web 的支持
1. 在 web 目录下新建一个 json-1.html ， 编写测试内容

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

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>JSON_秦疆</title>
</head>
<body>
<script type="text/javascript">
//编写一个js的对象var user = {
name:"秦疆", age:3,
sex:"男"
};
//将js对象转换成json字符串
var str = JSON.stringify(user); console.log(str);

//将 json 字符串转换为 js 对象
var user2 = JSON.parse(str); console.log(user2.age,user2.name,user2.sex);

</script>

</body>
</html>

1. 在 IDEA 中使用浏览器打开，查看控制台输出！

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834549506-7e0229d3-ece5-437f-b2a0-de60b80e8638.png#)

1.  **、Controller 返回 JSON 数据**

Jackson 应该是目前比较好的 json 解析工具了
当然工具不止这一个，比如还有阿里巴巴的 fastjson 等等。我们这里使用 Jackson，使用它需要导入它的 jar 包；

| 1

2
3
4
5
6 | <!--
[https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-) core -->
<dependency>
<groupId>com.fasterxml.jackson.core</groupId>
<artifactId>jackson-databind</artifactId>
<version>2.9.8</version>
</dependency> |
| --- | --- |

配置 SpringMVC 需要的配置
web.xml

1.  <?xml version="1.0" encoding="UTF-8"?>
1.  <web-app xmlns=["http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)"
1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1.      xsi:schemaLocation=["http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)[ http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd](http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd)"
1.  version="4.0">

6

1.  <!--1.注册servlet-->
1.  <servlet>
1.  <servlet-name>SpringMVC</servlet-name>
1.      <servlet- class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
1.  <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
1.  <init-param>
1.  <param-name>contextConfigLocation</param-name>
1.  <param-value>classpath:springmvc-servlet.xml</param-value>
1.  </init-param>
1.  <!-- 启动顺序，数字越小，启动越早 -->
1.  <load-on-startup>1</load-on-startup>
1.  </servlet> 19
1.  <!--所有请求都会被springmvc拦截 -->
1.  <servlet-mapping>
1.  <servlet-name>SpringMVC</servlet-name>
1.  <url-pattern>/</url-pattern>
1.  </servlet-mapping> 25

26 <filter>

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
<filter-name>encoding</filter-name>
<filter- class>org.springframework.web.filter.CharacterEncodingFilter</filter- class>
<init-param>
<param-name>encoding</param-name>
<param-value>utf-8</param-value>
</init-param>
</filter>
<filter-mapping>
<filter-name>encoding</filter-name>
<url-pattern>/</url-pattern>
</filter-mapping>
</web-app>

springmvc-servlet.xml

1.  <?xml version="1.0" encoding="UTF-8"?>
1.  <beans xmlns=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)"
1.  xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1.  xmlns:context=["http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)"
1.  xmlns:mvc=["http://www.springframework.org/schema/mvc](http://www.springframework.org/schema/mvc)"
1.  xsi:schemaLocation=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
1.  [http://www.springframework.org/schema/beans/spring-beans.xsd](http://www.springframework.org/schema/beans/spring-beans.xsd)
1.  [http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)
1.      [https://www.springframework.org/schema/context/spring-](http://www.springframework.org/schema/context/spring-) context.xsd
1.  [http://www.springframework.org/schema/mvc](http://www.springframework.org/schema/mvc)
1.  [https://www.springframework.org/schema/mvc/spring-mvc.xsd](http://www.springframework.org/schema/mvc/spring-mvc.xsd)"> 12
1.  <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
1.  <context:component-scan base-package="com.kuang.controller"/> 15

16 <!-- 视图解析器 -->

1.      <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver  "
1.  id="internalResourceViewResolver"> 19 <!-- 前缀 -->

20 <property name="prefix" value="/WEB-INF/jsp/" /> 21 <!-- 后缀 -->

1. <property name="suffix" value=".jsp" />
1. </bean> 24

25 </beans>

我们随便编写一个 User 的实体类，然后我们去编写我们的测试 Controller；

| 1   | package com.kuang.pojo;           |
| --- | --------------------------------- |
| 2   |                                   |
| 3   | import lombok.AllArgsConstructor; |
| 4   | import lombok.Data;               |
| 5   | import lombok.NoArgsConstructor;  |
| 6   |                                   |
| 7   | //需要导入 lombok                 |
| 8   | @Data                             |
| 9   | @AllArgsConstructor               |

10
11
12
13
14
15
16
17
@NoArgsConstructor
public class User {
private String name; private int age;
private String sex;
}

这里我们需要两个新东西，一个是@ResponseBody，一个是 ObjectMapper 对象，我们看下具体 的用法
编写一个 Controller；

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
@Controller
public class UserController {
@RequestMapping("/json1") @ResponseBody
public String json1() throws JsonProcessingException {
//创建一个 jackson 的对象映射器，用来解析数据 ObjectMapper mapper = new ObjectMapper();
//创建一个对象
User user = new User("秦疆 1 号", 3, "男");
//将我们的对象解析成为 json 格式
String str = mapper.writeValueAsString(user);
//由于@ResponseBody 注解，这里会将 str 转成 json 格式返回；十分方便 return str;
}
}

配置 Tomcat ， 启动测试一下！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834549760-9734f6bf-4247-4ef8-aaf8-d9b6895c8ce3.png#)http://localhost:8080/json1

发现出现了乱码问题，我们需要设置一下他的编码格式为 utf-8，以及它返回的类型； 通过@RequestMaping 的 produces 属性来实现，修改下代码

| 1
2 | //produces:指定响应体返回类型和编码@RequestMapping(value = "/json1",produces = "application/json;charset=utf-8") |
| --- | --- |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834550037-ed1b2638-80d3-48ab-8d33-48309eb08e42.png#)再次测试， http://localhost:8080/json1 ， 乱码问题 OK！

【注意：使用 json 记得处理乱码问题】

1.  **、代码优化**

**乱码统一解决**
上一种方法比较麻烦，如果项目中有许多请求则每一个都要添加，可以通过 Spring 配置统一指定，这样就不用每次都去处理了！
我们可以在 springmvc 的配置文件上添加一段消息 StringHttpMessageConverter 转换配置！

1. <mvc:annotation-driven>
1. <mvc:message-converters register-defaults="true">
1. <bean

class="org.springframework.http.converter.StringHttpMessageConverter">

1. <constructor-arg value="UTF-8"/>
1. </bean>
1. <bean

class="org.springframework.http.converter.json.MappingJackson2HttpMessageCon verter">

1. <property name="objectMapper">
1. <bean

class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBe an">

1. <property name="failOnEmptyBeans" value="false"/>
1. </bean>
1. </property>
1. </bean>
1. </mvc:message-converters>
1. </mvc:annotation-driven>

**返回 json 字符串统一解决**
在类上直接使用 **@RestController **，这样子，里面所有的方法都只会返回 json 字符串了，不用再每一个都添加@ResponseBody ！我们在前后端分离开发中，一般都使用 @RestController ，十分便捷！

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
@RestController
public class UserController {
//produces:指定响应体返回类型和编码
@RequestMapping(value = "/json1")
public String json1() throws JsonProcessingException {
//创建一个 jackson 的对象映射器，用来解析数据 ObjectMapper mapper = new ObjectMapper();
//创建一个对象
User user = new User("秦疆 1 号", 3, "男");
//将我们的对象解析成为 json 格式
String str = mapper.writeValueAsString(user);
//由于@ResponseBody 注解，这里会将 str 转成 json 格式返回；十分方便 return str;
}
}

启动 tomcat 测试，结果都正常输出！

1.  **、测试集合输出**

增加一个新的方法

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
@RequestMapping("/json2")
public String json2() throws JsonProcessingException {
//创建一个 jackson 的对象映射器，用来解析数据
ObjectMapper mapper = new ObjectMapper();
//创建一个对象
User user1 = new User("秦疆 1 号", 3, "男"); User user2 = new User("秦疆 2 号", 3, "男"); User user3 = new User("秦疆 3 号", 3, "男"); User user4 = new User("秦疆 4 号", 3, "男"); List<User> list = new ArrayList<User>(); list.add(user1);
list.add(user2); list.add(user3); list.add(user4);

//将我们的对象解析成为 json 格式
String str = mapper.writeValueAsString(list); return str;
}

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834550298-a5f89025-f78c-4d49-b581-92bbfdd9d3ac.jpeg#)运行结果 : 十分完美，没有任何问题！

1.  **、输出时间对象**

增加一个新的方法

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
@RequestMapping("/json3")
public String json3() throws JsonProcessingException {
ObjectMapper mapper = new ObjectMapper();

//创建时间一个对象，java.util.Date Date date = new Date();
//将我们的对象解析成为 json 格式
String str = mapper.writeValueAsString(date); return str;
}

运行结果 :
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834550582-3c609cb6-4870-4363-b572-97439ee3a8d4.png#)

默认日期格式会变成一个数字，是 1970 年 1 月 1 日到当前日期的毫秒数！
Jackson 默认是会把时间转成 timestamps 形式
**解决方案：取消 timestamps 形式 ， 自定义时间格式**

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
@RequestMapping("/json4")
public String json4() throws JsonProcessingException {
ObjectMapper mapper = new ObjectMapper();

//不使用时间戳的方式 mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
//自定义日期格式对象
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
//指定日期格式 mapper.setDateFormat(sdf);

Date date = new Date();
String str = mapper.writeValueAsString(date);

return str;
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834551126-1b1ff66a-43aa-4da2-999a-a73b078e4a60.png#)运行结果 : 成功的输出了时间！

1.  **、抽取为工具类**

**如果要经常使用的话，这样是比较麻烦的，我们可以将这些代码封装到一个工具类中；我们去编写下**

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
package com.kuang.utils;
import com.fasterxml.jackson.core.JsonProcessingException; import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

import java.text.SimpleDateFormat;

public class JsonUtils {

public static String getJson(Object object) { return getJson(object,"yyyy-MM-dd HH:mm:ss");

13
14
15
16
17
18
}
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
public static String getJson(Object object,String dateFormat) { ObjectMapper mapper = new ObjectMapper();
//不使用时间差的方式
mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,
false);
//自定义日期格式对象
SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
//指定日期格式 mapper.setDateFormat(sdf); try {
return mapper.writeValueAsString(object);
} catch (JsonProcessingException e) { e.printStackTrace();
}
return null;
}
}

我们使用工具类，代码就更加简洁了！

1. @RequestMapping("/json5")
1. public String json5() throws JsonProcessingException {
1. Date date = new Date();
1. String json = JsonUtils.getJson(date);
1. return json; 6 }

大工告成！完美！

1.  **、FastJson**

fastjson.jar 是阿里开发的一款专门用于 Java 开发的包，可以方便的实现 json 对象与 JavaBean 对象的转换，实现 JavaBean 对象与 json 字符串的转换，实现 json 对象与 json 字符串的转换。实现 json 的转换方法 很多，最后的实现结果都是一样的。
fastjson 的 pom 依赖！

1. <dependency>
1. <groupId>com.alibaba</groupId>
1. <artifactId>fastjson</artifactId>
1. <version>1.2.60</version>
1. </dependency>

fastjson 三个主要的类：
【JSONObject 代表 json 对象 】
JSONObject 实现了 Map 接口, 猜想 JSONObject 底层操作是由 Map 实现的。
JSONObject 对应 json 对象，通过各种形式的 get()方法可以获取 json 对象中的数据，也可利用 诸如 size()，isEmpty()等方法获取"键：值"对的个数和判断是否为空。其本质是通过实现 Map 接口并调用接口中的方法完成的。

【JSONArray 代表 json 对象数组】
内部是有 List 接口中的方法来完成操作的。

【JSON 代表 JSONObject 和 JSONArray 的转化】JSON 类源码分析与使用
仔细观察这些方法，主要是实现 json 对象，json 对象数组，javabean 对象，json 字符串之间 的相互转化。

**代码测试，我们新建一个 FastJsonDemo 类**

1 package com.kuang.controller; 2

1.  import com.alibaba.fastjson.JSON;
1.  import com.alibaba.fastjson.JSONObject;
1.  import com.kuang.pojo.User; 6
1.  import java.util.ArrayList;
1.  import java.util.List; 9
1.  public class FastJsonDemo {
1.  public static void main(String[] args) {
1.  //创建一个对象
1.  User user1 = new User("秦疆 1 号", 3, "男");
1.  User user2 = new User("秦疆 2 号", 3, "男");
1.  User user3 = new User("秦疆 3 号", 3, "男");
1.  User user4 = new User("秦疆 4 号", 3, "男");
1.  List<User> list = new ArrayList<User>();
1.  list.add(user1);
1.  list.add(user2);
1.  list.add(user3);
1.  list.add(user4); 22
1.  System.out.println("**\*\*\***Java 对象 转 JSON 字符串**\*\*\***");
1.  String str1 = JSON.toJSONString(list);
1.  System.out.println("JSON.toJSONString(list)==>"+str1);
1.  String str2 = JSON.toJSONString(user1);
1.  System.out.println("JSON.toJSONString(user1)==>"+str2); 28
1.  System.out.println("\n**\*\*** JSON 字符串 转 Java 对象**\*\*\***");
1.  User jp_user1=JSON.parseObject(str2,User.class);
1.  System.out.println("JSON.parseObject(str2,User.class)==>"+jp_user1); 32
1.  System.out.println("\n**\*\*** Java 对象 转 JSON 对象 **\*\***");
1.  JSONObject jsonObject1 = (JSONObject) JSON.toJSON(user2);
1.      System.out.println("(JSONObject) JSON.toJSON(user2)==>"+jsonObject1.getString("name"));

36

1.  System.out.println("\n**\*\*** JSON 对象 转 Java 对象 **\*\***");
1.  User to_java_user = JSON.toJavaObject(jsonObject1, User.class);
1.      System.out.println("JSON.toJavaObject(jsonObject1, User.class)==>"+to_java_user);

40 }
41 }

这种工具类，我们只需要掌握使用就好了，在使用的时候在根据具体的业务去找对应的实现。和以前的
commons-io 那种工具包一样，拿来用就好了！

**9、Ajax**

1.  **、简介**

**A JAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。**
A JAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。
**Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的 Web 应用程序的技术。**
在 2005 年，Google 通过其 Google Suggest 使 A JAX 变得流行起来。Google Suggest 能够自动帮你完成搜索单词。
Google Suggest 使用 A JAX 创造出动态性极强的 web 界面：当您在谷歌的搜索框输入关键字时，
JavaScript 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表。
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834551673-0efddbf2-fa55-4862-b1b1-a8d299ff2a87.png#)就和国内百度的搜索框一样：

传统的网页(即不用 ajax 技术的网页)，想要更新内容或者提交一个表单，都需要重新加载整个网页。
使用 ajax 技术的网页，通过在后台服务器进行少量的数据交换，就可以实现异步局部更新。
使用 Ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的 Web 用户界面。

1.  **、伪造 Ajax**

我们可以使用前端的一个标签来伪造一个 ajax 的样子。 iframe 标签

1. 新建一个 module ： sspringmvc-06-ajax ， 导入 web 支持！
1. 编写一个 ajax-frame.html 使用 iframe 测试，感受下效果

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

<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title>kuangshen</title>
</head>
<body>
<script type="text/javascript"> window.onload = function(){
var myDate = new Date();

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
document.getElementById('currentTime').innerText = myDate.getTime();
};
function LoadPage(){
var targetUrl = document.getElementById('url').value; console.log(targetUrl); document.getElementById("iframePosition").src = targetUrl;
}

</script>

<div>
<p>请输入要加载的地址：<span id="currentTime"></span></p>
<p>
<input id="url" type="text" value=["https://www.baidu.com/](http://www.baidu.com/)"/>
<input type="button" value="提交" onclick="LoadPage()">
</p>
</div>
34
35
36
37
<div>
<h3>加载页面位置：</h3>
<iframe id="iframePosition" style="width: 100%;height: 500px;">
</iframe>
</div>
</body>
</html>

1. 使用 IDEA 开浏览器测试一下！

**利用 A JAX 可以做：**
注册时，输入用户名自动检测用户是否已经存在。登陆时，提示用户名密码错误
删除数据行时，将行 ID 发送到后台，后台在数据库中删除，数据库删除成功后，在页面 DOM 中将数据行也删除。
....等等

1.  **、jQuery.ajax**

纯 JS 原生实现 Ajax 我们不去讲解这里，直接使用 jquery 提供的，方便学习和使用，避免重复造轮 子，有兴趣的同学可以去了解下 JS 原生 XMLHttpRequest ！
Ajax 的核心是 XMLHttpRequest 对象(XHR)。XHR 为向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。
jQuery 提供多个与 A JAX 有关的方法。
通过 jQuery A JAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、
XML 或 JSON – 同时您能够把这些外部数据直接载入网页的被选元素中。
jQuery 不是生产者，而是大自然搬运工。
jQuery Ajax 本质就是 XMLHttpRequest，对他进行了封装，方便调用！

1 jQuery.ajax(...)
2
3
4
部分参数：
url：请求地址
type：请求方式，GET、POST（1.9.0 之后用 method）

1.  headers：请求头
1.  data：要发送的数据
1.      contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www- form-urlencoded; charset=UTF-8")
1.  async：是否异步
1.  timeout：设置请求超时时间（毫秒）
1.  beforeSend：发送请求前执行的函数(全局)
1.  complete：完成之后执行的回调函数(全局)
1.  success：成功之后执行的回调函数(全局)
1.  error：失败之后执行的回调函数(全局)
1.  accepts：通过请求头发送给服务器，告诉服务器当前客户端课接受的数据类型
1.  dataType：将服务器端返回的数据转换成指定类型
1.  "xml": 将服务器端返回的内容转换成 xml 格式
1.  "text": 将服务器端返回的内容转换成普通文本格式
1.  "html": 将服务器端返回的内容转换成普通文本格式，在插入 DOM 中时，如果包含

JavaScript 标签，则会尝试去执行。

1.      "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
1.  "json": 将服务器端返回的内容转换成相应的 JavaScript 对象
1.  "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?"

jQuery 将自动替换 ? 为正确的函数名，以执行回调函数

**我们来个简单的测试，使用最原始的 HttpServletResponse 处理 , .最简单 , 最通用**

      1. 配置web.xml 和 springmvc的配置文件，复制上面案例的即可 【记得静态资源过滤和注解驱动配置上】



         1. <?xml version="1.0" encoding="UTF-8"?>
         1. <beans xmlns=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)"
         1. xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
         1. xmlns:context=["http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)"
         1. xmlns:mvc=["http://www.springframework.org/schema/mvc](http://www.springframework.org/schema/mvc)"
         1. xsi:schemaLocation=["http://www.springframework.org/schema/beans](http://www.springframework.org/schema/beans)
         1. [http://www.springframework.org/schema/beans/spring-beans.xsd](http://www.springframework.org/schema/beans/spring-beans.xsd)
         1. [http://www.springframework.org/schema/context](http://www.springframework.org/schema/context)
         1. 	[https://www.springframework.org/schema/context/spring-](http://www.springframework.org/schema/context/spring-) context.xsd
         1. [http://www.springframework.org/schema/mvc](http://www.springframework.org/schema/mvc)
         1. [https://www.springframework.org/schema/mvc/spring-mvc.xsd](http://www.springframework.org/schema/mvc/spring-mvc.xsd)"> 12

1. <!-- 自动扫描指定的包，下面所有注解类交给IOC容器管理 -->
1. <context:component-scan base-package="com.kuang.controller"/>
1. <mvc:default-servlet-handler />
1. <mvc:annotation-driven /> 17

18 <!-- 视图解析器 -->

1.      <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver  "
1.  id="internalResourceViewResolver"> 21 <!-- 前缀 -->

22 <property name="prefix" value="/WEB-INF/jsp/" /> 23 <!-- 后缀 -->

1. <property name="suffix" value=".jsp" />
1. </bean> 26

27 </beans>

      1. 编写一个AjaxController

1. @Controller
1. public class AjaxController { 3

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
@RequestMapping("/a1")
public void ajax1(String name , HttpServletResponse response) throws IOException {
if ("admin".equals(name)){ response.getWriter().print("true");
}else{
response.getWriter().print("false");
}
}
}

      1. 导入jquery ， 可以使用在线的CDN ， 也可以下载导入

| 1
2 | <script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>

<script src="${pageContext.request.contextPath}/statics/js/jquery- 3.1.1.min.js"></script> |

| --- | --- |

      1. 编写index.jsp测试

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
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
<title>$Title$</title>
<%--<script src="https://code.jquery.com/jquery-3.1.1.min.js">
</script>--%>
<script src="${pageContext.request.contextPath}/statics/js/jquery- 3.1.1.min.js"></script>
<script>
function a1(){
$.post({
url:"${pageContext.request.contextPath}/a1", data:{'name':$("#txtName").val()}, success:function (data,status) {
alert(data); alert(status);
}
});
}
</script>
</head>
<body>
<%--onblur：失去焦点触发事件--%>
用户名:<input type="text" id="txtName" onblur="a1()"/>

</body>
</html>

      1. 启动tomcat测试！ 打开浏览器的控制台，当我们鼠标离开输入框的时候，可以看到发出了一个

ajax 的请求！是后台返回给我们的结果！测试成功！
**Springmvc 实现**
实体类 user

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
@Data @AllArgsConstructor @NoArgsConstructor
public class User {
private String name; private int age;
private String sex;
}

我们来获取一个集合对象，展示到前端页面

1. @RequestMapping("/a2")
1. public List<User> ajax2(){
1. List<User> list = new ArrayList<User>();
1. list.add(new User("秦疆 1 号",3,"男"));
1. list.add(new User("秦疆 2 号",3,"男"));
1. list.add(new User("秦疆 3 号",3,"男"));
1. return list; //由于@RestController 注解，将 list 转成 json 格式返回 8 }

前端页面

1. <%@ page contentType="text/html;charset=UTF-8" language="java" %>
1. <html>
1. <head>
1. <title>Title</title>
1. </head>
1. <body>
1. <input type="button" id="btn" value="获取数据"/>
1. <table width="80%" align="center"> 9		<tr>
1. <td>姓名</td>
1. <td>年龄</td>
1. <td>性别</td> 13	</tr>
1. <tbody id="content">
1. </tbody>
1. </table> 17

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

<script src="${pageContext.request.contextPath}/statics/js/jquery- 3.1.1.min.js"></script>
<script>
$(function () {
$("#btn").click(function () {
$.post("${pageContext.request.contextPath}/a2",function (data) { console.log(data)
var html="";
for (var i = 0; i <data.length ; i++) { html+= "<tr>" +



| 28 | 

})
})
</script>
</body>
</html> |

}); | "<td>" + data[i].name + "</td>" + "<td>" + data[i].age + "</td>" + "<td>" + data[i].sex + "</td>" + "</tr>"
}
$("#content").html(html); |
| --- | --- | --- | --- |
| 29 | | | |
| 30 | | | |
| 31 | | | |
| 32 | | | |
| 33 | | | |
| 34 | | | |
| 35 | | | |
| 36 | | | |
| 37 | | | |
| 38 | | | |
| 39 | | | |

**成功实现了数据回显！可以体会一下 Ajax 的好处！**

1.  **、注册提示效果**

我们再测试一个小 Demo，思考一下我们平时注册时候，输入框后面的实时提示怎么做到的；如何优化 我们写一个 Controller

1. @RequestMapping("/a3")
1. public String ajax3(String name,String pwd){
1. String msg = "";
1. //模拟数据库中存在数据
1. if (name!=null){
1. if ("admin".equals(name)){
1. msg = "OK";
1. }else {
1. msg = "用户名输入错误"; 10 }

11 }
12 if (pwd!=null){
13 if ("123456".equals(pwd)){

1. msg = "OK";
1. }else {
1. msg = "密码输入有误"; 17 }

18 }
19 return msg; //由于@RestController 注解，将 msg 转成 json 格式返回 20 }

前端页面 login.jsp

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
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
<title>ajax</title>
<script src="${pageContext.request.contextPath}/statics/js/jquery- 3.1.1.min.js"></script>
<script>
function a1(){
$.post({
url:"${pageContext.request.contextPath}/a3", data:{'name':$("#name").val()},

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
success:function (data) {
if (data.toString()=='OK'){
$("#userInfo").css("color","green");
}else {
$("#userInfo").css("color","red");
}
$("#userInfo").html(data);
}
});
}
function a2(){
$.post({
url:"${pageContext.request.contextPath}/a3", data:{'pwd':$("#pwd").val()}, success:function (data) {
if (data.toString()=='OK'){
$("#pwdInfo").css("color","green");
}else {
$("#pwdInfo").css("color","red");
}
$("#pwdInfo").html(data);
}
});
}

</script>
</head>
<body>
<p>
用户名:<input type="text" id="name" onblur="a1()"/>
<span id="userInfo"></span>
</p>
<p>
密码:<input type="text" id="pwd" onblur="a2()"/>
<span id="pwdInfo"></span>
</p>
</body>
</html>

【记得处理 json 乱码问题】
测试一下效果，动态请求响应，局部刷新，就是如此！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834552196-64355fc1-a447-4e92-a7c5-f91b91373e9e.png#)

1. **、获取 baidu 接口 Demo**

1. <!DOCTYPE HTML>
1. <html>
1. <head>
1. <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
1. <title>JSONP百度搜索</title>
1. <style>

| 7   | #q{      |                         |
| --- | -------- | ----------------------- |
| 8   |          | width: 500px;           |
| 9   |          | height: 30px;           |
| 10  |          | border:1px solid #ddd;  |
| 11  |          | line-height: 30px;      |
| 12  |          | display: block;         |
| 13  |          | margin: 0 auto;         |
| 14  |          | padding: 0 10px;        |
| 15  |          | font-size: 14px;        |
| 16  | }        |                         |
| 17  | #ul{     |                         |
| 18  |          | width: 520px;           |
| 19  |          | list-style: none;       |
| 20  |          | margin: 0 auto;         |
| 21  |          | padding: 0;             |
| 22  |          | border:1px solid #ddd;  |
| 23  |          | margin-top: -1px;       |
| 24  |          | display: none;          |
| 25  | }        |                         |
| 26  | #ul      | li{                     |
| 27  |          | line-height: 30px;      |
| 28  |          | padding: 0 10px;        |
| 29  | }        |                         |
| 30  | #ul      | li:hover{               |
| 31  |          | background-color: #f60; |
| 32  |          | color: #fff;            |
| 33  | }        |                         |
| 34  | </style> |                         |
| 35  | <script> |                         |
| 36  |          |                         |

| 37
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
59
60
61
62
63
64 | // 2.步骤二
// 定义 demo 函数 (分析接口、数据) function demo(data){
var Ul = document.getElementById('ul'); var html = '';
// 如果搜索数据存在 把内容添加进去
if (data.s.length) {
// 隐藏掉的 ul 显示出来
Ul.style.display = 'block';
// 搜索到的数据循环追加到 li 里
for(var i = 0;i<data.s.length;i++){ html += '<li>'+data.s[i]+'</li>';
}
// 循环的 li 写入 ul Ul.innerHTML = html;
}
}

// 1.步骤一
window.onload = function(){
// 获取输入框和 ul
var Q = document.getElementById('q'); var Ul = document.getElementById('ul');

// 事件鼠标抬起时候
Q.onkeyup = function(){
// 如果输入框不等于空
if (this.value != '') { | |

65

66
67
68
69
70

71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
// ☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆JSONPz 重点
☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
// 创建标签
var script = document.createElement('script');
//给定要跨域的地址 赋值给 src
//这里是要请求的跨域的地址 我写的是百度搜索的跨域地址
script.src = 'https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su? wd='+this.value+'&cb=demo';
// 将组合好的带 src 的 script 标签追加到 body 里
document.body.appendChild(script);
}
}
}
</script>

</head>
<body>
<input type="text" id="q" />
<ul id="ul">

</ul>
</body>
</html>

**10、拦截器**

1.  **、概述**

SpringMVC 的处理器拦截器类似于 Servlet 开发中的过滤器 Filter,用于对处理器进行预处理和后处理。开 发者可以自己定义一些拦截器来实现特定的功能。
**过滤器与拦截器的区别：**拦截器是 AOP 思想的具体应用。**过滤器**
servlet 规范中的一部分，任何 java web 工程都可以使用
在 url-pattern 中配置了/\*之后，可以对所有要访问的资源进行拦截
**拦截器**
拦截器是 SpringMVC 框架自己的，只有使用了 SpringMVC 框架的工程才能使用
拦截器只会拦截访问的控制器方法， 如果访问的是 jsp/html/css/image/js 是不会进行拦截的

1.  **、自定义拦截器**

那如何实现拦截器呢？
想要自定义拦截器，必须实现 HandlerInterceptor 接口。

      1. 新建一个Moudule ， springmvc-07-Interceptor ， 添加web支持
      1. 配置web.xml 和 springmvc-servlet.xml 文件
      1. 编写一个拦截器

1 package com.kuang.interceptor; 2

1. import org.springframework.web.servlet.HandlerInterceptor;
1. import org.springframework.web.servlet.ModelAndView; 5
1. import javax.servlet.http.HttpServletRequest;
1. import javax.servlet.http.HttpServletResponse; 8

9 public class MyInterceptor implements HandlerInterceptor { 10

         1. //在请求处理的方法之前执行
         1. //如果返回true执行下一个拦截器
         1. //如果返回false就不执行下一个拦截器
         1. 	public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {

15 System.out.println("------------处理前 ");
16 return true; 17 }
18

1.  //在请求处理方法执行之后执行
1.      public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {

21 System.out.println("------------处理后 ");
22 }
23

1.  //在 dispatcherServlet 处理后执行,做清理工作.
1.      public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {

26 System.out.println("------------清理 ");
27 }
28 }

      1. 在springmvc的配置文件中配置拦截器

1. <!--关于拦截器的配置-->
1. <mvc:interceptors>
1. <mvc:interceptor>

4 <!--/** 包括路径及其子路径-->

1. <!--/admin/* 拦截的是/admin/add等等这种 , /admin/add/user不会被拦截-

->

1. <!--/admin/** 拦截的是/admin/下的所有-->
1. <mvc:mapping path="/\*\*"/>
1. <!--bean配置的就是拦截器-->
1. <bean class="com.kuang.interceptor.MyInterceptor"/>
1. </mvc:interceptor>
1. </mvc:interceptors>

   1. 编写一个 Controller，接收请求

| 1   | package com.kuang.controller;                                  |
| --- | -------------------------------------------------------------- |
| 2   |                                                                |
| 3   | import org.springframework.stereotype.Controller;              |
| 4   | import org.springframework.web.bind.annotation.RequestMapping; |
| 5   | import org.springframework.web.bind.annotation.ResponseBody;   |
| 6   |                                                                |
| 7   | //测试拦截器的控制器                                           |

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
@Controller
public class InterceptorController {
@RequestMapping("/interceptor") @ResponseBody
public String testFunction() { System.out.println("控制器中的方法执行了"); return "hello";
}
}

      1. 前端 index.jsp

| 1   | <a href="${pageContext.request.contextPath}/interceptor">拦截器测试</a> |
| --- | ----------------------------------------------------------------------- |

      1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834552728-3205049f-9021-4d0a-90bf-f9b69351ac83.png#)启动tomcat 测试一下！

1.  **、验证用户是否登录 (认证用户)**

**实现思路**

      1. 有一个登陆页面，需要写一个controller访问页面。
      1. 登陆页面有一提交表单的动作。需要在controller中处理。判断用户名密码是否正确。如果正确， 向session中写入用户信息。返回登陆成功。
      1. 拦截用户请求，判断用户是否登陆。如果用户已经登陆。放行， 如果用户未登陆，跳转到登陆页面

1. 编写一个登陆页面 login.jsp

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
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
<title>Title</title>
</head>
<h1>登录页面</h1>
<hr>

<body>
<form action="${pageContext.request.contextPath}/user/login">
用户名：<input type="text" name="username"> <br>
密码： <input type="password" name="pwd"> <br>
<input type="submit" value="提交">
</form>
</body>
</html>

1. 编写一个 Controller 处理请求

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
package com.kuang.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpSession;

@Controller @RequestMapping("/user") public class UserController {

//跳转到登陆页面@RequestMapping("/jumplogin")
public String jumpLogin() throws Exception { return "login";
}

//跳转到成功页面@RequestMapping("/jumpSuccess")
public String jumpSuccess() throws Exception { return "success";
}
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
//登陆提交
@RequestMapping("/login")
public String login(HttpSession session, String username, String pwd) throws Exception {
// 向 session 记录用户身份信息
System.out.println("接收前端==="+username); session.setAttribute("user", username); return "success";
}

//退出登陆@RequestMapping("logout")
public String logout(HttpSession session) throws Exception {
// session 过期 session.invalidate(); return "login";
}
}

1. 编写一个登陆成功的页面 success.jsp

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
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<html>
<head>
<title>Title</title>
</head>
<body>
<h1>登录成功页面</h1>
<hr>

${user}
<a href="${pageContext.request.contextPath}/user/logout">注销</a>

| 13  | </body> |
| --- | ------- |
| 14  | </html> |

1. 在 index 页面上测试跳转！启动 Tomcat 测试，未登录也可以进入主页！

1. <%@ page contentType="text/html;charset=UTF-8" language="java" %>
1. <html>
1. <head>
1. <title>$Title$</title>
1. </head>
1. <body>

7 <h1>首页</h1>
8 <hr>
9 <%--登录--%>

1. <a href="${pageContext.request.contextPath}/user/jumplogin">登录</a>
1. <a href="${pageContext.request.contextPath}/user/jumpSuccess">成功页面

</a>

1. </body>
1. </html>

1. 编写用户登录拦截器

1 package com.kuang.interceptor; 2

1. import org.springframework.web.servlet.HandlerInterceptor;
1. import org.springframework.web.servlet.ModelAndView; 5
1. import javax.servlet.ServletException;
1. import javax.servlet.http.HttpServletRequest;
1. import javax.servlet.http.HttpServletResponse;
1. import javax.servlet.http.HttpSession;
1. import java.io.IOException; 11

12 public class LoginInterceptor implements HandlerInterceptor { 13

1.      public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws ServletException, IOException {
1.  // 如果是登陆页面则放行
1.  System.out.println("uri: " + request.getRequestURI());
1.  if (request.getRequestURI().contains("login")) {
1.  return true;

19 }
20
21 HttpSession session = request.getSession(); 22

1. // 如果用户已登陆也放行
1. if(session.getAttribute("user") != null) {
1. return true;

26 }
27

1.  // 用户没有登陆跳转到登陆页面
1.      request.getRequestDispatcher("/WEB- INF/jsp/login.jsp").forward(request, response);
1.  return false; 31 }

32

33 public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView
modelAndView) throws Exception {
34
35
36
37
}
public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {
38
39
40
}
}

1. 在 Springmvc 的配置文件中注册拦截器

| 1
2
3
4
5

6
7 | <!--关于拦截器的配置-->
<mvc:interceptors>
<mvc:interceptor>
<mvc:mapping path="/\*\*"/>
<bean id="loginInterceptor" class="com.kuang.interceptor.LoginInterceptor"/>
</mvc:interceptor>
</mvc:interceptors> |
| --- | --- |

1. 再次重启 Tomcat 测试！

**OK，测试登录拦截功能无误.**

**11、文件上传和下载**

1.  **、准备工作**

文件上传是项目开发中最常见的功能之一 ,springMVC 可以很好的支持文件上传，但是 SpringMVC 上下文中默认没有装配 MultipartResolver，因此默认情况下其不能处理文件上传工作。如果想使用 Spring 的文件上传功能，则需要在上下文中配置 MultipartResolver。
前端表单要求：为了能上传文件，必须将表单的 method 设置为 POST，并将 enctype 设置为 multipart/form-data。只有在这样的情况下，浏览器才会把用户选择的文件以二进制数据发送给服务器；
**对表单中的 enctype 属性做个详细的说明：**
application/x-www=form-urlencoded：默认方式，只处理表单域中的 value 属性值，采用这种编码方式的表单会将表单域中的值处理成 URL 编码方式。
multipart/form-data：这种编码方式会以二进制流的方式来处理表单数据，这种编码方式会把文件域指定文件的内容也封装到请求参数中，不会对字符编码。
text/plain：除了把空格转换为 "+" 号外，其他字符都不做编码处理，这种方式适用直接通过表单发送邮件。

1. <form action="" enctype="multipart/form-data" method="post">
1. <input type="file" name="file"/>
1. <input type="submit">
1. </form>

一旦设置了 enctype 为 multipart/form-data，浏览器即会采用二进制流的方式来处理表单数据，而对 于文件上传的处理则涉及在服务器端解析原始的 HTTP 响应。在 2003 年，Apache Software Foundation 发布了开源的 Commons FileUpload 组件，其很快成为 Servlet/JSP 程序员上传文件的最佳选择。
Servlet3.0 规范已经提供方法来处理文件上传，但这种上传需要在 Servlet 中完成。 而 Spring MVC 则提供了更简单的封装。
Spring MVC 为文件上传提供了直接的支持，这种支持是用即插即用的 MultipartResolver 实现的。Spring MVC 使用 Apache Commons FileUpload 技术实现了一个 MultipartResolver 实现类：
CommonsMultipartResolver。因此，SpringMVC 的文件上传还需要依赖 Apache Commons
FileUpload 的组件。

1.  **、文件上传**
    1. 导入文件上传的 jar 包，commons-ﬁleupload ， Maven 会自动帮我们导入他的依赖包 commons-

io 包；

1 <!--文件上传-->

1. <dependency>
1. <groupId>commons-fileupload</groupId>
1. <artifactId>commons-fileupload</artifactId>
1. <version>1.3.3</version>
1. </dependency>
1. <!--servlet-api导入高版本的-->
1. <dependency>
1. <groupId>javax.servlet</groupId>
1. <artifactId>javax.servlet-api</artifactId>
1. <version>4.0.1</version>
1. </dependency>

   1. 配置 bean：multipartResolver

【**注意！！！这个 bena 的 id 必须为：multipartResolver ， 否则上传文件会报 400 的错误！在这里栽过坑,教训！**】

| 1
2

3

4
5
6
7
8 | <!--文件上传配置-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolve
r">

<!-- 请求的编码格式，必须和jSP的pageEncoding属性一致，以便正确读取表单的内容， 默认为ISO-8859-1 -->
<property name="defaultEncoding" value="utf-8"/>
<!-- 上传文件大小上限，单位为字节（10485760=10M） -->
<property name="maxUploadSize" value="10485760"/>
<property name="maxInMemorySize" value="40960"/>
</bean> |
| --- | --- |

CommonsMultipartFile 的 常用方法：
**String getOriginalFilename()：获取上传文件的原名 InputStream getInputStream()：获取文件流**
**void transferTo(File dest)：将上传文件保存到一个目录文件中**
我们去实际测试一下

      1. 编写前端页面

| 1
2
3
4 | <form action="/upload" enctype="multipart/form-data" method="post">
<input type="file" name="file"/>
<input type="submit" value="upload">

</form> |
| --- | --- |

      1. **Controller**

1 package com.kuang.controller; 2

1. import org.springframework.stereotype.Controller;
1. import org.springframework.web.bind.annotation.RequestMapping;
1. import org.springframework.web.bind.annotation.RequestParam;
1. import org.springframework.web.multipart.commons.CommonsMultipartFile; 7
1. import javax.servlet.http.HttpServletRequest;
1. import java.io.\*; 10
1. @Controller
1. public class FileController {
1. //@RequestParam("file") 将 name=file 控件得到的文件封装成

CommonsMultipartFile 对象

1.  //批量上传 CommonsMultipartFile 则为数组即可
1.  @RequestMapping("/upload")
1.      public String fileUpload(@RequestParam("file") CommonsMultipartFile file , HttpServletRequest request) throws IOException {

17

1. //获取文件名 : file.getOriginalFilename();
1. String uploadFileName = file.getOriginalFilename(); 20
1. //如果文件名为空，直接回到首页！
1. if ("".equals(uploadFileName)){
1. return "redirect:/index.jsp"; 24 }

25 System.out.println("上传文件名 : "+uploadFileName);
26

1.  //上传路径保存设置
1.      String path = request.getServletContext().getRealPath("/upload");
1.  //如果路径不存在，创建一个
1.  File realPath = new File(path);
1.  if (!realPath.exists()){
1.  realPath.mkdir(); 33 }

34 System.out.println("上传文件保存地址："+realPath);
35

1.  InputStream is = file.getInputStream(); //文件输入流
1.      OutputStream os = new FileOutputStream(new File(realPath,uploadFileName)); //文件输出流

38

1. //读取写出
1. int len=0;
1. byte[] buffer = new byte[1024];
1. while ((len=is.read(buffer))!=-1){
1. os.write(buffer,0,len);
1. os.flush();

45 }
46 os.close();

| 47 |

} |

} | is.close();
return "redirect:/index.jsp"; |
| --- | --- | --- | --- |
| 48 | | | |
| 49 | | | |
| 50 | | | |

      1. 测试上传文件，OK！

**采用 ﬁle.Transto 来保存上传的文件**

1. 编写 Controller

1 /_
2 _ 采用 file.Transto 来保存上传的文件
3 \*/

1. @RequestMapping("/upload2")
1. public String fileUpload2(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {

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
//上传路径保存设置
String path = request.getServletContext().getRealPath("/upload"); File realPath = new File(path);
if (!realPath.exists()){ realPath.mkdir();
}
// 上 传 文 件 地 址 System.out.println("上传文件保存地址："+realPath);
//通过 CommonsMultipartFile 的方法直接写文件（注意这个时候）
file.transferTo(new File(realPath +"/"+ file.getOriginalFilename()));
18
19
20
return "redirect:/index.jsp";
}

1. 前端表单提交地址修改
1. 访问提交测试，OK！

   1. **、文件下载**

文件下载步骤：

      1. 设置 response 响应头
      1. 读取文件 -- InputStream
      1. 写出文件 -- OutputStream
      1. 执行操作
      1. 关闭流 （先开后关）

代码实现：

1. @RequestMapping(value="/download")
1. public String downloads(HttpServletResponse response ,HttpServletRequest request) throws Exception{
1. //要下载的图片地址
1. String path = request.getServletContext().getRealPath("/upload");
1. String fileName = "基础语法.jpg";

| 6 |

} |
//1、设置 response 响应头
response.reset(); // 设 置 页 面 不 缓 存 , 清 空 buffer response.setCharacterEncoding("UTF-8"); //字符编码 response.setContentType("multipart/form-data"); //二进制传输数据
//设置响应头
response.setHeader("Content-Disposition", "attachment;fileName="+URLEncoder.encode(fileName, "UTF-8"));

File file = new File(path,fileName);
//2、 读取文件--输入流
InputStream input=new FileInputStream(file);
//3、 写出文件--输出流
OutputStream out = response.getOutputStream();

byte[] buff =new byte[1024]; int index=0;
//4、执行 写出操作
while((index= input.read(buff))!= -1){
out.write(buff, 0, index); out.flush();
}
out.close(); input.close(); return null; |
| --- | --- | --- |
| 7 | | |
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
| 29 | | |
| 30 | | |
| 31 | | |

前端

1 <a href="/download">点击下载</a>

测试，文件下载 OK，大家可以和我们之前学习的 JavaWeb 原生的方式对比一下，就可以知道这个便捷多了!
