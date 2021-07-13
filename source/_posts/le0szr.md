---
title: 分布式微服务架构之SpringCloud基础篇
urlname: le0szr
date: '2021-07-12 14:19:02 +0800'
author: 阿松
top: true
hide: false
cover: false
toc: false
mathjax: false
summary: title: 一种架构风格，将项目的不同功能拆分成一个个模块，每个模块都能独立运行，彼此间通过远程调用实现功能。
categories: SpringCloud
tags:
  - SpringCloud
  - 后端
  - 微服务
  - java
---

# 一，微服务相关概念

## 1.微服务与分布式

**微服务理论**：一种架构风格，将项目的不同功能拆分成一个个模块，每个模块都能独立运行，彼此间通过远程调用实现功能。

**分布式概念**：很多建立在网络上的软件系统的集合。

**分布式与集群的关系**：     集群指的是将几台服务器集中在一起，实现同一个业务。

分布式中的每一个节点都可以做集群。而集群并不一定就是分布式的。

## 2.软件架构风格

**单一应用架构**：         网站流量比较小，将所有功能部署在一起，减少部署节点和成本。

（书城系统）

**垂直应用架构**：         当访问量逐渐增大，将一个应用拆成互不相关的几个应用，以提升效率。

（前台系统，后台系统）

**分布式服务架构**：         将每个核心业务抽取出来，作为单独的模块，彼此间通过远程调用实现功能。

（订单模块，用户模块...）

## 3.远程调用

`RPC`：远程调用的一种思想。

服务之间的交互可以采用两种方式：

1.`RPC`        Netty+自定义序列化

2.`RestAPI（cloud）`        HTTP+JSON

## 4.分布式思想的基本概念

```properties
    <--
    高并发：
        1.概念：指在同一个时间点，有很多用户同时的访问同一 API 接口或者 Url 地址
        2.衡量指标：
            1)一次http请求返回所用时间
            2)吞吐量：系统在单位时间内处理的请求的数量。
            3）QPS,TPS
            4)每秒查询数，每秒事务数
            5）系统本身能承载的用户数量
    高可用：
        服务集群部署，数据库主从复制+双机热备。
            1.主备方式：一台服务器激活，另一台备用。
            2.双主机：两种不同业务分别在两台服务器互为主备。
    注册中心：
        保存某个服务所在地址等信息，方便调用者实时获取其他服务信息。
        服务注册：服务提供者
        服务发现：服务消费者
    负载均衡：
        1.概念：动态将请求派发给比较闲的服务器。
        2.策略：轮询，加权轮询，随机，哈希，最小连接数，最短响应时间。
    服务血崩：
        服务之间复杂调用，一个服务不可用，导致整个系统受影响不可用。
        正常流程：A->B->C->D->C->B->A，由于D服务down了，所以整个服务都不能用了。
        此时采用的解决办法：采用熔断机制，返回兜底数据或方法。
    限流：
        限制某个服务每秒调用本服务的频率（防爬虫）
    API网关：
        系统后端总入口，承载着所有服务的组合路由转换等工作，
        安全，限流，缓存，日志，监控，重试，熔断。
    服务跟踪：
        追踪服务调用链，记录整个系统执行请求过程。
    弹性云：
        弹性计算服务，动态扩容，压榨服务器闲时能力。
        （高峰期时多配制一些服务器，平时较少多余的服务器配置）-->
```

## 5.学习资源

附件：中文学习文档（https://www.bookstack.cn/read/spring-cloud-docs/docs-index.md）
`Springboot`和`SpringCloud`版本选择
`SpringBoot2.X`版和`SpringCloud H`版
`SpringCloudAlibaba2.1`

---

# 二，项目搭建与 RestTemplate 远程调用

## 1.需求说明

创建父工程，创建子模块 8001，然后 80 模块通过`RestTemplate`实现对 8001 的远程调用

`RestTemplate`
`RestTemplate`提供了多种便捷访问远程 Http 服务的方法，是一种简单便捷的访问 Restful
服务模板类，是 Spring 提供的用于访问 Rest 服务的客户端模板工具集。
使用：
简单粗暴（url,requestMap,ResponseBean.class）
REST 请求地址，请求参数，Http 响应转换被转换成的对象类型

## 2.创建父工程（cloud0623）

打包方式为`pom`，并导入依赖

```xml
    <!-- 统一管理jar包版本 -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <junit.version>4.12</junit.version>
        <log4j.version>1.2.17</log4j.version>
        <lombok.version>1.16.18</lombok.version>
        <mysql.version>5.1.47</mysql.version>
        <druid.version>1.1.16</druid.version>
        <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
    </properties>
    <!-- 子模块继承之后，提供作用：锁定版本+子modlue不用写groupId和version  -->
    <dependencyManagement>
        <dependencies>
            <!--spring boot 2.2.2-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.2.2.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud Hoxton.SR1-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR1</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud alibaba 2.1.0.RELEASE-->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.1.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${mybatis.spring.boot.version}</version>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
            </dependency>
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>${log4j.version}</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
                <optional>true</optional>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                    <addResources>true</addResources>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

## 3.创建公共模块（model-common）

### 1.导入依赖

```xml
    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
```

### 2.创建公用实体类

```java
/**
 * @author yinhuidong
 * @createTime 2020-06-23-21:13
 */
@Data
public class Payment implements Serializable {
    private static final long serialVersionUID = -6849794470754667666L;
    private Long id;
    private String serial;

}
```

### 3.统一返回结果集

```java
/**
 * @author yinhuidong
 * @createTime 2020-06-23-21:16
 */
@Data
public class CommonResult<T> implements Serializable {

    private Integer code;
    private String message;
    private T data;


    public static CommonResult error(){
        CommonResult<Object> result = new CommonResult<>();
        result.setCode(20001);
        result.setMessage(Message.ERROR.toString());
        return result;
    }
    public static CommonResult ok(){
        CommonResult<Object> result = new CommonResult<>();
        result.setCode(20000);
        result.setMessage(Message.SUCCESS.toString());
        return result;
    }
    public static CommonResult okWithMessage(Object data){
        CommonResult<Object> result = new CommonResult<>();
        result.setCode(20000);
        result.setMessage(Message.SUCCESS.toString());
        return result;
    }
}
```

### 4.创建数据库

```mysql
CREATE DATABASE  IF NOT EXISTS cloud0623 DEFAULT CHARACTER SET utf8 ;

USE cloud0623 ;

DROP TABLE IF EXISTS payment ;

CREATE TABLE payment (
                       id BIGINT (20) NOT NULL AUTO_INCREMENT COMMENT 'ID',
                       SERIAL VARCHAR (300) DEFAULT NULL,
                       PRIMARY KEY (id)
) ENGINE = INNODB AUTO_INCREMENT = 33 DEFAULT CHARSET = utf8 ;

INSERT INTO payment (id, SERIAL) VALUES(1, '尹会东'),(2, '马云') ;
```

## 4.创建服务提供者 8001 模块（model-8001）

### 1.导入依赖

```xml
    <dependencies>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控系统健康状况-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <!--热部署依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>
```

### 2.编写主启动类

```java
@SpringBootApplication
public class Model8001 {
    public static void main(String[] args) {
        SpringApplication.run(Model8001.class,args);
    }
}
```

### 3.编写配置文件

```yml
server:
  port: 8001
