---
title: SpringBoot集成数据库
urlname: tondvp
date: '2021-07-12 14:12:32 +0800'
tags: []
categories: []
abbrlink: 54903
---

# SpringBoot 集成数据库

## 1.1 集成 DruidDataSource 连接池

第一步，在 pom.xml 中加入

```java
        <!-- MYSQL -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.48</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
            <version>2.3.0.RELEASE</version>
        </dependency>
        <!-- druid数据源 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.5</version>
        </dependency>
```

第二步，配置 application.yml

```
#数据库配置
spring:
  #数据源配置
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:mysql://${DEV.HOST:localhost}:3306/cloud?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&pinGlobalTxToPhysicalConnection=true&useSSL=false
    username: root
    password: root
    initialSize: 5  #初始建立连接数量
    minIdle: 5  #最小连接数量
    maxActive: 20 #最大连接数量
    maxWait: 10000  #获取连接最大等待时间，毫秒
    testOnBorrow: true #申请连接时检测连接是否有效
    testOnReturn: false #归还连接时检测连接是否有效
    timeBetweenEvictionRunsMillis: 60000 #配置间隔检测连接是否有效的时间（单位是毫秒）
    minEvictableIdleTimeMillis: 300000  #连接在连接池的最小生存时间（毫秒）
```

## 1.2 springboot2.x 默认 Hikari 连接池

Hikari 是一款非常强大，高效，并且号称“史上最快连接池”。并且在 springboot2.0 之后，采用的默认数据库连接池就是 Hikari。不需要引入依赖，已经在 SpringBoot 中包含了。

```
# 数据库配置
spring:
  datasource:
  type: com.zaxxer.hikari.HikariDataSource
  driverClassName: com.mysql.jdbc.Driver
  url: jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=utf-8&useSSL=false
  username: root
  password: root
  # Hikari 连接池配置
  # 最小空闲连接数量
  hikari:
    minimum-idle: 5
    # 空闲连接存活最大时间，默认600000（10分钟）
    idle-timeout: 180000
    # 连接池最大连接数，默认是10
    maximum-pool-size: 10
    # 此属性控制从池返回的连接的默认自动提交行为,默认值：true
    auto-commit: true
    # 连接池名称
    pool-name: MyHikariCP
    # 此属性控制池中连接的最长生命周期，值0表示无限生命周期，默认1800000即30分钟
    max-lifetime: 1800000
    # 数据库连接超时时间,默认30秒，即30000
    connection-timeout: 30000
    connection-test-query: SELECT 1
```

Hikari 配置说明：
![微信图片_20200828170339.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1598605441710-9e5ea601-af78-4221-95ba-29d68f331793.png#height=775&id=i6l5u&margin=%5Bobject%20Object%5D&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20200828170339.png&originHeight=775&originWidth=967&originalType=binary∶=1&size=156082&status=done&style=none&width=967)
![微信图片_20200828170346.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1598605451711-04ff669c-aebe-4388-a49e-8cbbc12f5dee.png#height=801&id=D1tum&margin=%5Bobject%20Object%5D&name=%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20200828170346.png&originHeight=801&originWidth=970&originalType=binary∶=1&size=161892&status=done&style=none&width=970)
