---
title: 29、SpringSecurity权限控制
urlname: truqx6
date: '2021-07-09 20:44:33 +0800'
tags: []
categories: []
---

# 1、简介

在 Web 开发中，安全一直是非常重要的一个方面。安全虽然属于应用的非功能性需求，但是应该在应用开发的初期就考虑进来。如果在应用开发的后期才考虑安全的问题，就可能陷入一个两难的境地：一方 面，应用存在严重的安全漏洞，无法满足用户的要求，并可能造成用户的隐私数据被攻击者窃取；另一 方面，应用的基本架构已经确定，要修复安全漏洞，可能需要对系统的架构做出比较重大的调整，因而 需要更多的开发时间，影响应用的发布进程。因此，从应用开发的第一天就应该把安全相关的因素考虑 进来，并在整个应用的开发过程中。
市面上存在比较有名的：Shiro，Spring Security ！
这里需要阐述一下的是，每一个框架的出现都是为了解决某一问题而产生了，那么 Spring Security 框架的出现是为了解决什么问题呢？
首先我们看下它的官网介绍：[Spring Security 官网地址](https://spring.io/projects/spring-security#overview)
Spring Security is a powerful and highly customizable authentication and access-control framework. It is the de-facto standard for securing Spring-based applications.
Spring Security is a framework that focuses on providing both authentication and authorization to Java applications. Like all Spring projects, the real power of Spring Security is found in how easily it can be extended to meet custom requirements
Spring Security 是一个功能强大且高度可定制的身份验证和访问控制框架。它实际上是保护基于 spring
的应用程序的标准。
Spring Security 是一个框架，侧重于为 Java 应用程序提供身份验证和授权。与所有 Spring 项目一样，
Spring 安全性的真正强大之处在于它可以轻松地扩展以满足定制需求
从官网的介绍中可以知道这是一个权限框架。想我们之前做项目是没有使用框架是怎么控制权限的？对 于权限 一般会细分为功能权限，访问权限，和菜单权限。代码会写的非常的繁琐，冗余。
怎么解决之前写权限代码繁琐，冗余的问题，一些主流框架就应运而生而 Spring Scecurity 就是其中的一种。
Spring 是一个非常流行和成功的 Java 应用开发框架。Spring Security 基于 Spring 框架，提供了一套
Web 应用安全性的完整解决方案。一般来说，Web 应用的安全性包括用户认证（Authentication）和用户授权（Authorization）两个部分。用户认证指的是验证某个用户是否为系统中的合法主体，也就是说用户能否访问该系统。用户认证一般要求用户提供用户名和密码。系统通过校验用户名和密码来完成认 证过程。用户授权指的是验证某个用户是否有权限执行某个操作。在一个系统中，不同用户所具有的权 限是不同的。比如对一个文件来说，有的用户只能进行读取，而有的用户可以进行修改。一般来说，系 统会为不同的用户分配不同的角色，而每个角色则对应一系列的权限。
对于上面提到的两种应用情景，Spring Security 框架都有很好的支持。在用户认证方面，Spring
Security 框架支持主流的认证方式，包括 HTTP 基本认证、HTTP 表单验证、HTTP 摘要认证、OpenID 和 LDAP 等。在用户授权方面，Spring Security 提供了基于角色的访问控制和访问控制列表（Access Control List，ACL），可以对应用中的领域对象进行细粒度的控制。

# 2、实战测试

## 1、实验环境搭建

1. 新建一个初始的 springboot 项目 ，

web 模块
thymeleaf 模块

1. 导入静态资源

1. welcome.html
1. |views
1. |level1
1. 1.html
1. 2.html
1. 3.html
1. |level2
1. 1.html
1. 2.html
1. 3.html
1. |level3
1. 1.html
1. 2.html
1. 3.html
1. Login.html

1. controller 跳转！

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
package com.kuang.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable; import org.springframework.web.bind.annotation.RequestMapping;
@Controller
public class RouterController {
@RequestMapping({"/","/index"}) public String index(){
return "index";
}

@RequestMapping("/toLogin") public String toLogin(){
return "views/login";
}

@RequestMapping("/level1/{id}")
public String level1(@PathVariable("id") int id){ return "views/level1/"+id;
}

@RequestMapping("/level2/{id}")
public String level2(@PathVariable("id") int id){ return "views/level2/"+id;
}

@RequestMapping("/level3/{id}")
public String level3(@PathVariable("id") int id){ return "views/level3/"+id;
}
}

1. 测试实验环境是否 OK！

## 2、认识 SpringSecurity

Spring Security 是针对 Spring 项目的安全框架，也是 Spring Boot 底层安全模块默认的技术选型，他可
以实现强大的 Web 安全控制，对于安全控制，我们仅需要引入 模
spring-boot-starter-security
块，进行少量的配置，即可实现强大的安全管理！
记住几个类：
WebSecurityConﬁgurerAdapter： 自定义 Security 策略
AuthenticationManagerBuilder：自定义认证策略
@EnableWebSecurity：开启 WebSecurity 模式
Spring Security 的两个主要目标是 “认证” 和 “授权”（访问控制）。

### “认证”（Authentication）

身份验证是关于验证您的凭据，如用户名/用户 ID 和密码，以验证您的身份。身份验证通常通过用户名和密码完成，有时与身份验证因素结合使用。

### “授权” （Authorization）

授权发生在系统成功验证您的身份后，最终会授予您访问资源（如信息，文件，数据库，资金，位置， 几乎任何内容）的完全权限。
这个概念是通用的，而不是只在 Spring Security 中存在。

## 3、认证和授权

目前，我们的测试环境，是谁都可以访问的，我们使用

增加上认证和授权的功能
Spring Security

1、引入 模块
Spring Security

1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-starter-security</artifactId>
1. </dependency>

2、编写 配置类
Spring Security
参考官网：[https://spring.io/projects/spring-security ](https://spring.io/projects/spring-security)查看我们自己项目中的版本，找到对应的帮助文档：
[https://docs.spring.io/spring-security/site/docs/5.3.0.RELEASE/reference/html5](https://docs.spring.io/spring-security/site/docs/5.3.0.RELEASE/reference/html5) #servlet- applications 8.16.4

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834674344-eb82c734-c4af-4222-b49f-d60260354b64.jpeg#)

3、编写基础配置类

1 package com.kuang.config; 2
3

4
5
import org.springframework.security.config.annotation.web.builders.HttpSecurity; import org.springframework.security.config.annotation.web.configuration.EnableWebSe curity;
import org.springframework.security.config.annotation.web.configuration.WebSecurity
ConfigurerAdapter;
6
7
8
9
10
11
12
13
14
@EnableWebSecurity // 开启 WebSecurity 模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {
@Override
protected void configure(HttpSecurity http) throws Exception {

}
}

4、定制请求的授权规则

1. @Override
1. protected void configure(HttpSecurity http) throws Exception {
1. // 定制请求的授权规则
1. // 首页所有人可以访问
1. http.authorizeRequests().antMatchers("/").permitAll()
1. .antMatchers("/level1/\*\*").hasRole("vip1")
1. .antMatchers("/level2/\*\*").hasRole("vip2")
1. .antMatchers("/level3/\*\*").hasRole("vip3"); 9 }

5、测试一下：发现除了首页都进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有 对应的权限才可以！
6、在 方法中加入以下配置，开启自动配置的登录功能！
configure()

| 1   | // 开启自动配置的登录功能                |
| --- | ---------------------------------------- |
| 2   | // /login 请求来到登录页                 |
| 3   | // /login?error 重定向到这里表示登录失败 |
| 4   | http.formLogin();                        |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834674766-9e6abf15-c5c9-4f49-8871-47ddfec569ce.jpeg#)7、测试一下：发现，没有权限的时候，会跳转到登录的页面！
8、查看刚才登录页的注释信息；
我们可以定义认证规则，重写 方法
configure(AuthenticationManagerBuilder auth)

1. //定义认证规则
1. @Override
1. protected void configure(AuthenticationManagerBuilder auth) throws Exception

{
4

1. //在内存中定义，也可以在 jdbc 中去拿....
1. auth.inMemoryAuthentication()
1. .withUser("kuangshen").password("123456").roles("vip2","vip3") 8 .and()
1. .withUser("root").password("123456").roles("vip1","vip2","vip3")
1. .and()
1. .withUser("guest").password("123456").roles("vip1","vip2"); 12

}

9、测试，我们可以使用这些账号登录进行测试！发现会报错！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834675293-69d94f03-9d73-494c-a38e-319ad4f5ef4f.jpeg#)
There is no PasswordEncoder mapped for the id “null”

10、原因，我们要将前端传过来的密码进行某种方式加密，否则就无法登录，修改代码

1. //定义认证规则
1. @Override
1. protected void configure(AuthenticationManagerBuilder auth) throws Exception

{

1.  //在内存中定义，也可以在 jdbc 中去拿....
1.  //Spring security 5.0 中新增了多种加密方式，也改变了密码的格式。
1.      //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密  码进行某种方式加密
1.  //spring security 官方推荐的是使用 bcrypt 加密方式。

8

9 auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())

1.      .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
1.  .and()
1.      .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
1.  .and()
1.      .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");

15 }

11、测试，发现，登录成功，并且每个角色只能访问自己认证下的规则！搞定

## 4、权限控制和注销

1. 开启自动配置的注销的功能

1. //定制请求的授权规则
1. @Override
1. protected void configure(HttpSecurity http) throws Exception { 4 //....
1. //开启自动配置的注销的功能
1. // /logout 注销请求
1. http.logout(); 8 }

1. 我们在前端，增加一个注销的按钮， 导航栏中

index.html

1. <a class="item" th:href="@{/logout}">
1. <i class="address card icon"></i> 注销

3 </a>

1. 我们可以去测试一下，登录成功后点击注销，发现注销完毕会跳转到登录页面！
1. 但是，我们想让他注销成功后，依旧可以跳转到首页，该怎么处理呢？

1. // .logoutSuccessUrl("/"); 注销成功来到首页
1. http.logout().logoutSuccessUrl("/");

1. 测试，注销完毕后，发现跳转到首页 OK
1. 我们现在又来一个需求：用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏 可以显示登录的用户信息及注销按钮！还有就是，比如 kuangshen 这个用户，它只有 vip2，vip3 功能，那么登录则只显示这两个功能，而 vip1 的功能菜单不显示！这个就是真实的网站情况了！该如 何做呢？

我们需要结合 thymeleaf 中的一些功能
来显示不同的页面
sec：authorize="isAuthenticated()":是否认证登录！
Maven 依赖：

1. <!-- [https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-](https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-) extras-springsecurity4 -->
1. <dependency>
1. <groupId>org.thymeleaf.extras</groupId>
1. <artifactId>thymeleaf-extras-springsecurity5</artifactId>
1. <version>3.0.4.RELEASE</version>
1. </dependency>

1. 修改我们的 前端页面
   1. 导入命名空间

1 [xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5](http://www.thymeleaf.org/thymeleaf-extras-springsecurity5)"

1.  修改导航栏，增加认证判断

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

<!--登录注销-->
<div class="right menu">
<!--如果未登录-->
<div  sec:authorize="!isAuthenticated()">
<a class="item" th:href="@{/login}">
<i class="address card icon"></i> 登录
</a>
</div>
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
<!--如果已登录-->
<div sec:authorize="isAuthenticated()">
<a class="item">
<i class="address card icon"></i>
用户名： <span sec:authentication="principal.username">
</span>
角 色 ：<span sec:authentication="principal.authorities">
</span>
</a>
</div>
<div sec:authorize="isAuthenticated()">
<a class="item" th:href="@{/logout}">
<i class="address card icon"></i> 注销
</a>
</div>
</div>

1. 重启测试，我们可以登录试试看，登录成功后确实，显示了我们想要的页面；
1. 如果注销 404 了，就是因为它默认防止 csrf 跨站请求伪造，因为会产生安全问题，我们可以将请求改为 post 表单提交，或者在 spring security 中关闭 csrf 功能；我们试试：在 配置中增加

http.csrf().disable();

1. http.csrf().disable();//关闭 csrf 功能:跨站请求伪造,默认只能通过 post 方式提交 logout

请求

1. http.logout().logoutSuccessUrl("/");

1. 我们继续将下面的角色功能块认证完成！

1. <!-- sec:authorize="hasRole('vip1')" -->
1. <div class="column" sec:authorize="hasRole('vip1')">

1. <div class="ui raised segment">
1. <div class="ui">
1. <div class="content">
1. <h5 class="content">Level 1</h5>
1. <hr>
1. <div><a th:href="@{/level1/1}"><i class="bullhorn icon">

</i> Level-1-1</a></div>

1. <div><a th:href="@{/level1/2}"><i class="bullhorn icon">

</i> Level-1-2</a></div>

1. <div><a th:href="@{/level1/3}"><i class="bullhorn icon">

</i> Level-1-3</a></div>

1. </div>
1. </div>
1. </div>
1. </div> 15
1. <div class="column" sec:authorize="hasRole('vip2')">
1. <div class="ui raised segment">
1. <div class="ui">
1. <div class="content">
1. <h5 class="content">Level 2</h5>
1. <hr>
1. <div><a th:href="@{/level2/1}"><i class="bullhorn icon">

</i> Level-2-1</a></div>

1. <div><a th:href="@{/level2/2}"><i class="bullhorn icon">

</i> Level-2-2</a></div>

1. <div><a th:href="@{/level2/3}"><i class="bullhorn icon">

</i> Level-2-3</a></div>

1. </div>
1. </div>
1. </div>
1. </div> 29
1. <div class="column" sec:authorize="hasRole('vip3')">
1. <div class="ui raised segment">
1. <div class="ui">
1. <div class="content">
1. <h5 class="content">Level 3</h5>
1. <hr>
1. <div><a th:href="@{/level3/1}"><i class="bullhorn icon">

</i> Level-3-1</a></div>

1. <div><a th:href="@{/level3/2}"><i class="bullhorn icon">

</i> Level-3-2</a></div>

1. <div><a th:href="@{/level3/3}"><i class="bullhorn icon">

</i> Level-3-3</a></div>

1. </div>
1. </div>
1. </div>
1. </div>

1. 测试一下！
1. 权限控制和注销搞定！

## 5、记住我

现在的情况，我们只要登录之后，关闭浏览器，再登录，就会让我们重新登录，但是很多网站的情况， 就是有一个记住密码的功能，这个该如何实现呢？很简单

1. 开启记住我功能

1. //定制请求的授权规则
1. @Override
1. protected void configure(HttpSecurity http) throws Exception {
1. //。。。。。。。。。。。
1. //记住我
1. http.rememberMe(); 7 }

1. 我们再次启动项目测试一下，发现登录页多了一个记住我功能，我们登录之后关闭 浏览器，然后重新打开浏览器访问，发现用户依旧存在！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834675880-b48c1b5d-2ae2-4ceb-be76-0de6bb2c986d.jpeg#)思考：如何实现的呢？其实非常简单我们可以查看浏览器的 cookie

1. 我们点击注销的时候，可以发现，spring security 帮我们自动删除了这个 cookie

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834676340-82ba8a5c-ce54-434b-bdba-05813052e8a9.jpeg#)

1. 结论：登录成功后，将 cookie 发送给浏览器保存，以后登录带上这个 cookie，只要通过检查就可以 免登录了。如果点击注销，则会删除这个 cookie，具体的原理我们在 JavaWeb 阶段都讲过了，这里 就不在多说了！

## 6、定制登录页

现在这个登录页面都是 spring security 默认的，怎么样可以使用我们自己写的 Login 界面呢？

1. 在刚才的登录页配置后面指定 loginpage

1 http.formLogin().loginPage("/toLogin");

1. 然后前端也需要指向我们自己定义的 login 请求

1. <a class="item" th:href="@{/toLogin}">
1. <i class="address card icon"></i> 登录

3 </a>

1. 我们登录，需要将这些信息发送到哪里，我们也需要配置，login.html 配置提交请求及方式，方式必须为 post:

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834676809-60265f02-aef6-40f9-bb87-d55f7ac3d827.png#)在 loginPage()源码中的注释上有写明：

1. <form th:action="@{/login}" method="post">
1. <div class="field">
1. <label>Username</label>
1. <div class="ui left icon input">
1. <input type="text" placeholder="Username" name="username">
1. <i class="user icon"></i> 7 </div>
1. </div>
1. <div class="field">
1. <label>Password</label>
1. <div class="ui left icon input">
1. <input type="password" name="password">
1. <i class="lock icon"></i>
1. </div>
1. </div>
1. <input type="submit" class="ui blue submit button"/>
1. </form>

1. 这个请求提交上来，我们还需要验证处理，怎么做呢？我们可以查看码！我们配置接收登录的用户名和密码的参数！

formLogin()
方法的源

1. http.formLogin()
1. .usernameParameter("username")
1. .passwordParameter("password")
1. .loginPage("/toLogin")
1. .loginProcessingUrl("/login"); // 登陆表单提交请求

1. 在登录页增加记住我的多选框

| 1   | <input | type="checkbox" | name="remember"> | 记住我 |
| --- | ------ | --------------- | ---------------- | ------ |

1. 后端验证处理！

1. //定制记住我的参数！
1. http.rememberMe().rememberMeParameter("remember");

1. 测试，OK

# 3、完整配置代码

1 package com.kuang.config; 2

1. import org.springframework.security.config.annotation.authentication.builders.Authe nticationManagerBuilder;
1. import org.springframework.security.config.annotation.web.builders.HttpSecurity;
1. import org.springframework.security.config.annotation.web.configuration.EnableWebSe curity;
1. import org.springframework.security.config.annotation.web.configuration.WebSecurity ConfigurerAdapter;
1. import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder; 8

9 @EnableWebSecurity
10 public class SecurityConfig extends WebSecurityConfigurerAdapter { 11

1. //定制请求的授权规则
1. @Override
1. protected void configure(HttpSecurity http) throws Exception { 15
1. http.authorizeRequests().antMatchers("/").permitAll()
1. .antMatchers("/level1/\*\*").hasRole("vip1")
1. .antMatchers("/level2/\*\*").hasRole("vip2")
1. .antMatchers("/level3/\*\*").hasRole("vip3"); 20

21

1. //开启自动配置的登录功能：如果没有权限，就会跳转到登录页面！
1. // /login 请求来到登录页
1. // /login?error 重定向到这里表示登录失败
1. http.formLogin()
1. .usernameParameter("username")
1. .passwordParameter("password")
1. .loginPage("/toLogin")
1. .loginProcessingUrl("/login"); // 登陆表单提交请求

30

1. //开启自动配置的注销的功能
1. // /logout 注销请求
1. // .logoutSuccessUrl("/"); 注销成功来到首页

34

1. http.csrf().disable();//关闭 csrf 功能:跨站请求伪造,默认只能通过 post 方式提交

logout 请求

1. http.logout().logoutSuccessUrl("/"); 37
1. //记住我
1. http.rememberMe().rememberMeParameter("remember"); 40 }

41

1.  //定义认证规则
1.  @Override
1.      protected void configure(AuthenticationManagerBuilder auth) throws Exception {
1.  //在内存中定义，也可以在 jdbc 中去拿....
1.  //Spring security 5.0 中新增了多种加密方式，也改变了密码的格式。
1.      //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过  来的密码进行某种方式加密
1.  //spring security 官方推荐的是使用 bcrypt 加密方式。

49

1.      auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
1.      .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
1.  .and()
1.      .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
1.  .and()
1.      .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");

56 }
57 }
