---
title: 25、SpringBoot入门及原理
urlname: uoyu1r
date: '2021-07-09 20:43:19 +0800'
tags: []
categories: []
---

# SpringBoot 简介

回顾：什么是 Spring
Spring 是一个开源框架，2003 年兴起的一个轻量级的 Java 开发框架，作者：Rod Johnson 。

### Spring 是为了解决企业级应用开发的复杂性而创建的，简化开发。

Spring 是如何简化 Java 开发的
为了降低 Java 开发的复杂性，Spring 采用了以下 4 种关键策略：
1、基于 POJO 的轻量级和最小侵入性编程，所有东西都是 bean；
2、通过 IOC，依赖注入（DI）和面向接口实现松耦合；
3、基于切面（AOP）和惯例进行声明式编程；
4、通过切面和模版减少样式代码，RedisTemplate，xxxTemplate；

什么是 SpringBoot
学过 javaweb 的同学就知道，开发一个 web 应用，从最初开始接触 Servlet 结合 Tomcat, 跑出一个 Hello
Wolrld 程序，是要经历特别多的步骤； 后来就用了框架 Struts，再后来是 SpringMVC，到了现在的
SpringBoot，过一两年又会有其他 web 框架出现；你们有经历过框架不断的演进，然后自己开发项目所有的技术也再不断的变化、改造吗？建议都可以去经历一遍；
言归正传，什么是 SpringBoot 呢，就是一个 javaweb 的开发框架，和 SpringMVC 类似，对比其他
javaweb 框架的好处，官方说是简化开发，约定大于配置， you can "just run"，能迅速的开发 web 应用，几行代码开发一个 http 接口。
所有的技术框架的发展似乎都遵循了一条主线规律：从一个复杂应用场景 衍生 一种规范框架，人们只需要进行各种配置而不需要自己去实现它，这时候强大的配置功能成了优点；发展到一定程度之后，人们 根据实际生产应用情况，选取其中实用功能和设计精华，重构出一些轻量级的框架；之后为了提高开发 效率，嫌弃原先的各类配置过于麻烦，于是开始提倡“约定大于配置”，进而衍生出一些一站式的解决方 案。
是的这就是 Java 企业级应用->J2EE->spring->springboot 的过程。
随着 Spring 不断的发展，涉及的领域越来越多，项目整合开发需要配合各种各样的文件，慢慢变得不那么易用简单，违背了最初的理念，甚至人称配置地狱。Spring Boot 正是在这样的一个背景下被抽象出来的开发框架，目的为了让大家更容易的使用 Spring 、更容易的集成各种常用的中间件、开源软件；
Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring
Boot 应用中这些第三方库几乎可以零配置的开箱即用。

简单来说就是 SpringBoot 其实不是什么新的框架，它默认配置了很多框架的使用方式，就像 maven 整合 了所有的 jar 包，spring boot 整合了所有的框架 。
Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的发展，生态足够完善，
Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

### Spring Boot 的主要优点：

为所有 Spring 开发者更快的入门
**开箱即用**，提供各种默认配置来简化项目配置内嵌式容器简化 Web 项目
没有冗余代码生成和 XML 配置的要求
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834601944-c375ac77-d32d-4dbf-bf58-7c5b8e6ba76b.jpeg#)使用 Spring Boot 到底有多爽，用下面这幅图来表达

# HelloWorld

准备工作

我们将学习如何快速的创建一个 Spring Boot 应用，并且实现一个简单的 Http 请求处理。通过这个例子对 Spring Boot 有一个初步的了解，并体验其结构简单、开发快速的特性。
我的环境准备：
java version "1.8.0_181" Maven-3.6.1 SpringBoot 2.x 最新版
开发工具：
IDEA

