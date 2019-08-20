3. ### 基于java配置SSM，eclipse

#### 新建maven，web项目

  ....

  项目结构：

  ![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819163332846-1746435826.png)

#### jar包

```xml
  <properties>
  		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  		<maven.compiler.source>1.7</maven.compiler.source>
  		<maven.compiler.target>1.7</maven.compiler.target>
  		<spring.version>4.3.14.RELEASE</spring.version>
  		<mybatis.version>3.5.0</mybatis.version>
  		<log4j.version>1.2.17</log4j.version>
  	</properties>
  
  	<dependencies>
  		<dependency>
  			<groupId>junit</groupId>
  			<artifactId>junit</artifactId>
  			<version>4.11</version>
  			<scope>test</scope>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-core</artifactId>
  			<version>${spring.version}</version>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-web</artifactId>
  			<version>${spring.version}</version>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-webmvc</artifactId>
  			<version>${spring.version}</version>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-tx</artifactId>
  			<version>${spring.version}</version>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-orm</artifactId>
  			<version>${spring.version}</version>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-aop</artifactId>
  			<version>${spring.version}</version>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-jdbc</artifactId>
  			<version>${spring.version}</version>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-context-support</artifactId>
  			<version>${spring.version}</version>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-test</artifactId>
  			<version>${spring.version}</version>
  		</dependency>
  
  
  		<!--mybatis 核心包 -->
  		<dependency>
  			<groupId>org.mybatis</groupId>
  			<artifactId>mybatis</artifactId>
  			<version>${mybatis.version}</version>
  		</dependency>
  		<!--spring/mybatis -->
  		<dependency>
  			<groupId>org.mybatis</groupId>
  			<artifactId>mybatis-spring</artifactId>
  			<version>2.0.0</version>
  		</dependency>
  		<!--mysql驱动 -->
  		<dependency>
  			<groupId>mysql</groupId>
  			<artifactId>mysql-connector-java</artifactId>
  			<version>8.0.15</version>
  		</dependency>
  		<!--连接池 -->
  		<dependency>
  			<groupId>commons-dbcp</groupId>
  			<artifactId>commons-dbcp</artifactId>
  			<version>1.4</version>
  		</dependency>
  		<!--日志文件 -->
  		<dependency>
  			<groupId>log4j</groupId>
  			<artifactId>log4j</artifactId>
  			<version>${log4j.version}</version>
  		</dependency>
  		<!--格式化输出日志 -->
  		<dependency>
  			<groupId>com.alibaba</groupId>
  			<artifactId>fastjson</artifactId>
  			<version>1.2.54</version>
  		</dependency>
  		<dependency>
  			<groupId>org.slf4j</groupId>
  			<artifactId>slf4j-api</artifactId>
  			<version>1.7.26</version>
  		</dependency>
  		<!--log end -->
  
  		<!--文件上传 -->
  		<dependency>
  			<groupId>commons-fileupload</groupId>
  			<artifactId>commons-fileupload</artifactId>
  			<version>1.3.3</version>
  		</dependency>
  		<dependency>
  			<groupId>commons-io</groupId>
  			<artifactId>commons-io</artifactId>
  			<version>2.6</version>
  		</dependency>
  		<dependency>
  			<groupId>commons-codec</groupId>
  			<artifactId>commons-codec</artifactId>
  			<version>1.11</version>
  		</dependency>
  		<dependency>
  			<groupId>javax.servlet</groupId>
  			<artifactId>jstl</artifactId>
  			<version>1.2</version>
  		</dependency>
  	</dependencies>
  
  	  <!-- servlet -->
      <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
      </dependency>
  </build>
```

#### spring和DispatcherServlet上下文

```java
  public class DemoWebApplicationInitializer
  	extends AbstractAnnotationConfigDispatcherServletInitializer  {
  	@Override
  	protected Class<?>[] getRootConfigClasses() {
  		// TODO Auto-generated method stub
  		return new Class<?>[] {RootConfig.class};
  	}
  	@Override
  	protected Class<?>[] getServletConfigClasses() {
  		// TODO Auto-generated method stub
  		return new Class<?>[] {WebConfig.class};
  	}
  	@Override
  	protected String[] getServletMappings() {
  		// TODO Auto-generated method stub
  		return new String[] {"/"};
  	}
  }
```

#### DispatcherServlet

```java
  @Configuration
  @EnableWebMvc
  @ComponentScan(basePackages = {"com.getword.controller"})
  public class WebConfig extends WebMvcConfigurerAdapter {
  	@Bean
  	public ViewResolver viewResolver() {
  		InternalResourceViewResolver resolver = new InternalResourceViewResolver();
  		resolver.setPrefix("/WEB-INF/views/");
  		resolver.setSuffix(".jsp");
  		resolver.setExposeContextBeansAsAttributes(true);
  		return resolver;
  	}
  	/**
  	 * 静态资源
  	 */
  	@Override
  	public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
  		// TODO Auto-generated method stub
  		configurer.enable();
  	}
  }
```

#### spring 上下文

```java
  @Configuration
  @ComponentScan(basePackages = {"com.getword"},
  		excludeFilters = {@Filter(type = FilterType.ANNOTATION, value = EnableWebMvc.class)})
  @Import(DataSourceConfig.class)
  public class RootConfig {
  	@Bean
  	public BeanNameAutoProxyCreator autoProxyCreator() {
  		BeanNameAutoProxyCreator autoProxyCreator = new BeanNameAutoProxyCreator();
  		autoProxyCreator.setProxyTargetClass(true);
  		// 设置要创建代理的那些Bean的名字
  		autoProxyCreator.setBeanNames("*Service");
  		autoProxyCreator.setInterceptorNames("transactionInterceptor");
  		return autoProxyCreator;
  	}
  }
```

  

