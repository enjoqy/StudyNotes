# 一、Spring Boot入门，谷粒学院
## 1、Spring Boot简介
- 简化Spring应用开发的一个框架；
- 整个Spring技术栈的一个大整合；
- J2EE开发的一站式解决方案
## 2、微服务
- 2014、martin fowler
- 微服务：架构风格（微服务化）
- 一个应用应该是一组小型应用；可以通过HTTP的方式进行互通
- 每一个功能元素最终都是一个可独立替换和独立升级的单元；

- 详细参照微服务文档https://martinfowler.com/articles/microservices.html<br>

## 3、环境约束:<br>
- jdk1.8: 
- maven3.x
- SpringBoot1.5.9
***
- maven设置：
***
- IDEA设置：

## 4、Spring Boot HelloWorld
- 一个功能：

- 浏览器发送hello请求，服务器接收请求并处理，相应hello worl字符串；

### 1、创建一个maven工程（jar）
### 2、导入spring boot相关的依赖（最新版的idea不需要）
### 3、编写一个主程序；启动spring boot应用（最新版的idea有默认主程序，可以自己配置）


	/**
	 * @SpringBootApplication 这个注解用来标注一个主程序类，说明这是一个spring boot应用
   	*
	 *  @author: zhuhao
	 * @date: 2019/4/18 0018 12:24
	 */

	@SpringBootApplication
	public class HelloWorld {

   	 public static void main(String[] args) {
   	     SpringApplication.run(HelloWorld.class, args);
   	 }
	}

### 4、编写相关的Controller、Service

### 5、运行主程序测试
### 6、简化部署
将这个应用打成jar包，直接使用java -jar的命令执行；  
## 5、Hello World探究
### 1、POM文件
#### 1、父项目
	<parent>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter-parent</artifactId>
	    <version>2.1.4.RELEASE</version>
	    <relativePath/> <!-- lookup parent from repository -->
	</parent>
	
	他的父项目是
	<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-dependencies</artifactId>
	<version>2.1.4.RELEASE</version>
	<relativePath>../../spring-boot-dependencies</relativePath>
	</parent>
	他来真正管理Spring Boot应用里面的所有依赖版本

Spring Boot的版本的仲裁中心；  
以后我们导入依赖是不需要版本的；（没有在dependencies里面管理的依赖自然需要声明版本号）  
### 2、启动器
	<dependency>
	        <groupId>org.springframework.boot</groupId>
	        <artifactId>spring-boot-starter-web</artifactId>
	    </dependency> 	 
spring-boot-starter-web：  

- spring-boot-starter：spring-boot场景启动器；帮我们导入了web模块正常运行所依赖的组件

Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导进来。要用什么功能就导入什么场景的启动器。

## 2、主程序类、入口类
	/**
 	* @SpringBootApplication 这个注解用来标注一个主程序类，说明这是一个spring boot应用
	*
 	*  @author: zhuhao
 	* @date: 2019/4/18 0018 12:24
	 	*/

	@SpringBootApplication
	public class HelloWorld {

	 public static void main(String[] args) {
	     SpringApplication.run(HelloWorld.class, args);
	 		}
	}  
