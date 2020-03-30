# SSM整合总结
## 准备工作
### 环境配置
#### JAVA配置
#### Tomcat配置
#### Maven+Eclipse配置
### Maven Web项目创建
1. 新建`Maven Project`
2. 勾选`Create a simple project`
3. 输入`Group Id`通常为`com`(公司) `org`(非盈利) `cn` 开头，加上开发者名字
4. 输入`Artifact Id`(项目名称)
5. Packaging选择`war`
6. 点击`Finish`
7. 选择项目--->`Properties`--->`Resource` 确认项目编码为`UTF-8`
8. 选择`Java Build Path`--->双击`JRE System Libray[...]`--->`Workspace default JRE(...)`--->`Finish`
9. 选择`Project Facets`--->取消勾选`Dynamic Web Module`--->`Apply`--->重新勾选`Dynamic Web Module`--->点击左下角的`Further configuration available...`
10. 在`Content directiory`中输入`src/main/webapp`--->`OK`
11. `Dynamic Web Module` 选择`4.0`--->`Java`选择`1.8`--->`Apply`
---
## 建立SSM架构
### SSM整合思路
* `spring` `mybatis`的整合：将`sqlsessionFactory`交给`spring`来处理
* `spring` `springmvc`整合：各自配置各自的即可
### 引入必要jar包
在`pom.xml`中添加如下信息
```xml
<properties>
		<!-- spring版本号 -->
		<spring.version>5.1.1.RELEASE</spring.version>
		<!-- mybatis版本号 -->
		<mybatis.version>3.4.6</mybatis.version>
		<!-- log4j日志文件管理包版本 -->
		<slf4j.version>1.7.25</slf4j.version>
		<log4j.version>2.11.1</log4j.version>
</properties>
 
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<!-- 表示开发的时候引入，发布的时候不会加载此包 -->
			<scope>test</scope>
		</dependency>
		<!-- spring核心包 -->
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
			<artifactId>spring-oxm</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
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
        
		<!-- mybatis核心包 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>${mybatis.version}</version>
		</dependency>
        
		<!-- mybatis/spring包 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.3.2</version>
		</dependency>
        
		<!-- servlet API包 -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>4.0.1</version>
			<scope>provided</scope>
		</dependency>
        
        <!-- 导入druid的jar包，用来在applicationContext.xml中配置数据库 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.1.12</version>
		</dependency>
        
		<!-- 导入Mysql驱动 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>8.0.13</version>
		</dependency>
        
        <!-- 导入SQL server驱动 -->
		<dependency>
			<groupId>com.microsoft.sqlserver</groupId>
			<artifactId>mssql-jdbc</artifactId>
			<version>7.0.0.jre8</version>
		</dependency>
        
		<!-- 日志文件管理包 -->
		<!-- log start -->
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-web</artifactId>
			<version>${log4j.version}</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-api</artifactId>
			<version>$log4j.version}</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>${log4j.version}</version>
		</dependency>
		
		<!-- 格式化对象，方便输出日志 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.1.41</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${slf4j.version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${slf4j.version}</version>
		</dependency>
		<!-- log end -->
        
		<!-- 上传组件包 -->
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
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.8.1</version>
        </dependency>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>

        <!-- 分页助手 -->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.7</version>
        </dependency>
	</dependencies>
```

### 创建配置文件
1. 创建一个`applicationContext.xml`文件
2. 配置`web.xml`文件
```xml
<display-name>Archetype Created Web Application</display-name>
<!-- 启动spring容器 ,项目已启动就会加载applicationContext.xml配置文件 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```
3. 继续配置`applicationContext.xml`文件
```xml
<!-- 数据源，mapper.xml -->
<!-- springMVC配置文件，包含跳转逻辑的控制， -->
<context:component-scan base-package="com.ssm">
    <!-- 只扫描控制器 -->
    <context:exclude-filter type="annotation"
                          expression="org.springframework.stereotype.Controller" />
</context:component-scan>
```
4. 配置数据源
```xml
<!-- 使用spring的配置文件，主要是配置业务和逻辑有关的事务处理 -->
<!-- 数据源，事务控制 -->
<!-- 此处用于加载 dbconflig.properties配置文件 -->
<context:property-placeholder location="classpath:db.properties"/>
<bean id="dataSource"
class="org.apache.tomcat.dbcp.dbcp2.BasicDataSource">
    <property name="driverClassName" value="${jdbc.driverClass}"></property>
    <property name="url" value="${jdbc.jdbcUrl}"></property>
    <property name="username" value="${jdbc.user}"></property>
    <property name="password" value="${jdbc.password}"></property>
</bean>
```
5. `Mybatis`与`Spring`整合
```xml
<!-- 配置和myBatis与spring的整合 -->
<!-- 配置sqlSessionFacotory ，因为mybatis 是通过sqlSessionFactory 来创建sqlSession 
来执行statement 语句的。 现在mybatis交由spring管理，所以需要在这里配置。 -->
<bean id="sqlSessionFactory"
class="org.mybatis.spring.SqlSessionFactoryBean">
<!-- 注入数据库连接池 -->
    <property name="dataSource" ref="dataSource" />
    <!-- 配置MyBaties全局配置文件:mybatis-config.xml ,加载mybatis的配置文件-->
    <property name="configLocation"
    value="classpath:mybatis-config.xml" />
    <!-- 加载mapper.xml的路径 
    value="classpath:mapper/*":加载mapper目录下的所有文件
    -->
    <property name="mapperLocations" value="classpath:mapper/*"></property>
</bean>
```
6. 使用`Spring`整合`Mybatis`扫描包
```xml
<!-- 以下步骤就是将sqlSessionFactiory交给spring来处理 -->
<!-- 配置扫描器，将mybatis接口的实现 加到Ioc容器中 -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<!-- 注入sqlSessionFactory -->
    <property name="sqlSessionFactoryBeanName"
    value="sqlSessionFactory" />
    <!-- 
    给出需要扫描Dao接口包 ，加入ioc容器 property name="basePackage"
    value="com.ssm.mapper":作用是将mapper中的所有的接口产生与之动态代理的对象
    对象名就是接口首字母小写的接口名：
    IStudentMapper:对应的对象名就是iStudentMapper
    -->
    <property name="basePackage" value="com.ssm.mapper" />
</bean>
```
7. 配置SpringMVC
在`web.xml`中加入`dispatcherServlet`
8. 编写SpringMVC配置文件`applicationContext-controller.xml`
```xml
<bean id="id" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<!-- SpringMVC基础配置、标配 -->
			<mvc:annotation-driven></mvc:annotation-driven>
```


