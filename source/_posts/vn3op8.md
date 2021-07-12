---
title: Mybatis-Plus
urlname: vn3op8
date: '2021-07-12 12:45:46 +0800'
author: 阿松
top: false
hide: false
cover: false
toc: false
mathjax: false
summary: 项目中经常会遇到一些数据，每次都使用相同的方式填充，例如记录的创建时间，更新时间等。
categories: Mybatis
tags:
  - java
  - JavaSE
  - Mybatis
  - 数据库
---

# 一、分布式系统唯一 ID 生成方案汇总（数据库 ID 号）

- **数据库自增长序列或字段**
- **UUID**
- **Redis 生成 ID**
- **Twitter 的 snowflake 算法**

博客链接： [https://www.cnblogs.com/haoxinyue/p/5208136.html](https://www.cnblogs.com/haoxinyue/p/5208136.html)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1281683/1610283728066-5da35eab-f23d-463d-bff8-3daf8e559213.png#height=207&id=q2Ukm&margin=%5Bobject%20Object%5D&name=image.png&originHeight=207&originWidth=599&originalType=binary∶=1&size=76775&status=done&style=none&width=599)

# 二、自动填充

**2、自动填充**
项目中经常会遇到一些数据，每次都使用相同的方式填充，例如记录的创建时间，更新
时间等。
我们可以使用 MyBatis Plus 的自动填充功能，完成这些字段的赋值工作：
**（1）数据库表中添加自动填充字段**
在 User 表中添加 datetime 类型的新的字段 create_time、update_time
**（2）实体上添加注解**

```java
@TableField(fill = FieldFill.INSERT_UPDATE)
private Date createTime;
@TableField(fill = FieldFill.UPDATE)
private Date updateTime;




@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    private static final Logger LOGGER = LoggerFactory.getLogger(MyMetaObjectHandler.class);
    // 使用mp实现添加操作，这个方法会执行
    @Override
    public void insertFill(MetaObject metaObject) {
        LOGGER.info("start insert fill ....");
        this.setFieldValByName("createTime", new Date(),metaObject);
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
    // 使用mp实现修改操作，这个方法会执行
    @Override
    public void updateFill(MetaObject metaObject) {
        LOGGER.info("start update fill ....");
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
}

```

# 三、乐观锁

**主要适用场景：**当要更新一条记录的时候，希望这条记录没有被别人更新，也就是说实现线程安全的数据更新。​
**如果不考虑事务隔离性的前提条件下，可能会产生的问题**

- 读问题： 脏读、 不可重复读、幻读
- 写问题： 丢失更新问题

悲观锁： 串行，只能一个人操作
乐观锁： 并行，可以同时操作![image.png](https://cdn.nlark.com/yuque/0/2021/png/1281683/1610287353447-1e5c2386-e5e6-4eb6-bf6e-0cbf4f0b3bf9.png#height=215&id=iHRH0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=255&originWidth=784&originalType=binary∶=1&size=53064&status=done&style=none&width=662)
实现：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1281683/1610625458171-65b5874e-6141-47ba-b4bc-224107538cc3.png#height=293&id=rIZml&name=image.png&originHeight=411&originWidth=1017&originalType=binary∶=1&size=163564&status=done&style=none&width=724)
配置乐观锁插件

```java
 ------------1---------------
@Version
@TableField(fill=FieldFill.INSERT)
private Integer version; // 版本号

 -----------2---------------
@Override
public void insertFill(MetaObject metaObject) {
    LOGGER.info("start insert fill ....");
    this.setFieldValByName("createTime", new Date(),metaObject);
    this.setFieldValByName("updateTime",new Date(),metaObject);
    this.setFieldValByName("version",1,metaObject);
}

 ------------3---------------
@Configuration
public class MybatisPlusConfig {

    //乐观锁插件
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }
}
-------------4--------------
// 测试乐观锁
    @Test
    public void testOptimisticLockerInterceptor(){
        // 根据id查询数据
        User user = userMapper.selectById(1349686194753024002L);
        // 进行修改
        user.setAge(200);
        userMapper.updateById(user);

    }
```

# 四、查询

## **1、根据 id 查询记录**

```java
 User user = userMapper.selectById(1L);
System.out.println(user);
```

## **2、通过多个 id 批量查询**

完成了动态 sql 的 foreach 的功能

```java
List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
users.forEach(System.out::println);
```

## **3、分页查询**

```java
/**
 * 分页插件
 */
@Bean
public PaginationInterceptor paginationInterceptor() {
    return new PaginationInterceptor();
}


// 分页查询
@Test
public void testPage(){
    Page<User> page = new Page<>(1, 3);
    userMapper.selectPage(page, null);

    // 调用mp分页查询方法
    //调用mp分页查询过程中，底层封装，把分页所有数据封装到page对象里面

    page.getRecords().forEach(System.out::println);
    System.out.println(page.getCurrent());  // 当前页数据
    System.out.println(page.getRecords());
    System.out.println(page.getPages());    // 总页数
    System.out.println(page.getSize());     //每页显示记录
    System.out.println(page.getTotal());    // 总的记录数
    System.out.println(page.hasNext());     // 是否有下一页
    System.out.println(page.hasPrevious()); //是否有上一页
}
```

# 五、删除

## **1、物理删除**

**数据真正被删除了**

```java
// 删除
@Test
public void testDelete(){
    // 物理删除
    int i = userMapper.deleteById(7);
    System.out.println(i);

    //批量删除
    int result = userMapper.deleteBatchIds(Arrays.asList(6,8));
    System.out.println(result);
}
```

## **2、逻辑删除**

**用一个标志位，值为 0 表示真正删除了，值为 1 表示只是表面上删除了，数据库中还留有该条数据**

```java
@TableLogic
private Integer deleted;    //逻辑删除

// 逻辑删除插件
@Bean
public ISqlInjector sqlInjector() {
    return new LogicSqlInjector();
}

配置文件
# 逻辑删除
mybatis-plus.global-config.db-config.logic-delete-value=1
mybatis-plus.global-config.db-config.logic-not-delete-value=0


// 逻辑删除
 @Test
public void testDelete2(){
    // 逻辑删除
    int i = userMapper.deleteById(1349695402466684929L);
    System.out.println(i);

}
```

# 六、性能监测插件

​

​

性能分析拦截器，用于输出每条 SQL 语句及其执行时间
SQL 性能执行分析,开发环境使用，超过指定时间，停止运行。有助于发现问题

## 1、配置插件

**（1）参数说明**
参数：maxTime： SQL 执行最大时长，超过自动停止运行，有助于发现问题。
参数：format： SQL 是否格式化，默认 false。
**（2）在 MybatisPlusConfig 中配置**

```java
/**
 * SQL 执行性能分析插件
 * 开发环境使用，线上不推荐。 maxTime 指的是 sql 最大执行时长
 */
@Bean
@Profile({"dev","test"})// 设置 dev test 环境开启
public PerformanceInterceptor performanceInterceptor() {
    PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
    performanceInterceptor.setMaxTime(100);//ms，超过此处设置的ms则sql不执行
    performanceInterceptor.setFormat(true);
    return performanceInterceptor;
}
```

**（3）Spring Boot 中设置 dev 环境**

```java
#环境设置：dev、test、prod
spring.profiles.active=dev
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1281683/1610628460416-626ca849-8488-405f-80b1-45967bf1c3b2.png#height=171&id=cPO3k&margin=%5Bobject%20Object%5D&name=image.png&originHeight=342&originWidth=974&originalType=binary∶=1&size=128481&status=done&style=none&width=487)

# 七、mp 实现复杂一些的条件查询

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1281683/1610628516824-217972ad-7d74-46fb-b9e9-cde972b37f88.png#height=191&id=nbdsh&margin=%5Bobject%20Object%5D&name=image.png&originHeight=382&originWidth=963&originalType=binary∶=1&size=68226&status=done&style=none&width=481.5)

Wrapper ： 条件构造抽象类，最顶端父类

- AbstractWrapper ： 用于查询条件封装，生成 sql 的 where 条件
- **QueryWrapper **： Entity 对象封装操作类，不是用 lambda 语法
- UpdateWrapper ： Update 条件封装，用于 Entity 对象更新操作
- AbstractLambdaWrapper ： Lambda 语法使用 Wrapper 统一处理解析 lambda 获取 column。
- LambdaQueryWrapper ：看名称也能明白就是用于 Lambda 语法使用的查询 Wrapper
- LambdaUpdateWrapper ： Lambda 更新封装 Wrapper

注意：以下条件构造器的方法入参中的  column  均表示数据库字段

```java
    //实现复杂的查询操作
    @Test
    public void testComplexSelect(){
        // 创建对象
        QueryWrapper<User> wrapper = new QueryWrapper<>();
/*
        // 通过QueryWrapper设置条件
        // ge、gt、le、lt、isNull、isNotNull
        // 查询年龄大于 age 》= 0 记录
        wrapper.ge("age",30);
        List<User> users1 = userMapper.selectList(wrapper);
        System.out.println(users1);

        // eq 、 ne
        wrapper.eq("name","LiBai");
        // wrapper.ne("name","LiBai");
        List<User> users2 = userMapper.selectList(wrapper);
        System.out.println(users2);

        // between
        // 年龄在20-30
        wrapper.between("age",20,30);
        List<User> users3 = userMapper.selectList(wrapper);
        System.out.println(users3);
        // like
        wrapper.like("name","东方");
        List<User> users4 = userMapper.selectList(wrapper);
        System.out.println(users4);

        // orderByDesc orderByAsc
        wrapper.orderByAsc("id");
        List<User> users5 = userMapper.selectList(wrapper);
        System.out.println(users5);

        // last
        wrapper.last("limit 1");
        List<User> users5 = userMapper.selectList(wrapper);
        System.out.println(users5);

        // 指定要查询的列
        wrapper.select("id","name");
        List<User> users6 = userMapper.selectList(wrapper);
        System.out.println(users6);
        */
    }
}
```

## ![image.png](https://cdn.nlark.com/yuque/0/2021/png/1281683/1610632302198-8a4e21a8-ebe7-478b-ab1f-dfb2b6940670.png#height=332&id=y9Tog&margin=%5Bobject%20Object%5D&name=image.png&originHeight=332&originWidth=513&originalType=binary∶=1&size=91224&status=done&style=none&width=513)