spring:
  application:
    name: model-8001
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/cloud0623?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: root
mybatis:
  mapper-locations: classpath:/mapper/*.xml
  type-aliases-package: com.example.entities
```

### 4.编写配置类

```java
@SpringBootConfiguration
@MapperScan("com.example.mapper")
public class MybatisConfig {
}
```

### 5.编写 mapper 接口

```java
@Repository
public interface PaymentMapper {

    void save(Payment payment);

    Payment selectById(Long id);
}
```

### 6.编写 mybatis 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.mapper.PaymentMapper">
    <insert id="save"  useGeneratedKeys="true" keyProperty="id">
        insert into payment(serial) values(#{serial});
    </insert>

    <resultMap id="BaseResultMap" type="com.example.entities.Payment">
        <id column="id" property="id" jdbcType="BIGINT"></id>
        <result column="serial" property="serial" jdbcType="VARCHAR"></result>
    </resultMap>

    <select id="selectById"  parameterType="Long" resultMap="BaseResultMap">
        select * from payment where id=#{id}
    </select>

</mapper>
```

### 7.service

```java
public interface PaymentService {
    void save(Payment payment);

    Payment selectById(Long id);
}
```

```java
@Service
public class PaymentServiceImpl implements PaymentService {
    @Autowired
    private PaymentMapper mapper;
    @Override
    public void save(Payment payment) {
        mapper.save(payment);
    }

    @Override
    public Payment selectById(Long id) {
        return mapper.selectById(id);
    }
}
```

### 8.controller

```java
@RestController
@Slf4j
public class PaymentController {

    @Autowired
    private PaymentService service;

    @PostMapping("/save")
    public CommonResult<Payment> save(Payment payment){
        service.save(payment);
        return CommonResult.ok();
    }
    @GetMapping("/selectById/{id}")
    public Payment selectById(@PathVariable Long id){
        Payment payment = service.selectById(id);

        return payment;
    }
}
```

## 5.创建服务调用者 80 模块（model-80）

### 1.导入依赖

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
```

### 2.编写配置文件

```yml
server:
  port: 80
spring:
  application:
    name: model-80
```

### 3.编写主启动类

```java
@SpringBootApplication
public class Model80 {
    public static void main(String[] args) {
        SpringApplication.run(Model80.class,args);
    }
}
```

### 4.配置类

```java
@SpringBootConfiguration
public class ApplicationConfig {

    @Bean
    //@LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

### 5.控制器

```java
@RestController
@Slf4j
public class M80controller {
    private String url="http://localhost:8001";
    @Autowired
    private RestTemplate restTemplate;
    @PostMapping("/80/save")
    public CommonResult<Payment>   create(Payment payment){
        return restTemplate.postForObject(url+"/save",payment, CommonResult.class);  //写操作
    }

    @GetMapping("/80/selectById/{id}")
    public Payment getPayment(@PathVariable("id") Long id){
        return restTemplate.getForObject(url+"/selectById/"+id,Payment.class);
    }

}
```

## 6.远程调用测试

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2020100617204386.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=55561f926a7c3090e4f53f9e13f3fd8589c756577537922b5fcf44d18a087546#from=url&id=XgRY1&margin=%5Bobject%20Object%5D&originHeight=489&originWidth=600&originalType=binary∶=1&status=done&style=none)

**服务提供者接口方法需要增加@RequestBody 注解(踩雷 or 破雷)；否则，接收不到数据。**

---

# 三，Eureka 服务注册与发现

Eureka 1.他解决的问题：
传统的 RPC 远程调用框架中，管理每个服务与服务之间依赖关系比较复杂、所以需要进行服务治理，
管理服务与服务之间依赖关联，以实现服务调用，负载均衡、容错等，实现服务发现与注册。 2.系统中的其他微服务使用 Eureka 的客户端连接到 Eureka Server 并维持心跳连接。便于运维监控，
和解决远程调用的依赖。
在简单配置以后，当服务器启动，会把自己的信息注册到注册中心上。
服务调用者通过服务提供者的名字到注册中心去获取实际的 URL，然后再实现本地 RPC 远程调用。
3.Eureka 的两个组件
Eureka Server 提供服务注册服务，保存服务注册的信息
Eureka Client 通过注册中心进行访问，如果一个 Client3 个 30 秒没发心跳
给 Server，Server 就当他死了，把它移除。

## 1.创建 Eureka 服务端模块（Eureka-7001）

### 1.导入依赖

```xml
    <dependencies>
        <!--Eureka所需依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        <!--Eureka所需依赖-->
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
    </dependencies>
```

### 2.编写配置文件

```yml
server:
  port: 7001
eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://localhost:7001/eureka
```

### 3.编写主启动类

```java
@SpringBootApplication
@EnableEurekaServer
public class Eureka7001 {
    public static void main(String[] args) {
        SpringApplication.run(Eureka7001.class,args);
    }
}
```

### 4.测试 Eureka 服务端

[http://localhost:7001/](http://localhost:7001/)

## 2.将服务提供者注册进 Eureka（model-8001）

### 1.导入依赖

```xml
<!--Eureka所需依赖-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    <version>2.2.1.RELEASE</version>
</dependency>
<!--Eureka所需依赖-->
```

### 2.修改配置文件

```yml
eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
```

### 3.主启动类上加注解

```java
@EnableEurekaClient
```

### 4.测试

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006172547338.jpg%23pic_center&sign=f9c8a76e3d51967cd37a2d7e5f01c6fd1abebdd98866123b723c45298c68423a#from=url&id=cXbdo&margin=%5Bobject%20Object%5D&originHeight=163&originWidth=1360&originalType=binary∶=1&status=done&style=none)

## 3.同理将服务调用者（model-80）注册进入 Eureka

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006172608972.jpg%23pic_center&sign=a95377c6daff9f42b1d57bf787937142847a5c35c7771c4b8a8139e6259c5264#from=url&id=kCHA5&margin=%5Bobject%20Object%5D&originHeight=146&originWidth=1354&originalType=binary∶=1&status=done&style=none)

---

# 四，Ribbon 负载均衡服务调用

## Ribbon

### 1.一套基于客户端的负载均衡工具

### 2.可以自定义负载均衡算法

### 3.作用

负载均衡

### 4.与 nginx 的区别：

nginx 是服务器负载均衡，客户端的请求都交给 nginx，然后，由 nginx 实现转发请求。
ribbon 本地负载均衡，在调用微服务接口的时候，会在注册中心上获取注册信息服务列表
之后缓存到 JVM 本地，从而在本地实现 RPC 远程服务调用。

### 5.进程内的负载均衡

总结：ribbon=负载均衡+RestTemplate 远程调用

### 6.工作流程：

1)先选择同一个区域内负载较少的 EurekaServer。 2)根据用户指定策略，在从 server 拿到的服务注册列表选一个地址。
本身提供多种策略：轮询，随机，根据响应时间加权。

### 7.依赖

```xml
    <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
    </dependency>
```

注意：这个不需要手动引用，Eureka 客户端自带 Ribbon

### 8.RestTemplate 的两个方法

getForObject()返回对象为 JSON
    getForEntity()返回对象为 ResponseEntity 对象

## 自定义轮询策略

为了达到演示负载均衡的目的，创建 module-8002 与 model-8001 完全相同，就是访问端口号不同。

**在服务调用端（model-80）配置**

### 1.配置类

**自定义配置类不能放在@ComponentScan 所扫描的当前包下以及子包下，否则我们自定义的这个配置类就会被所有的 Ribbon 客户端所共享，达不到特殊化订制的目的了。**

```java
package com.fibbon;
import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
@Configuration
public class FibbonConfig {

    @Bean
    public IRule myRule(){
        return new RandomRule();
    }
}
```

### 2.主启动类

```java
//name指定的是它要调用的微服务的名字，配置类
@RibbonClient(name = "model-8001",configuration = FibbonConfig.class)
@EnableEurekaClient
@SpringBootApplication
public class Model80 {
    public static void main(String[] args) {
        SpringApplication.run(Model80.class,args);
    }
}
```

### 3.修改 Controller

```java
private String url="http://model-8001";
```

### 4.修改原来的配置类

```java
@SpringBootConfiguration
public class ApplicationConfig {

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

### 5.测试

[http://localhost:80/80/selectById/2](http://localhost:80/80/selectById/2)

---

# 五，OpenFeign 服务接口调用

```properties
1.是啥？
    Feign是一个声明式的web服务客户端，让编写web服务客户端更容易，
    只需要创建一个接口并在接口上添加注解即可。
    SpringCloud对Feign进行了封装，使其支持了SpringMVC标准注解和HttpMessageConverters。
    Feign可以与Eureka和Ribbon组合使用以支持负载均衡。
2.能干啥？
    使javaHttp客户端变得更简单。
    前面使用的Ribbon+RestTemplate进行远程调用
    实际上，由于服务以来的调用可能不止一处，往往一个接口会被多处调用，
    所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务端调用。
    所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。
    我们只需要创建一个接口，使用注解来配置它就可以完成对服务提供方的接口绑定。
    Feign集成了Ribbon
```

## 1.改造服务调用者 80 模块

### 1.导入依赖

```xml
        <!--openFeign所需要的依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
            <version>2.2.1.RELEASE</version>
        </dependency>
        <!--openFeign所需要的依赖-->
```

### 2.主启动类加注解

```java
@EnableFeignClients
```

### 3.创建接口

```java
package com.example.feign;

import com.example.CommonResult;
import com.example.entities.Payment;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;

/**
 * @author yinhuidong
 * @createTime 2020-06-24-10:17
 */
@Component
@FeignClient(name = "model-8001")
public interface Model8001 {
    @GetMapping("/selectById/{id}")
    Payment getPayment(@PathVariable("id") Long id);
    @PostMapping("/save")
    CommonResult<Payment> save(Payment payment);
}
```

### 4.改造 Controller

```java
/**
 * @author yinhuidong
 * @createTime 2020-06-23-23:38
 */
@RestController
@Slf4j
public class M80controller {
    @Autowired
    private Model8001 model8001;

    @PostMapping("/80/save")
    public CommonResult<Payment> create(Payment payment) {
        return model8001.save(payment);  //写操作
    }

    @GetMapping("/80/selectById/{id}")
    public Payment getPayment(@PathVariable("id") Long id) {
        return model8001.getPayment(id);
    }

}
```

## 2.测试：[http://localhost:80/80/selectById/2](http://localhost:80/80/selectById/2)

## 3.OpenFeign 超时控制

**超时设置，故意设置超时演示出错情况**

### 1.服务提供方 8001 故意写暂停程序

​

```java
    @GetMapping("/timeout")
    public String testTimeOut(){
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return serverPort;
    }
```

### 2.服务调用方 80 的 feign 接口添加对应方法

```java
    @GetMapping("/timeout")
    public String testTimeOut();
```

### 3.服务调用方 80 控制层

```java
    @GetMapping("/80/timeout")
    public String testTimeOut(){
        return model8001.testTimeOut();
    }
