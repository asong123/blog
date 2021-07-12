---
title: JAVAWEB
urlname: siqkqy
date: '2021-07-12 12:55:04 +0800'
tags: []
categories: []
---

---
# 基本概念
---

## 前言

web 开发：

- web，网页的意思,www.baidu.com
- 静态 web
  - html,css
  - 提供给所有人看的数据不会发生变化
- 动态 web
  - 淘宝
  - 技术栈：Servlet/JSP,ASP,PHP

在 java 中，动态 web 资源开发的技术统称为 JavaWeb；

## web 应用程序

web 应用程序：可以提供浏览器访问的程序

- a.html、b.html    .....多个 web 资源，这些资源可以被外界访问，对外界提供服务。
- 能访问到的任何一个页面或者资源，都存在与网络世界的某个计算机上。
- URL
- 这个统一的 web 资源会被放在同一个文件夹下，web 应用程序-->Tomcat：服务器
- 一个 web 应用程序由多个部分组成：
  - html、css、js
  - jsp、servlet
  - java 程序
  - 配置文件（Properties）

web 应用程序编写完后，若想提供给外界访问：需要一个服务器统一来管理；

## 静态 web

- _.htm _.html ，如果服务器上存在这些东邪，我们就可以通过网络读取。
- 静态 web 请求响应过程

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596896598463-669e4279-0bbe-40fe-ba4d-f21db66aede0.png#height=111&id=SOFNO&name=image.png&originHeight=169&originWidth=880&originalType=binary∶=1&size=14019&status=done&style=none&width=577)

- 静态 web 存在的缺点
  - Web 页面无法动态更新，所有用户看到的都是一个页面
    - 轮播图： 点击特效：伪动态
    - JavaScript: 实际开发中，它用的最多
    - VBScript
  - 它无法和数据库交互（数据无法持久化，用户无法交互）

## 动态 web

页面会动态显示：“web 的页面展示的效果因人而异”；
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597891092019-7ace5268-434a-46a1-b274-60841d2fb7b8.png#height=306&id=ft4Ib&name=image.png&originHeight=461&originWidth=898&originalType=binary∶=1&size=30949&status=done&style=none&width=597)
缺点：

- 加入服务器的动态 web 资源出现错误，我们需要重新编写我们的后台程序，重新发布;
  - 停机维护

优点：

- Web 页面可以动态更新，所有用户看到的都不是一个页面
- 它可以与数据库交互（数据持久化：注册，商品信息，用户信息....）

---

# web 服务器

---

## 技术详解

**ASP: **

- 微软：国内最早流行的就是 ASP
- 在 HTML 中嵌入了 VB 的脚本， ASP+COM；
- 维护成本高
- C#
- IIS

**PHP:**

- PHP 开发速度很快，功能强大，跨屏台，代码简单（70%）
- 无法承载大访问的情况（局限性）

**JSP/Servlet:**

- sun 公司主推的 B/S(浏览器/服务器)架构
- 基于 java 语言
- 可以承载三高（高并发、高可用、高性能）带来的问题

## web 服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户的一些响应信息；

- IIS
  - 微软的：ASP.. window 中自带的
- **Tomcat 服务器**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597891780413-6713d404-492c-45b5-9c0b-205720944987.png#height=69&id=vp9b8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=137&originWidth=515&originalType=binary∶=1&size=39259&status=done&style=none&width=257.5)
Tomcat 服务器是一个免费的开放源代码的 Web 应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试 JSP 程序的首选。对于一个初学者来说它是最佳的选择。Tomcat 实际上运行 JSP 页面和 Servlet。目前 Tomcat 最新版本为**9.0.37。**
学习：

