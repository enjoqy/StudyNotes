# 														mybatis





## 1、基础知识（重点、内容多）

### 1、对原生态jdbc程序（单独使用jdbc开发）问题总结

#### 1、环境

- java环境：1.8
- idea：2019.1.3
- mysql：5.7

#### 2、jdbc程序

- 使用jdbc查询mysql数据库中用户表的记录
- sql_data.sql:  记录表结构
- sql_table_sql:  记录测试数据，在实际开发中，最后提供一个初始化数据脚本  

#### 3、问题总结

- 数据库连接，使用时创建，不使用就立即释放，对象数据库进行频繁的连接开启和关闭，造成数据库资源的浪费，影响数据库的性能
  - 解决方案：使用数据库连接池管理数据库连接
- 将sql语句硬编码到java代码中，如果sql语句修改，需要重新编译java代码，不利于系统维护。
  - 解决方案：将sql语句配置在xml配置文件中，即使sql变化，不需要对java进行重新编译。
- 向preparedStatement中设置参数，对占位符号位置和设置参数值，硬编码在java代码中，不利于系统维护。
  - 解决方案：将sql语句及占位符和参数全部配置在xml文件中。
- 从resutSet中遍历结果集数据时，存在硬编码，将获取表的字段进行硬编码，不利于系统维护。
  - 解决方案：将查询结果集，自动映射成java对象。

### 2、mybatis框架原理（掌握）

#### 2.1mybatis是什么

- mybatis是一个持久层框架，是apache下的顶级项目。
- mybatis托管在googlecode下，再后来托管在github下
- mybatis让程序员将主要精力放在sql上，通过mybatis提供的映射的方式，自由灵活生成（半自动化，大部分需要程序员自动编写sql）满足需要sql语句
- mybatis可以将向preparedStatement中的输入参数自动进行输入映射，将查询结果集灵活的映射成java对象（输出映射）

```java
SqlMapConfig.xml(是mybatis的全局配置文件)，
配置了数据源、事务等mybatis运行环境，
配置映射文件（配置sql语句）
mapper.xml(映射文件)、mapper.xml、。。。。
```

```java
SqlSessionFactory(会话工厂)
	作用：创建SqlSession
```

```java
SqlSession(会话)，是一个接口，面向用户（程序员）的接口
	作用：操作数据库（发出sql增、删、改、查）
```

```java
Executor(执行器)，是一个接口（基本执行器、缓存执行器）
	作用：SqlSessison内部通过执行器操作数据库
```

```java
mapped statement(底层封装对象)
	作用：对操作数据库存储封装，包括sql语句，输入参数，输出结果类型
```

```sql
mysql
```

```java
输入参数类型：
	java简单类型
	hashmap
	pojo自定义
```

```java
输出结果类型：
	java简单类型
	hashmap
	pojo自定义
```









### 3、mybatis入门程序

### 4、用户的增、删、改、查

### 5、mybatis开发dao两种方法

- 原始dao开发方法（程序需要编写dao接口和dao实现类）（掌握）

- mybatis的mapper接口（相当于dao接口）代理开发方法（掌握）

### 6、mybatis配置文件SqlMapConfig.xml

### 7、mybatis核心：

- mybatis输入映射（掌握）
- mybatis输出映射（掌握）

### 8、mybatis动态sql（掌握）

## 2、高级知识

### 1、高级结果集映射（一对一、一对多、多对多）

### 2、mybatis延迟加载

### 3、mybatis查询缓存（一级缓存、二级缓存）

### 4、mybatis和spring进行整合（掌握）

### 5、mybatis逆向工程





# mybatis 2.0

## 1、什么是框架

- 他是我们软件开发中一套解决方案，不同的框架解决的是不同的问题
- 使用框架的好处：
  - 框架封装了很多细节，使开发者可以使用极简的方式实现功能，大大提高开发效率

## 2、三层架构

- 表现层
  - 用于展示数据
- 业务层
  - 是处理业务需求
- 持久层
  - 是和数据库交互的

## 3、持久层技术解决方案

- JDBC技术：
  - Connection
  - PrepareStatement
  - ResultSet
- Spring的JdbcTemplate
  - Spring中对jdbc的简单封装
- Apache的DBUtiles：
  - 和Spring的jdbcTemplate很像，也是对jdbc的简单封装
- 以上这些都不是框架
  - JDBC是规范
  - Spring的jdbcTemplate和Apache的DBUtiles都只是工具类

