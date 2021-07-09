---
title: 15、MySQL和JDBC
urlname: ly7r5v
date: '2021-07-09 20:38:43 +0800'
tags: []
categories: []
---

# 1、初识 MySQL

只会写代码的是码农；学好数据库，基本能混口饭吃；在此基础上再学好操作系统和计算机网络， 就能当一个不错的程序员。如果能再把离散数学、数字电路、体系结构、数据结构/算法、编译原理 学通透，再加上丰富的实践经验与领域特定知识，就能算是一个优秀的工程师了。

## 、为什么学习数据库

1、岗位技能需求
2、现在的世界,得数据者得天下
3、存储数据的方法
4、程序,网站中,大量数据如何长久保存?
5、**数据库是几乎软件体系中最核心的一个存在。**

## 、什么是数据库

数据库 ( **DataBase **, 简称**DB **)
**概念 **: 长期存放在计算机内,有组织,可共享的大量数据的集合,是一个数据 "仓库"
**作用 **: 保存,并能安全管理数据(如:增删改查等),减少冗余...

### 数据库总览 :

关系型数据库 ( SQL )
MySQL , Oracle , SQL Server , SQLite , DB2 , ...
关系型数据库通过外键关联来建立表与表之间的关系非关系型数据库 ( NOSQL )
Redis , MongoDB , ...
非关系型数据库通常指数据以对象的形式存储在数据库中，而对象之间的关系通过每个对象自 身的属性来决定

## 、什么是 DBMS

数据库管理系统 ( **D**ata**B**ase **M**anagement **S**ystem )
数据库管理软件 , 科学组织和存储数据 , 高效地获取和维护数据

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834327827-643866b1-245a-4285-a7af-2ed63fcf5a60.jpeg#)

为什么要说这个呢?
因为我们要学习的 MySQL 应该算是一个数据库管理系统.

## ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834328304-13c837bd-3f56-437c-8ffc-02cef607696b.jpeg#)、MySQL 简介

**概念 : **是现在**流行**的**开源**的,**免费**的 **关系型**数据库
**历史 : **由瑞典 MySQL AB 公司开发，目前属于 Oracle 旗下产品。

### 特点 :

