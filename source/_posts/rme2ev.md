---
title: 20、MyBatis
urlname: rme2ev
date: '2021-07-09 20:41:33 +0800'
tags: []
categories: []
---

**狂神说 MyBatis**

环境说明：
jdk 8 +
MySQL 5.7.19
maven-3.6.0 IDEA
学习前需要掌握：
JDBC MySQL
Java 基础
Maven Junit

# 1、Mybatis 简介

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834496670-82fc0175-53a4-4234-bd61-12a8cfe5c441.jpeg#)

## 、什么是 MyBatis

MyBatis 是一款优秀的**持久层框架**
MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程
MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 实体类 【Plain Old Java Objects,普通的 Java 对象】映射成数据库中的记录。
MyBatis 本是 apache 的一个开源项目 ibatis, 2010 年这个项目由 apache 迁移到了 google code，并且改名为 MyBatis 。
2013 年 11 月迁移到**Github **.
Mybatis 官方文档 : [http://www.mybatis.org/mybatis-3/zh/index.html](http://www.mybatis.org/mybatis-3/zh/index.html)
GitHub : [https://github.com/mybatis/mybatis-3](https://github.com/mybatis/mybatis-3)

## 、持久化

#### 持久化是将程序数据在持久状态和瞬时状态间转换的机制。

即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。持久化的主要应用 是将内存中的对象存储在数据库中，或者存储在磁盘文件中、XML 数据文件中等等。
JDBC 就是一种持久化机制。文件 IO 也是一种持久化机制。
在生活中 : 将鲜肉冷藏，吃的时候再解冻的方法也是。将水果做成罐头的方法也是。

#### 为什么需要持久化服务呢？那是由于内存本身的缺陷引起的

内存断电后数据会丢失，但有一些对象是无论如何都不能丢失的，比如银行账号等，遗憾的 是，人们还无法保证内存永不掉电。
内存过于昂贵，与硬盘、光盘等外存相比，内存的价格要高 2~3 个数量级，而且维持成本也 高，至少需要一直供电吧。所以即使对象不需要永久保存，也会因为内存的容量限制不能一直 呆在内存中，需要持久化来缓存到外存。

## 、持久层

什么是持久层？
完成持久化工作的代码块 . ----> dao 层 【DAO (Data Access Object) 数据访问对象】
大多数情况下特别是企业级应用，数据持久化往往也就意味着将内存中的数据保存到磁盘上加 以固化，而持久化的实现过程则大多通过各种**关系数据库**来完成。
不过这里有一个字需要特别强调，也就是所谓的“层”。对于应用系统而言，数据持久功能大多 是必不可少的组成部分。也就是说，我们的系统中，已经天然的具备了“持久层”概念？也许 是，但也许实际情况并非如此。之所以要独立出一个“持久层”的概念,而不是“持久模块”，“持久单元”，也就意味着，我们的系统架构中，应该有一个相对独立的逻辑层面，专著于数据持久 化逻辑的实现.
与系统其他部分相对而言，这个层面应该具有一个较为清晰和严格的逻辑边界。 【说白了就是用来操作数据库存在的！】

## 、为什么需要 Mybatis

Mybatis 就是帮助程序猿将数据存入数据库中 , 和从数据库中取数据 .
传统的 jdbc 操作 , 有很多重复代码块 .比如 : 数据取出时的封装 , 数据库的建立连接等等... , 通过框架可以减少重复代码,提高开发效率 .
MyBatis 是一个半自动化的**ORM 框架 (Object Relationship Mapping) -->对象关系映射**
所有的事情，不用 Mybatis 依旧可以做到，只是用了它，所有实现会更加简单！**技术没有高低之分，只有使用这个技术的人有高低之别**
MyBatis 的优点
简单易学：本身就很小且简单。没有任何第三方依赖，最简单安装只要两个 jar 文件+配置几个
sql 映射文件就可以了，易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现。
灵活：mybatis 不会对应用程序或者数据库的现有设计强加任何影响。 sql 写在 xml 里，便于统一管理和优化。通过 sql 语句可以满足操作数据库的所有需求。
解除 sql 与程序代码的耦合：通过提供 DAO 层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql 和代码的分离，提高了可维护性。
提供 xml 标签，支持编写动态 sql。
.......
最重要的一点，使用的人多！公司需要！

# 2、MyBatis 第一个程序

#### 思路流程：搭建环境-->导入 Mybatis--->编写代码--->测试

1. **、代码演示**
1. 搭建实验数据库

| 1   | CREATE DATABASE `mybatis`;   |
| --- | ---------------------------- |
| 2   |                              |
| 3   | USE `mybatis`;               |
| 4   |                              |
| 5   | DROP TABLE IF EXISTS `user`; |
| 6   |                              |
| 7   | CREATE TABLE `user` (        |
| 8   | `id` int(20) NOT NULL,       |

| 9   | `name` varchar(30) DEFAULT NULL,                                        |
| --- | ----------------------------------------------------------------------- |
| 10  | `pwd` varchar(30) DEFAULT NULL,                                         |
| 11  | PRIMARY KEY (`id`)                                                      |
| 12  | ) ENGINE=InnoDB DEFAULT CHARSET=utf8;                                   |
| 13  |                                                                         |
| 14  | insert into `user`(`id`,`name`,`pwd`) values (1,'狂神','123456'),(2,'张 |
|     | 三','abcdef'),(3,'李四','987654');                                      |

1. 导入 MyBatis 相关 jar 包

GitHub 上找

1.  <dependency>
1.  <groupId>org.mybatis</groupId>
1.  <artifactId>mybatis</artifactId>
1.  <version>3.5.2</version>
1.  </dependency>
1.  <dependency>
1.  <groupId>mysql</groupId>
1.  <artifactId>mysql-connector-java</artifactId>
1.  <version>5.1.47</version>
1.  </dependency>

1.  编写 MyBatis 核心配置文件查看帮助文档

1.  <?xml version="1.0" encoding="UTF-8" ?>
1.  <!DOCTYPE configuration
1.  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
1.  ["http://mybatis.org/dtd/mybatis-3-config.dtd](http://mybatis.org/dtd/mybatis-3-config.dtd)">
1.  <configuration>
1.  <environments default="development">
1.  <environment id="development">
1.  <transactionManager type="JDBC"/>
1.  <dataSource type="POOLED">
1.  <property name="driver" value="com.mysql.jdbc.Driver"/>
1.      <property name="url" value="jdbc:mysql://localhost:3306/mybatis? useSSL=true&useUnicode=true&characterEncoding=utf8"/>
1.  <property name="username" value="root"/>
1.  <property name="password" value="123456"/>
1.  </dataSource>
1.  </environment>
1.  </environments>
1.  <mappers>
1.  <mapper resource="com/kuang/dao/userMapper.xml"/>
1.  </mappers>
1.  </configuration>

1.  编写 MyBatis 工具类

查看帮助文档

| 1   | import | org.apache.ibatis.io.Resources;                     |
| --- | ------ | --------------------------------------------------- |
| 2   | import | org.apache.ibatis.session.SqlSession;               |
| 3   | import | org.apache.ibatis.session.SqlSessionFactory;        |
| 4   | import | org.apache.ibatis.session.SqlSessionFactoryBuilder; |
| 5   | import | java.io.IOException;                                |

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
import java.io.InputStream;
public class MybatisUtils {

private static SqlSessionFactory sqlSessionFactory;
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
26
27
static {
try {
String resource = "mybatis-config.xml"; InputStream inputStream =
Resources.getResourceAsStream(resource);
sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
} catch (IOException e) { e.printStackTrace();
}
}

//获取 SqlSession 连接
public static SqlSession getSession(){ return sqlSessionFactory.openSession();
}
}

1. 创建实体类

| 1 | public class User {

private int id; //id private String name; private String pwd;

//构造,有参,无参
//set/get
//toString()

} |

//姓名
//密码 |
| --- | --- | --- |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |
| 6 | | |
| 7 | | |
| 8 | | |
| 9 | | |
| 10 | | |
| 11 | | |

1. 编写 Mapper 接口类

1
2
3
4
5
6
import com.kuang.pojo.User;
import java.util.List;
public interface UserMapper { List<User> selectUser();
}

1. 编写 Mapper.xml 配置文件

namespace 十分重要，不能写错！

1. <?xml version="1.0" encoding="UTF-8" ?>
1. <!DOCTYPE mapper
1. PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
1. ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)">
1. <mapper namespace="com.kuang.dao.UserMapper">
1. <select id="selectUser" resultType="com.kuang.pojo.User">
1. select \* from user
1. </select>
1. </mapper>

1. 编写测试类

Junit 包测试

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
public class MyTest { @Test
public void selectUser() {
SqlSession session = MybatisUtils.getSession();
//方法一:
//List<User> users = session.selectList("com.kuang.mapper.UserMapper.selectUser");
//方法二:
UserMapper mapper = session.getMapper(UserMapper.class); List<User> users = mapper.selectUser();
for (User user: users){ System.out.println(user);
}
session.close();
}
}

1. 运行测试
   1. **、问题说明**

#### 可能出现问题说明：Maven 静态资源过滤问题

1. <resources>
1. <resource>
1. <directory>src/main/java</directory>
1. <includes>
1. <include>\*_/_.properties</include>
1. <include>\*_/_.xml</include>
1. </includes>
1. <filtering>false</filtering>
1. </resource>
1. <resource>
1. <directory>src/main/resources</directory>
1. <includes>
1. <include>\*_/_.properties</include>
1. <include>\*_/_.xml</include>
1. </includes>
1. <filtering>false</filtering>
1. </resource>
1. </resources>

**3、CRUD 操作**

1. **、namespace**
1. 将上面案例中的 UserMapper 接口改名为 UserDao；
1. 将 UserMapper.xml 中的 namespace 改为为 UserDao 的路径 .
1. 再次测试

#### 结论：

配置文件中 namespace 中的名称为对应 Mapper 接口或者 Dao 接口的完整包名,必须一致！

## 、select

select 标签是 mybatis 中最常用的标签之一
select 语句有很多属性可以详细配置每一条 SQL 语句 id
命名空间中唯一的标识符
接口中的方法名与映射文件中的 SQL 语句 ID 一一对应
parameterType
传入 SQL 语句的参数类型 。【万能的 Map，可以多尝试使用】
resultType
SQL 语句返回值类型。【完整的类名或者别名】

#### 需求：根据 id 查询用户

1. 在 UserMapper 中添加对应方法

1. public interface UserMapper {
1. //查询全部用户
1. List<User> selectUser();
1. //根据 id 查询用户
1. User selectUserById(int id); 6 }

1. 在 UserMapper.xml 中添加 Select 语句

1. <select id="selectUserById" resultType="com.kuang.pojo.User">
1. select \* from user where id = #{id}
1. </select>

1. 测试类中测试

1. @Test
1. public void tsetSelectUserById() {
1. SqlSession session = MybatisUtils.getSession(); //获取 SqlSession 连接
1. UserMapper mapper = session.getMapper(UserMapper.class);
1. User user = mapper.selectUserById(1);
1. System.out.println(user);
1. session.close(); 8 }

**课堂练习**：根据 密码 和 名字 查询用户思路一：直接在方法中传递参数

1. 在接口方法的参数前加 @Param 属性
1. Sql 语句编写的时候，直接取@Param 中设置的值即可，不需要单独设置参数类型

1. //通过密码和名字查询用户
1. User selectUserByNP(@Param("username") String username,@Param("pwd") String pwd);

3
4
5
6
7
8
/_
<select id="selectUserByNP" resultType="com.kuang.pojo.User"> select _ from user where name = #{username} and pwd = #{pwd}
</select>
\*/

思路二：使用万能的 Map

1. 在接口方法中，参数直接传递 Map；

1 User selectUserByNP2(Map<String,Object> map);

1. 编写 sql 语句的时候，需要传递参数类型，参数类型为 map

1. <select id="selectUserByNP2" parameterType="map" resultType="com.kuang.pojo.User">
1. select \* from user where name = #{username} and pwd = #{pwd}
1. </select>

1. 在使用方法的时候，Map 的 key 为 sql 中取的值即可，没有顺序要求！

| 1   | Map<String, Object> map = new HashMap<String, Object>(); |
| --- | -------------------------------------------------------- |
| 2   | map.put("username","小明");                              |
| 3   | map.put("pwd","123456");                                 |
| 4   | User user = mapper.selectUserByNP2(map);                 |

总结：
如果参数过多，我们可以考虑直接使用 Map 实现，如果参数比较少，直接传递参数即可

## 、insert

我们一般使用 insert 标签进行插入操作，它的配置和 select 标签差不多！

#### 需求：给数据库增加一个用户

1. 在 UserMapper 接口中添加对应的方法

1. //添加一个用户
1. int addUser(User user);

1. 在 UserMapper.xml 中添加 insert 语句

1. <insert id="addUser" parameterType="com.kuang.pojo.User">
1. insert into user (id,name,pwd) values (#{id},#{name},#{pwd})
1. </insert>

1. 测试

1. @Test
1. public void testAddUser() {
1. SqlSession session = MybatisUtils.getSession();
1. UserMapper mapper = session.getMapper(UserMapper.class);
1. User user = new User(5,"王五","zxcvbn");
1. int i = mapper.addUser(user);
1. System.out.println(i);
1. session.commit(); //提交事务,重点!不写的话不会提交到数据库
1. session.close(); 10 }

#### 注意点：增、删、改操作需要提交事务！

1.  **、update**

我们一般使用 update 标签进行更新操作，它的配置和 select 标签差不多！

#### 需求：修改用户的信息

1. 同理，编写接口方法

1. //修改一个用户
1. int updateUser(User user);

1. 编写对应的配置文件 SQL

1. <update id="updateUser" parameterType="com.kuang.pojo.User">
1. update user set name=#{name},pwd=#{pwd} where id = #{id}
1. </update>

1. 测试

1. @Test
1. public void testUpdateUser() {
1. SqlSession session = MybatisUtils.getSession();
1. UserMapper mapper = session.getMapper(UserMapper.class);
1. User user = mapper.selectUserById(1);
1. user.setPwd("asdfgh");
1. int i = mapper.updateUser(user);
1. System.out.println(i);
1. session.commit(); //提交事务,重点!不写的话不会提交到数据库
1. session.close(); 11 }

## 、delete

我们一般使用 delete 标签进行删除操作，它的配置和 select 标签差不多！

#### 需求：根据 id 删除一个用户

1. 同理，编写接口方法

1. //根据 id 删除用户
1. int deleteUser(int id);

1. 编写对应的配置文件 SQL

1. <delete id="deleteUser" parameterType="int">
1. delete from user where id = #{id}
1. </delete>

1. 测试

1. @Test
1. public void testDeleteUser() {
1. SqlSession session = MybatisUtils.getSession();
1. UserMapper mapper = session.getMapper(UserMapper.class);
1. int i = mapper.deleteUser(5);
1. System.out.println(i);
1. session.commit(); //提交事务,重点!不写的话不会提交到数据库
1. session.close(); 9 }

#### 小结：

所有的增删改操作都需要提交事务！
接口所有的普通参数，尽量都写上@Param 参数，尤其是多个参数时，必须写上！ 有时候根据业务的需求，可以考虑使用 map 传递参数！
为了规范操作，在 SQL 的配置文件中，我们尽量将 Parameter 参数和 resultType 都写上！

## 、思考题

#### 模糊查询 like 语句该怎么写?

第 1 种：在 Java 代码中添加 sql 通配符。

1
2
3
4
5
6
string wildcardname = “%smi%”;
list<name> names = mapper.selectlike(wildcardname);
<select id=”selectlike”>
select \* from foo where bar like #{value}
</select>

第 2 种：在 sql 语句中拼接通配符，会引起 sql 注入

1
2
3
4
5
6
7
string wildcardname = “smi”;
list<name> names = mapper.selectlike(wildcardname);
<select id=”selectlike”>
select \* from foo where bar like "%"#{value}"%"
</select>

# 4、配置解析

## 、核心配置文件

mybatis-conﬁg.xml 系统核心配置文件
MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。能配置的内容如下：

1. configuration（配置）
1. properties（属性）
1. settings（设置）
1. typeAliases（类型别名）
1. typeHandlers（类型处理器）
1. objectFactory（对象工厂）
1. plugins（插件）
1. environments（环境配置）
1. environment（环境变量）
1. transactionManager（事务管理器）
1. dataSource（数据源）
1. databaseIdProvider（数据库厂商标识）
1. mappers（映射器）
1. <!-- 注意元素节点的顺序！顺序不对会报错 -->

我们可以阅读 mybatis-conﬁg.xml 上面的 dtd 的头文件！【演示】

## 、environments 元素

1. <environments default="development">
1. <environment id="development">
1. <transactionManager type="JDBC">
1. <property name="..." value="..."/>
1. </transactionManager>
1. <dataSource type="POOLED">
1. <property name="driver" value="${driver}"/>
1. <property name="url" value="${url}"/>
1. <property name="username" value="${username}"/>
1. <property name="password" value="${password}"/>
1. </dataSource>
1. </environment>
1. </environments>

配置 MyBatis 的多套运行环境，将 SQL 映射到多个不同的数据库上，必须指定其中一个为默认运行环境（通过 default 指定）
子元素节点：**environment**
具体的一套环境，通过设置 id 进行区别，id 保证唯一！ 子元素节点：transactionManager - [ 事务管理器 ]

1 <!-- 语法 -->
2 <transactionManager type="[ JDBC | MANAGED ]"/>

详情：[点击查看官方文档](http://www.mybatis.org/mybatis-3/zh/configuration.html#environments)
这两种事务管理器类型都不需要设置任何属性。子元素节点：**数据源（dataSource）**
dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。

数据源是必须配置的。有三种内建的数据源类型

1 type="[UNPOOLED|POOLED|JNDI]"）

unpooled： 这个数据源的实现只是每次被请求时打开和关闭连接。
**pooled**： 这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来 , 这是一种使得并发 Web 应用快速响应请求的流行处理方式。
jndi：这个数据源的实现是为了能在如 Spring 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。
数据源也有很多第三方的实现，比如 dbcp，c3p0，druid 等等....

## 、mappers 元素

### 、mappers

映射器 : 定义映射 SQL 语句文件
既然 MyBatis 的行为其他元素已经配置完了，我们现在就要定义 SQL 映射语句了。但是首先我们需要告诉 MyBatis 到哪里去找到这些语句。 Java 在自动查找这方面没有提供一个很好的方法，所以最佳的方式是告诉 MyBatis 到哪里去找映射文件。你可以使用相对于类路径的资源引用， 或完
全限定资源定位符（包括 的 URL），或类名和包名等。映射器是 MyBatis 中最核心
file:///
的组件之一，在 MyBatis 3 之前，只支持 xml 映射器，即：所有的 SQL 语句都必须在 xml 文件中配
置。而从 MyBatis 3 开始，还支持接口映射器，这种映射器方式允许以 Java 代码的方式注解定义 SQL
语句，非常简洁。

### 、引入资源方式

1. <!-- 使用相对于类路径的资源引用 -->
1. <mappers>
1. <mapper resource="org/mybatis/builder/PostMapper.xml"/>
1. </mappers>
1. <!-- 使用完全限定资源定位符（URL） -->
1. <mappers>
1. <mapper url="file:///var/mappers/AuthorMapper.xml"/>
1. </mappers>

1 <!--

1. 使用映射器接口实现类的完全限定类名
1. 需要配置文件名称和接口名称一致，并且位于同一目录下

4 -->

1. <mappers>
1. <mapper class="org.mybatis.builder.AuthorMapper"/>
1. </mappers>

1 <!--

1. 将包内的映射器接口实现全部注册为映射器
1. 但是需要配置文件名称和接口名称一致，并且位于同一目录下

4 -->

1. <mappers>
1. <package name="org.mybatis.builder"/>
1. </mappers>

   1. **、Mapper 文件**

1
2
3
4
5
6
7

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)">
<mapper namespace="com.kuang.mapper.UserMapper">
</mapper>

namespace 中文意思：命名空间，作用如下：

         1. namespace和子元素的id联合保证唯一 , 区别不同的mapper
         1. 绑定DAO接口

namespace 的命名必须跟某个接口同名
接口中的方法与映射文件中 sql 语句 id 应该一一对应

         1. namespace命名规则 : 包名+类名

MyBatis 的真正强大在于它的映射语句，这是它的魔力所在。由于它的异常强大，映射器的 XML 文件就显得相对简单。如果拿它跟具有相同功能的 JDBC 代码进行对比，你会立即发现省掉了将近 95% 的代码。MyBatis 为聚焦于 SQL 而构建，以尽可能地为你减少麻烦。

## 、Properties 优化

数据库这些属性都是可外部配置且可动态替换的，既可以在典型的 Java 属性文件中配置，亦可通过
properties 元素的子元素来传递。[具体的官方文档](http://www.mybatis.org/mybatis-3/zh/configuration.html#properties)
我们来优化我们的配置文件
第一步 ; 在资源目录下新建一个 db.properties

| 1   | driver=com.mysql.jdbc.Driver                       |
| --- | -------------------------------------------------- |
| 2   | url=jdbc:mysql://localhost:3306/mybatis?           |
|     | useSSL=true&useUnicode=true&characterEncoding=utf8 |
| 3   | username=root                                      |
| 4   | password=123456                                    |

第二步 : 将文件导入 properties 配置文件

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
<configuration>

<!--导入properties文件-->
<properties resource="db.properties"/>
<environments default="development">
<environment id="development">
<transactionManager type="JDBC"/>
<dataSource type="POOLED">
<property name="driver" value="${driver}"/>
<property name="url" value="${url}"/>
<property name="username" value="${username}"/>
<property name="password" value="${password}"/>
</dataSource>
</environment>
</environments>
<mappers>
<mapper resource="mapper/UserMapper.xml"/>
</mappers>
</configuration>

更多操作，可以查看官方文档！【演示带领学习】
配置文件优先级问题新特性：使用占位符

## 、typeAliases 优化

类型别名是为 Java 类型设置一个短的名字。它只和 XML 配置有关，存在的意义仅在于用来减少类完全限定名的冗余。

1. <!--配置别名,注意顺序-->
1. <typeAliases>
1. <typeAlias type="com.kuang.pojo.User" alias="User"/>
1. </typeAliases>

当这样配置时，
User
可以用在任何使用
的地方。

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如:
com.kuang.pojo.User

1. <typeAliases>
1. <package name="com.kuang.pojo"/>
1. </typeAliases>

每一个在包
com.kuang.pojo
非限定类名来作为它的别名。
中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的

若有注解，则别名为其注解值。见下面的例子：

1. @Alias("user")
1. public class User { 3 ...

4 }

【演示】去官网查看一下 Mybatis 默认的一些类型别名！

## 、其他配置浏览

### 、设置

[**设置（settings）**](http://www.mybatis.org/mybatis-3/zh/configuration.html#settings)相关 => 查看帮助文档懒加载
日志实现
缓存开启关闭
一个配置完整的 settings 元素的示例如下：

| 1 | <settings>
<setting
<setting
<setting
<setting
<setting
<setting
<setting
<setting
<setting
<setting
<setting |
name="cacheEnabled" value="true"/> name="lazyLoadingEnabled" value="true"/> name="multipleResultSetsEnabled" value="true"/> name="useColumnLabel" value="true"/> name="useGeneratedKeys" value="false"/> name="autoMappingBehavior" value="PARTIAL"/> name="autoMappingUnknownColumnBehavior" value="WARNING"/> name="defaultExecutorType" value="SIMPLE"/> name="defaultStatementTimeout" value="25"/> name="defaultFetchSize" value="100"/> name="safeRowBoundsEnabled" value="false"/> |
| --- | --- | --- |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |
| 6 | | |
| 7 | | |
| 8 | | |
| 9 | | |
| 10 | | |
| 11 | | |
| 12 | | |

1.  <setting name="mapUnderscoreToCamelCase" value="false"/>
1.  <setting name="localCacheScope" value="SESSION"/>
1.  <setting name="jdbcTypeForNull" value="OTHER"/>
1.      <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
1.  </settings>

### 、类型处理器

[官方文档](http://www.mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。
你可以重写类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。【了解即可】

### 、对象工厂

[官方文档](http://www.mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
MyBatis 每次创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成。
默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认构造方法，要么在参数映射存在的时 候通过有参构造方法来实例化。
如果想覆盖对象工厂的默认行为，则可以通过创建自己的对象工厂来实现。【了解即可】

## 生命周期和作用域

#### 作用域（Scope）和生命周期

理解我们目前已经讨论过的不同作用域和生命周期类是至关重要的，因为错误的使用会导致非常严重的 并发问题。
我们可以先画一个流程图，分析一下 Mybatis 的执行过程！
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834497204-138d6d07-c9e8-4fdd-a556-9e8ba611c736.jpeg#)

#### 作用域理解

SqlSessionFactoryBuilder 的作用在于创建 SqlSessionFactory，创建成功后，
SqlSessionFactoryBuilder 就失去了作用，所以它只能存在于创建 SqlSessionFactory 的方法中， 而不要让其长期存在。因此 **SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域**（也就是局部方法变量）。
SqlSessionFactory 可以被认为是一个数据库连接池，它的作用是创建 SqlSession 接口对象。因为
MyBatis 的本质就是 Java 对数据库的操作，所以 SqlSessionFactory 的生命周期存在于整个
MyBatis 的应用之中，所以一旦创建了 SqlSessionFactory，就要长期保存它，直至不再使用 MyBatis 应用，所以可以认为 SqlSessionFactory 的生命周期就等同于 MyBatis 的应用周期。
由于 SqlSessionFactory 是一个对数据库的连接池，所以它占据着数据库的连接资源。如果创建多个 SqlSessionFactory，那么就存在多个数据库连接池，这样不利于对数据库资源的控制，也会导致数据库连接资源被消耗光，出现系统宕机等情况，所以尽量避免发生这样的情况。
因此在一般的应用中我们往往希望 SqlSessionFactory 作为一个单例，让它在应用中被共享。所以说 **SqlSessionFactory 的最佳作用域是应用作用域。**
如果说 SqlSessionFactory 相当于数据库连接池，那么 SqlSession 就相当于一个数据库连接
（Connection 对象），你可以在一个事务里面执行多条 SQL，然后通过它的 commit、rollback 等方法，提交或者回滚事务。所以它应该存活在一个业务请求中，处理完整个请求后，应该关闭这 条连接，让它归还给 SqlSessionFactory，否则数据库资源就很快被耗费精光，系统就会瘫痪，所以用 try...catch...ﬁnally... 语句来保证其正确关闭。

#### 所以 SqlSession 的最佳的作用域是请求或方法作用域。

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834497659-6f9ff940-f044-43f6-b155-9ce8b5a05adf.jpeg#)

**5、ResultMap**

**要解决的问题：属性名和字段名不一致**
环境：新建一个项目，将之前的项目拷贝过来

## 、查询为 null 问题

1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834498130-d1496748-a6bb-4f48-8202-90192948cdc4.png#)查看之前的数据库的字段名
1. Java 中的实体类设计

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
public class User {
private int id; //id private String name; //姓名
private String password;
//密码和数据库不一样！
//构造
//set/get
//toString()
}

1. 接口

1. //根据 id 查询用户
1. User selectUserById(int id);

1. mapper 映射文件

1. <select id="selectUserById" resultType="user">
1. select \* from user where id = #{id}
1. </select>

1. 测试

1. @Test
1. public void testSelectUserById() {
1. SqlSession session = MybatisUtils.getSession(); //获取 SqlSession 连接
1. UserMapper mapper = session.getMapper(UserMapper.class);
1. User user = mapper.selectUserById(1);
1. System.out.println(user);
1. session.close(); 8 }

#### 结果:

User{id=1, name='狂神', password='null'}
查询出来发现 password 为空 . 说明出现了问题！

#### 分析：

select \* from user where id = #{id} 可以看做
select id,name,pwd from user where id = #{id}

mybatis 会根据这些查询的列名(会将列名转化为小写,数据库不区分大小写) , 去对应的实体类中查找相应列名的 set 方法设值 , 由于找不到 setPwd() , 所以 password 返回 null ; 【自动映射】

## 、解决方案

方案一：为列名指定别名 , 别名和 java 实体类的属性名一致 .

1. <select id="selectUserById" resultType="User">
1. select id , name , pwd as password from user where id = #{id}
1. </select>

#### 方案二：使用结果集映射->ResultMap 【推荐】

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
<resultMap id="UserMap" type="User">

<!-- id为主键 -->
<id column="id" property="id"/>
<!-- column是数据库表的列名 , property是对应实体类的属性名 -->
<result column="name" property="name"/>
<result column="pwd" property="password"/>
</resultMap>
<select id="selectUserById" resultMap="UserMap"> select id , name , pwd from user where id = #{id}
</select>

1.  **、ResultMap**
    1. **、自动映射**

resultMap 元素是 MyBatis 中最重要最强大的元素。它可以让你从 90% 的 JDBC
ResultSets 数据提取代码中解放出来。
resultMap
实际上，在为一些比如连接的复杂语句编写映射代码的时候，一份等功能的长达数千行的代码。
能够代替实现同

ResultMap 的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语
句只需要描述它们的关系就行了。
你已经见过简单映射语句的示例了，但并没有显式指定 。比如：
resultMap

1. <select id="selectUserById" resultType="map">
1. select id , name , pwd
1. from user
1. where id = #{id}
1. </select>

上述语句只是简单地将所有的列映射到
的键上，这由
属性指定。虽然在

大部分情况下都够用，但是 HashMap 不是一个很好的模型。你的程序更可能会使用 JavaBean 或
HashMap
resultType
POJO（Plain Old Java Objects，普通老式 Java 对象）作为模型。
最优秀的地方在于，虽然你已经对它相当了解了，但是根本就不需要显式地用到他们。
ResultMap

      1. **、手动映射**
         1. 返回值类型为resultMap

1.  <select id="selectUserById" resultMap="UserMap">
1.  select id , name , pwd from user where id = #{id}
1.  </select>

         1. 编写resultMap，实现手动映射！

1 <resultMap id="UserMap" type="User"> 2 <!-- id为主键 -->

1. <id column="id" property="id"/>
1. <!-- column是数据库表的列名 , property是对应实体类的属性名 -->
1. <result column="name" property="name"/>
1. <result column="pwd" property="password"/>
1. </resultMap>

如果世界总是这么简单就好了。但是肯定不是的，数据库中，存在一对多，多对一的情况，我们之后会 使用到一些高级的结果集映射，association，collection 这些，我们将在之后讲解，今天你们需要把这 些知识都消化掉才是最重要的！理解结果集映射的这个概念！

# 6、分页的实现

## 、日志工厂

思考：我们在测试 SQL 的时候，要是能够在控制台输出 SQL 的话，是不是就能够有更快的排错效率？ 如果一个 数据库相关的操作出现了问题，我们可以根据输出的 SQL 语句快速排查问题。
对于以往的开发过程，我们会经常使用到 debug 模式来调节，跟踪我们的代码执行过程。但是现在使用
Mybatis 是基于接口，配置文件的源代码执行过程。因此，我们必须选择日志工具来作为我们开发，调节程序的工具。
Mybatis 内置的日志工厂提供日志功能，具体的日志实现有以下几种工具： SLF4J
Apache Commons Logging Log4j 2
Log4j
JDK logging
具体选择哪个日志实现工具由 MyBatis 的内置日志工厂确定。它会使用最先找到的（按上文列举的顺序查找）。 如果一个都未找到，日志功能就会被禁用。

#### 标准日志实现

指定 MyBatis 应该使用哪个日志记录实现。如果此设置不存在，则会自动发现日志记录实现。

1. <settings>
1. <setting name="logImpl" value="STDOUT_LOGGING"/>
1. </settings>

测试，可以看到控制台有大量的输出！我们可以通过这些输出来判断程序到底哪里出了 Bug

## 、Log4j

#### 简介：

Log4j 是 Apache 的一个开源项目
通过使用 Log4j，我们可以控制日志信息输送的目的地：控制台，文本，GUI 组件....
我们也可以控制每一条日志的输出格式；

通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。最令人感兴趣的就 是，这些可以通过一个配置文件来灵活地进行配置，而不需要修改应用的代码。

#### 使用步骤：

1. 导入 log4j 的包

1. <dependency>
1. <groupId>log4j</groupId>
1. <artifactId>log4j</artifactId>
1. <version>1.2.17</version>
1. </dependency>

1. 配置文件编写

| 1   | #将等级为 DEBUG 的日志信息输出到 console 和 file 这两个目的地，console 和 file 的定义在下 |
| --- | ----------------------------------------------------------------------------------------- |
|     | 面的代码                                                                                  |
| 2   | log4j.rootLogger=DEBUG,console,file                                                       |
| 3   |                                                                                           |
| 4   | #控制台输出的相关设置                                                                     |
| 5   | log4j.appender.console = org.apache.log4j.ConsoleAppender                                 |
| 6   | log4j.appender.console.Target = System.out                                                |
| 7   | log4j.appender.console.Threshold=DEBUG                                                    |
| 8   | log4j.appender.console.layout = org.apache.log4j.PatternLayout                            |
| 9   | log4j.appender.console.layout.ConversionPattern=[%c]-%m%n                                 |
| 10  |                                                                                           |
| 11  | #文件输出的相关设置                                                                       |
| 12  | log4j.appender.file = org.apache.log4j.RollingFileAppender                                |
| 13  | log4j.appender.file.File=./log/kuang.log                                                  |
| 14  | log4j.appender.file.MaxFileSize=10mb                                                      |
| 15  | log4j.appender.file.Threshold=DEBUG                                                       |
| 16  | log4j.appender.file.layout=org.apache.log4j.PatternLayout                                 |
| 17  | log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-mm-dd}][%c]%m%n                   |
| 18  |                                                                                           |
| 19  | #日志输出级别                                                                             |
| 20  | log4j.logger.org.mybatis=DEBUG                                                            |
| 21  | log4j.logger.java.sql=DEBUG                                                               |
| 22  | log4j.logger.java.sql.Statement=DEBUG                                                     |
| 23  | log4j.logger.java.sql.ResultSet=DEBUG                                                     |
| 24  | log4j.logger.java.sql.PreparedStatement=DEBUG                                             |

1. setting 设置日志实现

1. <settings>
1. <setting name="logImpl" value="LOG4J"/>
1. </settings>

1. 在程序中使用 Log4j 进行输出！

1
2
3
4
5
6
7
8
9
//注意导包：org.apache.log4j.Logger
static Logger logger = Logger.getLogger(MyTest.class);
@Test
public void selectUser() {
logger.info("info：进入 selectUser 方法");
logger.debug("debug：进入 selectUser 方法");
logger.error("error: 进 入 selectUser 方 法 "); SqlSession session = MybatisUtils.getSession();

| 10 |

} | UserMapper mapper = session.getMapper(UserMapper.class); List<User> users = mapper.selectUser();
for (User user: users){ System.out.println(user);
}
session.close(); |
| --- | --- | --- |
| 11 | | |
| 12 | | |
| 13 | | |
| 14 | | |
| 15 | | |
| 16 | | |

1. 测试，看控制台输出！

使用 Log4j 输出日志
可以看到还生成了一个日志的文件 【需要修改 ﬁle 的日志级别】

## 、limit 实现分页

#### 思考：为什么需要分页？

在学习 mybatis 等持久层框架的时候，会经常对数据进行增删改查操作，使用最多的是对数据库进行查询操作，如果查询大量数据的时候，我们往往使用分页进行查询，也就是每次处理小部分数据，这样对 数据库压力就在可控范围内。

#### 使用 Limit 实现分页

| 1   | #语法                                                                        |
| --- | ---------------------------------------------------------------------------- |
| 2   | SELECT \* FROM table LIMIT stratIndex，pageSize                              |
| 3   |                                                                              |
| 4   | SELECT \* FROM table LIMIT 5,10; // 检索记录行 6-15                          |
| 5   |                                                                              |
| 6   | #为了检索从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为 -1： |
| 7   | SELECT \* FROM table LIMIT 95,-1; // 检索记录行 96-last.                     |
| 8   |                                                                              |
| 9   | #如果只给定一个参数，它表示返回最大的记录行数目：                            |
| 10  | SELECT \* FROM table LIMIT 5; //检索前 5 个记录行                            |
| 11  |                                                                              |
| 12  | #换句话说，LIMIT n 等价于 LIMIT 0,n。                                        |

**步骤：**

1. 修改 Mapper 文件

1. <select id="selectUser" parameterType="map" resultType="user">
1. select \* from user limit #{startIndex},#{pageSize}
1. </select>

1. Mapper 接口，参数为 map

1. //选择全部用户实现分页
1. List<User> selectUser(Map<String,Integer> map);

1. 在测试类中传入参数测试

推断：起始位置 = （当前页面 - 1 ） \* 页面大小

1. //分页查询 , 两个参数 startIndex , pageSize
1. @Test
1. public void testSelectUser() {
1. SqlSession session = MybatisUtils.getSession();

| 5 |

} | UserMapper mapper = session.getMapper(UserMapper.class);

int currentPage = 1; //第几页 int pageSize = 2; //每页显示几个
Map<String,Integer> map = new HashMap<String,Integer>(); map.put("startIndex",(currentPage-1)\*pageSize); map.put("pageSize",pageSize);

List<User> users = mapper.selectUser(map);

for (User user: users){ System.out.println(user);
}

| session.close(); |
| ---------------- | --- | --- |
| 6                |     |     |
| 7                |     |     |
| 8                |     |     |
| 9                |     |     |
| 10               |     |     |
| 11               |     |     |
| 12               |     |     |
| 13               |     |     |
| 14               |     |     |
| 15               |     |     |
| 16               |     |     |
| 17               |     |     |
| 18               |     |     |
| 19               |     |     |
| 20               |     |     |

## 、RowBounds 分页

我们除了使用 Limit 在 SQL 层面实现分页，也可以使用 RowBounds 在 Java 代码层面实现分页，当然此种方 式作为了解即可。我们来看下如何实现的！

#### 步骤：

1. mapper 接口

1. //选择全部用户 RowBounds 实现分页
1. List<User> getUserByRowBounds();

1. mapper 文件

1. <select id="getUserByRowBounds" resultType="user">
1. select \* from user
1. </select>

1. 测试类

在这里，我们需要使用 RowBounds 类

1
2
3
4
5
6
7
@Test
public void testUserByRowBounds() {
SqlSession session = MybatisUtils.getSession();
int currentPage = 2; //第几页
int pageSize = 2; //每页显示几个
RowBounds rowBounds = new RowBounds((currentPage- 1)\*pageSize,pageSize);
8
9
10
//通过 session.\*\*方法进行传递 rowBounds，[此种方式现在已经不推荐使用了]
List<User> users = session.selectList("com.kuang.mapper.UserMapper.getUserByRowBounds", null, rowBounds);
11

1. for (User user: users){
1. System.out.println(user); 14 }

| 15 |
} | session.close(); |
| --- | --- | --- |
| 16 | | |

## 、PageHelper

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834498690-98611c74-ba30-47ed-8756-b61fc0a24368.jpeg#)

了解即可，可以自己尝试使用
官方文档：[https://pagehelper.github.io/](https://pagehelper.github.io/)

# 7、使用注解开发

## 、面向接口编程

大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口 编程

#### 根本原因 : 解耦 , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准

**, 使得开发变得容易 , 规范性更好**
在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下， 各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了；
而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交 互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按 照这种思想来编程。

#### 关于接口的理解

接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。 接口的本身反映了系统设计人员对系统的抽象理解。
接口应有两类：
第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)； 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；
一个体有可能有多个抽象面。抽象体与抽象面是有区别的。

#### 三个面向区别

面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .
面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .
接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题.更多的体现就是 对系统整体的架构

## 、利用注解开发

#### mybatis 最初配置信息是基于 XML ,映射语句(SQL)也是定义在 XML 中的。而到 MyBatis 3 提供了新的基于注解的配置。不幸的是，Java 注解的的表达力和灵活性十分有限。最强大的 MyBatis 映射并不能用注解来构建

sql 类型主要分成 : @select () @update ()
@Insert ()
@delete ()

【注意】利用注解开发就不需要 mapper.xml 映射文件了 .

1. 我们在我们的接口中添加注解

1. //查询全部用户
1. @Select("select id,name,pwd password from user")
1. public List<User> getAllUser();

1. 在 mybatis 的核心配置文件中注入

1. <!--使用class绑定接口-->
1. <mappers>
1. <mapper class="com.kuang.mapper.UserMapper"/>
1. </mappers>

1. 我们去进行测试

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
@Test
public void testGetAllUser() {
SqlSession session = MybatisUtils.getSession();
//本质上利用了 jvm 的动态代理机制
UserMapper mapper = session.getMapper(UserMapper.class);
●
List<User> users = mapper.getAllUser(); for (User user : users){
System.out.println(user);
}
session.close();
}

1. 利用 Debug 查看本质

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834499258-2f2eb541-c72d-46fd-8b12-4bb0b45819fa.png#)

1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834499767-5b9ef9bb-9721-4fd2-b33a-42028565b2bd.png#)本质上利用了 jvm 的动态代理机制
1. Mybatis 详细的执行流程

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834500315-5b49928e-a865-4a7d-b442-6cdbfbfa8991.jpeg#)

## 、注解增删改

改造 MybatisUtils 工具类的 getSession( ) 方法，重载实现。【鸡汤：多看源码实现】

1
2
3
4
5
6
7
8
//获取 SqlSession 连接
public static SqlSession getSession(){ return getSession(true); //事务自动提交
}
public static SqlSession getSession(boolean flag){ return sqlSessionFactory.openSession(flag);
}

【注意】确保实体类和数据库字段对应

#### 查询：

1. 编写接口方法注解

1. //根据 id 查询用户
1. @Select("select \* from user where id = #{id}")
1. User selectUserById(@Param("id") int id);

1. 测试

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
@Test
public void testSelectUserById() {
SqlSession session = MybatisUtils.getSession(); UserMapper mapper = session.getMapper(UserMapper.class);
User user = mapper.selectUserById(1); System.out.println(user);

session.close();
}

#### 新增：

1. 编写接口方法注解

1. //添加一个用户
1. @Insert("insert into user (id,name,pwd) values (#{id},#{name},#{pwd})")
1. int addUser(User user);

1. 测试

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
@Test
public void testAddUser() {
SqlSession session = MybatisUtils.getSession(); UserMapper mapper = session.getMapper(UserMapper.class);
User user = new User(6, "秦疆", "123456");
mapper.addUser(user);

session.close();
}

#### 修改：

1. 编写接口方法注解

1. //修改一个用户
1. @Update("update user set name=#{name},pwd=#{pwd} where id = #{id}")
1. int updateUser(User user);

1. 测试

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
@Test
public void testUpdateUser() {
SqlSession session = MybatisUtils.getSession(); UserMapper mapper = session.getMapper(UserMapper.class);
User user = new User(6, "秦疆", "zxcvbn");
mapper.updateUser(user);

session.close();
}

#### 删除：

1. 编写接口方法注解

1. //根据 id 删除用
1. @Delete("delete from user where id = #{id}")
1. int deleteUser(@Param("id")int id);

1. 测试

1
2
3
4
5
6
7
8
9
@Test
public void testDeleteUser() {
SqlSession session = MybatisUtils.getSession(); UserMapper mapper = session.getMapper(UserMapper.class);
mapper.deleteUser(6);

session.close();
}

【注意点：增删改一定记得对事务的处理】

## 、关于@Param

@Param 注解用于给方法参数起一个名字。以下是总结的使用原则：
在方法只接受一个参数的情况下，可以不使用@Param。
在方法接受多个参数的情况下，建议一定要使用@Param 注解给参数命名。如果参数是 JavaBean ， 则不能使用@Param。
不使用@Param 注解时，参数只能有一个，并且是 Javabean。

## 、#与$的区别

#{} 的作用主要是替换预编译语句(PrepareStatement)中的占位符? 【推荐使用】

| 1   | INSERT | INTO | user | (name) | VALUES | (#{name}); |
| --- | ------ | ---- | ---- | ------ | ------ | ---------- |
| 2   | INSERT | INTO | user | (name) | VALUES | (?);       |

${} 的作用是直接进行字符串替换

| 1   | INSERT | INTO | user | (name) | VALUES | ('${name}');   |
| --- | ------ | ---- | ---- | ------ | ------ | -------------- |
| 2   | INSERT | INTO | user | (name) | VALUES | ('kuangshen'); |

# 8、多对一的处理

多对一的理解：
多个学生对应一个老师
如果对于学生这边，就是一个多对一的现象，即从学生这边关联一个老师！

## 、数据库设计

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834500771-9b3c0f47-a021-4730-8f3a-17233c57304b.png#)

| 1   | CREATE TABLE `teacher` (                                               |
| --- | ---------------------------------------------------------------------- |
| 2   | `id` INT(10) NOT NULL,                                                 |
| 3   | `name` VARCHAR(30) DEFAULT NULL,                                       |
| 4   | PRIMARY KEY (`id`)                                                     |
| 5   | ) ENGINE=INNODB DEFAULT CHARSET=utf8                                   |
| 6   |                                                                        |
| 7   | INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');                |
| 8   |                                                                        |
| 9   | CREATE TABLE `student` (                                               |
| 10  | `id` INT(10) NOT NULL,                                                 |
| 11  | `name` VARCHAR(30) DEFAULT NULL,                                       |
| 12  | `tid` INT(10) DEFAULT NULL,                                            |
| 13  | PRIMARY KEY (`id`),                                                    |
| 14  | KEY `fktid` (`tid`),                                                   |
| 15  | CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)     |
| 16  | ) ENGINE=INNODB DEFAULT CHARSET=utf8                                   |
| 17  |                                                                        |
| 18  |                                                                        |
| 19  | INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1'); |
| 20  | INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1'); |
| 21  | INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1'); |
| 22  | INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1'); |
| 23  | INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1'); |

1.  **、搭建测试环境**

【Lombok 的使用】

1. IDEA 安装 Lombok 插件
1. 引入 Maven 依赖

1. <!-- [https://mvnrepository.com/artifact/org.projectlombok/lombok](https://mvnrepository.com/artifact/org.projectlombok/lombok) -->
1. <dependency>
1. <groupId>org.projectlombok</groupId>
1. <artifactId>lombok</artifactId>
1. <version>1.16.10</version>
1. </dependency>

1. 在代码中增加注解

1. @Data //GET,SET,ToString，有参，无参构造
1. public class Teacher {
1. private int id;
1. private String name; 5 }
1. @Data
1. public class Student {
1. private int id;
1. private String name;
1. //多个学生可以是同一个老师，即多对一
1. private Teacher teacher; 7 }

8

1. 编写实体类对应的 Mapper 接口 【两个】

#### 无论有没有需求，都应该写上，以备后来之需！

1 public interface StudentMapper { 2 }
1 public interface TeacherMapper { 2 }

1. 编写 Mapper 接口对应的 mapper.xml 配置文件 【两个】

#### 无论有没有需求，都应该写上，以备后来之需！

1
2
3
4
5
6
7

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)">
<mapper namespace="com.kuang.mapper.StudentMapper">
</mapper>
1
2
3
4
5
6
7
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)">
<mapper namespace="com.kuang.mapper.TeacherMapper">
</mapper>

1. **、按查询嵌套处理**
1. 给 StudentMapper 接口增加方法

1. //获取所有学生及对应老师的信息
1. public List<Student> getStudents();

1. 编写对应的 Mapper 文件

   1. <?xml version="1.0" encoding="UTF-8" ?>
   1. <!DOCTYPE mapper
   1. PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
   1. ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)">
   1. <mapper namespace="com.kuang.mapper.StudentMapper"> 6

7 <!--

1.  需求：获取所有学生及对应老师的信息
1.  思路：
1.  1. 获取所有学生的信息
1.  2. 根据获取的学生信息的老师 ID->获取该老师的信息
1.      3. 思考问题，这样学生的结果集中应该包含老师，该如何处理呢，数据库中我们一般使用关联查询？
1.  1. 做一个结果集映射：StudentTeacher
1.  2. StudentTeacher 结果集的类型为 Student
1.  3. 学生中老师的属性为 teacher，对应数据库中为 tid。
1.  多个 [1,...）学生关联一个老师=> 一对一，一对多
1.      4. 查看官网找到：association – 一个复杂类型的关联；使用它来处理关联查询

18 -->

1.  <select id="getStudents" resultMap="StudentTeacher">
1.  select \* from student
1.  </select>
1.  <resultMap id="StudentTeacher" type="Student">
1.      <!--association关联属性	property属性名 javaType属性类型 column在多的一方的表中的列名-->
1.      <association property="teacher"	column="tid" javaType="Teacher" select="getTeacher"/>
1.  </resultMap>

26 <!--

1.  这里传递过来的 id，只有一个属性的时候，下面可以写任何值
1.  association 中 column 多参数配置：
1.  column="{key=value,key=value}"
1.      其实就是键值对的形式，key是传给下个sql的取值名称，value是片段一中sql查询的 字段名。

31 -->

1. <select id="getTeacher" resultType="teacher">
1. select \* from teacher where id = #{id}
1. </select> 35

36 </mapper>

1.  编写完毕去 Mybatis 配置文件中，注册 Mapper！
1.  注意点说明：

1.  <resultMap id="StudentTeacher" type="Student">
1.      <!--association关联属性	property属性名 javaType属性类型 column在多的一方的表中的列名-->
1.      <association property="teacher"	column="{id=tid,name=tid}" javaType="Teacher" select="getTeacher"/>
1.  </resultMap> 5 <!--
1.  这里传递过来的 id，只有一个属性的时候，下面可以写任何值
1.  association 中 column 多参数配置：
1.  column="{key=value,key=value}"
1.      其实就是键值对的形式，key是传给下个sql的取值名称，value是片段一中sql查询的字段 名。

10 -->

1. <select id="getTeacher" resultType="teacher">
1. select \* from teacher where id = #{id} and name = #{name}
1. </select>

1. 测试

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
@Test
public void testGetStudents(){
SqlSession session = MybatisUtils.getSession();
StudentMapper mapper = session.getMapper(StudentMapper.class);
List<Student> students = mapper.getStudents();

for (Student student : students){ System.out.println(
"学生名:"+ student.getName()
+"\t 老师:"+student.getTeacher().getName());
}
}

## 、按结果嵌套处理

除了上面这种方式，还有其他思路吗？ 我们还可以按照结果进行嵌套处理；

1. 接口方法编写

1 public List<Student> getStudents2();

1. 编写对应的 mapper 文件

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

<!--
按查询结果嵌套处理思路：
1. 直接查询出结果，进行结果集的映射
-->

<select id="getStudents2" resultMap="StudentTeacher2" > select s.id sid, s.name sname , t.name tname
from student s,teacher t where s.tid = t.id
</select>
<resultMap id="StudentTeacher2" type="Student">
<id property="id" column="sid"/>

1. <result property="name" column="sname"/>
1. <!--关联对象property 关联对象在Student实体类中的属性-->
1. <association property="teacher" javaType="Teacher">
1. <result property="name" column="tname"/>
1. </association>
1. </resultMap>

1. 去 mybatis-conﬁg 文件中注入【此处应该处理过了】
1. 测试

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
@Test
public void testGetStudents2(){
SqlSession session = MybatisUtils.getSession();
StudentMapper mapper = session.getMapper(StudentMapper.class);
List<Student> students = mapper.getStudents2();

for (Student student : students){ System.out.println(
"学生名:"+ student.getName()
+"\t 老师:"+student.getTeacher().getName());
}
}

## 、小结

按照查询进行嵌套处理就像 SQL 中的子查询 按照结果进行嵌套处理就像 SQL 中的联表查询

# 9、一对多的处理

一对多的理解：
一个老师拥有多个学生
如果对于老师这边，就是一个一对多的现象，即从一个老师下面拥有一群学生（集合）！

## 、实体类编写

1. @Data
1. public class Student {
1. private int id;
1. private String name;
1. private int tid; 6 }
1. @Data
1. public class Teacher {
1. private int id;
1. private String name;
1. //一个老师多个学生
1. private List<Student> students; 7 }

..... 和之前一样，搭建测试的环境！

## 、按结果嵌套处理

1. TeacherMapper 接口编写方法

1. //获取指定老师，及老师下的所有学生
1. public Teacher getTeacher(int id);

1. 编写接口对应的 Mapper 配置文件

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
25
26
<mapper namespace="com.kuang.mapper.TeacherMapper">

<!--
思路:

1. 从学生表和老师表中查出学生id，学生姓名，老师姓名
1. 对查询出来的操作做结果集映射
   1. 集合的话，使用collection！

JavaType和ofType都是用来指定对象类型的
JavaType是用来指定pojo中属性的类型
ofType指定的是映射到list集合属性中pojo的类型。
-->
<select id="getTeacher" resultMap="TeacherStudent">
select s.id sid, s.name sname , t.name tname, t.id tid from student s,teacher t
where s.tid = t.id and t.id=#{id}
</select>

<resultMap id="TeacherStudent" type="Teacher">
<result	property="name" column="tname"/>
<collection property="students" ofType="Student">
<result property="id" column="sid" />
<result property="name" column="sname" />
<result property="tid" column="tid" />
</collection>
</resultMap>
</mapper>

1. 将 Mapper 文件注册到 MyBatis-conﬁg 文件中

1. <mappers>
1. <mapper resource="mapper/TeacherMapper.xml"/>
1. </mappers>

1. 测试

1. @Test
1. public void testGetTeacher(){
1. SqlSession session = MybatisUtils.getSession();
1. TeacherMapper mapper = session.getMapper(TeacherMapper.class);
1. Teacher teacher = mapper.getTeacher(1);
1. System.out.println(teacher.getName());
1. System.out.println(teacher.getStudents()); 8 }

## 、按查询嵌套处理

1. TeacherMapper 接口编写方法

| 1   | public | Teacher | getTeacher2(int | id); |
| --- | ------ | ------- | --------------- | ---- |

1.  编写接口对应的 Mapper 配置文件

1.  <select id="getTeacher2" resultMap="TeacherStudent2">
1.  select \* from teacher where id = #{id}
1.  </select>
1.  <resultMap id="TeacherStudent2" type="Teacher">
1.  <!--column是一对多的外键 , 写的是一的主键的列名-->
1.      <collection property="students" javaType="ArrayList" ofType="Student" column="id" select="getStudentByTeacherId"/>
1.  </resultMap>
1.  <select id="getStudentByTeacherId" resultType="Student">
1.  select \* from student where tid = #{id}
1.  </select>

1.  将 Mapper 文件注册到 MyBatis-conﬁg 文件中
1.  测试

1.  @Test
1.  public void testGetTeacher2(){
1.  SqlSession session = MybatisUtils.getSession();
1.  TeacherMapper mapper = session.getMapper(TeacherMapper.class);
1.  Teacher teacher = mapper.getTeacher2(1);
1.  System.out.println(teacher.getName());
1.  System.out.println(teacher.getStudents()); 8 }

## 、小结

1. 关联-association
1. 集合-collection
1. 所以 association 是用于一对一和多对一，而 collection 是用于一对多的关系
1. JavaType 和 ofType 都是用来指定对象类型的

JavaType 是用来指定 pojo 中属性的类型
ofType 指定的是映射到 list 集合属性中 pojo 的类型。

#### 注意说明：

1. 保证 SQL 的可读性，尽量通俗易懂
1. 根据实际要求，尽量编写性能更高的 SQL 语句
1. 注意属性名和字段不一致的问题
1. 注意一对多和多对一 中：字段和属性对应的问题
1. 尽量使用 Log4j，通过日志来查看自己的错误

# 10、动态 SQL

[官方文档](http://www.mybatis.org/mybatis-3/zh/dynamic-sql.html)

## 、介绍

什么是动态 SQL：**动态 SQL 指的是根据不同的查询条件 , 生成不同的 Sql 语句.**

1.  官网描述：
1.      MyBatis 的强大特性之一便是它的动态 SQL。如果你有使用 JDBC 或其它类似框架的经验，你就能体会到根据不同条件拼接 SQL 语句的痛苦。例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL 这一特性可以彻底摆脱这种痛苦。
1.      虽然在以前使用动态 SQL 并非一件易事，但正是 MyBatis 提供了可以被用在任意 SQL 映射语句中的强大的动态 SQL 语言得以改进这种情形。
1.      动态 SQL 元素和 JSTL 或基于类似 XML 的文本处理器相似。在 MyBatis 之前的版本中，有很多元素需要花时间了解。MyBatis 3 大大精简了元素种类，现在只需学习原来一半的元素便可。

MyBatis 采用功能强大的基于 OGNL 的表达式来淘汰其它大部分元素。
5
6 -------------------------------

1. - if
1. - choose (when, otherwise)
1. - trim (where, set)
1. - foreach

11 -------------------------------

我们之前写的 SQL 语句都比较简单，如果有比较复杂的业务，我们需要写复杂的 SQL 语句，往往需要拼接，而拼接 SQL ，稍微不注意，由于引号，空格等缺失可能都会导致错误。
那么怎么去解决这个问题呢？这就要使用 mybatis 动态 SQL，通过 if, choose, when, otherwise, trim, where, set, foreach 等标签，可组合成非常灵活的 SQL 语句，从而在提高 SQL 语句的准确性的同时，也大大提高了开发人员的效率。

## 、搭建环境

#### 新建一个数据库表：blog

字段：id，title，author，create_time，views

| 1   | CREATE TABLE `blog` (                               |
| --- | --------------------------------------------------- |
| 2   | `id` varchar(50) NOT NULL COMMENT '博客 id',        |
| 3   | `title` varchar(100) NOT NULL COMMENT '博客标题',   |
| 4   | `author` varchar(30) NOT NULL COMMENT '博客作者',   |
| 5   | `create_time` datetime NOT NULL COMMENT '创建时间', |
| 6   | `views` int(30) NOT NULL COMMENT '浏览量'           |
| 7   | ) ENGINE=InnoDB DEFAULT CHARSET=utf8                |
| 8   |                                                     |

      1. 创建Mybatis基础工程

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834501308-cdaae4b9-b6b0-4ccd-97ff-6257a7fff2b1.png#)

      1. IDutil工具类

1
2
3
4
5
6
7
public class IDUtil {
public static String genId(){
return UUID.randomUUID().toString().replaceAll("-","");
}
}

      1. 实体类编写 【注意set方法作用】

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
import java.util.Date;
public class Blog {
private String id; private String title; private String author; private Date createTime; private int views;
//set，get....
}

      1. 编写Mapper接口及xml文件

1 public interface BlogMapper { 2 }
1
2
3
4
5
6
7

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" ["http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)">
<mapper namespace="com.kuang.mapper.BlogMapper">
</mapper>

      1. mybatis核心配置文件，下划线驼峰自动转换

1. <settings>
1. <setting name="mapUnderscoreToCamelCase" value="true"/>
1. <setting name="logImpl" value="STDOUT_LOGGING"/>
1. </settings>
1. <!--注册Mapper.xml-->
1. <mappers>
1. <mapper resource="mapper/BlogMapper.xml"/>
1. </mappers>

   1. 插入初始数据编写接口

1. //新增一个博客
1. int addBlog(Blog blog);

sql 配置文件

1. <insert id="addBlog" parameterType="blog">
1. insert into blog (id, title, author, create_time, views)
1. values (#{id},#{title},#{author},#{createTime},#{views});
1. </insert>

初始化博客方法

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
25
26
27
28
@Test
public void addInitBlog(){
SqlSession session = MybatisUtils.getSession(); BlogMapper mapper = session.getMapper(BlogMapper.class);
Blog blog = new Blog(); blog.setId(IDUtil.genId());
blog.setTitle("Mybatis 如此简单");
blog.setAuthor(" 狂 神 说 "); blog.setCreateTime(new Date()); blog.setViews(9999);

mapper.addBlog(blog);

blog.setId(IDUtil.genId());
blog.setTitle("Java 如此简单"); mapper.addBlog(blog);

blog.setId(IDUtil.genId());
blog.setTitle("Spring 如此简单"); mapper.addBlog(blog);

blog.setId(IDUtil.genId());
blog.setTitle("微服务如此简单"); mapper.addBlog(blog);

session.close();
}

初始化数据完毕！

## 、if 语句

#### 需求：根据作者名字和博客名字来查询博客！如果作者名字为空，那么只根据博客名字查询，反之，则 根据作者名来查询

      1. 编写接口类

1 //需求 1
2 List<Blog> queryBlogIf(Map map);

      1. 编写SQL语句

1 <!--需求 1：

1. 根据作者名字和博客名字来查询博客！
1. 如果作者名字为空，那么只根据博客名字查询，反之，则根据作者名来查询
1. select \* from blog where title = #{title} and author = #{author} 5 -->
1. <select id="queryBlogIf" parameterType="map" resultType="blog">
1. select \* from blog where
1. <if test="title != null">
1. title = #{title} 10 </if>
1. <if test="author != null">
1. and author = #{author} 13 </if>

14 </select>

      1. 测试

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
@Test
public void testQueryBlogIf(){
SqlSession session = MybatisUtils.getSession(); BlogMapper mapper = session.getMapper(BlogMapper.class);
HashMap<String, String> map = new HashMap<String, String>();
map.put("title","Mybatis 如此简单");
map.put("author","狂神说");
List<Blog> blogs = mapper.queryBlogIf(map);

System.out.println(blogs);

session.close();
}

这样写我们可以看到，如果 author 等于 null，那么查询语句为 select _ from user where title=#{title},
但是如果 title 为空呢？那么查询语句为 select _ from user where and author=#{author}，这是错误的
SQL 语句，如何解决呢？请看下面的 where 语句！

## 、Where

修改上面的 SQL 语句；

1. <select id="queryBlogIf" parameterType="map" resultType="blog">
1. select \* from blog
1. <where>
1. <if test="title != null">
1. title = #{title} 6 </if>
1. <if test="author != null">
1. and author = #{author} 9 </if>
1. </where>
1. </select>

这个“where”标签会知道如果它包含的标签中有返回值的话，它就插入一个‘where’。此外，如果标签返 回的内容是以 AND 或 OR 开头的，则它会剔除掉。【这是我们使用的最多的案例】

## 、Set

同理，上面的对于查询 SQL 语句包含 where 关键字，如果在进行更新操作的时候，含有 set 关键词， 我们怎么处理呢？

      1. 编写接口方法

1 int updateBlog(Map map);

      1. sql配置文件

1. <!--注意set是用的逗号隔开-->
1. <update id="updateBlog" parameterType="map">
1. update blog
1. <set>
1. <if test="title != null">
1. title = #{title}, 7 </if>
1. <if test="author != null">
1. author = #{author} 10 </if>
1. </set>
1. where id = #{id};
1. </update>

   1. 测试

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
@Test
public void testUpdateBlog(){
SqlSession session = MybatisUtils.getSession(); BlogMapper mapper = session.getMapper(BlogMapper.class);
HashMap<String, String> map = new HashMap<String, String>();
map.put("title","动态 SQL");
map.put("author"," 秦 疆 "); map.put("id","9d6a763f5e1347cebda43e2a32687a77");

mapper.updateBlog(map);

session.close();
}

【演示】SQL 分析

## 、choose 语句

有时候，我们不想用到所有的查询条件，只想选择其中的一个，查询条件有一个满足即可，使用 choose
标签可以解决此类问题，类似于 Java 的 switch 语句

      1. 编写接口方法

1 List<Blog> queryBlogChoose(Map map);

      1. sql配置文件

1. <select id="queryBlogChoose" parameterType="map" resultType="blog">
1. select \* from blog
1. <where>
1. <choose>
1. <when test="title != null">
1. title = #{title}
1. </when>
1. <when test="author != null">
1. and author = #{author}
1. </when>
1. <otherwise>
1. and views = #{views}
1. </otherwise>
1. </choose>
1. </where>
1. </select>

   1. 测试类

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
@Test
public void testQueryBlogChoose(){
SqlSession session = MybatisUtils.getSession(); BlogMapper mapper = session.getMapper(BlogMapper.class);
HashMap<String, Object> map = new HashMap<String, Object>();
map.put("title","Java 如此简单");
map.put("author","狂神说");
map.put("views",9999);
List<Blog> blogs = mapper.queryBlogChoose(map);

System.out.println(blogs);

session.close();
}

【演示】SQL 分析

## 、SQL 片段

有时候可能某个 sql 语句我们用的特别多，为了增加代码的重用性，简化代码，我们需要将这些代码抽取出来，然后使用时直接调用。

#### 提取 SQL 片段：

1. <sql id="if-title-author">
1. <if test="title != null">
1. title = #{title} 4 </if>
1. <if test="author != null">
1. and author = #{author} 7 </if>

8 </sql>

**引用 SQL 片段：**

1. <select id="queryBlogIf" parameterType="map" resultType="blog">
1. select \* from blog
1. <where>
1. <!-- 引用 sql 片段，如果refid 指定的不在本文件中，那么需要在前面加上 namespace

-->

1. <include refid="if-title-author"></include>
1. <!-- 在这里还可以引用其他的 sql 片段 -->
1. </where>
1. </select>

注意：①、最好基于 单表来定义 sql 片段，提高片段的可重用性
②、在 sql 片段中不要包括 where

## 、Foreach

将数据库中前三个数据的 id 修改为 1,2,3；
需求：我们需要查询 blog 表中 id 分别为 1,2,3 的博客信息

      1. 编写接口

1 List<Blog> queryBlogForeach(Map map);

      1. 编写SQL语句

1. <select id="queryBlogForeach" parameterType="map" resultType="blog">
1. select \* from blog
1. <where>

4 <!--

1.  collection:指定输入对象中的集合属性
1.  item:每次遍历生成的对象
1.  open:开始遍历时的拼接字符串
1.  close:结束时拼接的字符串
1.  separator:遍历对象之间需要拼接的字符串
1.  select \* from blog where 1=1 and (id=1 or id=2 or id=3) 11 -->
1.      <foreach collection="ids"	item="id" open="and (" close=")" separator="or">
1.  id=#{id}
1.  </foreach>
1.  </where>
1.  </select>

    1. 测试

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
@Test
public void testQueryBlogForeach(){
SqlSession session = MybatisUtils.getSession(); BlogMapper mapper = session.getMapper(BlogMapper.class);
HashMap map = new HashMap();
List<Integer> ids = new ArrayList<Integer>(); ids.add(1);
ids.add(2);
ids.add(3); map.put("ids",ids);

List<Blog> blogs = mapper.queryBlogForeach(map);

System.out.println(blogs);

session.close();
}

小结：其实动态 sql 语句的编写往往就是一个拼接的问题，为了保证拼接准确，我们最好首先要写原生的 sql 语句出来，然后在通过 mybatis 动态 sql 对照着改，防止出错。多在实践中使用才是熟练掌握它的技巧

# 11、缓存

## 、简介

      1. 什么是缓存 [ Cache ]？

存在内存中的临时数据。
将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库 数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。

      1. 为什么使用缓存？

减少和数据库的交互次数，减少系统开销，提高系统效率。

      1. 什么样的数据能使用缓存？

经常查询并且不经常改变的数据。

## 、Mybatis 缓存

MyBatis 包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。
MyBatis 系统中默认定义了两级缓存：**一级缓存**和**二级缓存**
默认情况下，只有一级缓存开启。（SqlSession 级别的缓存，也称为本地缓存） 二级缓存需要手动开启和配置，他是基于 namespace 级别的缓存。
为了提高扩展性，MyBatis 定义了缓存接口 Cache。我们可以通过实现 Cache 接口来自定义二 级缓存

## 、一级缓存

一级缓存也叫本地缓存：

与数据库同一次会话期间查询到的数据会放在本地缓存中。
以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库；

### 、初体验测试

         1. 在mybatis中加入日志，方便测试结果
         1. 编写接口方法

1.  //根据 id 查询用户
1.  User queryUserById(@Param("id") int id);

         1. 接口对应的Mapper文件

1.  <select id="queryUserById" resultType="user">
1.  select \* from user where id = #{id}
1.  </select>

         1. 测试

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
@Test
public void testQueryUserById(){
SqlSession session = MybatisUtils.getSession(); UserMapper mapper = session.getMapper(UserMapper.class);
User user = mapper.queryUserById(1); System.out.println(user);
User user2 = mapper.queryUserById(1); System.out.println(user2); System.out.println(user==user2);

session.close();
}

         1. ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834501853-ba0757a3-34fa-4aea-bf9b-ae38a3e35bd2.png#)结果分析

### 、一级缓存失效的四种情况

一级缓存是 SqlSession 级别的缓存，是一直开启的，我们关闭不了它；
一级缓存失效情况：没有使用到当前的一级缓存，效果就是，还需要再向数据库中发起一次查询请 求！

         1. sqlSession不同

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
@Test
public void testQueryUserById(){
SqlSession session = MybatisUtils.getSession(); SqlSession session2 = MybatisUtils.getSession(); UserMapper mapper = session.getMapper(UserMapper.class); UserMapper mapper2 = session2.getMapper(UserMapper.class);
User user = mapper.queryUserById(1); System.out.println(user);
User user2 = mapper2.queryUserById(1); System.out.println(user2); System.out.println(user==user2);

session.close(); session2.close();
}

观察结果：发现发送了两条 SQL 语句！

#### 结论：每个 sqlSession 中的缓存相互独立

         1. sqlSession相同，查询条件不同

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
@Test
public void testQueryUserById(){
SqlSession session = MybatisUtils.getSession(); UserMapper mapper = session.getMapper(UserMapper.class); UserMapper mapper2 = session.getMapper(UserMapper.class);
User user = mapper.queryUserById(1); System.out.println(user);
User user2 = mapper2.queryUserById(2); System.out.println(user2); System.out.println(user==user2);

session.close();
}

观察结果：发现发送了两条 SQL 语句！很正常的理解结论：**当前缓存中，不存在这个数据**

         1. sqlSession相同，两次查询之间执行了增删改操作！

增加方法

1. //修改用户
1. int updateUser(Map map);

编写 SQL

1. <update id="updateUser" parameterType="map">
1. update user set name = #{name} where id = #{id}
1. </update>

测试

1. @Test
1. public void testQueryUserById(){
1. SqlSession session = MybatisUtils.getSession();

| 4   |     | UserMapper mapper = session.getMapper(UserMapper.class); |
| --- | --- | -------------------------------------------------------- |
| 5   |     |                                                          |
| 6   |     | User user = mapper.queryUserById(1);                     |
| 7   |     | System.out.println(user);                                |
| 8   |     |                                                          |
| 9   |     | HashMap map = new HashMap();                             |
| 10  |     | map.put("name","kuangshen");                             |
| 11  |     | map.put("id",4);                                         |
| 12  |     | mapper.updateUser(map);                                  |
| 13  |     |                                                          |
| 14  |     | User user2 = mapper.queryUserById(1);                    |
| 15  |     | System.out.println(user2);                               |
| 16  |     |                                                          |
| 17  |     | System.out.println(user==user2);                         |
| 18  |     |                                                          |
| 19  |     | session.close();                                         |
| 20  | }   |                                                          |

观察结果：查询在中间执行了增删改操作后，重新执行了结论：**因为增删改操作可能会对当前数据产生影响**

         1. sqlSession相同，手动清除一级缓存

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
@Test
public void testQueryUserById(){
SqlSession session = MybatisUtils.getSession(); UserMapper mapper = session.getMapper(UserMapper.class);
User user = mapper.queryUserById(1); System.out.println(user);

session.clearCache();//手动清除缓存

User user2 = mapper.queryUserById(1); System.out.println(user2);

System.out.println(user==user2);

session.close();
}

一级缓存就是一个 map

## 、二级缓存

二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存基于 namespace 级别的缓存，一个名称空间，对应一个二级缓存；
工作机制
一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一 级缓存中的数据被保存到二级缓存中；
新的会话查询信息，就可以从二级缓存中获取内容；
不同的 mapper 查出的数据会放在自己对应的缓存（map）中；

### 、使用步骤

[官方文档](http://www.mybatis.org/mybatis-3/zh/sqlmap-xml.html#cache)

         1. 开启全局缓存 【mybatis-conﬁg.xml】

1 <setting name="cacheEnabled" value="true"/>

         1. 去每个mapper.xml中配置使用二级缓存，这个配置非常简单；【xxxMapper.xml】

1
2
3
4
5
6
7
8
9
<cache/>
官方示例=====>查看官方文档
<cache
eviction="FIFO" flushInterval="60000" size="512" readOnly="true"/>
这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的
512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。

         1. 代码测试

所有的实体类先实现序列化接口测试代码

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
@Test
public void testQueryUserById(){
SqlSession session = MybatisUtils.getSession(); SqlSession session2 = MybatisUtils.getSession();
UserMapper mapper = session.getMapper(UserMapper.class); UserMapper mapper2 = session2.getMapper(UserMapper.class);

User user = mapper.queryUserById(1); System.out.println(user); session.close();

User user2 = mapper2.queryUserById(1); System.out.println(user2); System.out.println(user==user2);

session2.close();
}

### 、结论

只要开启了二级缓存，我们在同一个 Mapper 中的查询，可以在二级缓存中拿到数据查出的数据都会被默认先放在一级缓存中
只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中

## 、缓存原理

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834502451-9f8b27d5-8ad5-4a06-b4b3-a4068552adab.jpeg#)

1.  **、EhCache**

第三方缓存实现--EhCache: 查看百度百科[官方文档](http://www.mybatis.org/ehcache-cache/)
Ehcache 是一种广泛使用的 java 分布式缓存，用于通用缓存；
要在应用程序中使用 Ehcache，需要引入依赖的 jar 包

1. <!-- [https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-](https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-) ehcache -->
1. <dependency>
1. <groupId>org.mybatis.caches</groupId>
1. <artifactId>mybatis-ehcache</artifactId>
1. <version>1.1.0</version>
1. </dependency>

在 mapper.xml 中使用对应的缓存即可
/ehcache.xml

1. <mapper namespace = “org.acme.FooMapper” >
1. <cache type = “org.mybatis.caches.ehcache.EhcacheCache” />
1. </mapper>

编写 ehcache.xml 文件，如果在认配置。
加载时
未找到
资源或出现问题，则将使用默

1. <?xml version="1.0" encoding="UTF-8"?>
1. <ehcache xmlns:xsi=["http://www.w3.org/2001/XMLSchema-instance](http://www.w3.org/2001/XMLSchema-instance)"
1. xsi:noNamespaceSchemaLocation=["http://ehcache.org/ehcache.xsd](http://ehcache.org/ehcache.xsd)"
1. updateCheck="false"> 5 <!--
1. diskStore：为缓存路径，ehcache 分为内存和磁盘两级，此属性定义磁盘的缓存位

置。参数解释如下：

1. user.home – 用户主目录
1. user.dir – 用户当前工作目录
1. java.io.tmpdir – 默认临时文件路径

10 -->
11 <diskStore path="./tmpdir/Tmp_EhCache"/>

12

1. <defaultCache
1. eternal="false"
1. maxElementsInMemory="10000"
1. overflowToDisk="false"
1. diskPersistent="false"
1. timeToIdleSeconds="1800"
1. timeToLiveSeconds="259200"
1. memoryStoreEvictionPolicy="LRU"/> 21
1. <cache
1. name="cloud_user"
1. eternal="false"
1. maxElementsInMemory="5000"
1. overflowToDisk="false"
1. diskPersistent="false"
1. timeToIdleSeconds="1800"
1. timeToLiveSeconds="1800"
1. memoryStoreEvictionPolicy="LRU"/> 31 <!--

32 defaultCache：默认缓存策略，当 ehcache 找不到定义的缓存时，则使用这个缓存策
略。只能定义一个。
33 -->
34 <!--

1. name:缓存名称。
1. maxElementsInMemory:缓存最大数目
1. maxElementsOnDisk：硬盘最大缓存个数。
1. eternal:对象是否永久有效，一但设置了，timeout 将不起作用。
1. overflowToDisk:是否保存到磁盘，当系统当机时
1. timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当

eternal=false 对象不是永久有效时使用，可选属性，默认值是 0，也就是可闲置时间无穷大。

1.      timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建  时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存  活时间无穷大。
1.      diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
1.  diskSpoolBufferSizeMB：这个参数设置 DiskStore（磁盘缓存）的缓存区大小。默

认是 30MB。每个 Cache 都应该有自己的一个缓冲区。

1.  diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是 120 秒。
1.      memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将 会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
1.  clearOnFlush：内存数量最大时是否清除。
1.      memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
1.  FIFO，first in first out，这个是大家最熟的，先进先出。
1.      LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
1.      LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的  元素将被清出缓存。

51 -->
52
53 </ehcache>
