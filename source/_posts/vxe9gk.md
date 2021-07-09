---
title: 27、SpringBoot开发单体应用
urlname: vxe9gk
date: '2021-07-09 20:43:48 +0800'
tags: []
categories: []
---

# Web 开发探究

简介
好的，同学们，那么接下来呢，我们开始学习 SpringBoot 与 Web 开发，从这一章往后，就属于我们实战部分的内容了；
其实 SpringBoot 的东西用起来非常简单，因为 SpringBoot 最大的特点就是自动装配。

## 使用 SpringBoot 的步骤：

1、创建一个 SpringBoot 应用，选择我们需要的模块，SpringBoot 就会默认将我们的需要的模块自动配 置好。
2、手动在配置文件中配置部分配置项目就可以运行起来了。
3、专注编写业务代码，不需要考虑以前那样一大堆的配置了。

要熟悉掌握开发，之前学习的自动配置的原理一定要搞明白！
比如 SpringBoot 到底帮我们配置了什么？我们能不能修改？我们能修改哪些配置？我们能不能扩展？ 向容器中自动配置组件 ： **_ Autoconﬁguration
自动配置类，封装配置文件的内容：_**Properties
没事就找找类，看看自动装配原理！
我们之后来进行一个单体项目的小项目测试，让大家能够快速上手开发！

# 静态资源处理

静态资源映射规则

## 首先，我们搭建一个普通的 SpringBoot 项目，回顾一下 HelloWorld 程序！【演示】

写请求非常简单，那我们要引入我们前端资源，我们项目中有许多的静态资源，比如 css，js 等文件，这个 SpringBoot 怎么处理呢？
如果我们是一个 web 应用，我们的 main 下会有一个 webapp，我们以前都是将所有的页面导在这里面
的，对吧！但是我们现在的 pom 呢，打包方式是为 jar 的方式，那么这种方式 SpringBoot 能不能来给我们写页面呢？当然是可以的，但是 SpringBoot 对于静态资源放置的位置，是有规定的！

## 我们先来聊聊这个静态资源映射规则：

SpringBoot 中，SpringMVC 的 web 配置都在 WebMvcAutoConﬁguration 这个配置类里面； 我们可以去看看 WebMvcAutoConﬁgurationAdapter 中有很多配置方法；
有一个方法： 添加资源处理
addResourceHandlers

1. @Override
1. public void addResourceHandlers(ResourceHandlerRegistry registry) {
1. if (!this.resourceProperties.isAddMappings()) {

1. // 已禁用默认资源处理
1. logger.debug("Default resource handling disabled");
1. return;

7 }

1.  // 缓存控制
1.  Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
1.      CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
1.  // webjars 配置
1.  if (!registry.hasMappingForPattern("/webjars/\*\*")) { 13

customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/
\*\*")
14
.addResourceLocations("classpath:/META-INF/resources/webjars/")
15
.setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl)); 16 }

1. // 静态资源配置
1. String staticPathPattern = this.mvcProperties.getStaticPathPattern();
1. if (!registry.hasMappingForPattern(staticPathPattern)) { 20

customizeResourceHandlerRegistration(registry.addResourceHandler(staticPath Pattern)
21
.addResourceLocations(getResourceLocations(this.resourceProperties.getStatic Locations()))
22
.setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl)); 23 }
24 }

读一下源代码：比如所有的 /webjars/\*\* ， 都需要去 classpath:/META-INF/resources/webjars/ 找对应的资源；

那什么是 webjars 呢？
Webjars 本质就是以 jar 包的方式引入我们的静态资源 ， 我们以前要导入一个静态资源文件，直接导入即可。
使用 SpringBoot 需要使用 Webjars，我们可以去搜索一下：
网站：[https://www.webjars.org ](https://www.webjars.org/)【网站带看，并引入 jQuery 测试】要使用 jQuery，我们只要要引入 jQuery 对应版本的 pom 依赖即可！

1. <dependency>
1. <groupId>org.webjars</groupId>
1. <artifactId>jquery</artifactId>
1. <version>3.4.1</version>
1. </dependency>

导入完毕，查看 webjars 目录结构，并访问 Jquery.js 文件！

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834630807-95cf083b-a057-4325-a521-59dd5090e422.png#)
访问：只要是静态资源，SpringBoot 就会去对应的路径寻找资源，我们这里访问 ：
http://localhost:8080/webjars/jquery/3.4.1/jquery.js
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834631054-87a812eb-0c36-4fcb-b0d7-0e1e52692484.png#)

第二种静态资源映射规则
那我们项目中要是使用自己的静态资源该怎么导入呢？我们看下一行代码；
我们去找 staticPathPattern 发现第二种映射规则 ： /\*\* , 访问当前的项目任意资源，它会去找
resourceProperties 这个类，我们可以点进去看一下分析：

1. // 进入方法
1. public String[] getStaticLocations() {
1. return this.staticLocations; 4 }
1. // 找到对应的值
1. private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
1. // 找到路径
1. private static final String[] CLASSPATH_RESOURCE_LOCATIONS = {
1. "classpath:/META-INF/resources/",
1. "classpath:/resources/",
1. "classpath:/static/",
1. "classpath:/public/" 13 };

ResourceProperties 可以设置和我们静态资源有关的参数；这里面指向了它会去寻找资源的文件夹，即上面数组的内容。
所以得出结论，以下四个目录存放的静态资源可以被我们识别：

| 1   | "classpath:/META-INF/resources/" |
| --- | -------------------------------- |
| 2   | "classpath:/resources/"          |
| 3   | "classpath:/static/"             |
| 4   | "classpath:/public/"             |

我们可以在 resources 根目录下新建对应的文件夹，都可以存放我们的静态文件；
比如我们访问 http://localhost:8080/1.js , 他就会去这些文件夹中寻找对应的静态资源文件；

自定义静态资源路径
我们也可以自己通过配置文件来指定一下，哪些文件夹是需要我们放静态资源文件的，在
application.properties 中配置；

1 spring.resources.static-locations=classpath:/coding/,classpath:/kuang/

一旦自己定义了静态文件夹的路径，原来的自动配置就都会失效了！

# 首页处理

静态资源文件夹说完后，我们继续向下看源码！可以看到一个欢迎页的映射，就是我们的首页！

1. @Bean
1. public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,

3
FormattingConversionService mvcConversionService,
4
ResourceUrlProvider mvcResourceUrlProvider) {

1.      WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
1.      new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(), // getWelcomePage 获得欢迎页
1.  this.mvcProperties.getStaticPathPattern()); 8

welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionServ ice, mvcResourceUrlProvider));
9 return welcomePageHandlerMapping;
10 }

点进去继续看

1.  private Optional<Resource> getWelcomePage() {
1.      String[] locations = getResourceLocations(this.resourceProperties.getStaticLocations());
1.  // ::是 java8 中新引入的运算符
1.  // Class::function 的时候 function 是属于 Class 的，应该是静态方法。
1.  // this::function 的 funtion 是属于这个对象的。
1.  // 简而言之，就是一种语法糖而已，是一种简写
1.      return Arrays.stream(locations).map(this::getIndexHtml).filter(this::isReadable).fi ndFirst();

8 }

1. // 欢迎页就是一个 location 下的的 index.html 而已
1. private Resource getIndexHtml(String location) {
1. return this.resourceLoader.getResource(location + "index.html"); 12 }

欢迎页，静态资源文件夹下的所有 index.html 页面；被 /\*\* 映射。
比如我访问 http://localhost:8080/ ，就会找静态资源文件夹下的 index.html 【可以测试一下】
新建一个 index.html ，在我们上面的 3 个目录中任意一个；然后访问测试 http://localhost:8080/ 看结果！

**关于网站图标说明**：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834631379-ea057bf7-1352-48d7-908c-b520d5d177dc.jpeg#)

与其他静态资源一样，Spring Boot 在配置的静态内容位置中查找 favicon.ico。如果存在这样的文件， 它将自动用作应用程序的 favicon。
1、关闭 SpringBoot 默认图标

1. #关闭默认图标
1. spring.mvc.favicon.enabled=false

2、自己放一个图标在静态资源目录下，我放在 public 目录下
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834631850-26126fe3-d3b4-418e-b88b-eaea1e4edf81.png#)3、清除浏览器缓存！刷新网页，发现图标已经变成自己的了！

# Thymeleaf

模板引擎
前端交给我们的页面，是 html 页面。如果是我们以前开发，我们需要把他们转成 jsp 页面，jsp 好处就是当我们查出一些数据转发到 JSP 页面以后，我们可以用 jsp 轻松实现数据的显示，及交互等。
jsp 支持非常强大的功能，包括能写 Java 代码，但是呢，我们现在的这种情况，SpringBoot 这个项目首先 是以 jar 的方式，不是 war，像第二，我们用的还是嵌入式的 Tomcat，所以呢，**他现在默认是不支持 jsp 的**。
那不支持 jsp，如果我们直接用纯静态页面的方式，那给我们开发会带来非常大的麻烦，那怎么办呢？

## SpringBoot 推荐你可以来使用模板引擎：

