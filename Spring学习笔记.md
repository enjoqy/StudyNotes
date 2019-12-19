# Spring学习笔记

## 1、Spring几大核心

- IOC/DI  控制反转/依赖注入
- AOP  面向切面编程
- 声明式事务

## 2、Spring框架中重要概念

### 1、核心架包

- Core Container：核心容器，Spring启动的最基本条件
  - Beans：Spring负责创建类对象并管理对象
  - Core：核心类
  - Content：上下文参数，获取外部资源或者管理注解等
  - SpEl：expression.jar
- AOP：实现aop功能需要依赖
- Aspects：切面AOP依赖包
- Data Access/Integration：Spring封装数据访问层的相关内容
  - JDBC：Spring对JDBC封装后的代码
  - ORM：封装了持久层框架的代码。例如：Hibernate
  - transactions：对应spring-tx.jar，声明式事务使用
- WEB：需要Spring完成web相关功能时需要
  - 例如：由tomcat加载spring配置文件时，需要spring-web包
- 容器（Container）：Spring当作一个大容器
- BeanFactory接口：老版本（Spring2之前）
  - 新版本中ApplicationContent接口，是BeanFactory子接口，BeanFactory中的功能ApplicationContent都有

## 3、IOC

- 控制翻转，Inversion of Control
- IOC是什么：
  - Ioc完成的事情原先由程序员主动new实例化对象事情，转交给spring负责
  - 控制反转中控制指的是：控制类的对象
  - 控制反转中反转指的是：转交给spring负责
  - Ioc最大的作用：
    - 解耦
      - 程序员不需要管理对象，解除了对象管理和程序员之间的耦合

## 4、搭建环境

- 导jar(4个核心包 + 1个日志包)
- 在src下建applicationContent.xml
  - 文件名称和路径自定义
  
  - 记住Spring的容器ApplicationContent，applicationContent.xml配置的信息最终存储到了ApplicationContent容器中
  
  - spring配置文件是基于schema
    - schema文件扩展名.xsd
    - 把schema理解成DTD升级版
    
  - 每次引入一个xsd文件是一个namespace（xmlns）
- 配置文件只需要引入基本的schema
  - 通过<bean/>创建对象
  - 默认文件被加载时创建对象

```java
//获取Spring容器
ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContent.xml");
//从容器中获取指定对象
User userBean = ac.getBean("us", User.class);
//从容器中获取所有对象
String[] names = ac.getBeanDefinitionNames();
```

## 5、创建对象的几种方式

- 1、通过构造方法创建
  - 无参构造创建：默认情况
  - 有参构造创建：需要明确配置
    - 需要在类中提供有参构造方法
    - 在applicationContent.xml中设置调用哪个构造方法创建对象
      - 如果设定的条件匹配多个构造方法，执行最后一个
      - index：参数的索引，从0开始
      - name：参数名
      - type：类型（区分开关键字和封装类int和Integer）
- 实例工厂
  - 工厂设计模式：帮助创建类对象，一个 工厂可以创建多个对象
  - 实例工厂：需要先创建工厂，才能生产对象
- 静态工厂

## 6、DI

- DI：中文名称：依赖注入
- 英文名称：Dependency Injection
- DI：是什么？
  - DI和IOC是一样的
  - 当一个类（A）中需要依赖另一个类（B）对象时，把B赋值给A的过程就叫做依赖注入

## 7、Spring配置AOP切入点execution详解

**例： execution (\* com.sample.service..\*. \*(..))**

整个表达式可以分为五个部分：

1、execution():：表达式主体。

2、第一个*号：表示返回类型， *号表示所有的类型。

3、包名：表示需要拦截的包名，后面的两个句点表示当前包和当前包的所有子包，com.sample.service包、子孙包下所有类的方法。

4、第二个*号：表示类名，*号表示所有的类。

5、***(..)**：最后这个星号表示方法名，*号表示所有的方法，后面括弧里面表示方法的参数，两个句点表示任何参数







































