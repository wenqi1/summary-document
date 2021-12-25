# 1. Spring Boot的概述
## 1.1 什么是Spring Boot
Spring Boot是Spring开源组织下的子项目，是Spring组件一站式解决方案，主要是简化了使用 Spring的难度，简省了繁重的配置，提供了各种启动器，开发者能快速上手。
## 1.2 Spring Boot的优点
1. 独立运行Spring项目；
2. 内嵌Servlet容器；
3. 提供Starter简化Maven配置；
4. 自动装配Spring；
5. 提供了一系列非功能性特性，如安全、指标、健康检查、配置等。
# 2. Spring Boot的配置
## 2.1 Spring Boot的配置文件
Spring Boot支持yaml和properties两种格式的配置文件（yaml文件不支持@PropertySource导入）。Spring Boot有两个核心配置文件bootstrap.properties(yml)和application.properties(yml)，bootstrap在application之前加载，配置在应用程序上下文的引导阶段生效，一般在使用Spring Cloud Config加载远程配置文件时用到，bootstrap的属性不会被覆盖；application用于Spring Boot的自动化配置。
## 2.2 Spring Boot自动化配置原理
@EnableAutoConfiguration、 @Configuration和 @ConditionalOnClass注解组成了Spring Boot自动配置的核心。具体是通过Maven读取每个Sarter中的Spring.factories文件，该文件配置了所有需要被创建在Spring容器中的bean。
## 2.3 Spring Boot配置加载顺序
1. properties文件；
2. yaml文件；
3. 系统环境变量；
4. 命令行参数。
相同的配置项会被后加载的值覆盖。
## 2.4 Spring Boot对XML配置文件的支持
Spring Boot推荐使用Java配置同时支持XML配置，通过@ImportResource加载XML配置。
## 2.5 Spring Boot的核心注解
@SpringBootApplication是启动类上的注解，主要包含3个注解：
1. @SpringBootConfiguration：组合Configuration实现配置文件功能；
2. @EnableAutoCofiguration：打开自动配置功能，也可以关闭某个自动配置；
3. @ComponentScan：Spring组件扫描。
## 2.6 开启Spring Boot特性几种方式
1. 继承spring-boot-starter-parent项目
2. 导入spring-boot-dependencies项目依赖
# 3. Spring Boot Starter
## 3.1 什么是Starter
Starter可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包，可以一站式集成。
## 3.2 Starter的工作原理
Spring Boot在启动时扫描项目所依赖的JAR包，寻找包含Spring.factories文件的JAR包，根据Spring.factories配置加载AutoConfigure类，根据 @Conditional注解的条件，进行自动配置并将Bean注入Spring Context。
## 3.3 常用的Starter
spring-boot-starter-amqp
通过spring-rabbit来支持AMQP协议（Advanced Message Queuing Protocol）

spring-boot-starter-aop
支持面向方面的编程即AOP，包括spring-aop和AspectJ

spring-boot-starter-jdbc
支持JDBC数据库

mybatis-spring-boot-starter
提供MyBatis持久层操作数据库

spring-boot-starter-redis
支持Redis键值存储数据库，包括spring-redis

spring-boot-starter-security
支持spring-security

spring-boot-starter-web
S支持全栈式Web开发，包括Tomcat和spring-webmvc