模板引擎，我们其实大家听到很多，其实 jsp 就是一个模板引擎，还有以用的比较多的 freemarker，包括
SpringBoot 给我们推荐的 Thymeleaf，模板引擎有非常多，但再多的模板引擎，他们的思想都是一样 的，什么样一个思想呢我们来看一下这张图：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834632392-d7c611a0-074b-4496-9b29-91fbff968409.jpeg#)

模板引擎的作用就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些 值，从哪来呢，就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引 擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们
想要的内容给我们写出去，这就是我们这个模板引擎，不管是 jsp 还是其他模板引擎，都是这个思想。只 不过呢，就是说不同模板引擎之间，他们可能这个语法有点不一样。其他的我就不介绍了，我主要来介 绍一下 SpringBoot 给我们推荐的 Thymeleaf 模板引擎，这模板引擎呢，是一个高级语言的模板引擎，他 的这个语法更简单。而且呢，功能更强大。
我们呢，就来看一下这个模板引擎，那既然要看这个模板引擎。首先，我们来看 SpringBoot 里边怎么用。

引入 Thymeleaf
怎么引入呢，对于 springboot 来说，什么事情不都是一个 start 的事情嘛，我们去在项目中引入一下。给大家三个网址：
Thymeleaf 官网：[https://www.thymeleaf.org/](https://www.thymeleaf.org/)
Thymeleaf 在 Github 的主页：[https://github.com/thymeleaf/thymeleaf](https://github.com/thymeleaf/thymeleaf)
Spring 官方文档： 找到我们对应的版本
[https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter](https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter)

找到对应的 pom 依赖：可以适当点进源码看下本来的包！

1. <!--thymeleaf-->
1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-starter-thymeleaf</artifactId>
1. </dependency>

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834632903-ad8b8036-a289-46f9-9e66-5e5a29094b69.png#)Maven 会自动下载 jar 包，我们可以去看下下载的东西；

thymeleaf 分析
前面呢，我们已经引入了 Thymeleaf，那这个要怎么使用呢？
我们首先得按照 SpringBoot 的自动配置原理看一下我们这个 Thymeleaf 的自动配置规则，在按照那个规 则，我们进行使用。
我们去找一下 Thymeleaf 的自动配置类：ThymeleafProperties

1. @ConfigurationProperties(
1. prefix = "spring.thymeleaf" 3 )
1. public class ThymeleafProperties {
1. private static final Charset DEFAULT_ENCODING;
1. public static final String DEFAULT_PREFIX = "classpath:/templates/";
1. public static final String DEFAULT_SUFFIX = ".html";
1. private boolean checkTemplate = true;
1. private boolean checkTemplateLocation = true;
1. private String prefix = "classpath:/templates/";
1. private String suffix = ".html";
1. private String mode = "HTML";
1. private Charset encoding; 14 }

我们可以在其中看到默认的前缀和后缀！
我们只需要把我们的 html 页面放在类路径下的 templates 下，thymeleaf 就可以帮我们自动渲染了。 使用 thymeleaf 什么都不需要配置，只需要将他放在指定的文件夹下即可！

## 测试：

1、编写一个 TestController

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
@Controller
public class TestController {
@RequestMapping("/t1") public String test1(){
//classpath:/templates/test.html return "test";
}
}

2、编写一个测试页面 test.html 放在 templates 目录下

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
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Title</title>
</head>
<body>
<h1>测试页面</h1>
</body>
</html>

3、启动项目请求测试

Thymeleaf 语法学习
要学习语法，还是参考官网文档最为准确，我们找到对应的版本看一下；
Thymeleaf 官网：[https://www.thymeleaf.org/ ](https://www.thymeleaf.org/)， 简单看一下官网！我们去下载 Thymeleaf 的官方文档！

## 我们做个最简单的练习 ： 我们需要查出一些数据，在页面中展示

1、修改测试请求，增加数据传输；

1. @RequestMapping("/t1")
1. public String test1(Model model){
1. //存入数据
1. model.addAttribute("msg","Hello,Thymeleaf");
1. //classpath:/templates/test.html
1. return "test"; 7 }

2、我们要使用 thymeleaf，需要在 html 文件中导入命名空间的约束，方便提示。 我们可以去官方文档的#3 中看一下命名空间拿来过来：
1 [xmlns:th="http://www.thymeleaf.org](http://www.thymeleaf.org/)"

3、我们去编写下前端页面

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

<!DOCTYPE html>
<html lang="en" xmlns:th=["http://www.thymeleaf.org](http://www.thymeleaf.org/)">
<head>
<meta charset="UTF-8">
<title>狂神说</title>
</head>
<body>
<h1>测试页面</h1>
<!--th:text就是将div中的内容设置为它指定的值，和之前学习的Vue一样-->
<div th:text="${msg}"></div>
</body>
</html>

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834633371-9ce63481-4598-4627-aa4f-d99ff60e11ff.jpeg#)4、启动测试！

## OK，入门搞定，我们来认真研习一下 Thymeleaf 的使用语法！

**1、我们可以使用任意的 th:attr 来替换 Html 中原生属性的值！**参考官网文档#10； th 语法
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834633724-a1248ea2-7542-41f8-a377-512eedff0b4d.jpeg#)

## 2、我们能写那些表达式呢？我们可以看到官方文档 #4

1.  Simple expressions:（表达式语法）
1.  Variable Expressions: ${...}：获取变量值；OGNL；

    1. 1）、获取对象的属性、调用方法
    1. 2）、使用内置的基本对象： #18
    1. #ctx : the context object.
    1. #vars: the context variables.
    1. #locale : the context locale.
    1. #request : (only in Web Contexts) the HttpServletRequest object.
    1. #response : (only in Web Contexts) the HttpServletResponse object.
    1. #session : (only in Web Contexts) the HttpSession object.
    1. #servletContext : (only in Web Contexts) the ServletContext object. 12

1.  3）、内置的一些工具对象：
1.  #execInfo : information about the template being processed.
1.  #uris : methods for escaping parts of URLs/URIs
1.      #conversions : methods for executing the configured conversion service (if any).
1.      #dates : methods for java.util.Date objects: formatting, component extraction, etc.
1.      #calendars : analogous to #dates , but for java.util.Calendar objects.
1.  #numbers : methods for formatting numeric objects.
1.      #strings : methods for String objects: contains, startsWith, prepending/appending, etc.
1.  #objects : methods for objects in general.
1.  #bools : methods for boolean evaluation.
1.  #arrays : methods for arrays.
1.  #lists : methods for lists.
1.  #sets : methods for sets.
1.  #maps : methods for maps.
1.      #aggregates : methods for creating aggregates on arrays or collections.

# 28 ============================================================================

29

1. Selection Variable Expressions: \*{...}：选择表达式：和${}在功能上是一样；
1. Message Expressions: #{...}：获取国际化内容
1. Link URL Expressions: @{...}：定义 URL；
1. Fragment Expressions: ~{...}：片段引用表达式 34
1. Literals（字面量）
1. Text literals: 'one text' , 'Another one!' ,… 37 Number literals: 0 , 34 , 3.0 , 12.3 ,…
1. Boolean literals: true , false
1. Null literal: null
1. Literal tokens: one , sometext , main ,… 41
1. Text operations:（文本操作）
1. String concatenation: +
1. Literal substitutions: |The name is ${name}| 45
1. Arithmetic operations:（数学运算）
1. Binary operators: + , - , \* , / , %
1. Minus sign (unary operator): - 49
1. Boolean operations:（布尔运算）
1. Binary operators: and , or
1. Boolean negation (unary operator): ! , not 53

54 Comparisons and equality:（比较运算）

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
Comparators: > , < , >= , <= ( gt , lt , ge , le )
Equality operators: == , != ( eq , ne )
Conditional operators:条件运算（三元运算符）
If-then: (if) ? (then)
If-then-else: (if) ? (then) : (else) Default: (value) ?: (defaultvalue)

Special tokens:
No-Operation: \_

## 练习测试：

1、 我们编写一个 Controller，放一些数据

1. @RequestMapping("/t2")
1. public String test2(Map<String,Object> map){
1. //存入数据
1. map.put("msg","<h1>Hello</h1>");
1. map.put("users", Arrays.asList("qinjiang","kuangshen"));
1. //classpath:/templates/test.html
1. return "test"; 8 }

2、测试页面取出数据

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

<!DOCTYPE html>
<html lang="en" xmlns:th=["http://www.thymeleaf.org](http://www.thymeleaf.org/)">
<head>
<meta charset="UTF-8">
<title>狂神说</title>
</head>
<body>
<h1>测试页面</h1>
<div th:text="${msg}"></div>
<!--不转义-->
<div th:utext="${msg}"></div>

<!--遍历数据-->
<!--th:each每次遍历都会生成当前这个标签：官网#9-->
<h4 th:each="user :${users}" th:text="${user}"></h4>

<h4>
<!--行内写法：官网#12-->
<span th:each="user:${users}">[[${user}]]</span>
</h4>

</body>
</html>

3、启动项目测试！

## 我们看完语法，很多样式，我们即使现在学习了，也会忘记，所以我们在学习过程中，需要使用什么， 根据官方文档来查询，才是最重要的，要熟练使用官方文档！

**MVC 自动配置原理**

官网阅读
在进行项目编写前，我们还需要知道一个东西，就是 SpringBoot 对我们的 SpringMVC 还做了哪些配置， 包括如何扩展，如何定制。
只有把这些都搞清楚了，我们在之后使用才会更加得心应手。 途径一：源码分析，途径二：官方文档！
[地址 ：https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-feature s-spring-mvc-auto-conﬁguration](https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-auto-configuration)

1. Spring MVC Auto-configuration
1. // Spring Boot 为 Spring MVC 提供了自动配置，它可以很好地与大多数应用程序一起工作。
1. Spring Boot provides auto-configuration for Spring MVC that works well with most applications.
1. // 自动配置在 Spring 默认设置的基础上添加了以下功能：
1. The auto-configuration adds the following features on top of Spring’s defaults:
1. // 包含视图解析器
1. Inclusion of ContentNegotiatingViewResolver and BeanNameViewResolver beans.
1. // 支持静态资源文件夹的路径，以及 webjars
1. Support for serving static resources, including support for WebJars
1. // 自动注册了 Converter：
1. // 转换器，这就是我们网页提交数据到后台自动封装成为对象的东西，比如把"1"字符串自动转换为

int 类型

1. // Formatter：【格式化器，比如页面给我们了一个 2019-8-10，它会给我们自动格式化为 Date 对象】
1. Automatic registration of Converter, GenericConverter, and Formatter beans.
1. // HttpMessageConverters
1. // SpringMVC 用来转换 Http 请求和响应的的，比如我们要把一个 User 对象转换为 JSON 字符串，可以去看官网文档解释；
1. Support for HttpMessageConverters (covered later in this document).
1. // 定义错误代码生成规则的
1. Automatic registration of MessageCodesResolver (covered later in this document).
1. // 首页定制
1. Static index.html support.
1. // 图标定制
1. Custom Favicon support (covered later in this document).
1. // 初始化数据绑定器：帮我们把请求数据绑定到 JavaBean 中！
1. Automatic use of a ConfigurableWebBindingInitializer bean (covered later in this document).

25
26 /\*

1. 如果您希望保留 Spring Boot MVC 功能，并且希望添加其他 MVC 配置（拦截器、格式化程序、视图控制器和其他功能），则可以添加自己
1. 的@configuration 类，类型为 webmvcconfiguer，但不添加@EnableWebMvc。如果希望提供
1. RequestMappingHandlerMapping、RequestMappingHandlerAdapter 或

ExceptionHandlerExceptionResolver 的自定义

1. 实例，则可以声明 WebMVCregistrationAdapter 实例来提供此类组件。

31 \*/

1.  If you want to keep Spring Boot MVC features and you want to add additional MVC configuration
1.      (interceptors, formatters, view controllers, and other features), you can add your own

1.      @Configuration class of type WebMvcConfigurer but without @EnableWebMvc. If you wish to provide
1.      custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or
1.      ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.

37

1. // 如果您想完全控制 Spring MVC，可以添加自己的@Configuration，并用@EnableWebMvc 进行注释。
1. If you want to take complete control of Spring MVC, you can add your own

@Configuration annotated with @EnableWebMvc.

我们来仔细对照，看一下它怎么实现的，它告诉我们 SpringBoot 已经帮我们自动配置好了 SpringMVC， 然后自动配置了哪些东西呢？

**ContentNegotiatingViewResolver 内容协商视图解析器**
自动配置了 ViewResolver，就是我们之前学习的 SpringMVC 的视图解析器；
即根据方法的返回值取得视图对象（View），然后由视图对象决定如何渲染（转发，重定向）。我们去看看这里的源码：我们找到 WebMvcAutoConﬁguration ， 然后搜索
ContentNegotiatingViewResolver。找到如下方法！

1. @Bean
1. @ConditionalOnBean(ViewResolver.class)
1. @ConditionalOnMissingBean(name = "viewResolver", value = ContentNegotiatingViewResolver.class)
1. public ContentNegotiatingViewResolver viewResolver(BeanFactory beanFactory)

{

1.      ContentNegotiatingViewResolver resolver = new ContentNegotiatingViewResolver();

6
resolver.setContentNegotiationManager(beanFactory.getBean(ContentNegotiatio nManager.class));

1. // ContentNegotiatingViewResolver 使用所有其他视图解析器来定位视图，因此它应该具

有较高的优先级

1. resolver.setOrder(Ordered.HIGHEST_PRECEDENCE);
1. return resolver; 10 }

我们可以点进这类看看！找到对应的解析视图的代码；

1. @Nullable // 注解说明：@Nullable 即参数可为 null
1. public View resolveViewName(String viewName, Locale locale) throws Exception

{

1.  RequestAttributes attrs = RequestContextHolder.getRequestAttributes();
1.      Assert.state(attrs instanceof ServletRequestAttributes, "No current ServletRequestAttributes");
1.      List<MediaType> requestedMediaTypes = this.getMediaTypes(((ServletRequestAttributes)attrs).getRequest());
1.  if (requestedMediaTypes != null) {
1.  // 获取候选的视图对象
1.      List<View> candidateViews = this.getCandidateViews(viewName, locale, requestedMediaTypes);
1.  // 选择一个最适合的视图对象，然后把这个对象返回

1.      View bestView = this.getBestView(candidateViews, requestedMediaTypes, attrs);
1.  if (bestView != null) {
1.  return bestView; 13 }

14 }
15 // .....
16 }

我们继续点进去看，他是怎么获得候选的视图的呢？
getCandidateViews 中看到他是把所有的视图解析器拿来，进行 while 循环，挨个解析！

| 1   | Iterator | var5 | =   | this.viewResolvers.iterator(); |
| --- | -------- | ---- | --- | ------------------------------ |

## 所以得出结论：ContentNegotiatingViewResolver 这个视图解析器就是用来组合所有的视图解析器的

我们再去研究下他的组合逻辑，看到有个属性 viewResolvers，看看它是在哪里进行赋值的！

1.  protected void initServletContext(ServletContext servletContext) {
1.  // 这里它是从 beanFactory 工具中获取容器中的所有视图解析器
1.  // ViewRescolver.class 把所有的视图解析器来组合的
1.      Collection<ViewResolver> matchingBeans = BeanFactoryUtils.beansOfTypeIncludingAncestors(this.obtainApplicationContext (), ViewResolver.class).values();
1.  ViewResolver viewResolver;
1.  if (this.viewResolvers == null) {
1.  this.viewResolvers = new ArrayList(matchingBeans.size()); 8 }

9 // ...............
10 }

既然它是在容器中去找视图解析器，我们是否可以猜想，我们就可以去实现一个视图解析器了呢？
我们可以自己给容器中去添加一个视图解析器；这个类就会帮我们自动的将它组合进来；**我们去实现一 下**
1、我们在我们的主程序中去写一个视图解析器来试试；

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
@Bean //放到 bean 中
public ViewResolver myViewResolver(){ return new MyViewResolver();
}
//我们写一个静态内部类，视图解析器就需要实现 ViewResolver 接口
private static class MyViewResolver implements ViewResolver{ @Override
public View resolveViewName(String s, Locale locale) throws Exception { return null;
}
}

2、怎么看我们自己写的视图解析器有没有起作用呢？
我们给 DispatcherServlet 中的 doDispatch 方法 加个断点进行调试一下，因为所有的请求都会走到这个方法中

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834634043-c7c80080-1699-44b6-ac86-a250176f3f83.png#)
3、我们启动我们的项目，然后随便访问一个页面，看一下 Debug 信息； 找到 this
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834634579-c721b31b-1d16-47f4-9e83-5e130b2698a4.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834635004-bd189a0f-33bf-4854-ade6-4d90f0fe8c38.jpeg#)找到视图解析器，我们看到我们自己定义的就在这里了；
所以说，我们如果想要使用自己定制化的东西，我们只需要给容器中添加这个组件就好了！剩下的事情
SpringBoot 就会帮我们做了

转换器和格式化器
找到格式化转换器：

1. @Bean
1. @Override
1. public FormattingConversionService mvcConversionService() {
1. // 拿到配置文件中的格式化规则
1. WebConversionService conversionService =
1. new WebConversionService(this.mvcProperties.getDateFormat());
1. addFormatters(conversionService);
1. return conversionService; 9 }

点击去：

1
2
3
4
5
6
7
8
public String getDateFormat() { return this.dateFormat;
}
/\*\*

- Date format to use. For instance, `dd/MM/yyyy`. 默认的
  \*/
  private String dateFormat;

可以看到在我们的 Properties 文件中，我们可以进行自动配置它！

如果配置了自己的格式化方式，就会注册到 Bean 中生效，我们可以在配置文件中配置日期格式化的规则：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834635514-221fafa8-5f10-42ec-99ce-33ed3aa12af7.png#)![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834635793-c2c3d1a4-ce3d-4891-997a-a8cb73bcc0ee.png#)

其余的就不一一举例了，大家可以下去多研究探讨即可！

修改 SpringBoot 的默认配置
这么多的自动配置，原理都是一样的，通过这个 WebMVC 的自动配置原理分析，我们要学会一种学习方式，通过源码探究，得出结论；这个结论一定是属于自己的，而且一通百通。
SpringBoot 的底层，大量用到了这些设计细节思想，所以，没事需要多阅读源码！得出结论；
SpringBoot 在自动配置很多组件的时候，先看容器中有没有用户自己配置的（如果用户自己配置@bean），如果有就用用户配置的，如果没有就用自动配置的；
如果有些组件可以存在多个，比如我们的视图解析器，就将用户配置的和自己默认的组合起来！
**扩展使用 SpringMVC **官方文档如下：
If you want to keep Spring Boot MVC features and you want to add additional MVC conﬁguration (interceptors, formatters, view controllers, and other features), you can add your own @Conﬁguration class of type WebMvcConﬁgurer but without @EnableWebMvc. If you wish to provide custom instances of RequestMappingHandlerMapping, RequestMappingHandlerAdapter, or ExceptionHandlerExceptionResolver, you can declare a WebMvcRegistrationsAdapter instance to provide such components.
我们要做的就是编写一个@Conﬁguration 注解类，并且类型要为 WebMvcConﬁgurer，还不能标注
@EnableWebMvc 注解；我们去自己写一个；我们新建一个包叫 conﬁg，写一个类 MyMvcConﬁg；

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
//应为类型要求为 WebMvcConfigurer，所以我们实现其接口
//可以使用自定义类扩展 MVC 的功能@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
@Override
public void addViewControllers(ViewControllerRegistry registry) {
// 浏览器发送/test ， 就会跳转到 test 页面；
registry.addViewController("/test").setViewName("test");
}
}

我们去浏览器访问一下：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834636107-954d77d8-e6cc-479b-aec7-885a7be9c16a.jpeg#)

## 确实也跳转过来了！所以说，我们要扩展 SpringMVC，官方就推荐我们这么去使用，既保 SpringBoot 留所有的自动配置，也能用我们扩展的配置！

我们可以去分析一下原理：
1、WebMvcAutoConﬁguration 是 SpringMVC 的自动配置类，里面有一个类 WebMvcAutoConﬁgurationAdapter
2、这个类上有一个注解，在做其他自动配置时会导入：

@Import(EnableWebMvcConfiguration.class)

3、我们点进 EnableWebMvcConﬁguration 这个类看一下，它继承了一个父类： DelegatingWebMvcConﬁguration
这个父类中有这样一段代码：

1. public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
1. private final WebMvcConfigurerComposite configurers = new

WebMvcConfigurerComposite();
3

1. // 从容器中获取所有的 webmvcConfigurer
1. @Autowired(required = false)
1. public void setConfigurers(List<WebMvcConfigurer> configurers) {
1. if (!CollectionUtils.isEmpty(configurers)) {
1. this.configurers.addWebMvcConfigurers(configurers); 9 }

10 }
11
}

4、我们可以在这个类中去寻找一个我们刚才设置的 viewController 当做参考，发现它调用了一个

1. protected void addViewControllers(ViewControllerRegistry registry) {
1. this.configurers.addViewControllers(registry); 3 }

5、我们点进去看一下

1. public void addViewControllers(ViewControllerRegistry registry) {
1. Iterator var2 = this.delegates.iterator(); 3

4
5
6
7
8
9
10
while(var2.hasNext()) {
// 将所有的 WebMvcConfigurer 相关配置来一起调用！包括我们自己配置的和 Spring 给我
们配置的
WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next(); delegate.addViewControllers(registry);
}
}

所以得出结论：所有的 WebMvcConﬁguration 都会被作用，不止 Spring 自己的配置类，我们自己的配置 类当然也会被调用；

全面接管 SpringMVC
官方文档：

| 1   | If you want to @Configuration | take complete control of Spring annotated with @EnableWebMvc. | MVC, | you | can | add | your | own |
| --- | ----------------------------- | ------------------------------------------------------------- | ---- | --- | --- | --- | ---- | --- |

全面接管即：SpringBoot 对 SpringMVC 的自动配置不需要了，所有都是我们自己去配置！ 只需在我们的配置类中要加一个@EnableWebMvc。
我们看下如果我们全面接管了 SpringMVC 了，我们之前 SpringBoot 给我们配置的静态资源映射一定会无 效，我们可以去测试一下；
不加注解之前，访问首页：
@EnableWebMvc
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834636607-8e061474-4ba3-4417-a28d-330d596fd72a.png#)
给配置类加上注解：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834637004-c7892938-876d-4f59-b5a0-543e45d99a96.jpeg#)
我们发现所有的 SpringMVC 自动配置都失效了！回归到了最初的样子；

## 当然，我们开发中，不推荐使用全面接管 SpringMVC

思考问题？为什么加了一个注解，自动配置就失效了！我们看下源码：
1、这里发现它是导入了一个类，我们可以继续进去看

1. @Import({DelegatingWebMvcConfiguration.class})
1. public @interface EnableWebMvc { 3 }

2、它继承了一个父类 WebMvcConﬁgurationSupport

1 public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport
{
2 // ......
3 }

3、我们来回顾一下 Webmvc 自动配置类

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
@Configuration(proxyBeanMethods = false) @ConditionalOnWebApplication(type = Type.SERVLET) @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
// 这个注解的意思就是：容器中没有这个组件的时候，这个自动配置类才生效
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class) @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10) @AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
}

总结一句话：@EnableWebMvc 将 WebMvcConﬁgurationSupport 组件导入进来了； 而导入的 WebMvcConﬁgurationSupport 只是 SpringMVC 最基本的功能！

## 在 SpringBoot 中会有非常多的扩展配置，只要看见了这个，我们就应该多留心注意~

**配置项目环境及首页**

把昨天的 mybatis 整合代码拿过来
1、导入依赖

1 <!-- lombok -->

1. <dependency>
1. <groupId>org.projectlombok</groupId>
1. <artifactId>lombok</artifactId>
1. </dependency> 6 <!-- 数据层 -->
1. <dependency>
1. <groupId>org.mybatis.spring.boot</groupId>
1. <artifactId>mybatis-spring-boot-starter</artifactId>
1. <version>2.1.1</version>
1. </dependency>
1. <dependency>
1. <groupId>mysql</groupId>
1. <artifactId>mysql-connector-java</artifactId>
1. <scope>runtime</scope>
1. </dependency>

2、导入实体类

1
2
3
4
5
6
7
@Data
public class Department {
private Integer id;
private String departmentName;
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
@Data
public class Employee {
private Integer id; private String lastName; private String email;
//1 male, 0 female private Integer gender;
private Integer department; private Date birth;

private Department eDepartment;
}

3、导入 mapp 接口以及对应的配置文件

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

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)">
<mapper namespace="com.kuang.mapper.DepartmentMapper">

<select id="getDepartments" resultType="Department"> select \* from department;
</select>

<select id="getDepartment" resultType="Department" parameterType="int"> select \* from department where id = #{id};
</select>

</mapper>

1.  <?xml version="1.0" encoding="UTF-8" ?>
1.  <!DOCTYPE mapper
1.  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
1.  ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)"> 5