创建基础项目说明
Spring 官方提供了非常方便的工具让我们快速构建应用 , Spring Initializr： [https://start.spring.io/](https://start.spring.io/)
**项目创建方式一：**使用 Spring Initializr 的 Web 页面创建项目
1、打开 [https://start.spring.io/](https://start.spring.io/) 2、填写项目信息
3、点击”Generate Project“按钮生成项目；下载此项目
4、解压项目包，并用 IDEA 以 Maven 项目导入，一路下一步即可，直到项目导入完毕。

5、如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。

**项目创建方式二：**使用 IDEA 直接创建项目
1、创建一个新项目
2、选择 spring initalizr ， 可以看到默认就是去官网的快速构建工具那里实现
3、填写项目信息
4、选择初始化的组件（初学勾选 Web 即可）
5、填写项目路径
6、等待项目构建成功

### 项目结构分析：

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。
1、程序的主启动类
2、一个 application.properties 配置文件
3、一个 测试类
4、一个 pom.xml
pom.xml

pom.xml 分析
打开 ，看看 Spring Boot 项目的依赖：

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

<!-- 父依赖 -->
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.2.5.RELEASE</version>
<relativePath/>
</parent>
<dependencies>
<!-- web场景启动器 -->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!-- springboot单元测试 -->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-test</artifactId>
<scope>test</scope>
<!-- 剔除依赖 -->
<exclusions>
<exclusion>
<groupId>org.junit.vintage</groupId>
<artifactId>junit-vintage-engine</artifactId>

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
</exclusion>
</exclusions>
</dependency>
</dependencies>
<build>
<plugins>

<!-- 打包插件 -->
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
</plugins>
</build>

编写 HTTP 接口
1、在主程序的同级目录下，新建一个 controller 包，一定要在同级目录下，否则识别不到
2、在包中新建一个 HelloController 类

1
2
3
4
5
6
7
8
9
@RestController
public class HelloController {
@RequestMapping("/hello") public String hello() {
return "Hello World";
}
}

3、编写完毕后，从主程序启动项目，浏览器发起请求，看页面返回；控制台输出了 Tomcat 访问的端口号！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834602488-59bb7817-7573-4710-bc80-717d2191a8ea.png#)

简单几步，就完成了一个 web 接口的开发，SpringBoot 就是这么简单。所以我们常用它来建立我们的微服务项目！

将项目打成 jar 包，点击 maven 的 package

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834603052-5ddcb897-ac13-4cd2-96e4-8ca55a50b148.jpeg#)

如果遇到以上错误，可以配置打包时 跳过项目运行测试用例

1 <!--

1. 在工作中,很多情况下我们打包是不想执行测试用例的
1. 可能是测试用例不完事,或是测试用例会影响数据库数据
1. 跳过测试用例执

5 -->

1. <plugin>
1. <groupId>org.apache.maven.plugins</groupId>
1. <artifactId>maven-surefire-plugin</artifactId>
1. <configuration>
1. <!--跳过项目运行测试用例-->
1. <skipTests>true</skipTests>
1. </configuration>
1. </plugin>

如果打包成功，则会在 target 目录下生成一个 jar 包
java -jar xxx.jar
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834603584-cd118715-a8a1-41b9-a105-22403a62cf18.jpeg#)
打成了 jar 包后，就可以在任何地方运行了！OK

彩蛋
如何更改启动时显示的字符拼成的字母，SpringBoot 呢？ 也就是 banner 图案； 只需一步：到项目下的 resources 目录下新建一个 banner.txt 即可。
图案可以到：[https://www.bootschool.net/ascii ](https://www.bootschool.net/ascii)这个网站生成，然后拷贝到文件中即可！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834604068-8c10fea8-d922-4ae7-b08c-9cda3a2e4835.png#)

### SpringBoot 这么简单的东西背后一定有故事，我们之后去进行一波源码分析！

**运行原理探究**

我们之前写的 HelloSpringBoot，到底是怎么运行的呢，Maven 项目，我们一般从 pom.xml 文件探究 起；
**Pom.xml**

父依赖
其中它主要是依赖一个父项目，主要是管理项目的资源过滤及插件！

1. <parent>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-starter-parent</artifactId>
1. <version>2.2.5.RELEASE</version>
1. <relativePath/> <!-- lookup parent from repository -->
1. </parent>

点进去，发现还有一个父依赖

1. <parent>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-dependencies</artifactId>
1. <version>2.2.5.RELEASE</version>
1. <relativePath>../../spring-boot-dependencies</relativePath>
1. </parent>

这里才是真正管理 SpringBoot 应用里面所有依赖版本的地方，SpringBoot 的版本控制中心；

### 以后我们导入依赖默认是不需要写版本；但是如果导入的包没有在依赖中管理着就需要手动配置版本 了；

启动器 spring-boot-starter

1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-starter-web</artifactId>
1. </dependency>

**springboot-boot-starter-xxx**：就是 spring-boot 的场景启动器
**spring-boot-starter-web**：帮我们导入了 web 模块正常运行所依赖的组件；
SpringBoot 将所有的功能场景都抽取出来，做成一个个的 starter （启动器），只需要在项目中引入这些 starter 即可，所有相关的依赖都会导入进来 ， 我们要用什么功能就导入什么样的场景启动器即可 ；我们未来也可以自己自定义 starter；

## 主启动类

分析完了 pom.xml 来看看这个启动类

默认的主启动类
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
//@SpringBootApplication 来标注一个主程序类 ， 说明这是一个 Spring Boot 应用
@SpringBootApplication
public class SpringbootApplication {
public static void main(String[] args) {
//以为是启动了一个方法，没想到启动了一个服务 SpringApplication.run(SpringbootApplication.class, args);
}
}

但是**一个简单的启动类并不简单！**我们来分析一下这些注解都干了什么

@SpringBootApplication
作用：标注在某个类上说明这个类是 SpringBoot 的主配置类 ， SpringBoot 就应该运行这个类的 main 方法来启动 SpringBoot 应用；
进入这个注解：可以看到上面还有很多其他注解！

1. @SpringBootConfiguration
1. @EnableAutoConfiguration
1. @ComponentScan(
1. excludeFilters = {@Filter(
1. type = FilterType.CUSTOM,
1. classes = {TypeExcludeFilter.class}
1. ), @Filter(
1. type = FilterType.CUSTOM,
1. classes = {AutoConfigurationExcludeFilter.class} 10 )}

11 )

| 12 | public
//
} | @interface
...... | SpringBootApplication | { |
| --- | --- | --- | --- | --- |
| 13 | | | | |
| 14 | | | | |

@ComponentScan
这个注解在 Spring 中很重要 ,它对应 XML 配置中的元素。
作用：自动扫描并加载符合条件的组件或者 bean ， 将这个 bean 定义加载到 IOC 容器中

@SpringBootConﬁguration
作用：SpringBoot 的配置类 ，标注在某个类上 ， 表示这是一个 SpringBoot 的配置类； 我们继续进去这个注解查看

| 1   | // 点进去得到下面的 @Component               |
| --- | -------------------------------------------- |
| 2   | @Configuration                               |
| 3   | public @interface SpringBootConfiguration {} |
| 4   |                                              |
| 5   | @Component                                   |
| 6   | public @interface Configuration {}           |

这里的 @Conﬁguration，说明这是一个配置类 ，配置类就是对应 Spring 的 xml 配置文件； 里面的 @Component 这就说明，启动类本身也是 Spring 中的一个组件而已，负责启动应用！ 我们回到 SpringBootApplication 注解中继续看。

@EnableAutoConﬁguration

### @EnableAutoConﬁguration ：开启自动配置功能

以前我们需要自己配置的东西，而现在 SpringBoot 可以自动帮我们配置 ； @EnableAutoConﬁguration
告诉 SpringBoot 开启自动配置功能，这样自动配置才能生效；
点进注解接续查看：

### @AutoConﬁgurationPackage ： 自动配置包

1. @Import({Registrar.class})
1. public @interface AutoConfigurationPackage { 3 }

**@import **：Spring 底层注解@import ， 给容器中导入一个组件 Registrar.class 作用：将主启动类的所在包及包下面所有子包里面的所有组件扫描到 Spring 容器 ； 这个分析完了，退到上一步，继续看

### @Import({AutoConﬁgurationImportSelector.class}) ：给容器导入组件 ；

AutoConﬁgurationImportSelector ： 自动配置导入选择器，那么它会导入哪些组件的选择器呢？ 我们点击去这个类看源码：
1、这个类中有一个这样的方法

1.  // 获得候选的配置
1.  protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
1.  //这里的 getSpringFactoriesLoaderFactoryClass（）方法
1.  //返回的就是我们最开始看的启动自动导入配置文件的注解类；EnableAutoConfiguration
1.      List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryCl ass(), this.getBeanClassLoader());
1.      Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
1.  return configurations;

8 }

2、这个方法又调用了 SpringFactoriesLoader 类的静态方法！我们进入 SpringFactoriesLoader 类
loadFactoryNames() 方法

1.  public static List<String> loadFactoryNames(Class<?> factoryClass, @Nullable ClassLoader classLoader) {
1.  String factoryClassName = factoryClass.getName();
1.  //这里它又调用了 loadSpringFactories 方法
1.      return (List)loadSpringFactories(classLoader).getOrDefault(factoryClassName, Collections.emptyList());

5 }

3、我们继续点击查看 loadSpringFactories 方法

1. private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {
1. //获得 classLoader ， 我们返回可以看到这里得到的就是 EnableAutoConfiguration 标注

的类本身

1.      MultiValueMap<String, String> result = (MultiValueMap)cache.get(classLoader);
1.  if (result != null) {
1.  return result;
1.  } else {
1.  try {
1.  //去获取一个资源 "META-INF/spring.factories"
1.      Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");
1.  LinkedMultiValueMap result = new LinkedMultiValueMap(); 11
1.  //将读取到的资源遍历，封装成为一个 Properties
1.  while(urls.hasMoreElements()) {
1.  URL url = (URL)urls.nextElement();
1.  UrlResource resource = new UrlResource(url);
1.      Properties properties = PropertiesLoaderUtils.loadProperties(resource);
1.  Iterator var6 = properties.entrySet().iterator(); 18
1.  while(var6.hasNext()) {
1.  Entry<?, ?> entry = (Entry)var6.next();
1.      String factoryClassName = ((String)entry.getKey()).trim();

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
String[] var9 = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
int var10 = var9.length;
for(int var11 = 0; var11 < var10; ++var11) { String factoryName = var9[var11];
result.add(factoryClassName, factoryName.trim());
}
}
}
36
37
38
cache.put(classLoader, result); return result;
} catch (IOException var13) {
throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var13);
}
}
}

