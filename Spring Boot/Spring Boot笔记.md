# Spring Boot学习笔记
## 微服务
**一个项目**可以由多个小型服务构成（微服务）
## Spring Boot可以快速开发**微服务模块**
- 简化j2ee开发
- 整个spring技术栈的整合（整合springmvc spring）
- 整个j2ee技术的整合（整合mybatis redis）

## 准备
- jdk
- maven
- ide

## Spring Boot目录结构resources
static：静态资源（js css 图片 视频 音频）
templates：末班文件（末班引擎freemarker，thymeleaf；默认不支持jsp）
application.properties：配置文件

spring boot内置了tomcat，并不需要打成war再执行
可以在appication.properties对端口号等服务端信息进行配置

spring boot将各个应用/第三方框架 设置成了一个个**场景**stater，以后要用那个，只需引用场景即可
选完之后，spring boot就会将该场景所需要的所有依赖自动注入
例如 选择“web”，spring boot就会将web相关的依赖（tomcat json） 全部引入本项目

## Spring Boot注解
### @SpringBootApplication:spring boot的主配置类
@SpringBootConfiguration:
- @Configuration: ①表示配置类,该类是一个配置类。②加了@Configuration注解的类，会自动纳入Spring容器（@component）
- @EnableAutoConfiguration: 使spring boot可以自动配置，可以找到@SpringBootApplication所在类的报，作用是将该包及所有的子包全部纳入spring容器
- spring boot在启动时，会根据META-INF/spring.factories找到相应的第三方依赖，并将这些依赖引入本项目
总结：
编写项目时，一般会对自己写的代码以及第三方依赖进行配置。但是spring boot可以自动进行配置：
1. 自己写的代码，spring boot通过@SpringBootConfiguration自动帮我们配置；
2. 第三方依赖 通过spring-boot-autoconfigure-x.x.x.RELEASE.jar中的META-INF/spring.factories进行声明 
3. spring-boot-autoconfigure-x.x.x.RELEASE.jar包中 包含了 J2EE整合体系中需要的依赖
## 如何自动装配
研究org.sprinframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration
通过观察该源码 发现：
@Configuration:标识是一个配置类、将此类纳入springioc容器
@EnableConfigurationProperties（HttpEncodingProperties.class)：通过HttpEncodingProperties将编码设置为UTF_8(即自动装配为UTF_8，如何修改编码：通过该Properties的 prefix+属性名 进行修改[在配置文件中，yml/properties])
> 即：该注解给了默认编码utf8，并提供了prefix+属性名的方式 供我们修改编码
@ConditionalOnProperty(prefix="spring.http.encoding",value="enable",matchIFMissing=true)
当属性满足要求时，此条件成立: 要求 如果没有配置spring.http.endcoding.endabled=xxx,则成立。
总结：
1. 每一个XXAutoConfiguration都有很多条件@ConditionalOnXXX，当这些条件都满足时，则此配置自动装配生效(utf-8)。但是我们可以手动修改，自动装配：xxxproperties文件中的prefix.属性名=value
2. 全局配置文件中的key，来源于某个Properties文件中的prefix+属性名
3. 如何知道spring boot 开启了那些自动装配、禁止了哪些自动装配：application.properties
4. Postitive matches列表：spring boot开启了哪些自动装配
5. Negative matches列表：spring boot 在此时并没有启动的自动装配

## 配置文件
作用：spring boot 自动装配（约定，8080）可以使用配置文件，对默认的
默认的全局配置文件：
- application.properties:k=v
- application.yml:yaml ain't myarkup language,不是一个标记文档，注意：1. k:空格v 2.通过垂直对齐，指定层次关系 3.默认不写引号；""会将其中的转义符进行转义，其他符号不会
```yml
server:
        port: 8882
```
> xml:是一个标记文档
通过yaml给对象注入值：
```java
@Component // 将此Javabean
@ConfigurationProperties(prefix="student")
public class Student
```
通过注解给对象注入值
```java
@Value("aaaa")
```