```

### 4.测试

[http://localhost:80/80/timeout](http://localhost:80/80/timeout)

错误页面，OpenFeign 默认等待一秒钟，超过后报错

### 5.超时控制概念

默认 Feign 客户端只等待一秒钟，但是，服务端处理需要超过 1 秒钟，导致 Feign 客户端不想等待了，直接报错。
     为了避免这样的情况，有时候我们需要设置 Feign 客户端的超时控制，也即 Ribbon 的超时时间，因为 Feign 集成了 Ribbon 进行负载均衡。

### 6.配置

在 80 端的配置文件配置

```yml
#设置Feign客户端超时时间（openFeign默认支持ribbon）
ribbon:
  #指的是建立连接所用的时间
  ReadTimeout: 6000
  #指的是建立连接以后从服务端读取到资源所用时间
  ConnectTimeout: 6000
```

### 7.测试

[http://localhost:80/80/timeout](http://localhost:80/80/timeout)

不在报错了

## 4.OpenFeign 日志打印功能

```properties
1.Feign提供了日志打印功能，我们可以对Feign接口的调用情况进行监控和输出。
2.日志级别：
    NONE：默认的，不显示任何日志
    BASIC：仅记录请求方法、RUL、响应状态码及执行时间
    HEADERS：除了BASIC中定义的信息之外，还有请求和响应的头信息
    FULL：除了HEADERS中定义的信息之外，还有请求和响应的正文及元数据
```

### 1.在 80 服务端配置日志 bean

```java
@SpringBootConfiguration
public class FeignConfig {
    @Bean
    public Logger.Level feignLoggerLevel(){
        return Logger.Level.FULL;
    }
}
```

### 2.在 80 配置文件添加

```yml
#给指定的远程调用接口设置日志级别
logging:
  level:
    com.example.feign.Model8001: debug
```

### 3.测试

[http://localhost:80/80/selectById/2](http://localhost:80/80/selectById/2)

---

# 六，Hystrix 断路器

```properties
简单点说，A调用B，B挂了，就得给A返回兜底的方法或者数据。
Hystrix是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，
许多依赖不可避免的会调用失败，比如超时，异常等。
Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，
以提高分布式系统的弹性。
“断路器本身是一种开关装置”，当某个服务单元发生故障之后，通过断路器的故障监控
，向调用方返回一个符合预期的，可处理的备选响应（Fallback），而不是长时间的等待
或者抛出调用方无法处理的异常，这样就保证了服务调用方的线程不会被长时间，不必要的
占用，从而避免了故障在分布式系统的蔓延，乃至雪崩。
作用：
    服务降级
    服务熔断
    实时监控
1.服务降级Fallback
    程序运行异常
    超时自动降级
    服务熔断出发服务降级
    线程池信号量打满
    人工降级
2.服务熔断Breaker
    降级-熔断-恢复调用链路
3.服务限流FlowLimit
    秒杀等高并发场景
```

## 1.搭建基础子模块（hystrix-8003）

### 1.导入依赖

```xml
   <dependencies>
        <!--新增hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <!--新增hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

### 2.编写配置文件

```yml
server:
  port: 8003
spring:
  application:
    name: hystrix-8003
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:7001/eureka/
```

### 3.编写主启动类

```java
@SpringBootApplication
@EnableEurekaClient
public class Hystrix8003 {
    public static void main(String[] args) {
        SpringApplication.run(Hystrix8003.class,args);
    }
}
```

### 4.编写服务层

```java
public interface PaymentService {
    String info_ok(Integer id);
    String info_Timeout(Integer id);
}
```

```java
@Service
public class PaymentServiceImpl implements PaymentService {
    @Override
    public String info_ok(Integer id) {
        return "线程池：" + Thread.currentThread().getName() + "   paymentInfo_OK,id：  " + id + "\t" + "哈哈哈";
    }

    @Override
    public String info_Timeout(Integer id) {
        int timeNumber = 3;
        try {
            TimeUnit.SECONDS.sleep(timeNumber);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return "线程池：" + Thread.currentThread().getName() + "   paymentInfo_TimeOut,id：  " + id + "\t" + "呜呜呜" + " 耗时(秒)" + timeNumber;

    }
}
```

### 5.控制层

```java
@RestController
@Slf4j
public class PaymentController {
    @Autowired
    private PaymentService service;
    @Value("${server.port}")
    private String PORT;
    @GetMapping("/ok/{id}")
    public String ok(@PathVariable("id") Integer id){
        String result = service.info_ok(id);
        log.info("result:"+result);
        return result;
    }
    @GetMapping("/timeout/{id}")
    public String timeOut(@PathVariable("id") Integer id){
        String result = service.info_Timeout(id);
        log.info("result:"+result);
        return result;
    }
}
```

### 6.测试

[http://localhost:8003/timeout/1](http://localhost:8003/timeout/1)   可以正常访问：每次调用耗费 3 秒

### 7.高并发测试

每秒整 2000 个访问量

此时在用 postman 访问，发现卡死。

Tomcat 的默认的工作线程数被打满了，没有多余的线程来分解压力和处理。

## 2.搭建服务调用者 81（model-81）

### 1.需求

**81 调用 8003，看看会咋样**

### 2.导入依赖

```xml
    <dependencies>
        <!--新增hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

### 3.编写配置文件

```yml
server:
  port: 8181
spring:
  application:
    name: model-81
eureka:
  client:
    register-with-eureka: true #表识不向注册中心注册自己
    fetch-registry: true #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    service-url:
      defaultZone: http://localhost:7001/eureka/
#设置Feign客户端超时时间（openFeign默认支持ribbon）
ribbon:
  #指的是建立连接所用的时间
  ReadTimeout: 6000
  #指的是建立连接以后从服务端读取到资源所用时间
  ConnectTimeout: 6000
```

### 4.编写启动类

```java
@SpringBootApplication
@EnableFeignClients
public class Model81 {
    public static void main(String[] args) {
        SpringApplication.run(Model81.class,args);
    }
}
```

### 5.远程调用接口

```java
@Component
@FeignClient(name="hystrix-8003")
public interface Hystrix8003 {
    @GetMapping("/ok/{id}")
    public String ok(@PathVariable("id") Integer id);
    @GetMapping("/timeout/{id}")
    public String timeOut(@PathVariable("id") Integer id);
}
```

### 6.控制器

```java
@RestController
@Slf4j
public class PaymentController {
    @Autowired
    private Hystrix8003 hystrix8003;
    @GetMapping("/ok/{id}")
    public String ok(@PathVariable("id") Integer id){
        return hystrix8003.ok(id);
    }
    @GetMapping("/timeout/{id}")
    public String timeOut(@PathVariable("id") Integer id){
        return hystrix8003.timeOut(id);
    }
}
```

### 7.测试

[http://localhost:8181/timeout/1](http://localhost:8181/timeout/1) 消耗 3 秒可以正常访问

### 8.高并发测试

2W 个线程压 8003 的 timeout

此时通过 81 去访问 ok 不是超时就是报错

此种现象原因：tomcat 线程池被打满。

## 3.如何解决

超时导致服务器变慢
         超时不在等待
出错
          返回兜底方法
解决
        8003 超时了，调用者 81 不能一直卡死，必须有服务降级。
        8003down 了，调用者 81 不能一直卡死等待，必须有服务降级。
        8003OK，但是调用者 81 自己出问题了（自己的等待时间小于服务提供者），自己处理降级。

## 4.服务降级

### 1.改造 8003 模块

#### 服务层

```java
    @HystrixCommand(
            fallbackMethod = "fallbackHandler",
            commandProperties = {
                    @HystrixProperty(
                            name = "execution.isolation.thread.timeoutInMilliseconds",
                            value = "5000"//5秒以内就是正常的业务逻辑
                    )
            }
    )
    @Override
    public String info_ok(Integer id) {
        int a=1/0;
        return "线程池：" + Thread.currentThread().getName() + "   paymentInfo_OK,id：  " + id + "\t" + "哈哈哈";
    }
        public String fallbackHandler(Integer id){

        return "我是兜底方法";
    }
```

#### 主启动类

```java
@EnableCircuitBreaker//开启服务降级
@SpringBootApplication
@EnableEurekaClient
public class Hystrix8003 {
    public static void main(String[] args) {
        SpringApplication.run(Hystrix8003.class,args);
    }
}
```

### 2.改造 81 模块

服务降级可以在服务提供者侧，也可以再服务消费者侧。更多是在服务消费者侧。

#### 配置文件

```yml
feign:
  hystrix:
    enabled: true #如果处理自身容错就开启。开启方式与生产端不一样
```

#### 主启动类

```java
@EnableHystrix
@SpringBootApplication
@EnableFeignClients
public class Model81 {
    public static void main(String[] args) {
        SpringApplication.run(Model81.class,args);
    }
}
```

#### Controller

```java
    @HystrixCommand(
            fallbackMethod = "ExceptionHandler",
            commandProperties = {
                    @HystrixProperty(
                            name = "execution.isolation.thread.timeoutInMilliseconds",
                            value = "1500"
                    )
            }
    )
    @GetMapping("/timeout/{id}")
    public String timeOut(@PathVariable("id") Integer id){
        return hystrix8003.timeOut(id);
    }
    public String ExceptionHandler(Integer id){
        return "我是8181的降级方法";
    }
```

#### 测试：[http://localhost:8181/timeout/1](http://localhost:8181/timeout/1)

返回 8181 的兜底方法。

### 3.目前的问题：（统一兜底方法）

每个业务方法对应一个兜底的方法，代码膨胀，代码耦合

统一通用处理和自定义独立处理的分开

**统一兜底方法**：

```java
@RestController
@Slf4j
@DefaultProperties(defaultFallback = "GlobalHandler")  //全局的
public class PaymentController {

    @Autowired
    private Hystrix8003 hystrix8003;

    @GetMapping("/ok/{id}")
    public String ok(@PathVariable("id") Integer id){
        return hystrix8003.ok(id);
    }
    @HystrixCommand(
            fallbackMethod = "ExceptionHandler",
            commandProperties = {
                    @HystrixProperty(
                            name = "execution.isolation.thread.timeoutInMilliseconds",
                            value = "1500"
                    )
            }
    )
    @GetMapping("/timeout/{id}")
    public String timeOut(@PathVariable("id") Integer id){
        return hystrix8003.timeOut(id);
    }
    public String ExceptionHandler(Integer id){
        return "我是8181的降级方法";
    }
    public String GlobalHandler(Integer id){
        return "我是8181的降级方法";
    }

}
```

### 4.仍然存在的问题（解耦）

**因为本次的兜底方法其实是 01 调用 8003，所以其实可以直接改造 81 模块**

#### 1.给 80 的远程调用接口安排一个实现类

```java
@Component
public class Hystrix8003Impl implements Hystrix8003 {
    @Override
    public String ok(Integer id) {
        return "81的兜底方法。。。。。";
    }

    @Override
    public String timeOut(Integer id) {
        return "81的兜底方法。。。。。";
    }
}
```

#### 2.完善远程调用接口

```java
@Component
@FeignClient(name="hystrix-8003",fallback = Hystrix8003Impl.class)
public interface Hystrix8003 {
    @GetMapping("/ok/{id}")
    public String ok(@PathVariable("id") Integer id);
    @GetMapping("/timeout/{id}")
    public String timeOut(@PathVariable("id") Integer id);
}
```

#### 3.配置文件

```yml
feign:
  hystrix:
    enabled: true #如果处理自身容错就开启。开启方式与生产端不一样
#
```

#### 4.测试

[http://localhost:8181/timeout/1](http://localhost:8181/timeout/1)

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-9Ihd85Dn-1601977228334)(cloud/snipaste_20200624_151757.jpg)]

## 5.服务熔断

熔断机制是应对雪崩效应的一种微服务链路保护机制。
当扇出链路的某个微服务出错不可用或者响应时间太长时，会进行服务的降级，
进而熔断该节点微服务的调用，快速返回错误的响应信息。
当检测到该节点微服务调用响应正常后，恢复调用链路。
在 SpringCloud 框架里，熔断机制通过 Hystrix 实现。
Hystrix 会监控微服务间调用的状态，当失败的调用到一定阈值，
缺省是 5 秒内 20 次调用失败，就会启动熔断机制。
熔断机制的注解是[@HystrixCommand ](/HystrixCommand)

### 修改 8003

#### 1.服务层添加方法

```java
String paymentCircuitBreaker(Integer id);
```

```java
    //服务熔断
    @HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback",commandProperties = {
            @HystrixProperty(name = "circuitBreaker.enabled",value = "true"),  //是否开启断路器
            @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"),   //当在配置时间窗口内达到此数量的失败后，打开断路，默认20个
            @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"),  //断路多久以后开始尝试是否恢复，默认5s
            @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60"), //出错百分比阈值，当达到此阈值后，开始短路。默认50%
    })
    public String paymentCircuitBreaker(Integer id){
        if (id < 0){
            throw new RuntimeException("*****id 不能负数");
        }
        String serialNumber = IdUtil.simpleUUID();//hutool.cn工具包

        return Thread.currentThread().getName()+"\t"+"调用成功,流水号："+serialNumber;
    }
    public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id){
        return "id 不能负数，请稍候再试,(┬＿┬)/~~     id: " +id;
    }