4、发现一个多次出现的文件： ，全局搜索它
spring.factories

spring.factories

### 我们根据源头打开 spring.factories ， 看到了很多自动配置的文件；这就是自动配置根源所在！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834604643-5a3ab499-cc75-4381-a7a6-292974d605a1.jpeg#)
**WebMvcAutoConﬁguration**
我们在上面的自动配置类随便找一个打开看看，比如 ： WebMvcAutoConﬁguration

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834605116-7413e129-eb6c-4b13-a379-7ea298d4c812.png#)

可以看到这些一个个的都是 JavaConﬁg 配置类，而且都注入了一些 Bean，可以找一些自己认识的类，看 着熟悉一下！
所以，自动配置真正实现是从 classpath 中搜寻所有的 META-INF/spring.factories 配置文件 ，并将其中对应的 org.springframework.boot.autoconﬁgure. 包下的配置项，通过反射实例化为对应标注了
@Conﬁguration 的 JavaConﬁg 形式的 IOC 容器配置类 ， 然后将这些都汇总成为一个实例并加载到 IOC 容器中。

### 结论：

1. SpringBoot 在启动的时候从类路径下的 META-INF/spring.factories 中获取

EnableAutoConﬁguration 指定的值

1. 将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作；
1. 整个 J2EE 的整体解决方案和自动配置都在 springboot-autoconﬁgure 的 jar 包中；
1. 它会给容器中导入非常多的自动配置类 （xxxAutoConﬁguration）, 就是给容器中导入这个场景需要的所有组件 ， 并配置好这些组件 ；
1. 有了自动配置类 ， 免去了我们手动编写配置注入功能组件等的工作；

