---
title: 28、SpringBootWeb开发提升(Swagger)
urlname: cfsf3w
date: '2021-07-09 20:44:18 +0800'
tags: []
categories: []
---

# 集成 Swagger

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834660603-3a098f26-66df-4d7d-84db-5eddd48c05db.jpeg#)

### 学习目标：

了解 Swagger 的概念及作用
掌握在项目中集成 Swagger 自动生成 API 文档

**1、Swagger 简介**

### 前后端分离

前端 -> 前端控制层、视图层
后端 -> 后端控制层、服务层、数据访问层前后端通过 API 进行交互
前后端相对独立且松耦合

### 产生的问题

前后端集成，前端或者后端无法做到“及时协商，尽早解决”，最终导致问题集中爆发

### 解决方案

首先定义 schema [ 计划的提纲 ]，并实时跟踪最新的 API，降低集成风险

### Swagger

号称世界上最流行的 API 框架
Restful Api 文档在线自动生成器 => **API 文档 与 API 定义同步更新**
直接运行，在线测试 API
支持多种语言 （如：Java，PHP 等） 官网：[https://swagger.io/](https://swagger.io/)

## 2、SpringBoot 集成

**SpringBoot 集成 Swagger **=> **springfox**，两个 jar 包

### Springfox-swagger2

swagger-springmvc

### 使用 Swagger

要求：jdk 1.8 + 否则 swagger2 无法运行步骤：

1. 新建一个 SpringBoot-web 项目
1. 添加 Maven 依赖

<!-- [https://mvnrepository.com/artifact/io.springfox/springfox-swagger2](https://mvnrepository.com/artifact/io.springfox/springfox-swagger2)
-->
<dependency>
<groupId>io.springfox</groupId>
<artifactId>springfox-swagger2</artifactId>
<version>2.9.2</version>
</dependency>
<!-- [https://mvnrepository.com/artifact/io.springfox/springfox-swagger-](https://mvnrepository.com/artifact/io.springfox/springfox-swagger-) ui -->
<dependency>
<groupId>io.springfox</groupId>
<artifactId>springfox-swagger-ui</artifactId>
<version>2.9.2</version>
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

1. 编写 HelloController，测试确保运行成功！
1. 要使用 Swagger，我们需要编写一个配置类-SwaggerConﬁg 来配置 Swagger

| 1   | @Configuration //配置类                    |
| --- | ------------------------------------------ |
| 2   | @EnableSwagger2// 开启 Swagger2 的自动配置 |
| 3   | public class SwaggerConfig {               |
| 4   | }                                          |

1. 访问测试 ：http://localhost:8080/swagger-ui.html ，可以看到 swagger 的界面；

## 3、配置 Swagger

1. Swagger 实例 Bean 是 Docket，所以通过配置 Docket 实例来配置 Swaggger。

@Bean //配置 docket 以配置 Swagger 具体参数 public Docket docket() {
return new Docket(DocumentationType.SWAGGER_2);
}
1
2
3
4

1.  可以通过 apiInfo()属性配置文档信息

1.  //配置文档信息
1.  private ApiInfo apiInfo() {
1.      Contact contact = new Contact("联系人名字", "[http://xxx.xxx.com/联系人](http://xxx.xxx.com/联系人)访问链接", "联系人邮箱");
1.  return new ApiInfo(
1.  "Swagger 学习", // 标题
1.  "学习演示如何配置 Swagger", // 描述

7 "v1.0", // 版本

| 8   |     |     | "[http://terms.service.url/组织链接](http://terms.service.url/组织链接)", // 组织链接 |
| --- | --- | --- | ------------------------------------------------------------------------------------- |
| 9   |     |     | contact, // 联系人信息                                                                |
| 10  |     |     | "Apach 2.0 许可", // 许可                                                             |
| 11  |     |     | "许可链接", // 许可连接                                                               |
| 12  |     |     | new ArrayList<>()// 扩展                                                              |
| 13  |     | );  |                                                                                       |
| 14  | }   |     |                                                                                       |

1. Docket 实例关联上 apiInfo()

@Bean
public Docket docket() {
return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
}
1
2
3
4

1. 重启项目，访问测试 http://localhost:8080/swagger-ui.html 看下效果；

## 4、配置扫描接口

1. 构建 Docket 时通过 select()方法配置怎么扫描接口。

| 1   | @Bean                                                                     |
| --- | ------------------------------------------------------------------------- |
| 2   | public Docket docket() {                                                  |
| 3   | return new Docket(DocumentationType.SWAGGER_2)                            |
| 4   | .apiInfo(apiInfo())                                                       |
| 5   | .select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors     |
|     | 配置如何扫描接口                                                          |
| 6   |                                                                           |
|     | .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller") |
|     | )                                                                         |
| 7   | .build();                                                                 |
| 8   | }                                                                         |

1. 重启项目测试，由于我们配置根据包的路径扫描接口，所以我们只能看到一个类
1. 除了通过包路径配置扫描接口外，还可以通过配置其他方式扫描接口，这里注释一下所有的配置方 式：

| 1   | any() // 扫描所有，项目中的所有接口都会被扫描到                                   |
| --- | --------------------------------------------------------------------------------- |
| 2   | none() // 不扫描接口                                                              |
| 3   | // 通过方法上的注解扫描，如 withMethodAnnotation(GetMapping.class)只扫描 get 请求 |
| 4   | withMethodAnnotation(final Class<? extends Annotation> annotation)                |
| 5   | // 通过类上的注解扫描，如.withClassAnnotation(Controller.class)只扫描有           |
|     | controller 注解的类中的接口                                                       |
| 6   | withClassAnnotation(final Class<? extends Annotation> annotation)                 |
| 7   | basePackage(final String basePackage) // 根据包路径扫描接口                       |

1. 除此之外，我们还可以配置接口扫描过滤：
   | 1 | @Bean |
   | --- | --- |
   | 2 | public Docket docket() { |
   | 3 | return new Docket(DocumentationType.SWAGGER_2) |
   | 4 | .apiInfo(apiInfo()) |
   | 5 | .select()// 通过.select()方法，去配置扫描接 |
   | | 口,RequestHandlerSelectors 配置如何扫描接口 |
   | 6 | |
   | | .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller" |
   | | )) |
   | 7 | // 配置如何通过 path 过滤,即这里只扫描请求以/kuang 开头的接口 |
   | 8 | .paths(PathSelectors.ant("/kuang/\*\*")) |
   | 9 | .build(); |
   | 10 | } |

