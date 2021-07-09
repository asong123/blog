---
title: 19、JavaWeb
urlname: ogltn2
date: '2021-07-09 20:40:51 +0800'
tags: []
categories: []
---

JavaWeb

Java Web

# 1、基本概念

## 、前言

web 开发：
web，网页的意思 ， [www.baidu.com](http://www.baidu.com/)
静态 web
html，css
提供给所有人看的数据始终不会发生变化！ 动态 web
淘宝，几乎是所有的网站；
提供给所有人看的数据始终会发生变化，每个人在不同的时间，不同的地点看到的信息各不相 同！
技术栈：Servlet/JSP，ASP，PHP
在 Java 中，动态 web 资源开发的技术统称为 JavaWeb；

## 、web 应用程序

web 应用程序：可以提供浏览器访问的程序；
a.html、b.html. 多个 web 资源，这些 web 资源可以被外界访问，对外界提供服务；
你们能访问到的任何一个页面或者资源，都存在于这个世界的某一个角落的计算机上。
URL
这个统一的 web 资源会被放在同一个文件夹下，web 应用程序-->Tomcat：服务器 一个 web 应用由多部分组成 （静态 web，动态 web）
html，css，js jsp，servlet Java 程序
jar 包
配置文件 （Properties）
web 应用程序编写完毕后，若想提供给外界访问：需要一个服务器来统一管理；

## 、静态 web

_.htm, _.html,这些都是网页的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取。通络；

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834455500-fb9745eb-5664-4fdb-ba36-2c426631c955.png#)
静态 web 存在的缺点
Web 页面无法动态更新，所有用户看到都是同一个页面轮播图，点击特效：伪动态
JavaScript [实际开发中，它用的最多]
VBScript
它无法和数据库交互（数据无法持久化，用户无法交互）

## 、动态 web

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834455944-ce99b723-0dc1-469c-be89-baf669b50f26.png#)页面会动态展示： “Web 的页面展示的效果因人而异”；
缺点：
加入服务器的动态 web 资源出现了错误，我们需要重新编写我们的**后台程序**,重新发布； 停机维护
优点：
Web 页面可以动态更新，所有用户看到都不是同一个页面
它可以与数据库交互 （数据持久化：注册，商品信息，用户信息 ）

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834456463-5d0d0571-1272-4143-b950-8ccc7acdfafc.png#)

新手村：--魔鬼训练（分析原理，看源码）--> PK 场

# 2、web 服务器

## 、技术讲解

#### ASP:

微软：国内最早流行的就是 ASP；
在 HTML 中嵌入了 VB 的脚本， ASP + COM；
在 ASP 开发中，基本一个页面都有几千行的业务代码，页面极其换乱 维护成本高！
C# IIS
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

<h1>
<h1><h1>
<h1>
<h1>
<h1>
<h1>
<%
System.out.println("hello")
%>
<h1>
<h1>
<h1><h1>
<h1>

#### php：

PHP 开发速度很快，功能很强大，跨平台，代码很简单 （70% , WP）

无法承载大访问量的情况（局限性）

#### JSP/Servlet :

B/S：浏览和服务器 C/S: 客户端和服务器
sun 公司主推的 B/S 架构
基于 Java 语言的 (所有的大公司，或者一些开源的组件，都是用 Java 写的)
可以承载三高问题带来的影响；
语法像 ASP ， ASP-->JSP , 加强市场强度；

.....

## 、web 服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息；

#### IIS

微软的； ASP...,Windows 中自带的

#### Tomcat

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834456990-d58effaa-faed-4871-89f9-02c265b9c0ec.jpeg#)
面向百度编程；
Tomcat 是 Apache 软件基金会（Apache Software Foundation）的 Jakarta 项目中的一个核心项目，最新的 Servlet 和 JSP 规范总是能在 Tomcat 中得到体现，因为 Tomcat 技术先进、性能稳定，而且**免费**，因而深受 Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的 Web 应用服务器。
Tomcat 服务器是一个免费的开放源代码的 Web 应用服务器，属于轻量级应用[服务器](https://baike.baidu.com/item/%E6%9C%8D%E5%8A%A1%E5%99%A8)，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试 JSP 程序的首选。对于一个 Java 初学 web 的人来说，它是最佳的选择
Tomcat 实际上运行 JSP 页面和 Servlet。Tomcat 最新版本为**9.0。**
....

#### 工作 3-5 年之后，可以尝试手写 Tomcat 服务器；

下载 tomcat：

1. 安装 or 解压
1. 了解配置文件及目录结构
1. 这个东西的作用

# 3、Tomcat

## 、 安装 tomcat

tomcat 官网：[http://tomcat.apache.org/](http://tomcat.apache.org/)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834457769-0471fbd5-41a2-4e27-8ddb-cfcd9bffa35d.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834458066-bdf900c4-2584-4b78-b52d-0890cdd74499.png#)

## 、Tomcat 启动和配置

文件夹作用：

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834458572-80063f77-66a2-4c4f-8982-b842f840f45c.png#)

#### 启动。关闭 Tomcat

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834459091-344ffc63-4868-40e4-a308-cbf44cd9c4b5.png#)
访问测试：http://localhost:8080/ 可能遇到的问题：

1. Java 环境变量没有配置
1. 闪退问题：需要配置兼容性
1. 乱码问题：配置文件中设置

## 、配置

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834459639-a3525545-9b64-4045-bc00-991eeea15ba9.jpeg#)

可以配置启动的端口号
tomcat 的默认端口号为：8080 mysql：3306
http：80 https：443

<Connector port="8081" protocol="HTTP/1.1" connectionTimeout="20000"
redirectPort="8443" />
1
2
3

可以配置主机的名称
默认的主机名为：localhost->127.0.0.1 默认网站应用存放的位置为：webapps

| 1   | <Host name="[www.qinjiang.com](http://www.qinjiang.com/)" appBase="webapps" |
| --- | --------------------------------------------------------------------------- |
| 2   | unpackWARs="true" autoDeploy="true">                                        |

### 高难度面试题：

请你谈谈网站是如何进行访问的！

1. 输入一个域名；回车
1. 检查本机的 C:\Windows\System32\drivers\etc\hosts 配置文件下有没有这个域名映射；
   1. 有：直接返回对应的 ip 地址，这个地址中，有我们需要访问的 web 程序，可以直接访问

[www.qinjiang.com](http://www.qinjiang.com/)
127.0.0.1
1

1.  没有：去 DNS 服务器找，找到的话就返回，找不到就返回找不到；

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834460118-ce945fb9-8681-4fbf-ba09-3d81a797a762.png#)

1.  可以配置一下环境变量（可选性）

## 、发布一个 web 网站

不会就先模仿
将自己写的网站，放到服务器(Tomcat)中指定的 web 应用的文件夹（webapps）下，就可以访问了 网站应该有的结构
--webapps ：Tomcat 服务器的 web 目录
-ROOT
-kuangstudy ：网站的目录名

- WEB-INF
  -classes : java 程序
  -lib：web 应用所依赖的 jar 包
  -web.xml ：网站配置文件

- index.html 默认的首页
- static

-css
-style.css
-js
-img
-.....
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

HTTP 协议 ： 面试 Maven：构建工具 Maven 安装包
Servlet 入门
HelloWorld！ Servlet 配置原理

# 4、Http

## 、什么是 HTTP

HTTP（超文本传输协议）是一个简单的请求-响应协议，它通常运行在 TCP 之上。
文本：html，字符串，~ ….
超文本：图片，音乐，视频，定位，地图…….
80
Https：安全的 443

## 、两个时代

http1.0
HTTP/1.0：客户端可以与 web 服务器连接后，只能获得一个 web 资源，断开连接 http2.0
HTTP/1.1：客户端可以与 web 服务器连接后，可以获得多个 web 资源。‘

## 、Http 请求

客户端---发请求（Request）---服务器百度：

| 1   | Request URL:https://[www.baidu.com/](http://www.baidu.com/) 请求地址 |
| --- | -------------------------------------------------------------------- |
| 2   | Request Method:GET get 方法/post 方法                                |
| 3   | Status Code:200 OK 状态码：200                                       |
| 4   | Remote（远程） Address:14.215.177.39:443                             |

| 1   | Accept:text/html                  |      |
| --- | --------------------------------- | ---- |
| 2   | Accept-Encoding:gzip, deflate, br |      |
| 3   | Accept-Language:zh-CN,zh;q=0.9    | 语言 |
| 4   | Cache-Control:max-age=0           |      |
| 5   | Connection:keep-alive             |      |

### 1、请求行

请求行中的请求方式：GET
请求方式：**Get，Post**，HEAD,DELETE,PUT,TRACT…
get：请求能够携带的参数比较少，大小有限制，会在浏览器的 URL 地址栏显示数据内容，不 安全，但高效
post：请求能够携带的参数没有限制，大小没有限制，不会在浏览器的 URL 地址栏显示数据内 容，安全，但不高效。
**2、消息头**

| 1   | Accept：告诉浏览器，它所支持的数据类型             |                  |
| --- | -------------------------------------------------- | ---------------- |
| 2   | Accept-Encoding：支持哪种编码格式 GBK UTF-8        | GB2312 ISO8859-1 |
| 3   | Accept-Language：告诉浏览器，它的语言环境          |                  |
| 4   | Cache-Control：缓存控制                            |                  |
| 5   | Connection：告诉浏览器，请求完成是断开还是保持连接 |                  |
| 6   | HOST：主机 /.                                      |                  |

## 、Http 响应

服务器---响应 客户端
百度：

| 1   | Cache-Control:private  | 缓存控制 |
| --- | ---------------------- | -------- |
| 2   | Connection:Keep-Alive  | 连接     |
| 3   | Content-Encoding:gzip  | 编码     |
| 4   | Content-Type:text/html | 类型     |

### 响应体

| 1   | Accept：告诉浏览器，它所支持的数据类型             |                  |
| --- | -------------------------------------------------- | ---------------- |
| 2   | Accept-Encoding：支持哪种编码格式 GBK UTF-8        | GB2312 ISO8859-1 |
| 3   | Accept-Language：告诉浏览器，它的语言环境          |                  |
| 4   | Cache-Control：缓存控制                            |                  |
| 5   | Connection：告诉浏览器，请求完成是断开还是保持连接 |                  |
| 6   | HOST：主机 /.                                      |                  |
| 7   | Refresh：告诉客户端，多久刷新一次；                |                  |
| 8   | Location：让网页重新定位；                         |                  |

**2、响应状态码**
200：请求响应成功 200
3xx：请求重定向
重定向：你重新到我给你新位置去；
4xx：找不到资源 404
资源不存在；
5xx：服务器代码错误 500 502:网关错误

#### 常见面试题：

当你的浏览器中地址栏输入地址并回车的一瞬间到页面能够展示回来，经历了什么？

# 5、Maven

#### 我为什么要学习这个技术？

1.  在 Javaweb 开发中，需要使用大量的 jar 包，我们手动去导入；
1.  如何能够让一个东西自动帮我导入和配置这个 jar 包。由此，Maven 诞生了！

1.  **Maven 项目架构管理工具**

我们目前用来就是方便导入 jar 包的！
Maven 的核心思想：**约定大于配置**
有约束，不要去违反。
Maven 会规定好你该如何去编写我们的 Java 代码，必须要按照这个规范来；

## 下载安装 Maven

官网;[https://maven.apache.org/](https://maven.apache.org/)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834461081-a073281f-f7e3-48f9-88a9-0bfac5d0c884.jpeg#)
下载完成后，解压即可；
小狂神友情建议：电脑上的所有环境都放在一个文件夹下，方便管理；

## 配置环境变量

在我们的系统环境变量中配置如下配置：
M2_HOME maven 目录下的 bin 目录 MAVEN_HOME maven 的目录
在系统的 path 中配置 %MAVEN_HOME%\bin
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834461775-42698d9f-f1d9-49cb-a3f5-8bece0e5e68c.png#)
测试 Maven 是否安装成功，保证必须配置完毕！

## 阿里云镜像

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834462214-748ee47a-2f08-4b2b-95a5-7adcbebe549c.jpeg#)

镜像：mirrors
作用：加速我们的下载国内建议使用阿里云的镜像

<mirror>
<id>nexus-aliyun</id>
<mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
<name>Nexus aliyun</name>
<url>[http://maven.aliyun.com/nexus/content/groups/public](http://maven.aliyun.com/nexus/content/groups/public)</url>
</mirror>
1
2
3
4
5
6

## 本地仓库

在本地的仓库，远程仓库；
**建立一个本地仓库：**localRepository

<localRepository>D:\Environment\apache-maven-3.6.2\maven-
repo</localRepository>
1

## 、在 IDEA 中使用 Maven

1. 启动 IDEA
1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834462589-ec0f7b1e-32f2-41dd-acb5-19345c1ec8ef.png#)创建一个 MavenWeb 项目

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834463013-54bebba4-e283-4468-8255-b12ba0071807.png#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834463443-56782191-a358-40a8-be9c-70c9f917bc9a.png#)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834463712-f943be83-c5f7-421a-ac59-861a84560d92.png#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834464006-2f658549-46af-4bf2-8c97-c31a48333c7a.png#)

1. 等待项目初始化完毕

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834464632-d07d0789-235c-414a-8ffb-256dad8c96ae.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834465184-86ec4b27-1bfa-4fa4-a12f-d3165f0c08d8.jpeg#)

1. 观察 maven 仓库中多了什么东西？
1. IDEA 中的 Maven 设置

注意：IDEA 项目创建成功后，看一眼 Maven 的配置

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834465543-ef33b0e3-9640-4706-86b1-d2e75901a6d3.png#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834466144-f3b35645-fdeb-4d8f-90d4-0efd6c7ff87d.jpeg#)

1. 到这里，Maven 在 IDEA 中的配置和使用就 OK 了!

## 、创建一个普通的 Maven 项目

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834466481-73dacf1a-489a-490d-ae7d-98c18ed1c3b1.png#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834466828-e7da84cf-17b2-4f16-9f36-c0c5f572e0ed.jpeg#)
这个只有在 Web 应用下才会有！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834467199-5c896ab3-676e-4147-b201-b4803b90e487.png#)

## 标记文件夹功能

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834467743-92c9a787-d6e1-4667-a4c8-56e5d4654a85.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834468027-2f00b220-2d3f-47b5-bffa-17534f274028.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834468478-b286051a-bced-44cc-86b2-49cc34e13da5.png#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834468818-aa5a3fa3-8aa1-4ba1-a9d7-75376c7cd9b6.jpeg#)

1.  **在 IDEA 中配置 Tomcat**

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834469141-f809bafe-90b3-489e-b0fb-aff41439640f.png#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834469467-344fcb1f-7e0f-434a-ab95-1e43428913b2.png#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834469806-cc2e0103-9905-4dc4-8d4d-b527b630394b.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834470189-12d8a632-65f4-49a5-b124-61ece6773d54.png#)
解决警告问题
必须要的配置：**为什么会有这个问题：我们访问一个网站，需要指定一个文件夹名字；**
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834470576-df8ccf6f-4aaf-4aef-8229-71c0901c7009.png#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834470810-cab200c8-4768-4781-b909-61a8d1fd9943.png#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834471104-f79d9639-97b2-43a5-8cd4-6950abfd1b27.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834471553-7ae92b0b-146d-42fe-9012-59aebf641603.jpeg#)

## pom 文件

pom.xml 是 Maven 的核心配置文件

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834471907-839e0eee-612b-4436-ab44-ad590df7f9f9.jpeg#)

| 1   | <?xml version="1.0" encoding="UTF-8"?>                                                             |
| --- | -------------------------------------------------------------------------------------------------- |
| 2   |                                                                                                    |
| 3   | <!--Maven版本和头文件-->                                                                           |
| 4   | <project xmlns="[http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)"            |
|     | xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" |
| 5   | xsi:schemaLocation="[http://maven.apache.org/POM/4.0.0](http://maven.apache.org/POM/4.0.0)         |
|     | [http://maven.apache.org/xsd/maven-4.0.0.xsd](http://maven.apache.org/xsd/maven-4.0.0.xsd)">       |
| 6   | <modelVersion>4.0.0</modelVersion>                                                                 |
| 7   |                                                                                                    |
| 8   | <!--这里就是我们刚才配置的GAV-->                                                                   |
| 9   | <groupId>com.kuang</groupId>                                                                       |
| 10  | <artifactId>javaweb-01-maven</artifactId>                                                          |
| 11  | <version>1.0-SNAPSHOT</version>                                                                    |
| 12  | <!--Package：项目的打包方式                                                                        |
| 13  | jar：java 应用                                                                                     |
| 14  | war：JavaWeb 应用                                                                                  |
| 15  | -->                                                                                                |
| 16  | <packaging>war</packaging>                                                                         |
| 17  |                                                                                                    |
| 18  |                                                                                                    |
| 19  | <!--配置-->                                                                                        |
| 20  | <properties>                                                                                       |
| 21  | <!--项目的默认构建编码-->                                                                          |
| 22  | <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>                                 |
| 23  | <!--编码版本-->                                                                                    |
| 24  | <maven.compiler.source>1.8</maven.compiler.source>                                                 |
| 25  | <maven.compiler.target>1.8</maven.compiler.target>                                                 |
| 26  | </properties>                                                                                      |
| 27  |                                                                                                    |
| 28  | <!--项目依赖-->                                                                                    |
| 29  | <dependencies>                                                                                     |
| 30  | <!--具体依赖的jar包配置文件-->                                                                     |
| 31  | <dependency>                                                                                       |
| 32  | <groupId>junit</groupId>                                                                           |
| 33  | <artifactId>junit</artifactId>                                                                     |
| 34  | <version>4.11</version>                                                                            |
| 35  | </dependency>                                                                                      |
| 36  | </dependencies>                                                                                    |
| 37  |                                                                                                    |
| 38  | <!--项目构建用的东西-->                                                                            |
| 39  | <build>                                                                                            |
| 40  | <finalName>javaweb-01-maven</finalName>                                                            |

<pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
<plugins>
<plugin>
<artifactId>maven-clean-plugin</artifactId>
<version>3.1.0</version>
</plugin>

<!-- see [http://maven.apache.org/ref/current/maven-core/default-](http://maven.apache.org/ref/current/maven-core/default-) bindings.html#Plugin_bindings_for_war_packaging -->
<plugin>
<artifactId>maven-resources-plugin</artifactId>
<version>3.0.2</version>
</plugin>
<plugin>
<artifactId>maven-compiler-plugin</artifactId>
<version>3.8.0</version>
</plugin>
<plugin>
<artifactId>maven-surefire-plugin</artifactId>
<version>2.22.1</version>
</plugin>
<plugin>
<artifactId>maven-war-plugin</artifactId>
<version>3.2.2</version>
</plugin>
<plugin>
<artifactId>maven-install-plugin</artifactId>
<version>2.5.2</version>
</plugin>
<plugin>
<artifactId>maven-deploy-plugin</artifactId>
<version>2.8.2</version>
</plugin>
</plugins>
</pluginManagement>
</build>
</project>
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
64
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
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834472316-9328ff54-db06-4ddb-80ab-30e41f901357.jpeg#)

maven 由于他的约定大于配置，我们之后可以能遇到我们写的配置文件，无法被导出或者生效的问题， 解决方案：

<!--在build中配置resources，来防止我们资源导出失败的问题-->
<build>
<resources>
<resource>
<directory>src/main/resources</directory>
<includes>
<include>**/*.properties</include>
<include>**/*.xml</include>
</includes>
<filtering>true</filtering>
</resource>
<resource>
<directory>src/main/java</directory>
<includes>
<include>**/*.properties</include>
<include>**/*.xml</include>
</includes>
<filtering>true</filtering>
</resource>
</resources>
</build>
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

## IDEA 操作

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834472626-da08759c-7ef1-45a3-ba90-6d9b737cea2e.png#)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834473056-2600c5db-82d9-4d0e-ab9e-d9c46931bb22.png#)

1.  **解决遇到的问题**
    1. Maven 3.6.2

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834473378-86e83fd7-3ba4-4599-8e0a-7a176e8b85cf.png#)解决方法：降级为 3.6.1

      1. Tomcat闪退



      1. IDEA中每次都要重复配置Maven

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834473678-e9a329c7-793b-43e0-848d-b92984d51051.jpeg#)在 IDEA 中的全局默认配置中去配置

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834473999-10d4c718-e10a-4f6b-9f61-e26943c30df2.jpeg#)

      1. Maven项目中Tomcat无法配置
      1. ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834474347-e40af0ae-8db0-4626-bf7b-8090913400e3.jpeg#)maven默认web项目中的web.xml版本问题
      1. 替换为webapp4.0版本和tomcat一致

| 1   | <?xml version="1.0" encoding="UTF-8"?>                                                             |
| --- | -------------------------------------------------------------------------------------------------- |
| 2   | <web-app xmlns="[http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)"          |
| 3   | xmlns:xsi="[http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)" |
| 4   | xsi:schemaLocation="[http://xmlns.jcp.org/xml/ns/javaee](http://xmlns.jcp.org/xml/ns/javaee)       |
| 5   | [http://xmlns.jcp.org/xml/ns/javaee/web-](http://xmlns.jcp.org/xml/ns/javaee/web-)                 |
|     | app_4_0.xsd"                                                                                       |
| 6   | version="4.0"                                                                                      |
| 7   | metadata-complete="true">                                                                          |
| 8   |                                                                                                    |
| 9   |                                                                                                    |
| 10  |                                                                                                    |
| 11  | </web-app>                                                                                         |

      1. Maven仓库的使用

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834474679-75ff201b-c911-47d0-bc72-0965cc00dfde.png#)地址：[https://mvnrepository.com/](https://mvnrepository.com/)

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834475015-ae4d8238-e7dc-49de-8417-7c1d0d78f125.png#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834475460-01e8603d-fe3b-4d2a-a32e-c6887f79308d.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834475955-a70eb146-6b39-4c3a-b366-a6ee73464aba.jpeg#)

# 6、Servlet

## 、Servlet 简介

Servlet 就是 sun 公司开发动态 web 的一门技术
Sun 在这些 API 中提供一个接口叫做：Servlet，如果你想开发一个 Servlet 程序，只需要完成两个小 步骤：
编写一个类，实现 Servlet 接口
把开发好的 Java 类部署到 web 服务器中。
**把实现了 Servlet 接口的 Java 程序叫做，Servlet**

## 、HelloServlet

Serlvet 接口 Sun 公司有两个默认的实现类：HttpServlet，GenericServlet

1. 构建一个普通的 Maven 项目，删掉里面的 src 目录，以后我们的学习就在这个项目里面建立

Moudel；这个空的工程就是 Maven 主工程；

1. 关于 Maven 父子工程的理解： 父项目中会有

<modules>
<module>servlet-01</module>
</modules>
1
2
3

子项目会有

<parent>
<artifactId>javaweb-02-servlet</artifactId>
<groupId>com.kuang</groupId>
<version>1.0-SNAPSHOT</version>
</parent>
1
2
3
4
5

父项目中的 java 子项目可以直接使用

son extends father
1

1. Maven 环境优化
   1. 修改 web.xml 为最新的
   1. 将 maven 的结构搭建完整
2. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834476326-11add920-dd4f-4231-b94e-e2f2bb7bb090.png#)编写一个 Servlet 程序
   1. 编写一个普通类
   1. 实现 Servlet 接口，这里我们直接继承 HttpServlet

| 1   | public class HelloServlet extends HttpServlet {                               |
| --- | ----------------------------------------------------------------------------- |
| 2   |                                                                               |
| 3   | //由于 get 或者 post 只是请求实现的不同的方式，可以相互调用，业务逻辑都一样； |
| 4   | @Override                                                                     |
| 5   | protected void doGet(HttpServletRequest req,                                  |
|     | HttpServletResponse resp) throws ServletException, IOException {              |
| 6   | //ServletOutputStream outputStream =                                          |
|     | resp.getOutputStream();                                                       |
| 7   | PrintWriter writer = resp.getWriter(); //响应流                               |
| 8   | writer.print("Hello,Serlvet");                                                |
| 9   | }                                                                             |
| 10  |                                                                               |
| 11  | @Override                                                                     |
| 12  | protected void doPost(HttpServletRequest req,                                 |
|     | HttpServletResponse resp) throws ServletException, IOException {              |
| 13  | doGet(req, resp);                                                             |
| 14  | }                                                                             |
| 15  | }                                                                             |
| 16  |                                                                               |

1. 编写 Servlet 的映射

为什么需要映射：我们写的是 JAVA 程序，但是要通过浏览器访问，而浏览器需要连接 web 服务器， 所以我们需要再 web 服务中注册我们写的 Servlet，还需给他一个浏览器能够访问的路径；

<!--注册Servlet-->
<servlet>
<servlet-name>hello</servlet-name>
<servlet-class>com.kuang.servlet.HelloServlet</servlet-class>
</servlet>
<!--Servlet的请求路径-->
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello</url-pattern>
</servlet-mapping>
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

1. 配置 Tomcat

注意：配置项目发布的路径就可以了

1. 启动测试，OK！

## 、Servlet 原理

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834476595-df4bf867-498a-4fa3-a331-9af49b34ef42.png#)Servlet 是由 Web 服务器调用，web 服务器在收到浏览器请求之后，会：

## 、Mapping 问题

1. 一个 Servlet 可以指定一个映射路径

<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello</url-pattern>
</servlet-mapping>
1
2
3
4

1. 一个 Servlet 可以指定多个映射路径

<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello2</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello3</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello4</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello5</url-pattern>
</servlet-mapping>
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

1. 一个 Servlet 可以指定通用映射路径

<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello/*</url-pattern>
</servlet-mapping>
1
2
3
4

1. 默认请求路径

<!--默认请求路径-->
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/*</url-pattern>
</servlet-mapping>
1
2
3
4
5

1. 指定一些后缀或者前缀等等….

<!--可以自定义后缀实现请求映射
注意点，*前面不能加项目映射的路径
hello/sajdlkajda.qinjiang
-->
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>*.qinjiang</url-pattern>
</servlet-mapping>
1
2
3
4
5
6
7
8
9

1. 优先级问题

指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求；

<!--404-->
<servlet>
<servlet-name>error</servlet-name>
<servlet-class>com.kuang.servlet.ErrorServlet</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>error</servlet-name>
<url-pattern>/*</url-pattern>
</servlet-mapping>
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

## 、ServletContext

web 容器在启动的时候，它会为每个 web 程序都创建一个对应的 ServletContext 对象，它代表了当前的 web 应用；

### 1、共享数据

我在这个 Servlet 中保存的数据，可以在另外一个 servlet 中拿到；

| 1   | public class HelloServlet extends HttpServlet {                        |
| --- | ---------------------------------------------------------------------- |
| 2   | @Override                                                              |
| 3   | protected void doGet(HttpServletRequest req, HttpServletResponse resp) |
|     | throws ServletException, IOException {                                 |
| 4   |                                                                        |
| 5   | //this.getInitParameter() 初始化参数                                   |
| 6   | //this.getServletConfig() Servlet 配置                                 |
| 7   | //this.getServletContext() Servlet 上下文                              |
| 8   | ServletContext context = this.getServletContext();                     |
| 9   |                                                                        |
| 10  | String username = "秦疆"; //数据                                       |
| 11  | context.setAttribute("username",username); //将一个数据保存在了        |
|     | ServletContext 中，名字为：username 。值 username                      |
| 12  |                                                                        |
| 13  | }                                                                      |
| 14  |                                                                        |
| 15  | }                                                                      |
| 16  |                                                                        |

| 1   | public class GetServlet extends HttpServlet {                          |
| --- | ---------------------------------------------------------------------- |
| 2   | @Override                                                              |
| 3   | protected void doGet(HttpServletRequest req, HttpServletResponse resp) |
|     | throws ServletException, IOException {                                 |
| 4   | ServletContext context = this.getServletContext();                     |
| 5   | String username = (String) context.getAttribute("username");           |
| 6   |                                                                        |
| 7   | resp.setContentType("text/html");                                      |
| 8   | resp.setCharacterEncoding("utf-8");                                    |
| 9   | resp.getWriter().print("名字"+username);                               |
| 10  |                                                                        |
| 11  | }                                                                      |
| 12  |                                                                        |

@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
doGet(req, resp);
}
}
13
14

15
16
17
18

| 1   | <servlet>                                                     |
| --- | ------------------------------------------------------------- |
| 2   | <servlet-name>hello</servlet-name>                            |
| 3   | <servlet-class>com.kuang.servlet.HelloServlet</servlet-class> |
| 4   | </servlet>                                                    |
| 5   | <servlet-mapping>                                             |
| 6   | <servlet-name>hello</servlet-name>                            |
| 7   | <url-pattern>/hello</url-pattern>                             |
| 8   | </servlet-mapping>                                            |
| 9   |                                                               |
| 10  |                                                               |
| 11  | <servlet>                                                     |
| 12  | <servlet-name>getc</servlet-name>                             |
| 13  | <servlet-class>com.kuang.servlet.GetServlet</servlet-class>   |
| 14  | </servlet>                                                    |
| 15  | <servlet-mapping>                                             |
| 16  | <servlet-name>getc</servlet-name>                             |
| 17  | <url-pattern>/getc</url-pattern>                              |
| 18  | </servlet-mapping>                                            |

测试访问结果；

### 2、获取初始化参数

<!--配置一些web应用初始化参数-->
<context-param>
<param-name>url</param-name>
<param-value>jdbc:mysql://localhost:3306/mybatis</param-value>
</context-param>
1
2
3
4
5

1. protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
1. ServletContext context = this.getServletContext();
1. String url = context.getInitParameter("url");
1. resp.getWriter().print(url); 5 }

**3、请求转发**

@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
ServletContext context = this.getServletContext(); System.out.println("进入了 ServletDemo04");
//RequestDispatcher requestDispatcher = context.getRequestDispatcher("/gp"); //转发的请求路径
//requestDispatcher.forward(req,resp); //调用 forward 实现请求转发；
context.getRequestDispatcher("/gp").forward(req,resp);
}
1
2

3
4
5

6
7
8
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834476923-5b233afc-3b8a-426e-91f6-0a706651aa61.png#)

### 4、读取资源文件

Properties
在 java 目录下新建 properties
在 resources 目录下新建 properties
发现：都被打包到了同一个路径下：classes，我们俗称这个路径为 classpath: 思路：需要一个文件流；

| 1   | username=root12312 |
| --- | ------------------ |
| 2   | password=zxczxczxc |

| 1   | public class ServletDemo05 extends HttpServlet {                       |
| --- | ---------------------------------------------------------------------- |
| 2   | @Override                                                              |
| 3   | protected void doGet(HttpServletRequest req, HttpServletResponse resp) |
|     | throws ServletException, IOException {                                 |
| 4   |                                                                        |
| 5   | InputStream is = this.getServletContext().getResourceAsStream("/WEB-   |
|     | INF/classes/com/kuang/servlet/aa.properties");                         |
| 6   |                                                                        |
| 7   | Properties prop = new Properties();                                    |
| 8   | prop.load(is);                                                         |
| 9   | String user = prop.getProperty("username");                            |
| 10  | String pwd = prop.getProperty("password");                             |
| 11  |                                                                        |
| 12  | resp.getWriter().print(user+":"+pwd);                                  |

| 13  |                                                                         |
| --- | ----------------------------------------------------------------------- |
| 14  | }                                                                       |
| 15  |                                                                         |
| 16  | @Override                                                               |
| 17  | protected void doPost(HttpServletRequest req, HttpServletResponse resp) |
|     | throws ServletException, IOException {                                  |
| 18  | doGet(req, resp);                                                       |
| 19  | }                                                                       |
| 20  | }                                                                       |
| 21  |                                                                         |

访问测试即可 ok；

## 、HttpServletResponse

web 服务器接收到客户端的 http 请求，针对这个请求，分别创建一个代表请求的 HttpServletRequest 对 象，代表响应的一个 HttpServletResponse；
如果要获取客户端请求过来的参数：找 HttpServletRequest 如果要给客户端响应一些信息：找 HttpServletResponse

### 1、简单分类

负责向浏览器发送数据的方法

| 1   | ServletOutputStream getOutputStream() throws IOException; |
| --- | --------------------------------------------------------- |
| 2   | PrintWriter getWriter() throws IOException;               |

负责向浏览器发送响应头的方法

| 1   | void | setCharacterEncoding(String var1);     |
| --- | ---- | -------------------------------------- |
| 2   |      |                                        |
| 3   | void | setContentLength(int var1);            |
| 4   |      |                                        |
| 5   | void | setContentLengthLong(long var1);       |
| 6   |      |                                        |
| 7   | void | setContentType(String var1);           |
| 8   |      |                                        |
| 9   | void | setDateHeader(String var1, long var2); |
| 10  |      |                                        |
| 11  | void | addDateHeader(String var1, long var2); |
| 12  |      |                                        |
| 13  | void | setHeader(String var1, String var2);   |
| 14  |      |                                        |
| 15  | void | addHeader(String var1, String var2);   |
| 16  |      |                                        |
| 17  | void | setIntHeader(String var1, int var2);   |
| 18  |      |                                        |
| 19  | void | addIntHeader(String var1, int var2);   |

响应的状态码

| 1   | int | SC_CONTINUE = 100;               |     |      |
| --- | --- | -------------------------------- | --- | ---- |
| 2   | int | SC_SWITCHING_PROTOCOLS = 101;    |     |      |
| 3   | int | SC_OK = 200;                     |     |      |
| 4   | int | SC_CREATED = 201;                |     |      |
| 5   | int | SC_ACCEPTED = 202;               |     |      |
| 6   | int | SC_NON_AUTHORITATIVE_INFORMATION | =   | 203; |

| 7   | int | SC_NO_CONTENT = 204;                      |
| --- | --- | ----------------------------------------- |
| 8   | int | SC_RESET_CONTENT = 205;                   |
| 9   | int | SC_PARTIAL_CONTENT = 206;                 |
| 10  | int | SC_MULTIPLE_CHOICES = 300;                |
| 11  | int | SC_MOVED_PERMANENTLY = 301;               |
| 12  | int | SC_MOVED_TEMPORARILY = 302;               |
| 13  | int | SC_FOUND = 302;                           |
| 14  | int | SC_SEE_OTHER = 303;                       |
| 15  | int | SC_NOT_MODIFIED = 304;                    |
| 16  | int | SC_USE_PROXY = 305;                       |
| 17  | int | SC_TEMPORARY_REDIRECT = 307;              |
| 18  | int | SC_BAD_REQUEST = 400;                     |
| 19  | int | SC_UNAUTHORIZED = 401;                    |
| 20  | int | SC_PAYMENT_REQUIRED = 402;                |
| 21  | int | SC_FORBIDDEN = 403;                       |
| 22  | int | SC_NOT_FOUND = 404;                       |
| 23  | int | SC_METHOD_NOT_ALLOWED = 405;              |
| 24  | int | SC_NOT_ACCEPTABLE = 406;                  |
| 25  | int | SC_PROXY_AUTHENTICATION_REQUIRED = 407;   |
| 26  | int | SC_REQUEST_TIMEOUT = 408;                 |
| 27  | int | SC_CONFLICT = 409;                        |
| 28  | int | SC_GONE = 410;                            |
| 29  | int | SC_LENGTH_REQUIRED = 411;                 |
| 30  | int | SC_PRECONDITION_FAILED = 412;             |
| 31  | int | SC_REQUEST_ENTITY_TOO_LARGE = 413;        |
| 32  | int | SC_REQUEST_URI_TOO_LONG = 414;            |
| 33  | int | SC_UNSUPPORTED_MEDIA_TYPE = 415;          |
| 34  | int | SC_REQUESTED_RANGE_NOT_SATISFIABLE = 416; |
| 35  | int | SC_EXPECTATION_FAILED = 417;              |
| 36  | int | SC_INTERNAL_SERVER_ERROR = 500;           |
| 37  | int | SC_NOT_IMPLEMENTED = 501;                 |
| 38  | int | SC_BAD_GATEWAY = 502;                     |
| 39  | int | SC_SERVICE_UNAVAILABLE = 503;             |
| 40  | int | SC_GATEWAY_TIMEOUT = 504;                 |
| 41  | int | SC_HTTP_VERSION_NOT_SUPPORTED = 505;      |

### 2、下载文件

1. 向浏览器输出消息 （一直在讲，就不说了）
1. 下载文件
   1. 要获取下载文件的路径
   1. 下载的文件名是啥？
   1. 设置想办法让浏览器能够支持下载我们需要的东西
   1. 获取下载文件的输入流
   1. 创建缓冲区
   1. 获取 OutputStream 对象
   1. 将 FileOutputStream 流写入到 buﬀer 缓冲区
   1. 使用 OutputStream 将缓冲区中的数据输出到客户端！

@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
// 1. 要获取下载文件的路径
String realPath = "F:\\班级管理\\西开【19525】\\2、代码\\JavaWeb\\javaweb- 02-servlet\\response\\target\\classes\\秦疆.png";
System.out.println("下载文件的路径："+realPath);
1
2

3
4

5

| 6
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
17 | // 2. 下载的文件名是啥？
String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);
// 3. 设置想办法让浏览器能够支持(Content-Disposition)下载我们需要的东西,中文文件名 URLEncoder.encode 编码，否则有可能乱码
resp.setHeader("Content- Disposition","attachment;filename="+URLEncoder.encode(fileName,"UTF-8"));
// 4. 获取下载文件的输入流
FileInputStream in = new FileInputStream(realPath);
// 5. 创建缓冲区
int len = 0;
byte[] buffer = new byte[1024];
// 6. 获取 OutputStream 对象
ServletOutputStream out = resp.getOutputStream();
// 7. 将 FileOutputStream 流写入到 buffer 缓冲区,使用 OutputStream 将缓冲区中的数据输出到客户端！ | |
| --- | --- | --- |
| 18 | | while ((len=in.read(buffer))>0){ |
| 19 | | out.write(buffer,0,len); |
| 20 | | } |
| 21 | | |
| 22 | | in.close(); |
| 23 | | out.close(); |
| 24 | } | |

### 3、验证码功能

验证怎么来的？
前端实现
后端实现，需要用到 Java 的图片类，生产一个图片

| 1   | package com.kuang.servlet;                                             |
| --- | ---------------------------------------------------------------------- |
| 2   |                                                                        |
| 3   | import javax.imageio.ImageIO;                                          |
| 4   | import javax.servlet.ServletException;                                 |
| 5   | import javax.servlet.http.HttpServlet;                                 |
| 6   | import javax.servlet.http.HttpServletRequest;                          |
| 7   | import javax.servlet.http.HttpServletResponse;                         |
| 8   | import java.awt.\*;                                                    |
| 9   | import java.awt.image.BufferedImage;                                   |
| 10  | import java.io.IOException;                                            |
| 11  | import java.util.Random;                                               |
| 12  |                                                                        |
| 13  | public class ImageServlet extends HttpServlet {                        |
| 14  |                                                                        |
| 15  | @Override                                                              |
| 16  | protected void doGet(HttpServletRequest req, HttpServletResponse resp) |
|     | throws ServletException, IOException {                                 |
| 17  |                                                                        |
| 18  | //如何让浏览器 3 秒自动刷新一次;                                       |
| 19  | resp.setHeader("refresh","3");                                         |
| 20  |                                                                        |
| 21  | //在内存中创建一个图片                                                 |
| 22  | BufferedImage image = new                                              |
|     | BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);                       |
| 23  | //得到图片                                                             |
| 24  | Graphics2D g = (Graphics2D) image.getGraphics(); //笔                  |
| 25  | //设置图片的背景颜色                                                   |
| 26  | g.setColor(Color.white);                                               |

| 27  | g.fillRect(0,0,80,20);                                                  |
| --- | ----------------------------------------------------------------------- |
| 28  | //给图片写数据                                                          |
| 29  | g.setColor(Color.BLUE);                                                 |
| 30  | g.setFont(new Font(null,Font.BOLD,20));                                 |
| 31  | g.drawString(makeNum(),0,20);                                           |
| 32  |                                                                         |
| 33  | //告诉浏览器，这个请求用图片的方式打开                                  |
| 34  | resp.setContentType("image/jpeg");                                      |
| 35  | //网站存在缓存，不让浏览器缓存                                          |
| 36  | resp.setDateHeader("expires",-1);                                       |
| 37  | resp.setHeader("Cache-Control","no-cache");                             |
| 38  | resp.setHeader("Pragma","no-cache");                                    |
| 39  |                                                                         |
| 40  | //把图片写给浏览器                                                      |
| 41  | ImageIO.write(image,"jpg", resp.getOutputStream());                     |
| 42  |                                                                         |
| 43  | }                                                                       |
| 44  |                                                                         |
| 45  | //生成随机数                                                            |
| 46  | private String makeNum(){                                               |
| 47  | Random random = new Random();                                           |
| 48  | String num = random.nextInt(9999999) + "";                              |
| 49  | StringBuffer sb = new StringBuffer();                                   |
| 50  | for (int i = 0; i < 7-num.length() ; i++) {                             |
| 51  | sb.append("0");                                                         |
| 52  | }                                                                       |
| 53  | num = sb.toString() + num;                                              |
| 54  | return num;                                                             |
| 55  | }                                                                       |
| 56  |                                                                         |
| 57  |                                                                         |
| 58  | @Override                                                               |
| 59  | protected void doPost(HttpServletRequest req, HttpServletResponse resp) |
|     | throws ServletException, IOException {                                  |
| 60  | doGet(req, resp);                                                       |
| 61  | }                                                                       |
| 62  | }                                                                       |
| 63  |                                                                         |

### ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834477204-f5bd9036-7432-4e38-a884-4aa02c61b8f0.png#)4、实现重定向

B 一个 web 资源收到客户端 A 请求后，B 他会通知 A 客户端去访问另外一个 web 资源 C，这个过程叫重定向 常见场景：

用户登录

| 1   | void | sendRedirect(String | var1) | throws | IOException; |
| --- | ---- | ------------------- | ----- | ------ | ------------ |

测试：

| 1   | @Override                                                                     |
| --- | ----------------------------------------------------------------------------- |
| 2   | protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws |
|     | ServletException, IOException {                                               |
| 3   |                                                                               |
| 4   | /\*                                                                           |
| 5   | resp.setHeader("Location","/r/img");                                          |
| 6   | resp.setStatus(302);                                                          |
| 7   | \*/                                                                           |
| 8   | resp.sendRedirect("/r/img");//重定向                                          |
| 9   | }                                                                             |

面试题：请你聊聊重定向和转发的区别？ 相同点
页面都会实现跳转不同点
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834477491-b1a7355a-bd3a-4f02-9f8f-d946122394f1.png#)请求转发的时候，url 不会产生变化重定向时候，url 地址栏会发生变化；
**5、简单实现登录重定向**

| 1   | <%--这里提交的路径，需要寻找到项目的路径--%>                          |
| --- | --------------------------------------------------------------------- |
| 2   | <%--${pageContext.request.contextPath}代表当前的项目--%>              |
| 3   |                                                                       |
| 4   | <form action="${pageContext.request.contextPath}/login" method="get"> |
| 5   | 用户名：<input type="text" name="username"> <br>                      |
| 6   | 密码：<input type="password" name="password"> <br>                    |
| 7   | <input type="submit">                                                 |
| 8   | </form>                                                               |
| 9   |                                                                       |

| 1   |                                                                        |
| --- | ---------------------------------------------------------------------- |
| 2   | @Override                                                              |
| 3   | protected void doGet(HttpServletRequest req, HttpServletResponse resp) |
|     | throws ServletException, IOException {                                 |
| 4   | //处理请求                                                             |
| 5   | String username = req.getParameter("username");                        |
| 6   | String password = req.getParameter("password");                        |
| 7   |                                                                        |
| 8   | System.out.println(username+":"+password);                             |
| 9   |                                                                        |
| 10  | //重定向时候一定要注意，路径问题，否则 404；                           |
| 11  | resp.sendRedirect("/r/success.jsp");                                   |
| 12  | }                                                                      |
| 13  |                                                                        |

<servlet>
<servlet-name>requset</servlet-name>
<servlet-class>com.kuang.servlet.RequestTest</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>requset</servlet-name>
<url-pattern>/login</url-pattern>
</servlet-mapping>
1
2
3
4
5
6
7
8

| 1   | <%@ page contentType="text/html;charset=UTF-8" language="java" %> |
| --- | ----------------------------------------------------------------- |
| 2   | <html>                                                            |
| 3   | <head>                                                            |
| 4   | <title>Title</title>                                              |
| 5   | </head>                                                           |
| 6   | <body>                                                            |
| 7   |                                                                   |
| 8   | <h1>Success</h1>                                                  |
| 9   |                                                                   |
| 10  | </body>                                                           |
| 11  | </html>                                                           |
| 12  |                                                                   |

## 、HttpServletRequest

HttpServletRequest 代表客户端的请求，用户通过 Http 协议访问服务器，HTTP 请求中的所有信息会被封 装到 HttpServletRequest，通过这个 HttpServletRequest 的方法，获得客户端的所有信息；

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834477859-6158c9d7-2d43-4c14-ba1b-d9fafa158e93.png#)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834478180-80430904-6133-4ada-be0e-0c2fd96d01c6.png#)

### ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834478468-47418bb8-3322-4d72-872b-4ee37050a62f.jpeg#)获取参数，请求转发

@Override
1

| 2

3
4
5
6
7
8
9 | protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

req.setCharacterEncoding("utf-8"); resp.setCharacterEncoding("utf-8");

| String username = req.getParameter("username"); String password = req.getParameter("password"); String[] hobbys = req.getParameterValues("hobbys"); |     |
| --------------------------------------------------------------------------------------------------------------------------------------------------- | --- | ----------------------------------------------------------- |
| 10                                                                                                                                                  |     | System.out.println("=============================");        |
| 11                                                                                                                                                  |     | //后台接收中文乱码问题                                      |
| 12                                                                                                                                                  |     | System.out.println(username);                               |
| 13                                                                                                                                                  |     | System.out.println(password);                               |
| 14                                                                                                                                                  |     | System.out.println(Arrays.toString(hobbys));                |
| 15                                                                                                                                                  |     | System.out.println("=============================");        |
| 16                                                                                                                                                  |     |                                                             |
| 17                                                                                                                                                  |     |                                                             |
| 18                                                                                                                                                  |     | System.out.println(req.getContextPath());                   |
| 19                                                                                                                                                  |     | //通过请求转发                                              |
| 20                                                                                                                                                  |     | //这里的 / 代表当前的 web 应用                              |
| 21                                                                                                                                                  |     | req.getRequestDispatcher("/success.jsp").forward(req,resp); |
| 22                                                                                                                                                  |     |                                                             |
| 23                                                                                                                                                  | }   |                                                             |

#### 面试题：请你聊聊重定向和转发的区别？

相同点
页面都会实现跳转不同点
请求转发的时候，url 不会产生变化 307
重定向时候，url 地址栏会发生变化； 302

# 7、Cookie、Session

## 、会话

**会话**：用户打开一个浏览器，点击了很多超链接，访问多个 web 资源，关闭浏览器，这个过程可以称之 为会话；
**有状态会话**：一个同学来过教室，下次再来教室，我们会知道这个同学，曾经来过，称之为有状态会 话；

#### 你能怎么证明你是西开的学生？

你 西开

1. 发票 西开给你发票
1. 学校登记 西开标记你来过了

#### 一个网站，怎么证明你来过？

客户端 服务端

1. 服务端给客户端一个 信件，客户端下次访问服务端带上信件就可以了； cookie
1. 服务器登记你来过了，下次你来的时候我来匹配你； seesion

## 、保存会话的两种技术

#### cookie

客户端技术 （响应，请求）

#### session

服务器技术，利用这个技术，可以保存用户的会话信息？ 我们可以把信息或者数据放在 Session 中！

常见常见：网站登录之后，你下次不用再登录了，第二次访问直接就上去了！

## ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834478800-a6634c1f-36a8-49d3-a79d-039be6788d55.png#)、Cookie

1. 从请求中拿到 cookie 信息
1. 服务器响应给客户端 cookie

| 1   | Cookie[] cookies = req.getCookies(); //获得 Cookie                            |
| --- | ----------------------------------------------------------------------------- |
| 2   | cookie.getName(); //获得 cookie 中的 key                                      |
| 3   | cookie.getValue(); //获得 cookie 中的 vlaue                                   |
| 4   | new Cookie("lastLoginTime", System.currentTimeMillis()+""); //新建一个 cookie |
| 5   | cookie.setMaxAge(24*60*60); //设置 cookie 的有效期                            |
| 6   | resp.addCookie(cookie); //响应给客户端一个 cookie                             |

#### cookie：一般会保存在本地的 用户目录下 appdata；

一个网站 cookie 是否存在上限！**聊聊细节问题**一个 Cookie 只能保存一个信息；
一个 web 站点可以给浏览器发送多个 cookie，最多存放 20 个 cookie；

Cookie 大小有限制 4kb； 300 个 cookie 浏览器上限

#### 删除 Cookie；

不设置有效期，关闭浏览器，自动失效； 设置有效期时间为 0 ；

**编码解码：**

| 1   | URLEncoder.encode("秦疆","utf-8")            |
| --- | -------------------------------------------- |
| 2   | URLDecoder.decode(cookie.getValue(),"UTF-8") |

## ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834479216-04fd6a43-f02c-48a4-b265-32919098928c.png#)、Session（重点）

什么是 Session：
服务器会给每一个用户（浏览器）创建一个 Seesion 对象；
一个 Seesion 独占一个浏览器，只要浏览器没有关闭，这个 Session 就存在；
用户登录之后，整个网站它都可以访问！--> 保存用户的信息；保存购物车的信息…..

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834479532-8a732b16-3615-42a6-a7f6-b2d043fbba0b.png#)
Session 和 cookie 的区别：
Cookie 是把用户的数据写给用户的浏览器，浏览器保存 （可以保存多个）
Session 把用户的数据写到用户独占 Session 中，服务器端保存 （保存重要的信息，减少服务器资源的浪费）
Session 对象由服务创建；

使用场景：
保存一个登录用户的信息； 购物车信息；
在整个网站中经常会使用的数据，我们将它保存在 Session 中；

使用 Session：

| 1 | package com.kuang.servlet;

import com.kuang.pojo.Person;

import javax.servlet.ServletException; import javax.servlet.http.\*;
import java.io.IOException;

public class SessionDemo01 extends HttpServlet { @Override
protected void doGet(HttpServletRequest req, throws ServletException, IOException {

//解决乱码问题 |

HttpServletResponse |

| resp) |
| ----- | --- | --- | --- |
| 2     |     |     |     |
| 3     |     |     |     |
| 4     |     |     |     |
| 5     |     |     |     |
| 6     |     |     |     |
| 7     |     |     |     |
| 8     |     |     |     |
| 9     |     |     |     |
| 10    |     |     |     |
| 11    |     |     |     |

|
12 | | | |
| 13 | | | |

| 14  | req.setCharacterEncoding("UTF-8");                                      |
| --- | ----------------------------------------------------------------------- |
| 15  | resp.setCharacterEncoding("UTF-8");                                     |
| 16  | resp.setContentType("text/html;charset=utf-8");                         |
| 17  |                                                                         |
| 18  | //得到 Session                                                          |
| 19  | HttpSession session = req.getSession();                                 |
| 20  | //给 Session 中存东西                                                   |
| 21  | session.setAttribute("name",new Person("秦疆",1));                      |
| 22  | //获取 Session 的 ID                                                    |
| 23  | String sessionId = session.getId();                                     |
| 24  |                                                                         |
| 25  | //判断 Session 是不是新创建                                             |
| 26  | if (session.isNew()){                                                   |
| 27  | resp.getWriter().write("session 创建成功,ID:"+sessionId);               |
| 28  | }else {                                                                 |
| 29  | resp.getWriter().write("session 以及在服务器中存在                      |
|     | 了,ID:"+sessionId);                                                     |
| 30  | }                                                                       |
| 31  |                                                                         |
| 32  | //Session 创建的时候做了什么事情；                                      |
| 33  | // Cookie cookie = new Cookie("JSESSIONID",sessionId);                  |
| 34  | // resp.addCookie(cookie);                                              |
| 35  |                                                                         |
| 36  | }                                                                       |
| 37  |                                                                         |
| 38  | @Override                                                               |
| 39  | protected void doPost(HttpServletRequest req, HttpServletResponse resp) |
|     | throws ServletException, IOException {                                  |
| 40  | doGet(req, resp);                                                       |
| 41  | }                                                                       |
| 42  | }                                                                       |
| 43  |                                                                         |
| 44  | //得到 Session                                                          |
| 45  | HttpSession session = req.getSession();                                 |
| 46  |                                                                         |
| 47  | Person person = (Person) session.getAttribute("name");                  |
| 48  |                                                                         |
| 49  | System.out.println(person.toString());                                  |
| 50  |                                                                         |
| 51  | HttpSession session = req.getSession();                                 |
| 52  | session.removeAttribute("name");                                        |
| 53  | //手动注销 Session                                                      |
| 54  | session.invalidate();                                                   |

**会话自动过期：web.xml 配置**

<!--设置Session默认的失效时间-->
<session-config>
<!--15分钟后Session自动失效，以分钟为单位-->
<session-timeout>15</session-timeout>
</session-config>
1
2
3
4
5

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834479805-8d6795a6-2330-4243-9ab1-e73d015e5939.png#)

# 8、JSP

## 、什么是 JSP

Java Server Pages ： Java 服务器端页面，也和 Servlet 一样，用于动态 Web 技术！ 最大的特点：
写 JSP 就像在写 HTML
区别：
HTML 只给用户提供静态的数据
JSP 页面中可以嵌入 JAVA 代码，为用户提供动态数据；

## 、JSP 原理

思路：JSP 到底怎么执行的！
代码层面没有任何问题服务器内部工作
tomcat 中有一个 work 目录；
IDEA 中使用 Tomcat 的会在 IDEA 的 tomcat 中生产一个 work 目录

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834480139-d09feb9e-e234-4d65-a839-ee3359413ec7.jpeg#)
我电脑的地址：

C:\Users\Administrator\.IntelliJIdea2018.1\system\tomcat\Unnamed_javaweb-
session-cookie\work\Catalina\localhost\ROOT\org\apache\jsp
1

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834480536-f8edce22-a24b-4787-8e98-3816daf88e5c.png#)发现页面转变成了 Java 程序！

#### 浏览器向服务器发送请求，不管访问什么资源，其实都是在访问 Servlet！

JSP 最终也会被转换成为一个 Java 类！

#### JSP 本质上就是一个 Servlet

| 1   | //初始化                                                                 |
| --- | ------------------------------------------------------------------------ |
| 2   | public void \_jspInit() {                                                |
| 3   |                                                                          |
| 4   | }                                                                        |
| 5   | //销毁                                                                   |
| 6   | public void \_jspDestroy() {                                             |
| 7   | }                                                                        |
| 8   | //JSPService                                                             |
| 9   | public void \_jspService(.HttpServletRequest request,HttpServletResponse |
|     | response)                                                                |
| 10  |                                                                          |

1. 判断请求
1. 内置一些对象
   | 1 | final javax.servlet.jsp.PageContext pageContext; | //页面上下文 |
   | --- | --- | --- |
   | 2 | javax.servlet.http.HttpSession session = null; | //session |
   | 3 | final javax.servlet.ServletContext application; | //applicationContext |
   | 4 | final javax.servlet.ServletConfig config; | //config |
   | 5 | javax.servlet.jsp.JspWriter out = null; | //out |
   | 6 | final java.lang.Object page = this; | //page：当前 |
   | 7 | HttpServletRequest request | //请求 |
   | 8 | HttpServletResponse response | //响应 |

1. 输出页面前增加的代码

pageContext = \_jspxFactory.getPageContext(this, request, response,
null, true, 8192, true);
\_jspx_page_context = pageContext;
application = pageContext.getServletContext(); config = pageContext.getServletConfig(); session = pageContext.getSession();
out = pageContext.getOut();
\_jspx_out = out;
//设置响应的页面类型
response.setContentType("text/html");
1
2
3
4
5
6
7
8
9

1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834480845-d51ecbee-31c0-4da6-bdb7-a8cb35f99077.png#)以上的这些个对象我们可以在 JSP 页面中直接使用！

在 JSP 页面中；

只要是 JAVA 代码就会原封不动的输出； 如果是 HTML 代码，就会被转换为：
out.write("<html>\r\n");
1

这样的格式，输出到前端！

## 、JSP 基础语法

任何语言都有自己的语法，JAVA 中有,。 JSP 作为 java 技术的一种应用，它拥有一些自己扩充的语法（了解，知道即可！），Java 所有语法都支持！

### JSP 表达式

| 1   | <%--JSP 表达式                       |
| --- | ------------------------------------ |
| 2   | 作用：用来将程序的输出，输出到客户端 |
| 3   | <%= 变量或者表达式%>                 |
| 4   | --%>                                 |
| 5   | <%= new java.util.Date()%>           |

**jsp 脚本片段**

| 1   |                                      |        |
| --- | ------------------------------------ | ------ |
| 2   | <%--jsp 脚本片段--%>                 |        |
| 3   | <%                                   |        |
| 4   | int sum = 0;                         |        |
| 5   | for (int i = 1; i <=100 ;            | i++) { |
| 6   | sum+=i;                              |        |
| 7   | }                                    |        |
| 8   | out.println("<h1>Sum="+sum+"</h1>"); |        |
| 9   | %>                                   |        |
| 10  |                                      |        |

**脚本片段的再实现**

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
<%
int x = 10; out.println(x);
%>

<p>这是一个JSP文档</p>
<%
int y = 2; out.println(y);
%>
<hr>

<%--在代码嵌入 HTML 元素--%>

| 15  | <%                            |
| --- | ----------------------------- |
| 16  | for (int i = 0; i < 5; i++) { |
| 17  | %>                            |
| 18  | <h1>Hello,World <%=i%> </h1>  |
| 19  | <%                            |
| 20  | }                             |
| 21  | %>                            |

### JSP 声明

| 1   | <%!                                       |
| --- | ----------------------------------------- |
| 2   | static {                                  |
| 3   | System.out.println("Loading Servlet!");   |
| 4   | }                                         |
| 5   |                                           |
| 6   | private int globalVar = 0;                |
| 7   |                                           |
| 8   | public void kuang(){                      |
| 9   | System.out.println("进入了方法 Kuang！"); |
| 10  | }                                         |
| 11  | %>                                        |

JSP 声明：会被编译到 JSP 生成 Java 的类中！其他的，就会被生成到\_jspService 方法中！ 在 JSP，嵌入 Java 代码即可！
<%%>
<%=%>
<%!%>

<%--注释--%>
1
2
3
4
5

JSP 的注释，不会在客户端显示，HTML 就会！

## 、JSP 指令

<%--jSP 标签
jsp:include：拼接页面，本质还是三个
--%>
<%@page args %>
<%@include file=""%>

<%--@include会将两个页面合二为一--%>

<%@include file="common/header.jsp"%>

<h1>网页主体</h1>

<%@include file="common/footer.jsp"%>

<hr>
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

| 17  | <jsp:include page="/common/header.jsp"/> |
| --- | ---------------------------------------- |
| 18  | <h1>网页主体</h1>                        |
| 19  | <jsp:include page="/common/footer.jsp"/> |
| 20  |                                          |

1.  **、9 大内置对象**

PageContext 存东西 Request 存东西 Response
Session 存东西
Application 【SerlvetContext】 存东西
conﬁg 【SerlvetConﬁg】
out
page ，不用了解
exception

1. pageContext.setAttribute("name1","秦疆 1 号"); //保存的数据只在一个页面中有效
1. request.setAttribute("name2","秦疆 2 号"); //保存的数据只在一次请求中有效，请求转发会携带这个数据
1. session.setAttribute("name3","秦疆 3 号"); //保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
1. application.setAttribute("name4","秦疆 4 号"); //保存的数据只在服务器中有效，从打开服务器到关闭服务器

request：客户端向服务器发送请求，产生的数据，用户看完就没用了，比如：新闻，用户看完没用的！ session：客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车；
application：客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用，比如： 聊天数据；

## 、JSP 标签、JSTL 标签、EL 表达式

<!-- JSTL表达式的依赖 -->
<dependency>
<groupId>javax.servlet.jsp.jstl</groupId>
<artifactId>jstl-api</artifactId>
<version>1.2</version>
</dependency>
<!-- standard标签库 -->
<dependency>
<groupId>taglibs</groupId>
<artifactId>standard</artifactId>
<version>1.1.2</version>
</dependency>
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

EL 表达式： ${ }

#### 获取数据执行运算

**获取 web 开发的常用对象**

#### JSP 标签

| 1   | <%--jsp:include--%>                                    |
| --- | ------------------------------------------------------ |
| 2   |                                                        |
| 3   | <%--                                                   |
| 4   | http://localhost:8080/jsptag.jsp?name=kuangshen&age=12 |
| 5   | --%>                                                   |
| 6   |                                                        |
| 7   | <jsp:forward page="/jsptag2.jsp">                      |
| 8   | <jsp:param name="name" value="kuangshen"></jsp:param>  |
| 9   | <jsp:param name="age" value="12"></jsp:param>          |
| 10  | </jsp:forward>                                         |

**JSTL 表达式**
JSTL 标签库的使用就是为了弥补 HTML 标签的不足；它自定义许多标签，可以供我们使用，标签的功能和 Java 代码一样！

#### 格式化标签 SQL 标签 XML 标签

**核心标签 **（掌握部分）
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834481140-6fff2f4d-ff9d-4123-83ee-6d47352f0619.png#)

#### JSTL 标签库使用步骤

引入对应的 taglib
使用其中的方法

#### 在 Tomcat 也需要引入 jstl 的包，否则会报错：JSTL 解析错误

c：if

| 1   | <head>                                                        |
| --- | ------------------------------------------------------------- |
| 2   | <title>Title</title>                                          |
| 3   | </head>                                                       |
| 4   | <body>                                                        |
| 5   |                                                               |
| 6   |                                                               |
| 7   | <h4>if 测试</h4>                                              |
| 8   |                                                               |
| 9   | <hr>                                                          |
| 10  |                                                               |
| 11  | <form action="coreif.jsp" method="get">                       |
| 12  | <%--                                                          |
| 13  | EL 表达式获取表单中的数据                                     |
| 14  | ${param.参数名}                                               |
| 15  | --%>                                                          |
| 16  | <input type="text" name="username" value="${param.username}"> |
| 17  | <input type="submit" value="登录">                            |
| 18  | </form>                                                       |
| 19  |                                                               |
| 20  | <%--判断如果提交的用户名是管理员，则登录成功--%>              |
| 21  | <c:if test="${param.username=='admin'}" var="isAdmin">        |
| 22  | <c:out value="管理员欢迎您！"/>                               |
| 23  | </c:if>                                                       |
| 24  |                                                               |
| 25  | <%--自闭合标签--%>                                            |
| 26  | <c:out value="${isAdmin}"/>                                   |
| 27  |                                                               |
| 28  | </body>                                                       |

c:choose c:when

| 1   | <body>                              |
| --- | ----------------------------------- |
| 2   |                                     |
| 3   | <%--定义一个变量 score，值为 85--%> |
| 4   | <c:set var="score" value="55"/>     |
| 5   |                                     |
| 6   | <c:choose>                          |
| 7   | <c:when test="${score>=90}">        |
| 8   | 你的成绩为优秀                      |
| 9   | </c:when>                           |
| 10  | <c:when test="${score>=80}">        |
| 11  | 你的成绩为一般                      |
| 12  | </c:when>                           |
| 13  | <c:when test="${score>=70}">        |
| 14  | 你的成绩为良好                      |
| 15  | </c:when>                           |
| 16  | <c:when test="${score<=60}">        |
| 17  | 你的成绩为不及格                    |
| 18  | </c:when>                           |
| 19  | </c:choose>                         |
| 20  |                                     |
| 21  | </body>                             |

c:forEach

<%
1

| 2   |                                                                      |
| --- | -------------------------------------------------------------------- |
| 3   | ArrayList<String> people = new ArrayList<>();                        |
| 4   | people.add(0,"张三");                                                |
| 5   | people.add(1,"李四");                                                |
| 6   | people.add(2,"王五");                                                |
| 7   | people.add(3,"赵六");                                                |
| 8   | people.add(4,"田六");                                                |
| 9   | request.setAttribute("list",people);                                 |
| 10  | %>                                                                   |
| 11  |                                                                      |
| 12  |                                                                      |
| 13  | <%--                                                                 |
| 14  | var , 每一次遍历出来的变量                                           |
| 15  | items, 要遍历的对象                                                  |
| 16  | begin, 哪里开始                                                      |
| 17  | end, 到哪里                                                          |
| 18  | step, 步长                                                           |
| 19  | --%>                                                                 |
| 20  | <c:forEach var="people" items="${list}">                             |
| 21  | <c:out value="${people}"/> <br>                                      |
| 22  | </c:forEach>                                                         |
| 23  |                                                                      |
| 24  | <hr>                                                                 |
| 25  |                                                                      |
| 26  | <c:forEach var="people" items="${list}" begin="1" end="3" step="1" > |
| 27  | <c:out value="${people}"/> <br>                                      |
| 28  | </c:forEach>                                                         |
| 29  |                                                                      |

# 9、JavaBean

实体类
JavaBean 有特定的写法：
必须要有一个无参构造属性必须私有化
必须有对应的 get/set 方法；
一般用来和数据库的字段做映射 ORM； ORM ：对象关系映射
表--->类
字段-->属性
行记录 >对象

#### people 表

| **id** | **name**  | **age** | **address** |
| ------ | --------- | ------- | ----------- |
| 1      | 秦疆 1 号 | 3       | 西安        |
| 2      | 秦疆 2 号 | 18      | 西安        |
| 3      | 秦疆 3 号 | 100     | 西安        |

| 1   | class People{                        |
| --- | ------------------------------------ |
| 2   | private int id;                      |
| 3   | private String name;                 |
| 4   | private int id;                      |
| 5   | private String address;              |
| 6   | }                                    |
| 7   |                                      |
| 8   | class A{                             |
| 9   | new People(1,"秦疆 1 号",3，"西安"); |
| 10  | new People(2,"秦疆 2 号",3，"西安"); |
| 11  | new People(3,"秦疆 3 号",3，"西安"); |
| 12  | }                                    |

过滤器文件上传邮件发送
JDBC 复习 ： 如何使用 JDBC , JDBC crud， jdbc 事务

# 10、MVC 三层架构

什么是 MVC： Model view Controller 模型、视图、控制器

## ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834481445-e2b52689-409f-464d-9ab9-21a2365bd81a.png#)、早些年

用户直接访问控制层，控制层就可以直接操作数据库；

| 1   | servlet--CRUD-->数据库                                                            |
| --- | --------------------------------------------------------------------------------- | --- |
| 2   | 弊端：程序十分臃肿，不利于维护                                                    |
| 3   | servlet 的代码中：处理请求、响应、视图跳转、处理 JDBC、处理业务代码、处理逻辑代码 |
| 4   |                                                                                   |
| 5   | 架构：没有什么是加一层解决不了的！                                                |
| 6   | 程序猿调用                                                                        |
| 7   |                                                                                   |     |
| 8   | JDBC                                                                              |
| 9   |                                                                                   |     |
| 10  | Mysql Oracle SqlServer ....                                                       |

## 、MVC 三层架构

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834481794-ed19dbed-9bae-4355-987f-206aed1f3d4d.jpeg#)

Model
业务处理 ：业务逻辑（Service） 数据持久层：CRUD （Dao）

View

展示数据
提供链接发起 Servlet 请求 （a，form，img…）

Controller （Servlet）
接收用户的请求 ：（req：请求参数、Session 信息….） 交给业务层处理对应的代码
控制视图的跳转

登录--->接收用户的登录请求--->处理用户的请求（获取用户登录的参数，username， password）---->交给业务层处理登录业务（判断用户名密码是否正确：事务）--->Dao 层查询用 户名和密码是否正确-->数据库
1

# 11、Filter （重点）

Filter：过滤器 ，用来过滤网站的数据； 处理中文乱码
登录验证….

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834482162-69f4c924-310c-4e59-8663-09b49dfe174d.png#)

Filter 开发步骤：

      1. 导包
      1. 编写过滤器
         1. ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834482533-aa79d066-cbe1-47a1-bcd5-23e339231f49.jpeg#)导包不要错

实现 Filter 接口，重写对应的方法即可

| 1   | public class CharacterEncodingFilter implements Filter {            |
| --- | ------------------------------------------------------------------- |
| 2   |                                                                     |
| 3   | //初始化：web 服务器启动，就以及初始化了，随时等待过滤对象出现！    |
| 4   | public void init(FilterConfig filterConfig) throws                  |
|     | ServletException {                                                  |
| 5   | System.out.println("CharacterEncodingFilter 初始化");               |
| 6   | }                                                                   |
| 7   |                                                                     |
| 8   | //Chain : 链                                                        |
| 9   | /\*                                                                 |
| 10  | 1. 过滤中的所有代码，在过滤特定请求的时候都会执行                   |
| 11  | 2. 必须要让过滤器继续同行                                           |
| 12  | chain.doFilter(request,response);                                   |
| 13  | \*/                                                                 |
| 14  | public void doFilter(ServletRequest request, ServletResponse        |
|     | response, FilterChain chain) throws IOException, ServletException { |
| 15  | request.setCharacterEncoding("utf-8");                              |
| 16  | response.setCharacterEncoding("utf-8");                             |
| 17  | response.setContentType("text/html;charset=UTF-8");                 |
| 18  |                                                                     |
| 19  | System.out.println("CharacterEncodingFilter 执行前 ");              |

| 20 | chain.doFilter(request,response); //让我们的请求继续走，如果不
写，程序到这里就被拦截停止！ | |
| --- | --- | --- |
| 21 | | System.out.println("CharacterEncodingFilter 执行后 "); |
| 22 | | } |
| 23 | | |
| 24 | | //销毁：web 服务器关闭的时候，过滤会销毁 |
| 25 | | public void destroy() { |
| 26 | | System.out.println("CharacterEncodingFilter 销毁"); |
| 27 | | } |
| 28 | } | |
| 29 | | |

      1. 在web.xml中配置 Filter

<filter>
<filter-name>CharacterEncodingFilter</filter-name>
<filter-class>com.kuang.filter.CharacterEncodingFilter</filter- class>
</filter>
<filter-mapping>
<filter-name>CharacterEncodingFilter</filter-name>
<!--只要是 /servlet的任何请求，会经过这个过滤器-->
<url-pattern>/servlet/*</url-pattern>
<!--<url-pattern>/*</url-pattern>-->
</filter-mapping>
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

# 12、监听器

实现一个监听器的接口；（有 N 种）

1. 编写一个监听器

实现监听器的接口…

| 1   | //统计网站在线人数 ： 统计 session                                |
| --- | ----------------------------------------------------------------- |
| 2   | public class OnlineCountListener implements HttpSessionListener { |
| 3   |                                                                   |
| 4   | //创建 session 监听： 看你的一举一动                              |
| 5   | //一旦创建 Session 就会触发一次这个事件！                         |
| 6   | public void sessionCreated(HttpSessionEvent se) {                 |
| 7   | ServletContext ctx = se.getSession().getServletContext();         |
| 8   |                                                                   |
| 9   | System.out.println(se.getSession().getId());                      |
| 10  |                                                                   |
| 11  | Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");  |
| 12  |                                                                   |
| 13  | if (onlineCount==null){                                           |
| 14  | onlineCount = new Integer(1);                                     |
| 15  | }else {                                                           |
| 16  | int count = onlineCount.intValue();                               |
| 17  | onlineCount = new Integer(count+1);                               |
| 18  | }                                                                 |
| 19  |                                                                   |
| 20  | ctx.setAttribute("OnlineCount",onlineCount);                      |
| 21  |                                                                   |
| 22  | }                                                                 |

| 23  |     |                                                                  |
| --- | --- | ---------------------------------------------------------------- |
| 24  |     | //销毁 session 监听                                              |
| 25  |     | //一旦销毁 Session 就会触发一次这个事件！                        |
| 26  |     | public void sessionDestroyed(HttpSessionEvent se) {              |
| 27  |     | ServletContext ctx = se.getSession().getServletContext();        |
| 28  |     |                                                                  |
| 29  |     | Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount"); |
| 30  |     |                                                                  |
| 31  |     | if (onlineCount==null){                                          |
| 32  |     | onlineCount = new Integer(0);                                    |
| 33  |     | }else {                                                          |
| 34  |     | int count = onlineCount.intValue();                              |
| 35  |     | onlineCount = new Integer(count-1);                              |
| 36  |     | }                                                                |
| 37  |     |                                                                  |
| 38  |     | ctx.setAttribute("OnlineCount",onlineCount);                     |
| 39  |     |                                                                  |
| 40  |     | }                                                                |
| 41  |     |                                                                  |
| 42  |     |                                                                  |
| 43  |     | /\*                                                              |
| 44  |     | Session 销毁：                                                   |
| 45  |     | 1. 手动销毁 getSession().invalidate();                           |
| 46  |     | 2. 自动销毁                                                      |
| 47  |     | \*/                                                              |
| 48  | }   |                                                                  |
| 49  |     |                                                                  |

1. web.xml 中注册监听器

<!--注册监听器-->
<listener>
<listener-class>com.kuang.listener.OnlineCountListener</listener- class>
</listener>
1
2
3

4

1. 看情况是否使用！

# 13、过滤器、监听器常见应用

#### 监听器：GUI 编程中经常使用；

| 1   | public class TestPanel {                                |
| --- | ------------------------------------------------------- |
| 2   | public static void main(String[] args) {                |
| 3   | Frame frame = new Frame("中秋节快乐"); //新建一个窗体   |
| 4   | Panel panel = new Panel(null); //面板                   |
| 5   | frame.setLayout(null); //设置窗体的布局                 |
| 6   |                                                         |
| 7   | frame.setBounds(300,300,500,500);                       |
| 8   | frame.setBackground(new Color(0,0,255)); //设置背景颜色 |
| 9   |                                                         |
| 10  | panel.setBounds(50,50,300,300);                         |
| 11  | panel.setBackground(new Color(0,255,0)); //设置背景颜色 |
| 12  |                                                         |
| 13  | frame.add(panel);                                       |

| 14  |     |     |                                               |
| --- | --- | --- | --------------------------------------------- |
| 15  |     |     | frame.setVisible(true);                       |
| 16  |     |     |                                               |
| 17  |     |     | //监听事件，监听关闭事件                      |
| 18  |     |     | frame.addWindowListener(new WindowAdapter() { |
| 19  |     |     | @Override                                     |
| 20  |     |     | public void windowClosing(WindowEvent e) {    |
| 21  |     |     | super.windowClosing(e);                       |
| 22  |     |     | }                                             |
| 23  |     |     | });                                           |
| 24  |     |     |                                               |
| 25  |     |     |                                               |
| 26  |     | }   |                                               |
| 27  | }   |     |                                               |

用户登录之后才能进入主页！用户注销后就不能进入主页了！

1. 用户登录之后，向 Sesison 中放入用户的数据
1. 进入主页的时候要判断用户是否已经登录；要求：在过滤器中实现！

HttpServletRequest request = (HttpServletRequest) req; HttpServletResponse response = (HttpServletResponse) resp;

if (request.getSession().getAttribute(Constant.USER_SESSION)==null){ response.sendRedirect("/error.jsp");
}

chain.doFilter(request,response);
1
2
3
4
5
6
7
8

# 14、JDBC

什么是 JDBC ： Java 连接数据库！

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834482964-b189b7f7-d4a7-40d4-8cff-c0cfe92ce82a.png#)

需要 jar 包的支持： java.sql javax.sql
mysql-conneter-java… 连接驱动（必须要导入）

#### 实验环境搭建

| 1   |                                                        |
| --- | ------------------------------------------------------ |
| 2   | CREATE TABLE users(                                    |
| 3   | id INT PRIMARY KEY,                                    |
| 4   | `name` VARCHAR(40),                                    |
| 5   | `password` VARCHAR(40),                                |
| 6   | email VARCHAR(60),                                     |
| 7   | birthday DATE                                          |
| 8   | );                                                     |
| 9   |                                                        |
| 10  | INSERT INTO users(id,`name`,`password`,email,birthday) |
| 11  | VALUES(1,'张三','123456','zs@qq.com','2000-01-01');    |
| 12  | INSERT INTO users(id,`name`,`password`,email,birthday) |
| 13  | VALUES(2,'李四','123456','ls@qq.com','2000-01-01');    |
| 14  | INSERT INTO users(id,`name`,`password`,email,birthday) |
| 15  | VALUES(3,'王五','123456','ww@qq.com','2000-01-01');    |
| 16  |                                                        |
| 17  |                                                        |
| 18  | SELECT \* FROM users;                                  |
| 19  |                                                        |

导入数据库依赖

<!--mysql的驱动-->
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>5.1.47</version>
</dependency>
1
2
3
4
5
6

IDEA 中连接数据库：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834483267-e6352cdc-fbdb-4098-8488-9cbd8996f935.png#)

#### JDBC 固定步骤：

1. 加载驱动
1. 连接数据库,代表数据库
1. 向数据库发送 SQL 的对象 Statement : CRUD
1. 编写 SQL （根据业务，不同的 SQL）
1. 执行 SQL
1. 关闭连接

| 1   | public class TestJdbc {                                               |
| --- | --------------------------------------------------------------------- |
| 2   | public static void main(String[] args) throws ClassNotFoundException, |
|     | SQLException {                                                        |
| 3   | //配置信息                                                            |
| 4   | //useUnicode=true&characterEncoding=utf-8 解决中文乱码                |
| 5   | String url="jdbc:mysql://localhost:3306/jdbc?                         |
|     | useUnicode=true&characterEncoding=utf-8";                             |
| 6   | String username = "root";                                             |
| 7   | String password = "123456";                                           |
| 8   |                                                                       |
| 9   | //1.加载驱动                                                          |
| 10  | Class.forName("com.mysql.jdbc.Driver");                               |
| 11  | //2.连接数据库,代表数据库                                             |

| 12  | Connection connection = DriverManager.getConnection(url, username, |
| --- | ------------------------------------------------------------------ |
|     | password);                                                         |
| 13  |                                                                    |
| 14  | //3.向数据库发送 SQL 的对象 Statement,PreparedStatement : CRUD     |
| 15  | Statement statement = connection.createStatement();                |
| 16  |                                                                    |
| 17  | //4.编写 SQL                                                       |
| 18  | String sql = "select \* from users";                               |
| 19  |                                                                    |
| 20  | //5.执行查询 SQL，返回一个 ResultSet ： 结果集                     |
| 21  | ResultSet rs = statement.executeQuery(sql);                        |
| 22  |                                                                    |
| 23  | while (rs.next()){                                                 |
| 24  | System.out.println("id="+rs.getObject("id"));                      |
| 25  | System.out.println("name="+rs.getObject("name"));                  |
| 26  | System.out.println("password="+rs.getObject("password"));          |
| 27  | System.out.println("email="+rs.getObject("email"));                |
| 28  | System.out.println("birthday="+rs.getObject("birthday"));          |
| 29  | }                                                                  |
| 30  |                                                                    |
| 31  | //6.关闭连接，释放资源（一定要做） 先开后关                        |
| 32  | rs.close();                                                        |
| 33  | statement.close();                                                 |
| 34  | connection.close();                                                |
| 35  | }                                                                  |
| 36  | }                                                                  |
| 37  |                                                                    |

#### 预编译 SQL

| 1   | public class TestJDBC2 {                                                   |
| --- | -------------------------------------------------------------------------- |
| 2   | public static void main(String[] args) throws Exception {                  |
| 3   | //配置信息                                                                 |
| 4   | //useUnicode=true&characterEncoding=utf-8 解决中文乱码                     |
| 5   | String url="jdbc:mysql://localhost:3306/jdbc?                              |
|     | useUnicode=true&characterEncoding=utf-8";                                  |
| 6   | String username = "root";                                                  |
| 7   | String password = "123456";                                                |
| 8   |                                                                            |
| 9   | //1.加载驱动                                                               |
| 10  | Class.forName("com.mysql.jdbc.Driver");                                    |
| 11  | //2.连接数据库,代表数据库                                                  |
| 12  | Connection connection = DriverManager.getConnection(url, username,         |
|     | password);                                                                 |
| 13  |                                                                            |
| 14  | //3.编写 SQL                                                               |
| 15  | String sql = "insert into users(id, name, password, email,                 |
|     | birthday) values (?,?,?,?,?);";                                            |
| 16  |                                                                            |
| 17  | //4.预编译                                                                 |
| 18  | PreparedStatement preparedStatement =                                      |
|     | connection.prepareStatement(sql);                                          |
| 19  |                                                                            |
| 20  | preparedStatement.setInt(1,2);//给第一个占位符？ 的值赋值为 1；            |
| 21  | preparedStatement.setString(2,"狂神说 Java");//给第二个占位符？ 的值赋值为 |
|     | 狂神说 Java；                                                              |

| 22  | preparedStatement.setString(3,"123456");//给第三个占位符？ 的值赋值为                            |
| --- | ------------------------------------------------------------------------------------------------ |
|     | 123456；                                                                                         |
| 23  | preparedStatement.setString(4,"[24736743@qq.com](mailto:24736743@qq.com)");//给第四个占位符？ 的 |
|     | 值赋值为 1；                                                                                     |
| 24  | preparedStatement.setDate(5,new Date(new                                                         |
|     | java.util.Date().getTime()));//给第五个占位符？ 的值赋值为 new Date(new                          |
|     | java.util.Date().getTime())；                                                                    |
| 25  |                                                                                                  |
| 26  | //5.执行 SQL                                                                                     |
| 27  | int i = preparedStatement.executeUpdate();                                                       |
| 28  |                                                                                                  |
| 29  | if (i>0){                                                                                        |
| 30  | System.out.println("插入成功@");                                                                 |
| 31  | }                                                                                                |
| 32  |                                                                                                  |
| 33  | //6.关闭连接，释放资源（一定要做） 先开后关                                                      |
| 34  | preparedStatement.close();                                                                       |
| 35  | connection.close();                                                                              |
| 36  | }                                                                                                |
| 37  | }                                                                                                |
| 38  |                                                                                                  |

**事务**
要么都成功，要么都失败！
ACID 原则：保证数据的安全。

| 1   | 开启事务 |            |         |
| --- | -------- | ---------- | ------- |
| 2   | 事务提交 | commit()   |         |
| 3   | 事务回滚 | rollback() |         |
| 4   | 关闭事务 |            |         |
| 5   |          |            |         |
| 6   | 转账：   |            |         |
| 7   | A:1000   |            |         |
| 8   | B:1000   |            |         |
| 9   |          |            |         |
| 10  | A(900)   | --100-->   | B(1100) |

#### Junit 单元测试

依赖

<!--单元测试-->
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>
</dependency>
1
2
3
4
5
6

简单使用
@Test 注解只有在方法上有效，只要加了这个注解的方法，就可以直接运行！

@Test
public void test(){ System.out.println("Hello");
}
1
2
3
4
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834483594-20b35299-8a97-4049-afa3-6c50a6d0641c.jpeg#)
失败的时候是红色：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834483912-bd3c0b17-ff15-405a-9f08-24a7e68562d9.jpeg#)

#### 搭建一个环境

| 1   | CREATE TABLE account(                               |
| --- | --------------------------------------------------- |
| 2   | id INT PRIMARY KEY AUTO_INCREMENT,                  |
| 3   | `name` VARCHAR(40),                                 |
| 4   | money FLOAT                                         |
| 5   | );                                                  |
| 6   |                                                     |
| 7   | INSERT INTO account(`name`,money) VALUES('A',1000); |
| 8   | INSERT INTO account(`name`,money) VALUES('B',1000); |
| 9   | INSERT INTO account(`name`,money) VALUES('C',1000); |

| 1   | @Test                                                           |
| --- | --------------------------------------------------------------- |
| 2   | public void test() {                                            |
| 3   | //配置信息                                                      |
| 4   | //useUnicode=true&characterEncoding=utf-8 解决中文乱码          |
| 5   | String url="jdbc:mysql://localhost:3306/jdbc?                   |
|     | useUnicode=true&characterEncoding=utf-8";                       |
| 6   | String username = "root";                                       |
| 7   | String password = "123456";                                     |
| 8   |                                                                 |
| 9   | Connection connection = null;                                   |
| 10  |                                                                 |
| 11  | //1.加载驱动                                                    |
| 12  | try {                                                           |
| 13  | Class.forName("com.mysql.jdbc.Driver");                         |
| 14  | //2.连接数据库,代表数据库                                       |
| 15  | connection = DriverManager.getConnection(url, username,         |
|     | password);                                                      |
| 16  |                                                                 |
| 17  | //3.通知数据库开启事务,false 开启                               |
| 18  | connection.setAutoCommit(false);                                |
| 19  |                                                                 |
| 20  | String sql = "update account set money = money-100 where name = |
|     | 'A'";                                                           |
| 21  | connection.prepareStatement(sql).executeUpdate();               |

| 22  |       |                                                                  |
| --- | ----- | ---------------------------------------------------------------- |
| 23  |       | //制造错误                                                       |
| 24  |       | //int i = 1/0;                                                   |
| 25  |       |                                                                  |
| 26  |       | String sql2 = "update account set money = money+100 where name = |
|     | 'B'"; |                                                                  |
| 27  |       | connection.prepareStatement(sql2).executeUpdate();               |
| 28  |       |                                                                  |
| 29  |       | connection.commit();//以上两条 SQL 都执行成功了，就提交事务！    |
| 30  |       | System.out.println("success");                                   |
| 31  |       | } catch (Exception e) {                                          |
| 32  |       | try {                                                            |
| 33  |       | //如果出现异常，就通知数据库回滚事务                             |
| 34  |       | connection.rollback();                                           |
| 35  |       | } catch (SQLException e1) {                                      |
| 36  |       | e1.printStackTrace();                                            |
| 37  |       | }                                                                |
| 38  |       | e.printStackTrace();                                             |
| 39  |       | }finally {                                                       |
| 40  |       | try {                                                            |
| 41  |       | connection.close();                                              |
| 42  |       | } catch (SQLException e) {                                       |
| 43  |       | e.printStackTrace();                                             |
| 44  |       | }                                                                |
| 45  |       | }                                                                |
| 46  | }     |                                                                  |