### 现在大家应该大概的了解了下，SpringBoot 的运行原理，后面我们还会深化一次！

**SpringApplication**

不简单的方法
我最初以为就是运行了一个 main 方法，没想到却开启了一个服务；

1. @SpringBootApplication
1. public class SpringbootApplication {
1. public static void main(String[] args) {
1. SpringApplication.run(SpringbootApplication.class, args); 5 }

6 }

### SpringApplication.run 分析

分析该方法主要分两部分，一部分是 SpringApplication 的实例化，二是 run 方法的执行；

SpringApplication

### 这个类主要做了以下四件事情：

1、推断应用的类型是普通的项目还是 Web 项目 2、查找并加载所有可用初始化器 ， 设置到 initializers 属性中 3、找出所有的应用程序监听器，设置到 listeners 属性中 4、推断并设置 main 方法的定义类，找到运行的主类
查看构造器：

1 public SpringApplication(ResourceLoader resourceLoader, Class... primarySources) {
2 // ......

1. this.webApplicationType = WebApplicationType.deduceFromClasspath();
1. this.setInitializers(this.getSpringFactoriesInstances(); 5

this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class
));
6 this.mainApplicationClass = this.deduceMainApplicationClass(); 7 }

run 方法

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834605549-0cd099d8-4a8a-4423-b48c-ac81d78567ae.jpeg#)

# Yaml 语法学习

配置文件
SpringBoot 使用一个全局的配置文件 ， 配置文件名称是固定的
application.properties
语法结构 ： key=value application.yml
语法结构 ：key：空格 value
**配置文件的作用 ：**修改 SpringBoot 自动配置的默认值，因为 SpringBoot 在底层都给我们自动配置好了；
比如我们可以在配置文件中修改 Tomcat 默认启动的端口号！测试一下！

1 server.port=8081

yaml 概述
YAML 是 "YAML Ain't a Markup Language" （YAML 不是一种标记语言）的递归缩写。
在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）
**这种语言以数据做为中心，而不是以标记语言为重点！**
以前的配置文件，大多数都是使用 xml 来配置；比如一个简单的端口配置，我们来对比下 yaml 和 xml 传统 xml 配置：

1. <server>
1. <port>8081<port>
1. </server>

yaml 配置：

1 server：
2 prot: 8080

yml 基础语法
说明：语法要求严格！
1、空格不能省略
2、以缩进来控制层级关系，只要是左边对齐的一列数据都是同一个层级的。
3、属性和值的大小写都是十分敏感的。

### 字面量：普通的值 [ 数字，布尔值，字符串 ]

字面量直接写在后面就可以 ， 字符串默认不用加上双引号或者单引号；

1 k: v

注意：
“ ” 双引号，不会转义字符串里面的特殊字符 ， 特殊字符会作为本身想表示的意思； 比如 ： name: "kuang \n shen" 输出 ： kuang 换行 shen
'' 单引号，会转义特殊字符 ， 特殊字符最终会变成和普通字符一样输出
比如 ： name: ‘kuang \n shen’ 输出 ： kuang \n shen

### 对象、Map（键值对）

1 #对象、Map 格式 2 k:
3 v1:
4 v2:

在下一行来写对象的属性和值得关系，注意缩进；比如：

1. student:
1. name: qinjiang
1. age: 3

行内写法

| 1   | student: | {name: | qinjiang,age: | 3}  |
| --- | -------- | ------ | ------------- | --- |

### 数组（ List、set ）

用 - 值表示数组中的一个元素,比如：

1. pets:
1. - cat
1. - dog
1. - pig

行内写法

1 pets: [cat,dog,pig]

### 修改 SpringBoot 的默认端口号

配置文件中添加，端口号的参数，就可以切换端口；

1 server:
2 port: 8082

# 注入配置文件

yaml 文件更强大的地方在于，他可以给我们的实体类直接注入匹配值！

Yaml 注入配置文件
1、在 springboot 项目中的 resources 目录下新建一个文件 application.yml
2、编写一个实体类 Dog；

1
2
3
4
5
6
7
8
9
package com.kuang.springboot.pojo;
@Component //注册 bean 到容器中
public class Dog { private String name; private Integer age;
//有参无参构造、get、set 方法、toString()方法
}

3、思考，我们原来是如何给 bean 注入属性值的！ @Value，给狗狗类测试一下：

1. @Component //注册 bean
1. public class Dog {
1. @Value("阿黄")
1. private String name;

5 @Value("18")
6 private Integer age; 7 }

