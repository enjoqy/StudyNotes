# 1、SpringBoot介绍

- 目标：简单、快速开发Spring框架项目
- 特性1：自动配置（无需xml配置，通过jar依赖自动识别）
- 特性2：通过启步（Starter）依赖集成第三方库，开箱即用
- 特性3：内嵌Servlet容器，无需war包部署
- 特性4：内置健康监测，度量指标功能
- 特性5：提供all-in-one打包插件

# 2、内嵌容器

- 减少外部容器依赖，可移植性高
- 易测试已部署
- Spring Boot提供了可插拔的内嵌容器方案

# 3、整合mybatis步骤

## 1、通过xml配置

### 	1、dataSource配置

### 	2、创建SqlSessionFactoryBean，MapperScannerConfigurer bean

### 	3、创建mybatis配置文件，mapper接口类及sql映射文件

## 2、通过Starter

### 	1、引入mybatis Starter

### 	2、application.properties添加spring mybatis和datasource配置

### 	3、创建mybatis配置文件，mapper接口类及sql映射文件

## 3、Druid

- Druid是java语言中最好的数据库连接池。Druid能够提供强大的监控和扩展功能
- Druid首先是一个数据库连接池。Druid是目前最好的数据库连接池，在功能、性能、扩展性方面，都超过其他数据库连接池，包括DBCP、C3P0、BoneCP、Proxool、JBoss DataSource。Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。Druid是阿里巴巴开发的号称为监控而生的数据库连接池！
-  **Druid是一个JDBC组件，它包括三个部分：**
  -  **Druid是一个JDBC组件，它包括三个部分：**
  -  **Druid是一个JDBC组件，它包括三个部分：**
  -  **Druid是一个JDBC组件，它包括三个部分：**
- **Druid的功能**
  - 1、替换DBCP和C3P0。Druid提供了一个高效、功能强大、可扩展性好的数据库连接池。
  - 2、可以监控数据库访问性能，Druid内置提供了一个功能强大的StatFilter插件，能够详细统计SQL的执行性能，这对于线上分析数据库访问性能有帮助。
  - 3、数据库密码加密。直接把数据库密码写在配置文件中，这是不好的行为，容易导致安全问题。DruidDruiver和DruidDataSource都支持PasswordCallback。
  - 4、SQL执行日志，Druid提供了不同的LogFilter，能够支持Common-Logging、Log4j和JdkLog，你可以按需要选择相应的LogFilter，监控你应用的数据库访问情况。
  - 5、扩展JDBC，如果你要对JDBC层有编程的需求，可以通过Druid提供的Filter机制，很方便编写JDBC层的扩展插件。
- 所以Druid可以：
  1、充当数据库连接池。
  2、可以监控数据库访问性能
  3、获得SQL执行日志
# 4、整合freemarker步骤

## 1、通过xml配置

- pom引入freemarker库
- 创建freemarkerConfigurer、viewResolver bean
- 编写模板引擎文件

## 2、通过Starter

- 引入freemarker starter
- application.properties添加freemarker配置 
- 编写模板引擎文件

## 3、Freemarker的结构化布局

- 抽取header、footer、nav、js、分页
- 页面中引入header、footer
- 编写页面中自定义的部分