1. 这里的可选值还有

| 1   | any() // 任何请求都扫描                             |
| --- | --------------------------------------------------- |
| 2   | none() // 任何请求都不扫描                          |
| 3   | regex(final String pathRegex) // 通过正则表达式控制 |
| 4   | ant(final String antPattern) // 通过 ant()控制      |

## 5、配置开关 Swagger

1. 通过 enable()方法配置是否启用 swagger，如果是 false，swagger 将不能在浏览器中访问了

| 1   | @Bean                                                                    |
| --- | ------------------------------------------------------------------------ |
| 2   | public Docket docket() {                                                 |
| 3   | return new Docket(DocumentationType.SWAGGER_2)                           |
| 4   | .apiInfo(apiInfo())                                                      |
| 5   | .enable(false) //配置是否启用 Swagger，如果是 false，在浏览器将无法访问  |
| 6   | .select()// 通过.select()方法，去配置扫描接                              |
|     | 口,RequestHandlerSelectors 配置如何扫描接口                              |
| 7   |                                                                          |
|     | .apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller" |
|     | ))                                                                       |
| 8   | // 配置如何通过 path 过滤,即这里只扫描请求以/kuang 开头的接口            |
| 9   | .paths(PathSelectors.ant("/kuang/\*\*"))                                 |
| 10  | .build();                                                                |
| 11  | }                                                                        |

1. 如何动态配置当项目处于 test、dev 环境时显示 swagger，处于 prod 时不显示？

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
@Bean
public Docket docket(Environment environment) {
// 设置要显示 swagger 的环境
Profiles of = Profiles.of("dev", "test");
// 判断当前是否处于该环境
// 通过 enable() 接收此参数判断是否要显示
boolean b = environment.acceptsProfiles(of);
return new Docket(DocumentationType.SWAGGER_2)
.apiInfo(apiInfo())
.enable(b) //配置是否启用 Swagger，如果是 false，在浏览器将无法访问
.select()// 通过.select()方法，去配置扫描接口,RequestHandlerSelectors 配置如何扫描接口

13
.apis(RequestHandlerSelectors.basePackage("com.kuang.swagger.controller"
))

1. // 配置如何通过 path 过滤,即这里只扫描请求以/kuang 开头的接口
1. .paths(PathSelectors.ant("/kuang/\*\*"))
1. .build(); 17 }

1. 可以在项目中增加一个 dev 的配置文件查看效果！

## 6、配置 API 分组

1. 如果没有配置分组，默认是 default。通过 groupName()方法即可配置分组：

@Bean
public Docket docket(Environment environment) {
return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
.groupName("hello") // 配置分组
// 省略配置....
}
1
2
3
4
5
6

1. 重启项目查看分组
1. 如何配置多个分组？配置多个分组只需要配置多个 docket 即可：