4、在 SpringBoot 的测试类下注入狗狗输出一下；

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
@SpringBootTest
class DemoApplicationTests {
@Autowired //将狗狗自动注入进来
Dog dog;

@Test
public void contextLoads() { System.out.println(dog); //打印看下狗狗对象
}
}

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834606136-c208b1ad-0a95-42d3-b45f-d57c03006b71.jpeg#)结果成功输出，@Value 注入成功，这是我们原来的办法对吧。
5、我们在编写一个复杂一点的实体类：Person 类

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
@Component //注册 bean 到容器中
public class Person { private String name; private Integer age; private Boolean happy; private Date birth;
private Map<String,Object> maps; private List<Object> lists;
private Dog dog;
//有参无参构造、get、set 方法、toString()方法
}

6、我们来使用 yaml 配置的方式进行注入，大家写的时候注意区别和优势，我们编写一个 yaml 配置

1. person:
1. name: qinjiang
1. age: 3
1. happy: false

5 birth: 2000/01/01
6 maps: {k1: v1,k2: v2}

1. lists:
1. - code
1. - girl
1. - music
1. dog:
1. name: 旺财
1. age: 1

7、我们刚才已经把 person 这个对象的所有值都写好了，我们现在来注入到我们的类中！

1 /\*

1. @ConfigurationProperties 作用：
1. 将配置文件中配置的每一个属性的值，映射到这个组件中；
1. 告诉 SpringBoot 将本类中的所有属性和配置文件中相关的配置进行绑定
1. 参数 prefix = “person” : 将配置文件中的 person 下面的所有属性一一对应

6 \*/

1. @Component //注册 bean
1. @ConfigurationProperties(prefix = "person")
1. public class Person {
1. private String name;
1. private Integer age;
1. private Boolean happy;
1. private Date birth;
1. private Map<String,Object> maps;
1. private List<Object> lists;
1. private Dog dog; 17 }

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834606504-1f395b4d-4e35-48a2-bfc8-725239272fd4.png#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834606804-c704a2c7-b252-4841-b0ba-c6274e6f2a44.jpeg#)8、IDEA 提示，springboot 配置注解处理器没有找到，让我们看文档，我们可以查看文档，找到一个依赖！

1. <!-- 导入配置文件处理器，配置文件进行绑定就会有提示，需要重启 -->
1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-configuration-processor</artifactId>
1. <optional>true</optional>
1. </dependency>