- 下载 tomact
- 安装 or 解压
- 了解配置文件及目录结构
- 这个东西的作用
- Tomcat 目录结构详细介绍： [https://www.jb51.net/article/148995.htm](https://www.jb51.net/article/148995.htm)

---

# Tomcat

---

## 安装 Tomcat

- 官网：[https://tomcat.apache.org/](https://tomcat.apache.org/)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597892365854-907c7565-7a20-461f-aa43-b156477bddb0.png#height=390&id=FUmX5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=779&originWidth=1006&originalType=binary∶=1&size=182618&status=done&style=none&width=503)
下载之后，解压即可：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597892626662-b9577a4a-1758-4f43-88bf-f355536ef5bf.png#height=203&id=So75l&margin=%5Bobject%20Object%5D&name=image.png&originHeight=406&originWidth=850&originalType=binary∶=1&size=57458&status=done&style=none&width=425)

## 启动 Tomcat

测试：http://localhost:8080
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597896554797-ab01edb3-42a1-4078-b424-a4e14dd30eeb.png#height=316&id=OLtpq&margin=%5Bobject%20Object%5D&name=image.png&originHeight=632&originWidth=1226&originalType=binary∶=1&size=101711&status=done&style=none&width=613)

## 配置 Tomcat

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597897226644-1f0a95a0-3192-418e-9943-7c673c891ef1.png#height=277&id=YOjmZ&margin=%5Bobject%20Object%5D&name=image.png&originHeight=380&originWidth=780&originalType=binary∶=1&size=53513&status=done&style=none&width=569)
       ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597897243102-25508f64-267d-4da3-b986-8ae1b420b165.png#height=67&id=cf147&name=image.png&originHeight=123&originWidth=1064&originalType=binary∶=1&size=16246&status=done&style=none&width=580)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597897252056-144941e0-6423-4106-9734-2d143765709b.png#height=58&id=dRx81&name=image.png&originHeight=91&originWidth=950&originalType=binary∶=1&size=10749&status=done&style=none&width=603)
可以更改端口和域名，更改域名要修改 C:\Windows\System32\drivers\etc，一般不做更改。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597897408412-863677eb-262f-46ab-87f9-e205c0774b22.png#height=432&id=G5Wvo&margin=%5Bobject%20Object%5D&name=image.png&originHeight=672&originWidth=844&originalType=binary∶=1&size=75988&status=done&style=none&width=542)
**面试题：**
**请你谈谈网站是如何进行访问的？**

      - 输入域名
      - 检查本机的C:\Windows\System32\drivers\etc\hosts配置文件下有没有这个域名映射，
         - 1有：直接返回对应的ip地址，这个地址中，有我们需要访问的web程序，可以直接访问

**127.0.0.1 www.java.com**

         - 没有，去DNS服务器找，找到的话就返回，找不到就返回找不到。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597897885444-71f09346-5003-42c4-8282-b862e00965dd.png#height=162&id=zYOLv&name=image.png&originHeight=234&originWidth=813&originalType=binary∶=1&size=25678&status=done&style=none&width=562)

## 发布一个 web 网站

- 将自己写的网站，放到服务器中指定的 web 应用的文件夹（webapps）下，就可以访问了
- 网站应该有的结构

```javascript
webapps:Tomcat	服务器的web目录
	- ROOT
  - moban4838 网站的目录名
  	 - WEB-INF
		 		- classes: java程序
        - lib web应用程序依赖的jar包
        - web.xml 网站的配置文件
     - index.html 默认的首页
     - static
 				- css
				- js
        - img
		- ....
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597898727958-ef3d0cb6-30c8-4733-9712-368aadaf4724.png#height=248&id=fBPH1&name=image.png&originHeight=496&originWidth=1128&originalType=binary∶=1&size=60077&status=done&style=none&width=564)

---

# HTTP 协议

---

## 什么是 HTTP?

HTTP（超文本传输协议）是一个简单的请求-响应协议，它通常运行在 TCP 之上。

- 文本： html,字符串~ ...
- 超文本：图片，音乐， 视频， 定位， 地图
- 端口： 80
- HTTPS： 安全的 HTTP     443
- 两个版本：
  - **HTTP1.0：**客户端可以与 web 服务器连接后，只能获得一个 web 资源，断开连接
  - **HTTP1.1： **客户端可以与 web 服务器连接后，可以获得多个资源

## HTTP 请求

客户端 ----发起请求（request）---- 服务器

### 请求行

请求方式： GET/POST HEAD/DELEDE/PUT/TRACT

- **get：**请求能够携带的参数比较少，大小有限制，会在浏览器的 URL 栏显示数据内容，不安全，但是高效。
- **post:** 请求能够携带的参数限制，大小没有有限制，会在浏览器的 URL 栏不会显示数据内容，安全，但不高效。

### 消息头

- **Accept: ** 告诉浏览器，它所支持的数据类型
- **Accept-Encoding: **支持的编码格式 UTF-8 GBK GB2312
- **Accept-Language:** 语言环境
- **Cache-Control: ** 缓存控制
- **Connection: **请求完成是断开还是保持连接
- **Cookie: **缓存
- **Host: **主机号
- **Refresh:** 告诉客户端，多久刷新一次
- **Location：**让网页重新定位

```java
Request URL: https://www.baidu.com/   //请求地址
Request Method: GET								   //get方法和post方法
Status Code: 200 OK                  //状态码
Remote Address: 14.215.177.38:443    // 远程地址
Referrer Policy: no-referrer-when-downgrade
```

```java
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate, br
Accept-Language: zh,zh-CN;q=0.9
Cache-Control: max-age=0
Connection: keep-alive
Cookie: PSTM=1594296714; BAIDUID=19D8FA91D88C0C015880D299FF456E00:FG=1; BD_UPN=12314753; BIDUPSID=70F48A94E0D47DAD6C644E8FCB359A5D; hide_hotsearch=1; sug=3; ORIGIN=2; bdime=0; MCITY=-%3A; BDUSS=UcxcWNxS1ZkWlAxMWIySzJkRlJqS0NVaEphVFJrWjRLQ0FqYVJVc3hJMTloMWxmRVFBQUFBJCQAAAAAAAAAAAEAAABtMSV1x-XT8NrkxLAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAH36MV99-jFfZ; BDUSS_BFESS=UcxcWNxS1ZkWlAxMWIySzJkRlJqS0NVaEphVFJrWjRLQ0FqYVJVc3hJMTloMWxmRVFBQUFBJCQAAAAAAAAAAAEAAABtMSV1x-XT8NrkxLAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAH36MV99-jFfZ; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; BDRCVFR[feWj1Vr5u3D]=I67x6TjHwwYf0; delPer=0; BD_CK_SAM=1; PSINO=6; BD_HOME=1; ZD_ENTRY=baidu; H_PS_PSSID=1457_32569_32531_31660_32045_32117_31708_26350_32506; sugstore=0
Host: www.baidu.com
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36
```

## HTTP 响应

      服务器     -----响应请求(response)----  客户端

## 响应状态码

200： 请求响应成功 200
3xx： 请求重定向
4xx： 找不到资源 404
5xx: 服务器代码错误 500 502（网关错误）

```java
Cache-Control: private       		//缓存控制
Connection: keep-alive				//连接：
Content-Encoding: gzip				//编码
Content-Type: text/html;charset=utf-8  //类型
Date: Thu, 20 Aug 2020 05:32:58 GMT
Expires: Thu, 20 Aug 2020 05:32:58 GMT
Server: BWS/1.1
Set-Cookie: BDSVRTM=453; path=/
Set-Cookie: BD_HOME=1; path=/
Set-Cookie: H_PS_PSSID=1457_32569_32536_31660_32045_32117_31708_26350_32506; path=/; domain=.baidu.com
Strict-Transport-Security: max-age=172800
Traceid: 1597901578043997236210089786624016751666
Transfer-Encoding: chunked
X-Ua-Compatible: IE=Edge,chrome=1
```

**面试题：**当你的浏览器中地址栏，输入地址并回车的一瞬间到页面能够展示回来，经历了什么？

---

# Maven

---

**为什么要学习 Maven?**

- 在 javaweb 开发中，需要使用大量的 jar 包，需要手动导入。
- 如何能够让一个东西自动帮我们导入和配置这个 jar 包？
- 由此：Maven 诞生了。

## Maven 项目架构管理工具

- 目前： 用来方便导入 jar 包
- 核心思想： 约定大于配置
  - 有约束，不要去违反
- Maven 会规定好你该如何编写我们的 Java 代码

## 下载安装 Maven

** Maven 官网：**[http://maven.apache.org/download.cgi](http://maven.apache.org/download.cgi)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597903104743-d35e45ab-fa0a-474e-9641-b8e469a3590b.png#height=138&id=A3XKG&margin=%5Bobject%20Object%5D&name=image.png&originHeight=275&originWidth=1475&originalType=binary∶=1&size=49629&status=done&style=none&width=737.5)

## 配置环境变量

- M2_HOME maven 目录下的 bin 目录
- MAVEN_HOME maven 的目录

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597903824975-a0124bab-109b-4b57-b93b-1b0f08796e38.png#height=111&id=gKZKT&margin=%5Bobject%20Object%5D&name=image.png&originHeight=222&originWidth=1274&originalType=binary∶=1&size=48670&status=done&style=none&width=637)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597903833833-bf9189d7-28e6-4318-842f-29ed6963b48f.png#height=67&id=V0Day&name=image.png&originHeight=134&originWidth=928&originalType=binary∶=1&size=27733&status=done&style=none&width=464)

## 阿里云镜像

- **mirrors**
  - 加速我们的下载
  - 国内建议使用阿里云镜像
  - conf 下的 settings.xml 中的 mirror 下添加

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597904101978-a9aa1a0f-2a4d-45e6-901f-ef690140a9b2.png#height=92&id=RhQUh&margin=%5Bobject%20Object%5D&name=image.png&originHeight=184&originWidth=714&originalType=binary∶=1&size=18861&status=done&style=none&width=357)

```xml
<mirror>
		<id>nexus-aliyun</id>
		<mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
		<name>Nexus aliyun</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1597904682206-ac920606-3dbd-47a4-bac6-f807bbbbcbad.png#height=278&id=foVHr&margin=%5Bobject%20Object%5D&name=image.png&originHeight=556&originWidth=1394&originalType=binary∶=1&size=61444&status=done&style=none&width=697)

## 本地仓库的配置

- 建立仓库
  - 建立一个本地仓库： localRepository

```xml
<localRepository>D:\Study Software\java\Maven\apache-maven-3.6.3\maven-repo</localRepository>
```

## IDEA 中使用 Maven

[https://blog.csdn.net/czc9309/article/details/80304074](https://blog.csdn.net/czc9309/article/details/80304074)

## IDEA 中配置 Tomact

[https://blog.csdn.net/With_Her/article/details/89243777](https://blog.csdn.net/With_Her/article/details/89243777)

---

# Servlet

---

## Servlet 简介

- Servlet 是 sun 公司开发动态 web 的一门技术
- Sun 公司在这些 API 中提供了一个接口叫做 Servlet,如果你想开发一个 Servlet 程序，只需要完成两个小步骤
  - 编写一个类，实现 Servlet 接口
  - 把开发好的 java 类部署到 web 服务器中
- 把实现了 Servlet 接口的 java 程序，叫做 Servlet。

## HelloServlet

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598015129450-7bc07b07-b75a-4098-afcd-5db2e13ec517.png#height=182&id=U5cEg&margin=%5Bobject%20Object%5D&name=image.png&originHeight=219&originWidth=820&originalType=binary∶=1&size=23358&status=done&style=none&width=682)
**HelloServlet.java**

```java
package edu.cqupt.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        writer.print("<h1>Hello Servlet!</h1>");
        System.out.println("----------------------------------");
        System.out.println("The First Servlet is running.");
        System.out.println("-----------------------------------");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       doGet(req, resp);
    }
}

```

**配置 web.xml**

```java
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
    <servlet>
        <servlet-name>home</servlet-name>
        <servlet-class>edu.cqupt.servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>home</servlet-name>
        <url-pattern>/home</url-pattern>
    </servlet-mapping>
</web-app>
```

## Servlet 原理

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598014925069-41ac134a-c048-4737-bde1-c83f5c99f22c.png#height=403&id=lTlQL&name=image.png&originHeight=557&originWidth=1030&originalType=binary∶=1&size=58698&status=done&style=none&width=746)

## Mapping 问题

- 一个 Servlet 可以指定一个映射路径

```java
<servlet-mapping>
     <servlet-name>home</servlet-name>
     <url-pattern>/home</url-pattern>
</servlet-mapping>
```

- 一个 Servlet 可以指定多个映射路径

```java
<servlet-mapping>
     <servlet-name>home</servlet-name>
     <url-pattern>/home</url-pattern>
</servlet-mapping>
<servlet-mapping>
     <servlet-name>home</servlet-name>
     <url-pattern>/home2</url-pattern>
</servlet-mapping>
<servlet-mapping>
     <servlet-name>home</servlet-name>
     <url-pattern>/home3</url-pattern>
</servlet-mapping>
```

- 一个 Servlet 可以指定通用映射路径

```java
<servlet-mapping>
     <servlet-name>home</servlet-name>
     <url-pattern>/home/*</url-pattern>
</servlet-mapping>
```

- 一个 Servlet 可以指定默认映射路径（一般将默认的映射路径设置为 404 页面，找不到就走到默认路径）

```java
<servlet-mapping>
     <servlet-name>home</servlet-name>
     <url-pattern>/*</url-pattern>
</servlet-mapping>
```

- 一个 Servlet 可以指定一些后缀或者前缀等...

```java
<!-- *前面不能加任何项目映射-->
<servlet-mapping>
     <servlet-name>home</servlet-name>
     <url-pattern>*.qingjiang</url-pattern>
</servlet-mapping>
```

- 优先级
  - 指定固有的映射路径优先级最高，找不到，就会走默认的处理请求

## ServletContext

web 容器在启动的时候，它会为每个 web 程序都创建一个对应的 ServletContext 对象，它代表了当前的 web 应用；

### 共享数据

- 在一个 Servlet 中，可以在另一个 Servlet 中拿到，实现了 Servlet 之间的通信

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598019995087-0c7b4fb8-bbe1-4285-9c61-a0c243e330ef.png#height=239&id=FksLK&name=image.png&originHeight=320&originWidth=714&originalType=binary∶=1&size=24381&status=done&style=none&width=534)

```java
public class HelloServlet  extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Hello");
        //this.getInitParameter()   初始化参数
        //this.getServletConfig()   Servlet配置
        //this.getServletContext()  Servlet上下文
        ServletContext context1 = this.getServletContext();
        //将一个数据保存到 ServletContext中，名字为：username,值为username
        resp.setContentType("text/html");
        resp.setCharacterEncoding("UTF-8");
        String username = "shilin.z";
        context1.setAttribute("username",username);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```java
public class GetServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context2 = this.getServletContext();
        String username = (String) context2.getAttribute("username");

        resp.setContentType("text/html");
        resp.setCharacterEncoding("UTF-8");
        resp.getWriter().print("s1传输的内容: " + username);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```xml
		<servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>edu.cqupt.servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>getc</servlet-name>
        <servlet-class>edu.cqupt.servlet.GetServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>getc</servlet-name>
        <url-pattern>/getc</url-pattern>
    </servlet-mapping>
```

**测试结果：**
先 http://localhost:8080/s2/hello， 不然输出为 null
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598019443651-f926ef34-ff4b-44ca-8f54-3340d780fa3d.png#height=172&id=yVFnX&name=image.png&originHeight=182&originWidth=621&originalType=binary∶=1&size=17988&status=done&style=none&width=588)

### 获取初始化参数

```xml
		<!-- 配置一些web应用的初始化参数-->
    <context-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql://local:3306/mybatis</param-value>
    </context-param>

		<servlet>
        <servlet-name>getp</servlet-name>
        <servlet-class>edu.cqupt.servlet.Servlet03</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>getp</servlet-name>
        <url-pattern>/getp</url-pattern>
    </servlet-mapping>
```

```java
  @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext context = this.getServletContext();
        String url = context.getInitParameter("url");
        resp.getWriter().print(url);
    }
```

**测试结果：**
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598020843447-09f45609-06df-433f-962b-2c01b4e4d301.png#height=126&id=lw6RQ&name=image.png&originHeight=126&originWidth=431&originalType=binary∶=1&size=13050&status=done&style=none&width=431)

### 请求转发(不是重定向）

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598058018610-8189e3ad-4167-4694-b53d-de8887e1bbd3.png#height=307&id=dK4x2&name=image.png&originHeight=386&originWidth=642&originalType=binary∶=1&size=26946&status=done&style=none&width=511)

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("进入了Servlet04");
        ServletContext context = this.getServletContext();
        //RequestDispatcher requestDispatcher = context.getRequestDispatcher("/getp");    //转发的请求路径
        //requestDispatcher.forward(req,resp);    //调用forward实现请求转发
        context.getRequestDispatcher("/getp").forward(req,resp);
    }
```

```xml
	<servlet>
        <servlet-name>sd4</servlet-name>
        <servlet-class>edu.cqupt.servlet.Servlet04</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>sd4</servlet-name>
        <url-pattern>/sd4</url-pattern>
    </servlet-mapping>
```

**测试结果：**
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598021244992-0bf8f402-c734-44db-8fe2-362d4c7a81a2.png#height=107&id=KO1tH&margin=%5Bobject%20Object%5D&name=image.png&originHeight=123&originWidth=443&originalType=binary∶=1&size=12004&status=done&style=none&width=387)

---

### 读取资源文件

- Properties 类
  - 在 java 目录下新建 properties
  - 在 resources 目录下新建 properties
- 发现：都被打包到了同一路径下：classes，我们俗称路径为 classpath

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598060043386-2b4a7650-59ad-42a2-9bc3-6cf0c40cf19d.png#height=241&id=NSSYS&margin=%5Bobject%20Object%5D&name=image.png&originHeight=540&originWidth=483&originalType=binary∶=1&size=34790&status=done&style=none&width=216)
**需要在本项目的 pom.xml 中配置 resources**

```java
    <!-- 在bulid中配置resources，来防止我们资源导出失败的问题-->
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
```

需要一个文件流：
**Servlet05.java**

```java
public class Servlet05 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       	//获得文件流
        InputStream is = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");
        Properties prop = new Properties();
        prop.load(is);
        String user  = prop.getProperty("username");
        String pwd = prop.getProperty("password");
        resp.getWriter().print(user + ":" + pwd);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

**db.properties**

```java
username=root
password=123456
```

**web.xml**

```java
<servlet>
        <servlet-name>sd5</servlet-name>
        <servlet-class>edu.cqupt.servlet.Servlet05</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>sd5</servlet-name>
        <url-pattern>/sd5</url-pattern>
    </servlet-mapping>
```

**测试结果：**
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598060812023-6388fb4a-2f0b-42cc-9070-a88c1c0b82f6.png#height=67&id=cVgXl&margin=%5Bobject%20Object%5D&name=image.png&originHeight=134&originWidth=610&originalType=binary∶=1&size=13458&status=done&style=none&width=305)

## HttpServletResponse

web 服务器接收到客户端的 http 请求，针对这个请求，分别创建一个代表请求的 HttpServletRequest 对象，一个代表响应的 HttpServletResponse 对象；

- 如果要获取客户端请求过来的参数：找 HttpServletRequset 对象
- 如果要给客户端响应一些信息： 找 HttpServletResponse 对象

###

### 简单分类**​**

**负责向浏览器发送数据的方法**

```java
ServletOutputStream getOutputStream() throws IOException;
PrintWriter getWriter() throws IOException;
```

**​**

**负责向浏览器发送响应头的方法**

```java
void setCharacterEncoding(String var1);
void setContentLength(int var1);
void setContentLengthLong(long var1);
void setContentType(String var1);
void setDateHeader(String var1, long var2);
void addDateHeader(String var1, long var2);
void setHeader(String var1, String var2);
void addHeader(String var1, String var2);
void setIntHeader(String var1, int var2);
void addIntHeader(String var1, int var2);
```

**响应的状态码**

```java
int SC_CONTINUE = 100;
int SC_OK = 200;
int SC_MULTIPLE_CHOICES = 300;
int SC_NOT_FOUND = 404;
int SC_INTERNAL_SERVER_ERROR = 500;
.....
```

### 常见应用

#### 向浏览器输出消息

（resp.getwriter().print()）

#### 下载文件

- 获取下载文件的路径
- 下载的文件名
- 设置想办法让浏览器能够支持下载我们需要的东西
- 获取下载文件的输入流
- 创建缓冲区
- 获取 OutputStream 对象
- 将 FileOutputStream 流写入到 buffer 缓冲区
- 使用 OutputStream 将缓冲区中的数据输出到客户端

```java
public class FileServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1.获取下载文件的路径
        //String realPath = this.getServletContext().getRealPath("/dali.jpg");
        String realPath = "E:\\CodePlace\\Java\\idea\\狂神说Java\\Maven\\javawebMaven\\javaweb\\servlet-03-response\\target\\classes\\dali.jpg";
        System.out.println("下载文件的路径为：" + realPath);
        // 2.下载的文件名
        String filename = realPath.substring(realPath.lastIndexOf("\\") + 1);
        // 3.设置想办法让浏览器能够支持下载我们需要的东西
        resp.setHeader("Content-Disposition", "attachment; filename=" + URLEncoder.encode(filename,"UTF-8"));
        // 4.获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);
        // 5.创建缓冲区
        int len = 0;
        byte[] buffer = new byte[1024];
        // 6.获取OutputStream对象
        ServletOutputStream out = resp.getOutputStream();
        // 7.将FileOutputStream流写入到buffer缓冲区,使用OutputStream将缓冲区中的数据输出到客户端
        while((len=in.read(buffer))!=-1){
            out.write(buffer,0,len);
        }
        in.close();
        out.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598064943480-8bf05768-d1c9-41b9-a0f3-54641c1902f8.png#height=498&id=wbpJB&name=image.png&originHeight=995&originWidth=934&originalType=binary∶=1&size=60658&status=done&style=none&width=467)

#### 验证码功能

**验证怎么来的？**

- 前端实现
- 后端实现，需要用到 java 的图片类，生成一个图片

```java
public class ImageServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1. 如何让浏览器3秒自动刷新一次;
        resp.setHeader("refresh","3");
        //在内存中创建一个图片
        BufferedImage image = new BufferedImage(80, 20, BufferedImage.TYPE_3BYTE_BGR);
        //得到图片
        Graphics2D g = (Graphics2D)image.getGraphics();
        // 设置图片的背景颜色
        g.setColor(Color.white);
        g.fillRect(0,0,80,20);
        //给图片写数据
        g.setColor(Color.blue);
        g.setFont(new Font("宋体",Font.BOLD,20));
        g.drawString(makeNum(),0,20);

        // 告诉浏览器，这个请求用图片的方式打开
        resp.setContentType("image/jpeg");
        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires", -1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");

        // 把图片写给浏览器
        boolean write = ImageIO.write(image, "jpg",resp.getOutputStream());
    }
    //生产随机数
    private String makeNum(){
        Random random = new Random();
        String num = random.nextInt(9999999) + "";
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < 7 - num.length() ; i++) {   //保证生成的随机数只有7位
            sb.append("0");  //不足7位用0填充
        }
        num = sb.toString() + num;
        return num;
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```xml
    <servlet>
        <servlet-name>img</servlet-name>
        <servlet-class>edu.cqupt.servlet.ImageServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>img</servlet-name>
        <url-pattern>/img</url-pattern>
    </servlet-mapping>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598104251154-19554502-a91d-4590-8178-5447323d9028.png#height=242&id=mPybC&name=image.png&originHeight=630&originWidth=1162&originalType=binary∶=1&size=35334&status=done&style=none&width=446)

#### 实现重定向

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598104464139-c3655ec2-84f2-4e4a-ac2e-63332a8230b9.png#height=184&id=ZVauT&name=image.png&originHeight=241&originWidth=591&originalType=binary∶=1&size=13553&status=done&style=none&width=450)
B 一个 web 资源收到客户端请求后，他会通知客户端去访问另外一个 web 资源，这个过程叫重定向。
**  常见场景：**

- 用户登录：登录成功，跳转到另外的页面。

```java
public class RedirectServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

//        resp.setHeader("Location", "/s3/img");
//        resp.setStatus(302);
        resp.sendRedirect("/s3/img");
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

**面试题：请求和重定向的区别？**

- 相同点
  - 页面都会跳转
- 不同点
  - 请求转发，url 地址不会发生变化     307
  - 重定向，url 地址会发生变化 302

```java
public class RequestTestServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("进入这个请求了----");
        // 处理请求
        String username = req.getParameter("username");
        String pwd = req.getParameter("password");
        System.out.println(username + ":" + pwd);
        // 重定向一定要注意，路径问题，否则就会404
        resp.sendRedirect("/s3/home.jsp");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1> Success ! </h1>
</body>
</html>
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598109018004-9f4c5614-2577-406e-b654-590118e9e1f4.png#height=123&id=WnyYb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=245&originWidth=508&originalType=binary∶=1&size=19612&status=done&style=none&width=254)

## ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598109052674-9ab55ab9-4fc2-411e-baca-ede515212ef9.png#height=82&id=or6ZO&margin=%5Bobject%20Object%5D&name=image.png&originHeight=157&originWidth=617&originalType=binary∶=1&size=18148&status=done&style=none&width=321)

## HttpServletRequest

​

HttpServletRequest 代表客户端的请求，用户通过 Http 协议访问服务器，Http 请求中的所有信息会被封装到 HttpServletRequest，通过这个 HttpServletRequest 方法，获得客户端的所有信息。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598145771426-8402d74e-22a2-4e2b-b209-2a960acc6300.png#height=266&id=VaGcD&name=image.png&originHeight=421&originWidth=658&originalType=binary∶=1&size=63915&status=done&style=none&width=415)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598145794826-95125fb5-f6d3-4368-920f-b81f61c2dbe2.png#height=253&id=UmmGa&name=image.png&originHeight=411&originWidth=674&originalType=binary∶=1&size=63435&status=done&style=none&width=415)

### 获取前端传递的参数和请求转发

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598145966194-14c085af-39c1-4e96-86fe-111934655f75.png#height=88&id=JFRDI&margin=%5Bobject%20Object%5D&name=image.png&originHeight=176&originWidth=640&originalType=binary∶=1&size=30434&status=done&style=none&width=320)

```java
public class LoginServlet  extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setCharacterEncoding("utf-8");
        req.setCharacterEncoding("utf-8");
        //获取前端传递参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobbies = req.getParameterValues("hobbies");
        System.out.println("---------------");
        System.out.println(username);
        System.out.println(password);
        System.out.println(Arrays.toString(hobbies));
        System.out.println("---------------");

        //重定向
        // resp.sendRedirect("/s4/success.jsp");
        //通过请求转发
        System.out.println(req.getContextPath());
        //这里的/代表当前的web应用
        req.getRequestDispatcher("/success.jsp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

## Cookie、Session

### 会话

**会话：**用户打开一个浏览器，点击了很多 web 资源，访问多个 web 资源，关闭浏览器，这个过程就叫做会话。
**有状态会话：**客户端访问服务器，下次在访问服务器，服务器知晓客户端曾今访问过。
一个网站怎么证明用户访问过？
客户端 服务器

- 服务端给客户端一个**cookie**，客户端下次访问带上 cookie 就可以了
- 服务器通过**session**登记客户端访问过，下次客户端再次访问，服务器匹配客户端

### 保存会话的两种技术

**cookie（发票）**

- 客户端技术（响应、请求）

**session（登记）**

- 服务器技术：利用这个技术，可以保存用户的会话信息，我们可以把信息或者数据放在 Session 中

常见场景：

- 网站登录过后，下次不用登录，第二次访问直接就进去。

### cookie

- 从请求中拿到 cookie
- 服务器响应给客户端 cookie

```java
// 保存用户上一次访问的时间
public class CookieDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 服务器告诉你 ，你访问的时间，把这个时间封装成一个信件，下次访问的时候，需要带上信件
        req.setCharacterEncoding("gbk");
        resp.setCharacterEncoding("gbk");

        PrintWriter out = resp.getWriter();
        // 服务器端从客户端获取
        Cookie[] cookies = req.getCookies();  // cookie可能存在多个
        // 判断cookie是否存在
        if(cookies!=null){
            //如果存在
            out.write("你上次访问的时间是:");
            for (int i = 0; i <cookies.length ; i++) {
                Cookie cookie = cookies[i];
                // 获取cookie的名字
                if(cookie.getName().equals("lastLoginTime")){

                    long lastLoginTime = Long.parseLong(cookie.getValue());
                    Date date = new Date(lastLoginTime);
                    out.write(date.toLocaleString());
                }
                System.out.println(cookie.getName());
            }
        }else{
            out.write("这是您第一次访问本网站。");
        }
        //服务器给客户端响应一个Cookie
        Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis() + "");
        cookie.setMaxAge(24*60*60);
        resp.addCookie(cookie);

    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598151296347-8c4c3c75-b480-4c9a-bffe-ed656a8b3f4f.png#height=102&id=Zo7WF&name=image.png&originHeight=149&originWidth=634&originalType=binary∶=1&size=14689&status=done&style=none&width=436)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598151279143-ef1dafd5-86c4-47d7-851f-366deae3b178.png#height=143&id=SA9Y9&name=image.png&originHeight=241&originWidth=746&originalType=binary∶=1&size=17598&status=done&style=none&width=442)

### Session（重点）

## ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598196442356-fc2ae54a-34e9-4a0d-9f67-88da1d869d8c.png#height=388&id=h0YWp&margin=%5Bobject%20Object%5D&name=image.png&originHeight=478&originWidth=759&originalType=binary∶=1&size=32893&status=done&style=none&width=616)

**什么是 Session：**

- 服务器给每一个用户（浏览器）创建一个 Session 对象
- 一个 Session 独占一个浏览器，只要浏览器没有关闭，这个 Session 就存在
- 用户登录之后，整个网站都可以访问！
- 场景：保存用户的信息，保存购物车信息，在整个网站中经常会使用的数据，我们将它保存在 session 中

**​**

**Session 和 Cookie 的区别:**

- Cookie 是把用户的数据写给用户得浏览器，浏览器保存（可以保存多个）
- Session 把用户的数据写到用户独占的 Session 中，服务器端保存（保存重要的信息，减少服务器资源的浪费）
- Session 对象由服务器创建

**使用 Session**

```java
public class SessionDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 解决乱码问题
        resp.setHeader("content-type","text/html; charset=utf-8");
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        //得到session
        HttpSession session = req.getSession();
        //获取Session的ID
        String sessionId  = session.getId();
        //判断session是否为新创建的
        if(session.isNew()){
            resp.getWriter().write("session 创建成功。session ID:" + sessionId );
        }else{
            resp.getWriter().write("已经在服务器中存在了。session ID:" + sessionId );
        }

        // Session创建的时候做了什么事情
        //Cookie cookie = new Cookie("JSESSIONID",sessionId);
        //resp.addCookie(cookie);

        //给Session中存字符串
        session.setAttribute("name","shilin.z");
        String name = (String) session.getAttribute("name");
        System.out.println(name);

        //给Session中存用户信息
        session.setAttribute("name",new Person("shilin.z",20));
        Person person = (Person) session.getAttribute("name");
        System.out.println(person.toString());

        //手动注销Session: 刷新，会重新生成sessionID
        //session.removeAttribute("name");
        //session.invalidate();

        //自动注销：在xml中配置
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```xml
<!--    设置Session 默认的失效时间，以分钟为单位-->
<session-config>
<!-- 1分钟后失效 -->
<session-timeout>1</session-timeout>
</session-config>
```

## JSP

### 什么是 JSP？

- Java Server Page: java 服务器端页面，和 Servlet 一样，用于开发动态 web
- 最大的特点，就是写 JSP，就像是在写 HTML
- 区别：
  - HTML 只能给用户提供静态数据
  - JSP 页面中，可以嵌入 Java 代码，为用户提供动态数据

### JSP 原理

思路： JSP 到底如何执行的？

- 代码层面： target 里面和项目里面的 JSP 类似
- 服务器层面：

Tomcat 服务器的 work 目录
    ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598196843150-754658d5-c6de-4246-bef0-7482b0ce0e48.png#height=135&id=WDmXt&margin=%5Bobject%20Object%5D&name=image.png&originHeight=269&originWidth=1003&originalType=binary∶=1&size=21166&status=done&style=none&width=501.5)
IDEA 工作空间
    ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598196911904-767ef6f1-46c4-45e7-aa1c-771fbd4cea6c.png#height=141&id=XMm4Y&margin=%5Bobject%20Object%5D&name=image.png&originHeight=282&originWidth=716&originalType=binary∶=1&size=35235&status=done&style=none&width=358)
页面转变为了 Java 程序：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598197216180-750bfc69-2088-4daa-bf4c-7d2fffcd40cb.png#height=73&id=eR5qD&margin=%5Bobject%20Object%5D&name=image.png&originHeight=145&originWidth=1339&originalType=binary∶=1&size=22264&status=done&style=none&width=669.5)
浏览器向服务器发送请求，不管访问什么资源，其实都是在访问 Servlet!
**JSP 本质上就是一个 Servlet:**

```java
// 初始化
public void _jspInit() {
}
// 销毁
public void _jspDestroy() {
}
//JSPService
public void _jspService(HttpServletRequest request, HttpServletResponse response)
```

1. 判断请求
1. 内置一些对象

```java
final javax.servlet.jsp.PageContext pageContext;   // 页面上下文
javax.servlet.http.HttpSession session = null;	   //session
final javax.servlet.ServletContext application;	   //applocationContext
final javax.servlet.ServletConfig config;          //config
javax.servlet.jsp.JspWriter out = null;            // out
final java.lang.Object page = this;                // page： 当前
javax.servlet.jsp.JspWriter _jspx_out = null;
javax.servlet.jsp.PageContext _jspx_page_context = null;
HttpServletRequest request		// 请求
HttpServletResponse response	// 响应
```

3. 输出页面前设置的代码

```java
response.setContentType("text/html");		//设置响应的页面类型

pageContext = _jspxFactory.getPageContext(this, request, response,null, true, 8192, true);
_jspx_page_context = pageContext;

application = pageContext.getServletContext();

config = pageContext.getServletConfig();

session = pageContext.getSession();

out = pageContext.getOut();
_jspx_out = out;

out.write("\r\n");
out.write("<html>\r\n");
out.write("<head>\r\n");
  ......
out.write("</html>\r\n");
// 在JSP中，只要是Java代码，就会原封不动的输出out.print(name);
// 如果是HTML代码就会被转换为out.write("<html>\r\n"); 这样的格式输出到前端
```

以上的这些对象我们可以在 JSP 的页面中直接使用。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598199502141-28e02361-8725-467e-a5ef-22900761d017.png#height=360&id=Kixgr&margin=%5Bobject%20Object%5D&name=image.png&originHeight=475&originWidth=796&originalType=binary∶=1&size=36106&status=done&style=none&width=603)

### JSP 基础语法

任何语言都有自己的语法，Java 中有，JSP 作为 java 技术的一种应用，它拥有一些自己扩充的语法，Java 所有语法都支持。
**JSP 表达式**

```java

<%--JSP 表达式
作用： 用来将程序的输出，输出到客户端
<%= 变量或者表达式%>
--%>

<%=new java.util.Date()%>
```

**JSP 脚本片段**

```java
<%
    int sum = 0;
    for (int i = 0; i <= 100; i++) {
        sum += i;
    }
    out.println("<h2>Sum=" + sum + "</h2>");
%>
```

**JSP 脚本片段的再实现**

```java
<%
    int x = 10;
    out.println(x);
%>
<p>这是一个JSP文档</p>
<%
    int y = 2;
    out.println(x);
    out.println(y);
%>
```

**在 JSP 代码中嵌入 HTML**

```java
<%
    for (int i = 0; i < 5 ; i++) {
%>
    <h1>Hello World! <%=i%> </h1>
<%
    }
%>
```

**JSP 声明**

```java
<%!
    static {
        System.out.println("Loading Servlet!");
    }
    private int globalVar = 0;
    public void study(){
        System.out.println("进入了方法。。。");
    }
%>
```

**JSP 声明：**会被编译到 JSP 生成的 Java 类中，其他的就会被生成到\_jspService 方法中
**​**

**EL 表达式${}**

```java
<%for (int i = 0; i < 5 ; i++) {%>
    <h1>Hello World! ${i} </h1>
<%}%>
```

此外：JSP 的注释不会在 HTML 的源代码中显示。

### JSP 指令

```java
<%@ page errorPage="/error/500.jsp" %>

<%@ include file="common/header.jsp"%>
<h1>我是body1</h1>
<%@ include file="common/footer.jsp"%>

<hr>
<jsp:include page="/common/header.jsp"/>
<h1>我是body2</h1>
<jsp:include page="/common/footer.jsp"/>
```

### 9 大内置对象

- **PageContext        存数据**
- **Request                存数据**
- **Response**
- **Session                                     存数据**
- **Application(ServletContext) 存数据**
- **Config(ServletConfig)**
- **out**
- **page**
- **Exception**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598249804087-cf0976fa-0b10-42ba-8fa2-d65297416506.png#height=387&id=rniGN&margin=%5Bobject%20Object%5D&name=image.png&originHeight=546&originWidth=713&originalType=binary∶=1&size=35114&status=done&style=none&width=506)

```java
<%--内置对象--%>
<%
	//保存的数据只在一个页面中有效
    pageContext.setAttribute("name1","shilin.z-1");
	//保存的数据只在一次请求中有效，请求转发携带数据
    request.setAttribute("name2","shilin.z-2");
	//保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器
    session.setAttribute("name3","shilin.z-3");
	//保存的数据只要服务器中有效，从打开服务器到关闭服务器
    application.setAttribute("name4","shilin.z-4");

%>
<%--通过pageContext 取出保存的值--%>
<%
    //
    String name1 = (String) pageContext.findAttribute("name1");
    String name2 = (String) pageContext.findAttribute("name2");
    String name3 = (String) pageContext.findAttribute("name3");
    String name4 = (String) pageContext.findAttribute("name4");
%>
<%--使用EL表达式输出${}--%>
<h1>取出的值为：</h1>
<h3>${name1}</h3>
<h3>${name2}</h3>
<h3>${name3}</h3>
<h3>${name4}</h3>
```

request: 客户端向服务器发送请求，产生的数据，用户看完了就没用了，比如：新闻
session：**客户端向服务器发送请求，产生的数据，用户看完了一会还有用，比如：购物车**
application: **客户端向服务器发送请求，产生的数据，一个用户用完了，其它用户还能用，比如：聊天数据**
**​**

### JSP 标签、JSTL 标签、EL 表达

```xml
  		<!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl-api -->
        <!--  JSTL表达式 依赖 -->
        <dependency>
            <groupId>javax.servlet.jsp.jstl</groupId>
            <artifactId>jstl-api</artifactId>
            <version>1.2</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/taglibs/standard -->
        <!-- Standard标签库 依赖 -->
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>
```

**EL 表达式：** ${}

- 获取数据
- 执行运算
- 获取 web 开发的常用对象

**JSP 标签**

```java
<jsp:include page="/common/header.jsp"/>
<jsp:forward page="/jsptag02.jsp">
    <jsp:param name="name" value="shilin"/>
    <jsp:param name="age" value="20"/>
</jsp:forward>
```

**JSTL 标签**

- JSTL 标签库的使用就是为了弥补 HTML 标签的不足；它自定义许多标签，可以供我们使用，标签的功能和 java 代码一样。

[https://www.runoob.com/jsp/jsp-jstl.html](https://www.runoob.com/jsp/jsp-jstl.html)

- 核心标签：核心标签是最常用的 JSTL 标签。引用核心标签库的语法如下：

` <%@ taglib prefix="c" uri="``[http://java.sun.com/jsp/jstl/core](http://java.sun.com/jsp/jstl/core)``" %> `
    ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598251283981-2256c802-773d-4a6e-94a9-f7f389c787ab.png#height=446&id=xYQLs&name=image.png&originHeight=724&originWidth=1048&originalType=binary∶=1&size=85165&status=done&style=none&width=645)

- JSTL 标签使用步骤：
  - 引入对应的 taglib
  - 使用其中的方法
  - 在 Tomcat 中也需要引入 jstl 的包， 否则会报错误：JSTL 解析错误

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h4>if测试</h4>
    <form action="coreif.jsp" method="get">
        <%--EL表达式获取表单中的数据 ${param.参数名}--%>
        名字：<input type="text" name="username" value="${param.username}">
        <input type="submit" value="登录">
    </form>

    <c:if test="${param.username == 'admin'}" var="isAdmin" >
        <c:out value="管理员欢迎您。。"/>
    </c:if>
    <c:out value="${isAdmin}"/>

    <c:set var="score" value="85"></c:set>
    <c:choose>
        <c:when test="${socre>90}">
            你的成绩为优秀
        </c:when>
        <c:when test="${socre>80}">
            你的成绩为一般
        </c:when>
        <c:when test="${socre>60}">
            你的成绩为及格
        </c:when>
        <c:when test="${socre<60}">
            你的成绩为不及格
        </c:when>
    </c:choose>

   <%
        ArrayList<String> people = new ArrayList<>();
        people.add(0,"张三");
        people.add(1,"李四");
        people.add(2,"王五");
        people.add(3,"赵六");
        people.add(4,"田七");
        request.setAttribute("list",people);
    %>
    <%--
        var: 每次遍历处理的变量
        items: 要遍历的对象
    --%>
    <c:forEach var="people" items="${list}">
        <c:out value="${people}"/><br>
    </c:forEach>
    <c:forEach var="people" items="${list}" begin="1" end="4" step="2">
        <c:out value="${people}"/><br>
    </c:forEach>
</body>
</html>
```

### JavaBean

- 实体类
- JavaBean 有特定的写法
  - 必须要有一个无参构造
  - 属性必须私有化
  - 必须有对应的 get/set 方法
  - 一般用来和数据库字段做映射 ORM。
  - ORM（对象关系映射） - 表 -- 类 - 字段 -- 属性 - 行记录 -- 对象
    | **id** | **name** | **age** | **address** |
    | --- | --- | --- | --- |
    | 1 | 张三 | 2 | 重庆 |
    | 2 | 李四 | 32 | 北京 |
    | 3 | 王五 | 45 | 上海 |

```java
class People{
	private int id;
    private String name;
    private int age;
    private String address;
}

class A{
	new People(1,"张三",2,"重庆");
    new People(2,"李四",32,"北京");
    new People(3,"王五",45,"上海");
}
```

# MVC 三层架构

什么是 MVC： ** Model     view     Controller   模型、视图、控制器**

## 早些年

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598259921486-0960b7a8-3b23-4ed4-b8be-55d27ed5e5dd.png#height=230&id=yLLzF&margin=%5Bobject%20Object%5D&name=image.png&originHeight=382&originWidth=1080&originalType=binary∶=1&size=33786&status=done&style=none&width=651)
用户直接访问控制层，控制层就可以直接操作数据库；

```java
servlet--CRUD-->数据库
弊端：程序十分臃肿，不利于维护
servlet的代码中：处理请求、响应、视图跳转、处理JDBC、处理业务代码、处理逻辑代码

架构：没有什么是加一层解决不了的！
程序猿调用
|
JDBC
|
Mysql Oracle SqlServer ....
```

## MVC 三层架构![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598260645899-65a9720b-653c-4425-a6ff-a73fb58f7aa7.png#height=339&id=sqdGj&name=image.png&originHeight=565&originWidth=1214&originalType=binary∶=1&size=73854&status=done&style=none&width=728)

**Model**

- 业务处理 ：业务逻辑（Service）
- 数据持久层：CRUD   （Dao）

**View**

- 展示数据
- 提供链接发起 Servlet 请求 （a，form，img…）

**Controller  （Servlet）**

- 接收用户的请求 ：（req：请求参数、Session 信息….）
- 交给业务层处理对应的代码
- 控制视图的跳转

```java
登录--->接收用户的登录请求--->处理用户的请求（获取用户登录的参数，username，password）---->交给业务层处理登录业务（判断用户名密码是否正确：事务）--->Dao层查询用户名和密码是否正确-->数据库
```

# Filter （重点）

Filter：过滤器 ，用来过滤网站的数据；

- 处理中文乱码
- 登录验证….

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598266945246-e9609476-9fbc-4a45-82cf-68db6f23fe41.png#height=196&id=iVJOd&name=image.png&originHeight=316&originWidth=888&originalType=binary∶=1&size=26564&status=done&style=none&width=552)
Filter 开发步骤：

1. 导包
1. 编写过滤器
   1. 导包不要错
      ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598267622591-6ed0aadc-8020-442c-b91d-2f0f9d48bea5.png#height=192&id=Ljdif&name=image.png&originHeight=383&originWidth=1159&originalType=binary∶=1&size=120931&status=done&style=none&width=579.5)
      实现 Filter 接口，重写对应的方法即可

```java
public class CharacterEncodingFilter implements Filter {

    //初始化：web服务器启动，就以及初始化了，随时等待过滤对象出现！
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("CharacterEncodingFilter初始化");
    }

    //Chain : 链
    /*
    1. 过滤中的所有代码，在过滤特定请求的时候都会执行
    2. 必须要让过滤器继续同行
        chain.doFilter(request,response);
     */
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        response.setContentType("text/html;charset=UTF-8");

        System.out.println("CharacterEncodingFilter执行前....");
        chain.doFilter(request,response); //让我们的请求继续走，如果不写，程序到这里就被拦截停止！
        System.out.println("CharacterEncodingFilter执行后....");
    }

    //销毁：web服务器关闭的时候，过滤会销毁
    public void destroy() {
        System.out.println("CharacterEncodingFilter销毁");
    }
}
```

3. 在 web.xml 中配置 Filter

```xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>com.kuang.filter.CharacterEncodingFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <!--只要是 /servlet的任何请求，会经过这个过滤器-->
    <url-pattern>/servlet/*</url-pattern>
    <!--<url-pattern>/*</url-pattern>-->
</filter-mapping>
```

# 监听器

实现一个监听器的接口；（有 N 种）

1. 编写一个监听器
   实现监听器的接口…

```java
//统计网站在线人数 ： 统计session
public class OnlineCountListener implements HttpSessionListener {

    //创建session监听： 看你的一举一动
    //一旦创建Session就会触发一次这个事件！
    public void sessionCreated(HttpSessionEvent se) {
        ServletContext ctx = se.getSession().getServletContext();

        System.out.println(se.getSession().getId());

        Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");

        if (onlineCount==null){
            onlineCount = new Integer(1);
        }else {
            int count = onlineCount.intValue();
            onlineCount = new Integer(count+1);
        }

        ctx.setAttribute("OnlineCount",onlineCount);

    }

    //销毁session监听
    //一旦销毁Session就会触发一次这个事件！
    public void sessionDestroyed(HttpSessionEvent se) {
        ServletContext ctx = se.getSession().getServletContext();

        Integer onlineCount = (Integer) ctx.getAttribute("OnlineCount");

        if (onlineCount==null){
            onlineCount = new Integer(0);
        }else {
            int count = onlineCount.intValue();
            onlineCount = new Integer(count-1);
        }
        ctx.setAttribute("OnlineCount",onlineCount);
    }
    /*
    Session销毁：
    1. 手动销毁  getSession().invalidate();
    2. 自动销毁
     */
}
```

2. web.xml 中注册监听器

```xml
<!--注册监听器-->
<listener>
    <listener-class>com.kuang.listener.OnlineCountListener</listener-class>