```

#### 2.控制层

```java
    @GetMapping("/hystrix/{id}")
    public String hystrix(@PathVariable("id") Integer id){
        String result = service.paymentCircuitBreaker(id);
        log.info("result:"+result);
        return result;
    }
```

#### 3.测试

[http://localhost:8003/hystrix/-1](http://localhost:8003/hystrix/-1)

狂点 20 次

在点击 http://localhost:8003/hystrix/1 发现服务熔断了

#### 4.熔断类型

`熔断打开`
        请求不再调用当前服务，内部设置时钟一般为 MTTR（平均故障处理时间），
        当打开时长达到所设时钟则进入熔断状态。
`熔断关闭`
        熔断关闭不会对服务进行熔断
`熔断半开`
        部分请求根据请求规则调用当前服务，如果请求成功且符合规则则认为当前服务恢复正常。
   `关闭熔断`

## 6.服务限流

## 7.服务监控 hystrixDashboard

Hystrix 会持续地记录所有通过 Hystrix 发起的请求的执行信息，并以统计报表和图形的形式展示给用户，
包括每秒执行多少请求多少成功，多少失败等。
Netflix 通过 hystrix-metrics-event-stram 项目实现了对以上指示的监控。
Spring Cloud 也提供了 Hystrix Dashboard 的整合，
对监控内容转化成可视化界面。

### 配置仪表盘 9001

#### 1.创建子模块（hystrix-9001）

```xml
    <dependencies>
        <!--新增hystrix dashboard-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

#### 2.编写配置文件

```yml
server:
  port: 9011
spring:
  application:
    name: hystrix-9001
eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
```

#### 3.主启动类

```java
@SpringBootApplication
@EnableHystrixDashboard
public class Hystrix9001 {
    public static void main(String[] args) {
        SpringApplication.run(Hystrix9001.class,args);
    }
}
```

#### 4.所有需要监控的模块加入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

#### 5.访问：[http://localhost:9011/hystrix](http://localhost:9011/hystrix)

---

# 七，Gateway 网关

## 1.概述

1.概述：
   Gateway 旨在提供一种简单而有效的方式来对 API 进行路由，以及提供强大的过滤器功能，
   例如：熔断，限流，重试等。
     SpringCloudGateway 是基于 WebFlux 框架实现的，二 WebFlux 框架底层则使用了高性能的
Reator 模式通讯框架 Netty。 2.能干嘛：
       反向代理
       鉴权
       流量控制
       熔断
       日志监控 3.三大概念
      1.路由（Route）
             路由是构建网关的基本模块，他由 ID，目标 URI，一系列的断言和过滤器组成，
             如果断言为 true 则匹配该路由。
      2.断言（Predicate）
             如果请求与断言相匹配则进行路由。
      3.过滤（Filter）
             指的是 Spring 框架中 GatewayFilter 的实例，使用过滤器，可以再请求被路由前或之后对请求进行修改。
       4.Web 请求，通过一些匹配条件，定位到真正的服务节点，并在这个转发过程前后，
进行一些精细化控制。Predicate 就是我们的匹配条件，而 Filter，就是一个拦截器，
有了这两个元素，唉加上目标 uri，就可以实现一个具体的路由了。
   4.工作流程
         客户端向 SpringCloudGateway 发出请求。然后在 GatewayHandlerMapping 中找到
与请求匹配的路由，将其发送到 GatewayWebHandler。
        Handler 通过指定的过滤器链将请求发送给实际的服务执行业务逻辑，然后返回。
        Filter 在代理请求之前执行可以做参数校验，权限校验，流量监控，日志输出，协议转换等，
         在代理请求之后执行可以做响应内容，响应头的修改，日志的输出，流量控制。
   5.核心逻辑：路由转发-执行过滤器链

## 2.搭建网关模块 Gateway9527

### 1.导入依赖

```xml
        <!--新增gateway，不需要引入web和actuator模块-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
```

### 2.配置文件

```yml
server:
  port: 9527
spring:
  application:
    name: gateway-9527

eureka:
  instance:
    hostname: gateway-9527
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://localhost:7001/eureka
```

### 3.主启动类

```java
@SpringBootApplication
@EnableEurekaClient
public class Gateway9527 {
    public static void main(String[] args) {
        SpringApplication.run(Gateway9527.class,args);
    }
}
```

## 3.路由映射

**希望我们在访问 8001 和 8002 端口的时候不需要直接访问，二是在外面来一个端口包装，隐藏实际端口。**

### 1.配置文件

```yml
server:
  port: 9527
spring:
  application:
    name: gateway-9527
  cloud:
    gateway:
      routes:
        - id: model-8001 #路由的ID，没有固定规则但要求唯一，建议配合服务名
          uri: http://localhost:8001 #匹配后提供服务的路由地址
          predicates:
            - Path=/selectById/** #断言,路径相匹配的进行路由

        - id: model-8002
          uri: http://localhost:8002
          predicates:
            - Path=/selectById/** #断言,路径相匹配的进行路由
eureka:
  instance:
    hostname: gateway-9527
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://localhost:7001/eureka
```

### 2.测试

