# 1.Spring的概述
## 1.1 什么是Spring
Spring是一个轻量级的Java开发框架，目的是为了简化企业级应用的开发。为了降低Java开发的复杂性，Spring采取了以下4中策略：
1. 基于POJO的轻量级和最小侵入性编程；
2. 通过依赖注入和面向接口实现松耦合；
3. 基于切面和惯例进行声明式编程；
4. 通过切面和模板减少样板式代码。
## 1.2 Spring的核心
IoC容器和AOP模块。通过IoC容器管理POJO对象以及他们之间的耦合关系；通过AOP以动态非侵入的方式增强服务。
## 1.3 Spring的优缺点
优点：
1. 方便解耦；
2. 支持AOP；
3. 支持声明式事务；
4. 支持集成各优秀框架。
缺点：
1. 定位是轻量级框架，却给人大而全的感觉；
2. 依赖反射影响性能；
3. 有一定的学习成本。
# 2. Spring控制反转（IoC）
## 2.1 什么是Spring IoC容器
IoC (Inversion of Control)，由程序代码直接操控的对象的调用权交给容器，通过容器来实现对象的装配和管理。Spring IoC容器负责创建对象，管理对象，装配对象，并且管理这些对象的整个生命周期。
## 2.2 Spring IoC的实现机制
Spring中的IoC的实现原理就是工厂模式加反射机制。
## 2.3 BeanFactory和ApplicationContext
BeanFactory和ApplicationContext是Spring的两个核心接口，都可以作为Spring IoC的容器。ApplicationContext是BeanFactory的子接口。

BeanFactory：是Spring里面最底层的接口，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，维护bean之间的依赖关系，控制bean的生命周期。
ApplicationContext提供了额外的功能：支持国际化、统一的资源文件访问方式、监听器中注册Bean的事件、加载多配置文件。

BeanFactroy：采用的是延迟加载来注入Bean，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。
ApplicationContext：在容器启动时，一次性创建了所有的Bean。
## 2.4 依赖注入的实现方式
Setting方法注入、构造器注入
# 3. Spring Bean
## 3.1 什么是Spring Bean
Spring Bean就是Spring应用中的Java对象，这些对象被Spring IoC容器初始化、装配和管理。这些Bean通过容器中配置的元数据创建。
## 3.2 Spring容器配置元数据
1. XML配置文件；
2. 基于注解的配置；
3. 基于Java类的配置。
## 3.3 Spring Bean的作用域
1. singleton：单例，默认的作用域；
2. prototype：每次请求都会创建一个Bean；
3. request：每次http请求都会创建一个Bean，该作用域仅在基于web的Spring ApplicationContext情形下有效；
4. session：在一个Session中，一个Bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效；
5. global-session：在一个全局的HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。
## 3.4 Spring Bean的生命周期
1. Spring对Bean进行实例化；
2. Spring将值和Bean的引用注入到Bean对应的属性中；
3. 如果Bean实现了BeanNameAware接口，Spring将Bean的ID传递给setBeanName()方法；
4. 如果Bean实现了BeanFactoryAware接口，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入；
5. 如果Bean实现了ApplicationContextAware接口，Spring将调用setApplicationContext()方法，将Bean所在的应用上下文的引用传入进来；
6. 如果Bean实现了BeanPostProcessor接口，Spring将调用它们的postProcessBeforeInitialization()方法；
7. 如果Bean实现了InitializingBean接口，Spring将调用它们的afterPropertiesSet()方法。
8. 如果Bean实现了BeanPostProcessor接口，Spring将调用它们的postProcessAfterInitialization()方法；
9. Bean已经准备就绪，可以被应用程序使用，一直驻留在应用上下文中，直到该应用上下文被销毁；
10. 如果Bean实现了DisposableBean接口，Spring将调用它的destroy()接口方法。
# 4. Spring注解
## 4.1 @Configuration
定义一个配置Bean的Java类。
## 4.2 @Autowired和@Resource
用于Bean的装配。

@Autowired：默认是按照类型装配，依赖对象必须存在（可以设置它required属性为false）。
@Resource：默认是按照名称来装配，只有当找不到与名称匹配的Bean才会按照类型来装配注入。
## 4.3 @Qualifier
当创建了多个类型相同的Bean时，可以使用@Qualifier和@Autowired指定应该装配哪个Bean。
# 5. Spring面向切面编程（AOP）
## 5.1 什么是AOP
AOP(Aspect-Oriented Programming)，称为面向切面编程。作为面向对象的一种补充，用于将与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为“切面”（Aspect）。减少系统中的重复代码，降低了模块间的耦合度，同时提高了系统的可维护性。
## 5.2 Spring AOP and AspectJ AOP
AspectJ：静态代理，所谓静态代理就是AOP框架会在编译阶段生成AOP代理类，因此也称为编译时增强，它会在编译阶段将AspectJ(切面)织入到Java字节码中，运行的时候就是增强之后的AOP对象。

Spring AOP：动态代理，所谓的动态代理就是说AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。
## 5.3 JDK动态代理和CGLIB动态代理
JDK动态代理：只提供接口的代理，不支持类的代理。核心InvocationHandler接口和Proxy类，InvocationHandler通过invoke()方法反射来调用目标类中的代码，动态地将横切逻辑和业务编织在一起；接着，Proxy利用InvocationHandler动态创建一个符合某一接口的的实例, 生成目标类的代理对象。

CGLIB（Code Generation Library）：是一个代码生成的类库，可以在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法并添加增强代码，从而实现AOP。CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。
# 6. Spring事务传播行为
spring事务的传播行为指的是当多个事务同时存在的时候，spring如何处理这些事务的行为。
1. PROPAGATION_REQUIRED：如果当前没有事务，就创建一个新事务，如果当前存在事务，就加入该事务。
2. PROPAGATION_SUPPORTS：如果当前存在事务，就加入该事务，如果当前不存在事务，就以非事务执行。
3. PROPAGATION_MANDATORY：如果当前存在事务，就加入该事务，如果当前不存在事务，就抛出异常。
4. PROPAGATION_REQUIRES_NEW：无论当前是否存在事务，都创建新事务。
5. PROPAGATION_NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
6. PROPAGATION_NEVER：以非事务方式执行，如果当前存在事务，则抛出异常。
7. PROPAGATION_NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则按REQUIRED属性执行。
# 7. Spring MVC的流程
1. 用户发送请求到控制器DispatcherServlet；
2. DispatcherServlet收到请求后，调用HandlerMapping处理器映射器，请求获取Handle；
3. 处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet；
4. DispatcherServlet通过HandlerAdapter处理器适配器调用处理器；
5. Handler执行完成返回ModelAndView；
6. HandlerAdapter将Handler执行结果ModelAndView返回给DispatcherServlet；
7. DispatcherServlet将ModelAndView传给ViewReslover视图解析器进行解析；
8. ViewReslover解析后返回具体View；
9. DispatcherServlet对View进行渲染视图；
10. DispatcherServlet响应用户。