</listener>
```

3. 看情况是否使用！

# 过滤器、监听器常见应用

**监听器：GUI 编程中经常使用；**

```java
public class TestPanel {
    public static void main(String[] args) {
        Frame frame = new Frame("中秋节快乐");  //新建一个窗体
        Panel panel = new Panel(null); //面板
        frame.setLayout(null); //设置窗体的布局

        frame.setBounds(300,300,500,500);
        frame.setBackground(new Color(0,0,255)); //设置背景颜色

        panel.setBounds(50,50,300,300);
        panel.setBackground(new Color(0,255,0)); //设置背景颜色

        frame.add(panel);
        frame.setVisible(true);

        //监听事件，监听关闭事件
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                super.windowClosing(e);
            }
        });
    }
}
```

**监听器：登录监听**
用户登录之后才能进入主页！用户注销后就不能进入主页了！

1.  用户登录之后，向 Sesison 中放入用户的数据
1.  进入主页的时候要判断用户是否已经登录；
1.  要求：在过滤器中实现！
1.

```java
HttpServletRequest request = (HttpServletRequest) req;
HttpServletResponse response = (HttpServletResponse) resp;

if (request.getSession().getAttribute(Constant.USER_SESSION)==null){
    response.sendRedirect("/error.jsp");
}

chain.doFilter(request,response);
```

# JDBC

什么是 JDBC ： Java 连接数据库！
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598318662059-e38798fc-6ff4-4b82-92fe-ebd896b71f43.png#height=396&id=tHsAk&name=image.png&originHeight=514&originWidth=545&originalType=binary∶=1&size=22022&status=done&style=none&width=420)
需要 jar 包的支持：

- java.sql
- javax.sql
- mysql-conneter-java…   连接驱动（必须要导入）

**实验环境搭建**

```sql
CREATE TABLE users(
    id INT PRIMARY KEY,
    `name` VARCHAR(40),
    `password` VARCHAR(40),
    email VARCHAR(60),
    birthday DATE
);

