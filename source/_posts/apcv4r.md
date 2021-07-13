---
title: SpringBoot集成mybatis
urlname: apcv4r
date: '2021-07-12 14:12:53 +0800'
author: 阿松
top: true
hide: false
cover: false
toc: false
mathjax: false
summary: SpringBoot集成mybatis
categories: springBoot
tags:
  - springBoot
  - 后端
  - mybatis
  - 数据库
  - spring
---

# SpringBoot 集成 mybatis

## 7.1 集成

step1，在 pom.xml 中加入

```java
<!--mybatis-->
<dependency>
	<groupId>org.mybatis.spring.boot</groupId>
	<artifactId>mybatis-spring-boot-starter</artifactId>
	<version>1.3.0</version>
</dependency>
```

step2，配置 application.yml

```java
#mybatis配置
mybatis:
  type-aliases-package: com.cloud.**.entity				#映射实体地址
  mapper-locations: classpath:mybatis/mapper/**/*.xml	#xml配置文件地址
  configuration:
    jdbc-type-for-null: null							#当传入null的时候对应的jdbctype
  config-location: classpath:mybatis/mybatis-config.xml	#mybatis全局配置
```

step3，配置 mybatis-config.xml

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
    </typeAliases>
</configuration>
```
