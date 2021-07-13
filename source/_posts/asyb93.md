---
title: SpringBoot集成Redis
urlname: asyb93
date: '2021-07-12 14:11:34 +0800'
author: 阿松
top: true
hide: false
cover: false
toc: false
mathjax: false
summary: SpringBoot集成Redis
categories: springBoot
tags:
  - springBoot
  - 后端
  - Redis
  - 数据库
  - spring
---

# SpringBoot 集成 Redis

## 1.1 redis 默认配置

![image.png](https://cdn.nlark.com/yuque/0/2020/png/635741/1596533365941-3c14c4ad-d4f3-44d6-968d-87711255a21f.png#height=514&id=X0BIs&margin=%5Bobject%20Object%5D&name=image.png&originHeight=514&originWidth=718&originalType=binary∶=1&size=38705&status=done&style=none&width=718)
说明：在 RedisAutoConfiguration 中，我们可以看到 SpringBoot 默认给我们提供的 RedisTemplate 方式，分别实现了 RedisTemplate<Object,Object>，这种实现方式底层用的是 JdkSerializationRedisSerializer，可以通过源码查看，还有实现了 StringRedisTemplate，这种 redis 的序列化方式是 StringRedisSerializer，也可以通过源码查看。

## 1.1 如何集成 redis

step1 引入依赖包：

```java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
         <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
```

step2 redisTemplate 配置文件配置：

```java
    @Bean(name = "redisTemplate")
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory connectionFactory,RedisProperties redisProperties) {
        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(connectionFactory);
        // 使用Jackson2JsonRedisSerialize替换默认序列化方式
        Jackson2JsonRedisSerializer<?> jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<>(Object.class);
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        //启用默认的类型
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        //序列化类，对象映射设置
        jackson2JsonRedisSerializer.setObjectMapper(om);
        redisTemplate.setKeySerializer(stringRedisSerializer);
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.setHashKeySerializer(stringRedisSerializer);
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }
```

step3 application.yml 配置文件配置：

```java
spring:
  redis:
    database: 0
    host: ${REDIS.HOST:localhost}
    port: 6379
    password:
    timeout: 10000ms
    lettuce:
      pool:
        max-active: 8
        max-wait: -1ms
        max-idle: 8
        min-idle: 0
```

step4 如何使用 RedisTemplate

```java
@RestController
public class DemoController {

    @Autowired
    RedisTemplate<Object, Object> redisTemplate;

    @RequestMapping("/test")
    public void testRedis(){
        Person p1 = new Person();
        p1.setAge(18);
        p1.setName("liulijun");

        redisTemplate.opsForValue().set("Person:1",p1);
        Person p1Redis = (Person)redisTemplate.opsForValue().get("Person:1");

        Person p2 = new Person();
        p2.setAge(20);
        p2.setName("liulijun");

        Person p3 = new Person();
        p2.setAge(20);
        p2.setName("liulijun");

        List<Person> personList = new ArrayList<>();
        personList.add(p2);
        personList.add(p3);

        redisTemplate.opsForValue().set("Person:2",personList);
        List<Person> personListRedis1 = (List<Person>) redisTemplate.opsForValue().get("Person:2");

        redisTemplate.opsForList().leftPushAll("Person:List",personList);
        List<Person> personListRedis2 = (List<Person>)redisTemplate.opsForList().rightPop("Person:List");

    }
}
```

注意：要引入 RedisTemplate<Object, Object>,不能直接引入 RedisTemplate<Object, Person>，不然会报错。

## 1.2 序列化方式比较

> GenericToStringSerializer: 可以将任何对象泛化为字符串并序列化
> Jackson2JsonRedisSerializer: 跟 JacksonJsonRedisSerializer 实际上是一样的
> JacksonJsonRedisSerializer: 序列化 object 对象为 json 字符串
> JdkSerializationRedisSerializer: 序列化 java 对象
> StringRedisSerializer: 简单的字符串序列化

压测结果：

> JdkSerializationRedisSerializer 序列化时间：8ms,序列化后的长度：1325
> JdkSerializationRedisSerializer 反序列化时间：4
> GenericJackson2JsonRedisSerializer 序列化时间：52ms,序列化后的长度：17425
> GenericJackson2JsonRedisSerializer 反序列化时间：60
> Jackson2JsonRedisSerializer 序列化时间：4ms,序列化后的长度：9801
> Jackson2JsonRedisSerializer 反序列化时间：4

性能总结：

> JdkSerializationRedisSerializer 序列化后长度最小，Jackson2JsonRedisSerializer 效率最高。
> 如果综合考虑效率和可读性，牺牲部分空间，推荐 key 使用 StringRedisSerializer，保持的 key 简明易读；value 可以使用 Jackson2JsonRedisSerializer
> 如果空间比较敏感，效率要求不高，推荐 key 使用 StringRedisSerializer，保持的 key 简明易读；value 可以使用 JdkSerializationRedisSerializer