INSERT INTO users(id,`name`,`password`,email,birthday)
VALUES(1,'张三','123456','zs@qq.com','2000-01-01');
INSERT INTO users(id,`name`,`password`,email,birthday)
VALUES(2,'李四','123456','ls@qq.com','2000-01-01');
INSERT INTO users(id,`name`,`password`,email,birthday)
VALUES(3,'王五','123456','ww@qq.com','2000-01-01');


SELECT	* FROM users;
```

导入数据库依赖

```xml
<!--mysql的驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>
```

IDEA 中连接数据库：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598321442535-750731c3-519b-4a89-a6c4-175664c38e74.png#height=235&id=tNT8k&name=image.png&originHeight=334&originWidth=393&originalType=binary∶=1&size=20979&status=done&style=none&width=277)

**JDBC 固定步骤：**

1. 加载驱动
1. 连接数据库,代表数据库
1. 向数据库发送 SQL 的对象 Statement : CRUD
1. 编写 SQL （根据业务，不同的 SQL）
1. 执行 SQL
1. 关闭连接

```java
public class TestJdbc {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //配置信息
        //useUnicode=true&characterEncoding=utf-8 解决中文乱码
        String url="jdbc:mysql://localhost:3306/jdbc?useUnicode=true&characterEncoding=utf-8";
        String username = "root";
        String password = "123456";

