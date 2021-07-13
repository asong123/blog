---
title: SpringBoot日志
urlname: fob9fm
date: '2021-07-12 14:10:24 +0800'
author: 阿松
top: true
hide: false
cover: false
toc: false
mathjax: false
summary: SpringBoot日志
categories: SpringBoot
tags:
  - SpringBoot
  - 后端
  - SpringBoot日志
---

# SpringBoot 日志

## 1.1 日志隔离级别

日志级别总共有 TRACE < DEBUG < INFO < WARN < ERROR < FATAL ，且级别是逐渐提供，如果日志级别设置为 INFO，则意味 TRACE 和 DEBUG 级别的日志都看不到。

## 1.2 SpringBoot 的日志关系

Spring Boot 默认使用 LogBack 日志系统，如果不需要更改为其他日志系统如 Log4j2 等，则无需多余的配置，LogBack 默认将日志打印到控制台上。

如果要使用 LogBack，原则上是需要添加 dependency 依赖的：

```java
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-logging</artifactId>
```

但是 spring-boot-starter`或者`spring-boot-starter-web 已经帮我们引入了这两个包

**SpringBoot 日志的底层依赖关系：**

![1646268-20190709113542010-191320310.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590053375265-8ab2b1fa-75b2-45a1-8f44-19f65f07f793.png#height=339&id=tHiQu&margin=%5Bobject%20Object%5D&name=1646268-20190709113542010-191320310.png&originHeight=339&originWidth=871&originalType=binary∶=1&size=30773&status=done&style=none&width=871)

通过这张图我们能总结出：
1）Spring Boot 底层也是使用 slf4j+logback 的方式进行日志记录
2）Spring Boot 也把其他的日志都替换成了 slf4j；

## 1.3 springboot 修改日志实现

我们可以给类路径下放上每个日志框架自己的配置文件，SpringBoot 就不使用他默认配置的了：
![image-20200426104826775.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590053451129-322404f4-9ec8-4d67-8345-bac0750a35ae.png#height=197&id=jBNgr&margin=%5Bobject%20Object%5D&name=image-20200426104826775.png&originHeight=197&originWidth=739&originalType=binary∶=1&size=13497&status=done&style=none&width=739)
我们可以通过配置日志文件来覆盖 SpringBoot 的默认配置。

## 1.4 通过 application.yml 配置

### 1.4.1 全局日志级别配置

SpringBoot 默认的隔离级别为**INFO**，如果要全局修改 SpringBoot 的隔离级别，那么需配置：

```java
logging:
  level:
    root: warn
```

### 1.4.2 包日志级别配置

```java
logging:
  level:
    root: INFO
    com:
      cloud: DEBUG	#com.cloud为包路径
```

### 1.4.3 日志文件存储配置

```yaml
# 日志打印
logging:
  level:
    root: DEBUG #root为全局日志打印级别
    com:
      cloud:
        bmsoft: INFO #具体包路径下的日志打印级别
  path: /Users/jackie/workspace/rome/ #日志存储路径
  file: springbootdemo.log #日志存储名称
  max-size: 10MB #存储日志文件大小
  pattern:
    console: %d{yyyy/MM/dd-HH:mm:ss} [%thread] %-5level %logger- %msg%n #控制台输出格式
    file: %d{yyyy/MM/dd-HH:mm} [%thread] %-5level %logger- %msg%n #文件输出格式
```

> pattern 说明

```json
#pattern说明
%d{HH:mm:ss.SSS}——日志输出时间
%thread——输出日志的进程名字，这在Web应用以及异步任务处理中很有用
%-5level——日志级别，并且使用5个字符靠左对齐
%logger- ——日志输出者的名字
%msg——日志消息
%n——平台的换行符
```

## 1.5 logback.xml 配置文件详解

### 1.5.1 配置文件架构

![image-20200426110102111.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590053515415-db18b3bf-de8b-4182-aa04-3704a3b6bb7a.png#height=558&id=LCS9T&margin=%5Bobject%20Object%5D&name=image-20200426110102111.png&originHeight=558&originWidth=1143&originalType=binary∶=1&size=25475&status=done&style=none&width=1143)

### 1.5.2 appender 类型

1）ConsoleAppender 把日志输出到控制台
2）FileAppender：把日志添加到文件
3）RollingFileAppender：滚动记录文件，先将日志记录到指定文件，当符合某个条件时，将日志记录到其他文件。

### 1.5.3 日志级别过滤

作用：过滤滚动日志的级别

```json
<appender name="ERROR" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<!-- filter过滤器，INFO级别 -->
		<filter class="ch.qos.logback.classic.filter.LevelFilter">
				<level>INFO</level>
				<onMatch>ACCEPT</onMatch>
				<onMismatch>DENY</onMismatch>
		</filter>
</appender>
```

### 1.5.4 滚动日志实现方式

#### 1.5.4.1 TimeBasedRollingPolicy

作用：按照时间滚动，fileNamePattern 属性设置滚动生成文件的格式,这里设置的精确到天，也就是按照天滚动，如果时间设置精确到秒，就按秒来滚动，maxHistory 属性设定最大的文件数，比如按天滚动，这里设置了 30 天，在第 31 天日志生成的时候，第一天的日志就会被删掉。

```json
<!-- 滚动文件的方式生成日志日志文件，文件的存储位置通过file标签指定 -->
<!-- 通过encoder指定日志的生成格式,每个appender的日志格式都可以自定义，不用相同 -->
<appender name="log" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>/logs/test.log</file>
    <encoder>
        <pattern>【logbck】%blue([requestId:%X{requestId:-syslogId}]) %d{yyyy-MM-dd HH:mm:ss.SSS} %red([%thread]) %5level - %msg%n</pattern>
    </encoder>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <fileNamePattern>/logs/test-%d{yyyy-MM-dd}.log</fileNamePattern>
        <maxHistory>30</maxHistory>
    </rollingPolicy>
</appender>
```

说明：fileNamePattern 是要格外说明的。如上我设置按天来滚动，前一天日志打印到 23 点 59 分，然后就一直没有请求日志，直到次日的 1 点才有新的日志进入。在 0 点到 1 点这个时间段，日志文件是不会滚动生成新的日志文件。因为滚动的动作是需要日志写入动作来触发。

#### 1.5.4.2 FixedWindowRollingPolicy

作用：按照索引的方式来滚动日志，使用%i 作为占位符，指定按照文件大小来触发。

```json
<!-- 设置为按照索引的方式滚动，定义文件名称的时候使用%i作为占位符，滚动后会会用角标替换 -->
<rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
    <fileNamePattern>/logback/log/test-%i.log</fileNamePattern>
    <minIndex>1</minIndex>
    <maxIndex>3</maxIndex>
</rollingPolicy>
<!-- 指定文件最大尺寸，达到该尺寸，就触发rollingPolicy对应的策略，maxFileSize属性指定文件大小 -->
<triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
    <maxFileSize>1MB</maxFileSize>
</triggeringPolicy>
```

说明：

- maxIndex 不可设置为过大，过大后会自动被置为默认值 12，这个过大的值具体是多少没测试过（欢迎小伙伴测试后在文末回复一下，就偷个懒了）。
- 第一次滚动，生成 test-1.log，第二次滚动时会将之前的 test-1.log 文件修改成 test-2.log，将最新生成的日志文件命名为 test-1.log，依此类推，指导索引达到设置的最大值。当超过最大值的时候，会将索引最大的文件直接删除，用前一个文件移到最后一位。就像传送带，最先放上的物品会最先掉出传送带。
- 在时间滚动的方式里面，可以用时间作为触发机制。但是在使用索引方式的时候，需要定义一个 triggeringPolicy 作为触发机制。

#### 1.5.4.3 SizeAndTimeBasedFNATP

作用：时间策略并配合文件大小触发器来实现日志文件定义

```json
<!-- 使用按照时间滚动策略，内嵌按照文件大小来分隔日志的触发器策略 -->
<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
    <fileNamePattern>/logback/log/test-%d{yyyy-MM-dd}-%i.log</fileNamePattern>
    <!-- 使用SizeAndTimeBasedFNATP实现，可以看一下TimeBasedRollingPolicy源码中对应timeBasedFileNamingAndTriggeringPolicy的类型，根据类型确定需要使用的class类 -->
    <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
        <maxFileSize>1MB</maxFileSize>
    </timeBasedFileNamingAndTriggeringPolicy>
</rollingPolicy>
```

说明：按照时间滚动的单位是天，按照文件大小的滚动是 1M，当今天产生的日志大小是 5M，就会生成 5 个日志文件，每个文件对应的大小是 1M，对应生成日期是一天，对应的编号不同，也就是%i 占位符替换后的编号(从 0 开始)。

#### 1.5.4.4 SizeAndTimeBasedRollingPolicy

作用：按照时间和大小来实现日志文件定义，感觉跟 1.5.4.3 作用是一样的。

```java
        <!-- 定义日志文件大小和时间策略 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>
                ${log.home_dir}/errorLog-%d{yyyy-MM-dd}.%i.log
            </fileNamePattern>
            <!-- 当每个文件达到100MB，会生成新的日志文件，日志文件最多保存60天，总日志文件大小达到20GB，会删除旧日志-->
            <maxFileSize>${log.maxFileSize}</maxFileSize>
            <maxHistory>${log.maxHistory}</maxHistory>
            <totalSizeCap>${log.totalSizeCap}</totalSizeCap>
        </rollingPolicy>
```

​

### 1.5.5 pattern 配置详解

```json
%d{HH:mm:ss.SSS}——日志输出时间
%thread——输出日志的进程名字，这在Web应用以及异步任务处理中很有用
%-5level——日志级别，并且使用5个字符靠左对齐
%-5p——日志级别，并且使用5个字符靠左对齐
%blue(xxx)——这段日志输出为蓝色
%red(xxx)——这段日志输出为红色
%logger- ——日志输出者的名字
%msg——日志消息
%n——平台的换行符
```

**​**

### 1.5.6 配置文件例子

```java
<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true" scanPeriod="60 seconds" debug="false">
    <!--scan: 此属性设置为true时，配置文件如果发生改变，将会被重新加载，默认值为true-->
    <!--scanPeriod: 设置监测配置文件是否有修改的时间间隔,当scan为true时生效-->
    <!--debug: 当此属性设置为true时，将打印出logback内部日志信息.默认false-->

    <!-- 定义日志文件 输出位置 -->
    <property name="log.home_dir" value="D:/logs" />

    <property name="log.maxHistory" value="60"/>
    <property name="log.maxFileSize" value="100MB" />
    <property name="log.totalSizeCap" value="20GB"/>


    <!--ConsoleAppender 控制台输出日志 -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} -| %boldGreen([%thread]) -| %highlight(%-5level) -| traceId=[%X{traceId}] -| %boldYellow(%logger{36}) -| %msg%n</pattern>
            </pattern>
        </encoder>
    </appender>


    <!-- RollingFileAppender 文件输出日志 ERROR级别-->
    <appender name="ERROR" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 过滤器，只记录ERROR级别的日志 -->
        <!-- 如果日志级别等于配置级别，过滤器会根据onMath 和 onMismatch接收或拒绝日志。 -->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <!-- 设置过滤级别 -->
            <level>ERROR</level>
            <!-- 用于配置符合过滤条件的操作 -->
            <onMatch>ACCEPT</onMatch>
            <!-- 用于配置不符合过滤条件的操作 -->
            <onMismatch>DENY</onMismatch>
        </filter>
        <!-- 定义日志文件大小和时间策略 -->
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>
                ${log.home_dir}/errorLog-%d{yyyy-MM-dd}.%i.log
            </fileNamePattern>
            <!-- 当每个文件达到100MB，会生成新的日志文件，日志文件最多保存60天，总日志文件大小达到20GB，会删除旧日志-->
            <maxFileSize>${log.maxFileSize}</maxFileSize>
            <maxHistory>${log.maxHistory}</maxHistory>
            <totalSizeCap>${log.totalSizeCap}</totalSizeCap>
        </rollingPolicy>
        <encoder>
            <pattern>
                <!-- 设置日志输出格式 -->
                %d{yyyy-MM-dd HH:mm:ss.SSS} -| [%thread] -| %-5level -| traceId=[%X{traceId}] -| %logger{36} -| %msg%n
            </pattern>
        </encoder>
    </appender>


    <!-- INFO级别-->
    <appender name="INFO" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>INFO</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>${log.home_dir}/appLog-%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <maxFileSize>${log.maxFileSize}</maxFileSize>
            <maxHistory>${log.maxHistory}</maxHistory>
            <totalSizeCap>${log.totalSizeCap}</totalSizeCap>
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} -| [%thread] -| %-5level -| traceId=[%X{traceId}] -| %logger{36} -| %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="ERROR"/>
        <appender-ref ref="INFO"/>
    </root>

</configuration>
```

## 1.6 自定义系统异常日志过滤

```java
@Slf4j
public class ThrowableFilter extends TurboFilter {

    @Override
    public FilterReply decide(Marker marker, Logger logger, Level level, String format, Object[] params, Throwable throwable) {
        if (!isStarted()) {
            return FilterReply.NEUTRAL;
        }
        Throwable t = throwable;
        if (Objects.isNull(t)) {
            if (Objects.nonNull(params) && params.length > 0) {
                val e = params[params.length - 1];
                if (e instanceof Throwable) {
                    t = (Throwable) e;
                } else {
                    return FilterReply.NEUTRAL;
                }
            } else {
                return FilterReply.NEUTRAL;
            }
        }
        Set<StackTraceElement> traceLines = ExceptionUtil.getStackTraceElements(t);
        t.setStackTrace(traceLines.toArray(new StackTraceElement[0]));
        return FilterReply.NEUTRAL;
    }

}
```

```
public class ExceptionUtil {

    public static Set<StackTraceElement> getStackTraceElements(Throwable t) {
        StackTraceElement[] stackTrace = t.getStackTrace();
        Set<StackTraceElement> traceLines = Sets.newLinkedHashSetWithExpectedSize(10);
        for (StackTraceElement stack : stackTrace) {
            String line = stack.toString();
            if (checkIsSystemPath(line)) {
                traceLines.add(stack);
            }
        }
        return traceLines;
    }

    private static boolean checkIsSystemPath(String line) {
        if (line.startsWith("com.cloud")) {
            return true;
        } else if (line.startsWith("\tat com.cloud")) {
            return true;
        } else if (line.startsWith("Caused")) {
            return true;
        } else {
            return line.startsWith("\t");
        }
    }

    public static String getSimpleStackTrace(Throwable throwable) {
        String wholeStackTraceString = ExceptionUtils.getStackTrace(throwable);
        String[] lines = StringUtils.split(wholeStackTraceString, "\n");
        Set<String> traceLines = Sets.newLinkedHashSetWithExpectedSize(10);
        for (String line : lines) {
            if (checkIsSystemPath(line)) {
                traceLines.add(line);
            }
        }
        return StringUtils.join(traceLines, "\n");
    }

    public static String getMessage(Throwable throwable) {
        return Match(throwable).of(
                Case($(Predicates.instanceOf(BusinessException.class)), ((BusinessException) throwable)::getMsg),
                Case($(), throwable::getMessage)
        );
    }

}
```

在现有的 logback-spring.xml 的 configuration 标签下添加如下配置：

```
<turboFilter class="com.cloud.simul.tool.log.ThrowableFilter" />
```

## 1.7 微服务调用 TraceId 日志追踪