## 4、mybatis的概述

- mybatis是一个持久层框架，用java编写的
- 它封装了jdbc操作的很多细节，使开发者只需要关注sql语句本身，而无需关注注册驱动，创建连接等复杂过程
- 它使用ORM思想实现了结果集的封装
  - ORM：
    - Object Relational Mapping 对象关系映射
    - 简单的说：
      - 就是把数据库表和实体类及实体类的属性对应起来
      - 让我们可以操作实体类就实现操作数据库表

## 5、mybatis入门

### 1、mybatis的环境搭建

- 第一步：创建maven工程并导入坐标
- 第二步：创建实体类和dao接口
- 第三步：创建mybatis的主配置文件
  - SqlMapConfig.xml
- 第四步：创建映射文件
  - IUserDao.xml

### 2、环境搭建的注意事项：

- 第一个：
  - 创建IUserDao.xml和IuserDao.java时名称是为了和我们之前的知识保持一致。
  - 在mybatis中它把持久层的接口名称和映射文件也叫做：Mapper。
  - 所以IuserDao和IUserMapper是一样的
- 第二个：
  - 在idea创建目录时，他和包是不一样的，
  - 包在创建时：org.junhi.dao他是三级结构
  - 目录在创建时：org.junhi.dao是一级目录（一个文件夹）
- 第三个：
  - mybatis的映射配置文件位置必须和到接口的包结构相同
- 第四个：
  - 映射配置文件的mapper标签namespace属性的取值必须是dao接口的全限定类名
- 第五个：
  - 映射配置文件的操作配置（select），id属性的取值必须是dao接口的方法名
-  
- 当我们遵从了第三、四、五点之后，我们在开发中就无须在写dao的实现类

### 3、读取配置文件的路径问题：

- 建议使用：
  - 使用类加载器，它只能读取类路径的配置文件
  - 使用ServletContext对象的getRelPath（）读取项目所在的真实路径

- 创建SqlSessionFactory工厂，mybatis使用了构建者模式
  - 优势：把对象的创建细节隐藏，使用者直接调用方法即可拿到对象
- 生产SqlSession使用了工厂模式，
  - 优势：解耦（降低类之间的依赖关系）
- 创建dao接口实现类使用了代理模式，
  - 优势：不修改源码的基础上对已有方法进行增强

### 4、mybatis的入门案列

- 第一步：读取配置文件
- 第二步：创建SQLSessionFactory工厂
- 第三步：创建SqlSession
- 第四步：创建Dao接口的代理对象
- 第五步：执行dao中方法
- 第六步：释放资源
-  
- 注意事项：
  - 不要忘记在映射配置中告知mybatis要封装到哪个实体类
  - 配置的方式：指定实体类的全限定类名
- mybatis基于注解的入门案列
  - 把IuserDao.xml移除，在dao接口的方法上使用@Select注解，并且指定sql语句
  - 同时需要在SqlMapConfig.xml中的mapper配置时，使用class属性指定dao接口的全限定类名
- 明确：
  - 我们在实际开发中，都是越简单越好，所以都是采用不写dao实现类的方式
  - 不管使用xml还是注解配置
  - 但是Mybatis它是支持写dao实现类的

### 5、OGNL表达式

- Object Graphic Navigation Language
-   对象        图           导航               语言

- 它是通过对象的取值方法来获取数据，在写法上把get给省略了。
- 比如：我们获取用户名称：
  - 类中的写法：user.getUsername();
  - OGNL表达式的写法：user.username



# 尚学堂MyBatis笔记

## 1、命名规范

- 项目名：没有要求，不起中文
- 包：公司域名倒写
- 数据访问层：dao, persist, mapper
- 实体类：entity， model，bean，javabean，pojo
- 业务逻辑：service，biz
- 控制器：controller，servlet，action，web
- 过滤器：filter
- 异常：exception
- 监听器：listener
- 注释：
  - 类和方法上使用文档注释/**  */
  - 在方法里面使用/*  */ 或 //
- 类：大驼峰
- 方法属性：小驼峰

## 2 、MVC开发模式

- M：Model 模型，实体类和业务和dao
-  V：View 视图，jsp，html
- C：Controller 控制器，servlet
  - 作用：视图和逻辑分离
- MVC使用场景：大型项目开发
- 开发顺序
  - 设计数据库-》实体类-》持久层-》业务逻辑-》控制器-》视图



























