        //1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.连接数据库,代表数据库
        Connection connection = DriverManager.getConnection(url, username, password);

        //3.向数据库发送SQL的对象Statement,PreparedStatement : CRUD
        Statement statement = connection.createStatement();

        //4.编写SQL
        String sql = "select * from users";

        //5.执行查询SQL，返回一个 ResultSet  ： 结果集
        ResultSet rs = statement.executeQuery(sql);

        while (rs.next()){
            System.out.println("id="+rs.getObject("id"));
            System.out.println("name="+rs.getObject("name"));
            System.out.println("password="+rs.getObject("password"));
            System.out.println("email="+rs.getObject("email"));
            System.out.println("birthday="+rs.getObject("birthday"));
        }

        //6.关闭连接，释放资源（一定要做） 先开后关
        rs.close();
        statement.close();
        connection.close();
    }
}
```

**预编译 SQL**

```java
public class TestJDBC2 {
    public static void main(String[] args) throws Exception {
        //配置信息
        //useUnicode=true&characterEncoding=utf-8 解决中文乱码
        String url="jdbc:mysql://localhost:3306/jdbc?useUnicode=true&characterEncoding=utf-8";
        String username = "root";
        String password = "123456";

        //1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.连接数据库,代表数据库
        Connection connection = DriverManager.getConnection(url, username, password);

        //3.编写SQL
        String sql = "insert into  users(id, name, password, email, birthday) values (?,?,?,?,?);";

        //4.预编译
        PreparedStatement preparedStatement = connection.prepareStatement(sql);

        preparedStatement.setInt(1,2);//给第一个占位符？ 的值赋值为1；
        preparedStatement.setString(2,"狂神说Java");//给第二个占位符？ 的值赋值为狂神说Java；
        preparedStatement.setString(3,"123456");//给第三个占位符？ 的值赋值为123456；
        preparedStatement.setString(4,"24736743@qq.com");//给第四个占位符？ 的值赋值为1；
        preparedStatement.setDate(5,new Date(new java.util.Date().getTime()));//给第五个占位符？ 的值赋值为new Date(new java.util.Date().getTime())；

        //5.执行SQL
        int i = preparedStatement.executeUpdate();

        if (i>0){
            System.out.println("插入成功@");
        }

        //6.关闭连接，释放资源（一定要做） 先开后关
        preparedStatement.close();
        connection.close();
    }
}
```

**事务**

要么都成功，要么都失败！
ACID 原则：保证数据的安全。

```java
开启事务
事务提交  commit()
事务回滚  rollback()
关闭事务