9、确认以上配置都 OK 之后，我们去测试类中测试一下：

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
@SpringBootTest
class DemoApplicationTests {
@Autowired
Person person; //将 person 自动注入进来

@Test
public void contextLoads() { System.out.println(person); //打印 person 信息
}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834607210-225caebb-d0bf-4de5-b6da-0c31a2014d1b.png#)结果：所有值全部注入成功！

### yaml 配置注入到实体类完全 OK！

课堂测试：
1、将配置文件的 key 值 和 属性的值设置为不一样，则结果输出为 null，注入失败
2、在配置一个 person2，然后将 @ConﬁgurationProperties(preﬁx = "person2") 指向我们的
person2；

加载指定配置文件
**@PropertySource ：**加载指定的配置文件；
**@conﬁgurationProperties**：默认从全局配置文件中获取值；
1、我们去在 resources 目录下新建一个**person.properties**文件

1 name=kuangshen

2、然后在我们的代码中指定加载 person.properties 文件

1
2
3
4
5
6
7
8
9
@PropertySource(value = "classpath:person.properties") @Component //注册 bean
public class Person {
@Value("${name}") private String name;

......
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834607413-158a83dc-d081-4e90-b722-5ad3b9784ce9.png#)3、再次输出测试一下：指定配置文件绑定成功！

配置文件占位符

1. person:
1. name: qinjiang${random.uuid} # 随机 uuid
1. age: ${random.int} # 随机 int
1. happy: false

5 birth: 2000/01/01
6 maps: {k1: v1,k2: v2}

1. lists:
1. - code
1. - girl
1. - music
1. dog:
1. # 引用 person.hello 的值，如果不存在就用 ：后面的值，即 other，然后拼接上\_旺财
1. name: ${person.hello:other}\_旺财
1. age: 1

回顾 properties 配置
我们上面采用的 yaml 方法都是最简单的方式，开发中最常用的；也是 springboot 所推荐的！那我们来唠唠其他的实现方式，道理都是相同得；写还是那样写；配置文件除了 yml 还有我们之前常用的 properties
， 我们没有讲，我们来唠唠！
【注意】properties 配置文件在写中文的时候，会有乱码 ， 我们需要去 IDEA 中设置编码格式为 UTF-8；
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834607636-950cb1d6-ffcf-4815-a0cf-3e36a169e907.png#)settings-->FileEncodings 中配置；

### 测试步骤：

1、新建一个实体类 User

1. @Component //注册 bean
1. public class User {
1. private String name;
1. private int age;
1. private String sex; 6 }

2、编辑配置文件 user.properties

1. user1.name=kuangshen
1. user1.age=18
1. user1.sex=男

3、我们在 User 类上使用@Value 来进行注入！

1. @Component //注册 bean
1. @PropertySource(value = "classpath:user.properties")
1. public class User {
1. //直接使用@value
1. @Value("${user.name}") //从配置文件中取值
1. private String name;
1. @Value("#{9\*2}") // #{SPEL} Spring 表达式
1. private int age;
1. @Value("男") // 字面量
1. private String sex; 11 }

4、Springboot 测试

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
@SpringBootTest
class DemoApplicationTests {
@Autowired User user;

@Test
public void contextLoads() { System.out.println(user);
}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834607901-4d01bff4-8d34-4b67-9da2-72c375aa21d2.png#)结果正常输出：

对比小结
@Value 这个使用起来并不友好！我们需要为每个属性单独注解赋值，比较麻烦；我们来看个功能对比图

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834608244-333edb10-da1b-4617-af23-e5e09e78a57f.jpeg#)

1、@ConﬁgurationProperties 只需要写一次即可 ， @Value 则需要每个字段都添加
2、松散绑定：这个什么意思呢? 比如我的 yml 中写的 last-name，这个和 lastName 是一样的， - 后面跟着的字母默认是大写的。这就是松散绑定。可以测试一下
3、JSR303 数据校验 ， 这个就是我们可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性
4、复杂类型封装，yml 中可以封装对象 ， 使用 value 就不支持

### 结论：

配置 yml 和配置 properties 都可以获取到值 ， 强烈推荐 yml；
如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value； 如果说，我们专门编写了一个 JavaBean 来和配置文件进行一一映射，就直接
@conﬁgurationProperties，不要犹豫！

JSR303 数据校验
Springboot 中可以用@validated 来校验数据，如果数据异常则会统一抛出异常，方便异常中心统一处 理。我们这里来写个注解让我们的 name 只能支持 Email 格式；

1
2
3
4
5
6
7
8
@Component //注册 bean
@ConfigurationProperties(prefix = "person") @Validated //数据校验
public class Person {
@Email(message="邮箱格式错误") //name 必须是邮箱格式
private String name;
}

运行结果 ： default message [不是一个合法的电子邮件地址];
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834608643-36a94cde-04b1-4288-a9f4-81b3dfa29100.png#)
**使用数据校验，可以保证数据的正确性； **下面列出一些常见的使用

| 1   | @NotNull(message="名字不能为空")               |
| --- | ---------------------------------------------- |
| 2   | private String userName;                       |
| 3   | @Max(value=120,message="年龄最大不能查过 120") |
| 4   | private int age;                               |
| 5   | @Email(message="邮箱格式错误")                 |
| 6   | private String email;                          |
| 7   |                                                |
| 8   | 空检查                                         |

| 9   | @Null 验证对象是否为 null                                                                  |
| --- | ------------------------------------------------------------------------------------------ |
| 10  | @NotNull 验证对象是否不为 null, 无法查检长度为 0 的字符串                                  |
| 11  | @NotBlank 检查约束字符串是不是 Null 还有被 Trim 的长度是否大于 0,只对字符串,且会去掉前后空 |
|     | 格.                                                                                        |
| 12  | @NotEmpty 检查约束元素是否为 NULL 或者是 EMPTY.                                            |
| 13  |                                                                                            |
| 14  | Booelan 检查                                                                               |
| 15  | @AssertTrue 验证 Boolean 对象是否为 true                                                   |
| 16  | @AssertFalse 验证 Boolean 对象是否为 false                                                 |
| 17  |                                                                                            |
| 18  | 长度检查                                                                                   |
| 19  | @Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内          |

|
20 |
@Length(min=, max=) string is between min and max included. |
| 21 | |
| 22 | 日期检查 |
| 23 | @Past 验证 Date 和 Calendar 对象是否在当前时间之前 |
| 24 | @Future 验证 Date 和 Calendar 对象是否在当前时间之后 |
| 25 | @Pattern 验证 String 对象是否符合正则表达式的规则 |
| 26 | |
| 27 | .......等等 |
| 28 | 除此以外，我们还可以自定义一些数据校验规则 |

# 多环境切换

proﬁle 是 Spring 对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境；

多配置文件
我们在主配置文件编写的时候，文件名可以是 application-{proﬁle}.properties/yml , 用来指定多个环境版本；
例如：application-test.properties 代表测试环境配置 application-dev.properties 代表开发环境配置但是 Springboot 并不会直接启动这些配置文件，它**默认使用 application.properties 主配置文件**；
我们需要通过一个配置来选择需要激活的环境：

1. #比如在配置文件中指定使用 dev 环境，我们可以通过设置不同的端口号进行测试；
1. #我们启动 SpringBoot，就可以看到已经切换到 dev 下的配置了；
1. spring.profiles.active=dev

yml 的多文档块
和 properties 配置文件中一样，但是使用 yml 去实现不需要创建多个配置文件，更加方便了 !

1 server:
2 port: 8081

1. #选择要激活那个环境块
1. spring:
1. profiles:
1. active: prod

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

---

server: port: 8083
spring:
profiles: dev #配置环境的名称

---

server: port: 8084
spring:
profiles: prod #配置环境的名称

### 注意：如果 yml 和 properties 同时都配置了端口，并且没有激活其他环境 ， 默认会使用 properties 配置文件的！

配置文件加载位置
**外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！**
[官方外部配置文件说明参考文档](https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-external-config-command-line-args)

springboot 启动会扫描以下位置的 application.properties 或者 application.yml 文件作为 Spring boot 的默认配置文件

| 1   | 优先级 1：项目路径下的 config 文件夹配置文件 |
| --- | -------------------------------------------- |
| 2   | 优先级 2：项目路径下配置文件                 |
| 3   | 优先级 3：资源路径下的 config 文件夹配置文件 |
| 4   | 优先级 4：资源路径下配置文件                 |

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

### SpringBoot 会从这四个位置全部加载主配置文件；互补配置；

我们在最低级的配置文件中设置一个项目访问路径的配置来测试互补问题；

| 1   | #配置项目的访问路径                |
| --- | ---------------------------------- |
| 2   | server.servlet.context-path=/kuang |

**扩展**：指定位置加载配置文件
我们还可以通过 spring.conﬁg.location 来改变默认的配置文件位置
项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；这种情 况，一般是后期运维做的多，相同配置，外部指定的配置文件优先级最高

1 java -jar spring-boot-config.jar -- spring.config.location=F:/application.properties

# 自动配置原理

配置文件到底能写什么？怎么写？
[SpringBoot 官方文档](https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#common-application-properties)

分析自动配置原理
我们以**HttpEncodingAutoConﬁguration（Http 编码自动配置）**为例解释自动配置原理；

1.  //表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
1.  @Configuration 3
1.  //启动指定类的 ConfigurationProperties 功能；
1.  //进入这个 HttpProperties 查看，将配置文件中对应的值和 HttpProperties 绑定起来；
1.  //并把 HttpProperties 加入到 ioc 容器中
1.  @EnableConfigurationProperties({HttpProperties.class}) 8

9 //Spring 底层@Conditional 注解

1. //根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
1. //这里的意思就是判断当前应用是否是 web 应用，如果是，当前配置类生效
1. @ConditionalOnWebApplication(
1. type = Type.SERVLET 14 )

15

1. //判断当前项目有没有这个类 CharacterEncodingFilter；SpringMVC 中进行乱码解决的过滤器；
1. @ConditionalOnClass({CharacterEncodingFilter.class}) 18
1. //判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
1. //如果不存在，判断也是成立的
1. //即使我们配置文件中不配置 pring.http.encoding.enabled=true，也是默认生效的；
1. @ConditionalOnProperty(
1. prefix = "spring.http.encoding",
1. value = {"enabled"},
1. matchIfMissing = true 26 )

27

1. public class HttpEncodingAutoConfiguration {
1. //他已经和 SpringBoot 的配置文件映射了
1. private final Encoding properties;
1. //只有一个有参构造器的情况下，参数的值就会从容器中拿
1. public HttpEncodingAutoConfiguration(HttpProperties properties) {
1. this.properties = properties.getEncoding(); 34 }

35

1.  //给容器中添加一个组件，这个组件的某些值需要从 properties 中获取
1.  @Bean
1.  @ConditionalOnMissingBean //判断容器没有这个组件？
1.  public CharacterEncodingFilter characterEncodingFilter() {
1.      CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
1.  filter.setEncoding(this.properties.getCharset().name());

42
filter.setForceRequestEncoding(this.properties.shouldForce(org.springframew ork.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
43
filter.setForceResponseEncoding(this.properties.shouldForce(org.springframe work.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
44 return filter; 45 }
46 //。。。。。。。
47 }

### 一句话总结 ： 根据当前不同的条件判断，决定这个配置类是否生效！

一但这个配置类生效；这个配置类就会给容器中添加各种组件；
这些组件的属性是从对应的 properties 类中获取的，这些类里面的每一个属性又是和配置文件绑定的；
所有在配置文件中能配置的属性都是在 xxxxProperties 类中封装着； 配置文件能配置什么就可以参照某个功能对应的这个属性类

1. //从配置文件中获取指定的值和 bean 的属性进行绑定
1. @ConfigurationProperties(prefix = "spring.http")
1. public class HttpProperties { 4 // .....

5 }

我们去配置文件里面试试前缀，看提示！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834608924-58047ed0-d040-4794-8b15-0e6c60619454.png#)

### 这就是自动装配的原理！

精髓
1、SpringBoot 启动会加载大量的自动配置类
2、我们看我们需要的功能有没有在 SpringBoot 默认写好的自动配置类当中；
3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需 要再手动配置了）
4、给容器中自动配置类添加组件的时候，会从 properties 类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；
**xxxxAutoConﬁgurartion：自动配置类；**给容器中添加组件

### xxxxProperties:封装配置文件中相关属性；

了解：@Conditional
了解完自动装配的原理后，我们来关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**

### @Conditional 派生注解（Spring 注解版原生的@Conditional 作用）

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834609317-cd575d1d-41e3-48c1-80d4-90002f129895.jpeg#)作用：必须是@Conditional 指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

### 那么多的自动配置类，必须在一定的条件下才能生效；也就是说，我们加载了这么多的配置类，但不是 所有的都生效了。

我们怎么知道哪些自动配置类生效？

### 我们可以通过启用 debug=true 属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；

| 1   | #开启 springboot 的调试类 |
| --- | ------------------------- |
| 2   | debug=true                |

**Positive matches:（自动配置类启用的：正匹配）**
**Negative matches:（没有启动，没有匹配成功的自动配置类：负匹配） Unconditional classes: （没有条件的类）**
【演示：查看输出的日志】
掌握吸收理解原理，即可以不变应万变！

# 提高：自定义 starter

我们分析完毕了源码以及自动装配的过程，我们可以尝试自定义一个启动器来玩玩！

说明

启动器模块是一个 空 jar 文件，仅提供辅助性依赖管理，这些依赖可能用于自动装配或者其他类库；

### 命名归约：

官方命名：
前缀： spring-boot-starter-xxx
比如：spring-boot-starter-web....
自定义命名：
xxx-spring-boot-starter
比如：mybatis-spring-boot-starter

编写启动器
1、在 IDEA 中新建一个空项目 spring-boot-starter-diy
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834609678-94ce177a-78aa-4754-aaf1-53b82397435d.png#)2、新建一个普通 Maven 模块：kuang-spring-boot-starter
3、新建一个 Springboot 模块：kuang-spring-boot-starter-autoconﬁgure
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834609953-c8704801-c104-4b25-8921-bd48ce90a07f.png#)
4、点击 apply 即可，基本结构

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834610379-27cec563-e3f1-4b29-ae3e-2198a3cd90d1.jpeg#)

5、在我们的 starter 中 导入 autoconﬁgure 的依赖！

1 <!-- 启动器 -->

1. <dependencies>
1. <!--	引入自动配置模块 -->
1. <dependency>
1. <groupId>com.kuang</groupId>
1. <artifactId>kuang-spring-boot-starter-autoconfigure</artifactId>
1. <version>0.0.1-SNAPSHOT</version>
1. </dependency>
1. </dependencies>

6、将 autoconﬁgure 项目下多余的文件都删掉，Pom 中只留下一个 starter，这是所有的启动器基本配置
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834610801-d2fc39f1-8443-475d-b146-df4550716c1a.jpeg#)
7、我们编写一个自己的服务

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
package com.kuang;
public class HelloService {

HelloProperties helloProperties;

public HelloProperties getHelloProperties() { return helloProperties;
}

public void setHelloProperties(HelloProperties helloProperties) { this.helloProperties = helloProperties;
}

15
16

17
18
19
public String sayHello(String name){
return helloProperties.getPrefix() + name + helloProperties.getSuffix();
}
}

8、编写 HelloProperties 配置类

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
import org.springframework.boot.context.properties.ConfigurationProperties;
// 前缀 kuang.hello
@ConfigurationProperties(prefix = "kuang.hello") public class HelloProperties {
private String prefix; private String suffix;

public String getPrefix() { return prefix;
}

public void setPrefix(String prefix) { this.prefix = prefix;
}

public String getSuffix() { return suffix;
}

public void setSuffix(String suffix) { this.suffix = suffix;
}
}

9、编写我们的自动配置类并注入 bean，测试！

1 package com.kuang; 2
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
import org.springframework.beans.factory.annotation.Autowired; import
org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication
;
import org.springframework.boot.context.properties.EnableConfigurationProperties; import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
@Configuration @ConditionalOnWebApplication //web 应用生效
@EnableConfigurationProperties(HelloProperties.class) public class HelloServiceAutoConfiguration {

@Autowired
HelloProperties helloProperties;

| 17 |

} | @Bean
public HelloService helloService(){ HelloService service = new HelloService(); service.setHelloProperties(helloProperties); return service;
} |
| --- | --- | --- |
| 18 | | |
| 19 | | |
| 20 | | |
| 21 | | |
| 22 | | |
| 23 | | |
| 24 | | |

10、在 resources 编写一个自己的
META-INF\spring.factories

| 1   | # Auto Configure                                                  |
| --- | ----------------------------------------------------------------- |
| 2   | org.springframework.boot.autoconfigure.EnableAutoConfiguration=\  |
| 3   | com.kuang.HelloServiceAutoConfiguration                           |

11、编写完成后，可以安装到 maven 仓库中！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834611499-2863b0c3-dbc4-4770-85aa-322e4b70c627.jpeg#)

新建项目测试我们自己的写的启动器
1、新建一个 SpringBoot 项目
2、导入我们自己写的启动器

1. <dependency>
1. <groupId>com.kuang</groupId>
1. <artifactId>kuang-spring-boot-starter</artifactId>
1. <version>1.0-SNAPSHOT</version>
1. </dependency>

3、编写一个 HelloController 进行测试我们自己的写的接口！

1
2
3
4
5
6
package com.kuang.controller;
@RestController
public class HelloController {

@Autowired

| 7 |

} | HelloService helloService;

@RequestMapping("/hello") public String hello(){
return helloService.sayHello("zxc");
} |
| --- | --- | --- |
| 8 | | |
| 9 | | |
| 10 | | |
| 11 | | |
| 12 | | |
| 13 | | |
| 14 | | |

4、编写配置文件 application.properties

| 1   | kuang.hello.prefix="ppp" |
| --- | ------------------------ |
| 2   | kuang.hello.suffix="sss" |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834611875-cf9fe534-a3a5-4f66-af9b-fa5cf4d86ab9.png#)5、启动项目进行测试，结果成功 !

### 小狂神温馨提示：学完的东西一定要多下去实践！