@Bean
public Docket docket1() {
return new Docket(DocumentationType.SWAGGER_2).groupName("group1");
}
@Bean
public Docket docket2() {
return new Docket(DocumentationType.SWAGGER_2).groupName("group2");
}
@Bean
public Docket docket3() {
return new Docket(DocumentationType.SWAGGER_2).groupName("group3");
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

1. 重启项目查看

## 7、实体配置

1. 新建一个实体类

@ApiModel("用户实体") public class User {
@ApiModelProperty("用户名") public String username; @ApiModelProperty("密码") public String password;
}
1
2
3
4
5
6
7

1. 只要这个实体在**请求接口**的返回值上（即使是泛型），都能映射到实体项中：

@RequestMapping("/getUser") public User getUser(){
return new User();
}
1
2
3
4

1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834660941-1a422a42-1195-4e6d-b9d5-40e563951de8.png#)重启查看测试

注：并不是因为@ApiModel 这个注解让实体显示在这里了，而是只要出现在接口方法的返回值上的实体都会显示在这里，而@ApiModel 和@ApiModelProperty 这两个注解只是为实体添加注释的。
@ApiModel 为类添加注释@ApiModelProperty 为类属性添加注释

## 8、常用注解

Swagger 的所有注解定义在 io.swagger.annotations 包下下面列一些经常用到的，未列举出来的可以另行查阅说明：

| **Swagger 注解**                                        | **简单说明**                                             |
| ------------------------------------------------------- | -------------------------------------------------------- |
| @Api(tags = "xxx 模块说明")                             | 作用在模块类上                                           |
| @ApiOperation("xxx 接口说明")                           | 作用在接口方法上                                         |
| @ApiModel("xxxPOJO 说明")                               | 作用在模型类上：如 VO、BO                                |
| @ApiModelProperty(value = "xxx 属性说明",hidden = true) | 作用在类方法和属性上，hidden 设置为 true 可 以隐藏该属性 |
| @ApiParam("xxx 参数说明")                               | 作用在参数、方法和字段上，类似                           |
| @ApiModelProperty                                       |

我们也可以给请求的接口配置一些注释

@ApiOperation("狂神的接口") @PostMapping("/kuang") @ResponseBody
public String kuang(@ApiParam("这个名字会被返回")String username){
return username;
}
1
2
3
4
5
6

这样的话，可以给一些比较难理解的属性或者接口，增加一些配置信息，让人更容易阅读！
相较于传统的 Postman 或 Curl 方式测试接口，使用 swagger 简直就是傻瓜式操作，不需要额外说明文档 (写得好本身就是文档)而且更不容易出错，只需要录入数据然后点击 Execute，如果再配合自动化框架， 可以说基本就不需要人为操作了。
Swagger 是个优秀的工具，现在国内已经有很多的中小型互联网公司都在使用它，相较于传统的要先出 Word 接口文档再测试的方式，显然这样也更符合现在的快速迭代开发行情。当然了，提醒下大家在正式 环境要记得关闭 Swagger，一来出于安全考虑二来也可以节省运行时内存。

## 9、其他皮肤

我们可以导入不同的包实现不同的皮肤定义：

### 1、默认的 访问 http://localhost:8080/swagger-ui.html

<dependency>
<groupId>io.springfox</groupId>
<artifactId>springfox-swagger-ui</artifactId>
<version>2.9.2</version>
</dependency>
1
2
3
4
5

2、bootstrap-ui **访问 \*\***http://localhost:8080/doc.html\*\*

<!-- 引入swagger-bootstrap-ui包 /doc.html-->
<dependency>
<groupId>com.github.xiaoymin</groupId>
<artifactId>swagger-bootstrap-ui</artifactId>
<version>1.9.1</version>
</dependency>
1
2
3
4
5
6

3、Layui-ui **访问 \*\***http://localhost:8080/docs.html\*\*

<!-- 引入swagger-ui-layer包 /docs.html-->
<dependency>
<groupId>com.github.caspar-chen</groupId>
<artifactId>swagger-ui-layer</artifactId>
<version>1.1.3</version>
</dependency>
1
2
3
4
5
6

4、mg-ui **访问 \*\***http://localhost:8080/document.html\*\*

<!-- 引入swagger-ui-layer包 /document.html-->
<dependency>
<groupId>com.zyplayer</groupId>
<artifactId>swagger-mg-ui</artifactId>
<version>1.0.6</version>
</dependency>
1
2
3
4
5
6

# 异步任务

1. 创建一个 service 包
1. 创建一个类 AsyncService

异步处理还是非常常用的，比如我们在网站上发送邮件，后台会去发送邮件，此时前台会造成响应 不动，直到邮件发送完毕，响应才会成功，所以我们一般会采用多线程的方式去处理这些任务。
编写方法，假装正在处理数据，使用线程设置一些延时，模拟同步等待的情况；

| 1   | @Service                           |
| --- | ---------------------------------- |
| 2   | public class AsyncService {        |
| 3   |                                    |
| 4   | public void hello(){               |
| 5   | try {                              |
| 6   | Thread.sleep(3000);                |
| 7   | } catch (InterruptedException e) { |
| 8   | e.printStackTrace();               |
| 9   | }                                  |
| 10  | System.out.println("数据处理中 "); |
| 11  | }                                  |
| 12  | }                                  |

1. 编写 controller 包
1. 编写 AsyncController 类

我们去写一个 Controller 测试一下

| 1   | @RestController                |
| --- | ------------------------------ |
| 2   | public class AsyncController { |
| 3   |                                |
| 4   | @Autowired                     |
| 5   | AsyncService asyncService;     |
| 6   |                                |
| 7   | @GetMapping("/hello")          |
| 8   | public String hello(){         |
| 9   | asyncService.hello();          |
| 10  | return "success";              |
| 11  | }                              |
| 12  |                                |
| 13  | }                              |

1. 访问 http://localhost:8080/hello 进行测试，3 秒后出现 success，这是同步等待的情况。

问题：我们如果想让用户直接得到消息，就在后台使用多线程的方式进行处理即可，但是每次都需 要自己手动去编写多线程的实现的话，太麻烦了，我们只需要用一个简单的办法，在我们的方法上 加一个简单的注解即可，如下：

1. 给 hello 方法添加@Async 注解；

//告诉 Spring 这是一个异步方法@Async
public void hello(){ try {
Thread.sleep(3000);
} catch (InterruptedException e) { e.printStackTrace();
}
System.out.println("数据处理中 ");
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

SpringBoot 就会自己开一个线程池，进行调用！但是要让这个注解生效，我们还需要在主程序上添加一个注解@EnableAsync ，开启异步注解功能；

| 1   | @EnableAsync //开启异步注解功能                               |
| --- | ------------------------------------------------------------- |
| 2   | @SpringBootApplication                                        |
| 3   | public class SpringbootTaskApplication {                      |
| 4   |                                                               |
| 5   | public static void main(String[] args) {                      |
| 6   | SpringApplication.run(SpringbootTaskApplication.class, args); |
| 7   | }                                                             |
| 8   |                                                               |
| 9   | }                                                             |

1. 重启测试，网页瞬间响应，后台代码依旧执行！

# 定时任务

项目开发中经常需要执行一些定时任务，比如需要在每天凌晨的时候，分析一次前一天的日志信息，
Spring 为我们提供了异步执行任务调度的方式，提供了两个接口。
TaskExecutor 接口 TaskScheduler 接口
两个注解：
@EnableScheduling @Scheduled

### cron 表达式：

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834661286-8bc7ce78-cbf5-4ae2-8da4-3bb7c22d9ed7.jpeg#)![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834661608-0a647dfd-7a47-4a50-9ed1-b557913f4b4a.jpeg#)

1. 创建一个 ScheduledService

我们里面存在一个 hello 方法，他需要定时执行，怎么处理呢？

| 1   | @Service                               |
| --- | -------------------------------------- |
| 2   | public class ScheduledService {        |
| 3   |                                        |
| 4   | //秒 分 时 日 月 周几                  |
| 5   | //0 \* \* \* \* MON-FRI                |
| 6   | //注意 cron 表达式的用法；             |
| 7   | @Scheduled(cron = "0 \* \* \* \* 0-7") |
| 8   | public void hello(){                   |
| 9   | System.out.println("hello ");          |
| 10  | }                                      |
| 11  | }                                      |

1. 这里写完定时任务之后，我们需要在主程序上增加@EnableScheduling 开启定时任务功能

| 1   | @EnableAsync //开启异步注解功能                               |
| --- | ------------------------------------------------------------- |
| 2   | @EnableScheduling //开启基于注解的定时任务                    |
| 3   | @SpringBootApplication                                        |
| 4   | public class SpringbootTaskApplication {                      |
| 5   |                                                               |
| 6   | public static void main(String[] args) {                      |
| 7   | SpringApplication.run(SpringbootTaskApplication.class, args); |
| 8   | }                                                             |
| 9   |                                                               |
| 10  | }                                                             |

1. 我们来详细了解下 cron 表达式；

[http://www.bejson.com/othertools/cron/](http://www.bejson.com/othertools/cron/)

1. 课堂练习

| 1   | /\* |                                                                             |
| --- | --- | --------------------------------------------------------------------------- |
| 2   | 【0 | 0/5 14,18 \* \* ?】每天 14 点整和 18 点整，每隔 5 分钟执行一次              |
| 3   | 【0 | 15 10 ？ \* 1-6】每个月的周一-周六 10:15 分执行一次                         |
| 4   | 【0 | 0 2 ？ \* 6L】每个月的最后一个周六凌晨 2 点执行一次                         |
| 5   | 【0 | 0 2 LW \* ？】每个月的最后一个工作日凌晨 2 点执行一次                       |
| 6   | 【0 | 0 2-4 ？ \* 1#1】每个月的第一个周一凌晨 2 点到 4 点期间，每个整点都执行一次 |
| 7   | \*/ |                                                                             |

# 邮件任务

邮件发送，在我们的日常开发中，也非常的多，Springboot 也帮我们做了支持邮件发送需要引入 spring-boot-start-mail
SpringBoot 自动配置 MailSenderAutoConﬁguration 定义 MailProperties 内容，配置在 application.yml 中 自动装配 JavaMailSender
测试邮件发送
演示

1. 引入 pom 依赖

<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-mail</artifactId>
</dependency>
1
2
3
4

看它引入的依赖，可以看到 jakarta.mail

<dependency>
<groupId>com.sun.mail</groupId>
<artifactId>jakarta.mail</artifactId>
<version>1.6.4</version>
<scope>compile</scope>
</dependency>
1
2
3
4
5
6

1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834662179-f7f2a869-f00a-49b9-b031-0720389132ad.png#)查看自动配置类：MailSenderAutoConﬁguration

这个类中存在 bean，JavaMailSenderImpl
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834662696-c9ffbe61-7623-4fc1-80a7-63bb0ab8c732.png#)
然后我们去看下配置文件

@ConfigurationProperties( prefix = "spring.mail"
)
public class MailProperties {
private static final Charset DEFAULT_CHARSET; private String host;
private Integer port; private String username; private String password;
private String protocol = "smtp"; private Charset defaultEncoding; private Map<String, String> properties; private String jndiName;
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

1. 配置文件：
   | 1 | [spring.mail.username=24736743@qq.com](mailto:spring.mail.username%3D24736743@qq.com) |
   | --- | --- |
   | 2 | spring.mail.password=yhkrgtqwbnrcbhcj |
   | 3 | spring.mail.host=smtp.qq.com |
   | 4 | # qq 需要配置 ssl |
   | 5 | spring.mail.properties.mail.smtp.ssl.enable=true |

1. Spring 单元测试

| 1   | @Autowired                                                           |
| --- | -------------------------------------------------------------------- |
| 2   | JavaMailSenderImpl mailSender;                                       |
| 3   |                                                                      |
| 4   | @Test                                                                |
| 5   | public void contextLoads() {                                         |
| 6   | //邮件设置 1：一个简单的邮件                                         |
| 7   | SimpleMailMessage message = new SimpleMailMessage();                 |
| 8   | message.setSubject("通知-明天来狂神这听课");                         |
| 9   | message.setText("今晚 7:30 开会");                                   |
| 10  |                                                                      |
| 11  | message.setTo("[24736743@qq.com](mailto:24736743@qq.com)");          |
| 12  | message.setFrom("[24736743@qq.com](mailto:24736743@qq.com)");        |
| 13  | mailSender.send(message);                                            |
| 14  | }                                                                    |
| 15  |                                                                      |
| 16  | @Test                                                                |
| 17  | public void contextLoads2() throws MessagingException {              |
| 18  | //邮件设置 2：一个复杂的邮件                                         |
| 19  | MimeMessage mimeMessage = mailSender.createMimeMessage();            |
| 20  | MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true); |
| 21  |                                                                      |
| 22  | helper.setSubject("通知-明天来狂神这听课");                          |
| 23  | helper.setText("<b style='color:red'>今天 7:30 来开会</b>",true);    |
| 24  |                                                                      |
| 25  | //发送附件                                                           |
| 26  | helper.addAttachment("1.jpg",new File(""));                          |
| 27  | helper.addAttachment("2.jpg",new File(""));                          |
| 28  |                                                                      |
| 29  | helper.setTo("[24736743@qq.com](mailto:24736743@qq.com)");           |
| 30  | helper.setFrom("[24736743@qq.com](mailto:24736743@qq.com)");         |
| 31  |                                                                      |
| 32  | mailSender.send(mimeMessage);                                        |
| 33  | }                                                                    |

# 富文本编辑器

## 1、简介

思考：我们平时在博客园，或者 CSDN 等平台进行写作的时候，有同学思考过他们的编辑器是怎么实现的吗？
在博客园后台的选项设置中，可以看到一个文本编辑器的选项：

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834663245-99f2a333-b1f0-4a48-97d3-d9ccda6cd57f.png#)
其实这个就是富文本编辑器，市面上有许多非常成熟的富文本编辑器，比如：
**Editor.md**——功能非常丰富的编辑器，左端编辑，右端预览，非常方便，完全免费 官网：[https://pandao.github.io/editor.md/](https://pandao.github.io/editor.md/)
**wangEditor**——基于 javascript 和 css 开发的 Web 富文本编辑器， 轻量、简洁、界面美观、易用、
开源免费。
官网：[http://www.wangeditor.com/](http://www.wangeditor.com/)
**TinyMCE**——TinyMCE 是一个轻量级的基于浏览器的所见即所得编辑器，由 JavaScript 写成。它对 IE6+和 Firefox1.5+都有着非常良好的支持。功能齐全，界面美观，就是文档是英文的，对开发人员 英文水平有一定要求。
官 网 ：[https://www.tiny.cloud/docs/demo/full-featured/ ](https://www.tiny.cloud/docs/demo/full-featured/)博客园
**百度 ueditor**——UEditor 是由百度 web 前端研发部开发所见即所得富文本 web 编辑器，具有轻量， 功能齐全，可定制，注重用户体验等特点，开源基于 MIT 协议，允许自由使用和修改代码，缺点是 已经没有更新了
官网：[https://ueditor.baidu.com/website/onlinedemo.html](https://ueditor.baidu.com/website/onlinedemo.html)
**kindeditor**——界面经典。
官网：[http://kindeditor.net/demo.php](http://kindeditor.net/demo.php)
**Textbox**——Textbox 是一款极简但功能强大的在线文本编辑器，支持桌面设备和移动设备。主要 功能包含内置的图像处理和存储、文件拖放、拼写检查和自动更正。此外，该工具还实现了屏幕阅 读器等辅助技术，并符合 WAI-ARIA 可访问性标准。
官网：[https://textbox.io/](https://textbox.io/)
**CKEditor**——国外的，界面美观。
官网：[https://ckeditor.com/ckeditor-5/demo/](https://ckeditor.com/ckeditor-5/demo/)
**quill**——功能强大，还可以编辑公式等
官网：[https://quilljs.com/](https://quilljs.com/)
**simditor**——界面美观，功能较全。
官网：[https://simditor.tower.im/](https://simditor.tower.im/)
**summernote**——UI 好看，精美
官网：[https://summernote.org/](https://summernote.org/)
**jodit**——功能齐全
官网：[https://xdsoft.net/jodit/](https://xdsoft.net/jodit/)
**froala Editor**——界面非常好看，功能非常强大，非常好用（非免费）
官网：[https://www.froala.com/wysiwyg-editor](https://www.froala.com/wysiwyg-editor)

总之，目前可用的富文本编辑器有很多 这只是其中的一部分

## 2、Editor.md

我这里使用的就是下面，就爱上了！
Editor.md
，作为一个资深码农，Mardown 必然是我们程序猿最喜欢的格式，看

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834663852-7460c1ef-1254-448f-a0fb-d68ac0718ff5.jpeg#)
我们可以在官网下载它：[https://pandao.github.io/editor.md/ ](https://pandao.github.io/editor.md/)， 得到它的压缩包！

解压以后，在
examples
然后进行模仿就好了！
目录下面，可以看到他的很多案例使用！学习，其实就是看人家怎么写的，

我们可以将整个解压的文件倒入我们的项目，将一些无用的测试和案例删掉即可！

## 3、基础工程搭建

数据库设计
article：文章表

| **字段** |          | **备注**      |
| -------- | -------- | ------------- |
| id       | int      | 文章的唯一 ID |
| author   | varchar  | 作者          |
| title    | varchar  | 标题          |
| content  | longtext | 文章的内容    |

建表 SQL：

| 1   | CREATE TABLE `article` (                                          |
| --- | ----------------------------------------------------------------- |
| 2   | `id` int(10) NOT NULL AUTO_INCREMENT COMMENT 'int 文章的唯一 ID', |
| 3   | `author` varchar(50) NOT NULL COMMENT '作者',                     |
| 4   | `title` varchar(100) NOT NULL COMMENT '标题',                     |
| 5   | `content` longtext NOT NULL COMMENT '文章的内容',                 |
| 6   | PRIMARY KEY (`id`)                                                |
| 7   | ) ENGINE=InnoDB DEFAULT CHARSET=utf8                              |

基础项目搭建

1、新建一个 SpringBoot 项目（模块：web，mysql 驱动，mybatis，thymeleaf、lombok. ）

spring: datasource:
username: root password: 123456
#?serverTimezone=UTC 解决时区的报错
url: jdbc:mysql://localhost:3306/springboot? serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
driver-class-name: com.mysql.cj.jdbc.Driver
1
2
3
4
5
6

7
<resources>
<resource>
<directory>src/main/java</directory>
<includes>
<include>\*_/_.xml</include>
</includes>
<filtering>true</filtering>
</resource>
</resources>
1
2
3
4
5
6
7
8
9

2、实体类：

| 1   | //文章类                                       |
| --- | ---------------------------------------------- |
| 2   | @Data                                          |
| 3   | @NoArgsConstructor                             |
| 4   | @AllArgsConstructor                            |
| 5   | public class Article implements Serializable { |
| 6   |                                                |
| 7   | private int id; //文章的唯一 ID                |
| 8   | private String author; //作者名                |
| 9   | private String title; //标题                   |
| 10  | private String content; //文章的内容           |
| 11  |                                                |
| 12  | }                                              |

3、mapper 接口：

| 1   | @Mapper                          |
| --- | -------------------------------- |
| 2   | @Repository                      |
| 3   | public interface ArticleMapper { |
| 4   | //查询所有的文章                 |
| 5   | List<Article> queryArticles();   |
| 6   |                                  |
| 7   | //新增一个文章                   |
| 8   | int addArticle(Article article); |
| 9   |                                  |
| 10  | //根据文章 id 查询文章           |
| 11  | Article getArticleById(int id);  |
| 12  |                                  |
| 13  | //根据文章 id 删除文章           |
| 14  | int deleteArticleById(int id);   |
| 15  |                                  |
| 16  | }                                |

| 1   | <?xml version="1.0" encoding="UTF-8"?>                       |
| --- | ------------------------------------------------------------ |
| 2   | <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" |

| 3   | "[http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)"> |
| --- | --------------------------------------------------------------------------------------------- |
| 4   |                                                                                               |
| 5   | <mapper namespace="com.kuang.mapper.ArticleMapper">                                           |
| 6   |                                                                                               |
| 7   | <select id="queryArticles" resultType="Article">                                              |
| 8   | select \* from article                                                                        |
| 9   | </select>                                                                                     |
| 10  |                                                                                               |
| 11  | <select id="getArticleById" resultType="Article">                                             |
| 12  | select \* from article where id = #{id}                                                       |
| 13  | </select>                                                                                     |
| 14  |                                                                                               |
| 15  | <insert id="addArticle" parameterType="Article">                                              |
| 16  | insert into article (author,title,content) values (#{author},#                                |
|     | {title},#{content});                                                                          |
| 17  | </insert>                                                                                     |
| 18  |                                                                                               |
| 19  | <delete id="deleteArticleById" parameterType="int">                                           |
| 20  | delete from article where id = #{id}                                                          |
| 21  | </delete>                                                                                     |
| 22  |                                                                                               |
| 23  | </mapper>                                                                                     |

### 既然已经提供了 myBatis 的映射配置文件，自然要告诉 spring boot 这些文件的位置

mybatis:
mapper-locations: classpath:com/kuang/mapper/\*.xml type-aliases-package: com.kuang.pojo
1
2
3

编写一个 Controller 测试下，是否 ok；

## 4、文章编辑整合

1、导入 editor.md 资源 ，删除多余文件
2、编辑文章页面 editor.html、需要引入 jQuery；

| 1   | <!DOCTYPE html>                                                                                      |
| --- | ---------------------------------------------------------------------------------------------------- |
| 2   | <html class="x-admin-sm" lang="zh" xmlns:th="[http://www.thymeleaf.org](http://www.thymeleaf.org/)"> |
| 3   |                                                                                                      |
| 4   | <head>                                                                                               |
| 5   | <meta charset="UTF-8">                                                                               |
| 6   | <title>秦疆'Blog</title>                                                                             |
| 7   | <meta name="renderer" content="webkit">                                                              |
| 8   | <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">                                       |
| 9   | <meta name="viewport" content="width=device-width,user-scalable=yes,                                 |
|     | minimum-scale=0.4, initial-scale=0.8,target-densitydpi=low-dpi" />                                   |
| 10  | <!--Editor.md-->                                                                                     |
| 11  | <link rel="stylesheet" th:href="@{/editormd/css/editormd.css}"/>                                     |
| 12  | <link rel="shortcut icon"                                                                            |
|     | href="https://pandao.github.io/editor.md/favicon.ico" type="image/x-icon"                            |
|     | />                                                                                                   |
| 13  | </head>                                                                                              |
| 14  |                                                                                                      |

| 15  | <body>                                                            |
| --- | ----------------------------------------------------------------- | --- |
| 16  |                                                                   |
| 17  | <div class="layui-fluid">                                         |
| 18  | <div class="layui-row layui-col-space15">                         |
| 19  | <div class="layui-col-md12">                                      |
| 20  | <!--博客表单-->                                                   |
| 21  | <form name="mdEditorForm">                                        |
| 22  | <div>                                                             |
| 23  | 标题： <input type="text" name="title">                           |
| 24  | </div>                                                            |
| 25  | <div>                                                             |
| 26  | 作者： <input type="text" name="author">                          |
| 27  | </div>                                                            |
| 28  | <div id="article-content">                                        |
| 29  | <textarea name="content" id="content"                             |
|     | style="display:none;"> </textarea>                                |
| 30  | </div>                                                            |
| 31  | </form>                                                           |
| 32  |                                                                   |
| 33  | </div>                                                            |
| 34  | </div>                                                            |
| 35  | </div>                                                            |
| 36  | </body>                                                           |
| 37  |                                                                   |
| 38  | <!--editormd-->                                                   |
| 39  | <script th:src="@{/editormd/lib/jquery.min.js}"></script>         |
| 40  | <script th:src="@{/editormd/editormd.js}"></script>               |
| 41  |                                                                   |
| 42  | <script type="text/javascript">                                   |
| 43  | var testEditor;                                                   |
| 44  |                                                                   |
| 45  | //window.onload = function(){ }                                   |
| 46  | $(function() {                                                    |
| 47  | testEditor = editormd("article-content", {                        |
| 48  | width : "95%",                                                    |
| 49  | height : 400,                                                     |
| 50  | syncScrolling : "single",                                         |
| 51  | path : "../editormd/lib/",                                        |
| 52  | saveHTMLToTextarea : true, // 保 存 HTML 到 Textarea              |
| 53  | emoji: true,                                                      |
| 54  | theme: "dark",//工具栏主题                                        |
| 55  | previewTheme: "dark",//预览主题                                   |
| 56  | editorTheme: "pastel-on-dark",//编辑主题                          |
| 57  | tex : true, // 开启科学公式 TeX 语言支持，默认关闭                |
| 58  | flowChart : true, // 开启流程图支持，默认关闭                     |
| 59  | sequenceDiagram : true, // 开启时序/序列图支持，默认关闭,         |
| 60  | //图片上传                                                        |
| 61  | imageUpload : true,                                               |
| 62  | imageFormats : ["jpg", "jpeg", "gif", "png", "bmp", "webp"],      |
| 63  | imageUploadURL : "/article/file/upload",                          |
| 64  | onload : function() {                                             |
| 65  | console.log('onload', this);                                      |
| 66  | },                                                                |
| 67  | /_指定需要显示的功能按钮_/                                        |
| 68  | toolbarIcons : function() {                                       |
| 69  | return ["undo","redo","                                           | ",  |
| 70  |                                                                   |
|     | "bold","del","italic","quote","ucwords","uppercase","lowercase"," | ",  |

| 71  | "h1","h2","h3","h4","h5","h6","                                     | ",  |
| --- | ------------------------------------------------------------------- | --- |
| 72  | "list-ul","list-ol","hr","                                          | ",  |
| 73  | "link","reference-link","image","code","preformatted-               |
|     | text",                                                              |
| 74  | "code-block","table","datetime","emoji","html-                      |
|     | entities","pagebreak","                                             | ",  |
| 75  | "goto-                                                              |
|     | line","watch","preview","fullscreen","clear","search","             | ",  |
| 76  | "help","info","releaseIcon", "index"]                               |
| 77  | },                                                                  |
| 78  |                                                                     |
| 79  | /_自定义功能按钮，下面我自定义了 2 个，一个是发布，一个是返回首页_/ |
| 80  | toolbarIconTexts : {                                                |
| 81  | releaseIcon : "<span bgcolor=\"gray\">发布</span>",                 |
| 82  | index : "<span bgcolor=\"red\">返回首页</span>",                    |
| 83  | },                                                                  |
| 84  |                                                                     |
| 85  | /_给自定义按钮指定回调函数_/                                        |
| 86  | toolbarHandlers:{                                                   |
| 87  | releaseIcon : function(cm, icon, cursor, selection) {               |
| 88  | //表单提交                                                          |
| 89  | mdEditorForm.method = "post";                                       |
| 90  | mdEditorForm.action = "/article/addArticle";//提交至服务            |
|     | 器的路径                                                            |
| 91  | mdEditorForm.submit();                                              |
| 92  | },                                                                  |
| 93  | index : function(){                                                 |
| 94  | window.location.href = '/';                                         |
| 95  | },                                                                  |
| 96  | }                                                                   |
| 97  | });                                                                 |
| 98  | });                                                                 |
| 99  | </script>                                                           |
| 100 |                                                                     |
| 101 | </html>                                                             |

3、编写 Controller，进行跳转，以及保存文章

| 1   | @Controller                                |
| --- | ------------------------------------------ |
| 2   | @RequestMapping("/article")                |
| 3   | public class ArticleController {           |
| 4   |                                            |
| 5   | @GetMapping("/toEditor")                   |
| 6   | public String toEditor(){                  |
| 7   | return "editor";                           |
| 8   | }                                          |
| 9   |                                            |
| 10  | @PostMapping("/addArticle")                |
| 11  | public String addArticle(Article article){ |
| 12  | articleMapper.addArticle(article);         |
| 13  | return "editor";                           |
| 14  | }                                          |
| 15  |                                            |
| 16  | }                                          |

图片上传问题

1、前端 js 中添加配置

| 1   | //图片上传                                                               |
| --- | ------------------------------------------------------------------------ |
| 2   | imageUpload : true,                                                      |
| 3   | imageFormats : ["jpg", "jpeg", "gif", "png", "bmp", "webp"],             |
| 4   | imageUploadURL : "/article/file/upload", // //这个是上传图片时的访问地址 |

2、后端请求，接收保存这个图片, 需要导入 FastJson 的依赖！

| 1   | //博客图片上传问题                                                        |
| --- | ------------------------------------------------------------------------- |
| 2   | @RequestMapping("/file/upload")                                           |
| 3   | @ResponseBody                                                             |
| 4   | public JSONObject fileUpload(@RequestParam(value = "editormd-image-file", |
|     | required = true) MultipartFile file, HttpServletRequest request) throws   |
|     | IOException {                                                             |
| 5   | //上传路径保存设置                                                        |
| 6   |                                                                           |
| 7   | //获得 SpringBoot 当前项目的路径：System.getProperty("user.dir")          |
| 8   | String path = System.getProperty("user.dir")+"/upload/";                  |
| 9   |                                                                           |
| 10  | //按照月份进行分类：                                                      |
| 11  | Calendar instance = Calendar.getInstance();                               |
| 12  | String month = (instance.get(Calendar.MONTH) + 1)+"月";                   |
| 13  | path = path+month;                                                        |
| 14  |                                                                           |
| 15  | File realPath = new File(path);                                           |
| 16  | if (!realPath.exists()){                                                  |
| 17  | realPath.mkdir();                                                         |
| 18  | }                                                                         |
| 19  |                                                                           |
| 20  | //上传文件地址                                                            |
| 21  | System.out.println("上传文件保存地址："+realPath);                        |
| 22  |                                                                           |
| 23  | //解决文件名字问题：我们使用 uuid;                                        |
| 24  | String filename = "ks-"+UUID.randomUUID().toString().replaceAll("-",      |
|     | "");                                                                      |
| 25  | //通过 CommonsMultipartFile 的方法直接写文件（注意这个时候）              |
| 26  | file.transferTo(new File(realPath +"/"+ filename));                       |
| 27  |                                                                           |
| 28  | //给 editormd 进行回调                                                    |
| 29  | JSONObject res = new JSONObject();                                        |
| 30  | res.put("url","/upload/"+month+"/"+ filename);                            |
| 31  | res.put("success", 1);                                                    |
| 32  | res.put("message", "upload success!");                                    |
| 33  |                                                                           |
| 34  | return res;                                                               |
| 35  | }                                                                         |

3、解决文件回显显示的问题，设置虚拟目录映射！在我们自己拓展的 MvcConﬁg 中进行配置即可！

| 1   | @Configuration                                                                     |
| --- | ---------------------------------------------------------------------------------- |
| 2   | public class MyMvcConfig implements WebMvcConfigurer {                             |
| 3   |                                                                                    |
| 4   | // 文件保存在真实目录/upload/下，                                                  |
| 5   | // 访问的时候使用虚路径/upload，比如文件名为 1.png，就直接/upload/1.png 就 ok 了。 |
| 6   | @Override                                                                          |
| 7   | public void addResourceHandlers(ResourceHandlerRegistry registry) {                |
| 8   | registry.addResourceHandler("/upload/\*\*")                                        |
| 9   |                                                                                    |
|     | .addResourceLocations("file:"+System.getProperty("user.dir")+"/upload/");          |
| 10  | }                                                                                  |
| 11  |                                                                                    |
| 12  | }                                                                                  |

表情包问题
自己手动下载，emoji 表情包，放到图片路径下：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834665326-af95a1a8-7139-4788-8740-9145c89bfcc4.png#)
修改 editormd.js 文件

// Emoji graphics files url path editormd.emoji = {
path : "../editormd/plugins/emoji-dialog/emoji/", ext : ".png"
};
1
2
3
4
5

## 5、文章展示

1、Controller 中增加方法

@GetMapping("/{id}")
public String show(@PathVariable("id") int id,Model model){ Article article = articleMapper.getArticleById(id); model.addAttribute("article",article);
return "article";
}
1
2
3
4
5
6

2、编写页面 article.html

| 1   | <!DOCTYPE html>                                                                   |
| --- | --------------------------------------------------------------------------------- |
| 2   | <html lang="en" xmlns:th="[http://www.thymeleaf.org](http://www.thymeleaf.org/)"> |
| 3   | <head>                                                                            |

| 4   | <meta charset="UTF-8">                                                    |
| --- | ------------------------------------------------------------------------- |
| 5   | <meta name="viewport" content="width=device-width, initial-scale=1,       |
|     | maximum-scale=1">                                                         |
| 6   | <title th:text="${article.title}"></title>                                |
| 7   | </head>                                                                   |
| 8   | <body>                                                                    |
| 9   |                                                                           |
| 10  | <div>                                                                     |
| 11  | <!--文章头部信息：标题，作者，最后更新日期，导航-->                       |
| 12  | <h2 style="margin: auto 0" th:text="${article.title}"></h2>               |
| 13  | 作者：<span style="float: left" th:text="${article.author}"></span>       |
| 14  | <!--文章主体内容-->                                                       |
| 15  | <div id="doc-content">                                                    |
| 16  | <textarea style="display:none;" placeholder="markdown"                    |
|     | th:text="${article.content}"></textarea>                                  |
| 17  | </div>                                                                    |
| 18  |                                                                           |
| 19  | </div>                                                                    |
| 20  |                                                                           |
| 21  | <link rel="stylesheet" th:href="@{/editormd/css/editormd.preview.css}" /> |
| 22  | <script th:src="@{/editormd/lib/jquery.min.js}"></script>                 |
| 23  | <script th:src="@{/editormd/lib/marked.min.js}"></script>                 |
| 24  | <script th:src="@{/editormd/lib/prettify.min.js}"></script>               |
| 25  | <script th:src="@{/editormd/lib/raphael.min.js}"></script>                |
| 26  | <script th:src="@{/editormd/lib/underscore.min.js}"></script>             |
| 27  | <script th:src="@{/editormd/lib/sequence-diagram.min.js}"></script>       |
| 28  | <script th:src="@{/editormd/lib/flowchart.min.js}"></script>              |
| 29  | <script th:src="@{/editormd/lib/jquery.flowchart.min.js}"></script>       |
| 30  | <script th:src="@{/editormd/editormd.js}"></script>                       |
| 31  |                                                                           |
| 32  | <script type="text/javascript">                                           |
| 33  | var testEditor;                                                           |
| 34  | $(function () {                                                           |
| 35  | testEditor = editormd.markdownToHTML("doc-content", {//注意：这里是上面   |
|     | DIV 的 id                                                                 |
| 36  | htmlDecode: "style,script,iframe",                                        |
| 37  | emoji: true,                                                              |
| 38  | taskList: true,                                                           |
| 39  | tocm: true,                                                               |
| 40  | tex: true, // 默认不解析                                                  |
| 41  | flowChart: true, // 默认不解析                                            |
| 42  | sequenceDiagram: true, // 默认不解析                                      |
| 43  | codeFold: true                                                            |
| 44  | });});                                                                    |
| 45  | </script>                                                                 |
| 46  | </body>                                                                   |
| 47  | </html>                                                                   |

重启项目，访问进行测试！