#### DataSourceConfig

```java
  @Configuration
  @MapperScan("com.getword.dao")
  public class DataSourceConfig {
  	private final static Logger LOG = Logger.getLogger(DataSourceConfig.class);
  	private String driver = "com.mysql.jdbc.Driver";;
  	private String url = "jdbc:mysql://localhost:3306/spring?characterEncoding=UTF-8&serverTimezone=UTC";
  	private String username = "root";
  	private String password = "123";
  	@Bean
  	public BasicDataSource dataSource() {
  		LOG.info("Initialize the BasicDataSource...");
  		BasicDataSource dataSource = new BasicDataSource();
  		dataSource.setDriverClassName(driver);
  		dataSource.setUrl(url);
  		dataSource.setUsername(username);
  		dataSource.setPassword(password);
  		return dataSource;
  	}
  	// mybatis的配置
  	@Bean
  	public SqlSessionFactoryBean sqlSessionFactoryBean() throws IOException {
  		ResourcePatternResolver resourcePatternResolver = new PathMatchingResourcePatternResolver();
  		SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
  		sqlSessionFactoryBean.setDataSource(dataSource());
  		sqlSessionFactoryBean.setMapperLocations(resourcePatternResolver.getResources("classpath*:mappers/*.xml"));
  		sqlSessionFactoryBean.setTypeAliasesPackage("com.getword.domain");// 别名，让*Mpper.xml实体类映射可以不加上具体包名
  		return sqlSessionFactoryBean;
  	}
  
  	// 事务管理器 对mybatis操作数据库事务控制，spring使用jdbc的事务控制类
  	@Bean(name = "transactionManager")
  	public DataSourceTransactionManager dataSourceTransactionManager() {
  		DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
  		dataSourceTransactionManager.setDataSource(dataSource());
  		return dataSourceTransactionManager;
  	}
  
  	@Bean(name = "transactionInterceptor")
  	public TransactionInterceptor interceptor() {
  		TransactionInterceptor interceptor = new TransactionInterceptor();
  		interceptor.setTransactionManager(dataSourceTransactionManager());
  		Properties transactionAttributes = new Properties();
  		transactionAttributes.setProperty("save*", "PROPAGATION_REQUIRED");
  		transactionAttributes.setProperty("del*", "PROPAGATION_REQUIRED");
  		transactionAttributes.setProperty("update*", "PROPAGATION_REQUIRED");
  		transactionAttributes.setProperty("get*", "PROPAGATION_REQUIRED,readOnly");
  		transactionAttributes.setProperty("find*", "PROPAGATION_REQUIRED,readOnly");
  		transactionAttributes.setProperty("*", "PROPAGATION_REQUIRED");
  		interceptor.setTransactionAttributes(transactionAttributes);
  		return interceptor;
  	}
  }
```

> 注意：mapper的命名空间必须和对应的接口的全路径一致！！！

  

### idea，从maven简单java项目转web项目

#### 新建maven项目

1. 新建maven项目

  ![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819163400847-2092553825.png)

2. 填写group id和artifictId，next  

```
![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819163522392-1008079704.png)
```

3. 输入项目名称，finish
4. 配置maven，次步骤最后在新建项目之前
5. ![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819163722381-1923061362.png)

​    

> 项目结果如下：

![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819163817287-875344901.png)



#### 添加web模块

1. 项目结构->Modules->add->web

![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819164610682-908751136.png)



2. 删除web.xml

![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819164718982-598805534.png)

3. 设置web项目根路径

![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819164844317-1740600229.png)

- 添加artifact

![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819171643219-1833564188.png)

4. 配置Tomcat
5. 此时项目结构

![](https://img2018.cnblogs.com/blog/1250855/201908/1250855-20190819171547498-1825423205.png)



此时可以访问webapp下的静态文件了

5. jar包，pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.getword</groupId>
  <artifactId>ssmJavaConfig</artifactId>
  <version>1.0-SNAPSHOT</version>

  <!--设置源代码编码-->
  <properties>
    <project.build.sourceEncoding>utf-8</project.build.sourceEncoding>
    <spring.version>4.3.14.RELEASE</spring.version>
    <mybatis.version>3.5.0</mybatis.version>
    <log4j.version>1.2.17</log4j.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <!--mybatis 核心包 -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>${mybatis.version}</version>
    </dependency>
    <!--spring/mybatis -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.0</version>
    </dependency>
    <!--mysql驱动 -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
    </dependency>
    <!--连接池 -->
    <dependency>
      <groupId>commons-dbcp</groupId>
      <artifactId>commons-dbcp</artifactId>
      <version>1.4</version>
    </dependency>
    <!--日志文件 -->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>${log4j.version}</version>
    </dependency>
    <!--格式化输出日志 -->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>fastjson</artifactId>
      <version>1.2.54</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.7.26</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.12</version>
    </dependency>
    <!--log end -->

    <!--文件上传 -->
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.3</version>
    </dependency>
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.6</version>
    </dependency>
    <dependency>
      <groupId>commons-codec</groupId>
      <artifactId>commons-codec</artifactId>
      <version>1.11</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
    <dependency>
      <groupId>org.testng</groupId>
      <artifactId>testng</artifactId>
      <version>7.0.0-beta7</version>
      <scope>compile</scope>
    </dependency>

    <!-- servlet -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>

  <!--设置编译源代码的jdk版本-->
  <build>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
  </build>

</project>
```

> 注意:使用maven时也要添加servlet依赖，注意作用域。此时可以使用servlet了

#### spring配置

同eclipse。



### 源码附件

[ssm基本配置]()