转账：
A:1000
B:1000

A(900)   --100-->   B(1100)
```

**Junit 单元测试**
依赖

```xml
<!--单元测试-->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

简单使用
@Test 注解只有在方法上有效，只要加了这个注解的方法，就可以直接运行！

```java
@Test
public void test(){
    System.out.println("Hello");
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598322547933-a4a2fadf-fb05-4d5e-86a1-9fa7d0462773.png#height=54&id=o6PZo&margin=%5Bobject%20Object%5D&name=image.png&originHeight=108&originWidth=959&originalType=binary∶=1&size=11680&status=done&style=none&width=479.5)

失败的时候是红色：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598322566108-db32e72f-7ed4-4a1d-b9fd-53b539f5b6ea.png#height=57&id=i6HQH&margin=%5Bobject%20Object%5D&name=image.png&originHeight=113&originWidth=934&originalType=binary∶=1&size=15027&status=done&style=none&width=467)

**搭建一个环境**

```sql
CREATE TABLE account(
   id INT PRIMARY KEY AUTO_INCREMENT,
   `name` VARCHAR(40),
   money FLOAT
);

INSERT INTO account(`name`,money) VALUES('A',1000);
INSERT INTO account(`name`,money) VALUES('B',1000);
INSERT INTO account(`name`,money) VALUES('C',1000);
```

```java
    @Test
    public void test() {
        //配置信息
        //useUnicode=true&characterEncoding=utf-8 解决中文乱码
        String url="jdbc:mysql://localhost:3306/jdbc?useUnicode=true&characterEncoding=utf-8";
        String username = "root";
        String password = "123456";

        Connection connection = null;

        //1.加载驱动
        try {
            Class.forName("com.mysql.jdbc.Driver");
            //2.连接数据库,代表数据库
             connection = DriverManager.getConnection(url, username, password);

            //3.通知数据库开启事务,false 开启
            connection.setAutoCommit(false);

            String sql = "update account set money = money-100 where name = 'A'";
            connection.prepareStatement(sql).executeUpdate();

            //制造错误
            //int i = 1/0;

            String sql2 = "update account set money = money+100 where name = 'B'";
            connection.prepareStatement(sql2).executeUpdate();

            connection.commit();//以上两条SQL都执行成功了，就提交事务！
            System.out.println("success");
        } catch (Exception e) {
            try {
                //如果出现异常，就通知数据库回滚事务
                connection.rollback();
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            e.printStackTrace();
        }finally {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
```

# 上传文件

## 文件上传注意事项

- 1.为保证服务器安全，上传文件应该放在外界无法直接访问你得目录下，比如放在 WEB-INF 目录下
- 2.为防止文件覆盖现象的发生，要为文件产生一个唯一的文件名 （添加时间戳 或者 uuid 或者 MD5 或者位运算）
- 3.要限制上传文件的大小
- 4.可以限制上传文件的类型，在收到上传文件名时，要判断后缀名是否合格。

## 需要用到的类详解

- ServletFileUpload 负责处理上传的文件数据，并将表单中每个输入项封装成一个 FileItem 对象，在使用 ServletFileUpload 对象解析请求时，需要 DiskFileItemFactory 对象。所以，我们需要在进行解析工作前构造号 DiskFileItemFactory 对象，通过 ServletFileUpload 对象的构造方法，或 setFileItemFactory()方法设置 ServletFileUpload 对象的 fileItemFactory 属性。

​

```java
package edu.cqupt.servlet;

import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileUploadException;
import org.apache.commons.fileupload.ProgressListener;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.util.List;
import java.util.UUID;

public class FileServlet  extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        if(!ServletFileUpload.isMultipartContent(req)){ //判断文件是带文件表单还是普通表单
            return;    //终止运行，说明这一定是一个不带文件的
        }
        //为保证服务器安全，上传文件应该放在外界无法直接访问你得目录下，比如放在WEB-INF目录下
        String uploadPath = this.getServletContext().getRealPath("/WEB-INF/upload");
        File uploadFile = new File(uploadPath);
        if(!uploadFile.exists()){
            uploadFile.mkdir();
        }

        // 缓存
        String tempPath = this.getServletContext().getRealPath("/WEB-INF/temp");
        File file = new File(tempPath);
        if(!file.exists()){
            file.mkdir();
        }
        String msg = null;
        try {
        //1.创建 DiskFileItemFactory
        DiskFileItemFactory factory = getDiskFileItemFactory(file);
        //2.获取ServletFileUpload
        ServletFileUpload upload = getServletFileUpload(factory);
        // 3.处理上传文件
         msg = uploadParseRequest(upload,req,uploadPath);
        } catch (FileUploadException e) {
            e.printStackTrace();
        }
        // 请求转发消息
        req.setAttribute("msg",msg);
        req.getRequestDispatcher("info.jsp").forward(req,resp);
    }

    public static DiskFileItemFactory getDiskFileItemFactory(File file){
        DiskFileItemFactory factory = new DiskFileItemFactory();
        factory.setSizeThreshold(1024*1024);    // 缓存区大小为1M
        factory.setRepository(file);            // 临时目录的保存目录，需要一个file
        return  factory;
    }
    public static ServletFileUpload getServletFileUpload(DiskFileItemFactory factory){
        ServletFileUpload upload = new ServletFileUpload(factory);
        upload.setProgressListener(new ProgressListener() {
            @Override
            public void update(long pBytesRead, long pContenLength, int pItems) {
                System.out.println("总大小：" + pContenLength + "已上传：" + pBytesRead );
            }
        });
        upload.setHeaderEncoding("UTF-8");
        upload.setFileSizeMax(1024*1024*10);
        upload.setSizeMax(1024*1024*10);
        return upload;
    }

    public static String  uploadParseRequest(ServletFileUpload upload,HttpServletRequest req,String uploadPath) throws FileUploadException, IOException {
        String message = null;
        List<FileItem> fileItems = upload.parseRequest(req); // 把前端请求解析，封装成一个FileItem对象
        for (FileItem fileItem : fileItems) {
            if (fileItem.isFormField()) {     // 普通表单
                String name = fileItem.getName();
                String value = fileItem.getString("utf-8");
                System.out.println(name + ":" + value);
            } else {  //判断是文件表单
                String uploadFileName = fileItem.getName(); // ===== 处理文件 =============
                if (uploadFileName.trim().equals("") || uploadFileName == null) {
                    continue;
                }
                String fileName = uploadFileName.substring(uploadFileName.lastIndexOf("/") + 1);
                String fileExtName = uploadFileName.substring(uploadFileName.lastIndexOf(".") + 1);
                // UUID.randomUUID() 随机生成一个唯一识别的通用码
                // 网络中传输东西，都需要序列化
                // POJO, 实体类， 如果想要生成在多个电脑上运行， 传输-->需要把对象都序列化
                // JNI java native Interface
                // implements Serializable :标记接口 ，JVM --> Java栈 本地方法栈 native --> c++
                String uuidPath = UUID.randomUUID().toString();// 可以 使用UUID(唯一识别的通用码),保证文件唯一
                String realPath = uploadPath + "/" + uuidPath; // ========= 存放地址 ========
                File realPathFile = new File(realPath);
                if (!realPathFile.exists()) {
                    realPathFile.mkdir();
                }
                InputStream is = fileItem.getInputStream(); // ========= 文件传输 ========
                FileOutputStream fos = new FileOutputStream(realPath + "/" + fileName);
                byte[] buffer = new byte[1024];
                int len = 0;
                while ((len = is.read(buffer)) != -1) {
                    fos.write(buffer, 0, len);
                }
                fos.close();
                is.close();
                message = "文件上传成功";
                fileItem.delete();  //上传成功，清除临时文件
            }
        }
        return message;
    }
}
```

# 发送邮件

加载包**pom.xml**

```xml
   <!-- https://mvnrepository.com/artifact/javax.mail/mail -->
    <dependency>
      <groupId>javax.mail</groupId>
      <artifactId>mail</artifactId>
      <version>1.4.7</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/javax.activation/activation -->
    <dependency>
      <groupId>javax.activation</groupId>
      <artifactId>activation</artifactId>
      <version>1.1.1</version>
    </dependency>
```

注册页面** index.jsp**

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>注册</title>
</head>
<body>
    <form action="${pageContext.request.contextPath}/RegisterServlet.do" method="post">
        <div class="txt txt0">
            <span style="letter-spacing:8px;">用户名:</span>
            <input name="username" type="text" class="txtphone" placeholder="请输入用户名"/>
        </div>
        <div class="txt txt0">
            <span style="letter-spacing:4px;">登录密码:</span>
            <input name="password" type="text" class="txtphone" placeholder="请输入密码"/>
        </div>
        <div class="txt txt0">
            <span style="letter-spacing:4px;">邮箱:</span>
            <input name="email" type="text" class="txtphone" placeholder="请输入邮箱"/>
        </div>
        <div class="txt txt0">
            <input type="submit" value="注册"/>
        </div>
    </form>
</body>
</html>
```

注册成功跳转页面：**info.jsp**

```java
<%--
  Created by IntelliJ IDEA.
  User: ASUS
  Date: 2020/8/29
  Time: 22:30
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    ${message}
</body>
</html>
```

用户实体类**User.java**

```java
package edu.cqupt.pojo;

import java.io.Serializable;
public class User implements Serializable {

    private String username;
    private String password;
    private String email;

    public User() {
    }

    public User(String username, String password, String email) {
        this.username = username;
        this.password = password;
        this.email = email;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}
```

**RegisterServlet.java**

```java
package edu.cqupt.servlet;

import edu.cqupt.pojo.User;
import edu.cqupt.utils.SendEmail;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class RegisterServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String email = req.getParameter("email");
        System.out.println(username);
        System.out.println(password);
        System.out.println(email);
        User user = new User(username, password, email);
        SendEmail send = new SendEmail(user);
        send.start();		// 使用线程，加快邮件发送
        req.setAttribute("message","注册成功，请查收邮件");
        req.getRequestDispatcher("info.jsp").forward(req,resp);

    }
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

**SendEmail.java**

```java
package edu.cqupt.utils;

import com.sun.mail.util.MailSSLSocketFactory;
import edu.cqupt.pojo.User;

import javax.mail.*;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.util.Properties;

public class SendEmail extends Thread{
    private String from = "发送方@qq.com";
    private String username = "发送方@qq.com";
    private String password = "邮箱授权码";
    private String host = "smtp.qq.com";
    private  User user;

    public SendEmail(User user) {
        this.user = user;
    }

    @Override
    public void run() {
        try {
            Properties prop = new Properties();
            prop.setProperty("mail.host", "smtp.qq.com");
            prop.setProperty("mail.transport.protocol", "smtp");
            prop.setProperty("mail.smtp.auth", "true");

            //QQ邮箱，设置SSL加密
            MailSSLSocketFactory sf = new MailSSLSocketFactory();
            sf.setTrustAllHosts(true);
            prop.put("mail.smtp.ssl.enable", "true");
            prop.put("mail.smtp.socketFactory", sf);

            // 使用JavaMail发送邮件的5个步骤
            // 1、创建session
            Session session = Session.getInstance(prop, new Authenticator() {
                @Override
                protected PasswordAuthentication getPasswordAuthentication() {
                    return new PasswordAuthentication(username, password);
                }
            });
            // 2. 开启Session的debug模式:true
            session.setDebug(false);
            // 3.通过session得到transport对象
            Transport ts = session.getTransport();
            ts.connect(host, username, password);
            // 4. 创建邮件
            Message message = new MimeMessage(session);
            message.setFrom(new InternetAddress(from));
            message.setRecipient(Message.RecipientType.TO, new InternetAddress(user.getEmail()));
            message.setSubject("注册邮件");
            String info = "Yours username:" + user.getUsername() + "password:" + user.getPassword();
            message.setContent(info, "text/html;chartset=UTF-8");
            // 5.发送邮件
            ts.sendMessage(message, message.getAllRecipients());
            // 6.关闭连接
            ts.close();
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

**  注册效果**
  ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1598834799823-56aadf4f-dd2e-479a-b344-e67e370418cf.png#align=left&display=inline&height=149&margin=%5Bobject%20Object%5D&name=image.png&originHeight=297&originWidth=705&size=32186&status=done&style=none&width=352.5#height=297&id=o6J1q&originHeight=297&originWidth=705&originalType=binary∶=1&status=done&style=none&width=705)