[http://localhost:9527/selectById/1](http://localhost:9527/selectById/1)

```json
{
  "id": 1,
  "serial": "尹会东"
}
```

## 4.通过微服务名实现动态路由

默认情况下 Gateway 会根据注册中心的服务列表，以注册中心上微服务名为路径创建动态路由进行转发，从而实现动态路由的功能

### 1.配置文件

```yml
server:
  port: 9527
spring:
  application:
    name: gateway-9527
  cloud:
    gateway:
      discovery: #开启从注册中心动态创建路由的功能，利用微服务名进行路由
        locator:
          enabled: true #开启从注册中心动态创建路由的功能，利用微服务名进行路由
      routes:
        - id: model-8001 #路由的ID，没有固定规则但要求唯一，建议配合服务名
          #uri: http://localhost:8001   #匹配后提供服务的路由地址
          uri: lb://model-8001
          predicates:
            - Path=/selectById/** #断言,路径相匹配的进行路由

        - id: model-8002
          uri: lb://model-8001
          predicates:
            - Path=/selectById/** #断言,路径相匹配的进行路由
eureka:
  instance:
    hostname: gateway-9527
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://localhost:7001/eureka
```

### 2.注意

需要注意的是 uri 的协议为 lb，表示启用 Gateway 的负载均衡功能。

lb://serviceName 是 spring cloud gateway 在微服务中自动为我们创建的负载均衡 uri

### 3.测试

[http://localhost:9527/selectById/1](http://localhost:9527/selectById/1)

8001 和 8002 两个服务实现了负载均衡效果。

## 5.Predicate

```yml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能，利用微服务名进行路由
      routes:
        - id: payment_routh #路由的ID，没有固定规则但要求唯一，建议配合服务名
          #uri: http://localhost:8001   #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/get/** #断言,路径相匹配的进行路由

        - id: payment_routh2
          #uri: http://localhost:8001   #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service
          predicates:
            - Path=/payment/lb/** #断言,路径相匹配的进行路由
            #- After=2020-03-08T10:59:34.102+08:00[Asia/Shanghai]
            #- Cookie=username,zhangshuai #并且Cookie是username=zhangshuai才能访问
            #- Header=X-Request-Id, \d+ #请求头中要有X-Request-Id属性并且值为整数的正则表达式
            #- Host=**.atguigu.com
            #- Method=GET
            #- Query=username, \d+ #要有参数名称并且是正整数才能路由

eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://localhost:7001/eureka
```

`predicate`就是提前定义好的规则，只有符合规则的请求才能进行路由映射。

## 6.Filter

路由过滤器可用于修改进入的 HTTP 请求和返回的 HTTP 响应，路由过滤器只能指定路由进行使用。

SpringCloud Gateway 内置了多种路由过滤器，他们都由 GatewayFilter 的工厂类来产生。

1.生命周期
pre：
在业务逻辑执行之前
post：
在业务逻辑执行之后 2.种类：
1）GatewayFilter（31 种）
2）GlobalFilter 3.自定义 GlobalFilter
1.implements GlobalFilter,Ordered

在 9527 模块下新建过滤器

```java
@Component
@Slf4j
public class MyFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("时间："+new Date());
        String username = exchange.getRequest().getQueryParams().getFirst("username");
        if (StringUtils.isEmpty(username)){
            log.info("用户名为空");
            exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);
            return exchange.getResponse().setComplete();
        }
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        return 0;
    }
}
```

此过滤器会过滤所有请求的 URL，查看是否有 username，没有就拦截。

[http://localhost:9527/selectById/1](http://localhost:9527/selectById/1)   控制台打印用户名为空，无响应。

[http://localhost:9527/selectById/1?username=zz](http://localhost:9527/selectById/1?username=zz)   正常响应。

---

# 八，SpringCloudSleuth 分布式链路请求跟踪

## 1.干啥的？

一个请求在服务端可能经过多个微服务，如果报错了不好查找， 分布式链路请求跟踪用来提供一套服务跟踪的方案。

## 2.搭建链路监控步骤

### 1.运行 jar 包

java -jar zipkin-server-2.12.9-exec.jar

运行控制台

[http://localhost:9411/zipkin/](http://localhost:9411/zipkin/)

### 2.服务提供者 8001

#### 1.依赖

```xml
        <!--包含了sleuth+zipkin-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zipkin</artifactId>
        </dependency>
```

#### 2.配置文件

```yml
server:
  port: 8001
spring: #此次添加配置
  zipkin:
    base-url: http://localhost:9411
  sleuth:
    sampler:
      #采样率值介于0~1之间，1表示全部采样
      probability: 1 #此次添加配置
  application:
    name: model-8001
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/cloud0623?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: root
mybatis:
  mapper-locations: classpath:/mapper/*.xml
  type-aliases-package: com.example.entities
eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
```

### 3.服务调用者 80

#### 1.依赖

```xml
        <!--包含了sleuth+zipkin-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zipkin</artifactId>
        </dependency>
```

#### 2.配置文件

```yml
zipkin:
  base-url: http://localhost:9411
sleuth:
  sampler:
    #采样率值介于0~1之间，1表示全部采样
    probability: 1 #此次添加配置
```

### 4.测试

[http://localhost:80/test](http://localhost:80/test)

此时去控制台查询
![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006180025615.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=0147aa454c50a299902d464dc8f6ddae8fdb65c5646200a866bb498f1a8b61e9#from=url&id=uddZj&margin=%5Bobject%20Object%5D&originHeight=333&originWidth=809&originalType=binary∶=1&status=done&style=none)

---

# 九，微服务执行流程

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006181650790.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=9ddf23747f0d27e08a96bc6672b10df186c03ce52c73d71366c28807430ed6fc#from=url&id=vDOUY&margin=%5Bobject%20Object%5D&originHeight=1182&originWidth=1921&originalType=binary∶=1&status=done&style=none)

请求经过网关进入服务端，服务端的所有服务都是注册在注册中心的，A 通过 RestTemplate 远程调用 B，B 又通过 Ribbon 负载均衡调用 E 和 C，E 通过 openFeign 远程调用 D，C 通过 Hystrix 熔断器对调用的服务 F 进行服务降级，限流，熔断。整个请求过程，一直有分布式链路请求跟踪系统在监控。

# 十，springCloudAlibaba 概述

## 1.功能：

`服务限流降级（Sentinel）`
   `服务注册与发现（Nacos）`
   `分布式配置管理（Nacos）`
   `消息驱动能力(RocketMQ/ACM)`
   `分布式事务(Seata)`
   `阿里云对象存储(OSS)`
   `分布式任务调度(SchedulerX)`
   `阿里云信息服务(SMS)`

## 2.依赖

```xml
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.2.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
    </dependency>
```

## 3.资料获取

[https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md](https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md)

---

# 十一，SpringCloudAlibabaNacos 服务注册和配置中心

## 1.是啥？

注册中心+配置中心
       Nacos=Eureka+Config+Bus

## 2.干啥的？

替换 Eureka 做服务注册中心
    替换 Config 做服务配置中心
   CAP 原则又称 CAP 定理，指的是在一个分布式系统中，一致性，可用性，分区容错性。
   CAP 原则指的是，这三个要素最多只能同时实现两点，不可能三者兼顾。

## 3.安装并运行 Nacos

进入 bin 目录下 startup.cmd
   [http://localhost:8848/nacos](http://localhost:8848/nacos)
    默认账号密码都是 nacos

## 4.作为注册中心

### 1.父项目引入依赖

```xml
            <!--spring cloud alibaba 2.1.0.RELEASE-->
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.1.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
```

### 2.新建子模块（nacos-8004），（nacos-8005）

```xml
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

### 3.配置文件

```yml
server:
  port: 8004
spring:
  application:
    name: nacos-productor
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #配置Nacos地址

management:
  endpoints:
    web:
      exposure:
        include: "*" #默认只公开了/health和/info端点，要想暴露所有端点只需设置成星号
```

### 4.主启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
public class Nacos8004 {
    public static void main(String[] args) {
        SpringApplication.run(Nacos8004.class,args);
    }
}
```

### 5.控制层

```java
@RestController
@Slf4j
public class NacosController {

    @Value("${server.port}")
    private String PORT;

    @GetMapping("/getPort")
    public String getPORT(){
        log.info("PORT:"+PORT);
        return PORT;
    }
}
```

### 6.测试

[http://localhost:8005/getPort](http://localhost:8005/getPort)

[http://localhost:8004/getPort](http://localhost:8004/getPort)

**此时查看控制台**

### 7.新建模块（nacos-83）服务调用者

```xml
    <dependencies>
        <!--SpringCloud ailibaba nacos -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

### 8.配置文件

```yml
server:
  port: 83

spring:
  application:
    name: nacos-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
#消费者将要去访问的微服务名称(注册成功进nacos的微服务提供者【可选】，注意：nacos-payment-provider含有IP和端口)
service-url:
  nacos-user-service: http://nacos-productor
```

### 9.主启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
public class Nacos83 {
    public static void main(String[] args) {
        SpringApplication.run(Nacos83.class,args);
    }

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

### 10.controller

```java
@RestController
@Slf4j
public class NacosController {

    @Autowired
    private RestTemplate restTemplate;
    @Value("${service-url.nacos-user-service}")
    private String URL;
    @GetMapping("/getPort")
    public String getPort(){
        return restTemplate.getForObject(URL+"/getPort",String.class);
    }
}
```

### 11.测试

[http://localhost:83/getPort](http://localhost:83/getPort)

响应结果依次是 8004 8005，说明实现了负载均衡。

为啥实现了负载均衡？

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203548451.jpg%23pic_center&sign=2cef65db272341b0be2e2def0aced818239d9c3ec9398229fd4697113884772c#from=url&id=NllBf&margin=%5Bobject%20Object%5D&originHeight=173&originWidth=725&originalType=binary∶=1&status=done&style=none)

## 5.Nacos 和 CAP

CAP 原则又称 CAP 定理，
指的是在一个分布式系统中， Consistency（一致性）、 Availability（可用性）、Partition tolerance（分区容错性），
三者不可得兼。
一致性（C）：在分布式系统中的所有数据备份，在同一时刻是否同样的值。（等同于所有节点访问同一份最新的数据副本）
可用性（A）：在集群中一部分节点故障后，集群整体是否还能响应客户端的读写请求。（对数据更新具备高可用性）
分区容忍性（P）：以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在 C 和 A 之间做出选择。
CAP 原则的精髓就是要么 AP，要么 CP，要么 AC，但是不存在 CAP。
如果在某个分布式系统中数据无副本， 那么系统必然满足强一致性条件， 因为只有独一数据，不会出现数据不一致的情况，此时 C 和 P 两要素具备，但是如果系统发生了网络分区状况或者宕机，必然导致某些数据不可以访问，此时可用性条件就不能被满足，即在此情况下获得了 CP 系统，但是 CAP 不可同时满足。因此在进行分布式架构设计时，必须做出取舍。当前一般是通过分布式缓存中各节点的最终一致性来提高系统的性能，通过使用多节点之间的数据异步复制技术来实现集群化的数据一致性。

## 6.Nacos 支持 AP 和 CP 模式的切换

**C 是所有节点在同一时间看到的数据是一致的；而 A 的定义是所有的请求都会收到响应。**

## 7.Nacos 作为配置中心

### 1.基础配置

#### 1.创建模块（config-8006）

```xml
    <dependencies>
        <!--nacos-config-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>
        <!--nacos-discovery-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--web + actuator-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--一般基础配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

#### 2.编写配置文件

**why 配置两个**
Nacos 同 springcloud-config 一样，在项目初始化时，要保证先从配置中心进行配置拉取，
拉取配置之后，才能保证项目的正常启动
springboot 中配置文件的加载是存在优先级顺序的，bootstrap 优先级高于 application

bootstrap.yml

```yml
server:
  port: 8006

spring:
  application:
    name: nacos-config
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #服务注册中心地址
      config:
        server-addr: localhost:8848 #配置中心地址
        file-extension: yml #指定yaml格式的配置（yml和yaml都可以）
#${spring.application.name}-${spring.profile.active}.${spring.cloud.nacos.config.file-extension}
#nacos-config-client-dev.yaml  (一定要与file-extension值保持一致)
```

application.yml

```yml
spring:
  profiles:
    active: dev #表示开发环境
```

#### 3.编写启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
public class Config8006 {
    public static void main(String[] args) {
        SpringApplication.run(Config8006.class,args);
    }
}
```

#### 4.Controller

```java
@RestController
@Slf4j
@RefreshScope
//通过SpringCould原生注解@RefreshScope实现配置自动更新
public class NacosController {

    @Value("${config.info}")
    private String INFO;
    @GetMapping("/info")
    public String getINFO(){
        return INFO;
    }

}
```

#### 5.打开 nacos 控制台

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203622734.jpg%23pic_center&sign=fb15ffb974e90f3df66678df86eeaa81ec45ddbd57cfdafd50c9f5519ae55606#from=url&id=NOrH8&margin=%5Bobject%20Object%5D&originHeight=169&originWidth=1913&originalType=binary∶=1&status=done&style=none)

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203635497.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=eedb002fb1a3d4736e00a62ca7ff3b190ac65eab2513347fedd4aca4b76c33f0#from=url&id=nQj3Y&margin=%5Bobject%20Object%5D&originHeight=530&originWidth=686&originalType=binary∶=1&status=done&style=none)

DataID 的写法：

`${spring.application.name}-${spring.profile.active}.${spring.cloud.nacos.config.file-extension}`

`bootstrap.xml`配置文件里补全。

2.@RefreshScope,自带动态刷新

3.测试

[http://localhost:8006/info](http://localhost:8006/info)

`测试自动刷新`

### 2.分类配置

#### 1.解决问题：多环境，多项目管理

#### 2.Namespace+Group+Data ID 三者关系？为什么这么设计？

```
    最外层的namespace是可以用来区分部署环境的，Group和DataID逻辑上区分两个目标对象。
    默认情况下：
        NameSpace=public，Group=DEFAULT_GROUP,默认Cluster是DEFAULT
    Nacos默认的命名空间是public，Namespace主要用来实现隔离。
    比方说我们现在有三个环境：开发、测试、生产环境，我们就可以创建三个Namespace，
    不同的 Namespace之间是隔离的。
    Group默认是DEFAULT_GROUP，Group可以把不同的微服务划分到同一个分组里面去
    Service就是微服务；一个Service可以包含多个Cluster(集群)，Nacos默认Cluster是DEFAULT，
    Cluster是对指定微服务的一个虚拟划分。比方说为了容灾，将Service微服务分别部署在了杭州机房和广州机房，
    这时就可以给杭州机房的Service微服务起一个集群名称(HZ)，给广州机房的Service微服务起一个集群名字(GZ)，
    还可以尽量让同一个机房的微服务互相调用，以提升性能。最后是Instance，就是微服务的实例。
```

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203701705.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=59c6ad3e7d977b70f2d0a235f02b99a2e263e545a2a63914bee902e0f4047de5#from=url&id=P1jKh&margin=%5Bobject%20Object%5D&originHeight=491&originWidth=1401&originalType=binary∶=1&status=done&style=none)

#### 3.DataID 方案

`指定spring.profile.active和配置文件的DataID来使不同环境下读取不同的配置`

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203721817.jpg%23pic_center&sign=0c85bda78db39e92bfcd5d9ed94f2d9d7f39ce5cf0f95fbaaaa5be8f5f51eb29#from=url&id=IVTZk&margin=%5Bobject%20Object%5D&originHeight=139&originWidth=651&originalType=binary∶=1&status=done&style=none)

#### 4.Group 方案

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203738185.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=18430896423561eef4133aa23a26e48cf646d46dd690e3a96cbece595f074e7e#from=url&id=adPAT&margin=%5Bobject%20Object%5D&originHeight=417&originWidth=522&originalType=binary∶=1&status=done&style=none)

此时修改`bootstrap.yml`配置文件

```yml
server:
  port: 8006

spring:
  application:
    name: nacos-config
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #服务注册中心地址
      config:
        server-addr: localhost:8848 #配置中心地址
        file-extension: yml #指定yaml格式的配置（yml和yaml都可以）
        group: MY_GROUP #指定分组
```

测试：[http://localhost:8006/info](http://localhost:8006/info)

`MY_GROUP`

#### 5.Namespace 方案

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203754541.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=bba59961363aea24d56edaf0d29836a6869c51210f07089bc218d4e677dadcc5#from=url&id=zZE7d&margin=%5Bobject%20Object%5D&originHeight=416&originWidth=978&originalType=binary∶=1&status=done&style=none)

**复制命名空间 ID**

`62743e40-773b-495c-9d9f-072b22d38e51`

**点击切换命名空间**

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203811722.jpg%23pic_center&sign=3fedfaf49305281be7576af21b5eb6bce60c5b541457aab61a9ac6f5e40a69aa#from=url&id=oQ6VC&margin=%5Bobject%20Object%5D&originHeight=58&originWidth=220&originalType=binary∶=1&status=done&style=none)

**新建配置**

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203826191.jpg%23pic_center&sign=8185be566108506eee379de661192a2cdd025ab7139081d0e39f6dbbb784e959#from=url&id=uF8tQ&margin=%5Bobject%20Object%5D&originHeight=62&originWidth=632&originalType=binary∶=1&status=done&style=none)

**修改配置文件**

```yml
server:
  port: 8006

spring:
  application:
    name: nacos-config
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #服务注册中心地址
      config:
        server-addr: localhost:8848 #配置中心地址
        file-extension: yml #指定yaml格式的配置（yml和yaml都可以）
        group: MY_GROUP
        namespace: 62743e40-773b-495c-9d9f-072b22d38e51
```

**测试**

[http://localhost:8006/info](http://localhost:8006/info)

`测试命名空间`

## 8.Nacos 集群和持久化配置

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006203849238.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=32b16c9fb6c068b2d664a91a5fba86403366f98528779066271859704982c5ec#from=url&id=tNFYM&margin=%5Bobject%20Object%5D&originHeight=491&originWidth=1401&originalType=binary∶=1&status=done&style=none)

```
默认Nacos使用嵌入式数据库实现数据的存储。所以，如果启动多个默认配置下的Nacos节点，数据存储是存在一致性问题的。为了解决这个问题，Nacos采用了集中式存储的方式来支持集群化部署，目前只支持MySQL 的存储。
```

1.Nacos 默认自带的是嵌入式数据库 derby
2.derby 到 mysql 的切换步骤：
nacos-server-1.1.4\nacos\conf 目录下找到 sql 脚本
nacos-mysql.sql
执行脚本
nacos-server-1.1.4\nacos\conf 目录下找到 application.properties

```properties
    spring.datasource.platform=mysql
    db.num=1
    db.url.0=jdbc:mysql://localhost:3306/nacos_devtest?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
    db.user=nacos_devtest
    db.password=root
```

3.启动 nacos，可以看到是个全新的空记录界面，以前是记录进 derby 4.测试：新建配置，发现配置信息写入了 Mysql 数据库

---

# 十二，SpringCloud Alibaba Sentinel 实现熔断与限流

## 1.是啥？

用来代替前面说的 Hystrix

## 2.Sentinel 分为两个部分：

核心库，不依赖任何框架，能够运行于所有 java 运行时环境，同时对 Dubbo/SpringCloud 等框架也有较好的支持。
    控制台，基于 SpringBoot 开发，打包后可以直接运行，不需要额外的 Tomcat 等应用容器。

## 3.安装 Sentinel 控制台

前提：8080 端口不能被占用
   `java -jar sentinel-dashboard-1.7.0.jar`
    访问管理页面
   [http://localhost:8080](http://localhost:8080)

## 4.新建项目（sentinel-8007）

### 1.导入依赖

```xml
    <dependencies>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-datasource-nacos</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>4.6.3</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

### 2.配置文件

```yml
server:
  port: 8007

spring:
  application:
    name: sentinel-8007
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        port: 8719 #默认8719，应用与Sentinel控制台交互的端口，应用本地会起一个该端口占用HttpServer

management:
  endpoints:
    web:
      exposure:
        include: "*"
```

### 3.主启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
public class Sentinel8007 {
    public static void main(String[] args) {
        SpringApplication.run(Sentinel8007.class,args);
    }
}
```

### 4.Controller

```java
@RestController
@Slf4j
public class TestController {

    @GetMapping("/testA")
    public String testA(){
        return "testA";
    }

    @GetMapping("/testB")
    public String testB(){
        return "testB";
    }
}
```

### 5.测试

测试打开 sentinel 控制台，发现啥也没有，默认采用懒加载。

访问：[http://localhost:8007/testB](http://localhost:8007/testB) 以后查看控制台，发现 8080 开始监控 8007。

## 5.流控规则

```properties
    资源名：唯一名称默认请求路径
    针对来源：sentinel可以针对调用者进行限流，填写微服务名，默认default，不区分来源。
    阈值类型/单击阈值：
        QPS（每秒钟的请求数量）:当调用该API的QPS达到阈值时进行限流。
        线程数：当调用该API的线程数达到阈值时，进行限流。
    是否集群：不需要集群。
    流控模式：
        直接：api达到限流条件，直接限流。
        关联：当关联的资源达到阈值时，就限流自己。
        链路：只记录指定链路上的流量（指定资源从入口资源进来的流量，如果达到阈值，就进行限流）
    流控效果：
        快速失败：直接失败，抛异常。
        Warm Up：根据codeFactor(冷加载因子，默认为3)的值，从阈值/codeFactor，
        经过预热时长，才达到设置的QPS阈值。
        排队等待：匀速排队，让请求匀速通过，阈值类型必须设置为QPS，否则无效。
```

### 1.测试 QPS

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2020100620421447.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=96627329f7ded8623be06bf037b14601e79bcc820007a08bb612a3023c59c5b2#from=url&id=ST310&margin=%5Bobject%20Object%5D&originHeight=200&originWidth=803&originalType=binary∶=1&status=done&style=none)

此时访问 http://localhost:8007/testA，每秒一次可以，超过一次就会抛出异常。

### 2.测试线程数

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-adkMk3MK-1601988027131)(cloud/snipaste_20200625_114229.jpg)]

此时直接访问 http://localhost:8007/testB，可以正常访问。

```java
    @GetMapping("/testB")
    public String testB(){
        try {
            Thread.sleep(800);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "testB";
    }
```

此时在访问，`Blocked by Sentinel (flow limiting)`。

### 3.关联

A 与 B 关联，当 B 的访问量达到阈值，A 就限流自己。

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204232186.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=046838b18913f6198d04af9fe2b21dc35afdb1f686fe8abfb49d01a730fd0d38#from=url&id=gPtiP&margin=%5Bobject%20Object%5D&originHeight=438&originWidth=771&originalType=binary∶=1&status=done&style=none)

当/`testB`的访问量超过阈值，`/testA`就会被限流。

## 6.流控效果

### 1.直接->快速失败（默认的流控处理）

直接失败，抛出异常：Blocked by Sentinel (flow limiting)

### 2.预热

公式：阈值除以 coldFactor（默认值为 3），经过预热时长后才会达到阈值

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204248598.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=1a5fe1a74c422575f17085c8630e1f755aa70064d5d7ddaba14e1ae36de66328#from=url&id=haw7I&margin=%5Bobject%20Object%5D&originHeight=450&originWidth=694&originalType=binary∶=1&status=done&style=none)

### 3.排队等待

·   匀速排队，让请求以均匀的速度通过，阈值类型必须设置成 QPS，否则无效。

·   设置含义：/testB 每秒 1 次请求，超过的话就排队等待，等待的超时时间为 20000 毫秒。

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204312663.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=adb4a33acaf9d2e79f67f65a4079c117e574aa62f848eaac8489b7b1c9411798#from=url&id=HDsQS&margin=%5Bobject%20Object%5D&originHeight=451&originWidth=772&originalType=binary∶=1&status=done&style=none)

## 7.降级规则

```properties
RT(平均响应时间，秒级):
    超过阈值，且时间窗口内的请求>=5,两个条件同时满足后触发降级，
    窗口期过后关闭断路器。
    RT最大4900ms，更大的需要启动配置项
    -Dcsp.sentinel.statistic.max.rt=xxx来配置
异常比例(秒级)：
    QPS>=5且异常比例超过阈值时，触发降级；时间窗口结束，关闭降级。
异常数（分钟级）
    异常数超过阈值时，触发降级；时间窗口结束后，关闭降级。
    当资源被降级后，在接下来的降级时间窗口之内，对该资源的调用都自动熔断（默认行为是抛出DegradeException）
    Sentinel的断路器是没有半开状态的
```

### 1.RT

```java
    @GetMapping("/testC")
    public String testC(){
        try {
            Thread.sleep(800);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "testC";
    }
```

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204329812.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=747766fcbfd270f9f880210ef9b64e6263dd3c3e624429f7aa3aa871b0ec5808#from=url&id=qHUR2&margin=%5Bobject%20Object%5D&originHeight=206&originWidth=796&originalType=binary∶=1&status=done&style=none)

每个请求时间超过 200ms 且每秒请求大于 5 个就会触发降级，5 秒内都是熔断状态。

### 2.异常比例

```java
    @GetMapping("/testD")
    public String testD(){
        int a=10/0;
        return "testD";
    }
```

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204349148.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=8948307fb34d2906cd4737638bc66c3a9c62aa01752be75aaf5855ad4997fdbe#from=url&id=vxJqk&margin=%5Bobject%20Object%5D&originHeight=203&originWidth=768&originalType=binary∶=1&status=done&style=none)

每秒钟的请求大于 5 个，异常比例超过 0.1，在接下来的 5s 内，是熔断状态。

### 3.异常数

```java
    @GetMapping("/testE")
    public String testE(){
        int a=10/0;
        return "testE";
    }
```

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204402916.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=a2beb82804bc6a91601d014dd3e42c6a7ae77db46392e096ea314f0a41c45b2b#from=url&id=PELV4&margin=%5Bobject%20Object%5D&originHeight=213&originWidth=802&originalType=binary∶=1&status=done&style=none)

单独访问每次都报错，高并发的情况下不报错了，因为服务降级了。

## 8.热点 key 限流

**经常被访问的某个 url，限制他。**

### 自定义兜底方法

```java
    @GetMapping("/testF")
    @SentinelResource(value = "testF",blockHandler ="handler" )
    public String testF(@RequestParam(required = false) Integer a, @RequestParam(required = false) Integer b) {
        //int a=10/0;
        return "testF";
    }
    public String handler(Integer a,Integer b, BlockException exception){
        return "handler()....";
    }
```

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204418595.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=b20288afc458745702fcd9f163b1aebe4545c27235e8ecea543f34c472eb0428#from=url&id=ls6zk&margin=%5Bobject%20Object%5D&originHeight=296&originWidth=799&originalType=binary∶=1&status=done&style=none)

如果带着参数 a 访问`testF`且每秒超过一个，就会才去降级策略并返回兜底方法。

### 参数例外项

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204434972.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=8b32e1a7402a53a6b8678621b37f53fc2e7cbf9b94986bc72016a7eb3e172401#from=url&id=Eh7Se&margin=%5Bobject%20Object%5D&originHeight=295&originWidth=785&originalType=binary∶=1&status=done&style=none)

[http://localhost:8007/testF?a=5](http://localhost:8007/testF?a=5)

当第一个参数的值等于 5 时，限流阈值达到 200.

`@SentinelResource主管配置出错，运行出错该走异常走异常`

## 9.系统规则

### 1.是啥？

从应用级别的入口流量进行控制，让系统尽可能泡在最大吞吐量的同时保证系统整体的稳定性。

### 2.系统规则支持以下的模式：

`Load自适应`：
   `平均RT`：
   `并发线程数`：
   `入口QPS`：

### 3.配置系统规则

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204453251.jpg%23pic_center&sign=6adf4a2fbab684f336e9c688d4e7ae2f58d918c537b2ac8831ca3696db2e8b26#from=url&id=RiJlH&margin=%5Bobject%20Object%5D&originHeight=164&originWidth=822&originalType=binary∶=1&status=done&style=none)

## 10[@SentinelResource ](/SentinelResource)

### 1.按照资源名称限流

```java
    @GetMapping("/byResource")
    @SentinelResource(value = "byResource",blockHandler = "byResourceHandler")
    public String byResource(){
        return "byResource";
    }
    public String byResourceHandler(BlockException exception){
        return "byResourceHandler()....";
    }
```

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204511503.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=f5d97244e9dc5c5bd6600eb385605f70d5e6e2d9fb89687d6b10deb75ffd0ae3#from=url&id=mMmbJ&margin=%5Bobject%20Object%5D&originHeight=304&originWidth=802&originalType=binary∶=1&status=done&style=none)

`http://localhost:8007/byResource`

狂点执行兜底方法

### 2.按照 URL 地址限流

```java
    @GetMapping("/byURL")
    @SentinelResource(value = "byURL")
    public String byURL(){
        return "byURL";
    }
```

![](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20201006204526282.jpg%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTU5NjAyMg%3D%3D%2Csize_16%2Ccolor_FFFFFF%2Ct_70%23pic_center&sign=817beec62ba9226a133ff7c7924eea6d8bc5103e1567caeed5a0289e2c158944#from=url&id=TbmO0&margin=%5Bobject%20Object%5D&originHeight=284&originWidth=796&originalType=binary∶=1&status=done&style=none)

`http://localhost:8007/byURL`

狂点返回`sentinel`自带的兜底方法‘。

### 3.自定义限流处理逻辑

```java
public class CloudHandler {
    public static String Handler1(BlockException exception){
        return "Handler1";
    }
    public static String Handler2(BlockException exception){
        return "Handler2";
    }
}
```

```java
    @GetMapping("/byURL")
    @SentinelResource(value = "byURL",
            blockHandlerClass = CloudHandler.class,
            blockHandler = "Handler1"
    )
    public String byURL(){
        return "byURL";
    }
```

`http://localhost:8007/byURL`

狂点返回的兜底方法是我们自己指定的。

## 11.服务熔断功能

`sentinel`整合`ribbon`+`openFeign`+`fallback`

### 1. Ribbon 系列

#### 1.启动`nacos`和`sentinel`

#### 2.创建服务提供者`sentinel-9003`，`sentinel-9004`

```xml
    <dependencies>
        <!--SpringCloud ailibaba nacos -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <!-- SpringBoot整合Web组件 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--日常通用jar包配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

```yml
server:
  port: 9003

spring:
  application:
    name: sentinel-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #配置Nacos地址

management:
  endpoints:
    web:
      exposure:
        include: "*"
```

```java
@SpringBootApplication
@EnableDiscoveryClient
public class Sentinel9003 {
    public static void main(String[] args) {
        SpringApplication.run(Sentinel9003.class,args);
    }
}
```

```java
@RestController
public class TestController {

    @Value("${server.port}")
    private String PORT;
    @GetMapping("/test")
    public String Test(){
        return PORT;
    }
}
```

#### 3.创建消费者`sentinel-84`

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

```yml
server:
  port: 84

spring:
  application:
    name: nacos-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080 #配置Sentinel dashboard地址
        port: 8719 #默认8719，应用与Sentinel控制台交互的端口，应用本地会起一个该端口占用的HttpServer

service-url:
  nacos-user-service: http://sentinel-provider
```

```java
@SpringBootApplication
@EnableDiscoveryClient
public class Sentinel84 {
    public static void main(String[] args) {
        SpringApplication.run(Sentinel84.class,args);
    }
}
```

```java
@SpringBootConfiguration
public class MyConfig {

    @Bean
    @LoadBalanced
    public RestTemplate template(){
        return new RestTemplate();
    }
}
```

```java
@RestController
@Slf4j
public class TestController {
    @Value("${service-url.nacos-user-service}")
    private String URL;
    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/test")
    public String test(){
        return restTemplate.getForObject(URL+"/test",String.class);
    }
}
```

`http://localhost:84/test`

发现实现了负载均衡

#### 4.测试

1.

`@SentinelResource(value = "fallback")//没有配置`

给客户 error 界面，不友好。

2.

`@SentinelResource(value = "fallback",fallback = "handlerFallback") //fallback只负责业务异常`

3.

`@SentinelResource(value="fallback",blockHandler="blockHandler")//blockHandler只负责sentinel控制台配置违规`

4.

`@SentinelResource(value = "fallback",fallback = "handlerFallback",blockHandler = "blockHandler")`

fallback 和 blockHandler 都配置：不超过降级规则执行 fallback 兜底处理；超过降级规则抛 BlockException 异常，被 blockHandler 处理

5.

`@SentinelResource(value = "fallback",fallback = "handlerFallback",blockHandler = "blockHandler", exceptionsToIgnore = {IllegalArgumentException.class})`

不考虑降级违规情况，发生**IllegalArgumentException**异常是不走兜底方法的。

### 2. Feign 系列

#### 1.新建模块`sentinel-85`

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>model-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

    </dependencies>
```

```yml
server:
  port: 84

spring:
  application:
    name: nacos-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080 #配置Sentinel dashboard地址
        port: 8719 #默认8719，应用与Sentinel控制台交互的端口，应用本地会起一个该端口占用的HttpServer

service-url:
  nacos-user-service: http://sentinel-provider
#对Feign的支持
feign:
  sentinel:
    enabled: true
```

```java
@EnableFeignClients
@SpringBootApplication
@EnableDiscoveryClient
public class Sentinel85 {
    public static void main(String[] args) {
        SpringApplication.run(Sentinel85.class,args);
    }
}
```

```java
@Component
@FeignClient(name = "sentinel-provider",fallback = ServiceImpl.class)
public interface Service {

    @GetMapping("/test")
    String Test();
}
```

```java
@Component
public class ServiceImpl implements Service {
    @Override
    public String Test() {
        return "feign->fallback->method()...";
    }
}
```

```java
@RestController
@Slf4j
public class TestController {

    @Autowired
    private Service service;
    @GetMapping("/test")
    public String test(){
        return service.Test();
    }
}
```

测试：

`http://localhost:85/test`

测试测试 85 调用 9003 和 9004，故意关闭 9003 和 9004 服务，发现 85 会自动降级。

## 12.规则持久化

### 1.存在的问题

一旦我们重启应用，Sentinel 规则将消失，生产环境需要将配置规则进行持久化

### 2.解决办法

```properties
将限流配置规则持久化进Nacos保存，只要刷新8007某个rest地址，
sentinel控制台的流控规则就能看到，只要Nacos里面的配置不删除，
针对8007上Sentinel上的流控规则持续有效
```

### 3.步骤（修改 8007）

#### 1.添加依赖

```xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

#### 2.配置文件

```yml
server:
  port: 8007

spring:
  application:
    name: sentinel-8007
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        port: 8719 #默认8719，应用与Sentinel控制台交互的端口，应用本地会起一个该端口占用HttpServer
      #新添加配置
      datasource:
        ds1:
          nacos:
            server-addr: localhost:8848
            dataId: sentinel-8007
            groupId: DEFAULT_GROUP
            data-type: json
            rule-type: flow
#新添加配置
management:
  endpoints:
    web:
      exposure:
        include: "*"
#新添加配置
feign:
  sentinel:
    enabled: true # 激活Sentinel对Feign的支持
#新添加配置
```

#### 3.添加 nacos 业务规则

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-NN15knKA-1601988027139)(cloud/snipaste_20200625_213914.jpg)]

| Field           | 说明                                                             | 默认值               |
| --------------- | ---------------------------------------------------------------- | -------------------- |
| resource        | 资源名，资源名是限流规则的作用对象                               |                      |
| count           | 限流阈值                                                         |                      |
| grade           | 限流阈值类型,QPS 模式(1)或并发线程数模式(0)                      | QPS                  |
| limitApp        | 流控针对的调用来源                                               | default，代表不区分  |
| strategy        | 调用关系限流策略：直接，链路，关联                               | 根据资源本身（直接） |
| controlBehavior | 流控效果（直接拒绝，排队等待，慢模式启动），不支持按调用关系限流 | 直接拒绝             |
| clusterMode     | 是否集群限流                                                     | 否                   |

#### 4.启动 8007 后刷新 sentinel 发现业务规则有了

#### 5.快速访问测试接口

`http://localhost:8007/test`

配置生效

#### 6.停止 8007 再看 sentinel

配置没了

#### 7.重启 8007 再看 sentinel

等一会就有了，持久化验证通过。

---