免费 , 开源数据库小巧 , 功能齐全使用便捷
可运行于 Windows 或 Linux 操作系统可适用于中小型甚至大型网站应用
**官网 : **[**https://www.mysql.com/**](https://www.mysql.com/)

## 、安装 MySQL

### 这里建议大家使用压缩版,安装快,方便.不复杂.

1、软件下载

mysql5.7 64 位下载地址:
[https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19-winx64.zip](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19-winx64.zip)
电脑是 64 位的就下载使用 64 位版本的！

2、步骤
1、下载后得到 zip 压缩包.
2、解压到自己想要安装到的目录，本人解压到的是 D:\Environment\mysql-5.7.19 3、添加环境变量：我的电脑->属性->高级->环境变量
选择 PATH,在其后面添加: 你的 mysql 安装文件下面的 bin 文件夹
1

4、编辑 my.ini 文件 ,注意替换路径位置

| 1   | [mysqld]                                  |
| --- | ----------------------------------------- |
| 2   | basedir=D:\Program Files\mysql-5.7\       |
| 3   | datadir=D:\Program Files\mysql-5.7\data\  |
| 4   | port=3306                                 |
| 5   | skip-grant-tables                         |

5、启动管理员模式下的 CMD，并将路径切换至 mysql 下的 bin 目录，然后输入 mysqld –install (安装 mysql)
6、再输入 初始化数据文件
mysqld --initialize-insecure --user=mysql
7、然后再次启动 mysql 然后用命令 mysql –u root –p 进入 mysql 管理界面（密码可为空）
8、进入界面后更改 root 密码

update mysql.user set authentication_string=password('123456') where
user='root' and Host = 'localhost';
1

9、刷新权限

flush privileges;
1

10、修改 my.ini 文件删除最后一句 skip-grant-tables 11、重启 mysql 即可正常使用

| 1   | net | stop mysql  |
| --- | --- | ----------- |
| 2   | net | start mysql |

12、连接上测试出现以下结果就安装好了

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834328804-7599eb75-744c-451d-bc0e-db90524ae65a.png#)

一步步去做 , 理论上是没有任何问题的 .
如果您以前装过,现在需要重装,一定要将环境清理干净 .
好了,到这里大家都装好了,因为刚接触,所以我们先不学习命令. 这里给大家推荐一个工具 : **SQLyog **.
即便有了可视化工具,可是基本的 DOS 命名大家还是要记住!

## 、SQLyog

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834329457-d404367f-3805-4959-9df2-b3a95826208d.jpeg#)可手动操作,管理 MySQL 数据库的软件工具特点 : 简洁 , 易用 , 图形化

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834329913-73d792a4-8076-44fc-97cc-23369e2be473.png#)

使用 SQLyog 管理工具自己完成以下操作 :
连接本地 MySQL 数据库新建 MySchool 数据库
数据库名称 MySchool
新建数据库表(grade) 字段
GradeID : int(11) , Primary Key (pk) GradeName : varchar(50)
在历史记录中可以看到相对应的数据库操作的语句 .

## 、连接数据库

打开 MySQL 命令窗口
在 DOS 命令行窗口进入 **安装目录\mysql\bin**
可设置环境变量，设置了环境变量，可以在任意目录打开！

### 连接数据库语句 :

mysql -h 服务器主机地址 -u 用户名 -p 用户密码
注意 : -p 后面不能加空格,否则会被当做密码的内容,导致登录失败 !
**几个基本的数据库操作命令 :**

| 1   | update user set password=password('123456')where user='root'; 修改密码 |
| --- | ---------------------------------------------------------------------- |
| 2   | flush privileges; 刷新数据库                                           |
| 3   | show databases; 显示所有数据库                                         |
| 4   | use dbname； 打开某个数据库                                            |
| 5   | show tables; 显示数据库 mysql 中所有的表                               |
| 6   | describe user; 显示表 mysql 数据库中 user 表的列信息                   |
| 7   | create database name; 创建数据库                                       |
| 8   | use databasename; 选择数据库                                           |
| 9   |                                                                        |
| 10  | exit; 退出 Mysql                                                       |
| 11  | ? 命令关键词 : 寻求帮助                                                |
| 12  | -- 表示注释                                                            |

# 2、操作数据库

## 、结构化查询语句分类

| **名称**             | **解释**                                     | **命令**                |
| -------------------- | -------------------------------------------- | ----------------------- |
| DDL （数据定义语言） | 定义和管理数据对象，如数据库，数据表等       | CREATE、DROP、ALTER     |
| DML （数据操作语言） | 用于操作数据库对象中所包含的数据             | INSERT、UPDATE、DELETE  |
| DQL （数据查询语言） | 用于查询数据库数据                           |
| SELECT               |
| DCL （数据控制语言） | 用于管理数据库的语言，包括管理权限及数据更改 | GRANT、commit、rollback |

1.  **、数据库操作**

命令行操作数据库
create database [if not exists] 数据库名;
drop database [if exists] 数据库名;
show databases;
创建数据库 : 删除数据库 : 查看数据库 : 使用数据库 :

对比工具操作数据库

### 学习方法：

对照 SQLyog 工具自动生成的语句学习固定语法中的单词需要记忆
use 数据库名;

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834330394-a9c29a88-45c5-4dfe-a7c0-601f7a6ac4c3.png#)

## 、创建数据表

属于 DDL 的一种，语法 :

1. create table [if not exists] `表名`(
1. '字段名 1' 列类型 [属性][索引][注释],
1. '字段名 2' 列类型 [属性][索引][注释], 4 #...
1. '字段名 n' 列类型 [属性][索引][注释]
1. )[表类型][表字符集][注释];

**说明 : **反引号用于区别 MySQL 保留字与普通字符而引入的 (键盘 esc 下面的键).

## 、数据值和列类型

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834330970-ed0905e2-7987-41fe-a042-8eeefe660b1f.jpeg#)列类型 : 规定数据库中该列存放的数据类型
数值类型

字符串类型

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834331526-03b90dd3-0cff-4cb6-88a0-d4930709db2e.jpeg#)
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834331990-231cfef9-0dbf-4c76-8b91-c184cacd87df.jpeg#)
日期和时间型数值类型
NULL 值

理解为 "没有值" 或 "未知值"
不要用 NULL 进行算术运算 , 结果仍为 NULL

## 、数据字段属性

### UnSigned

无符号的
声明该数据列不允许负数 .

### ZEROFILL

0 填充的
不足位数的用 0 来填充 , 如 int(3),5 则为 005

### Auto_InCrement

自动增长的 , 每添加一条数据 , 自动在上一个记录数上加 1(默认)
通常用于设置**主键 **, 且为整数类型可定义起始值和步长
当前表设置步长(AUTO_INCREMENT=100) : 只影响当前表
SET @@auto_increment_increment=5 ; 影响所有使用自增的表(全局)

### NULL 和 NOT NULL

默认为 NULL , 即没有插入该列的数值
如果设置为 NOT NULL , 则该列必须有值

### DEFAULT

默认的
用于设置默认值
例如,性别字段,默认为"男" , 否则为 "女" ; 若无指定该列的值 , 则默认值为"男"的值

| 1   | -- 目标 : 创建一个 school 数据库                                                |
| --- | ------------------------------------------------------------------------------- |
| 2   | -- 创建学生表(列,字段)                                                          |
| 3   | -- 学号 int 登录密码 varchar(20) 姓名,性别 varchar(2),出生日期(datatime),家庭住 |
|     | 址,email                                                                        |
| 4   | -- 创建表之前 , 一定要先选择数据库                                              |
| 5   |                                                                                 |
| 6   | CREATE TABLE IF NOT EXISTS `student` (                                          |
| 7   | `id` int(4) NOT NULL AUTO_INCREMENT COMMENT '学号',                             |
| 8   | `name` varchar(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',                      |
| 9   | `pwd` varchar(20) NOT NULL DEFAULT '123456' COMMENT '密码',                     |
| 10  | `sex` varchar(2) NOT NULL DEFAULT '男' COMMENT '性别',                          |
| 11  | `birthday` datetime DEFAULT NULL COMMENT '生日',                                |
| 12  | `address` varchar(100) DEFAULT NULL COMMENT '地址',                             |
| 13  | `email` varchar(50) DEFAULT NULL COMMENT '邮箱',                                |
| 14  | PRIMARY KEY (`id`)                                                              |
| 15  | ) ENGINE=InnoDB DEFAULT CHARSET=utf8                                            |
| 16  |                                                                                 |
| 17  | -- 查看数据库的定义                                                             |
| 18  | SHOW CREATE DATABASE school;                                                    |
| 19  | -- 查看数据表的定义                                                             |
| 20  | SHOW CREATE TABLE student;                                                      |
| 21  | -- 显示表结构                                                                   |
| 22  | DESC student; -- 设置严格检查模式(不能容错了)SET                                |
|     | sql_mode='STRICT_TRANS_TABLES';                                                 |

## 、数据表的类型

设置数据表的类型

| 1   | CREATE TABLE 表名(                      |
| --- | --------------------------------------- |
| 2   | -- 省略一些代码                         |
| 3   | -- Mysql 注释                           |
| 4   | -- 1. # 单行注释                        |
| 5   | -- 2. /_..._/ 多行注释                  |
| 6   | )ENGINE = MyISAM (or InnoDB)            |
| 7   |                                         |
| 8   | -- 查看 mysql 所支持的引擎类型 (表类型) |
| 9   | SHOW ENGINES;                           |

MySQL 的数据表的类型 : **MyISAM **, **InnoDB **, HEAP , BOB , CSV 等...
常见的 MyISAM 与 InnoDB 类型：

| **名称**   | **MyISAM** | **InnoDB**      |
| ---------- | ---------- | --------------- |
| 事务处理   | 不支持     | 支持            |
| 数据行锁定 | 不支持     | 支持            |
| 外键约束   | 不支持     | 支持            |
| 全文索引   | 支持       | 不支持          |
| 表空间大小 | 教小       | 较大，约 2 倍！ |

经验 ( 适用场合 ) :
适用 MyISAM : 节约空间及相应速度
适用 InnoDB : 安全性 , 事务处理及多用户操作数据表

数据表的存储位置
MySQL 数据表以文件方式存放在磁盘中
包括表文件 , 数据文件 , 以及数据库的选项文件
位置 : Mysql 安装目录\data\下存放数据表 . 目录名对应数据库名 , 该目录下文件名对应数据表
.
注意 :
InnoDB 类型数据表只有一个 \*.frm 文件 , 以及上一级目录的 ibdata1 文件
MyISAM 类型数据表对应三个文件 :

- . frm -- 表结构定义文件
- . MYD -- 数据文件 ( data )
- ![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834332578-85675634-97be-4322-9359-0d24c7e5cc3e.jpeg#). MYI -- 索引文件 ( index )

设置数据表字符集

我们可为数据库,数据表,数据列设定不同的字符集，设定方法 :
创建时通过命令来设置 , 如 :
CREATE TABLE 表名()CHARSET = utf8;
如无设定 , 则根据 MySQL 数据库配置文件 my.ini 中的参数设定

## 、修改数据库

修改表 ( ALTER TABLE )
ALTER TABLE 旧表名 RENAME AS 新表名
修改表名 : 添加字段 : 修改字段 :

删除字段 :
ALTER TABLE 表名 ADD 字段名 列属性[属性]
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 列属性[属性]
ALTER TABLE 表名 MODIFY 字段名 列类型[属性]
ALTER TABLE 表名 DROP 字段名

删除数据表
语法：
为可选 , 判断是否存在该数据表
DROP TABLE [IF EXISTS] 表名
IF EXISTS
如删除不存在的数据表会抛出错误

其他

| 1   | 1. 可用反引号（`）为标识符（库名、表名、字段名、索引、别名）包裹，以避免与关键字重名！中文 |
| --- | ------------------------------------------------------------------------------------------ |
|     | 也可以作为标识符！                                                                         |
| 2   |                                                                                            |
| 3   | 2. 每个库目录存在一个保存当前数据库的选项文件 db.opt。                                     |
| 4   |                                                                                            |
| 5   | 3. 注释：                                                                                  |
| 6   | 单行注释 # 注释内容                                                                        |
| 7   | 多行注释 /_ 注释内容 _/                                                                    |
| 8   | 单行注释 -- 注释内容 (标准 SQL 注释风格，要求双破折号后加一空格符（空格、TAB、             |
|     | 换行等）)                                                                                  |
| 9   |                                                                                            |
| 10  | 4. 模式通配符：                                                                            |
| 11  | \_ 任意单个字符                                                                            |
| 12  | % 任意多个字符，甚至包括零字符                                                             |
| 13  | 单引号需要进行转义 \'                                                                      |
| 14  |                                                                                            |
| 15  | 5. CMD 命令行内的语句结束符可以为 ";", "\G", "\g"，仅影响显示结果。其他地方还是用分号结    |
|     | 束。delimiter 可修改当前对话的语句结束符。                                                 |
| 16  |                                                                                            |
| 17  | 6. SQL 对大小写不敏感 （关键字）                                                           |
| 18  |                                                                                            |

19 7. 清除已有语句：\c

# 3、MySQL 数据管理

## 、外键

外键概念
如果公共关键字在一个关系中是主关键字，那么这个公共关键字被称为另一个关系的外键。由此可见， 外键表示了两个关系之间的相关联系。以另一个关系的外键作主关键字的表被称为**主表**，具有此外键的 表被称为主表的**从表**。
在实际操作中，将一个表的值放入第二个表来表示关联，所使用的值是第一个表的主键值(在必要时可包 括复合主键值)。此时，第二个表中保存这些值的属性称为外键(**foreign key**)。

### 外键作用

保持数据**一致性**，**完整性**，主要目的是控制存储在外键表中的数据,**约束**。 使两张表形成关联，外键只能引用外表中的列的值或使用空值。

创建外键
建表时指定外键约束

| 1   | -- 创建外键的方式一 : 创建子表同时创建外键                           |
| --- | -------------------------------------------------------------------- |
| 2   |                                                                      |
| 3   | -- 年级表 (id\年级名称)                                              |
| 4   | CREATE TABLE `grade` (                                               |
| 5   | `gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级 ID',         |
| 6   | `gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',                 |
| 7   | PRIMARY KEY (`gradeid`)                                              |
| 8   | ) ENGINE=INNODB DEFAULT CHARSET=utf8                                 |
| 9   |                                                                      |
| 10  | -- 学生信息表 (学号,姓名,性别,年级,手机,地址,出生日期,邮箱,身份证号) |
| 11  | CREATE TABLE `student` (                                             |
| 12  | `studentno` INT(4) NOT NULL COMMENT '学号',                          |
| 13  | `studentname` VARCHAR(20) NOT NULL DEFAULT '匿名' COMMENT '姓名',    |
| 14  | `sex` TINYINT(1) DEFAULT '1' COMMENT '性别',                         |
| 15  | `gradeid` INT(10) DEFAULT NULL COMMENT '年级',                       |
| 16  | `phoneNum` VARCHAR(50) NOT NULL COMMENT '手机',                      |
| 17  | `address` VARCHAR(255) DEFAULT NULL COMMENT '地址',                  |
| 18  | `borndate` DATETIME DEFAULT NULL COMMENT '生日',                     |
| 19  | `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',                     |
| 20  | `idCard` VARCHAR(18) DEFAULT NULL COMMENT '身份证号',                |
| 21  | PRIMARY KEY (`studentno`),                                           |
| 22  | KEY `FK_gradeid` (`gradeid`),                                        |
| 23  | CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`   |
|     | (`gradeid`)                                                          |
| 24  | ) ENGINE=INNODB DEFAULT CHARSET=utf8                                 |

建表后修改

| 1   | -- 创建外键方式二 : 创建子表完毕后,修改子表添加外键                    |
| --- | ---------------------------------------------------------------------- |
| 2   | ALTER TABLE `student`                                                  |
| 3   | ADD CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` |
|     | (`gradeid`);                                                           |

删除外键
操作：删除 grade 表，发现报错
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834333085-238630a4-0878-4efd-96d0-7a288a18f1dd.png#)
**注意 **: 删除具有主外键关系的表时 , 要先删子表 , 后删主表

| 1   | -- 删除外键                                      |
| --- | ------------------------------------------------ |
| 2   | ALTER TABLE student DROP FOREIGN KEY FK_gradeid; |
| 3   | -- 发现执行完上面的,索引还在,所以还要删除索引    |
| 4   | -- 注:这个索引是建立外键的时候默认生成的         |
| 5   | ALTER TABLE student DROP INDEX FK_gradeid;       |

## 、DML 语言

**数据库意义 **： 数据存储、数据管理

### 管理数据库数据方法：

通过 SQLyog 等管理工具管理数据库数据通过**DML 语句**管理数据库数据
**DML 语言 **： 数据操作语言
用于操作数据库对象中所包含的数据包括 :
INSERT (添加数据语句) UPDATE (更新数据语句)

DELETE (删除数据语句)

## 、添加数据

INSERT 命令

### 语法：

| 1   | INSERT | INTO | 表名[(字段 1,字段 2,字段 3,...)] | VALUES('值 1','值 2','值 3') |
| --- | ------ | ---- | -------------------------------- | ---------------------------- |

**注意 :**
字段或值之间用英文逗号隔开 .
' 字段 1,字段 2...' 该部分可省略 , 但添加的值务必与表结构,数据列,顺序相对应,且数量一致 .
可同时插入多条数据 , values 后用英文逗号隔开 .

| 1   | -- 使用语句如何增加语句?                                                                   |
| --- | ------------------------------------------------------------------------------------------ |
| 2   | -- 语法 : INSERT INTO 表名[(字段 1,字段 2,字段 3,...)] VALUES('值 1','值 2','值 3')        |
| 3   | INSERT INTO grade(gradename) VALUES ('大一');                                              |
| 4   |                                                                                            |
| 5   | -- 主键自增,那能否省略呢?                                                                  |
| 6   | INSERT INTO grade VALUES ('大二');                                                         |
| 7   |                                                                                            |
| 8   | -- 查询:INSERT INTO grade VALUE ('大二')错误代码： 1136                                    |
| 9   | Column count doesn`t match value count at row 1                                            |
| 10  |                                                                                            |
| 11  | -- 结论:'字段 1,字段 2...'该部分可省略 , 但添加的值务必与表结构,数据列,顺序相对应,且数量一 |
|     | 致.                                                                                        |
| 12  |                                                                                            |
| 13  | -- 一次插入多条数据                                                                        |
| 14  | INSERT INTO grade(gradename) VALUES ('大三'),('大四');                                     |

### 练习题目

自己使用 INSERT 语句为课程表 subject 添加数据 . 使用到外键.

## 、修改数据

update 命令
语法：

UPDATE 表名 SET column_name=value [,column_name2=value2,...] [WHERE condition];
1

### 注意 :

column_name 为要更改的数据列
value 为修改后的数据 , 可以为变量 , 具体指 , 表达式或者嵌套的 SELECT 结果
condition 为筛选条件 , 如不指定则修改该表的所有列数据

where 条件子句
可以简单的理解为 : 有条件地从表中筛选数据

| **运算符** | **含义**       | **范围**         | **结果** |
| ---------- | -------------- | ---------------- | -------- |
| =          | 等于           | 5=6              | false    |
| <> 或 ！=  | 不等于         | 5!=6             | true     |
| >          | 大于           | 5>6              | false    |
| <          | 小于           | 5<6              | true     |
| >=         | 大于等于       | 5>=6             | false    |
| <=         | 小于等于       | 5<=6             | true     |
| BETWEEN    | 在某个范围之间 | BETWEEN 5 AND 10 |          |
| AND        | 并且           | 5 > 1 AND 1 > 2  | false    |
| OR         | 或             | 5 > 1 OR 1 > 2   | true     |

测试：

| 1   | -- 修改年级信息                                        |
| --- | ------------------------------------------------------ |
| 2   | UPDATE grade SET gradename = '高中' WHERE gradeid = 1; |

## 、删除数据

DELETE 命令
语法：

| 1   | DELETE | FROM | 表名 | [WHERE | condition]; |
| --- | ------ | ---- | ---- | ------ | ----------- |

注意：condition 为筛选条件 , 如不指定则删除该表的所有列数据

| 1   | -- 删除最后一个数据                 |
| --- | ----------------------------------- |
| 2   | DELETE FROM grade WHERE gradeid = 5 |

TRUNCATE 命令
作用：用于完全清空表数据 , 但表结构 , 索引 , 约束等不变 ;
语法：

| 1   | TRUNCATE [TABLE] | table_name; |
| --- | ---------------- | ----------- |
| 2   |                  |             |
| 3   | -- 清空年级表    |             |
| 4   | TRUNCATE grade   |             |

### 注意：区别于 DELETE 命令

相同 : 都能删除数据 , 不删除表结构 , 但 TRUNCATE 速度更快不同 :
使用 TRUNCATE TABLE 重新设置 AUTO_INCREMENT 计数器
使用 TRUNCATE TABLE 不会对事务有影响 （事务后面会说）
测试：

| 1   | -- 创建一个测试表                                                                        |
| --- | ---------------------------------------------------------------------------------------- |
| 2   | CREATE TABLE `test` (                                                                    |
| 3   | `id` INT(4) NOT NULL AUTO_INCREMENT,                                                     |
| 4   | `coll` VARCHAR(20) NOT NULL,                                                             |
| 5   | PRIMARY KEY (`id`)                                                                       |
| 6   | ) ENGINE=INNODB DEFAULT CHARSET=utf8                                                     |
| 7   |                                                                                          |
| 8   | -- 插入几个测试数据                                                                      |
| 9   | INSERT INTO test(coll) VALUES('row1'),('row2'),('row3');                                 |
| 10  |                                                                                          |
| 11  | -- 删除表数据(不带 where 条件的 delete)                                                  |
| 12  | DELETE FROM test;                                                                        |
| 13  | -- 结论:如不指定 Where 则删除该表的所有列数据,自增当前值依然从原来基础上进行,会记录日志. |
| 14  |                                                                                          |
| 15  | -- 删除表数据(truncate)                                                                  |
| 16  | TRUNCATE TABLE test;                                                                     |
| 17  | -- 结论:truncate 删除数据,自增当前值会恢复到初始值重新开始;不会记录日志.                 |
| 18  |                                                                                          |
| 19  | -- 同样使用 DELETE 清空不同引擎的数据库表数据.重启数据库服务后                           |
| 20  | -- InnoDB : 自增列从初始值重新开始 (因为是存储在内存中,断电即失)                         |
| 21  | -- MyISAM : 自增列依然从上一个自增数据基础上开始 (存在文件中,不会丢失)                   |

# 4、使用 DQL 查询数据

## 、DQL 语言

### DQL( Data Query Language 数据查询语言 )

查询数据库数据 , 如**SELECT**语句
简单的单表查询或多表的复杂查询和嵌套查询是数据库语言中最核心,最重要的语句
使用频率最高的语句

SELECT 语法

SELECT [ALL | DISTINCT]
{_ | table._ | [table.field1[as alias1],table.field2[as alias2]][,...]]} FROM table_name [as table_alias]
[left | right | inner join table_name2] -- 联合查询
[WHERE ...] -- 指定结果需满足的条件
[GROUP BY ...] -- 指定结果按照哪几个字段来分组[HAVING] -- 过滤分组的记录必须满足的次要条件[ORDER BY ...] -- 指定查询记录按一个或多个条件排序
[LIMIT {[offset,]row_count | row_countOFFSET offset}];
-- 指定查询的记录从哪条至哪条
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

### 注意 : [ ] 括号代表可选的 , { }括号代表必选得

导入素材提供的 SQL

## 、指定查询字段

| 1   | -- 查询表中所有的数据列结果 , 采用 \*\*" | \* "\*\* 符号; | 但是效率低，不推荐 | .   |
| --- | ---------------------------------------- | -------------- | ------------------ | --- |
| 2   |                                          |                |                    |     |
| 3   | -- 查询所有学生信息                      |                |                    |     |
| 4   | SELECT \* FROM student;                  |                |                    |     |
| 5   |                                          |                |                    |     |
| 6   | -- 查询指定列(学号 , 姓名)               |                |                    |     |
| 7   | SELECT studentno,studentname FROM        | student;       |                    |     |

AS 子句作为别名
作用：
可给数据列取一个新别名可给表取一个新别名
可把经计算或总结的结果用另一个新名称来代替

| 1   | -- 这里是为列取别名(当然 as 关键词可以省略)  |      |            |     |
| --- | -------------------------------------------- | ---- | ---------- | --- |
| 2   | SELECT studentno AS 学号,studentname AS 姓名 | FROM | student;   |     |
| 3   |                                              |      |            |     |
| 4   | -- 使用 as 也可以为表取别名                  |      |            |     |
| 5   | SELECT studentno AS 学号,studentname AS 姓名 | FROM | student AS | s;  |
| 6   |                                              |      |            |     |
| 7   | -- 使用 as,为查询结果取一个新名字            |      |            |     |
| 8   | -- CONCAT()函数拼接字符串                    |      |            |     |
| 9   | SELECT CONCAT('姓名:',studentname) AS 新姓名 | FROM | student;   |     |

DISTINCT 关键字的使用
作用 : 去掉 SELECT 查询返回的记录结果中重复的记录 ( 返回所有列的值都相同 ) , 只返回一条

| 1   | -- # 查看哪些同学参加了考试(学号) 去除重复项            |            |     |              |
| --- | ------------------------------------------------------- | ---------- | --- | ------------ |
| 2   | SELECT \* FROM result; -- 查看考试成绩                  |            |     |              |
| 3   | SELECT studentno FROM result; -- 查看哪些同学参加了考试 |            |     |              |
| 4   | SELECT DISTINCT studentno FROM result; -- 了解:DISTINCT | 去除重复项 | ,   | (默认是 ALL) |

使用表达式的列
数据库中的表达式 : 一般由文本值 , 列值 , NULL , 函数和操作符等组成应用场景 :
SELECT 语句返回结果列中使用
SELECT 语句中的 ORDER BY , HAVING 等子句中使用 DML 语句中的 where 条件语句中使用表达式

| 1   | -- selcet 查询中可以使用表达式                     |         |
| --- | -------------------------------------------------- | ------- |
| 2   | SELECT @@auto_increment_increment; -- 查询自增步长 |         |
| 3   | SELECT VERSION(); -- 查询版本号                    |         |
| 4   | SELECT 100\*3-1 AS 计算结果; -- 表达式             |         |
| 5   |                                                    |         |
| 6   | -- 学员考试成绩集体提分一分查看                    |         |
| 7   | SELECT studentno,StudentResult+1 AS '提分后' FROM  | result; |

避免 SQL 返回结果中包含 ' . ' , ' \* ' 和括号等干扰开发语言程序.

## 、where 条件语句

作用：用于检索数据表中 符合条件 的记录
搜索条件可由一个或多个逻辑表达式组成 , 结果一般为真或假.

逻辑操作符

| **操作符名称** | **语法**          | **描述**                           |
| -------------- | ----------------- | ---------------------------------- | ----------- | --- | --- | -------------------------------- |
| AND 或 &&      | a AND b 或 a && b | 逻辑与，同时为真结果才为真         |
| OR 或          |                   |                                    | a OR b 或 a |     | b   | 逻辑或，只要一个为真，则结果为真 |
| NOT 或 ！      | NOT a 或 ！a      | 逻辑非，若操作数为假，则结果为真！ |

测试

| 1   | -- 满足条件的查询(where)                        |
| --- | ----------------------------------------------- |
| 2   | SELECT Studentno,StudentResult FROM result;     |
| 3   |                                                 |
| 4   | -- 查询考试成绩在 95-100 之间的                 |
| 5   | SELECT Studentno,StudentResult                  |
| 6   | FROM result                                     |
| 7   | WHERE StudentResult>=95 AND StudentResult<=100; |
| 8   |                                                 |
| 9   | -- AND 也可以写成 &&                            |
| 10  | SELECT Studentno,StudentResult                  |
| 11  | FROM result                                     |
| 12  | WHERE StudentResult>=95 && StudentResult<=100;  |
| 13  |                                                 |
| 14  | -- 模糊查询(对应的词:精确查询)                  |

| 15  | SELECT Studentno,StudentResult          |
| --- | --------------------------------------- |
| 16  | FROM result                             |
| 17  | WHERE StudentResult BETWEEN 95 AND 100; |
| 18  |                                         |
| 19  | -- 除了 1000 号同学,要其他同学的成绩    |
| 20  | SELECT studentno,studentresult          |
| 21  | FROM result                             |
| 22  | WHERE studentno!=1000;                  |
| 23  |                                         |
| 24  | -- 使用 NOT                             |
| 25  | SELECT studentno,studentresult          |
| 26  | FROM result                             |
| 27  | WHERE NOT studentno=1000;               |

模糊查询 ： 比较操作符

| **操作符名称** | **语法**             | **描述**                                |
| -------------- | -------------------- | --------------------------------------- |
| IS NULL        | a IS NULL            | 若操作符为 NULL，则结果为真             |
| IS NOT NULL    | a IS NOT NULL        | 若操作符不为 NULL，则结果为真           |
| BETWEEN        | a BETWEEN b AND c    | 若 a 范围在 b 与 c 之间，则结果为真     |
| LIKE           | a LIKE b             | SQL 模式匹配，若 a 匹配 b，则结果为真   |
| IN             | a IN (a1，a2，a3， ) | 若 a 等于 a1,a2. 中的某一个，则结果为真 |

注意：
数值数据类型的记录之间才能进行算术运算 ;
相同数据类型的数据之间才能进行比较 ;

测试：

| 1   | -- 模糊查询 between and \ like \ in \ null                       |
| --- | ---------------------------------------------------------------- |
| 2   |                                                                  |
| 3   | -- =============================================                 |
| 4   | -- LIKE                                                          |
| 5   | -- =============================================                 |
| 6   | -- 查询姓刘的同学的学号及姓名                                    |
| 7   | -- like 结合使用的通配符 : % (代表 0 到任意个字符) \_ (一个字符) |
| 8   | SELECT studentno,studentname FROM student                        |
| 9   | WHERE studentname LIKE '刘%';                                    |
| 10  |                                                                  |
| 11  | -- 查询姓刘的同学,后面只有一个字的                               |
| 12  | SELECT studentno,studentname FROM student                        |
| 13  | WHERE studentname LIKE '刘\_';                                   |
| 14  |                                                                  |
| 15  | -- 查询姓刘的同学,后面只有两个字的                               |
| 16  | SELECT studentno,studentname FROM student                        |
| 17  | WHERE studentname LIKE ' 刘 ';                                   |
| 18  |                                                                  |
| 19  | -- 查询姓名中含有 嘉 字的                                        |

| 20  | SELECT studentno,studentname FROM student         |
| --- | ------------------------------------------------- |
| 21  | WHERE studentname LIKE '%嘉%';                    |
| 22  |                                                   |
| 23  | -- 查询姓名中含有特殊字符的需要使用转义符号 '\'   |
| 24  | -- 自定义转义符关键字: ESCAPE ':'                 |
| 25  |                                                   |
| 26  | -- =============================================  |
| 27  | -- IN                                             |
| 28  | -- =============================================  |
| 29  | -- 查询学号为 1000,1001,1002 的学生姓名           |
| 30  | SELECT studentno,studentname FROM student         |
| 31  | WHERE studentno IN (1000,1001,1002);              |
| 32  |                                                   |
| 33  | -- 查询地址在北京,南京,河南洛阳的学生             |
| 34  | SELECT studentno,studentname,address FROM student |
| 35  | WHERE address IN ('北京','南京','河南洛阳');      |
| 36  |                                                   |
| 37  | -- =============================================  |
| 38  | -- NULL 空                                        |
| 39  | -- =============================================  |
| 40  | -- 查询出生日期没有填写的同学                     |
| 41  | -- 不能直接写=NULL , 这是代表错误的 , 用 is null  |
| 42  | SELECT studentname FROM student                   |
| 43  | WHERE BornDate IS NULL;                           |
| 44  |                                                   |
| 45  | -- 查询出生日期填写的同学                         |
| 46  | SELECT studentname FROM student                   |
| 47  | WHERE BornDate IS NOT NULL;                       |
| 48  |                                                   |
| 49  | -- 查询没有写家庭住址的同学(空字符串不等于 null)  |
| 50  | SELECT studentname FROM student                   |
| 51  | WHERE Address='' OR Address IS NULL;              |

## 、连接查询

JOIN 对比

| **操作符名称** | **描述**                                   |
| -------------- | ------------------------------------------ |
| INNER JOIN     | 如果表中有至少一个匹配，则返回行           |
| LEFT JOIN      | 即使右表中没有匹配，也从左表中返回所有的行 |
| RIGHT JOIN     | 即使左表中没有匹配，也从右表中返回所有的行 |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834333576-a2eba5cc-10fd-44d3-a378-19c08a5a8cf5.jpeg#)

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834334215-a8f27a1b-a4b7-43bd-9ae7-496cf4afc9a4.jpeg#)

测试

| 1   | /\*                                                                          |
| --- | ---------------------------------------------------------------------------- |
| 2   | 连接查询                                                                     |
| 3   | 如需要多张数据表的数据进行查询,则可通过连接运算符实现多个查询                |
| 4   | 内连接 inner join                                                            |
| 5   | 查询两个表中的结果集中的交集                                                 |
| 6   | 外连接 outer join                                                            |
| 7   | 左外连接 left join                                                           |
| 8   | (以左表作为基准,右边表来一一匹配,匹配不上的,返回左表的记录,右表以 NULL 填充) |
| 9   | 右外连接 right join                                                          |
| 10  | (以右表作为基准,左边表来一一匹配,匹配不上的,返回右表的记录,左表以 NULL 填充) |
| 11  |                                                                              |
| 12  | 等值连接和非等值连接                                                         |
| 13  |                                                                              |
| 14  | 自连接                                                                       |
| 15  | \*/                                                                          |
| 16  |                                                                              |
| 17  | -- 查询参加了考试的同学信息(学号,学生姓名,科目编号,分数)                     |
| 18  | SELECT \* FROM student;                                                      |
| 19  | SELECT \* FROM result;                                                       |
| 20  |                                                                              |
| 21  | /\*思路:                                                                     |
| 22  | (1):分析需求,确定查询的列来源于两个类,student result,连接查询                |
| 23  | (2):确定使用哪种连接查询?(内连接)                                            |
| 24  | \*/                                                                          |
| 25  | SELECT s.studentno,studentname,subjectno,StudentResult                       |
| 26  | FROM student s                                                               |
| 27  | INNER JOIN result r                                                          |
| 28  | ON r.studentno = s.studentno                                                 |

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
-- 右连接(也可实现)
SELECT s.studentno,studentname,subjectno,StudentResult FROM student s
RIGHT JOIN result r
ON r.studentno = s.studentno
-- 等值连接
SELECT s.studentno,studentname,subjectno,StudentResult FROM student s , result r
WHERE r.studentno = s.studentno

-- 左连接 (查询了所有同学,不考试的也会查出来)
SELECT s.studentno,studentname,subjectno,StudentResult FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno

-- 查一下缺考的同学(左连接应用场景)
SELECT s.studentno,studentname,subjectno,StudentResult FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno WHERE StudentResult IS NULL

-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数) SELECT s.studentno,studentname,subjectname,StudentResult FROM student s
INNER JOIN result r
ON r.studentno = s.studentno INNER JOIN `subject` sub
ON sub.subjectno = r.subjectno

自连接

| 1   | /\*                                                                      |
| --- | ------------------------------------------------------------------------ |
| 2   | 自连接                                                                   |
| 3   | 数据表与自身进行连接                                                     |
| 4   |                                                                          |
| 5   | 需求:从一个包含栏目 ID , 栏目名称和父栏目 ID 的表中                      |
| 6   | 查询父栏目名称和其他子栏目名称                                           |
| 7   | \*/                                                                      |
| 8   |                                                                          |
| 9   | -- 创建一个表                                                            |
| 10  | CREATE TABLE `category` (                                                |
| 11  | `categoryid` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主题 id', |
| 12  | `pid` INT(10) NOT NULL COMMENT '父 id',                                  |
| 13  | `categoryName` VARCHAR(50) NOT NULL COMMENT '主题名字',                  |
| 14  | PRIMARY KEY (`categoryid`)                                               |
| 15  | ) ENGINE=INNODB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8                    |
| 16  |                                                                          |
| 17  | -- 插入数据                                                              |
| 18  | INSERT INTO `category` (`categoryid`, `pid`, `categoryName`)             |
| 19  | VALUES('2','1','信息技术'),                                              |
| 20  | ('3','1','软件开发'),                                                    |

| 21  | ('4','3','数据库'),                                                     |
| --- | ----------------------------------------------------------------------- |
| 22  | ('5','1','美术设计'),                                                   |
| 23  | ('6','3','web 开发'),                                                   |
| 24  | ('7','5','ps 技术'),                                                    |
| 25  | ('8','2','办公信息');                                                   |
| 26  |                                                                         |
| 27  | -- 编写 SQL 语句,将栏目的父子关系呈现出来 (父栏目名称,子栏目名称)       |
| 28  | -- 核心思想:把一张表看成两张一模一样的表,然后将这两张表连接查询(自连接) |
| 29  | SELECT a.categoryName AS '父栏目',b.categoryName AS '子栏目'            |
| 30  | FROM category AS a,category AS b                                        |
| 31  | WHERE a.`categoryid`=b.`pid`                                            |
| 32  |                                                                         |
| 33  | -- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数)           |
| 34  | SELECT s.studentno,studentname,subjectname,StudentResult                |
| 35  | FROM student s                                                          |
| 36  | INNER JOIN result r                                                     |
| 37  | ON r.studentno = s.studentno                                            |
| 38  | INNER JOIN `subject` sub                                                |
| 39  | ON sub.subjectno = r.subjectno                                          |
| 40  |                                                                         |
| 41  | -- 查询学员及所属的年级(学号,学生姓名,年级名)                           |
| 42  | SELECT studentno AS 学号,studentname AS 学生姓名,gradename AS 年级名称  |
| 43  | FROM student s                                                          |
| 44  | INNER JOIN grade g                                                      |
| 45  | ON s.`GradeId` = g.`GradeID`                                            |
| 46  |                                                                         |
| 47  | -- 查询科目及所属的年级(科目名称,年级名称)                              |
| 48  | SELECT subjectname AS 科目名称,gradename AS 年级名称                    |
| 49  | FROM SUBJECT sub                                                        |
| 50  | INNER JOIN grade g                                                      |
| 51  | ON sub.gradeid = g.gradeid                                              |
| 52  |                                                                         |
| 53  | -- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)        |
| 54  | SELECT s.studentno,studentname,subjectname,StudentResult                |
| 55  | FROM student s                                                          |
| 56  | INNER JOIN result r                                                     |
| 57  | ON r.studentno = s.studentno                                            |
| 58  | INNER JOIN `subject` sub                                                |
| 59  | ON r.subjectno = sub.subjectno                                          |
| 60  | WHERE subjectname='数据库结构-1'                                        |

## 、排序和分页

测试

| 1   | /\*============== 排序 ================                          |
| --- | ---------------------------------------------------------------- |
| 2   | 语法 : ORDER BY                                                  |
| 3   | ORDER BY 语句用于根据指定的列对结果集进行排序。                  |
| 4   | ORDER BY 语句默认按照 ASC 升序对记录进行排序。                   |
| 5   | 如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。         |
| 6   |                                                                  |
| 7   | \*/                                                              |
| 8   |                                                                  |
| 9   | -- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩) |
| 10  | -- 按成绩降序排序                                                |

| 11  | SELECT s.studentno,studentname,subjectname,StudentResult                                |
| --- | --------------------------------------------------------------------------------------- | ------------------ |
| 12  | FROM student s                                                                          |
| 13  | INNER JOIN result r                                                                     |
| 14  | ON r.studentno = s.studentno                                                            |
| 15  | INNER JOIN `subject` sub                                                                |
| 16  | ON r.subjectno = sub.subjectno                                                          |
| 17  | WHERE subjectname='数据库结构-1'                                                        |
| 18  | ORDER BY StudentResult DESC                                                             |
| 19  |                                                                                         |
| 20  | /\*============== 分页 ================                                                 |
| 21  | 语法 : SELECT \* FROM table LIMIT [offset,] rows                                        | rows OFFSET offset |
| 22  | 好处 : (用户体验,网络传输,查询压力)                                                     |
| 23  |                                                                                         |
| 24  | 推导:                                                                                   |
| 25  | 第一页 : limit 0,5                                                                      |
| 26  | 第二页 : limit 5,5                                                                      |
| 27  | 第三页 : limit 10,5                                                                     |
| 28  | ......                                                                                  |
| 29  | 第 N 页 : limit (pageNo-1)\*pageSzie,pageSzie                                           |
| 30  | [pageNo:页码,pageSize:单页面显示条数]                                                   |
| 31  |                                                                                         |
| 32  | \*/                                                                                     |
| 33  |                                                                                         |
| 34  | -- 每页显示 5 条数据                                                                    |
| 35  | SELECT s.studentno,studentname,subjectname,StudentResult                                |
| 36  | FROM student s                                                                          |
| 37  | INNER JOIN result r                                                                     |
| 38  | ON r.studentno = s.studentno                                                            |
| 39  | INNER JOIN `subject` sub                                                                |
| 40  | ON r.subjectno = sub.subjectno                                                          |
| 41  | WHERE subjectname='数据库结构-1'                                                        |
| 42  | ORDER BY StudentResult DESC , studentno                                                 |
| 43  | LIMIT 0,5                                                                               |
| 44  |                                                                                         |
| 45  | -- 查询 JAVA 第一学年 课程成绩前 10 名并且分数大于 80 的学生信息(学号,姓名,课程名,分数) |
| 46  | SELECT s.studentno,studentname,subjectname,StudentResult                                |
| 47  | FROM student s                                                                          |
| 48  | INNER JOIN result r                                                                     |
| 49  | ON r.studentno = s.studentno                                                            |
| 50  | INNER JOIN `subject` sub                                                                |
| 51  | ON r.subjectno = sub.subjectno                                                          |
| 52  | WHERE subjectname='JAVA 第一学年'                                                       |
| 53  | ORDER BY StudentResult DESC                                                             |
| 54  | LIMIT 0,10                                                                              |

1.  **、子查询**

| 1   | /\*============== 子查询 ================                                |
| --- | ------------------------------------------------------------------------ |
| 2   | 什么是子查询?                                                            |
| 3   | 在查询语句中的 WHERE 条件子句中,又嵌套了另一个查询语句                   |
| 4   | 嵌套查询可由多个子查询组成,求解的方式是由里及外;                         |
| 5   | 子查询返回的结果一般都是集合,故而建议使用 IN 关键字;                     |
| 6   | \*/                                                                      |
| 7   |                                                                          |
| 8   | -- 查询 数据库结构-1 的所有考试结果(学号,科目编号,成绩),并且成绩降序排列 |

| 9   | -- 方法一:使用连接查询                                               |     |
| --- | -------------------------------------------------------------------- | --- |
| 10  | SELECT studentno,r.subjectno,StudentResult                           |     |
| 11  | FROM result r                                                        |     |
| 12  | INNER JOIN `subject` sub                                             |     |
| 13  | ON r.`SubjectNo`=sub.`SubjectNo`                                     |     |
| 14  | WHERE subjectname = '数据库结构-1'                                   |     |
| 15  | ORDER BY studentresult DESC;                                         |     |
| 16  |                                                                      |     |
| 17  | -- 方法二:使用子查询(执行顺序:由里及外)                              |     |
| 18  | SELECT studentno,subjectno,StudentResult                             |     |
| 19  | FROM result                                                          |     |
| 20  | WHERE subjectno=(                                                    |     |
| 21  | SELECT subjectno FROM `subject`                                      |     |
| 22  | WHERE subjectname = '数据库结构-1'                                   |     |
| 23  | )                                                                    |     |
| 24  | ORDER BY studentresult DESC;                                         |     |
| 25  |                                                                      |     |
| 26  | -- 查询课程为 高等数学-2 且分数不小于 80 分的学生的学号和姓名        |     |
| 27  | -- 方法一:使用连接查询                                               |     |
| 28  | SELECT s.studentno,studentname                                       |     |
| 29  | FROM student s                                                       |     |
| 30  | INNER JOIN result r                                                  |     |
| 31  | ON s.`StudentNo` = r.`StudentNo`                                     |     |
| 32  | INNER JOIN `subject` sub                                             |     |
| 33  | ON sub.`SubjectNo` = r.`SubjectNo`                                   |     |
| 34  | WHERE subjectname = '高等数学-2' AND StudentResult>=80               |     |
| 35  |                                                                      |     |
| 36  | -- 方法二:使用连接查询+子查询                                        |     |
| 37  | -- 分数不小于 80 分的学生的学号和姓名                                |     |
| 38  | SELECT r.studentno,studentname FROM student s                        |     |
| 39  | INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`                   |     |
| 40  | WHERE StudentResult>=80                                              |     |
| 41  |                                                                      |     |
| 42  | -- 在上面 SQL 基础上,添加需求:课程为 高等数学-2                      |     |
| 43  | SELECT r.studentno,studentname FROM student s                        |     |
| 44  | INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`                   |     |
| 45  | WHERE StudentResult>=80 AND subjectno=(                              |     |
| 46  | SELECT subjectno FROM `subject`                                      |     |
| 47  | WHERE subjectname = '高等数学-2'                                     |     |
| 48  | )                                                                    |     |
| 49  |                                                                      |     |
| 50  | -- 方法三:使用子查询                                                 |     |
| 51  | -- 分步写简单 sql 语句,然后将其嵌套起来                              |     |
| 52  | SELECT studentno,studentname FROM student WHERE studentno            | IN( |
| 53  | SELECT studentno FROM result WHERE StudentResult>=80 AND subjectno=( |     |
| 54  | SELECT subjectno FROM `subject` WHERE subjectname = '高等数学-2'     |     |
| 55  | )                                                                    |     |
| 56  | )                                                                    |     |
| 57  |                                                                      |     |
| 58  | /\*                                                                  |     |
| 59  | 练习题目:                                                            |     |
| 60  | 查 C 语言-1 的前 5 名学生的成绩信息(学号,姓名,分数)                  |     |
| 61  | 使用子查询,查询郭靖同学所在的年级名称                                |     |
| 62  | \*/                                                                  |     |

# 5、MySQL 函数

官方文档 : [官方文档](https://dev.mysql.com/doc/refman/5.7/en/func-op-summary-ref.html)

## 、常用函数

### 数据函数

| 1   | SELECT | ABS(-8); /_绝对值_/                                   |
| --- | ------ | ----------------------------------------------------- |
| 2   | SELECT | CEILING(9.4); /_向上取整_/                            |
| 3   | SELECT | FLOOR(9.4); /_向下取整_/                              |
| 4   | SELECT | RAND(); /_随机数,返回一个 0-1 之间的随机数_/          |
| 5   | SELECT | SIGN(0); /_符号函数: 负数返回-1,正数返回 1,0 返回 0_/ |

**字符串函数**

| 1   | SELECT CHAR_LENGTH('狂神说坚持就能成功'); /_返回字符串包含的字符数_/                |
| --- | ----------------------------------------------------------------------------------- |
| 2   | SELECT CONCAT('我','爱','程序'); /_合并字符串,参数可以有多个_/                      |
| 3   | SELECT INSERT('我爱编程 helloworld',1,2,'超级热爱'); /\*替换字符串,从某个位置开始替 |
|     | 换某个长度\*/                                                                       |
| 4   | SELECT LOWER('KuangShen'); /_小写_/                                                 |
| 5   | SELECT UPPER('KuangShen'); /_大写_/                                                 |
| 6   | SELECT LEFT('hello,world',5); /_从左边截取_/                                        |
| 7   | SELECT RIGHT('hello,world',5); /_从右边截取_/                                       |
| 8   | SELECT REPLACE('狂神说坚持就能成功','坚持','努力'); /_替换字符串_/                  |
| 9   | SELECT SUBSTR('狂神说坚持就能成功',4,6); /_截取字符串,开始和长度_/                  |
| 10  | SELECT REVERSE('狂神说坚持就能成功'); /\*反转                                       |
| 11  |                                                                                     |
| 12  | -- 查询姓周的同学,改成邹                                                            |
| 13  | SELECT REPLACE(studentname,'周','邹') AS 新名字                                     |
| 14  | FROM student WHERE studentname LIKE '周%';                                          |

### 日期和时间函数

| 1   | SELECT CURRENT_DATE(); /_获取当前日期_/    |
| --- | ------------------------------------------ |
| 2   | SELECT CURDATE(); /_获取当前日期_/         |
| 3   | SELECT NOW(); /_获取当前日期和时间_/       |
| 4   | SELECT LOCALTIME(); /_获取当前日期和时间_/ |
| 5   | SELECT SYSDATE(); /_获取当前日期和时间_/   |
| 6   |                                            |
| 7   | -- 获取年月日,时分秒                       |
| 8   | SELECT YEAR(NOW());                        |
| 9   | SELECT MONTH(NOW());                       |
| 10  | SELECT DAY(NOW());                         |
| 11  | SELECT HOUR(NOW());                        |
| 12  | SELECT MINUTE(NOW());                      |
| 13  | SELECT SECOND(NOW());                      |

**系统信息函数**

| 1   | SELECT | VERSION(); | /_版本_/ |
| --- | ------ | ---------- | -------- |
| 2   | SELECT | USER();    | /_用户_/ |
| 3   |        |            |          |
| 4   |        |            |          |

## 、聚合函数

| **函数名称** | **描述**                                                                      |
| ------------ | ----------------------------------------------------------------------------- |
| COUNT()      | 返回满足 Select 条件的记录总和数，如 select count(_) 【不建议使用 _，效率低】 |
| SUM()        | 返回数字字段或表达式列作统计，返回一列的总和。                                |
| AVG()        | 通常为数值字段或表达列作统计，返回一列的平均值                                |
| MAX()        | 可以为数值字段，字符字段或表达式列作统计，返回最大的值。                      |
| MIN()        | 可以为数值字段，字符字段或表达式列作统计，返回最小的值。                      |

| 1   | -- 聚合函数                                                                                |
| --- | ------------------------------------------------------------------------------------------ |
| 2   | /_COUNT:非空的_/                                                                           |
| 3   | SELECT COUNT(studentname) FROM student;                                                    |
| 4   | SELECT COUNT(\*) FROM student;                                                             |
| 5   | SELECT COUNT(1) FROM student; /_推荐_/                                                     |
| 6   |                                                                                            |
| 7   | -- 从含义上讲，count(1) 与 count(\*) 都表示对全部数据行的查询。                            |
| 8   | -- count(字段) 会统计该字段在表中出现的次数，忽略字段为 null 的情况。即不统计字段为 null   |
|     | 的记录。                                                                                   |
| 9   | -- count(\*) 包括了所有的列，相当于行数，在统计结果的时候，包含字段为 null 的记录；        |
| 10  | -- count(1) 用 1 代表代码行，在统计结果的时候，包含字段为 null 的记录 。                   |
| 11  | /\*                                                                                        |
| 12  | 很多人认为 count(1)执行的效率会比 count(_)高，原因是 count(_)会存在全表扫描，而 count(1)   |
|     | 可以针对一个字段进行查询。其实不然，count(1)和 count(\*)都会对全表进行扫描，统计所有记录的 |
|     | 条数，包括那些为 null 的记录，因此，它们的效率可以说是相差无几。而 count(字段)则与前两者不 |
|     | 同，它会统计该字段不为 null 的记录条数。                                                   |
| 13  |                                                                                            |
| 14  | 下面它们之间的一些对比：                                                                   |
| 15  |                                                                                            |
| 16  | 1）在表没有主键时，count(1)比 count(\*)快                                                  |
| 17  | 2）有主键时，主键作为计算条件，count(主键)效率最高；                                       |
| 18  | 3）若表格只有一个字段，则 count(\*)效率较高。                                              |
| 19  | \*/                                                                                        |
| 20  |                                                                                            |
| 21  | SELECT SUM(StudentResult) AS 总和 FROM result;                                             |
| 22  | SELECT AVG(StudentResult) AS 平均分 FROM result;                                           |
| 23  | SELECT MAX(StudentResult) AS 最高分 FROM result;                                           |
| 24  | SELECT MIN(StudentResult) AS 最低分 FROM result;                                           |

### 题目：

| 1   | -- 查询不同课程的平均分,最高分,最低分                                      |
| --- | -------------------------------------------------------------------------- |
| 2   | -- 前提:根据不同的课程进行分组                                             |
| 3   |                                                                            |
| 4   | SELECT subjectname,AVG(studentresult) AS 平均分,MAX(StudentResult) AS 最高 |
|     | 分,MIN(StudentResult) AS 最低分                                            |
| 5   | FROM result AS r                                                           |
| 6   | INNER JOIN `subject` AS s                                                  |
| 7   | ON r.subjectno = s.subjectno                                               |
| 8   | GROUP BY r.subjectno                                                       |
| 9   | HAVING 平均分>80;                                                          |
| 10  |                                                                            |
| 11  | /\*                                                                        |

| 12  | where 写在 group by 前面.                                                          |
| --- | ---------------------------------------------------------------------------------- |
| 13  | 要是放在分组后面的筛选                                                             |
| 14  | 要使用 HAVING..                                                                    |
| 15  | 因为 having 是从前面筛选的字段再筛选，而 where 是从数据表中的>字段直接进行的筛选的 |
| 16  | \*/                                                                                |

MD5 加密
**一、MD5 简介**
MD5 即 Message-Digest Algorithm 5（信息-摘要算法 5），用于确保信息传输完整一致。是计算机广泛使用的杂凑算法之一（又译摘要算法、哈希算法），主流编程语言普遍已有 MD5 实现。将数据（如汉 字）运算为另一固定长度值，是杂凑算法的基础原理，MD5 的前身有 MD2、MD3 和 MD4。
百度搜索 md5 介绍

### 二、实现数据加密

新建一个表 testmd5

| 1   | CREATE TABLE `testmd5` (             |
| --- | ------------------------------------ |
| 2   | `id` INT(4) NOT NULL,                |
| 3   | `name` VARCHAR(20) NOT NULL,         |
| 4   | `pwd` VARCHAR(50) NOT NULL,          |
| 5   | PRIMARY KEY (`id`)                   |
| 6   | ) ENGINE=INNODB DEFAULT CHARSET=utf8 |

插入一些数据

| 1   | INSERT | INTO | testmd5 | VALUES(1,'kuangshen','123456'),(2,'qinjiang','456789') |
| --- | ------ | ---- | ------- | ------------------------------------------------------ |

如果我们要对 pwd 这一列数据进行加密，语法是：

| 1   | update | testmd5 | set | pwd | =   | md5(pwd); |
| --- | ------ | ------- | --- | --- | --- | --------- |

如果单独对某个用户(如 kuangshen)的密码加密：

| 1   | INSERT | INTO testmd5 | VALUES(3,'kuangshen2','123456')           |
| --- | ------ | ------------ | ----------------------------------------- |
| 2   | update | testmd5 set  | pwd = md5(pwd) where name = 'kuangshen2'; |

插入新的数据自动加密

| 1   | INSERT | INTO | testmd5 | VALUES(4,'kuangshen3',md5('123456')); |
| --- | ------ | ---- | ------- | ------------------------------------- |

查询登录用户信息（md5 对比使用，查看用户输入加密后的密码进行比对）

| 1   | SELECT | \*  | FROM | testmd5 | WHERE | `name`='kuangshen' | AND | pwd=MD5('123456'); |
| --- | ------ | --- | ---- | ------- | ----- | ------------------ | --- | ------------------ |

1.  **、小结**

| 1
2 | -- | ================ | 内置函数 | ================ |
| --- | --- | --- | --- | --- |

| 3   | -- 数值函数                                                                      |
| --- | -------------------------------------------------------------------------------- |
| 4   | abs(x) -- 绝对值 abs(-10.9) = 10                                                 |
| 5   | format(x, d) -- 格式化千分位数值 format(1234567.456, 2) = 1,234,567.46           |
| 6   | ceil(x) -- 向上取整 ceil(10.1) = 11                                              |
| 7   | floor(x) -- 向下取整 floor (10.1) = 10                                           |
| 8   | round(x) -- 四舍五入去整                                                         |
| 9   | mod(m, n) -- m%n m mod n 求余 10%3=1                                             |
| 10  | pi() -- 获得圆周率                                                               |
| 11  | pow(m, n) -- m^n                                                                 |
| 12  | sqrt(x) -- 算术平方根                                                            |
| 13  | rand() -- 随机数                                                                 |
| 14  | truncate(x, d) -- 截取 d 位小数                                                  |
| 15  |                                                                                  |
| 16  | -- 时间日期函数                                                                  |
| 17  | now(), current_timestamp(); -- 当前日期时间                                      |
| 18  | current_date(); -- 当前日期                                                      |
| 19  | current_time(); -- 当前时间                                                      |
| 20  | date('yyyy-mm-dd hh:ii:ss'); -- 获取日期部分                                     |
| 21  | time('yyyy-mm-dd hh:ii:ss'); -- 获取时间部分                                     |
| 22  | date_format('yyyy-mm-dd hh:ii:ss', '%d %y %a %d %m %b %j'); -- 格式化时间        |
| 23  | unix_timestamp(); -- 获得 unix 时间戳                                            |
| 24  | from_unixtime(); -- 从时间戳获得时间                                             |
| 25  |                                                                                  |
| 26  | -- 字符串函数                                                                    |
| 27  | length(string) -- string 长度，字节                                              |
| 28  | char_length(string) -- string 的字符个数                                         |
| 29  | substring(str, position [,length]) -- 从 str 的 position 开始,取 length 个字符   |
| 30  | replace(str ,search_str ,replace_str) -- 在 str 中用 replace_str 替换 search_str |
| 31  | instr(string ,substring) -- 返回 substring 首次在 string 中出现的位置            |
| 32  | concat(string [,...]) -- 连接字串                                                |
| 33  | charset(str) -- 返回字串字符集                                                   |
| 34  | lcase(string) -- 转换成小写                                                      |
| 35  | left(string, length) -- 从 string2 中的左边起取 length 个字符                    |
| 36  | load_file(file_name) -- 从文件读取内容                                           |
| 37  | locate(substring, string [,start_position]) -- 同 instr,但可指定开始位置         |
| 38  | lpad(string, length, pad) -- 重复用 pad 加在 string 开头,直到字串长度为 length   |
| 39  | ltrim(string) -- 去除前端空格                                                    |
| 40  | repeat(string, count) -- 重复 count 次                                           |
| 41  | rpad(string, length, pad) --在 str 后用 pad 补充,直到长度为 length               |
| 42  | rtrim(string) -- 去除后端空格                                                    |
| 43  | strcmp(string1 ,string2) -- 逐字符比较两字串大小                                 |
| 44  |                                                                                  |
| 45  | -- 聚合函数                                                                      |
| 46  | count()                                                                          |
| 47  | sum();                                                                           |
| 48  | max();                                                                           |
| 49  | min();                                                                           |
| 50  | avg();                                                                           |
| 51  | group_concat()                                                                   |
| 52  |                                                                                  |
| 53  | -- 其他常用函数                                                                  |
| 54  | md5();                                                                           |
| 55  | default();                                                                       |

# 6、事务

## 、概述

什么是事务
事务就是将一组 SQL 语句放在同一批次内去执行
如果一个 SQL 语句出错,则该批次内的所有 SQL 都将被取消执行
MySQL 事务处理只支持 InnoDB 和 BDB 数据表类型
事务的 ACID 原则 百度 ACID

### 原子性(Atomic)

整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执 行过程中发生错误，会被回滚（ROLLBACK）到事务开始前的状态，就像这个事务从来没有执行过一样。

### 一致性(Consist)

一个事务可以封装状态改变（除非它是一个只读的）。事务必须始终保持系统处于一致的状态，不 管在任何给定的时间并发事务有多少。也就是说：如果事务是并发多个，系统也必须如同串行事务 一样操作。其主要特征是保护性和不变性(Preserving an Invariant)，以转账案例为例，假设有五个账户，每个账户余额是 100 元，那么五个账户总额是 500 元，如果在这个 5 个账户之间同时发生多个转账，无论并发多少个，比如在 A 与 B 账户之间转账 5 元，在 C 与 D 账户之间转账 10 元，在 B 与 E 之间转账 15 元，五个账户总额也应该还是 500 元，这就是保护性和不变性。

### 隔离性(Isolated)

隔离状态执行事务，使它们好像是系统在给定时间内执行的唯一操作。如果有两个事务，运行在相 同的时间内，执行相同的功能，事务的隔离性将确保每一事务在系统中认为只有该事务在使用系 统。这种属性有时称为串行化，为了防止事务操作间的混淆，必须串行化或序列化请求，使得在同 一时间仅有一个请求用于同一数据。

### 持久性(Durable)

在事务完成以后，该事务对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。

## 、事务实现

### 基本语法：

| 1   | -- 使用 set 语句来改变自动提交模式 |
| --- | ---------------------------------- |
| 2   | SET autocommit = 0; /_关闭_/       |
| 3   | SET autocommit = 1; /_开启_/       |
| 4   |                                    |
| 5   | -- 注意:                           |
| 6   | --- 1.MySQL 中默认是自动提交       |
| 7   | --- 2.使用事务时应先关闭自动提交   |
| 8   |                                    |
| 9   | -- 开始一个事务,标记事务的起始点   |
| 10  | START TRANSACTION                  |

| 11  |                                                  |
| --- | ------------------------------------------------ |
| 12  | -- 提交一个事务给数据库                          |
| 13  | COMMIT                                           |
| 14  |                                                  |
| 15  | -- 将事务回滚,数据回到本次事务的初始状态         |
| 16  | ROLLBACK                                         |
| 17  |                                                  |
| 18  | -- 还原 MySQL 数据库的自动提交                   |
| 19  | SET autocommit =1;                               |
| 20  |                                                  |
| 21  | -- 保存点                                        |
| 22  | SAVEPOINT 保存点名称 -- 设置一个事务保存点       |
| 23  | ROLLBACK TO SAVEPOINT 保存点名称 -- 回滚到保存点 |
| 24  | RELEASE SAVEPOINT 保存点名称 -- 删除保存点       |

![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834334688-8e9d09ea-b080-4adc-a2bb-2e3dcd0e466e.jpeg#)**事务处理步骤：**

1.  **、测试题目**

| 1   | /\*                                                               |
| --- | ----------------------------------------------------------------- |
| 2   | 课堂测试题目                                                      |
| 3   |                                                                   |
| 4   | A 在线买一款价格为 500 元商品,网上银行转账.                       |
| 5   | A 的银行卡余额为 2000,然后给商家 B 支付 500.                      |
| 6   | 商家 B 一开始的银行卡余额为 10000                                 |
| 7   |                                                                   |
| 8   | 创建数据库 shop 和创建表 account 并插入 2 条数据                  |
| 9   | \*/                                                               |
| 10  |                                                                   |
| 11  | CREATE DATABASE `shop`CHARACTER SET utf8 COLLATE utf8_general_ci; |
| 12  | USE `shop`;                                                       |
| 13  |                                                                   |
| 14  | CREATE TABLE `account` (                                          |
| 15  | `id` INT(11) NOT NULL AUTO_INCREMENT,                             |
| 16  | `name` VARCHAR(32) NOT NULL,                                      |

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
29
30
31
`cash` DECIMAL(9,2) NOT NULL, PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8
INSERT INTO account (`name`,`cash`) VALUES('A',2000.00),('B',10000.00)

-- 转账实现
SET autocommit = 0; -- 关闭自动提交
START TRANSACTION; -- 开始一个事务,标记事务的起始点 UPDATE account SET cash=cash-500 WHERE `name`='A'; UPDATE account SET cash=cash+500 WHERE `name`='B';
COMMIT; -- 提交事务

# rollback;

SET autocommit = 1; -- 恢复自动提交

# 7、索引

## 、索引分类

索引的作用
提高查询速度
确保数据的唯一性
可以加速表和表之间的连接 , 实现表与表之间的参照完整性
使用分组和排序子句进行数据检索时 , 可以显著减少分组和排序的时间全文检索字段进行搜索优化.

分类
主键索引 (Primary Key)
唯一索引 (Unique) 常规索引 (Index) 全文索引 (FullText)

## 、主键索引

主键 : 某一个属性组能唯一标识一条记录特点 :
最常见的索引类型
确保数据记录的唯一性
确定特定数据记录在数据库中的位置

## 、唯一索引

作用 : 避免同一个表中某数据列中的值重复与主键索引的区别
主键索引只能有一个唯一索引可能有多个

| 1   | CREATE TABLE `Grade`(                        |
| --- | -------------------------------------------- |
| 2   | `GradeID` INT(11) AUTO_INCREMENT PRIMARYKEY, |
| 3   | `GradeName` VARCHAR(32) NOT NULL UNIQUE      |
| 4   | -- 或 UNIQUE KEY `GradeID` (`GradeID`)       |
| 5   | )                                            |

## 、常规索引

作用 : 快速定位特定数据注意 :
index 和 key 关键字都可以设置常规索引应加在查询找条件的字段
不宜添加太多常规索引,影响数据的插入,删除和修改操作

1. CREATE TABLE `result`(
1. -- 省略一些代码
1. INDEX/KEY `ind` (`studentNo`,`subjectNo`) -- 创建表时添加

4 )

| 1   | -- 创建后添加                                                  |
| --- | -------------------------------------------------------------- |
| 2   | ALTER TABLE `result` ADD INDEX `ind`(`studentNo`,`subjectNo`); |

## 、全文索引

百度搜索：全文索引
作用 : 快速定位特定数据注意 :
只能用于 MyISAM 类型的数据表
只能用于 CHAR , VARCHAR , TEXT 数据列类型适合大型数据集

| 1   | /\*                                    |
| --- | -------------------------------------- | -------- | ---------------------- | --- |
| 2   | #方法一：创建表时                      |
| 3   | CREATE TABLE 表名 (                    |
| 4   | 字段名 1 数据类型 [完整性约束条件…],   |
| 5   | 字段名 2 数据类型 [完整性约束条件…],   |
| 6   | [UNIQUE                                | FULLTEXT | SPATIAL ] INDEX        | KEY |
| 7   | [索引名] (字段名[(长度)] [ASC          | DESC])   |
| 8   | );                                     |
| 9   |                                        |
| 10  |                                        |
| 11  | #方法二：CREATE 在已存在的表上创建索引 |
| 12  | CREATE [UNIQUE                         | FULLTEXT | SPATIAL ] INDEX 索引名 |

| 13  | ON 表名 (字段名[(长度)] [ASC                                                             | DESC]) ; |
| --- | ---------------------------------------------------------------------------------------- | -------- | --------------- |
| 14  |                                                                                          |
| 15  |                                                                                          |
| 16  | #方法三：ALTER TABLE 在已存在的表上创建索引                                              |
| 17  | ALTER TABLE 表名 ADD [UNIQUE                                                             | FULLTEXT | SPATIAL ] INDEX |
| 18  | 索引名 (字段名[(长度)] [ASC                                                              | DESC]) ; |
| 19  |                                                                                          |
| 20  |                                                                                          |
| 21  | #删除索引：DROP INDEX 索引名 ON 表名字;                                                  |
| 22  | #删除主键索引: ALTER TABLE 表名 DROP PRIMARY KEY;                                        |
| 23  |                                                                                          |
| 24  |                                                                                          |
| 25  | #显示索引信息: SHOW INDEX FROM student;                                                  |
| 26  | \*/                                                                                      |
| 27  |                                                                                          |
| 28  | /_增加全文索引_/                                                                         |
| 29  | ALTER TABLE `school`.`student` ADD FULLTEXT INDEX `studentname`                          |
|     | (`StudentName`);                                                                         |
| 30  |                                                                                          |
| 31  | /_EXPLAIN : 分析 SQL 语句执行性能_/                                                      |
| 32  | EXPLAIN SELECT \* FROM student WHERE studentno='1000';                                   |
| 33  |                                                                                          |
| 34  | /_使用全文索引_/                                                                         |
| 35  | -- 全文搜索通过 MATCH() 函数完成。                                                       |
| 36  | -- 搜索字符串做为 against() 的参数被给定。搜索以忽略字母大小写的方式执行。对于表中的每个 |
|     | 记录行，MATCH() 返回一个相关性值。即，在搜索字符串与记录行在 MATCH() 列表中指定的列的文  |
|     | 本之间的相似性尺度。                                                                     |
| 37  | EXPLAIN SELECT \*FROM student WHERE MATCH(studentname) AGAINST('love');                  |
| 38  |                                                                                          |
| 39  | /\*                                                                                      |
| 40  | 开始之前，先说一下全文索引的版本、存储引擎、数据类型的支持情况                           |
| 41  |                                                                                          |
| 42  | MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；                                 |
| 43  | MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;                         |
| 44  | 只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。                      |
| 45  | 测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。  |
| 46  | \*/                                                                                      |

关于 EXPLAIN ： [https://blog.csdn.net/jiadajing267/article/details/81269067](https://blog.csdn.net/jiadajing267/article/details/81269067)
百度搜索：explain 中文意思：解释，说明

## 拓展：测试索引

### 建表 app_user：

| 1   | CREATE TABLE `app_user` (                                               |
| --- | ----------------------------------------------------------------------- |
| 2   | `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,                       |
| 3   | `name` varchar(50) DEFAULT '' COMMENT '用户昵称',                       |
| 4   | `email` varchar(50) NOT NULL COMMENT '用户邮箱',                        |
| 5   | `phone` varchar(20) DEFAULT '' COMMENT '手机号',                        |
| 6   | `gender` tinyint(4) unsigned DEFAULT '0' COMMENT '性别（0:男；1：女）', |
| 7   | `password` varchar(100) NOT NULL COMMENT '密码',                        |
| 8   | `age` tinyint(4) DEFAULT '0' COMMENT '年龄',                            |
| 9   | `create_time` datetime DEFAULT CURRENT_TIMESTAMP,                       |
| 10  | `update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE        |
|     | CURRENT_TIMESTAMP,                                                      |
| 11  | PRIMARY KEY (`id`)                                                      |
| 12  | ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='app 用户表'            |

**批量插入数据：100w**

| 1   | DROP FUNCTION IF EXISTS mock_data;                                       |
| --- | ------------------------------------------------------------------------ |
| 2   | DELIMITER $$                                                             |
| 3   | CREATE FUNCTION mock_data()                                              |
| 4   | RETURNS INT                                                              |
| 5   | BEGIN                                                                    |
| 6   | DECLARE num INT DEFAULT 1000000;                                         |
| 7   | DECLARE i INT DEFAULT 0;                                                 |
| 8   | WHILE i < num DO                                                         |
| 9   | INSERT INTO app_user(`name`, `email`, `phone`, `gender`, `password`,     |
|     | `age`)                                                                   |
| 10  | VALUES(CONCAT('用户', i), '24736743@qq.com', CONCAT('18', FLOOR(RAND()\* |
|     | (999999999-100000000)+100000000)),FLOOR(RAND()\*2),UUID(),               |
|     | FLOOR(RAND()\*100));                                                     |
| 11  | SET i = i + 1;                                                           |
| 12  | END WHILE;                                                               |
| 13  | RETURN i;                                                                |
| 14  | END;                                                                     |
| 15  | SELECT mock_data();                                                      |

### 索引效率测试

无索引

| 1   | SELECT \* FROM app_user WHERE name = '用户 9999'; -- 查看耗时        |
| --- | -------------------------------------------------------------------- |
| 2   | SELECT \* FROM app_user WHERE name = '用户 9999';                    |
| 3   | SELECT \* FROM app_user WHERE name = '用户 9999';                    |
| 4   |                                                                      |
| 5   | mysql> EXPLAIN SELECT \* FROM app_user WHERE name = '用户 9999'\G    |
| 6   | ************\*\*\************* 1. row ************\*\*\************* |
| 7   | id: 1                                                                |
| 8   | select_type: SIMPLE                                                  |
| 9   | table: app_user                                                      |
| 10  | partitions: NULL                                                     |
| 11  | type: ALL                                                            |
| 12  | possible_keys: NULL                                                  |
| 13  | key: NULL                                                            |
| 14  | key_len: NULL                                                        |
| 15  | ref: NULL                                                            |
| 16  | rows: 992759                                                         |
| 17  | filtered: 10.00                                                      |
| 18  | Extra: Using where                                                   |
| 19  | 1 row in set, 1 warning (0.00 sec)                                   |

创建索引

| 1   | CREATE | INDEX | idx_app_user_name | ON  | app_user(name); |
| --- | ------ | ----- | ----------------- | --- | --------------- |

测试普通索引

| 1   | mysql> EXPLAIN SELECT \* FROM app_user WHERE name = '用户 9999'\G    |
| --- | -------------------------------------------------------------------- |
| 2   | ************\*\*\************* 1. row ************\*\*\************* |
| 3   | id: 1                                                                |
| 4   | select_type: SIMPLE                                                  |
| 5   | table: app_user                                                      |
| 6   | partitions: NULL                                                     |
| 7   | type: ref                                                            |
| 8   | possible_keys: idx_app_user_name                                     |
| 9   | key: idx_app_user_name                                               |
| 10  | key_len: 203                                                         |
| 11  | ref: const                                                           |
| 12  | rows: 1                                                              |
| 13  | filtered: 100.00                                                     |
| 14  | Extra: NULL                                                          |
| 15  | 1 row in set, 1 warning (0.00 sec)                                   |
| 16  |                                                                      |
| 17  | mysql> SELECT \* FROM app_user WHERE name = '用户 9999';             |
| 18  | 1 row in set (0.00 sec)                                              |
| 19  |                                                                      |
| 20  | mysql> SELECT \* FROM app_user WHERE name = '用户 9999';             |
| 21  | 1 row in set (0.00 sec)                                              |
| 22  |                                                                      |
| 23  | mysql> SELECT \* FROM app_user WHERE name = '用户 9999';             |
| 24  | 1 row in set (0.00 sec)                                              |

## 、索引准则

索引不是越多越好
不要对经常变动的数据加索引小数据量的表建议不要加索引索引一般应加在查找条件的字段

## 、索引的数据结构

| 1   | -- 我们可以在创建上述索引的时候，为其指定索引类型，分两类                                |
| --- | ---------------------------------------------------------------------------------------- |
| 2   | hash 类型的索引：查询单条快，范围查询慢                                                  |
| 3   | btree 类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为 innodb 默认支持它） |
| 4   |                                                                                          |
| 5   | -- 不同的存储引擎支持的索引类型也不一样                                                  |
| 6   | InnoDB 支持事务，支持行级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；       |
| 7   | MyISAM 不支持事务，支持表级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；     |
| 8   | Memory 不支持事务，支持表级别锁定，支持 B-tree、Hash 等索引，不支持 Full-text 索引；     |
| 9   | NDB 支持事务，支持行级别锁定，支持 Hash 索引，不支持 B-tree、Full-text 等索引；          |
| 10  | Archive 不支持事务，支持表级别锁定，不支持 B-tree、Hash、Full-text 等索引；              |

关于索引的本质：[http://blog.codinglabs.org/articles/theory-of-mysql-index.html](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)

# 8、权限管理

## ![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834335279-ee17dd66-3502-4524-b493-9d18d4545d2c.png#)、用户管理

1、使用 SQLyog 创建用户，并授予权限演示
2、基本命令

| 1   | /_ 用户和权限管理 _/ ------------------                                               |
| --- | ------------------------------------------------------------------------------------- |
| 2   | 用户信息表：mysql.user                                                                |
| 3   |                                                                                       |
| 4   | -- 刷新权限                                                                           |
| 5   | FLUSH PRIVILEGES                                                                      |
| 6   |                                                                                       |
| 7   | -- 增加用户 CREATE USER kuangshen IDENTIFIED BY '123456'                              |
| 8   | CREATE USER 用户名 IDENTIFIED BY [PASSWORD] 密码(字符串)                              |
| 9   | - 必须拥有 mysql 数据库的全局 CREATE USER 权限，或拥有 INSERT 权限。                  |
| 10  | - 只能创建用户，不能赋予权限。                                                        |
| 11  | - 用户名，注意引号：如 'user_name'@'192.168.1.1'                                      |
| 12  | - 密码也需引号，纯数字密码也要加引号                                                  |
| 13  | - 要在纯文本中指定密码，需忽略 PASSWORD 关键词。要把密码指定为由 PASSWORD()函数返回的 |
|     | 混编值，需包含关键字 PASSWORD                                                         |
| 14  |                                                                                       |
| 15  | -- 重命名用户 RENAME USER kuangshen TO kuangshen2                                     |
| 16  | RENAME USER old_user TO new_user                                                      |
| 17  |                                                                                       |
| 18  | -- 设置密码                                                                           |
| 19  | SET PASSWORD = PASSWORD('密码') -- 为当前用户设置密码                                 |
| 20  | SET PASSWORD FOR 用户名 = PASSWORD('密码') -- 为指定用户设置密码                      |

| 21  |                                                                        |
| --- | ---------------------------------------------------------------------- |
| 22  | -- 删除用户 DROP USER kuangshen2                                       |
| 23  | DROP USER 用户名                                                       |
| 24  |                                                                        |
| 25  | -- 分配权限/添加用户                                                   |
| 26  | GRANT 权限列表 ON 表名 TO 用户名 [IDENTIFIED BY [PASSWORD] 'password'] |
| 27  | - all privileges 表示所有权限                                          |
| 28  | - _._ 表示所有库的所有表                                               |
| 29  | - 库名.表名 表示某库下面的某表                                         |
| 30  |                                                                        |
| 31  | -- 查看权限 SHOW GRANTS FOR root@localhost;                            |
| 32  | SHOW GRANTS FOR 用户名                                                 |
| 33  | -- 查看当前用户权限                                                    |
| 34  | SHOW GRANTS; 或 SHOW GRANTS FOR CURRENT_USER; 或 SHOW GRANTS FOR       |
|     | CURRENT_USER();                                                        |
| 35  |                                                                        |
| 36  | -- 撤消权限                                                            |
| 37  | REVOKE 权限列表 ON 表名 FROM 用户名                                    |
| 38  | REVOKE ALL PRIVILEGES, GRANT OPTION FROM 用户名 -- 撤销所有权限        |

权限解释

| 1   | -- 权限列表                                                                   |
| --- | ----------------------------------------------------------------------------- |
| 2   | ALL [PRIVILEGES] -- 设置除 GRANT OPTION 之外的所有简单权限                    |
| 3   | ALTER -- 允许使用 ALTER TABLE                                                 |
| 4   | ALTER ROUTINE -- 更改或取消已存储的子程序                                     |
| 5   | CREATE -- 允许使用 CREATE TABLE                                               |
| 6   | CREATE ROUTINE -- 创建已存储的子程序                                          |
| 7   | CREATE TEMPORARY TABLES -- 允许使用 CREATE TEMPORARY TABLE                    |
| 8   | CREATE USER -- 允许使用 CREATE USER, DROP USER, RENAME USER 和 REVOKE ALL     |
|     | PRIVILEGES。                                                                  |
| 9   | CREATE VIEW -- 允许使用 CREATE VIEW                                           |
| 10  | DELETE -- 允许使用 DELETE                                                     |
| 11  | DROP -- 允许使用 DROP TABLE                                                   |
| 12  | EXECUTE -- 允许用户运行已存储的子程序                                         |
| 13  | FILE -- 允许使用 SELECT...INTO OUTFILE 和 LOAD DATA INFILE                    |
| 14  | INDEX -- 允许使用 CREATE INDEX 和 DROP INDEX                                  |
| 15  | INSERT -- 允许使用 INSERT                                                     |
| 16  | LOCK TABLES -- 允许对您拥有 SELECT 权限的表使用 LOCK TABLES                   |
| 17  | PROCESS -- 允许使用 SHOW FULL PROCESSLIST                                     |
| 18  | REFERENCES -- 未被实施                                                        |
| 19  | RELOAD -- 允许使用 FLUSH                                                      |
| 20  | REPLICATION CLIENT -- 允许用户询问从属服务器或主服务器的地址                  |
| 21  | REPLICATION SLAVE -- 用于复制型从属服务器（从主服务器中读取二进制日志事件）   |
| 22  | SELECT -- 允许使用 SELECT                                                     |
| 23  | SHOW DATABASES -- 显示所有数据库                                              |
| 24  | SHOW VIEW -- 允许使用 SHOW CREATE VIEW                                        |
| 25  | SHUTDOWN -- 允许使用 mysqladmin shutdown                                      |
| 26  | SUPER -- 允许使用 CHANGE MASTER, KILL, PURGE MASTER LOGS 和 SET GLOBAL 语句， |
|     | mysqladmin debug 命令；允许您连接（一次），即使已达到 max_connections。       |
| 27  | UPDATE -- 允许使用 UPDATE                                                     |
| 28  | USAGE -- “无权限”的同义词                                                     |
| 29  | GRANT OPTION -- 允许授予权限                                                  |
| 30  |                                                                               |
| 31  |                                                                               |
| 32  | /_ 表维护 _/                                                                  |
| 33  |                                                                               |

| 34  | -- 分析和存储表的关键字分布                        |
| --- | -------------------------------------------------- | --------------------------------------------------- | ------ | -------- | -------- |
| 35  | ANALYZE [LOCAL                                     | NO_WRITE_TO_BINLOG] TABLE 表名 ...                  |
| 36  | -- 检查一个或多个表是否有错误                      |
| 37  | CHECK TABLE tbl_name [, tbl_name] ... [option] ... |
| 38  | option = {QUICK                                    | FAST                                                | MEDIUM | EXTENDED | CHANGED} |
| 39  | -- 整理数据文件的碎片                              |
| 40  | OPTIMIZE [LOCAL                                    | NO_WRITE_TO_BINLOG] TABLE tbl_name [, tbl_name] ... |

1.  **、MySQL 备份**

数据库备份必要性
保证重要数据不丢失数据转移
MySQL 数据库备份方法
mysqldump 备份工具
数据库管理工具,如 SQLyog
直接拷贝数据库文件和相关配置文件

### mysqldump 客户端

作用 :
转储数据库
搜集数据库进行备份
![](https://cdn.nlark.com/yuque/0/2021/jpeg/21990331/1625834335765-17bcd9b1-87c1-453d-bf0c-ed9831495510.jpeg#)将数据转移到另一个 SQL 服务器,不一定是 MySQL 服务器

| 1   | -- 导出                                                                    |
| --- | -------------------------------------------------------------------------- |
| 2   | 1. 导出一张表 -- mysqldump -uroot -p123456 school student >D:/a.sql        |
| 3   | mysqldump -u 用户名 -p 密码 库名 表名 > 文件名(D:/a.sql)                   |
| 4   | 2. 导出多张表 -- mysqldump -uroot -p123456 school student result >D:/a.sql |
| 5   | mysqldump -u 用户名 -p 密码 库名 表 1 表 2 表 3 > 文件名(D:/a.sql)         |
| 6   | 3. 导出所有表 -- mysqldump -uroot -p123456 school >D:/a.sql                |
| 7   | mysqldump -u 用户名 -p 密码 库名 > 文件名(D:/a.sql)                        |
| 8   | 4. 导出一个库 -- mysqldump -uroot -p123456 -B school >D:/a.sql             |
| 9   | mysqldump -u 用户名 -p 密码 -B 库名 > 文件名(D:/a.sql)                     |
| 10  |                                                                            |
| 11  | 可以-w 携带备份条件                                                        |
| 12  |                                                                            |

| 13  | --  | 导入                                       |
| --- | --- | ------------------------------------------ |
| 14  | 1.  | 在登录 mysql 的情况下： -- source D:/a.sql |
| 15  |     | source 备份文件                            |
| 16  | 2.  | 在不登录的情况下                           |
| 17  |     | mysql -u 用户名 -p 密码 库名 < 备份文件    |

# 9、规范化数据库设计

## 、为什么需要数据库设计

### 当数据库比较复杂时我们需要设计数据库糟糕的数据库设计 :

数据冗余,存储空间浪费数据更新和插入的异常程序性能差

### 良好的数据库设计 :

节省数据的存储空间能够保证数据的完整性
方便进行数据库应用系统的开发

### 软件项目开发周期中数据库设计 :

需求分析阶段: 分析客户的业务和数据处理需求
概要设计阶段:设计数据库的 E-R 模型图 , 确认需求信息的正确和完整.

### 设计数据库步骤

收集信息
与该系统有关人员进行交流 , 座谈 , 充分了解用户需求 , 理解数据库需要完成的任务. 标识实体[Entity]
标识数据库要管理的关键对象或实体,实体一般是名词标识每个实体需要存储的详细信息[Attribute]
标识实体之间的关系[Relationship]

## 、三大范式

### 问题 : 为什么需要数据规范化?

不合规范的表设计会导致的问题： 信息重复
更新异常
插入异常
无法正确表示信息删除异常
丢失有效信息

百度搜索：三大范式

### 第一范式 (1st NF)

第一范式的目标是确保每列的原子性,如果每列都是不可再分的最小数据单元,则满足第一范式

### 第二范式(2nd NF)

第二范式（2NF）是在第一范式（1NF）的基础上建立起来的，即满足第二范式（2NF）必须先满足第一范式（1NF）。
第二范式要求每个表只描述一件事情

### 第三范式(3rd NF)

如果一个关系满足第二范式,并且除了主键以外的其他列都不传递依赖于主键列,则满足第三范式. 第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。

### 规范化和性能的关系

为满足某种商业目标 , 数据库性能比规范化数据库更重要在数据规范化的同时 , 要综合考虑数据库的性能
通过在给定的表中添加额外的字段,以大量减少需要从中搜索信息所需的时间
通过在给定的表中插入计算列,以方便查询

# 10、JDBC

## 、数据库驱动

这里的驱动的概念和平时听到的那种驱动的概念是一样的，比如平时购买的声卡，网卡直接插到计算机 上面是不能用的，必须要安装相应的驱动程序之后才能够使用声卡和网卡，同样道理，我们安装好数据 库之后，我们的应用程序也是不能直接使用数据库的，必须要通过相应的数据库驱动程序，通过驱动程 序去和数据库打交道，如下所示：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834336193-943040ff-9bc1-4528-a26c-5a355f1b8eca.png#)

## 、JDBC 介绍

SUN 公司为了简化、统一对数据库的操作，定义了一套 Java 操作数据库的规范（接口），称之为 JDBC。 这套接口由数据库厂商去实现，这样，开发人员只需要学习 jdbc 接口，并通过 jdbc 加载具体的驱动，就可以操作数据库。
如下图所示：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834336708-34ce4a27-fd49-4fbd-af32-8d96b73373dd.png#)
JDBC 全称为：Java Data Base Connectivity（java 数据库连接），它主要由接口组成。组成 JDBC 的２个包：java.sql、javax.sql
开发 JDBC 应用需要以上 2 个包的支持外，还需要导入相应 JDBC 的数据库实现(即数据库驱动)。

## 、编写 JDBC 程序

搭建实验环境

| 1   | CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci; |
| --- | --------------------------------------------------------------------- |
| 2   |                                                                       |
| 3   | USE jdbcStudy;                                                        |
| 4   |                                                                       |
| 5   | CREATE TABLE users(                                                   |
| 6   | id INT PRIMARY KEY,                                                   |
| 7   | NAME VARCHAR(40),                                                     |
| 8   | PASSWORD VARCHAR(40),                                                 |
| 9   | email VARCHAR(60),                                                    |
| 10  | birthday DATE                                                         |
| 11  | );                                                                    |
| 12  |                                                                       |
| 13  | INSERT INTO users(id,NAME,PASSWORD,email,birthday)                    |
| 14  | VALUES(1,'zhansan','123456','zs@sina.com','1980-12-04'),              |
| 15  | (2,'lisi','123456','lisi@sina.com','1981-12-04'),                     |
| 16  | (3,'wangwu','123456','wangwu@sina.com','1979-12-04');                 |

新建一个 Java 工程，并导入数据驱动

![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834337194-ecfb8d45-6f98-4fdc-b440-dad182b0564c.png#)

编写程序从 user 表中读取数据，并打印在命令行窗口中。

| 1   | package com.kuang.lesson01;                                            |
| --- | ---------------------------------------------------------------------- |
| 2   |                                                                        |
| 3   | import java.sql.Connection;                                            |
| 4   | import java.sql.DriverManager;                                         |
| 5   | import java.sql.ResultSet;                                             |
| 6   | import java.sql.Statement;                                             |
| 7   |                                                                        |
| 8   | public class JdbcFirstDemo {                                           |
| 9   |                                                                        |
| 10  | public static void main(String[] args) throws Exception {              |
| 11  | //要连接的数据库 URL                                                   |
| 12  | String url = "jdbc:mysql://localhost:3306/jdbcStudy?                   |
|     | useUnicode=true&characterEncoding=utf8&useSSL=true";                   |
| 13  | //连接的数据库时使用的用户名                                           |
| 14  | String username = "root";                                              |
| 15  | //连接的数据库时使用的密码                                             |
| 16  | String password = "123456";                                            |
| 17  |                                                                        |
| 18  | //1.加载驱动                                                           |
| 19  | //DriverManager.registerDriver(new com.mysql.jdbc.Driver());不推荐使用 |
|     | 这种方式来加载驱动                                                     |
| 20  | Class.forName("com.mysql.jdbc.Driver");//推荐使用这种方式来加载驱动    |
| 21  | //2.获取与数据库的链接                                                 |
| 22  | Connection conn = DriverManager.getConnection(url, username,           |
|     | password);                                                             |
| 23  |                                                                        |
| 24  | //3.获取用于向数据库发送 sql 语句的 statement                          |
| 25  | Statement st = conn.createStatement();                                 |
| 26  |                                                                        |
| 27  | String sql = "select id,name,password,email,birthday from users";      |
| 28  | //4.向数据库发 sql,并获取代表结果集的 resultset                        |
| 29  | ResultSet rs = st.executeQuery(sql);                                   |
| 30  |                                                                        |
| 31  | //5.取出结果集的数据                                                   |
| 32  | while(rs.next()){                                                      |
| 33  | System.out.println("id=" + rs.getObject("id"));                        |
| 34  | System.out.println("name=" + rs.getObject("name"));                    |
| 35  | System.out.println("password=" + rs.getObject("password"));            |
| 36  | System.out.println("email=" + rs.getObject("email"));                  |
| 37  | System.out.println("birthday=" + rs.getObject("birthday"));            |
| 38  | }                                                                      |
| 39  |                                                                        |

| 40  |     |     | //6.关闭链接，释放资源 |
| --- | --- | --- | ---------------------- |
| 41  |     |     | rs.close();            |
| 42  |     |     | st.close();            |
| 43  |     |     | conn.close();          |
| 44  |     | }   |                        |
| 45  | }   |     |                        |

1.  **、对象说明**

DriverManager 类讲解
Jdbc 程序中的 DriverManager 用于加载驱动，并创建与数据库的链接，这个 API 的常用方法：

| 1   | DriverManager.registerDriver(new | Driver())       |
| --- | -------------------------------- | --------------- |
| 2   | DriverManager.getConnection(url, | user, password) |

注意：**在实际开发中并不推荐采用 registerDriver 方法注册驱动**。原因有二：

      1. 查看Driver的源代码可以看到，如果采用此种方式，会导致驱动程序注册两次，也就是在内存中会有两个Driver对象。
      1. 程序依赖mysql的api，脱离mysql的jar包，程序将无法编译，将来程序切换底层数据库将会非常麻      烦。

### 推荐方式：Class.forName("com.mysql.jdbc.Driver");

采用此种方式不会导致驱动对象在内存中重复出现，并且采用此种方式，程序仅仅只需要一个字符串， 不需要依赖具体的驱动，使程序的灵活性更高。

数据库 URL 讲解
URL 用于标识数据库的位置，通过 URL 地址告诉 JDBC 程序连接哪个数据库，URL 的写法为：
![](https://cdn.nlark.com/yuque/0/2021/png/21990331/1625834337701-9c1334cb-11c7-4daa-9aea-c4772ed95bc5.png#)
常用数据库 URL 地址的写法：
Oracle 写法：jdbc:oracle:thin:@localhost:1521:sid
SqlServer 写 法 ：jdbc:microsoft:sqlserver://localhost:1433; DatabaseName=sid MySql 写法：jdbc:mysql://localhost:3306/sid
如果连接的是本地的 Mysql 数据库，并且连接使用的端口是 3306，那么的 url 地址可以简写为

jdbc:mysql:///数据库
Connection 类讲解

Jdbc 程序中的 Connection，它用于代表数据库的链接，Collection 是数据库编程中最重要的一个对象， 客户端与数据库所有交互都是通过 connection 对象完成的，这个对象的常用方法：
createStatement()：创建向数据库发送 sql 的 statement 对象。prepareStatement(sql) ：创建向数据库发送预编译 sql 的 PrepareSatement 对象。setAutoCommit(boolean autoCommit)：设置事务是否自动提交。
commit() ：在链接上提交事务。
rollback() ：在此链接上回滚事务。

Statement 类讲解
Jdbc 程序中的 Statement 对象用于向数据库发送 SQL 语句， Statement 对象常用方法： executeQuery(String sql) ：用于向数据发送查询语句。
executeUpdate(String sql)：用于向数据库发送 insert、update 或 delete 语句
execute(String sql)：用于向数据库发送任意 sql 语句 addBatch(String sql) ：把多条 sql 语句放到一个批处理中。executeBatch()：向数据库发送一批 sql 语句执行。

ResultSet 类讲解
Jdbc 程序中的 ResultSet 用于代表 Sql 语句的执行结果。Resultset 封装执行结果时，采用的类似于表格的方式。ResultSet 对象维护了一个指向表格数据行的游标，初始的时候，游标在第一行之前，调用 ResultSet.next() 方法，可以使游标指向具体的数据行，进行调用方法获取该行的数据。
ResultSet 既然用于封装执行结果的，所以该对象提供的都是用于获取数据的 get 方法： 获取任意类型的数据
getObject(int index)
getObject(string columnName)
获 取 指 定 类 型 的 数 据 ， 例 如 ： getString(int index) getString(String columnName)

ResultSet 还提供了对结果集进行滚动的方法：
next()：移动到下一行 Previous()：移动到前一行 absolute(int row)：移动到指定行
beforeFirst()：移动 resultSet 的最前面。afterLast() ：移动到 resultSet 的最后面。

释放资源
Jdbc 程序运行完后，切记要释放程序在运行过程中，创建的那些与数据库进行交互的对象，这些对象通常是 ResultSet, Statement 和 Connection 对象，特别是 Connection 对象，它是非常稀有的资源，用完后必须马上释放，如果 Connection 不能及时、正确的关闭，极易导致系统宕机。Connection 的使用原则 是尽量晚创建，尽量早的释放。

为确保资源释放代码能运行，资源释放代码也一定要放在 ﬁnally 语句中。

## 、statement 对象

Jdbc 中的 statement 对象用于向数据库发送 SQL 语句，想完成对数据库的增删改查，只需要通过这个对象 向数据库发送增删改查语句即可。
Statement 对象的 executeUpdate 方法，用于向数据库发送增、删、改的 sql 语句，executeUpdate 执行 完后，将会返回一个整数（即增删改语句导致了数据库几行数据发生了变化）。
Statement.executeQuery 方法用于向数据库发送查询语句，executeQuery 方法返回代表查询结果的 ResultSet 对象。

CRUD 操作-create
使用 executeUpdate(String sql)方法完成数据添加操作，示例操作：

Statement st = conn.createStatement();
String sql = "insert into user(….) values(…..) "; int num = st.executeUpdate(sql);
if(num>0){
System.out.println("插入成功！！！");
}
1
2
3
4
5
6

CRUD 操作-delete
使用 executeUpdate(String sql)方法完成数据删除操作，示例操作：

Statement st = conn.createStatement(); String sql = "delete from user where id=1"; int num = st.executeUpdate(sql);
if(num>0){
System.out.println(“删除成功！！！");
}
1
2
3
4
5
6

CRUD 操作-update
使用 executeUpdate(String sql)方法完成数据修改操作，示例操作：

Statement st = conn.createStatement();
String sql = "update user set name='' where name=''"; int num = st.executeUpdate(sql);
if(num>0){
System.out.println(“修改成功！！！");
}
1
2
3
4
5
6

CRUD 操作-read
使用 executeQuery(String sql)方法完成数据查询操作，示例操作：

Statement st = conn.createStatement();
String sql = "select \* from user where id=1"; ResultSet rs = st.executeUpdate(sql); while(rs.next()){
//根据获取列的数据类型，分别调用 rs 的相应方法映射到 java 对象中
}
1
2
3
4
5
6

### 使用 jdbc 对数据库增删改查

1、新建一个 lesson02 的包
2、在 src 目录下创建一个 db.properties 文件，如下图所示：

| 1   | driver=com.mysql.jdbc.Driver                       |
| --- | -------------------------------------------------- |
| 2   | url=jdbc:mysql://localhost:3306/jdbcStudy?         |
|     | useUnicode=true&characterEncoding=utf8&useSSL=true |
| 3   | username=root                                      |
| 4   | password=123456                                    |

3、在 lesson02 下新建一个 utils 包，新建一个类
JdbcUtils

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
package com.kuang.lesson02.utils;
import java.io.InputStream; import java.sql.Connection; import java.sql.DriverManager; import java.sql.ResultSet; import java.sql.SQLException; import java.sql.Statement; import java.util.Properties;

public class JdbcUtils {

private static String driver = null; private static String url = null; private static String username = null; private static String password = null;

static{
try{
22
23
24
25
26
27
28
29
30
//读取 db.properties 文件中的数据库连接信息
InputStream in = JdbcUtils.class.getClassLoader().getResourceAsStream("db.properties");
Properties prop = new Properties();
prop.load(in);
//获取数据库连接驱动
driver = prop.getProperty("driver");
//获取数据库连接 URL 地址
url = prop.getProperty("url");
//获取数据库连接用户名
username = prop.getProperty("username");

| 31  | //获取数据库连接密码                                                                   |
| --- | -------------------------------------------------------------------------------------- |
| 32  | password = prop.getProperty("password");                                               |
| 33  |                                                                                        |
| 34  | //加载数据库驱动                                                                       |
| 35  | Class.forName(driver);                                                                 |
| 36  |                                                                                        |
| 37  | }catch (Exception e) {                                                                 |
| 38  | throw new ExceptionInInitializerError(e);                                              |
| 39  | }                                                                                      |
| 40  | }                                                                                      |
| 41  |                                                                                        |
| 42  | // 获取数据库连接对象                                                                  |
| 43  | public static Connection getConnection() throws SQLException{                          |
| 44  | return DriverManager.getConnection(url, username,password);                            |
| 45  | }                                                                                      |
| 46  |                                                                                        |
| 47  | // 释放资源，要释放的资源包括 Connection 数据库连接对象，负责执行 SQL 命令的 Statement |
|     | 对象，存储查询结果的 ResultSet 对象                                                    |
| 48  | public static void release(Connection conn,Statement st,ResultSet rs){                 |
| 49  | if(rs!=null){                                                                          |
| 50  | try{                                                                                   |
| 51  | //关闭存储查询结果的 ResultSet 对象                                                    |
| 52  | rs.close();                                                                            |
| 53  | }catch (Exception e) {                                                                 |
| 54  | e.printStackTrace();                                                                   |
| 55  | }                                                                                      |
| 56  | rs = null;                                                                             |
| 57  | }                                                                                      |
| 58  | if(st!=null){                                                                          |
| 59  | try{                                                                                   |
| 60  | //关闭负责执行 SQL 命令的 Statement 对象                                               |
| 61  | st.close();                                                                            |
| 62  | }catch (Exception e) {                                                                 |
| 63  | e.printStackTrace();                                                                   |
| 64  | }                                                                                      |
| 65  | }                                                                                      |
| 66  |                                                                                        |
| 67  | if(conn!=null){                                                                        |
| 68  | try{                                                                                   |
| 69  | //关闭 Connection 数据库连接对象                                                       |
| 70  | conn.close();                                                                          |
| 71  | }catch (Exception e) {                                                                 |
| 72  | e.printStackTrace();                                                                   |
| 73  | }                                                                                      |
| 74  | }                                                                                      |
| 75  | }                                                                                      |
| 76  | }                                                                                      |

使用 statement 对象完成对数据库的 CRUD 操作
1、插入一条数据

1 package com.kuang.lesson02; 2
3 import com.kuang.lesson02.utils.JdbcUtils;

| 4   |                                                                  |
| --- | ---------------------------------------------------------------- |
| 5   | import java.sql.Connection;                                      |
| 6   | import java.sql.ResultSet;                                       |
| 7   | import java.sql.Statement;                                       |
| 8   |                                                                  |
| 9   | public class TestInsert {                                        |
| 10  | public static void main(String[] args) {                         |
| 11  | Connection conn = null;                                          |
| 12  | Statement st = null;                                             |
| 13  | ResultSet rs = null;                                             |
| 14  | try{                                                             |
| 15  | //获取一个数据库连接                                             |
| 16  | conn = JdbcUtils.getConnection();                                |
| 17  | //通过 conn 对象获取负责执行 SQL 命令的 Statement 对象           |
| 18  | st = conn.createStatement();                                     |
| 19  | //要执行的 SQL 命令                                              |
| 20  | String sql = "insert into users(id,name,password,email,birthday) |
|     | " +                                                              |
| 21  | "values(4,'kuangshen','123','24736743@qq.com','2020-01-          |
|     | 01')";                                                           |
| 22  | //执行插入操作，executeUpdate 方法返回成功的条数                 |
| 23  | int num = st.executeUpdate(sql);                                 |
| 24  | if(num>0){                                                       |
| 25  | System.out.println("插入成功！！");                              |
| 26  | }                                                                |
| 27  |                                                                  |
| 28  | }catch (Exception e) {                                           |
| 29  | e.printStackTrace();                                             |
| 30  | }finally{                                                        |
| 31  | //SQL 执行完成之后释放相关资源                                   |
| 32  | JdbcUtils.release(conn, st, rs);                                 |
| 33  | }                                                                |
| 34  | }                                                                |
| 35  | }                                                                |

2、删除一条数据

| 1   | package com.kuang.lesson02;                  |
| --- | -------------------------------------------- |
| 2   |                                              |
| 3   | import com.kuang.lesson02.utils.JdbcUtils;   |
| 4   |                                              |
| 5   | import java.sql.Connection;                  |
| 6   | import java.sql.ResultSet;                   |
| 7   | import java.sql.Statement;                   |
| 8   |                                              |
| 9   | public class TestDelete {                    |
| 10  | public static void main(String[] args) {     |
| 11  | Connection conn = null;                      |
| 12  | Statement st = null;                         |
| 13  | ResultSet rs = null;                         |
| 14  | try{                                         |
| 15  | conn = JdbcUtils.getConnection();            |
| 16  | String sql = "delete from users where id=4"; |
| 17  | st = conn.createStatement();                 |
| 18  | int num = st.executeUpdate(sql);             |
| 19  | if(num>0){                                   |
| 20  | System.out.println("删除成功！！");          |
| 21  | }                                            |

| 22  |     |     | }catch (Exception e) {  |     |      |
| --- | --- | --- | ----------------------- | --- | ---- |
| 23  |     |     | e.printStackTrace();    |     |
| 24  |     |     |                         |     |
| 25  |     |     | }finally{               |     |
| 26  |     |     | JdbcUtils.release(conn, | st, | rs); |
| 27  |     |     | }                       |     |      |
| 28  |     | }   |                         |     |      |
| 29  | }   |     |                         |     |      |

3、更新一条数据

| 1   | package com.kuang.lesson02;                           |
| --- | ----------------------------------------------------- |
| 2   |                                                       |
| 3   | import com.kuang.lesson02.utils.JdbcUtils;            |
| 4   |                                                       |
| 5   | import java.sql.Connection;                           |
| 6   | import java.sql.ResultSet;                            |
| 7   | import java.sql.Statement;                            |
| 8   |                                                       |
| 9   | public class TestUpdate {                             |
| 10  | public static void main(String[] args) {              |
| 11  | Connection conn = null;                               |
| 12  | Statement st = null;                                  |
| 13  | ResultSet rs = null;                                  |
| 14  | try{                                                  |
| 15  | conn = JdbcUtils.getConnection();                     |
| 16  | String sql = "update users set                        |
|     | name='kuangshen',email='24736743@qq.com' where id=3"; |
| 17  | st = conn.createStatement();                          |
| 18  | int num = st.executeUpdate(sql);                      |
| 19  | if(num>0){                                            |
| 20  | System.out.println("更新成功！！");                   |
| 21  | }                                                     |
| 22  | }catch (Exception e) {                                |
| 23  | e.printStackTrace();                                  |
| 24  |                                                       |
| 25  | }finally{                                             |
| 26  | JdbcUtils.release(conn, st, rs);                      |
| 27  | }                                                     |
| 28  | }                                                     |
| 29  | }                                                     |

4、查询数据

| 1   | package com.kuang.lesson02;                |
| --- | ------------------------------------------ |
| 2   |                                            |
| 3   | import com.kuang.lesson02.utils.JdbcUtils; |
| 4   |                                            |
| 5   | import java.sql.Connection;                |
| 6   | import java.sql.ResultSet;                 |
| 7   | import java.sql.Statement;                 |
| 8   |                                            |
| 9   | public class TestSelect {                  |
| 10  | public static void main(String[] args) {   |
| 11  | Connection conn = null;                    |
| 12  | Statement st = null;                       |
| 13  | ResultSet rs = null;                       |
| 14  | try{                                       |

| 15 |

} |

} | conn = JdbcUtils.getConnection();
String sql = "select \* from users where id=3"; st = conn.createStatement();
rs = st.executeQuery(sql); if(rs.next()){
System.out.println(rs.getString("name"));
}
}catch (Exception e) { e.printStackTrace();
}finally{
JdbcUtils.release(conn, st, rs);
} |
| --- | --- | --- | --- |
| 16 | | | |
| 17 | | | |
| 18 | | | |
| 19 | | | |
| 20 | | | |
| 21 | | | |
| 22 | | | |
| 23 | | | |
| 24 | | | |
| 25 | | | |
| 26 | | | |
| 27 | | | |
| 28 | | | |

SQL 注入问题
通过巧妙的技巧来拼接字符串，造成 SQL 短路，从而获取数据库数据

| 1   | package com.kuang.lesson02;                                      |
| --- | ---------------------------------------------------------------- |
| 2   |                                                                  |
| 3   | import com.kuang.lesson02.utils.JdbcUtils;                       |
| 4   |                                                                  |
| 5   | import java.sql.Connection;                                      |
| 6   | import java.sql.ResultSet;                                       |
| 7   | import java.sql.Statement;                                       |
| 8   |                                                                  |
| 9   | public class SQL 注入 {                                          |
| 10  | public static void main(String[] args) {                         |
| 11  | // login("zhangsan","123456"); // 正常登陆                       |
| 12  | login("'or '1=1","123456"); // SQL 注入                          |
| 13  | }                                                                |
| 14  |                                                                  |
| 15  | public static void login(String username,String password){       |
| 16  | Connection conn = null;                                          |
| 17  | Statement st = null;                                             |
| 18  | ResultSet rs = null;                                             |
| 19  | try{                                                             |
| 20  | conn = JdbcUtils.getConnection();                                |
| 21  | // select \* from users where name='' or '1=1' and password =    |
|     | '123456'                                                         |
| 22  | String sql = "select \* from users where name='"+username+"' and |
|     | password='"+password+"'";                                        |
| 23  | st = conn.createStatement();                                     |
| 24  | rs = st.executeQuery(sql);                                       |
| 25  | while(rs.next()){                                                |
| 26  | System.out.println(rs.getString("name"));                        |
| 27  | System.out.println(rs.getString("password"));                    |
| 28  | System.out.println("==============");                            |
| 29  | }                                                                |
| 30  | }catch (Exception e) {                                           |
| 31  | e.printStackTrace();                                             |
| 32  | }finally{                                                        |
| 33  | JdbcUtils.release(conn, st, rs);                                 |
| 34  | }                                                                |

}
}
35
36
37

## 、PreparedStatement 对象

PreperedStatement 是 Statement 的子类，它的实例对象可以通过调用 Connection.preparedStatement()方法获得，相对于 Statement 对象而言：PreperedStatement 可以避 免 SQL 注入的问题。
Statement 会使数据库频繁编译 SQL，可能造成数据库缓冲区溢出。
PreparedStatement 可对 SQL 进行预编译，从而提高数据库的执行效率。并且 PreperedStatement 对于 sql 中的参数，允许使用占位符的形式进行替换，简化 sql 语句的编写。

使用 PreparedStatement 对象完成对数据库的 CRUD 操作
1、插入数据

| 1   | package com.kuang.lesson03;                                                                 |
| --- | ------------------------------------------------------------------------------------------- |
| 2   |                                                                                             |
| 3   | import com.kuang.lesson02.utils.JdbcUtils;                                                  |
| 4   |                                                                                             |
| 5   | import java.sql.Connection;                                                                 |
| 6   | import java.util.Date;                                                                      |
| 7   | import java.sql.PreparedStatement;                                                          |
| 8   | import java.sql.ResultSet;                                                                  |
| 9   |                                                                                             |
| 10  | public class TestInsert {                                                                   |
| 11  | public static void main(String[] args) {                                                    |
| 12  | Connection conn = null;                                                                     |
| 13  | PreparedStatement st = null;                                                                |
| 14  | ResultSet rs = null;                                                                        |
| 15  | try{                                                                                        |
| 16  | //获取一个数据库连接                                                                        |
| 17  | conn = JdbcUtils.getConnection();                                                           |
| 18  | //要执行的 SQL 命令，SQL 中的参数使用?作为占位符                                            |
| 19  | String sql = "insert into users(id,name,password,email,birthday)                            |
|     | values(?,?,?,?,?)";                                                                         |
| 20  | //通过 conn 对象获取负责执行 SQL 命令的 prepareStatement 对象                               |
| 21  | st = conn.prepareStatement(sql);                                                            |
| 22  | //为 SQL 语句中的参数赋值，注意，索引是从 1 开始的                                          |
| 23  | st.setInt(1, 4);//id 是 int 类型的                                                          |
| 24  | st.setString(2, "kuangshen");//name 是 varchar(字符串类型)                                  |
| 25  | st.setString(3, "123");//password 是 varchar(字符串类型)                                    |
| 26  | st.setString(4, "[24736743@qq.com](mailto:24736743@qq.com)");//email 是 varchar(字符串类型) |
| 27  | st.setDate(5, new java.sql.Date(new                                                         |
|     | Date().getTime()));//birthday 是 date 类型                                                  |
| 28  | //执行插入操作，executeUpdate 方法返回成功的条数                                            |
| 29  | int num = st.executeUpdate();                                                               |
| 30  | if(num>0){                                                                                  |
| 31  | System.out.println("插入成功！！");                                                         |
| 32  | }                                                                                           |
| 33  |                                                                                             |

| 34 |

} |

} | }catch (Exception e) { e.printStackTrace();
}finally{
//SQL 执行完成之后释放相关资源 JdbcUtils.release(conn, st, rs);
} |
| --- | --- | --- | --- |
| 35 | | | |
| 36 | | | |
| 37 | | | |
| 38 | | | |
| 39 | | | |
| 40 | | | |
| 41 | | | |

2、删除一条数据

| 1   | package com.kuang.lesson03;                  |
| --- | -------------------------------------------- |
| 2   |                                              |
| 3   | import com.kuang.lesson02.utils.JdbcUtils;   |
| 4   |                                              |
| 5   | import java.sql.Connection;                  |
| 6   | import java.sql.PreparedStatement;           |
| 7   | import java.sql.ResultSet;                   |
| 8   |                                              |
| 9   | public class TestDelete {                    |
| 10  | public static void main(String[] args) {     |
| 11  | Connection conn = null;                      |
| 12  | PreparedStatement st = null;                 |
| 13  | ResultSet rs = null;                         |
| 14  | try{                                         |
| 15  | conn = JdbcUtils.getConnection();            |
| 16  | String sql = "delete from users where id=?"; |
| 17  | st = conn.prepareStatement(sql);             |
| 18  | st.setInt(1, 4);                             |
| 19  | int num = st.executeUpdate();                |
| 20  | if(num>0){                                   |
| 21  | System.out.println("删除成功！！");          |
| 22  | }                                            |
| 23  | }catch (Exception e) {                       |
| 24  | e.printStackTrace();                         |
| 25  | }finally{                                    |
| 26  | JdbcUtils.release(conn, st, rs);             |
| 27  | }                                            |
| 28  | }                                            |
| 29  | }                                            |

3、更新一条数据

| 1   | package com.kuang.lesson03;                |
| --- | ------------------------------------------ |
| 2   |                                            |
| 3   | import com.kuang.lesson02.utils.JdbcUtils; |
| 4   |                                            |
| 5   | import java.sql.Connection;                |
| 6   | import java.sql.PreparedStatement;         |
| 7   | import java.sql.ResultSet;                 |
| 8   |                                            |
| 9   | public class TestUpdate {                  |
| 10  | public static void main(String[] args) {   |
| 11  | Connection conn = null;                    |
| 12  | PreparedStatement st = null;               |
| 13  | ResultSet rs = null;                       |
| 14  | try{                                       |
| 15  | conn = JdbcUtils.getConnection();          |

| 16  |     |     | String sql = "update users set name=?,email=? where id=?";    |
| --- | --- | --- | ------------------------------------------------------------- |
| 17  |     |     | st = conn.prepareStatement(sql);                              |
| 18  |     |     | st.setString(1, "kuangshen");                                 |
| 19  |     |     | st.setString(2, "[24736743@qq.com](mailto:24736743@qq.com)"); |
| 20  |     |     | st.setInt(3, 2);                                              |
| 21  |     |     | int num = st.executeUpdate();                                 |
| 22  |     |     | if(num>0){                                                    |
| 23  |     |     | System.out.println("更新成功！！");                           |
| 24  |     |     | }                                                             |
| 25  |     |     | }catch (Exception e) {                                        |
| 26  |     |     | e.printStackTrace();                                          |
| 27  |     |     |                                                               |
| 28  |     |     | }finally{                                                     |
| 29  |     |     | JdbcUtils.release(conn, st, rs);                              |
| 30  |     |     | }                                                             |
| 31  |     | }   |                                                               |
| 32  | }   |     |                                                               |

4、查询一条数据

| 1   | package com.kuang.lesson03;                     |
| --- | ----------------------------------------------- |
| 2   |                                                 |
| 3   | import com.kuang.lesson02.utils.JdbcUtils;      |
| 4   |                                                 |
| 5   | import java.sql.Connection;                     |
| 6   | import java.sql.PreparedStatement;              |
| 7   | import java.sql.ResultSet;                      |
| 8   |                                                 |
| 9   | public class TestSelect {                       |
| 10  | public static void main(String[] args) {        |
| 11  | Connection conn = null;                         |
| 12  | PreparedStatement st = null;                    |
| 13  | ResultSet rs = null;                            |
| 14  | try{                                            |
| 15  | conn = JdbcUtils.getConnection();               |
| 16  | String sql = "select \* from users where id=?"; |
| 17  | st = conn.prepareStatement(sql);                |
| 18  | st.setInt(1, 1);                                |
| 19  | rs = st.executeQuery();                         |
| 20  | if(rs.next()){                                  |
| 21  | System.out.println(rs.getString("name"));       |
| 22  | }                                               |
| 23  | }catch (Exception e) {                          |
| 24  |                                                 |
| 25  | }finally{                                       |
| 26  | JdbcUtils.release(conn, st, rs);                |
| 27  | }                                               |
| 28  | }                                               |
| 29  | }                                               |

避免 SQL 注入

| 1   | package com.kuang.lesson03;                |
| --- | ------------------------------------------ |
| 2   |                                            |
| 3   | import com.kuang.lesson02.utils.JdbcUtils; |

| 4   |                                                                  |
| --- | ---------------------------------------------------------------- |
| 5   | import java.sql.Connection;                                      |
| 6   | import java.sql.PreparedStatement;                               |
| 7   | import java.sql.ResultSet;                                       |
| 8   | import java.sql.Statement;                                       |
| 9   |                                                                  |
| 10  | public class SQL 注入 {                                          |
| 11  | public static void main(String[] args) {                         |
| 12  | // login("zhangsan","123456"); // 正常登陆                       |
| 13  | login("'or '1=1","123456"); // SQL 注入                          |
| 14  | }                                                                |
| 15  |                                                                  |
| 16  | public static void login(String username,String password){       |
| 17  | Connection conn = null;                                          |
| 18  | PreparedStatement st = null;                                     |
| 19  | ResultSet rs = null;                                             |
| 20  | try{                                                             |
| 21  | conn = JdbcUtils.getConnection();                                |
| 22  | // select \* from users where name='' or '1=1' and password =    |
|     | '123456'                                                         |
| 23  | String sql = "select \* from users where name=? and password=?"; |
| 24  | st = conn.prepareStatement(sql);                                 |
| 25  | st.setString(1,username);                                        |
| 26  | st.setString(2,password);                                        |
| 27  |                                                                  |
| 28  | rs = st.executeQuery();                                          |
| 29  | while(rs.next()){                                                |
| 30  | System.out.println(rs.getString("name"));                        |
| 31  | System.out.println(rs.getString("password"));                    |
| 32  | System.out.println("==============");                            |
| 33  | }                                                                |
| 34  | }catch (Exception e) {                                           |
| 35  | e.printStackTrace();                                             |
| 36  | }finally{                                                        |
| 37  | JdbcUtils.release(conn, st, rs);                                 |
| 38  | }                                                                |
| 39  | }                                                                |
| 40  |                                                                  |
| 41  | }                                                                |

原理：执行的时候参数会用引号包起来，并把参数中的引号作为转义字符，从而避免了参数也作为条件 的一部分

## 、事务

概念

### 事务指逻辑上的一组操作，组成这组操作的各个单元，要不全部成功，要不全部不成功。

ACID 原则
**原子性(Atomic)**
整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执 行过程中发生错误，会被回滚（ROLLBACK）到事务开始前的状态，就像这个事务从来没有执行过

一样。

### 一致性(Consist)

一个事务可以封装状态改变（除非它是一个只读的）。事务必须始终保持系统处于一致的状态，不 管在任何给定的时间并发事务有多少。也就是说：如果事务是并发多个，系统也必须如同串行事务 一样操作。其主要特征是保护性和不变性(Preserving an Invariant)，以转账案例为例，假设有五个账户，每个账户余额是 100 元，那么五个账户总额是 500 元，如果在这个 5 个账户之间同时发生多个转账，无论并发多少个，比如在 A 与 B 账户之间转账 5 元，在 C 与 D 账户之间转账 10 元，在 B 与 E 之间转账 15 元，五个账户总额也应该还是 500 元，这就是保护性和不变性。

### 隔离性(Isolated)

隔离状态执行事务，使它们好像是系统在给定时间内执行的唯一操作。如果有两个事务，运行在相 同的时间内，执行相同的功能，事务的隔离性将确保每一事务在系统中认为只有该事务在使用系 统。这种属性有时称为串行化，为了防止事务操作间的混淆，必须串行化或序列化请求，使得在同 一时间仅有一个请求用于同一数据。

### 持久性(Durable)

在事务完成以后，该事务对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。

隔离性问题
1、脏读：脏读指一个事务读取了另外一个事务未提交的数据。
2、不可重复读：不可重复读指在一个事务内读取表中的某一行数据，多次读取结果不同。
3、虚读(幻读) : 虚读(幻读)是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致。

代码测试

| 1   | /_创建账户表_/                                    |
| --- | ------------------------------------------------- |
| 2   | CREATE TABLE account(                             |
| 3   | id INT PRIMARY KEY AUTO_INCREMENT,                |
| 4   | NAME VARCHAR(40),                                 |
| 5   | money FLOAT                                       |
| 6   | );                                                |
| 7   |                                                   |
| 8   | /_插入测试数据_/                                  |
| 9   | insert into account(name,money) values('A',1000); |
| 10  | insert into account(name,money) values('B',1000); |
| 11  | insert into account(name,money) values('C',1000); |

当 Jdbc 程序向数据库获得一个 Connection 对象时，默认情况下这个 Connection 对象会自动向数据库提交 在它上面发送的 SQL 语句。若想关闭这种默认提交方式，让多条 SQL 在一个事务中执行，可使用下列的 JDBC 控制事务语句
Connection.setAutoCommit(false);//开启事务(start transaction) Connection.rollback();// 回 滚 事 务 (rollback) Connection.commit();//提交事务(commit)

程序编写
1、模拟转账成功时的业务场景

| 1   | package com.kuang.lesson04;                                                 |
| --- | --------------------------------------------------------------------------- |
| 2   |                                                                             |
| 3   | import com.kuang.lesson02.utils.JdbcUtils;                                  |
| 4   |                                                                             |
| 5   | import java.sql.Connection;                                                 |
| 6   | import java.sql.PreparedStatement;                                          |
| 7   | import java.sql.ResultSet;                                                  |
| 8   |                                                                             |
| 9   | //模拟转账成功时的业务场景                                                  |
| 10  | public class TestTransaction1 {                                             |
| 11  | public static void main(String[] args) {                                    |
| 12  | Connection conn = null;                                                     |
| 13  | PreparedStatement st = null;                                                |
| 14  | ResultSet rs = null;                                                        |
| 15  |                                                                             |
| 16  | try{                                                                        |
| 17  | conn = JdbcUtils.getConnection();                                           |
| 18  | conn.setAutoCommit(false);//通知数据库开启事务(start transaction)           |
| 19  |                                                                             |
| 20  | String sql1 = "update account set money=money-100 where                     |
|     | name='A'";                                                                  |
| 21  | st = conn.prepareStatement(sql1);                                           |
| 22  | st.executeUpdate();                                                         |
| 23  |                                                                             |
| 24  | String sql2 = "update account set money=money+100 where                     |
|     | name='B'";                                                                  |
| 25  | st = conn.prepareStatement(sql2);                                           |
| 26  | st.executeUpdate();                                                         |
| 27  |                                                                             |
| 28  | conn.commit();//上面的两条 SQL 执行 Update 语句成功之后就通知数据库提交事务 |
|     | (commit)                                                                    |
| 29  | System.out.println("成功！！！"); //log4j                                   |
| 30  | }catch (Exception e) {                                                      |
| 31  | e.printStackTrace();                                                        |
| 32  | }finally{                                                                   |
| 33  | JdbcUtils.release(conn, st, rs);                                            |
| 34  | }                                                                           |
| 35  | }                                                                           |
| 36  | }                                                                           |

2、模拟转账过程中出现异常导致有一部分 SQL 执行失败后让数据库自动回滚事务

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
package com.kuang.lesson04;
import com.kuang.lesson02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement; import java.sql.ResultSet;

// 模拟转账过程中出现异常导致有一部分 SQL 执行失败后让数据库自动回滚事务
public class TestTransaction2 {
public static void main(String[] args) {

| 12  | Connection conn = null;                                                         |
| --- | ------------------------------------------------------------------------------- |
| 13  | PreparedStatement st = null;                                                    |
| 14  | ResultSet rs = null;                                                            |
| 15  |                                                                                 |
| 16  | try{                                                                            |
| 17  | conn = JdbcUtils.getConnection();                                               |
| 18  | conn.setAutoCommit(false);//通知数据库开启事务(start transaction)               |
| 19  | String sql1 = "update account set money=money-100 where                         |
|     | name='A'";                                                                      |
| 20  | st = conn.prepareStatement(sql1);                                               |
| 21  | st.executeUpdate();                                                             |
| 22  | //用这句代码模拟执行完 SQL1 之后程序出现了异常而导致后面的 SQL 无法正常执行，事 |
|     | 务也无法正常提交，此时数据库会自动执行回滚操作                                  |
| 23  | int x = 1/0;                                                                    |
| 24  | String sql2 = "update account set money=money+100 where                         |
|     | name='B'";                                                                      |
| 25  | st = conn.prepareStatement(sql2);                                               |
| 26  | st.executeUpdate();                                                             |
| 27  | conn.commit();//上面的两条 SQL 执行 Update 语句成功之后就通知数据库提交事务     |
|     | (commit)                                                                        |
| 28  | System.out.println("成功！！！");                                               |
| 29  | }catch (Exception e) {                                                          |
| 30  | e.printStackTrace();                                                            |
| 31  | }finally{                                                                       |
| 32  | JdbcUtils.release(conn, st, rs);                                                |
| 33  | }                                                                               |
| 34  | }                                                                               |
| 35  | }                                                                               |

3、模拟转账过程中出现异常导致有一部分 SQL 执行失败时手动通知数据库回滚事务

| 1   | package com.kuang.lesson04;                                                     |
| --- | ------------------------------------------------------------------------------- |
| 2   |                                                                                 |
| 3   | import com.kuang.lesson02.utils.JdbcUtils;                                      |
| 4   |                                                                                 |
| 5   | import java.sql.Connection;                                                     |
| 6   | import java.sql.PreparedStatement;                                              |
| 7   | import java.sql.ResultSet;                                                      |
| 8   | import java.sql.SQLException;                                                   |
| 9   |                                                                                 |
| 10  | //模拟转账过程中出现异常导致有一部分 SQL 执行失败时手动通知数据库回滚事务       |
| 11  | public class TestTransaction3 {                                                 |
| 12  | public static void main(String[] args) {                                        |
| 13  | Connection conn = null;                                                         |
| 14  | PreparedStatement st = null;                                                    |
| 15  | ResultSet rs = null;                                                            |
| 16  |                                                                                 |
| 17  | try{                                                                            |
| 18  | conn = JdbcUtils.getConnection();                                               |
| 19  | conn.setAutoCommit(false);//通知数据库开启事务(start transaction)               |
| 20  |                                                                                 |
| 21  | String sql1 = "update account set money=money-100 where                         |
|     | name='A'";                                                                      |
| 22  | st = conn.prepareStatement(sql1);                                               |
| 23  | st.executeUpdate();                                                             |
| 24  | //用这句代码模拟执行完 SQL1 之后程序出现了异常而导致后面的 SQL 无法正常执行，事 |
|     | 务也无法正常提交                                                                |
| 25  | int x = 1/0;                                                                    |

}
}
System.out.println("成功！！！");
}catch (Exception e) { try {
//捕获到异常之后手动通知数据库执行回滚事务的操作
conn.rollback();
} catch (SQLException e1) { e1.printStackTrace();
}
e.printStackTrace();
}finally{
JdbcUtils.release(conn, st, rs);
}
(commit)
st = conn.prepareStatement(sql2); st.executeUpdate();
conn.commit();//上面的两条 SQL 执行 Update 语句成功之后就通知数据库提交事务
name='B'";
String sql2 = "update account set money=money+100 where
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
40
41
42
43

## 、数据库连接池

用户每次请求都需要向数据库获得链接，而数据库创建连接通常需要消耗相对较大的资源，创建时间也 较长。假设网站一天 10 万访问量，数据库服务器就需要创建 10 万次连接，极大的浪费数据库的资源，并且极易造成数据库服务器内存溢出、拓机。
数据库连接池的基本概念
数据库连接是一种关键的有限的昂贵的资源,这一点在多用户的网页应用程序中体现的尤为突出.对数据库 连接的管理能显著影响到整个应用程序的伸缩性和健壮性,影响到程序的性能指标.数据库连接池正式针对 这个问题提出来的.**数据库连接池负责分配,管理和释放数据库连接,它允许应用程序重复使用一个现有的 数据库连接,而不是重新建立一个**。
数据库连接池在初始化时将创建一定数量的数据库连接放到连接池中, 这些数据库连接的数量是由最小数据库连接数来设定的.无论这些数据库连接是否被使用,连接池都将一直保证至少拥有这么多的连接数量. 连接池的最大数据库连接数量限定了这个连接池能占有的最大连接数,当应用程序向连接池请求的连接数 超过最大连接数量时,这些请求将被加入到等待队列中.
数据库连接池的最小连接数和最大连接数的设置要考虑到以下几个因素:

      1. 最小连接数:是连接池一直保持的数据库连接,所以如果应用程序对数据库连接的使用量不大,将会有 大量的数据库连接资源被浪费.
      1. 最大连接数:是连接池能申请的最大连接数,如果数据库连接请求超过次数,后面的数据库连接请求将 被加入到等待队列中,这会影响以后的数据库操作
      1. 如果最小连接数与最大连接数相差很大:那么最先连接请求将会获利,之后超过最小连接数量的连接 请求等价于建立一个新的数据库连接.不过,这些大于最小连接数的数据库连接在使用完不会马上被 释放,他将被放到连接池中等待重复使用或是空间超时后被释放.

### 编写连接池需实现 java.sql.DataSource 接口。

开源数据库连接池
现在很多 WEB 服务器(Weblogic, WebSphere, Tomcat)都提供了 DataSoruce 的实现，即连接池的实现。

### 通常我们把 DataSource 的实现，按其英文含义称之为数据源，数据源中都包含了数据库连接池的实 现。

也有一些开源组织提供了数据源的独立实现：
DBCP 数据库连接池
C3P0 数据库连接池
在使用了数据库连接池之后，在项目的实际开发中就不需要编写连接数据库的代码了，直接从数据源获 得数据库的连接。

DBCP 数据源
DBCP 是 Apache 软件基金组织下的开源连接池实现，要使用 DBCP 数据源，需要应用程序应在系统中增加如下两个 jar 文件：
Commons-dbcp.jar：连接池的实现 Commons-pool.jar：连接池实现的依赖库
Tomcat 的连接池正是采用该连接池来实现的。该数据库连接池既可以与应用服务器整合使用，也可由应用程序独立使用。
测试：
1、导入相关 jar 包
2、在类目录下加入 dbcp 的配置文件：dbcpconﬁg.properties

| 1   | #连接设置                                                                    |
| --- | ---------------------------------------------------------------------------- |
| 2   | driverClassName=com.mysql.jdbc.Driver                                        |
| 3   | url=jdbc:mysql://localhost:3306/jdbcStudy?                                   |
|     | useUnicode=true&characterEncoding=utf8&useSSL=true                           |
| 4   | username=root                                                                |
| 5   | password=123456                                                              |
| 6   |                                                                              |
| 7   | #<!-- 初始化连接 -->                                                         |
| 8   | initialSize=10                                                               |
| 9   |                                                                              |
| 10  | #最大连接数量                                                                |
| 11  | maxActive=50                                                                 |
| 12  |                                                                              |
| 13  | #<!-- 最大空闲连接 -->                                                       |
| 14  | maxIdle=20                                                                   |
| 15  |                                                                              |
| 16  | #<!-- 最小空闲连接 -->                                                       |
| 17  | minIdle=5                                                                    |
| 18  |                                                                              |
| 19  | #<!-- 超时等待时间以毫秒为单位 6000毫秒/1000等于60秒 -->                     |
| 20  | maxWait=60000                                                                |
| 21  |                                                                              |
| 22  |                                                                              |
| 23  | #JDBC 驱动建立连接时附带的连接属性属性的格式必须为这样：[属性名=property;]   |
| 24  | #注意："user" 与 "password" 两个属性会被明确地传递，因此这里不需要包含他们。 |

| 25  | connectionProperties=useUnicode=true;characterEncoding=UTF8                        |
| --- | ---------------------------------------------------------------------------------- |
| 26  |                                                                                    |
| 27  | #指定由连接池所创建的连接的自动提交（auto-commit）状态。                           |
| 28  | defaultAutoCommit=true                                                             |
| 29  |                                                                                    |
| 30  | #driver default 指定由连接池所创建的连接的只读（read-only）状态。                  |
| 31  | #如果没有设置该值，则“setReadOnly”方法将不被调用。（某些驱动并不支持只读模式，如： |
|     | Informix）                                                                         |
| 32  | defaultReadOnly=                                                                   |
| 33  |                                                                                    |
| 34  | #driver default 指定由连接池所创建的连接的事务级别（TransactionIsolation）。       |
| 35  | #可用值为下列之一：（详情可见 javadoc。）NONE,READ_UNCOMMITTED, READ_COMMITTED,    |
|     | REPEATABLE_READ, SERIALIZABLE                                                      |
| 36  | defaultTransactionIsolation=READ_UNCOMMITTED                                       |

3、编写工具类 JdbcUtils_DBCP

| 1   | package com.kuang.datasource.utils;                                                  |
| --- | ------------------------------------------------------------------------------------ |
| 2   |                                                                                      |
| 3   | import java.io.InputStream;                                                          |
| 4   | import java.sql.Connection;                                                          |
| 5   | import java.sql.ResultSet;                                                           |
| 6   | import java.sql.SQLException;                                                        |
| 7   | import java.sql.Statement;                                                           |
| 8   | import java.util.Properties;                                                         |
| 9   | import javax.sql.DataSource;                                                         |
| 10  | import org.apache.commons.dbcp.BasicDataSourceFactory;                               |
| 11  |                                                                                      |
| 12  | //数据库连接工具类                                                                   |
| 13  | public class JdbcUtils_DBCP {                                                        |
| 14  | /\*\*                                                                                |
| 15  | \* 在 java 中，编写数据库连接池需实现 java.sql.DataSource 接口，每一种数据库连接池都 |
|     | 是 DataSource 接口的实现                                                             |
| 16  | \* DBCP 连接池就是 java.sql.DataSource 接口的一个具体实现                            |
| 17  | \*/                                                                                  |
| 18  | private static DataSource ds = null;                                                 |
| 19  | //在静态代码块中创建数据库连接池                                                     |
| 20  | static{                                                                              |
| 21  | try{                                                                                 |
| 22  | //加载 dbcpconfig.properties 配置文件                                                |
| 23  | InputStream in =                                                                     |
|     | JdbcUtils_DBCP.class.getClassLoader().getResourceAsStream("dbcpconfig.proper         |
|     | ties");                                                                              |
| 24  | Properties prop = new Properties();                                                  |
| 25  | prop.load(in);                                                                       |
| 26  | //创建数据源                                                                         |
| 27  | ds = BasicDataSourceFactory.createDataSource(prop);                                  |
| 28  | }catch (Exception e) {                                                               |
| 29  | throw new ExceptionInInitializerError(e);                                            |
| 30  | }                                                                                    |
| 31  | }                                                                                    |
| 32  |                                                                                      |
| 33  | //从数据源中获取数据库连接                                                           |
| 34  | public static Connection getConnection() throws SQLException{                        |
| 35  | //从数据源中获取数据库连接                                                           |
| 36  | return ds.getConnection();                                                           |
| 37  | }                                                                                    |
| 38  |                                                                                      |

| 39  |     | // 释放资源                                                            |
| --- | --- | ---------------------------------------------------------------------- |
| 40  |     | public static void release(Connection conn,Statement st,ResultSet rs){ |
| 41  |     | if(rs!=null){                                                          |
| 42  |     | try{                                                                   |
| 43  |     | //关闭存储查询结果的 ResultSet 对象                                    |
| 44  |     | rs.close();                                                            |
| 45  |     | }catch (Exception e) {                                                 |
| 46  |     | e.printStackTrace();                                                   |
| 47  |     | }                                                                      |
| 48  |     | rs = null;                                                             |
| 49  |     | }                                                                      |
| 50  |     | if(st!=null){                                                          |
| 51  |     | try{                                                                   |
| 52  |     | //关闭负责执行 SQL 命令的 Statement 对象                               |
| 53  |     | st.close();                                                            |
| 54  |     | }catch (Exception e) {                                                 |
| 55  |     | e.printStackTrace();                                                   |
| 56  |     | }                                                                      |
| 57  |     | }                                                                      |
| 58  |     |                                                                        |
| 59  |     | if(conn!=null){                                                        |
| 60  |     | try{                                                                   |
| 61  |     | //将 Connection 连接对象还给数据库连接池                               |
| 62  |     | conn.close();                                                          |
| 63  |     | }catch (Exception e) {                                                 |
| 64  |     | e.printStackTrace();                                                   |
| 65  |     | }                                                                      |
| 66  |     | }                                                                      |
| 67  |     | }                                                                      |
| 68  | }   |                                                                        |

测试类

| 1   | package com.kuang.datasource;                                                               |
| --- | ------------------------------------------------------------------------------------------- |
| 2   |                                                                                             |
| 3   | import com.kuang.datasource.utils.JdbcUtils_DBCP;                                           |
| 4   |                                                                                             |
| 5   | import java.sql.Connection;                                                                 |
| 6   | import java.sql.PreparedStatement;                                                          |
| 7   | import java.sql.ResultSet;                                                                  |
| 8   | import java.util.Date;                                                                      |
| 9   |                                                                                             |
| 10  | public class DBCPTest {                                                                     |
| 11  | public static void main(String[] args) {                                                    |
| 12  | Connection conn = null;                                                                     |
| 13  | PreparedStatement st = null;                                                                |
| 14  | ResultSet rs = null;                                                                        |
| 15  | try{                                                                                        |
| 16  | //获取数据库连接                                                                            |
| 17  | conn = JdbcUtils_DBCP.getConnection();                                                      |
| 18  | String sql = "insert into users(id,name,password,email,birthday)                            |
|     | values(?,?,?,?,?)";                                                                         |
| 19  | st = conn.prepareStatement(sql);                                                            |
| 20  | st.setInt(1, 5);//id 是 int 类型的                                                          |
| 21  | st.setString(2, "kuangshen");//name 是 varchar(字符串类型)                                  |
| 22  | st.setString(3, "123");//password 是 varchar(字符串类型)                                    |
| 23  | st.setString(4, "[24736743@qq.com](mailto:24736743@qq.com)");//email 是 varchar(字符串类型) |

| 24  | st.setDate(5, new java.sql.Date(new        |
| --- | ------------------------------------------ |
|     | Date().getTime()));//birthday 是 date 类型 |
| 25  |                                            |
| 26  | int i = st.executeUpdate();                |
| 27  | if (i>0){                                  |
| 28  | System.out.println("插入成功");            |
| 29  | }                                          |
| 30  | }catch (Exception e) {                     |
| 31  | e.printStackTrace();                       |
| 32  | }finally{                                  |
| 33  | //释放资源                                 |
| 34  | JdbcUtils_DBCP.release(conn, st, rs);      |
| 35  | }                                          |
| 36  | }                                          |
| 37  | }                                          |

C3P0
C3P0 是一个开源的 JDBC 连接池，它实现了数据源和 JNDI 绑定，支持 JDBC3 规范和 JDBC2 的标准扩展。目前使用它的开源项目有**_Hibernate_**，**_Spring_**等。C3P0 数据源在项目开发中使用得比较多。

### c3p0 与 dbcp 区别

dbcp 没有自动回收空闲连接的功能 c3p0 有自动回收空闲连接功能

测试
1、导入相关 jar 包
2、在类目录下加入 C3P0 的配置文件：c3p0-conﬁg.xml

| 1   | <?xml version="1.0" encoding="UTF-8"?>                                      |
| --- | --------------------------------------------------------------------------- |
| 2   |                                                                             |
| 3   | <c3p0-config>                                                               |
| 4   | <!--                                                                        |
| 5   | C3P0 的缺省(默认)配置，                                                     |
| 6   | 如果在代码中“ComboPooledDataSource ds = new ComboPooledDataSource();”这样写 |
|     | 就表示使用的是 C3P0 的缺省(默认)配置信息来创建数据源                        |
| 7   | -->                                                                         |
| 8   | <default-config>                                                            |
| 9   | <property name="driverClass">com.mysql.jdbc.Driver</property>               |
| 10  | <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcStudy?             |
|     | useUnicode=true&characterEncoding=utf8&useSSL=true</property>               |
| 11  | <property name="user">root</property>                                       |
| 12  | <property name="password">123456</property>                                 |
| 13  |                                                                             |
| 14  | <property name="acquireIncrement">5</property>                              |
| 15  | <property name="initialPoolSize">10</property>                              |
| 16  | <property name="minPoolSize">5</property>                                   |
| 17  | <property name="maxPoolSize">20</property>                                  |
| 18  | </default-config>                                                           |
| 19  |                                                                             |
| 20  | <!--                                                                        |
| 21  | C3P0 的命名配置，                                                           |

| 22  | 如果在代码中“ComboPooledDataSource ds = new                                             |
| --- | --------------------------------------------------------------------------------------- |
|     | ComboPooledDataSource("MySQL");”这样写就表示使用的是 name 是 MySQL 的配置信息来创建数据 |
|     | 源                                                                                      |
| 23  | -->                                                                                     |
| 24  | <named-config name="MySQL">                                                             |
| 25  | <property name="driverClass">com.mysql.jdbc.Driver</property>                           |
| 26  | <property name="jdbcUrl">jdbc:mysql://localhost:3306/jdbcStudy?                         |
|     | useUnicode=true&characterEncoding=utf8&useSSL=true</property>                           |
| 27  | <property name="user">root</property>                                                   |
| 28  | <property name="password">123456</property>                                             |
| 29  |                                                                                         |
| 30  | <property name="acquireIncrement">5</property>                                          |
| 31  | <property name="initialPoolSize">10</property>                                          |
| 32  | <property name="minPoolSize">5</property>                                               |
| 33  | <property name="maxPoolSize">20</property>                                              |
| 34  | </named-config>                                                                         |
| 35  |                                                                                         |
| 36  | </c3p0-config>                                                                          |

3、创建工具类

| 1   | package com.kuang.datasource.utils;                                             |
| --- | ------------------------------------------------------------------------------- |
| 2   |                                                                                 |
| 3   | import java.sql.Connection;                                                     |
| 4   | import java.sql.ResultSet;                                                      |
| 5   | import java.sql.SQLException;                                                   |
| 6   | import java.sql.Statement;                                                      |
| 7   | import com.mchange.v2.c3p0.ComboPooledDataSource;                               |
| 8   |                                                                                 |
| 9   | //数据库连接工具类                                                              |
| 10  | public class JdbcUtils_C3P0 {                                                   |
| 11  |                                                                                 |
| 12  | private static ComboPooledDataSource ds = null;                                 |
| 13  | //在静态代码块中创建数据库连接池                                                |
| 14  | static{                                                                         |
| 15  | try{                                                                            |
| 16  | //通过代码创建 C3P0 数据库连接池                                                |
| 17  | /\*ds = new ComboPooledDataSource();                                            |
| 18  | ds.setDriverClass("com.mysql.jdbc.Driver");                                     |
| 19  | ds.setJdbcUrl("jdbc:mysql://localhost:3306/jdbcstudy");                         |
| 20  | ds.setUser("root");                                                             |
| 21  | ds.setPassword("123456");                                                       |
| 22  | ds.setInitialPoolSize(10);                                                      |
| 23  | ds.setMinPoolSize(5);                                                           |
| 24  | ds.setMaxPoolSize(20);\*/                                                       |
| 25  |                                                                                 |
| 26  | //通过读取 C3P0 的 xml 配置文件创建数据源，C3P0 的 xml 配置文件 c3p0-config.xml |
|     | 必须放在 src 目录下                                                             |
| 27  | //ds = new ComboPooledDataSource();//使用 C3P0 的默认配置来创建数据源           |
| 28  | ds = new ComboPooledDataSource("MySQL");//使用 C3P0 的命名配置来创建数          |
|     | 据源                                                                            |
| 29  |                                                                                 |
| 30  | }catch (Exception e) {                                                          |
| 31  | throw new ExceptionInInitializerError(e);                                       |
| 32  | }                                                                               |
| 33  | }                                                                               |
| 34  |                                                                                 |
| 35  | //从数据源中获取数据库连接                                                      |

| 36  |     | public static Connection getConnection() throws SQLException{          |
| --- | --- | ---------------------------------------------------------------------- |
| 37  |     | //从数据源中获取数据库连接                                             |
| 38  |     | return ds.getConnection();                                             |
| 39  |     | }                                                                      |
| 40  |     |                                                                        |
| 41  |     | //释放资源                                                             |
| 42  |     | public static void release(Connection conn,Statement st,ResultSet rs){ |
| 43  |     | if(rs!=null){                                                          |
| 44  |     | try{                                                                   |
| 45  |     | //关闭存储查询结果的 ResultSet 对象                                    |
| 46  |     | rs.close();                                                            |
| 47  |     | }catch (Exception e) {                                                 |
| 48  |     | e.printStackTrace();                                                   |
| 49  |     | }                                                                      |
| 50  |     | rs = null;                                                             |
| 51  |     | }                                                                      |
| 52  |     | if(st!=null){                                                          |
| 53  |     | try{                                                                   |
| 54  |     | //关闭负责执行 SQL 命令的 Statement 对象                               |
| 55  |     | st.close();                                                            |
| 56  |     | }catch (Exception e) {                                                 |
| 57  |     | e.printStackTrace();                                                   |
| 58  |     | }                                                                      |
| 59  |     | }                                                                      |
| 60  |     |                                                                        |
| 61  |     | if(conn!=null){                                                        |
| 62  |     | try{                                                                   |
| 63  |     | //将 Connection 连接对象还给数据库连接池                               |
| 64  |     | conn.close();                                                          |
| 65  |     | }catch (Exception e) {                                                 |
| 66  |     | e.printStackTrace();                                                   |
| 67  |     | }                                                                      |
| 68  |     | }                                                                      |
| 69  |     | }                                                                      |
| 70  | }   |                                                                        |

测试

| 1   | package com.kuang.datasource;                                    |
| --- | ---------------------------------------------------------------- |
| 2   |                                                                  |
| 3   | import com.kuang.datasource.utils.JdbcUtils_C3P0;                |
| 4   | import com.kuang.datasource.utils.JdbcUtils_DBCP;                |
| 5   |                                                                  |
| 6   | import java.sql.Connection;                                      |
| 7   | import java.sql.PreparedStatement;                               |
| 8   | import java.sql.ResultSet;                                       |
| 9   | import java.util.Date;                                           |
| 10  |                                                                  |
| 11  | public class C3P0Test {                                          |
| 12  | public static void main(String[] args) {                         |
| 13  | Connection conn = null;                                          |
| 14  | PreparedStatement st = null;                                     |
| 15  | ResultSet rs = null;                                             |
| 16  | try{                                                             |
| 17  | //获取数据库连接                                                 |
| 18  | conn = JdbcUtils_C3P0.getConnection();                           |
| 19  | String sql = "insert into users(id,name,password,email,birthday) |
|     | values(?,?,?,?,?)";                                              |

1.  st = conn.prepareStatement(sql);
1.  st.setInt(1, 6);//id 是 int 类型的
1.  st.setString(2, "kuangshen");//name 是 varchar(字符串类型)
1.  st.setString(3, "123");//password 是 varchar(字符串类型)
1.  st.setString(4, "[24736743@qq.com](mailto:24736743@qq.com)");//email 是 varchar(字符串类型)
1.      st.setDate(5, new java.sql.Date(new Date().getTime()));//birthday是date类型

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
int i = st.executeUpdate(); if (i>0){
System.out.println("插入成功");
}
}catch (Exception e) { e.printStackTrace();
}finally{
// 释 放 资 源 JdbcUtils_C3P0.release(conn, st, rs);
}
}
}
