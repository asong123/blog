---
title: 26、SpringBoot操作数据库
urlname: ru1hng
date: '2021-07-09 20:43:38 +0800'
tags: []
categories: []
---

# SpringData 简介

对于数据访问层，无论是 SQL(关系型数据库) 还是 NOSQL(非关系型数据库)，Spring Boot 底层都是采用 Spring Data 的方式进行统一处理。
Spring Boot 底层都是采用 Spring Data 的方式进行统一处理各种数据库，Spring Data 也是 Spring 中与 Spring Boot、Spring Cloud 等齐名的知名项目。
Sping Data 官网：[https://spring.io/projects/spring-data ](https://spring.io/projects/spring-data)数据库相关的启动器 ： 可以参考官方文档：
[https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter](https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter)

**集成 JDBC**

导入测试数据库

| 1   | CREATE DATABASE /_!32312 IF NOT EXISTS_/`springboot` /\*!40100 DEFAULT  |
| --- | ----------------------------------------------------------------------- |
|     | CHARACTER SET utf8 \*/;                                                 |
| 2   |                                                                         |
| 3   | USE `springboot`;                                                       |
| 4   |                                                                         |
| 5   | /_Table structure for table `department` _/                             |
| 6   |                                                                         |
| 7   | DROP TABLE IF EXISTS `department`;                                      |
| 8   |                                                                         |
| 9   | CREATE TABLE `department` (                                             |
| 10  | `id` int(3) NOT NULL AUTO_INCREMENT COMMENT '部门 id',                  |
| 11  | `department_name` varchar(20) NOT NULL COMMENT '部门名字',              |
| 12  | PRIMARY KEY (`id`)                                                      |
| 13  | ) ENGINE=InnoDB AUTO_INCREMENT=106 DEFAULT CHARSET=utf8;                |
| 14  |                                                                         |
| 15  | /_Data for the table `department` _/                                    |
| 16  |                                                                         |
| 17  | insert into `department`(`id`,`department_name`) values (101,'技术部'), |
|     | (102,'销售部'),(103,'售后部'),(104,'后勤部'),(105,'运营部');            |
| 18  |                                                                         |
| 19  | /_Table structure for table `employee` _/                               |
| 20  |                                                                         |
| 21  | DROP TABLE IF EXISTS `employee`;                                        |
| 22  |                                                                         |
| 23  | CREATE TABLE `employee` (                                               |
| 24  | `id` int(5) NOT NULL AUTO_INCREMENT COMMENT '雇员 id',                  |
| 25  | `last_name` varchar(100) NOT NULL COMMENT '名字',                       |
| 26  | `email` varchar(100) NOT NULL COMMENT '邮箱',                           |
| 27  | `gender` int(2) NOT NULL COMMENT '性别 1 男, 0 女',                     |
| 28  | `department` int(3) NOT NULL COMMENT '部门 id',                         |
| 29  | `birth` datetime NOT NULL COMMENT '生日',                               |
| 30  | PRIMARY KEY (`id`)                                                      |
| 31  | ) ENGINE=InnoDB AUTO_INCREMENT=1006 DEFAULT CHARSET=utf8;               |
| 32  |                                                                         |

| 33  | /_Data for the table `employee` _/                                        |
| --- | ------------------------------------------------------------------------- |
| 34  |                                                                           |
| 35  | insert into                                                               |
|     | `employee`(`id`,`last_name`,`email`,`gender`,`department`,`birth`) values |
|     | (1001,'张三','24736743@qq.com',1,101,'2020-03-06 15:04:33'),(1002,'李     |
|     | 四','24736743@qq.com',1,102,'2020-03-06 15:04:36'),(1003,'王              |
|     | 五','24736743@qq.com',0,103,'2020-03-06 15:04:37'),(1004,'赵              |
|     | 六','24736743@qq.com',1,104,'2020-03-06 15:04:39'),(1005,'孙              |
|     | 七','24736743@qq.com',0,105,'2020-03-06 15:04:45');                       |
| 36  |                                                                           |

创建测试项目测试数据源
1、我去新建一个项目测试：springboot-data-jdbc ; 引入相应的模块！基础模块
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834620049-72f11b3e-b77f-48c8-9714-b7355608e366.png#)
2、项目建好之后，发现自动帮我们导入了如下的启动器：

1. <dependency>
1. <groupId>org.springframework.boot</groupId>
1. <artifactId>spring-boot-starter-jdbc</artifactId>
1. </dependency>
1. <dependency>
1. <groupId>mysql</groupId>
1. <artifactId>mysql-connector-java</artifactId>
1. <scope>runtime</scope>
1. </dependency>

3、编写 yaml 配置文件连接数据库；

1.  spring:
1.  datasource:
1.  username: root
1.  password: 123456
1.  #?serverTimezone=UTC 解决时区的报错
1.      url: jdbc:mysql://localhost:3306/springboot? serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
1.  driver-class-name: com.mysql.cj.jdbc.Driver

4、配置完这一些东西后，我们就可以直接去使用了，因为 SpringBoot 已经默认帮我们进行了自动配置；去测试类测试一下

| 1   | @SpringBootTest                            |
| --- | ------------------------------------------ |
| 2   | class SpringbootDataJdbcApplicationTests { |
| 3   |                                            |

| 4 |

} | //DI 注入数据源@Autowired
DataSource dataSource;

@Test
public void contextLoads() throws SQLException {
//看一下默认数据源 System.out.println(dataSource.getClass());
//获得连接
Connection connection = dataSource.getConnection(); System.out.println(connection);
//关闭连接
connection.close();
} |
| --- | --- | --- |
| 5 | | |
| 6 | | |
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

结果：我们可以看到他默认给我们配置的数据源为 : class com.zaxxer.hikari.HikariDataSource ， 我们并没有手动配置
我们来全局搜索一下，找到数据源的所有自动配置都在 ：DataSourceAutoConﬁguration 文件：

1.  @Import(
1.      {Hikari.class, Tomcat.class, Dbcp2.class, Generic.class, DataSourceJmxConfiguration.class}

3 )

1. protected static class PooledDataSourceConfiguration {
1. protected PooledDataSourceConfiguration() { 6 }

7 }

这里导入的类都在 DataSourceConﬁguration 配置类下，可以看出 Spring Boot 2.2.5 默认使用
HikariDataSource 数据源，而以前版本，如 Spring Boot 1.5 默认使用
org.apache.tomcat.jdbc.pool.DataSource 作为数据源；
**HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat**
**jdbc 等连接池更加优秀；**
**可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。**

关于数据源我们并不做介绍，有了数据库连接，显然就可以 CRUD 操作数据库了。但是我们需要先了解一个对象
JdbcTemplate

JdbcTemplate
1、有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库；
2、即使不使用第三方第数据库操作框架，如 MyBatis 等，Spring 本身也对原生的 JDBC 做了轻量级的封
JdbcTemplate
装，即 。
3、数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。
4、Spring Boot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用
5、JdbcTemplate 的自动配置是依赖 org.springframework.boot.autoconﬁgure.jdbc 包下的
JdbcTemplateConﬁguration 类
**JdbcTemplate 主要提供以下几类方法：**
execute 方法：可以用于执行任何 SQL 语句，一般用于执行 DDL 语句；
update 方法及 batchUpdate 方法：update 方法用于执行新增、修改、删除等语句；batchUpdate
方法用于执行批处理相关语句；
query 方法及 queryForXXX 方法：用于执行查询相关语句；
call 方法：用于执行存储过程、函数相关语句。

测试
编写一个 Controller，注入 jdbcTemplate，编写测试方法进行访问测试；

1 package com.kuang.controller; 2

1. import org.springframework.beans.factory.annotation.Autowired;
1. import org.springframework.jdbc.core.JdbcTemplate;
1. import org.springframework.web.bind.annotation.GetMapping;
1. import org.springframework.web.bind.annotation.PathVariable;
1. import org.springframework.web.bind.annotation.RequestMapping;
1. import org.springframework.web.bind.annotation.RestController; 9
1. import java.util.Date;
1. import java.util.List;
1. import java.util.Map; 13
1. @RestController
1. @RequestMapping("/jdbc")
1. public class JdbcController { 17

18 /\*\*

1. - Spring Boot 默认提供了数据源，默认提供了

org.springframework.jdbc.core.JdbcTemplate

1. - JdbcTemplate 中会自己注入数据源，用于简化 JDBC 操作
1. - 还能避免一些常见的错误,使用起来也不用再自己来关闭数据库连接

22 \*/

1. @Autowired
1. JdbcTemplate jdbcTemplate; 25
1. //查询 employee 表中所有数据
1. //List 中的 1 个 Map 对应数据库的 1 行数据
1. //Map 中的 key 对应数据库的字段名，value 对应数据库的字段值
1. @GetMapping("/list")
1. public List<Map<String, Object>> userList(){
1. String sql = "select \* from employee";
1. List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
1. return maps; 34 }

35

1. //新增一个用户
1. @GetMapping("/add")
1. public String addUser(){
1. //插入语句，注意时间问题

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
64
65
66
67
68
69
70
71
String sql = "insert into employee(last_name, email,gender,department,birth)" +
" values ('狂神说','24736743@qq.com',1,101,'"+ new
Date().toLocaleString() +"')"; jdbcTemplate.update(sql);
//查询
return "addOk";
}
//修改用户信息
@GetMapping("/update/{id}")
public String updateUser(@PathVariable("id") int id){
//插入语句
String sql = "update employee set last_name=?,email=? where id="+id;
//数据
Object[] objects = new Object[2]; objects[0] = "秦疆";
objects[1] = ["24736743@sina.com](mailto:24736743@sina.com)"; jdbcTemplate.update(sql,objects);
//查询
return "updateOk";
}

//删除用户@GetMapping("/delete/{id}")
public String delUser(@PathVariable("id") int id){
//插入语句
String sql = "delete from employee where id=?"; jdbcTemplate.update(sql,id);
//查询
return "deleteOk";
}
}

到此，CURD 的基本操作，使用 JDBC 就搞定了。

**集成 Druid**

Druid 简介
Java 程序很大一部分要操作数据库，为了提高性能操作数据库的时候，又不得不使用数据库连接池。
Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控。
Druid 可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。
Druid 已经在阿里巴巴部署了超过 600 个应用，经过一年多生产环境大规模部署的严苛考验。
Spring Boot 2.0 以上默认使用 Hikari 数据源，可以说 Hikari 与 Driud 都是当前 Java Web 上最优秀的数据源，我们来重点介绍 Spring Boot 如何集成 Druid 数据源，如何实现数据库监控。
Github 地址：[https://github.com/alibaba/druid/](https://github.com/alibaba/druid/)
**com.alibaba.druid.pool.DruidDataSource 基本配置参数如下：**

| **配置** | **缺省值** | **说明** |
| -------- | ---------- | -------- |

|
name | | 配置这个属性的意义在于，如果存在多个数据源，监控的时候可以通过名字来区分开来。 如果没有配置，将会生成一 个 名 字 ， 格 式 是 ："DataSource-" + System.identityHashCode(this). |
|
url | | 连接数据库的 url，不同数据库不一样。例如： mysql : jdbc:mysql://10.20.153.104:3306/druid2 oracle : jdbc:oracle:thin:@10.20.149.85:1521:ocnauto |
| username | | 连接数据库的用户名 |
| password | | 连接数据库的密码。如果你不希望密码直接写在配置文件中，可以使用 ConﬁgFilter。 |
| driverClassName | 根据 url 自动识别 | 这一项可配可不配，如果不配置 druid 会根据 url 自动识别
dbType，然后选择相应的 driverClassName |
| initialSize | 0 | 初始化时建立物理连接的个数。初始化发生在显示调用 init 方法，或者第一次 getConnection 时 |
| maxActive | 8 | 最大连接池数量 |
| maxIdle | 8 | 已经不再使用，配置了也没效果 |
| minIdle | | 最小连接池数量 |
|
maxWait | | 获取连接时最大等待时间，单位毫秒。配置了 maxWait 之 后，缺省启用公平锁，并发效率会有所下降，如果需要可以通过配置 useUnfairLock 属性为 true 使用非公平锁。 |
|
poolPreparedStatements |
false | 是否缓存 preparedStatement，也就是 PSCache。
PSCache 对支持游标的数据库性能提升巨大，比如说
oracle。在 mysql 下建议关闭。 |
|
maxOpenPreparedStatements |
-1 | 要启用 PSCache，必须配置大于 0，当大于 0 时，
poolPreparedStatements 自动触发修改为 true。在 Druid 中，不会存在 Oracle 下 PSCache 占用内存过多的问题，可以把这个数值配置大一些，比如说 100 |
|
validationQuery | | 用来检测连接是否有效的 sql，要求是一个查询语句。如果
validationQuery 为 null，testOnBorrow、
testOnReturn、testWhileIdle 都不会其作用。 |
|
validationQueryTimeout | | 单位：秒，检测连接是否有效的超时时间。底层调用 jdbc
Statement 对象的 void setQueryTimeout(int seconds)方法 |
| testOnBorrow | true | 申请连接时执行 validationQuery 检测连接是否有效，做了这个配置会降低性能。 |
| testOnReturn | false | 归还连接时执行 validationQuery 检测连接是否有效，做了这个配置会降低性能 |
|
testWhileIdle |
false | 建议配置为 true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于
timeBetweenEvictionRunsMillis，执行 validationQuery
检测连接是否有效。 |
|
timeBetweenEvictionRunsMillis |
1 分钟
（1.0.14） | 有两个含义： 1) Destroy 线程会检测连接的间隔时间，如果连接空闲时间大于等于 minEvictableIdleTimeMillis 则关闭物理连接 2) testWhileIdle 的判断依据，详细看
testWhileIdle 属性的说明 |

| **配置**                   | **缺省值**                       | **说明**                                              |
| -------------------------- | -------------------------------- | ----------------------------------------------------- |
| numTestsPerEvictionRun     |                                  | 不再使用，一个 DruidDataSource 只支持一个 EvictionRun |
| minEvictableIdleTimeMillis | 30 分钟                          |
| （1.0.14）                 | 连接保持空闲而不被驱逐的最长时间 |
| connectionInitSqls         |                                  | 物理连接初始化的时候执行的 sql                        |

|
exceptionSorter | 根据
dbType 自动识别 |
当数据库抛出一些不可恢复的异常时，抛弃连接 |
|
ﬁlters | | 属性类型是字符串，通过别名的方式配置扩展插件，常用的插件有： 监控统计用的 ﬁlter:stat 日志用的 ﬁlter:log4j 防御 sql 注入的 ﬁlter:wall |
| proxyFilters | | 类型是 List<com.alibaba.druid.ﬁlter.Filter>，如果同时配置了 ﬁlters 和 proxyFilters，是组合关系，并非替换关系 |

配置数据源
1、添加上 Druid 数据源依赖。

1. <!-- [https://mvnrepository.com/artifact/com.alibaba/druid](https://mvnrepository.com/artifact/com.alibaba/druid) -->
1. <dependency>
1. <groupId>com.alibaba</groupId>
1. <artifactId>druid</artifactId>
1. <version>1.1.21</version>
1. </dependency>

2、切换数据源；之前已经说过 Spring Boot 2.0 以上默认使用 com.zaxxer.hikari.HikariDataSource 数据源，但可以 通过 spring.datasource.type 指定数据源。

1.  spring:
1.  datasource:
1.  username: root
1.  password: 123456
1.      url: jdbc:mysql://localhost:3306/springboot? serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
1.  driver-class-name: com.mysql.cj.jdbc.Driver
1.  type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源

3、数据源切换之后，在测试类中注入 DataSource，然后获取到它，输出一看便知是否成功切换；
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834620320-53b0bd68-a9aa-4432-9350-f4b77f771dd3.jpeg#)
4、切换成功！既然切换成功，就可以设置数据源连接初始化大小、最大连接数、等待时间、最小连接数 等设置项；可以查看源码

1.  spring:
1.  datasource:
1.  username: root
1.  password: 123456
1.  #?serverTimezone=UTC 解决时区的报错

    1.      url: jdbc:mysql://localhost:3306/springboot? serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    1.  driver-class-name: com.mysql.cj.jdbc.Driver
    1.  type: com.alibaba.druid.pool.DruidDataSource 9

1.  #Spring Boot 默认是不注入这些属性值的，需要自己绑定
1.  #druid 数据源专有配置
1.  initialSize: 5
1.  minIdle: 5
1.  maxActive: 20
1.  maxWait: 60000
1.  timeBetweenEvictionRunsMillis: 60000
1.  minEvictableIdleTimeMillis: 300000
1.  validationQuery: SELECT 1 FROM DUAL
1.  testWhileIdle: true
1.  testOnBorrow: false
1.  testOnReturn: false
1.  poolPreparedStatements: true 23
1.  #配置监控统计拦截的 filters，stat:监控统计、log4j：日志记录、wall：防御 sql 注入
1.      #如果允许时报错	java.lang.ClassNotFoundException: org.apache.log4j.Priority
1.  #则导入 log4j 依赖即可，Maven 地址：

[https://mvnrepository.com/artifact/log4j/log4j](https://mvnrepository.com/artifact/log4j/log4j)

1.  filters: stat,wall,log4j
1.  maxPoolPreparedStatementPerConnectionSize: 20
1.  useGlobalDataSourceStat: true
1.      connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500

5、导入 Log4j 的依赖

1. <!-- [https://mvnrepository.com/artifact/log4j/log4j](https://mvnrepository.com/artifact/log4j/log4j) -->
1. <dependency>
1. <groupId>log4j</groupId>
1. <artifactId>log4j</artifactId>
1. <version>1.2.17</version>
1. </dependency>

6、现在需要程序员自己为 DruidDataSource 绑定全局配置文件中的参数，再添加到容器中，而不再使用 Spring Boot 的自动生成了；我们需要 自己添加 DruidDataSource 组件到容器中，并绑定属性；

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
package com.kuang.config;
import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.boot.context.properties.ConfigurationProperties; import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class DruidConfig {

/\*
将自定义的 Druid 数据源添加到容器中，不再让 Spring Boot 自动创建绑定全局配置文件中的 druid 数据源属性到
com.alibaba.druid.pool.DruidDataSource 从而让它们生效

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
@ConfigurationProperties(prefix = "spring.datasource")：作用就是将 全局
配置文件中
前缀为 spring.datasource 的属性值注入到
com.alibaba.druid.pool.DruidDataSource 的同名参数中
\*/
@ConfigurationProperties(prefix = "spring.datasource") @Bean
public DataSource druidDataSource() { return new DruidDataSource();
}
}

7、去测试类中测试一下；看是否成功！

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
@SpringBootTest
class SpringbootDataJdbcApplicationTests {
//DI 注入数据源
@Autowired
DataSource dataSource;
@Test
public void contextLoads() throws SQLException {
//看一下默认数据源 System.out.println(dataSource.getClass());
//获得连接
Connection connection =
dataSource.getConnection();
System.out.println(connection);
18
DruidDataSource druidDataSource = (DruidDataSource) dataSource;
System.out.println("druidDataSource 数据源最大连接数：" + druidDataSource.getMaxActive());
System.out.println("druidDataSource 数据源初始化连接数：" +
druidDataSource.getInitialSize());
19
20
21
22
23
//关闭连接
connection.close();
}
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834620610-efebe126-8268-44cb-862d-3b9dc87c5566.png#)输出结果 ：可见配置参数已经生效！
配置 Druid 数据源监控

Druid 数据源具有监控的功能，并提供了一个 web 界面方便用户查看，类似安装 路由器 时，人家也提供了一个默认的 web 页面。
所以第一步需要设置 Druid 的后台管理页面，比如 登录账号、密码 等；配置后台管理；

1.  //配置 Druid 监控管理后台的 Servlet；
1.  //内置 Servlet 容器时没有 web.xml 文件，所以使用 Spring Boot 的注册 Servlet 方式
1.  @Bean
1.  public ServletRegistrationBean statViewServlet() {
1.      ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");

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
// 这些参数可以在 com.alibaba.druid.support.http.StatViewServlet
// 的父类 com.alibaba.druid.support.http.ResourceServlet 中找到 Map<String, String> initParams = new HashMap<>(); initParams.put("loginUsername", "admin"); //后台管理界面的登录账号 initParams.put("loginPassword", "123456"); //后台管理界面的登录密码
//后台允许谁可以访问
//initParams.put("allow", "localhost")：表示只有本机可以访问
//initParams.put("allow", "")：为空或者为 null 时，表示允许所有访问 initParams.put("allow", "");
//deny：Druid 后台拒绝谁访问
//initParams.put("kuangshen", "192.168.1.20");表示禁止此 ip 访问

//设置初始化参数 bean.setInitParameters(initParams); return bean;
}

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834620890-c60216f1-ad37-437a-837e-896be2a1f536.png#)配置完毕后，我们可以选择访问 ： http://localhost:8080/druid/login.html
进入之后
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834621164-a887f8bc-0dd3-4bc0-a2c2-0bbc95def3ee.png#)
**配置 Druid web 监控 ﬁlter 过滤器**

| 1   | //配置 | Druid | 监控 | 之  | web | 监控的 | filter |
| --- | ------ | ----- | ---- | --- | --- | ------ | ------ |

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
//WebStatFilter：用于配置 Web 和 Druid 数据源之间的管理关联监控统计
@Bean
public FilterRegistrationBean webStatFilter() { FilterRegistrationBean bean = new FilterRegistrationBean(); bean.setFilter(new WebStatFilter());
//exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
Map<String, String> initParams = new HashMap<>(); initParams.put("exclusions", "_.js,_.css,/druid/_,/jdbc/_"); bean.setInitParameters(initParams);

//"/_" 表示过滤所有请求 bean.setUrlPatterns(Arrays.asList("/_")); return bean;
}

平时在工作中，按需求进行配置即可，主要用作监控！

**整合 MyBatis**

官方文档：[http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconﬁgure/](http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)
[Maven 仓库地址：https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot](https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.1)
[-starter/2.1.1](https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.1.1)

整合测试
**1、导入 MyBatis 所需要的依赖**

1. <dependency>
1. <groupId>org.mybatis.spring.boot</groupId>
1. <artifactId>mybatis-spring-boot-starter</artifactId>
1. <version>2.1.1</version>
1. </dependency>

**2、配置数据库连接信息（不变）**

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
spring: datasource:
username: root password: 123456
#?serverTimezone=UTC 解决时区的报错
url: jdbc:mysql://localhost:3306/springboot? serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
driver-class-name: com.mysql.cj.jdbc.Driver
type: com.alibaba.druid.pool.DruidDataSource
#Spring Boot 默认是不注入这些属性值的，需要自己绑定
#druid 数据源专有配置
initialSize: 5
minIdle: 5
maxActive: 20
maxWait: 60000

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
timeBetweenEvictionRunsMillis: 60000
minEvictableIdleTimeMillis: 300000 validationQuery: SELECT 1 FROM DUAL testWhileIdle: true
testOnBorrow: false testOnReturn: false
poolPreparedStatements: true
26
27
28
29
30 #配置监控统计拦截的 filters，stat:监控统计、log4j：日志记录、wall：防御 sql 注入 #如果允许时报错 java.lang.ClassNotFoundException: org.apache.log4j.Priority #则导入 log4j 依赖即可，Maven 地址：
[https://mvnrepository.com/artifact/log4j/log4j](https://mvnrepository.com/artifact/log4j/log4j) filters: stat,wall,log4j maxPoolPreparedStatementPerConnectionSize: 20 useGlobalDataSourceStat: true connectionProperties:
druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500

**3、测试数据库是否连接成功！**
**4、创建实体类，导入 Lombok！**
Department.java

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
package com.kuang.pojo;
import lombok.AllArgsConstructor; import lombok.Data;
import lombok.NoArgsConstructor;
@Data @NoArgsConstructor @AllArgsConstructor
public class Department {
private Integer id;
private String departmentName;
}

**5、创建 mapper 目录以及对应的 Mapper 接口**
DepartmentMapper.java

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
//@Mapper : 表示本类是一个 MyBatis 的 Mapper
@Mapper @Repository
public interface DepartmentMapper {
// 获取所有部门信息
List<Department> getDepartments();

// 通过 id 获得部门
Department getDepartment(Integer id);
}

**6、对应的 Mapper 映射文件**
DepartmentMapper.xml

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

**7、maven 配置资源过滤问题**

1. <resources>
1. <resource>
1. <directory>src/main/java</directory>
1. <includes>
1. <include>\*_/_.xml</include>
1. </includes>
1. <filtering>true</filtering>
1. </resource>
1. </resources>

**既然已经提供了 myBatis 的映射配置文件，自然要告诉 spring boot 这些文件的位置**

| 1   | #指定 myBatis 的核心配置文件与 Mapper 映射文件           |
| --- | -------------------------------------------------------- |
| 2   | mybatis.mapper-locations=classpath:mybatis/mapper/\*.xml |
| 3   | # 注意：对应实体类的路径                                 |
| 4   | mybatis.type-aliases-package=com.kuang.mybatis.pojo      |

8、编写部门的 DepartmentController 进行测试！\*\*

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
@RestController
public class DepartmentController {
@Autowired
DepartmentMapper departmentMapper;

// 查询全部部门
@GetMapping("/getDepartments")
public List<Department> getDepartments(){ return departmentMapper.getDepartments();
}

// 查询全部部门
@GetMapping("/getDepartment/{id}")
public Department getDepartment(@PathVariable("id") Integer id){

| 16 |

} |
} | return departmentMapper.getDepartment(id); |
| --- | --- | --- | --- |
| 17 | | | |
| 18 | | | |
| 19 | | | |

**启动项目访问进行测试！**

我们增加一个员工类再测试下，为之后做准备
1、新建一个 pojo 类 Employee ；

| 1 | @Data @AllArgsConstructor @NoArgsConstructor public class Employee {

private Integer id; private String lastName; private String email;
//1 male, 0 female private Integer gender;
private Integer department; private Date birth;

private Department eDepartment;

} |

// |

| 冗余设计 |
| -------- | --- | --- | --- |
| 2        |     |     |     |
| 3        |     |     |     |
| 4        |     |     |     |
| 5        |     |     |     |
| 6        |     |     |     |
| 7        |     |     |     |
| 8        |     |     |     |
| 9        |     |     |     |
| 10       |     |     |     |
| 11       |     |     |     |
| 12       |     |     |     |
| 13       |     |     |     |
| 14       |     |     |     |
| 15       |     |     |     |
| 16       |     |     |     |

2、新建一个 EmployeeMapper 接口

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
//@Mapper : 表示本类是一个 MyBatis 的 Mapper
@Mapper @Repository
public interface EmployeeMapper {
// 获取所有员工信息
List<Employee> getEmployees();

// 新增一个员工
int save(Employee employee);

// 通过 id 获得员工信息
Employee get(Integer id);

// 通过 id 删除员工
int delete(Integer id);
}

3、编写 EmployeeMapper.xml 配置文件

1. <?xml version="1.0" encoding="UTF-8" ?>
1. <!DOCTYPE mapper
1. PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
1. ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)">

5
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
1.  </select> 25
1.  <insert id="save" parameterType="Employee">
1.  insert into employee (last_name,email,gender,department,birth)
1.  values (#{lastName},#{email},#{gender},#{department},#{birth});
1.  </insert> 30
1.  <select id="get" resultType="Employee">
1.  select \* from employee where id = #{id}
1.  </select> 34
1.  <delete id="delete" parameterType="int">
1.  delete from employee where id = #{id}
1.  </delete> 38

39 </mapper>

4、编写 EmployeeController 类进行测试

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
@RestController
public class EmployeeController {
@Autowired
EmployeeMapper employeeMapper;

// 获取所有员工信息
@GetMapping("/getEmployees")
public List<Employee> getEmployees(){ return employeeMapper.getEmployees();
}

@GetMapping("/save") public int save(){
Employee employee = new Employee(); employee.setLastName("kuangshen"); employee.setEmail(["qinjiang@qq.com](mailto:qinjiang@qq.com)"); employee.setGender(1);
employee.setDepartment(101);

| 20 |

} | employee.setBirth(new Date());
return employeeMapper.save(employee);
}

// 通过 id 获得员工信息
@GetMapping("/get/{id}")
public Employee get(@PathVariable("id") Integer id){ return employeeMapper.get(id);
}

// 通过 id 删除员工
@GetMapping("/delete/{id}")
public int delete(@PathVariable("id") Integer id){ return employeeMapper.delete(id);
} |
| --- | --- | --- |
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
| 36 | | |

测试结果完成，搞定收工
