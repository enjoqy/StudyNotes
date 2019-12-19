# 4、Web开发

## 1、使用SpringBoot；

- 创建SpringBoot应用，选中我们需要的模块；

- SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来

- 自己编写业务代码；

- 自动配置原理

  - 这个场景SpringBoot帮我们配置了什么？能不能修改？能修改那些内容？能不能扩展？xxx

  ```
  xxxAutoConfiguration:帮我们给容器中自动配置组件
  xxxProperties:配置类来封装配置文件的内容
  ```

  ## 2、SpringBoot对静态资源的映射规则

  ```java
  		@Override
  		public void addResourceHandlers(ResourceHandlerRegistry registry) {
  			if (!this.resourceProperties.isAddMappings()) {
  				logger.debug("Default resource handling disabled");
  				return;
  			}
  			Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
  			CacheControl cacheControl = this.resourceProperties.getCache()
  					.getCachecontrol().toHttpCacheControl();
  			if (!registry.hasMappingForPattern("/webjars/**")) {
  				customizeResourceHandlerRegistration(registry
  						.addResourceHandler("/webjars/**")
  						.addResourceLocations("classpath:/META-INF/resources/webjars/")
  						.setCachePeriod(getSeconds(cachePeriod))
  						.setCacheControl(cacheControl));
  			}
  			String staticPathPattern = this.mvcProperties.getStaticPathPattern();
  			if (!registry.hasMappingForPattern(staticPathPattern)) {
  				customizeResourceHandlerRegistration(
  						registry.addResourceHandler(staticPathPattern)
  								.addResourceLocations(getResourceLocations(
  										this.resourceProperties.getStaticLocations()))
  								.setCachePeriod(getSeconds(cachePeriod))
  								.setCacheControl(cacheControl));
  			}
  		}
  
  		//配置喜欢的图标
  		@Configuration
  		@ConditionalOnProperty(value = "spring.mvc.favicon.enabled",
  				matchIfMissing = true)
  		public static class FaviconConfiguration implements ResourceLoaderAware {
  
  			private final ResourceProperties resourceProperties;
  
  			private ResourceLoader resourceLoader;
  
  			public FaviconConfiguration(ResourceProperties resourceProperties) {
  				this.resourceProperties = resourceProperties;
  			}
  
  			@Override
  			public void setResourceLoader(ResourceLoader resourceLoader) {
  				this.resourceLoader = resourceLoader;
  			}
  
  			@Bean
  			public SimpleUrlHandlerMapping faviconHandlerMapping() {
  				SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
  				mapping.setOrder(Ordered.HIGHEST_PRECEDENCE + 1);
  				mapping.setUrlMap(Collections.singletonMap("**/favicon.ico",
  						faviconRequestHandler()));
  				return mapping;
  			}
  
  			@Bean
  			public ResourceHttpRequestHandler faviconRequestHandler() {
  				ResourceHttpRequestHandler requestHandler = new ResourceHttpRequestHandler();
  				requestHandler.setLocations(resolveFaviconLocations());
  				return requestHandler;
  			}
  
  			private List<Resource> resolveFaviconLocations() {
  				String[] staticLocations = getResourceLocations(
  						this.resourceProperties.getStaticLocations());
  				List<Resource> locations = new ArrayList<>(staticLocations.length + 1);
  				Arrays.stream(staticLocations).map(this.resourceLoader::getResource)
  						.forEach(locations::add);
  				locations.add(new ClassPathResource("/"));
  				return Collections.unmodifiableList(locations);
  			}
  
  		}
  ```

  - 所有的/webjars/**，都去classpath:/META-INF/resources/webjars/找资源；
    - webjars:以jar包的方式引入静态资源；

  <https://www.webjars.org/>

  - localhost:8080/webjar/jquery/3.4.0/jquery.js

  ```java
  <!-- 引入jquery-webjar -->在访问的时候只需要写webjars下面资源的名称即可
  <dependency>
     <groupId>org.webjars</groupId>
     <artifactId>jquery</artifactId>
      <version>3.4.0</version>
   </dependency>
  ```

  - 欢迎页；静态资源文件夹下面的所有的index.html页面；都被/**映射;
    - localhost:8080/ 找到index页面
  - 所有的**/favicon.ico都是在静态资源文件下面找

  ## 3、模板引擎

  ### 1、引入Thymeleaf  

  jsp、Velocity、Freemarker、Thymeleaf
  
  SpringBoot推荐使用Thymeleaf；
  
  语法简单，功能更强大
  
  ![img](https://img-blog.csdn.net/20180715160105812?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhbmdteDE5OTMzMjg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
  
  ```java
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-thymeleaf</artifactId>
          </dependency>
  ```
  
  ### 2、Thymeleaf使用&语法
  
  ```
  @ConfigurationProperties(prefix = "spring.thymeleaf")
  public class ThymeleafProperties {
  
  	private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;
  
  	public static final String DEFAULT_PREFIX = "classpath:/templates/";
  
  	public static final String DEFAULT_SUFFIX = ".html";
  	//只要我们把html页面放在classpath:/templates/，thymeleaf就可以自动渲染了；
  ```
  
  #### 1、导入Thymeleaf的名称空间
  
  ```xm
  <html lang="en" xmlns:th="http://www.thymeleaf.org">
  ```
  
  #### 2、使用Thymeleaf的语法规则
  
  ```java
  <!DOCTYPE html>
  <html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
  <h1>成功！</h1>
  <div th:text="${hello}">这是显示欢迎的信息</div>
  
  </body>
  </html>
  ```
  
  #### 3、语法规则
  
  - th:text; 改变当前元素的文本内容；
  
  
  
  
  
  
  
  