---
title: SpringBoot Actuator
urlname: fet5b2
date: '2021-07-12 14:10:47 +0800'
tags: []
categories: []
---

# SpringBoot Actuator

> 文档地址：[https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/production-ready-features.html](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/production-ready-features.html)

## 1.1 Actuator 的作用

Actuator 主要是对应用程序起一个监控的作用。它可以显示应用程序员的 Health 健康信息、显示 Info 应用信息、显示 HTTP Request 跟踪信息、显示当前应用程序的“Metrics”信息、显示所有的@RequestMapping 的路径信息、显示应用程序的各种配置信息、显示你的程序请求的次数 时间 等各种信息。

## 1.2 如何集成 Actuator

第一步，在 pom.xml 中添加依赖：

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

第二步，进行配置
在 spring boot 2.0 以后,actuator 默认只开启了 info 和 health 两个端点,要想使用其他的端点,需要在 application.yml 中打开,也就是进行如下配置：

```java
management:
  endpoints:
    web:
      exposure:
        include: '*'
  endpoint:
    health:
      show-details: ALWAYS
```

第三步，访问
[http://localhost:8080/actuator](http://localhost:8080/actuator)
[http://localhost:8080/actuator](http://localhost:8080/actuator)/{具体模块}

## 1.3 Actuator 有哪些端点？

```java
#这个信息可以通过访问http://localhost:8080/actuator获得
{
	"_links": {
		"self": {
			"href": "http://localhost:8080/actuator",
			"templated": false
		},
		"auditevents": {
			"href": "http://localhost:8080/actuator/auditevents",
			"templated": false
		},
		"beans": {
			"href": "http://localhost:8080/actuator/beans",
			"templated": false
		},
		"caches-cache": {
			"href": "http://localhost:8080/actuator/caches/{cache}",
			"templated": true
		},
		"caches": {
			"href": "http://localhost:8080/actuator/caches",
			"templated": false
		},
		"health": {
			"href": "http://localhost:8080/actuator/health",
			"templated": false
		},
		"health-component": {
			"href": "http://localhost:8080/actuator/health/{component}",
			"templated": true
		},
		"health-component-instance": {
			"href": "http://localhost:8080/actuator/health/{component}/{instance}",
			"templated": true
		},
		"conditions": {
			"href": "http://localhost:8080/actuator/conditions",
			"templated": false
		},
		"configprops": {
			"href": "http://localhost:8080/actuator/configprops",
			"templated": false
		},
		"env": {
			"href": "http://localhost:8080/actuator/env",
			"templated": false
		},
		"env-toMatch": {
			"href": "http://localhost:8080/actuator/env/{toMatch}",
			"templated": true
		},
		"info": {
			"href": "http://localhost:8080/actuator/info",
			"templated": false
		},
		"loggers": {
			"href": "http://localhost:8080/actuator/loggers",
			"templated": false
		},
		"loggers-name": {
			"href": "http://localhost:8080/actuator/loggers/{name}",
			"templated": true
		},
		"heapdump": {
			"href": "http://localhost:8080/actuator/heapdump",
			"templated": false
		},
		"threaddump": {
			"href": "http://localhost:8080/actuator/threaddump",
			"templated": false
		},
		"metrics": {
			"href": "http://localhost:8080/actuator/metrics",
			"templated": false
		},
		"metrics-requiredMetricName": {
			"href": "http://localhost:8080/actuator/metrics/{requiredMetricName}",
			"templated": true
		},
		"scheduledtasks": {
			"href": "http://localhost:8080/actuator/scheduledtasks",
			"templated": false
		},
		"httptrace": {
			"href": "http://localhost:8080/actuator/httptrace",
			"templated": false
		},
		"mappings": {
			"href": "http://localhost:8080/actuator/mappings",
			"templated": false
		}
	}
}
```

![image-20200426115050413.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590053665476-9b5a4102-2af1-42bc-8e58-43623f53c16a.png#height=605&id=of25U&margin=%5Bobject%20Object%5D&name=image-20200426115050413.png&originHeight=605&originWidth=774&originalType=binary∶=1&size=68687&status=done&style=none&width=774)
![image-20200426115104314.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1590053685651-6084670a-a83f-4dab-8b31-ea58c37d5984.png#height=154&id=Sa2Zh&margin=%5Bobject%20Object%5D&name=image-20200426115104314.png&originHeight=154&originWidth=772&originalType=binary∶=1&size=17600&status=done&style=none&width=772)

## 1.4 Actuator 端点详解

### 1.4.1 health

主要用来检测应用的运行状况，是使用最多的一个监控点。监控软件通常使用该接口实时监测应用运行状况，在系统出现故障时把报警信息推送给相关人员，如磁盘空间使用情况、数据库和缓存等的一些健康指标。
默认情况下 health 端点是开放的，访问 [http://127.0.0.1:8080/actuator/health](http://127.0.0.1:8080/actuator/health) 即可看到应用运行状态。

> {"status":"UP"}

如果需要看到详细信息，则需要做添加配置：

> management.endpoint.health.show-details=always

访问返回信息如下：

> {"status":"UP","details":{"diskSpace":{"status":"UP","details":{"total":180002725888,"free":8687988736,"threshold":10485760}}}}

​

### 1.4.2 info

查看应用信息是否在 application.properties 中配置。如我们在项目中配置是：

```java
info.app.name=Spring Boot Actuator Demo
info.app.version=v1.0.0
info.app.description=Spring Boot Actuator Demo
```

启动项目，访问 [http://127.0.0.1:8080/actuator/info](http://127.0.0.1:8080/actuator/info) 返回信息如下：

```java
{"app":{"name":"Spring Boot Actuator Demo","version":"v1.0.0","description":"Spring Boot Actuator Demo"}}
```

### 1.4.3 env

通过 env 可以获取到所有关于当前 Spring Boot 应用程序的运行环境信息，如：操作系统信息（systemProperties）、环境变量信息、JDK 版本及 ClassPath 信息、当前启用的配置文件（activeProfiles）、propertySources、应用程序配置信息（applicationConfig）等。
可以通过 [http://127.0.0.1:8080/actuator/env/{name}](http://127.0.0.1:8080/actuator/env/%7Bname%7D) ，name 表示想要查看的信息，可以独立显示。

比如：[http://localhost:8080/actuator/env/activeProfiles](http://localhost:9080/actuator/env/activeProfiles)

```java
{
	"activeProfiles": [],
	"propertySources": [{
		"name": "server.ports"
	}, {
		"name": "servletConfigInitParams"
	}, {
		"name": "servletContextInitParams"
	}, {
		"name": "systemProperties"
	}, {
		"name": "systemEnvironment"
	}, {
		"name": "random"
	}, {
		"name": "applicationConfig: [classpath:/application.yml]"
	}, {
		"name": "Management Server"
	}]
}
```

​

### 1.4.4 beans

访问 [http://127.0.0.1:8080/actuator/beans](http://127.0.0.1:8080/actuator/beans) 返回部分信息如下：

```java
{
    "contexts": {
        "Spring Boot Actuator Demo": {
            "beans": {
                "endpointCachingOperationInvokerAdvisor": {
                    "aliases": [
                    ],
                    "scope": "singleton",
                    "type": "org.springframework.boot.actuate.endpoint.invoker.cache.CachingOperationInvokerAdvisor",
                    "resource": "class path resource [org/springframework/boot/actuate/autoconfigure/endpoint/EndpointAutoConfiguration.class]",
                    "dependencies": [
                        "environment"
                    ]
                },
                "defaultServletHandlerMapping": {
                    "aliases": [
                    ],
                    "scope": "singleton",
                    "type": "org.springframework.web.servlet.HandlerMapping",
                    "resource": "class path resource [org/springframework/boot/autoconfigure/web/servlet/WebMvcAutoConfiguration$EnableWebMvcConfiguration.class]",
                    "dependencies": [
                    ]
                },
                ...
            }
        }
    }
}
```

从返回的信息中我们可以看出主要展示了 bean 的别名、类型、是否单例、类的地址、依赖等信息。
​

### 1.4.5 conditions

通过 conditions 可以在应用运行时查看代码了某个配置在什么条件下生效，或者某个自动配置为什么没有生效。
访问 [http://127.0.0.1:8080/actuator/conditions](http://127.0.0.1:8080/actuator/conditions) 返回部分信息如下：

```java
{
    "contexts": {
        "Spring Boot Actuator Demo": {
            "positiveMatches": {
                "SpringBootAdminClientAutoConfiguration": [
                    {
                        "condition": "OnWebApplicationCondition",
                        "message": "@ConditionalOnWebApplication (required) found 'session' scope"
                    },
                    {
                        "condition": "SpringBootAdminClientEnabledCondition",
                        "message": "matched"
                    }
                ],
                "SpringBootAdminClientAutoConfiguration#metadataContributor": [
                    {
                        "condition": "OnBeanCondition",
                        "message": "@ConditionalOnMissingBean (types: de.codecentric.boot.admin.client.registration.metadata.CompositeMetadataContributor; SearchStrategy: all) did not find any beans"
                    }
                ],
                ...
            }
        }
    }
}
```

​

### 1.4.6 loggers

获取系统的日志信息。
访问 [http://127.0.0.1:8080/actuator/loggers](http://127.0.0.1:8080/actuator/loggers) 返回部分信息如下：

```java
{
    "levels": [
        "OFF",
        "ERROR",
        "WARN",
        "INFO",
        "DEBUG",
        "TRACE"
    ],
    "loggers": {
        "ROOT": {
            "configuredLevel": "INFO",
            "effectiveLevel": "INFO"
        },
        "cn": {
            "configuredLevel": null,
            "effectiveLevel": "INFO"
        },
        "cn.zwqh": {
            "configuredLevel": null,
            "effectiveLevel": "INFO"
        },
        "cn.zwqh.springboot": {
            "configuredLevel": null,
            "effectiveLevel": "INFO"
        },
        ...
    }
}
```

​

### 1.4.7 mappings

查看所有 URL 映射，即所有 @RequestMapping 路径的整理列表。
访问 [http://127.0.0.1:8080/actuator/mappings](http://127.0.0.1:8080/actuator/mappings) 返回部分信息如下：

```java
{
    "contexts": {
        "Spring Boot Actuator Demo": {
            "mappings": {
                "dispatcherServlets": {
                    "dispatcherServlet": [
                        {
                            "handler": "ResourceHttpRequestHandler [class path resource [META-INF/resources/], class path resource [resources/], class path resource [static/], class path resource [public/], ServletContext resource [/], class path resource []]",
                            "predicate": "/**/favicon.ico",
                            "details": null
                        },
                        ...
                    ]
                }
            }
        }
    }
}
```

​

### 1.4.8 heapdump

访问：[http://127.0.0.1:8080/actuator/heapdump 会自动生成一个](http://127.0.0.1:8080/actuator/heapdump%E4%BC%9A%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E4%B8%80%E4%B8%AA) GZip 压缩的 Jvm 的堆文件 heapdump,我们可以使用 JDK 自带的 Jvm 监控工具 VisualVM 打开此文件查看。如图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1596526618843-b1b16038-d75c-4c8e-931d-a519ccca0a4c.png#height=470&id=aN4z5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=470&originWidth=804&originalType=binary∶=1&size=177585&status=done&style=none&width=804)

### 1.4.9 threaddump

获取系统线程的转储信息，主要展示了线程名、线程 ID、线程的状态、是否等待锁资源等信息。在工作中，我们可以通过查看线程的情况来排查相关问题。
访问 [http://127.0.0.1:8080/actuator/threaddump](http://127.0.0.1:8080/actuator/threaddump) 返回部分信息如下：

```java
{
    "threads": [
        {
            "threadName": "DestroyJavaVM",
            "threadId": 40,
            "blockedTime": -1,
            "blockedCount": 0,
            "waitedTime": -1,
            "waitedCount": 0,
            "lockName": null,
            "lockOwnerId": -1,
            "lockOwnerName": null,
            "inNative": false,
            "suspended": false,
            "threadState": "RUNNABLE",
            "stackTrace": [
            ],
            "lockedMonitors": [
            ],
            "lockedSynchronizers": [
            ],
            "lockInfo": null
        },
        ...
    ]
}
```

### 1.4.10 shutdown

开启可以接口关闭 Spring Boot 应用，要使用这个功能需要做如下配置：

> management.endpoint.shutdown.enabled=true

可以通过 post（仅支持 post） 请求访问  [http://127.0.0.1:8080/actuator/shutdown](http://127.0.0.1:8080/actuator/shutdown)  关闭应用。

### 1.4.11 metrics

访问  [http://127.0.0.1:8080/actuator/metrics](http://127.0.0.1:8080/actuator/metrics)  可以获取系统度量指标信息项如下：

```java
{
    "names": [
        "jvm.memory.max",
        "jvm.threads.states",
        "jvm.gc.pause",
        "http.server.requests",
        "process.files.max",
        "jvm.gc.memory.promoted",
        "system.load.average.1m",
        "jvm.memory.used",
        "jvm.gc.max.data.size",
        "jvm.memory.committed",
        "system.cpu.count",
        "logback.events",
        "tomcat.global.sent",
        "jvm.buffer.memory.used",
        "tomcat.sessions.created",
        "jvm.threads.daemon",
        "system.cpu.usage",
        "jvm.gc.memory.allocated",
        "tomcat.global.request.max",
        "tomcat.global.request",
        "tomcat.sessions.expired",
        "jvm.threads.live",
        "jvm.threads.peak",
        "tomcat.global.received",
        "process.uptime",
        "tomcat.sessions.rejected",
        "process.cpu.usage",
        "tomcat.threads.config.max",
        "jvm.classes.loaded",
        "jvm.classes.unloaded",
        "tomcat.global.error",
        "tomcat.sessions.active.current",
        "tomcat.sessions.alive.max",
        "jvm.gc.live.data.size",
        "tomcat.threads.current",
        "process.files.open",
        "jvm.buffer.count",
        "jvm.buffer.total.capacity",
        "tomcat.sessions.active.max",
        "tomcat.threads.busy",
        "process.start.time"
    ]
}
```

对应访问 names 中的指标，可以查看具体的指标信息。如访问  [http://127.0.0.1:8080/actuator/metrics/jvm.memory.used](http://127.0.0.1:8080/actuator/metrics/jvm.memory.used)  返回信息如下：

```java
{
    "name": "jvm.memory.used",
    "description": "The amount of used memory",
    "baseUnit": "bytes",
    "measurements": [
        {
            "statistic": "VALUE",
            "value": 1.16828136E8
        }
    ],
    "availableTags": [
        {
            "tag": "area",
            "values": [
                "heap",
                "nonheap"
            ]
        },
        {
            "tag": "id",
            "values": [
                "Compressed Class Space",
                "PS Survivor Space",
                "PS Old Gen",
                "Metaspace",
                "PS Eden Space",
                "Code Cache"
            ]
        }
    ]
}
```

## 1.5 Actuator 集成安全认证

第一步，加入依赖

```java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
```

​

第二步，加入配置

```java
spring:
  security:
    user:
      name: root
      password: admin
```

第三步，访问，会跳出需要登录
![image.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1596525772806-1533a041-3f5f-4706-b5d1-6af8543b7ac8.png#height=320&id=NlUsa&margin=%5Bobject%20Object%5D&name=image.png&originHeight=320&originWidth=741&originalType=binary∶=1&size=12449&status=done&style=none&width=741)