6 <mapper namespace="com.kuang.mapper.EmployeeMapper"> 7

1.  <resultMap id="EmployeeMap" type="Employee">
1.  <id property="id" column="eid"/>
1.  <result property="lastName" column="last_name"/>
1.  <result property="email" column="email"/>
1.  <result property="gender" column="gender"/>
1.  <result property="birth" column="birth"/>
1.  <association property="eDepartment"	javaType="Department">
1.  <id property="id" column="did"/>
1.  <result property="departmentName" column="dname"/>
1.  </association>
1.  </resultMap> 19
1.  <select id="getEmployees" resultMap="EmployeeMap">
1.      select e.id as eid,last_name,email,gender,birth,d.id as did,d.department_name as dname
1.  from department d,employee e
1.  where d.id = e.department

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
</select>
<insert id="save" parameterType="Employee">
insert into employee (last_name,email,gender,department,birth) values (#{lastName},#{email},#{gender},#{department},#{birth});
</insert>

<select id="get" resultType="Employee"> select \* from employee where id = #{id}
</select>

<delete id="delete" parameterType="int"> delete from employee where id = #{id}
</delete>

</mapper>

4、注意 Maven 资源导出问题！

1. <resources>
1. <resource>
1. <directory>src/main/java</directory>
1. <includes>
1. <include>\*_/_.xml</include>
1. </includes>
1. <filtering>true</filtering>
1. </resource>
1. </resources>

导入静态资源
1、css，js 等放在 static 文件夹下
2、html 放在 templates 文件夹下最终结构如下：

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834637249-48b4e3c8-002b-47c7-9bb6-ca46acbfb899.png#)

首页实现
方式一：写一个 controller 实现！

1. //会解析到 templates 目录下的 index.html 页面
1. @RequestMapping({"/","/index.html"})
1. public String index(){
1. return "index"; 5 }

方式二：自己编写 MVC 的扩展配置

1. @Override
1. public void addViewControllers(ViewControllerRegistry registry) {
1. registry.addViewController("/").setViewName("index");
1. registry.addViewController("/index.html").setViewName("index"); 5 }

解决了首页问题，我们还需要解决一个资源导入的问题；
为了保证资源导入稳定，我们建议在所有资源导入时候使用 th:去替换原有的资源路径！这也是模板规范

1. <html lang="en" xmlns:th=["http://www.thymeleaf.org](http://www.thymeleaf.org/)">
1. <link th:href="@{/asserts/css/bootstrap.min.css}" rel="stylesheet">

ok，如果都替换完成了，我们的准备工作也就全部结束了！

# 页面国际化

有的时候，我们的网站会去涉及中英文甚至多语言的切换，这时候我们就需要学习国际化了！

准备工作
先在 IDEA 中统一设置 properties 的编码问题！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834637643-d50010e2-5568-4bdd-90a4-cc02ad268fca.png#)
编写国际化配置文件，抽取页面需要显示的国际化页面消息。我们可以去登录页面查看一下，哪些内容 我们需要编写国际化的配置！

配置文件编写
1、我们在 resources 资源文件下新建一个 i18n 目录，存放国际化配置文件
2、建立一个 login.properties 文件，还有一个 login_zh_CN.properties；发现 IDEA 自动识别了我们要做国际化操作；文件夹变了！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834637984-273b26c7-e8c6-4b35-83e7-da1dae864c53.png#)
3、我们可以在这上面去新建一个文件；

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834638287-cb9e5163-31ec-4d71-89ee-4d161f3fa42a.jpeg#)

弹出如下页面：我们再添加一个英文的；
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834638664-766d7ec5-c36c-48eb-bb0c-7223dd2a821f.png#)
这样就快捷多了！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834638867-18ef3624-57da-40be-a1ed-c5480806ee4d.png#)

## ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834639545-6e81d09a-df0e-4245-b3f8-e4e7ff899286.png#)4、接下来，我们就来编写配置，我们可以看到 idea 下面有另外一个视图；

这个视图我们点击 + 号就可以直接添加属性了；我们新建一个 login.tip，可以看到边上有三个文件框可以输入

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834639811-7a45015f-9dce-4a5b-9f93-d920fc50aafc.png#)
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834640175-22eee870-94c4-40ff-8a4a-04071cff5b87.png#)我们添加一下首页的内容！
然后依次添加其他页面内容即可！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834640705-d24756f3-a461-4c06-aa70-321d33b724c7.png#)

然后去查看我们的配置文件；
login.properties ： 默认

| 1   | login.btn=登录        |
| --- | --------------------- |
| 2   | login.password=密码   |
| 3   | login.remember=记住我 |
| 4   | login.tip=请登录      |
| 5   | login.username=用户名 |

英文：

| 1   | login.btn=Sign in          |
| --- | -------------------------- |
| 2   | login.password=Password    |
| 3   | login.remember=Remember me |
| 4   | login.tip=Please sign in   |
| 5   | login.username=Username    |

中文：

| 1   | login.btn=登录        |
| --- | --------------------- |
| 2   | login.password=密码   |
| 3   | login.remember=记住我 |
| 4   | login.tip=请登录      |
| 5   | login.username=用户名 |

OK，配置文件步骤搞定！

配置文件生效探究
我们去看一下 SpringBoot 对国际化的自动配置！这里又涉及到一个类：

MessageSourceAutoConfiguration

里面有一个方法，这里发现 SpringBoot 已经自动配置好了管理我们国际化资源文件的组件
ResourceBundleMessageSource；

1.  // 获取 properties 传递过来的值进行判断
1.  @Bean
1.  public MessageSource messageSource(MessageSourceProperties properties) {
1.      ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
1.  if (StringUtils.hasText(properties.getBasename())) {
1.  // 设置国际化文件的基础名（去掉语言国家代码的）
1.  messageSource.setBasenames(
1.  StringUtils.commaDelimitedListToStringArray( 9

StringUtils.trimAllWhitespace(properties.getBasename()))); 10 }

1. if (properties.getEncoding() != null) {
1. messageSource.setDefaultEncoding(properties.getEncoding().name()); 13 }

14
messageSource.setFallbackToSystemLocale(properties.isFallbackToSystemLocale ());

1. Duration cacheDuration = properties.getCacheDuration();
1. if (cacheDuration != null) {

17
18
19
messageSource.setCacheMillis(cacheDuration.toMillis());
}
messageSource.setAlwaysUseMessageFormat(properties.isAlwaysUseMessageFormat ());
20
messageSource.setUseCodeAsDefaultMessage(properties.isUseCodeAsDefaultMessa ge());
21 return messageSource;
22 }

我们真实 的情况是放在了 i18n 目录下，所以我们要去配置这个 messages 的路径；

1 spring.messages.basename=i18n.login

配置页面国际化值
去页面获取国际化的值，查看 Thymeleaf 的文档，找到 message 取值操作为： #{...}。我们去页面测试下：
IDEA 还有提示，非常智能的！
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834641265-62a78a77-84e9-4931-a0b3-0bc4452ae17f.png#)
我们可以去启动项目，访问一下，发现已经自动识别为中文的了！

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834641760-31624c92-94f8-4b9d-9f4b-8cb2ed1a40b0.png#)

## 但是我们想要更好！可以根据按钮自动切换中文英文！

配置国际化解析
在 Spring 中有一个国际化的 Locale （区域信息对象）；里面有一个叫做 LocaleResolver （获取区域信息对象）的解析器！
我们去我们 webmvc 自动配置文件，寻找一下！看到 SpringBoot 默认配置：

1.  @Bean
1.  @ConditionalOnMissingBean
1.  @ConditionalOnProperty(prefix = "spring.mvc", name = "locale")
1.  public LocaleResolver localeResolver() {
1.  // 容器中没有就自己配，有的话就用用户配置的
1.      if (this.mvcProperties.getLocaleResolver() == WebMvcProperties.LocaleResolver.FIXED) {
1.  return new FixedLocaleResolver(this.mvcProperties.getLocale()); 8 }

9 // 接收头国际化分解

1.      AcceptHeaderLocaleResolver localeResolver = new AcceptHeaderLocaleResolver();
1.  localeResolver.setDefaultLocale(this.mvcProperties.getLocale());
1.  return localeResolver; 13 }

AcceptHeaderLocaleResolver 这个类中有一个方法

1.  public Locale resolveLocale(HttpServletRequest request) {
1.  Locale defaultLocale = this.getDefaultLocale();
1.  // 默认的就是根据请求头带来的区域信息获取 Locale 进行国际化

1.      if (defaultLocale != null && request.getHeader("Accept-Language") == null) {
1.  return defaultLocale;
1.  } else {
1.  Locale requestLocale = request.getLocale();
1.  List<Locale> supportedLocales = this.getSupportedLocales();
1.  if (!supportedLocales.isEmpty() &&

!supportedLocales.contains(requestLocale)) {

1.      Locale supportedLocale = this.findSupportedLocale(request, supportedLocales);
1.  if (supportedLocale != null) {
1.  return supportedLocale;
1.  } else {
1.      return defaultLocale != null ? defaultLocale : requestLocale;

15 }

1. } else {
1. return requestLocale; 18 }

19 }
20 }

那假如我们现在想点击链接让我们的国际化资源生效，就需要让我们自己的 Locale 生效！ 我们去自己写一个自己的 LocaleResolver，可以在链接上携带区域信息！
修改一下前端页面的跳转连接：

| 1   | <!-- 这里传入参数不需要使用 ？	使用 （key=value）-->                   |
| --- | --------------------------------------------------------------------- |
| 2   | <a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>    |
| 3   | <a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a> |

我们去写一个处理的组件类！

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
package com.kuang.component;
import org.springframework.util.StringUtils;
import org.springframework.web.servlet.LocaleResolver;

import javax.servlet.http.HttpServletRequest; import javax.servlet.http.HttpServletResponse; import java.util.Locale;

//可以在链接上携带区域信息
public class MyLocaleResolver implements LocaleResolver {

//解析请求@Override
public Locale resolveLocale(HttpServletRequest request) {

String language = request.getParameter("l");
Locale locale = Locale.getDefault(); // 如果没有获取到就使用系统默认的
//如果请求链接不为空
if (!StringUtils.isEmpty(language)){
//分割请求参数
String[] split = language.split("\_");
//国家，地区
locale = new Locale(split[0],split[1]);

25
26
27
28
29
30
}
return locale;
}
@Override
public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {
31
32
33
}
}

为了让我们的区域化信息能够生效，我们需要再配置一下这个组件！在我们自己的 MvcConoﬁg 下添加
bean；

1. @Bean
1. public LocaleResolver localeResolver(){
1. return new MyLocaleResolver(); 4 }

## 我们重启项目，来访问一下，发现点击按钮可以实现成功切换！搞定收工！

**登录+拦截器**

禁用模板缓存
说明：页面存在缓存，所以我们需要禁用模板引擎的缓存

| 1   | #禁用模板缓存                |
| --- | ---------------------------- |
| 2   | spring.thymeleaf.cache=false |

模板引擎修改后，想要实时生效！页面修改完毕后，IDEA 小技巧 ： Ctrl + F9 重新编译！即可生效！

登录
我们这里就先不连接数据库了，输入任意用户名都可以登录成功！
1、我们把登录页面的表单提交地址写一个 controller！

| 1   | <form class="form-signin" th:action="@{/user/login}" method="post"> |
| --- | ------------------------------------------------------------------- |
| 2   | //这里面的所有表单标签都需要加上一个 name 属性                      |
| 3   |                                                                     |
| 4   | </form>                                                             |

2、去编写对应的 controller

1
2
3
4
5
6
7
8
@Controller
public class LoginController {
//@RequestMapping(value = "/user/login",method = RequestMethod.POST) @PostMapping("/user/login")
public String login(@RequestParam("username") String username,
@RequestParam("password") String password, Model model, HttpSession session){

| 9 |

} |

} |
if (!StringUtils.isEmpty(username) && "123456".equals(password)){
// 登 录 成 功 ！ 将 用 户 信 息 放 入 session session.setAttribute("loginUser",username); return "dashboard"; // 跳转到首页
}else {
//登录失败！存放错误信息 model.addAttribute("msg","用户名密码错误"); return "index";
} |
| --- | --- | --- | --- |
| 10 | | | |
| 11 | | | |
| 12 | | | |
| 13 | | | |
| 14 | | | |
| 15 | | | |
| 16 | | | |
| 17 | | | |
| 18 | | | |
| 19 | | | |
| 20 | | | |
| 21 | | | |
| 22 | | | |

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834642261-373f7214-1f86-4445-b14b-7d354e45c297.png#)OK ，测试登录成功！
3、登录失败的话，我们需要将后台信息输出到前台，可以在首页标题下面加上判断！

| 1   | <!--判断是否显示，使用if, ${}可以使用工具类，可以看thymeleaf的中文文档-->    |
| --- | ---------------------------------------------------------------------------- |
| 2   | <p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"> |
|     | </p>                                                                         |

重启登录失败测试：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834642761-2c332a9c-00bd-4e16-98c0-20fcca871a99.png#)

## 优化，登录成功后，由于是转发，链接不变，我们可以重定向到首页！

4、我们再添加一个视图控制映射，在我们的自己的 MyMvcConﬁg 中：

1 registry.addViewController("/main.html").setViewName("dashboard");

5、将 Controller 的代码改为重定向；

| 1   | //登录成功！防止表单重复提交，我们重定向 |
| --- | ---------------------------------------- |
| 2   | return "redirect:/main.html";            |

## 重启测试，重定向成功！后台主页正常显示！

登录拦截器
但是又发现新的问题，我们可以直接登录到后台主页，不用登录也可以实现！怎么处理这个问题呢？我 们可以使用拦截器机制，实现登录检查！
1、我们先自定义一个拦截器：

1.  public class LoginHandlerInterceptor implements HandlerInterceptor {
1.  @Override
1.      public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
1.  // 获取 loginUser 信息进行判断
1.  Object user = request.getSession().getAttribute("loginUser");
1.  if (user == null){ // 未登录，返回登录页面
1.  request.setAttribute("msg","没有权限，请先登录"); 8

request.getRequestDispatcher("/index.html").forward(request,response);
9 return false;

1. }else {
1. // 登录，放行
1. return true;

13 }
14 }
15 }

2、然后将拦截器注册到我们的 SpringMVC 配置类当中！

1. @Override
1. public void addInterceptors(InterceptorRegistry registry) {
1. // 注册拦截器，及拦截请求和要剔除哪些请求!
1. // 我们还需要过滤静态资源文件，否则样式显示不出来
1. registry.addInterceptor(new LoginHandlerInterceptor())
1. .addPathPatterns("/\*\*")
1. .excludePathPatterns("/index.html","/","/user/login","/asserts/\*\*"); 8 }

3、我们然后在后台主页，获取用户登录的信息

| 1   | <!--后台主页显示登录用户的信息--> |
| --- | --------------------------------- |
| 2   | [[${session.loginUser}]]          |

## 然后我们登录测试拦截！完美！

**员工列表实现**

RestFul 风格

**要求 ： 我们需要使用 Restful 风格实现我们的 crud 操作！**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834643247-c29b9afd-9ca3-4546-9f74-54a5a1777f3c.jpeg#)
**看看一些具体的要求，就是我们小实验的架构；**
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834643808-6e9f6a81-2840-4c28-b206-f63956992d34.jpeg#)
**我们根据这些要求，来完成第一个功能，就是我们的员工列表功能！**

员工列表页面跳转
我们在主页点击 Customers，就显示列表页面；我们去修改下
1、将首页的侧边栏 Customers 改为员工管理
2、a 链接添加请求

1 <a class="nav-link" th:href="@{/emps}">员工管理</a>

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834644280-41cc9a5f-0027-4b45-9263-ad601dac8257.png#)3、将 list 放在 emp 文件夹下
4、编写处理请求的 controller

1
2
3
4
5
6
7
8
@Controller
public class EmployeeController {
@Autowired
EmployeeMapper employeeMapper;

//查询所有员工，返回列表页面@GetMapping("/emps")

| 9 |

} | public String list(Model model){
List<Employee> employees = employeeMapper.getEmployees();
//将结果放在请求中 model.addAttribute("emps",employees); return "emp/list";
} |
| --- | --- | --- |
| 10 | | |
| 11 | | |
| 12 | | |
| 13 | | |
| 14 | | |
| 15 | | |
| 16 | | |

我们启动项目，测试一下看是否能够跳转，测试 OK！我们只需要将数据渲染进去即可！ 但是发现了一个问题，侧边栏和顶部都相同，我们是不是应该将它抽取出来呢？
**Thymeleaf 公共页面元素抽取**

## 步骤：

1、抽取公共片段 th:fragment 定义模板名
2、引入公共片段 th:insert 插入模板名

## 实现：

1、我们来抽取一下，使用 list 列表做演示！我们要抽取头部 nav 标签，我们在 dashboard 中将 nav 部分定 义一个模板名；

1.  <!-- 定义th:fragment="topbar" -->
1.  <nav th:fragment="topbar" class="navbar navbar-dark sticky-top bg-dark ">
1.  <!--后台主页显示登录用户的信息-->
1.  <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="#">
1.  [[${session.loginUser}]] 6 </a>
1.      <input class="form-control form-control-dark w-100" type="text" placeholder="Search" aria-label="Search">
1.  <ul class="navbar-nav px-3">
1.  <li class="nav-item text-nowrap">
1.  <a class="nav-link" href="#">Sign out</a> 11 </li>

12 </ul>
13 </nav>

2、然后我们在 list 页面中去引入，可以删掉原来的 nav

| 1   | <!--引入抽取的topbar-->                                 |
| --- | ------------------------------------------------------- |
| 2   | <!--模板名 ： 会使用 thymeleaf 的前后缀配置规则进行解析 |
| 3   | 使用~{模板::标签名}-->                                  |
| 4   | <div th:insert="~{dashboard::topbar}"></div>            |

3、启动再次测试，可以看到已经成功加载过来了！

## 说明：

除了使用 insert 插入，还可以使用 replace 替换，或者 include 包含，三种方式会有一些小区别，可以见名 知义；
我们使用 replace 替换，可以解决 div 多余的问题，可以查看 thymeleaf 的文档学习 侧边栏也是同理，当做练手，可以也同步一下！

定义模板：

1 <nav th:fragment="sitebar" class="col-md-2 d-none d-md-block bg-light sidebar">

然后我们在 list 页面中去引入：

1 <div th:insert="~{dashboard::sitebar}"></div>

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834644499-42f63bd7-2816-44ad-baad-d0adbb72f9ef.png#)启动再试试，看效果！
我们发现一个小问题，侧边栏激活的问题，它总是激活第一个；按理来说，这应该是动态的才对！ 为了重用更清晰，我们建立一个 commons 文件夹，专门存放公共页面；
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834644844-afeb159f-59d7-4a00-bdf7-38d22223a651.png#)
我们去页面中引入一下

| 1   | <div | th:replace="~{commons/bar::topbar}"></div>  |
| --- | ---- | ------------------------------------------- |
| 2   | <div | th:replace="~{commons/bar::sitebar}"></div> |

我们先测试一下，保证所有的页面没有出问题！ok！

## 侧边栏激活问题：

1、将首页的超链接地址改到项目中
2、我们在 a 标签中加一个判断，使用 class 改变标签的值；

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
<a class="nav-link active"
th:class="${activeUrl=='main.html'?'nav-link active':'nav-link'}" th:href="@{/main.html}">
首页
</a>
<a class="nav-link"
th:class="${activeUrl=='emps'?'nav-link active':'nav-link'}"
th:href="@{/emps}">员工管理
</a>

3、修改请求链接

| 1   | <div | th:replace="~{commons/bar::sitebar(activeUrl='main.html')}"></div> |
| --- | ---- | ------------------------------------------------------------------ |
| 2   | <div | th:replace="~{commons/bar::sitebar(activeUrl='emps')}"></div>      |

4、我们刷新页面，去测试一下，OK，动态激活搞定！

员工信息页面展示
现在我们来遍历我们的员工信息！顺便美化一些页面，增加添加，修改，删除的按钮！

1 <main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4"> 2 <!--添加员工按钮-->

1. <h2> <button class="btn btn-sm btn-success">添加员工</button></h2>
1. <div class="table-responsive">
1. <table class="table table-striped table-sm">
1. <thead>
1. <tr>
1. <th>id</th>
1. <th>lastName</th>
1. <th>email</th>
1. <th>gender</th>
1. <th>department</th>
1. <th>birth</th>
1. <!--我们还可以在显示的时候带一些操作按钮-->
1. <th>操作</th>

16 </tr>

1. </thead>
1. <tbody>
1. <tr th:each="emp:${emps}">
1. <td th:text="${emp.id}"></td>
1. <td>[[${emp.lastName}]]</td>
1. <td th:text="${emp.email}"></td>

23 <td th:text="${emp.gender==0?'女':'男'}"></td>
24 <td th:text="${emp.EDepartment.departmentName}"></td>
25 <!--<td th:text="${emp.birth}"></td>-->

1.  <!--使用时间格式化工具-->
1.      <td th:text="${#dates.format(emp.birth,'yyyy-MM-dd HH:mm')}"></td>

28
29 <!--操作-->

1. <td>
1. <button class="btn btn-sm btn-primary">编辑</button>
1. <button class="btn btn-sm btn-danger">删除</button> 33 </td>

34 </tr>

1. </tbody>
1. </table>
1. </div>
1. </main>

## OK，显示全部员工 OK！

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834645424-732e190b-2121-44f1-96a2-b3cb502c3784.jpeg#)

**添加员工实现**

表单及细节优化
1、将添加员工信息改为超链接

1 <!--添加员工按钮-->
2 <h2>
<a class="btn btn-sm btn-success" href="emp" th:href="@{/emp}">添加员工
</a>
4 </h2>
3

2、编写对应的 controller

1. //to 员工添加页面
1. @GetMapping("/emp")
1. public String toAddPage(){
1. return "emp/add"; 5 }

3、添加前端页面；复制 list 页面，修改即可
bootstrap 官网文档 ： [https://v4.bootcss.com/docs/4.0/components/forms/](https://v4.bootcss.com/docs/4.0/components/forms/)
我们去可以里面找自己喜欢的样式！我这里给大家提供了编辑好的：

1.  <form>
1.  <div class="form-group">
1.  <label>LastName</label>
1.  <input type="text" class="form-control" placeholder="kuangshen"> 5 </div>
1.  <div class="form-group">
1.  <label>Email</label>
1.      <input type="email" class="form-control" placeholder=["24736743@qq.com](mailto:24736743@qq.com)">
1.  </div>
1.  <div class="form-group">
1.  <label>Gender</label><br/>
1.  <div class="form-check form-check-inline">
1.      <input class="form-check-input" type="radio" name="gender" value="1">
1.  <label class="form-check-label">男</label>
1.  </div>
1.  <div class="form-check form-check-inline">

17
<input class="form-check-input" type="radio" name="gender"
value="0">

1. <label class="form-check-label">女</label>
1. </div>
1. </div>
1. <div class="form-group">
1. <label>department</label>
1. <select class="form-control">
1. <option>1</option>
1. <option>2</option>
1. <option>3</option>
1. <option>4</option>
1. <option>5</option>
1. </select>
1. </div>
1. <div class="form-group">
1. <label>Birth</label>
1. <input type="text" class="form-control" placeholder="kuangstudy">
1. </div>
1. <button type="submit" class="btn btn-primary">添加</button>
1. </form>

4、部门信息下拉框应该选择的是我们提供的数据，所以我们要修改一下前端和后端 Controller
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
@Autowired
DepartmentMapper departmentMapper;
//to 员工添加页面
@GetMapping("/emp")
public String toAddPage(Model model){
//查出所有的部门，提供选择
List<Department> departments = departmentMapper.getDepartments(); model.addAttribute("departments",departments);
return "emp/add";
}

前端

1.  <div class="form-group">
1.  <label>department</label>
1.  <!--提交的是部门的ID-->
1.  <select class="form-control">
1.      <option th:each="dept:${departments}" th:text="${dept.departmentName}" th:value="${dept.id}">1</option>
1.  </select>
1.  </div>

## OK，修改了 controller，重启项目测试！

完整增加员工功能，我们来具体实现添加功能；
1、修改 add 页面 form 表单提交地址和方式

1 <form th:action="@{/emp}" method="post">

2、编写 controller；

1
2
3
4
5
6
7
//员工添加功能，使用 post 接收
@PostMapping("/emp") public String addEmp(){
// 回到员工列表页面，可以使用 redirect 或者 forward，就不会被视图解析器解析
return "redirect:/emps";
}

回忆：重定向和转发 以及 /的问题？ 原理探究 ： ThymeleafViewResolver

| 1 | public static final String REDIRECT_URL_PREFIX = "redirect:"; public static final String FORWARD_URL_PREFIX = "forward:";

protected View createView(String viewName, Locale locale) throws
// 简单分析下源码
} |

Exception |

| {   |
| --- | --- | --- | --- |
| 2   |     |     |     |
| 3   |     |     |     |
| 4   |     |     |     |
| 5   |     |     |     |
| 6   |     |     |     |

OK，看完源码，我们继续编写代码！
3、我们要接收前端传过来的属性，将它封装成为对象！首先需要将前端页面空间的 name 属性编写完毕！【操作】
4、编写 controller 接收调试打印【操作】

1. //员工添加功能
1. //接收前端传递的参数，自动封装成为对象[要求前端传递的参数名，和属性名一致]
1. @PostMapping("/emp")
1. public String addEmp(Employee employee){
1. System.out.println(employee);
1. employeeDao.save(employee); //保存员工信息
1. //回到员工列表页面，可以使用 redirect 或者 forward
1. return "redirect:/emps"; 9 }

启动测试，前端填写数据，注意时间问题：

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834645929-e6de74f3-7109-4a77-8190-5f34c49d7473.png#)

## 点击提交，后台输出正常！页面跳转及数据显示正常！OK！ 那我们将时间换一个格式提交

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834646408-759249e0-9d25-47ef-b20b-7a54e5079e40.png#)
提交发现页面出现了 400 错误！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834646927-8255294a-69b7-4038-a4d6-8ee3da754877.jpeg#)

生日我们提交的是一个日期 ， 我们第一次使用的 / 正常提交成功了，后面使用 - 就错误了，所以这里面应该存在一个日期格式化的问题；
SpringMVC 会将页面提交的值转换为指定的类型，默认日期是按照 / 的方式提交 ； 比如将 2019/01/01 转换为一个 date 对象。
那思考一个问题？我们能不能修改这个默认的格式呢？
我们去看 webmvc 的自动配置文件；找到一个日期格式化的方法，我们可以看一下

1.  @Bean
1.  public FormattingConversionService mvcConversionService() {
1.      WebConversionService conversionService = new WebConversionService(this.mvcProperties.getDateFormat());
1.  this.addFormatters(conversionService);
1.  return conversionService; 6 }

调用了 getDateFormat 方法；

1. public String getDateFormat() {
1. return this.dateFormat; 3 }

这个在配置类中，所以我们可以自定义的去修改这个时间格式化问题，我们在我们的配置文件中修改一 下；

1 spring.mvc.date-format=yyyy-MM-dd

## 这样的话，我们现在就支持 - 的格式了，但是又不支持 / 了 ， 2333 吧

测试 OK！

# 员工信息修改

## 逻辑分析：

我们要实现员工修改功能，需要实现两步；
1、点击修改按钮，去到编辑页面，我们可以直接使用添加员工的页面实现
2、显示原数据，修改完毕后跳回列表页面！

实现
1、我们去实现一下，首先修改跳转链接的位置；

| 1   | <a  | class="btn | btn-sm | btn-primary" | th:href="@{/emp/}+${emp.id}">编辑</a> |
| --- | --- | ---------- | ------ | ------------ | ------------------------------------- |

2、编写对应的 controller

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
//to 员工修改页面
@GetMapping("/emp/{id}")
public String toUpdateEmp(@PathVariable("id") Integer id,Model model){
//根据 id 查出来员工
Employee employee = employeeMapper.get(id); System.out.println(employee);
//将员工信息返回页面
model.addAttribute("emp",employee);
//查出所有的部门，提供修改选择
List<Department> departments = departmentMapper.getDepartments(); model.addAttribute("departments",departments);
return "emp/update";
}

3、我们需要在这里将 add 页面复制一份，改为 update 页面；需要修改页面，将我们后台查询数据回显

1.  <form th:action="@{/emp}" method="post">
1.  <div class="form-group">
1.  <label>LastName</label>
1.      <input name="lastName" type="text" class="form-control" th:value="${emp.lastName}">
1.  </div>
1.  <div class="form-group">
1.  <label>Email</label>
1.      <input name="email" type="email" class="form-control" th:value="${emp.email}">
1.  </div>
1.  <div class="form-group">
1.  <label>Gender</label><br/>
1.  <div class="form-check form-check-inline">
1.      <input class="form-check-input" type="radio" name="gender" value="1"
1.  th:checked="${emp.gender==1}">
1.  <label class="form-check-label">男</label>
1.  </div>
1.  <div class="form-check form-check-inline">
1.      <input class="form-check-input" type="radio" name="gender" value="0"
1.  th:checked="${emp.gender==0}">
1.  <label class="form-check-label">女</label>
1.  </div>
1.  </div>
1.  <div class="form-group">
1.  <label>department</label>
1.  <!--提交的是部门的ID-->
1.  <select class="form-control" name="department">
1.      <option th:selected="${dept.id == emp.department}" th:each="dept:${departments}"
1.  th:text="${dept.departmentName}" th:value="${dept.id}">1
1.  </option>
1.  </select>
1.  </div>
1.  <div class="form-group">
1.  <label>Birth</label>
1.      <input name="birth" type="text" class="form-control" th:value="${emp.birth}">
1.  </div>
1.  <button type="submit" class="btn btn-primary">修改</button>
1.  </form>

测试 OK！
发现我们的日期显示不完美，可以使用日期工具，进行日期的格式化！

1 <input name="birth" type="text" class="form-control" th:value="${#dates.format(emp.birth,'yyyy-MM-dd HH:mm')}">

数据回显 OK，我们继续完成数据修改问题！
4、修改表单提交的地址：

1 <form th:action="@{/updateEmp}" method="post">

5、编写对应的 controller

1. @PostMapping("/updateEmp")
1. public String updateEmp(Employee employee){
1. employeeMapper.update(employee);
1. //回到员工列表页面
1. return "redirect:/emps"; 6 }

6、编写 Mapper 接口及对应的 xml 文件

| 1   | // 修改员工信息                |
| --- | ------------------------------ |
| 2   | int update(Employee employee); |

1. <update id="update" parameterType="Employee">
1. update employee
1. set last_name = #{lastName},email=#{email},gender=#{gender},department=#

{department},birth=#{birth}

1. where id = #{id} ;
1. </update>

编写完毕后，启动测试！
问题：发现页面提交的没有 id；我们在前端加一个隐藏域，提交 id；

| 1   | <input | name="id" | type="hidden" | class="form-control" | th:value="${emp.id}"> |
| --- | ------ | --------- | ------------- | -------------------- | --------------------- |

重启，修改信息测试 OK！

# 删除员工实现

1、list 页面，编写提交地址

| 1   | <a  | class="btn | btn-sm | btn-danger" | th:href="@{/delEmp/}+${emp.id}">删除</a> |
| --- | --- | ---------- | ------ | ----------- | ---------------------------------------- |

2、编写 Controller

1. @GetMapping("/delEmp/{id}")
1. public String delEmp(@PathVariable("id") Integer id){
1. employeeMapper.delete(id);
1. return "redirect:/emps"; 5 }

测试 OK！

# 404 及注销

404
我们只需要在模板目录下添加一个 error 文件夹，文件夹中存放我们相应的错误页面； 比如 404.html 或者 4xx.html 等等，SpringBoot 就会帮我们自动使用了！

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834647299-c07b145c-eb74-4ea3-9404-df9387db4f46.png#)
测试使用！

注销
1、注销请求

| 1   | <a  | class="nav-link" | href="#" | th:href="@{/user/loginOut}">Sign | out</a> |
| --- | --- | ---------------- | -------- | -------------------------------- | ------- |

2、对应的 controller

1. @GetMapping("/user/loginOut")
1. public String loginOut(HttpSession session){
1. session.invalidate();
1. return "redirect:/index.html"; 5 }

相信大家学到这里，SpringBoot 的单体应用开发基本就没有问题了！
但是还是那句话，学会技术只是浅层次的东西，要消化理解我的思想方法，这才是最有价值的点！

# 定制错误数据

SpringBoot 默认的错误处理机制
1、浏览器访问的默认的错误处理效果：
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834647606-f3ecf6c7-94e8-49c4-813a-641ce207f642.jpeg#)
2、如果是其他客户端，默认响应一个 json 数据；

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834647982-ccbbf7ac-1bb6-4888-8af4-9a85735f32cc.jpeg#)

错误处理原理分析
我们看到自动配置类：ErrorMvcAutoConﬁguration 错误处理的自动配置类； 这里面注入了几个很重要的 bean；
1、DefaultErrorAttributes
2、BasicErrorController
3、ErrorPageCustomizer
4、DefaultErrorViewResolver

## 错误处理步骤：

一旦系统出现了 4xx 或者 5xx 之类的错误，ErrorPageCustomizer 就会生效（定制错误的响应规则）

1.  @Bean
1.  public ErrorPageCustomizer errorPageCustomizer(DispatcherServletPath dispatcherServletPath) {
1.  // 点进这个类
1.      return new ErrorPageCustomizer(this.serverProperties, dispatcherServletPath);

5 }

发现一个方法 registerErrorPages 注册错误页面

1. @Override
1. public void registerErrorPages(ErrorPageRegistry errorPageRegistry) {
1. ErrorPage errorPage = new ErrorPage(
1. // 这里有个 getPath() 路径，我们点进去

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
this.dispatcherServletPath.getRelativePath(this.properties.getError().getPa th()));
errorPageRegistry.addErrorPages(errorPage);
}
// getPath
public String getPath() { return this.path;
}

// this.path; @Value("${error.path:/error}") private String path = "/error";

系统一旦出现错误之后就会来到 /error 请求进行处理；这个请求会被 BasicErrorController 处理：

| 1   | @Controller                                                         |
| --- | ------------------------------------------------------------------- |
| 2   | // 处理默认的 /error 请求                                           |
| 3   | @RequestMapping("${server.error.path:${error.path:/error}}")        |
| 4   | public class BasicErrorController extends AbstractErrorController { |
| 5   |                                                                     |
| 6   | }                                                                   |

这个类有两个方法：

1.  // 产生 html 类型的数据，浏览器发送的请求会被这个方法处理
1.  @RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
1.  public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
1.  HttpStatus status = getStatus(request);
1.  Map<String, Object> model = Collections
1.      .unmodifiableMap(getErrorAttributes(request, isIncludeStackTrace(request, MediaType.TEXT_HTML)));
1.  response.setStatus(status.value());
1.  // 去哪个页面拿错误页面呢？resolveErrorView 方法
1.      ModelAndView modelAndView = resolveErrorView(request, response, status, model);
1.      return (modelAndView != null) ? modelAndView : new ModelAndView("error", model);

11 }
12

1. // 返回 json 类型的数据，其他的客户端请求会被这个方法处理
1. @RequestMapping
1. public ResponseEntity<Map<String, Object>> error(HttpServletRequest request)

{

1.  HttpStatus status = getStatus(request);
1.  if (status == HttpStatus.NO_CONTENT) {
1.  return new ResponseEntity<>(status); 19 }
1.      Map<String, Object> body = getErrorAttributes(request, isIncludeStackTrace(request, MediaType.ALL));
1.  return new ResponseEntity<>(body, status); 22 }

我们来看看 resolveErrorView 这个方法：

1.  protected ModelAndView resolveErrorView(HttpServletRequest request,
1.  HttpServletResponse response,
1.  HttpStatus status,
1.  Map<String, Object> model) {
1.  // 拿到所有的 errorViewResolvers 错误视图解析器
1.  for (ErrorViewResolver resolver : this.errorViewResolvers) {
1.      ModelAndView modelAndView = resolver.resolveErrorView(request, status, model);
1.  if (modelAndView != null) {
1.  return modelAndView; 10 }

11 }
12 return null; 13 }

我们在之前看到有这样一个 bean DefaultErrorViewResolver 默认的错误视图解析器 :

1 public class DefaultErrorViewResolver implements ErrorViewResolver, Ordered
{
2
3 private static final Map<Series, String> SERIES_VIEWS; 4

1. static {
1. Map<Series, String> views = new EnumMap<>(Series.class);
1. views.put(Series.CLIENT_ERROR, "4xx"); // 客户端错误
1. views.put(Series.SERVER_ERROR, "5xx"); // 服务端错误
1. SERIES_VIEWS = Collections.unmodifiableMap(views); 10 }

11 // .....
12

1.  @Override // HttpStatus 状态码
1.      public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model) {
1.      ModelAndView modelAndView = resolve(String.valueOf(status.value()), model);
1.      if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
1.  // 通过状态码解析视图
1.      modelAndView = resolve(SERIES_VIEWS.get(status.series()), model);

19 }
20 return modelAndView; 21 }
22

1. // 去 error 路径下解析视图
1. private ModelAndView resolve(String viewName, Map<String, Object> model)

{

1.  // 比如 error/404 error/500
1.  String errorViewName = "error/" + viewName;
1.      TemplateAvailabilityProvider provider = this.templateAvailabilityProviders.getProvider(errorViewName,
1.  this.applicationContext);
1.  if (provider != null) {
1.  return new ModelAndView(errorViewName, model); 31 }

32 return resolveResource(errorViewName, model); 33 }
34
35 }

所以说：定制错误页面，我们可以建立一个 error 目录，然后放入对应的错误码 html 文件！ 比 如 ：404.html 500.html 4xx.html 5xx.html
这些页面的信息数据在哪里呢？我们找到 DefaultErrorAttributes 这个 bean 对象； 里面有很多的 addxx 方法，就是添加不同的信息；

| 1   | //  | addStatus                                              |
| --- | --- | ------------------------------------------------------ |
| 2   | //  | addErrorDetails                                        |
| 3   | //  | addErrorMessage                                        |
| 4   | //  | addStackTrace                                          |
| 5   | //  | addPath                                                |
| 6   |     |                                                        |
| 7   | //  | 这里面存了一些错误的信息，我们可以在错误页面直接取出来 |

到了这里，我们的 SpringBoot 开发一个简单的单体应用对我们来说就没什么太大的问题了！