@**SpringBootApplication**：Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用。  

	@Target(ElementType.TYPE)
	@Retention(RetentionPolicy.RUNTIME)
	@Documented
	@Inherited
	@SpringBootConfiguration
	@EnableAutoConfiguration
	@ComponentScan(excludeFilters = {
			@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
			@Filter(type = FilterType.CUSTOM,
					classes = AutoConfigurationExcludeFilter.class) })
	public @interface SpringBootApplication {  

@SpringBootConfiguration：SpringBoot的配置类；

- 这个注解标注在某个类上，就代表这个类是SpringBoot的配置类  
	
- @Configuration：配置类上来标注这个注解；

>>配置类--配置文件;配置类也是容器中的一个组件；@Component  

@EnableAutoConfiguration：开启自动配置功能；

- 以前我们需要配置的东西，SpringBoot帮我们自动配置；@EnableAutoConfiguration告诉SpringBoot开启自动配置功能；这样自动配置才能生效；  

@AutoConfigurationPackage：自动配置包
@Import(AutoConfigurationImportSelector.class)：  

- Spring的底层注解@Import，给容器导入一个组件；导入的组件由 AutoConfigurationPackages.Registrar.class；  
- 将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器；  
- @Import(AutoConfigurationImportSelector.class)；
	- 给容器中导入组件
	- 给容器那些组件的选择器；
	- 会给容器中导入非常多的自动配置类（xxxAutoConfiguration）；就是给容器中导入这个场景所需要的所有组件，并配置好这些组件  
	- 有了自动配置类，免去了我们手动编写配置注入功能组件等的工作；
	- SpringBoot在启动的时候从类路径下META-INF/spring.factories加载一个配置文件，这个配置文件中有自动导入指定的值；将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置；以前需要我们自动配置的东西，自动配置类帮我们配置；
	- J2EE的整体整合解决方案和自动配置都在spring-boot-autoconfigure-2.1.4.RELEASE.jar

## 6、使用Spring Initializer快速创建一个	Spring Boot项目
- IDE都支持使用Spring的项目创建向导，快速创建一个SpringBoot项目  
- 选择我们需要的模块；向导会联网创建SpringBoot项目；
- 默认生成SpringBoot项目；
	- 主程序已经生成好了，我们只需要写自己的逻辑；
		- resources文件夹中的目录结构：
		- static：保存所有的静态资源；js css images;
		- templates:保存所有的模板页面；（SpringBoot默认jar包使用嵌入式的tomcat，默认不支持jsp页面）；可以使用模板引擎（freemarker、thymeleaf）
		- application.properties:SpringBoot应用的配置文件；所有的配置都可以在里面修改；
# 二、配置文件
## 1、配置文件
- SpringBoot默认使用一个全局的配置文件,配置名称是固定的
  - application.properties
  - application.yml

- 配置文件的作用：修改SpringBoot自动配置的默认值；Spring在底层都给我们自动配置好；

- YAML是"YAML Ain't a Markup Language"（YAML不是一种置标语言）的递归缩写。
  - YAML A Markup Language:是一个标记语言；
  - YAML isn`t Markup Language:不是一个标记语言；

- 标记语言：
  - 以前的配置文件：大多都是使用XXX.xml文件；
  - 以数据为中心，比json、xml等更适合做配置文件；

- YAML配置实例：  

     ```
      server:
      		port: 80
     ```

          ## 2、YAML语法：

     ### 1、基本语法

     - K：（空格）v：表示已对键值对（空格必须有）；

     + 以空格的缩进控制层级关系；只要是左对齐的一列数据，都是同一个层级的

     ```java
     spring:
       datasource:
         driver-class-name: com.mysql.jdbc.Driver  
     ```

     - 属性和值也是大小写敏感；  

     ### 2、值的写法

     - 字面量：普通的值（数字，字符串，布尔）

       - k: v：字面直接来写
       - 字符串默认不用加上单引号或者双引号；
       - “”: 双引号；会转义字符串里面的特殊字符；特殊字符会作为自身想表达的意思；
         - name：“zhangsan \n lisi”：输出zhangsan 换行 lisi；
       - ‘’：单引号：不会转义特殊字符，特殊字符最终只是一种特殊的字符串
         - name：“zhangsan \n lisi”：输出zhangsan \n lisi；

     - 对象、map（属性和值）（键值对）

       - k： v：在下一行来写对象的属性和值得关系；注意缩进

         - 对象还是k： v的方式

         ```yaml
         frends:
         	lastname: zhangsan
         	lage: 20
         ```

       - 行内写法：

         ```yam
         friends： {lastname： 张三，age： 20
         ```

     - 数组（list、set）

       - 用- 值表示数组中的一个元素

         ```yaml
         pets:
         	- cat
         	- dog
         	- pig
         ```

       - 行内写法

         ```yaml
         pets: [cat,dog,pig]
         ```

     ### 3、配置文件值注入

     配置文件  

     ```yaml
     #这是一个对象
     person:
       lastName: zhangsan
       age: 20
       boss: false
       birth: 2019/4/18
       maps: {k1: 牛逼,k2: AV,k3: 222,k4: 111 }
       lists:
         - lisi
         - zhaoliu
       dog:
         name: 小狗
         age: 2
     ```

     javaBean:

     ```java
     /**
      * 将配置文件中配置的每一个属性的值，映射到这个组件中
      * @ConfigurationProperties:告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
      *      prefix = "person": 配置文件中哪个下面的所有属性进行一一映射
      *
      * @Component 只有这个组件是容器中的组件，才能使用容器提供的@ConfigurationProperties功能；
      *
      *  @author: zhuhao 
      * @date: 2019/4/18 0018 19:22
      */
     @Data
     @Builder
     @AllArgsConstructor
     @NoArgsConstructor
     @Component
     @ConfigurationProperties(prefix = "person")
     public class Person {
     
         private String lastName;
         private Integer age;
         private Boolean boss;
         private Date birth;
     
         private Map<String, Object> maps;
         private List<Object> lists;
         private Dog dog;
     
     }
     ```

     我们可以导入配置文件处理器，以后编写配置就有提示了

     ```java
     <!-- 导入配置文件处理器，配置文件进行绑定就会有提示 -->
             <dependency>
                 <groupId>org.springframework.boot</groupId>
                 <artifactId>spring-boot-configuration-processor</artifactId>
                 <optional>true</optional>
             </dependency>
     ```




