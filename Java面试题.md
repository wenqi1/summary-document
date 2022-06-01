# 1.什么是面向对象
与面向过程是两种不同的处理问题方式，面向过程更注重事情的步骤和顺序，面向对象更注重事情的参与者及各自需要做什么。

比如：用洗衣机洗衣服
面向过程的思考方式：打开洗衣机->放衣服->放洗衣液->清洗->脱水
面向对象的思考方式：需要人（打开洗衣机，放衣服，放洗衣液）和洗衣机（清洗，脱水）
面向过程比较直接高效，面向对象更易于复用、扩展和维护。

面向对象的三大特性：

封装：封装的意义在于明确标识出允许外部使用的所有变量和方法

继承：继承基类的变量和方法，并可以进行自己的扩展

多态：基于对象所属类的不同，外部对同一个方法的调用，实际执行的逻辑不同

# 2.JDK、JRE和JVM
JDK：java开发工具

JRE：java运行环境

JVM：java虚拟机

JRE = JVM + 类库

JDK = JRE + 工具（javac、java、jconsole）

# 3.==和equals
==：对于基本数据类型比较的是值，对于引用类型比较的的内存地址

equals：默认是采用==，一般会被重写

# 4.final
修饰类：表示类不可被继承

修饰方法：表示方法不可被子类重写

修饰变量：表示变量一旦被赋值就不能修改

为什么局部内部类和匿名内部类只能访问final修饰的局部变量？

内部类和外部类是处于同一个级别的，内部类不会因为定义在方法中就会随着方法的执行完毕而销毁。这里会产生一个问题，当外部类的方法执行结束时，局部变量就会被销毁，但此时内部类可以还要访问该变量。为类解决这个问题，将局部变量复制了一份作为内部类的成员变量，当局部变量被销毁后，依然可以访问。这样会产生一个新的问题，怎么保证两个变量的一致性，就是将局部变量设置为final，一旦初始化不让再修改。

# 5.String、StringBuilder和StringBuffer
String是final修饰的不可变，每次操作都会产生一个新的对象

StringBuilder和StringBuffer是在原对象上操作

StringBuffer是线程安全的，方法都是synchronized修饰的

# 6.重载和重写
重载：发生在同一个类中，方法名必须相同，参数列表必须不同，返回值和访问修饰法可以不同

重写：发生在父子类中，方法名和参数列表必须相同，返回值范围小于等于父类，抛出异常范围小于等于父类，访问修饰法大于等于父类

# 7.接口和抽象类
抽象类可以存在普通方法，而接口只能存在public abstract方法

抽象类的成员变量可以是各种类型，而接口的成员变量只能是public static final类型的

抽象类只能单继承，而接口可以多实现

抽象类的设计目的，是代码的复用。当不同的类具有某些相同的行为，且一部分行为的实现方式一致时，可以让这些类派生于一个抽象类。抽象类是对类本质的抽象，表达的是is a的关系。

接口的设计目的，是对类的行为进行约束。也就是提供一种机制，可以强制要求不同的类具有相同的行为，它只约束行为的有无，但不对具体实现进行限制。接口是对行为的抽象，表达的是like a的关系。

# 8.ArrayList和LinkList的区别
1. 它们的底层数据结构不同，ArrayList底层基于数组实现，LinkList底层基于链表实现；
2. 由于底层数据结构不同，它们的使用场景不同，ArrayList更适合随机查找，LinkList更适合添加和删除；
3. 它们都实现了List接口，LinkList还额外实现了Deque接口，所以LinkList可以当作队列使用。

# 9.HashMap和HashTable的区别
1. HashMap方法没有synchronized修饰，线程非安全，HashTable线程安全；
2. HashMap允许key和value为null，HashTable不允许。

HashMap底层实现为数组+链表，数组是HashMap的主体，链表是为了解决哈希冲突而存在的。HashMap通过调用key所属类的hashCode方法计算出key的hash值，然后将hash值通过哈希函数计算出更加复杂的hash值，再将算出的hash值和数组长度进行&运算，获得在Entry数组中存放的位置。此时有两种情况：
1. 该数组位置上是空的，此时直接将键值对存放；
2. 该数组位置上不为空，此时就需要将key的哈希值和该位置上存放的所有元素的key的哈希值逐一进行比较。如果哈希值都不同，认为键值对是一个新的元素，继续以链表的形式存储；如果哈希值相同，还需要调用key的equals方法进行比较，如果equals后返回true，则证明这两个key是相同的，用新的value值替换旧的value值，如果equals后返回false，则证明这两个key是不同的，继续以链表的形式存储。

# 10.HashMap的容量为什么是2的幂次方
```java
// 计算key在HashMap中数组位置的过程
// 首先获取key的hashcode值
h = hashcode()
// 然后将hashcode值右移16位，再与自己做异或运算
hash = h ^ (h >>> 16)
// 所得的结果和HashMap的容量减1做与运算
index = (n-1) & hash
```
HashMap的容量为2的幂次方，与（n-1）& hash有关。因为只有当对应的位置都为1，与运算结果才为1，使得添加的元素均匀分布在HashMap的每个位置上，减少hash碰撞。

# 11.Java类加载器
Java自带有三个类加载器：
1. BootstrapClassLoader是ExtClassLoader的父加载器，负责加载%JAVA_HOME%/lib下的jar包和class文件；
2. ExtClassLoader是AppClassLoader的父加载器，负责加载%JAVA_HOME%/lib/ext下的jar包和class文件；
3. AppClassLoader是自定义类加载器的父加载器，负责加载classpath下的class文件。

# 12.双亲委派机制
加载一个类的过程，先是向上委派（查找缓存，是否加载了该类，有则直接返回，没有继续向上），委派到顶层之后，缓存中依然没有找到，则到加载路径中查找，有则加载返回，没有则向下查找。

双亲委派的好处：
1. 主要是为了安全性，避免用户自己编写的类动态替换Java的核心类；
2. 避免类的重复加载，JVM中区分不同类，不仅仅是根据类名，相同的class文件被不同的Classload加载就是两个不同的类。

# 13.Java的异常体系
Java中所有异常都来自顶级父类Throwable，Throwable有两个子类Error和Exception
1. Error无法通过程序处理的错误，一旦出现程序就会被迫停止；
2. Exception可以通过程序处理的错误，它又分为RunTimeException和CheckedException。RunTimeException发生在程序运行过程中，会导致程序当前线程执行失败；CheckedException发生在编译过程中，导致编译不通过。

# 14.GC如何判断对象可以被回收
1. 引用计数法：每个对象有一个引用计数属性，新增一个引用时计数加1，引用释放时计数减1，计算为0时可以回收；引用计算法可以会出现互相引用，a引用b，b引用a，导致永远无法回收；
2. 可达性分析法：从GC Roots开始向下搜索，搜索所走过的路径称为引用链。当一个对象到GC Roots没有任何引用链相连时，则该对象可以回收。

GC Roots的对象有：
1. JVM栈中引用的对象
2. 方法区中类静态属性引用的对象
3. 方法区中常量引用的对象
4. 本地方法栈中JNI引用的对象

# 15.线程的生命周期
线程有五种状态：
1. 新建状态（New）：新创建一个线程对象
2. 就绪状态（Runnable）：线程对象创建后，调用该对象的start()。该状态的线程位于可运行线程池中，变得可用行，等待获取CPU的使用权
3. 运行状态（Running）：就绪状态的线程获取了CPU，执行程序代码
4. 阻塞状态（Blocked）：线程因为某种原因放弃CPU的使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态
5. 死亡状态（Dead）：线程执行完毕或异常退出run(),线程生命周期结束

阻塞分三种情况：
1. 等待阻塞：运行的线程执行了wait()，该线程会释放占用的所有资源，JVM会把线程放入“等待池”中，必须依靠其他线程调用notify()或notifyAll()才能被唤醒
2. 同步阻塞：运行的线程中获取对象的同步锁时，若该同步锁被其他线程占用，JVM会把线程放入“锁池”中
3. 其他阻塞：运行的线程执行了sleep()、join()、发出了I/O请求时，JVM会把线程置为阻塞状态。当sleep超时、join等待线程终止或超时、I/O处理完毕时，线程重新进入就绪状态

# 16.sleep()、wait()、join()和yield()区别
sleep()和wait()
1. sleep()是Thread的静态本地方法，wait()是Object的静态本地方法
2. sleep()不会释放lock，wait()会释放lock
3. sleep()不需要被唤醒，wait()需要（不指定时间的等待）

yield()执行后线程直接进入就绪状态，马上释放CPU的执行权，但是有可能CPU的下次线程调度让该线程获得执行权

join()执行后线程进入阻塞状态，例如线程A中调用线程B的join()，那么线程A会进入到阻塞队列，直到线程B结束

# 17.对线程安全的理解
在Java中堆是虚拟机管理的最大一块内存，是所有线程共享的一块内存区域，在虚拟机启动时创建。堆所存在的内存区域的唯一目的就是存放对象的实例，几乎所有的对象实例和数组都在这里分配内存。因此存在线程安全问题。

在Java中栈时每个线程独有的，保存其运行状态和局部变量。栈在线程开始时初始化，每个线程的栈相互独立，因此栈是线程安全的。

# 18.对守护线程的理解
守护线程是为所有非守护线程提供服务的线程，它依赖整个进程而运行，当其他线程结束了，程序就结束了，守护线程也会被中断。

由于守护线程的终止是自身无法控制的，千万不能做IO、File等操作，随时可能被中断。

守护线程中产生的新线程也是守护线程。

# 19.Thread和Runnable区别
二者本身就没有本质区别，就是接口和类的区别。Thread实现了Runnable接口并进行了扩展，所以Runnable或Thread本身没有可比性。

# 20.ThreadLocal的原理
每一个Thread对象都含有一个ThreadLocalMap类型的成员变量threadLocals，它存储本线程中所有ThreadLocal对象及其对应的值。

ThreadLocalMap由一个个Entry对象构成，Entry继承自WeakReference<ThreadLocal<?>>，Entry的key是ThreadLocal类型，value是Object类型。

当执行set()时，ThreadLocal首先获取当前线程对象，再获取当前对象的ThreadLocalMap对象，最后以ThreadLocal为key，存入到ThreadLocalMap中。

当执行get()时，ThreadLocal首先获取当前线程对象，再获取当前对象的ThreadLocalMap对象，最后用ThreadLocal为key，取出value。

# 21.ThreadLocal内存泄露的原因
ThreadLocalMap使用的弱引用作为key，如果ThreadLocal不存在外部的强引用时，key就会被GC回收，导致ThreadLocalMap中key为null，而value还存在强引用，只有线程退出后，value的强引用链才会断掉。

ThreadLocal的正确使用方式：
1. 每次使用完TreadLocal都调用它的remove()清除数据；
2. 将ThreadLocal变量定义为private static，这样就一直存在ThreadLocal的强引用，能保证任何时候通过ThreadLocal的弱引用访问到Entry的value，进而清除。

# 22.为什么使用线程池
1. 降低资源消耗：提高线程的利用率，降低线程创建和销毁的消耗；
2. 提高响应速度：直接获取，不需要再创建；
3. 提高线程的可管理性。

# 23.线程池的参数介绍
1. corePoolSize：线程池中的常驻核心线程数。
2. maximumPoolSize：线程池能够容纳同时执行的最大线程数。
3. keepAliveTime：多余的空闲线程存活时间，当空间时间达到keepAliveTime值时，多余的线程会被销毁直到只剩下corePoolSize为止。
4. unit：keepAliveTime的单位。（H/min/s）
5. workQueue：任务队列，被提交但尚未被执行的任务。
6. threadFactory：创建线程池中工作线程的线程工厂，一般用默认即可。
7. handler：拒绝策略，表示当线程队列满了并且工作线程等于maxnumPoolSize时如何来拒绝请求执行的runnable的策略。

# 24.线程池的处理流程
线程池执行任务，判断核心线程是否已满，未满则创建核心线程执行；已满则判断任务队列是否已满，未满则放到任务队列；已满则判断是否达到最大线程数，未达到则创建临时线程数执行；已达到则根据拒绝策略处理任务。

# 25.线程池为什么采用阻塞队列
1. 一般的队列只能保证作为一个有限长度的缓冲区，如果超出了缓冲长度就无法保留当前的任务了，阻塞队列通过阻塞可以保留住当前想要继续入队的任务；
2. 阻塞队列可以保证任务队列中没有任务时阻塞获取任务的线程，使得线程进入wait状态，释放cpu资源。阻塞队列自带阻塞和唤醒的功能，不需要额外处理，无任务执行时，线程池利用阻塞队列的take()挂起，从而维持核心线程的存活、不至于一直占用cpu资源。

# 26.线程池有哪些阻塞队列
1. ArrayBlockingQueue
   
   ArrayBlockingQueue 是一个用数组实现的有界阻塞队列；

   队列满时插入操作被阻塞，队列空时，移除操作被阻塞；

   按照先进先出（FIFO）原则对元素进行排序；

   默认不保证线程公平的访问队列；

   公平访问队列：按照阻塞的先后顺序访问队列，即先阻塞的线程先访问队列；

   公平性会降低吞吐量。

2. LinkedBlockingQueue
   
   一个基于链表结构的阻塞队列，此队列按 FIFO 排序元素，吞吐量通常要高于ArrayBlockingQueue。静态工厂方法 Executors.newFixedThreadPool()使用了这个队列。

   LinkedBlockingQueue具有单链表和有界阻塞队列的功能；

   队列满时插入操作被阻塞，队列空时，移除操作被阻塞；

   默认和最大长度为Integer.MAX_VALUE，相当于无界 (值非常大：2^31-1)。

3. SynchronousQueue

   一个不存储元素的阻塞队列。每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于LinkedBlockingQueue，静态工厂方法 Executors.newCachedThreadPool()使用这个队列。

   SynchronousQueue本身不存储数据，调用了put方法后，队列里面也是空的;

   每一个put操作必须等待一个take操作完成，否则不能添加元素。

4. PriorityBlockQueue
   
   一个具有优先级的无限阻塞队列。

# 27.线程池的拒绝策略
1. AbortPolicy：默认策略；新任务提交时直接抛出异常RejectedExecutionException。
2. CallerRunsPolicy：使用调用者所在线程运行新的任务。
3. DiscardPolicy：丢弃新的任务，且不抛出异常。
4. DiscardOldestPolicy：调用poll方法丢弃工作队列队头的任务，然后尝试提交新任务

# 28.线程池中线程复用的原理
线程池将线程和任务进行解耦，线程是线程，任务是任务，摆脱了之前通过Thread创建线程时的一个线程必须对应一个任务的限制。在线程池中，同一个线程可以从阻塞队列中不断获取新任务来执行，其核心原理在于线程池对Thread进行了封装，并不是每次执行任务都会调用Thread.start()来创建新线程，而是让每个线程去执行一个"循环任务"，在这个"循环任务"中不停检查是否有任务需要被执行，如果有则直接执行，也就是调用任务中的run()，通过这种方式只使用固定的线程就将所有任务的run()串联起来。

# 29.对AOP的理解
将程序中的交叉业务逻辑（比例：安全、日志、事务等），封装成一个切面，然后注入到目标对象（具体的业务逻辑）中。

# 30.对IOC的理解
IOC容器：实际就是一个map，里面存放各种对象（xml里配置的bean、@Repository、@Service、@Controller、@Component），在项目启动时通过反射创建对象并放入容器。

依赖注入：依赖注入是实现IOC的方法，在IOC容器运行期间，动态的将某种依赖关系注入到对象中。

# 31.BeanFactory和ApplicationContext区别
1. ApplicationContext是BeanFactory的子接口，拥有BeanFactory的所有功能。同时还继承了其他的接口，像MessageSource、ApplicationEventPublisher等，所以它还支持资源访问ResourcePatternResolver、国际化，事件传播ApplicationEventPublisher、载入有继承关系的多个上下文等；
2. BeanFactory采用的是延迟加载形式来注入bean的，只有在使用到getBean()时，才对该bean进行实例化，这样我们就不能发现一些bean的配置问题。而ApplicationContext则相反，它是在容器启动的时候，一次性创建所有的bean，这样在容器启动时就能发现Spring中存在的配置错误。
3. BeanFactory和ApplicationContext都支持BeanPostProcesser、BeanFactoryPostProcessor的使用，但两者的区别是：BeanFactory是手动注册的，ApplicationContext是自动注册的

# 32.Spring中Bean的生命周期
Bean的生命周期简单分为4个阶段：实例化、属性赋值、初始化和销毁。实例化和属性赋值对应构造方法和setter方法的注入，初始化和销毁是用户能自定义扩展的两个阶段。

Spring之所以强大的原因是易扩展，生命周期相关的常用扩展点非常多。扩展点分2类：
1. 作用于多个bean的增强扩展
   
   InstantiationAwareBeanPostProcessor 作用于实例化阶段前后

   BeanPostProcessor 作用于初始化阶段前后

   DestructionAwareBeanPostProcessor 作用于销毁阶段前
2. 作用于单个bean的增强扩展

   初始化阶段：Aware接口（BeanNameAware BeanClassLoaderAware BeanFactoryAware），InitializingBean接口
   
   销毁阶段：DisposableBean接口

Bean的整个生命周期：
1. 解析类得到BeanDefinition；
2. InstantiationAwareBeanPostProcessor的postProcessBeforeInstantiation方法，该方法的返回值类型是Object；
3. 执行Bean的构造器；
4. InstantiationAwareBeanPostProcessor的postProcessAfterInstantiation方法，这个时候对象已经被实例化，但是该实例的属性还未被设置。因为它的返回值是决定要不要调用postProcessPropertyValues方法的其中一个因素，如果该方法返回false,并且不需要check，那么postProcessPropertyValues就会被忽略不执行；如果返回true，postProcessPropertyValues就会被执行；
5. InstantiationAwareBeanPostProcessor的postProcessPropertyValues方法，可以修改原本该设置进去的属性值；
6. 属性赋值；
7. Bean实现的Aware接口；
8. BeanPostProcessor的postProcessBeforeInitialization方法；
9. Bean实现的InitializingBean接口；
10. BeanPostProcessor的postProcessAfterInitialization方法；
11. DestructionAwareBeanPostProcessor的postProcessBeforeDestruction的法，Bean销毁前；
12. 销毁；
13. Bean实现的DiposibleBean接口。

# 33.Spring中Bean的作用域
1. singleton：默认，每个容器中只有一个Bean只有实例；
2. prototype：为每个Bean请求提供一个实例，在每次注入时都会创建一个新的对象；
3. request：每个http请求中创建一个单例对象；
4. session：每个session中只有一个Bean的实例；
5. globalSession：在一个全局的HTTP Session中，一个bean定义对应一个实例。典型情况下，仅在使用portlet context的时候有效。

# 34.Spring中单例Bean是否线程安全
Spring框架并没有对Bean进行多线程的封装处理，如果Bean是有状态的就需要开发人员进行多线程的安全保证。最简单的方法是设置Bean的作用域为prototype，或者使用synchronized、lock等实现线程同步的方法。

# 35.Spring中使用哪些设计模式
1. 工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；
2. 单例模式：Bean默认为单例模式。
3. 代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术；
4. 模板方法：用来解决代码重复的问题。比如：RestTemplate；
5. 观察者模式：Spring中listener的实现ApplicationListener；
6. 适配器模式：SpringMVC中的handler处理。

# 36.Spring的事务实现方式和隔离级别
Spring中有两个事务实现方式：编程式和声明式。编程式就是自己通过代码开启事务，提交事务，遇到异常回滚；声明式就是通过@Transactional注解，Spring会生成一个代理对象给业务逻辑添加事务。

@Transactional可以通过rollbackFor属性配置，针对指定异常进行事务回滚。默认对RuntimeException和Error进行回滚。

Spring的隔离级别就是数据库的隔离级别：
1. read uncommitted（读未提交）
2. read committed（读已提交）
3. repeatable read（可重复读）
4. serializeble（可串行化）

# 37.Spring的事务传播机制
1. REQUIRED：默认值，支持当前事务，如果没有事务会创建一个新的事务
2. SUPPORTS：支持当前事务，如果没有事务的话以非事务方式执行
3. MANDATORY：支持当前事务，如果没有事务抛出异常
4. REQUIRES_NEW：创建一个新的事务并挂起当前事务
5. NOT_SUPPORTED：以非事务方式执行，如果当前存在事务则将当前事务挂起
6. NEVER：以非事务方式进行，如果存在事务则抛出异常
7. NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则进行与REQUIRED类似的操作

# 38.Spring的事务什么情况会失效
Spring的事务原理是AOP，进行了切面增强。那么失效的根本原因就是AOP没起作用，常见的原因：
1. 发生自调用，在类里面通过this调用本类的方法，此时this对象不是代理类；
2. 方法不是public，@Transactional只能作用于public上，否则事务不生效；
3. 数据库不支持事务；
4. 没有被Spring管理；
5. 异常被捕获了，事务不会回滚。

# 39.Spring MVC的工作流程
1. 用户发送请求到前端控制器DispatcherServlet；
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器；
3. 处理器映射器会根据请求，找到负责处理该请求的处理器，并将其封装为处理器执行链返回 (HandlerExecutionChain) 给DispatcherServlet；
4. DispatcherServlet会根据处理器执行链中的处理器，找到能够执行该处理器的处理器适配器HandlerAdaptor；
5. 处理器适配器HandlerAdaptoer会调用对应的具体的Controller；
6. Controller将处理结果封装到ModelAndView中并将其返回给处理器适配器HandlerAdaptor；
7. HandlerAdaptor将ModelAndView交给DispatcherServlet；
8. DisptcherServlet调用视图解析器ViewResolver，将ModelAndView中的视图名称封装为视图对象
9. ViewResolver将封装好的视图View返回给DispatcherServlet；
10. DispatcherServlet调用视图对象，让其自己进行渲染（将模型数据填充至视图中），形成响应对象HttpResponse；
11. 前端控制器DispatcherServlet响应HttpResponse给浏览器。

# 40.Spring MVC的九大组件
1. HandlerMapping

   HandlerMapping是一个接口，内部只有一个方法getHandler()。每个请求都需要一个Handler处理，它的作用是根据request找到对应的Handler。

2. HandlerAdapter

   因为SpringMVC中的Handler可以是任意的形式，只要能处理请求就可以，但是Servlet需要的处理方法的结构是固定的，都是以request和response为参数的方法。任意形式的Handler通过使用适配器，可以“转换”成固定形式，然后交给Servlet来处理。每种Handler都要有对应的HandlerAdapter才能处理请求。

3. HandlerExceptionResolver

   Spring MVC中负责处理异常的类，根据异常设置ModelAndView。之后交给render方法进行渲染，HandlerExceptionResolver只能处理页面渲染之前的异常，页面渲染过程中的异常，它是不能处理的。

4. ViewResolver

   这个接口只有一个方法resolveViewName(String viewName, Locale locale)，用来将String类型的视图名(viewName)和Locale解析为View类型的视图。

5. RequestToViewNameTranslator

   从request中获取ViewName

6. LocaleResolver

   LocaleResolver就是用于从request解析出Locale。

7. ThemeResolver

   解析主题

8. MultipartResolver

   用于处理上传请求将普通的request包装成MultipartHttpServletRequest，就可以调用getFile方法获取File。

9. FlashMapManager

   管理FlashMap的，FlashMap是用在redirect中传递参数。

# 41.Spring Boot的自动装配原理
Spring Boot的启动类会添加一个@SpringBootApplication，该注解包含@SpringBootConfiguration、@EnableAutoConfiguration和@ComponentScan。

@EnableAutoConfiguration中的@import导入了AutoConfigurationImportSelector.class

AutoConfigurationImportSelector的selectImports()通过SpringFactoriesLoader.loadFactoryNames加载META-INF/spring.factories文件

获取key为EnableAutoConfiguration的值，该值就是starter的配置类全路径

Spring将该类加载到容器中

# 42.Spring、Spring MVC和Spring Boot的区别
Spring是一个IOC容器，用来管理Bean，使用依赖注入实现控制反转，可以很方便的整合各种框架；提供AOP机制弥补OOP的代码重复问题，将公共处理封装成切面注入。

Spring MVC是对Spring对web框架的一种解决方案，提供一个总的前端控制器DispatcherServlet用来接受请求，然后定义一套路由（url与hanlder的映射）及适配执行hanlder，最后将hanlder执行结果用视图解析器生成视图返回给前端。

Spring Boot是Spring提供的一个快速开发工具包，让程序员更方便、更快捷的开发Spring应用，简化了配置，通过starter机制整合了一系列的框架。

# 43.MyBatis的优缺点
优点：
1. 基于SQL语句编程，相对灵活；SQL语句配置在xml中，解除SQL语句与代码的耦合。提供xml标签，支持编写动态SQL语句；
2. 兼容多种数据库；
3. 支持对象与数据库的ORM字段关系映射。

缺点：
1. SQL编写工作量大，尤其当多字段、多表关联时，对开发人员编写SQL语句的功底有一定要求；
2. SQL语句依赖数据库，导致数据库移植性差。

# 44.MyBatis中#{}和${}的区别
#{}是预编译处理，是占位符；${}是字符串替换，是拼接符

MyBatis在处理#{}时，会将sql中的#{}替换成？，使用PreparedStatement执行

MyBatis在处理${}时，会将sql中的```${}```替换成变量的值，使用Statement执行

#{}的变量替换是在DBMS中，变量替换后，#{}对应的变量自动加上单引号 ‘’

```${}```的变量替换是在DBMS外，变量替换后，```${}```对应的变量不会加上单引号 ‘’

#{}能防止sql注入，```${}```不能防止sql注入

# 45.MyBatis的插件运行原理
MyBatis插件的运行是基于JDK动态代理 + 拦截器链实现

Interceptor是拦截器，可以拦截Executor, StatementHandle, ResultSetHandler, ParameterHandler四个接口

InterceptorChain是拦截器链，对象定义在 Configuration 类中

拦截器的解析是在XMLConfigBuilder对象的parseConfiguration方法中

# 46.MySQL的聚簇索引和非聚簇索引
聚簇索引：表数据按照索引的顺序来存储的，也就是说索引项的顺序与表中记录的物理顺序一致。对于聚集索引，叶子结点存储了真实的数据行，不再有另外单独的数据页。在一张表上最多只能创建一个聚集索引，因为真实数据的物理顺序只能有一种。

非聚集索引: 表数据存储顺序与索引顺序无关。对于非聚集索引，叶子节点只存储了主键的值和索引列的值。非聚簇索引的存储和数据的存储是分离的，也就是说可能找到了索引但没找到数据，需要根据索引上的值（主键）再次回表查询。

优势：

1. 聚簇索引可以直接获取数据行，非聚簇索引需要回表查询；
2. 聚簇索引对范围查询的效率高，因为数据是按照顺序排列的；

劣势：

1. 维护索引代价大，特别是插入新行或修改主键，数据行需要移到新的位置；
2. 主键过大，会导致辅助索引变大，辅助索引的叶子节点存储的是主键的值和索引列的值。

# 47.Hash索引和B+树索引
Hash索引：

哈希索引就是采用一定的hash算法，把键值换成新的哈希值，检索时不需要类似B+树那样从根节点到叶子节点逐级查找，只需要一次hash算法即可立即定位到相应的位置，速度非常快。

B+树索引：

B+树是有序结构，为了不至于树的高度太高，影响查找效率，在叶子节点上存储的不是单个数据，提高了查找效率； 为了更好的支持范围查询，B+树在叶子节点冗余了非叶子节点数据，为了支持翻页，叶子节点之间通过指针相连；

区别：
1. 如果是等值查询，Hash索引占有绝对的优势，因为只需要经过一次hash算法即可找到相应的键值；当然，前提键值都是唯一的，如果不唯一或者存在Hash冲突，就需要找到该key所在的位置，并且循环链表，直到找到相应数据,降低了hash索引的查询效率。
2. Hash是无序的，如果是范围查询，那么Hash索引就无法起到作用，即使原先是有序的键值，经过hash算法后，也会变成不连续的了。因此，hash索引无法实现范围检索，B+tree索引的叶子节点形成有序链表，便于范围查询；Hash索引无法做like ‘xxx%’ 这样的部分模糊查询，因为需要对完整key做hash计算，定位bucket，而B+tree索引具有最左前缀匹配，可以进行部分模糊查询；
3. Hash索引不支持多列联合索引，对于联合索引来说，Hash索引在计算Hash值的时候是将索引键合并在一起计算hash值，不会针对每个索引单独计算hash值。因此如果用到联合索引得一个或者几个索引时，联合索引无法使用。

# 48.索引的设计原则
查询更快，占用空间更小。

1. 选择唯一性索引，唯一性索引的值是唯一的，可以更快速的通过索引来确定某条记录
2. 经常作为查询条件的字段建立索引
3. 经常需要排序、分组和联合操作的字段建立索引
4. 小表不建议索引(超过200w数据的表，建立索引)
5. 尽量使用短索引，如果索引的值很长，那么查询的速度会受到影响
6. 限制索引的数目，每个索引都需要占用磁盘空间，索引越多，需要的磁盘空间就越大；修改表时，对索引的重构和更新很麻烦
7. 经常更新的字段不适合建立索引
8. 尽量使用前缀索引，如果索引字段的值很长，最好使用值的前缀来索引

# 49.联合索引的最左匹配原则
最左匹配原则：最左优先，以最左边的为起点任何连续的索引都能匹配上。同时遇到范围查询(>、<、between、like)就会停止匹配。

where子句搜索条件顺序调换不影响查询结果，因为Mysql中有查询优化器，会自动优化查询顺序。

# 50.MySQL锁的类型
表锁的两种模式：
1. 共享读锁语法：lock table 表名 read
2. 独占写锁语法：lock table 表名 write

在InnoDB的实现中，行锁有3中主要的算法：
1. Record Lock：对单个行记录上锁，称为记录锁。
2. Gap Lock：对不包含真实存在记录的某一个间隙/范围加锁，称为间隙锁。间隙锁只有一个目的就是在RR、SERIALIZABLE隔离级别下为了防止其他事务插入数据。
3. Next-Key Lock：相当于Record Lock + Gap Lock，对某一个行记录和这条记录与它前一条记录之间的范围/间隙都上锁，称为邻键锁。

在实际场景中，行级锁加锁规则比较复杂，不同的查询条件、不同的索引、不同的隔离级别，加锁的情况可能不同。RC隔离级别下只会对记录加Record Lock，不会加Gap Lock 和 Next-Key Lock。

# 51.MySQL的explain
在select语句前增加explain关键字，MySQL就会返回执行计划的信息。但如果from中包含子查询，MySQL仍会执行该子查询，并把子查询的结果放入临时表中。

执行计划的列介绍：
1. id

   id列的编号是select的序列号，有几个select就有几个id，id是按照select出现的顺序增长的，id列的值越大优先级越高，id相同则是按照执行计划列从上往下执行，id为空则是最后执行。

2. select_type

   SIMPLE：查询语句中不包含UNION或者子查询的查询都算作是SIMPLE类型，连接查询也算是SIMPLE类型

   PRIMARY：对于包含UNION或者子查询的查询来说，它是由几个查询组成的，其中最左边的那个查询的select_type值就是PRIMARY
   
   UNION：对于包含UNION的查询来说，它是由几个查询组成的，其中除了最左边的那个小查询以外，其余的小查询的select_type值就是UNION

   UNION RESULT：MySQL选择使用临时表来完成UNION查询的去重工作，针对该临时表的查询的select_type就是UNION RESULT

   DEPENDENT UNION：UNION中的第二个或后面的select语句，取决于外面的查询

   SUBQUERY：非最外层的select，在子查询中第一个select

   DEPENDENT SUBQUERY：子查询中的第一个select，取决于外面的查询

   DERIVED：派生表的select

3. table

   表示当前行访问的表。当from有子查询时，table列的格式为```<derivedN>```，表示当前查询依赖id=N行的查询，所以先执行id=N行的查询；当有union查询时，table列的格式为```<union1,2>```，1和2表示参与union的行id。

4. partitions

   查询匹配记录的分区，对于非分区表，该值为 NULL。

5. type

   此列表示关联类型或访问类型。也就是MySQL决定如何查找表中的行。依次从最优到最差分别为：system > const > eq_ref > ref > range > index > all
   
   system：该表只有一行（相当于系统表），system是const类型的特例

   const：针对主键或唯一索引的等值查询扫描, 最多只返回一行数据

   eq_ref：当使用了索引的全部组成部分，并且索引是PRIMARY或UNIQUE才会使用该类型

   ref：当满足索引的最左匹配原则，或者索引不是PRIMARY或UNIQUE时才会发生

   range：通常出现在范围查询中，比如in、between、 >、<等，使用索引来检索给定范围的行

   index：扫描全索引拿到结果，一般是扫描某个二级索引，二级索引一般比较少，所以通常比ALL快一点

   ALL：全表扫描，扫描聚簇索引的所有叶子节点

6. possible_keys

   表示在查询中可能用到的索引。如果该列为NULL，则表示没有相关索引，可以通过检查where子句看是否可以添加一个适当的索引来提高性能

7. key

   表示在查询时实际用到的索引。在执行计划中可能出现possible_keys列有值，而key列为null，这种情况可能是表中数据不多，MySQL认为索引对当前查询帮助不大做了全表查询

8. key_len

   表示M在索引里使用的字节数，通过此列可以算出具体使用了索引中的那些列。索引最大长度为768字节，当长度过大时，MySQL会做一个类似最左前缀处理，将前半部分字符提取出做索引。当字段可以为null时，还需要1个字节去记录

9. ref

   表示key列记录的索引中，表查找值时使用到的列或常量。常见的有const、字段名

10. rows

    表示在查询中估计要读取的行数，注意这里不是结果集的行数

11. Extra

    此列是一些额外信息。常见的重要值如下：

    Using index：使用覆盖索引（如果select后面查询的字段都可以从这个索引的树中获取，不需要通过辅助索引树找到主键，再通过主键去主键索引树里获取其它字段值，这种情况一般可以说是用到了覆盖索引）

    Using where：使用 where 语句来处理结果，并且查询的列未被索引覆盖

    Using index condition：查询的列不完全被索引覆盖，where条件中是一个查询的范围

    Using temporary：MySQL需要创建一张临时表来处理查询。出现这种情况一般是要进行优化的

    Using filesort：将使用外部排序而不是索引排序，数据较小时从内存排序，否则需要在磁盘完成排序

    Select tables optimized away：使用某些聚合函数（比如 max、min）来访问存在索引的某个字段时

# 52.RDB和AOF机制
RDB：Redis DataBase，在指定的时间间隔将内存中的数据集写入磁盘。实际操作是fork一个子线程，先将数据写入一个临时文件，写入成功后在替换之前的文件，用二进制压缩存储。

优点：

1. 整个Redis数据库只包含一个dump.rdb文件，方便持久化
2. 容灾性好，方便备份
3. 性能最大化，fork子线程来完成写操作，让主线程继续处理命令

缺点：

1. 数据安全性低，RDB是间隔一段时间进行持久化，如果持久化之间发生故障，会导致数据丢失

AOF：Append Only File，以日志的形式记录服务器处理的非读操作，可以打开文本查看到详细的操作记录。

优点：
1. 数据安全，Redis提供3种策略：不同步、每秒同步和每个操作同步
2. AOF机制的rewrite模式，定期对AOF文件进行重写，以达到压缩的目的

缺点：
1. AOF文件比RDB文件大，且恢复速度慢
2. 数据集大的时候，比RDB启动效率低
3. 运行效率没有RDB高

# 53.Redis的过期键删除策略
1. 定时删除：通过维护一个定时器，过期马上删除，是最有效的，但是也是最浪费cpu时间的
2. 惰性删除：程序在取出键时才判断它是否过期，过期才删除，这个方法对cpu时间友好，对内存不友好
3. 定期删除：每隔一定时间执行一次删除过期键的操作，并限制每次删除操作的执行时长和频率，是一种折中方案

Redis采用了惰性删除和定期删除的策略

# 54.Redis的线程模型
Redis基于Reactor的模型开发了文件事件处理器，这个文件事件处理器是单线程的，所以Redis才叫做单线程模型。它采用IO多路复用机制来监听多个Socket，根据Socket上的事件类型来选择对应的事件处理器处理事件。

文件事件处理器的结构：多个Sot + IO多路复用程序 + cke文件事件分发器 + 事件处理器（命令请求处理器、命令回复处理器、命令应答处理器...）。IO多路复用程序会监听多个Socket，多个Socket可能并发产生不同操作，IO多路复用程序会将Socket放入一个队列中，每次从队列中取出一个Socket给事件分发器，事件分发器分发给对应的事件处理器处理。一个Socket的事件处理完之后，IO多路复用程序才会将队列中下个Socket给事件分发器。

# 55.Redis单线程快的原因
1. Redis是基于内存的操作，CPU不是Redis的瓶颈，Redis的瓶颈可能是机器内存的大小或网络带宽
2. 避免了上下文切换
3. 非阻塞的IO多路复用机制

# 56.缓存穿透、缓存击穿、缓存雪崩
缓存穿透：当用户访问的数据，既不在缓存中，也不在数据库中，没办法构建缓存数据，后续的请求都需要访问数据库，数据库的压力骤增

解决方法：
1. 非法请求的限制

   当有大量恶意请求访问不存在的数据的时候，也会发生缓存穿透，因此在API入口处我们要判断求请求参数是否合理。

2. 缓存空值或者默认值

   数据库未查询到的数据，在缓存中设置一个空值或者默认值并给个很短的过期时间，这样后续请求就可以从缓存中读取到空值或者默认值，返回给应用，而不会继续查询数据库。

3. 使用布隆过滤器快速判断数据是否存在

   在写入数据库数据时，使用布隆过滤器做个标记，然后在用户请求到来时，缓存中找不到数据后，可以通过查询布隆过滤器快速判断数据是否存在，如果不存在，就不用查询数据库。

缓存击穿：如果缓存中的某个热点数据过期了，此时大量的请求访问了该热点数据，就无法从缓存中读取，直接访问数据库，数据库很容易就被高并发的请求冲垮

解决方法：
1. 互斥锁
   
   保证同一时间只有一个业务线程更新缓存，未能获取互斥锁的请求，要么等待锁释放后重新读取缓存，要么就返回空值或者默认值。

2. 不给热点数据设置过期时间
   
   由后台异步更新缓存，或者在热点数据准备要过期前，提前通知后台线程更新缓存以及重新设置过期时间；


缓存雪崩：大量缓存数据在同一时间过期或者Redis故障宕机时，请求都直接访问数据库，从而导致数据库的压力骤增

解决方法：
1. 均匀设置过期时间

   在对缓存数据设置过期时间时，给过期时间加上一个随机数，这样就保证数据不会在同一时间过期。

2. 互斥锁

   当业务线程在处理用户请求时，如果发现访问的数据不在Redis里，就加个互斥锁，保证同一时间内只有一个请求来构建缓存（从数据库读取数据，再将数据更新到Redis里），当缓存构建完成后，再释放锁。未能获取互斥锁的请求，要么等待锁释放后重新读取缓存，要么就返回空值或者默认值。

# 57.Redis的事务原理
1. 事务开始

   multi命令执行，表示一个事务的开始。multi命令会将客户端状态的flags属性中打开redis_multi标识

2. 命令入队

   当一个客户端切换到事务状态后，服务器会根据这个客户端发送来的命令来执行不同的操作：

        如果客户端发送的命令为multi、exec、watch、discard中的其中一个，立即执行这个命令；

        如果客户端发送的是四个命令以外的其他命令，那么服务器并不立即执行这个命令，先检查这个命令格式是否正确。如果不正确，服务器会在客户端状态的flags属性关闭redis_multi标识，并且返回错误信息给客户端；如果正确，将这个命令放入一个事务队列里面，然后向客户端返回QUEUED回复，事务队列是按照FIFO的方式保存入队命令。

3. 事务执行

   客户端发送EXEC命令，服务器执行EXEC命令逻辑

        如果客户端状态的flags属性不包含redis_multi或者包含redis_dirty_cas或者redis_dirty_exec标识，那么就直接取消事务执行；

        否则客户端处于事务状态(flag有redis_multi标识)服务器会遍历客户端事务队列，然后执行事务队列中的所有命令，最后将返回结果全部返回给客户端。

redis单条命令是原子性执行的，但是事务不保证原子性，并且没有回滚

# 58.Redis的集群方案
1. 主从模式
Redis提供了复（replication）功能，可以实现当一台数据库中的数据更新后，自动将更新的数据同步到其他数据库上。在复制的概念中，数据库分为两类，一类是主数据库（master），另一类是从数据库(slave）。主数据库可以进行读写操作，当写操作导致数据变化时会自动将数据同步给从数据库。而从数据库一般是只读的，并接受主数据库同步过来的数据。一个主数据库可以拥有多个从数据库。

优点：

    支持主从复制，主机会自动将数据同步到从机，可以进行读写分离；

    Master Server是以非阻塞的方式为Slaves提供服务，所以在Master-Slave同步期间，客户端仍然可以提交查询或修改请求；

    Slave Server同样是以非阻塞的方式完成数据同步，在同步期间如果有客户端提交查询请求，Redis则返回同步之前的数据；

缺点：

    Redis不具备自动容错和恢复功能，主机从机的宕机都会导致前端部分读写请求失败，需要等待机器重启或者手动切换前端的IP才能恢复；

    主机宕机，宕机前有部分数据未能及时同步到从机，切换IP后还会引入数据不一致的问题，降低了系统的可用性；

    如果多个Slave断线了，需要重启的时候，尽量不要在同一时间段进行重启。因为只要Slave启动，就会发送sync请求和主机全量同步，当多个Slave重启的时候，可能会导致Master IO剧增从而宕机。

    Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂；

2. 哨兵模式
哨兵模式是基于主从模式，Redis提供了哨兵的命令，哨兵是一个独立的进程，它独立运行。其原理是哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例。当哨兵监测到Redis主机宕机，会自动将slave切换成master，然后通过发布订阅模式通知其他服务器，修改配置文件。

一个哨兵进程对Redis服务器进行监控，可能会出现故障，为此可以使用多个哨兵进行监控，各个哨兵之间还会互相监控，形成多哨兵模式。

优点：

    哨兵模式是基于主从模式的，所有主从的优点，哨兵模式都具有；

    主从可以自动切换，系统更健壮，可用性更高。

缺点：

    Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。

3. Cluster模式
在redis3.0上加入了Cluster集群模式，实现了Redis的分布式存储，也就是说每台Redis节点上存储不同的内容。

在Redis的每一个节点上，都有这么两个东西，一个是插槽（slot），它的的取值范围是：0-16383；还有一个就是cluster，可以理解为是一个集群管理的插件。当我们的存取的Key，Redis会根据crc16的算法得出一个结果，然后把结果对16384取余，找到对应的插槽所对应的节点，然后直接自动跳转到对应的节点上进行存取操作。

为了保证高可用，redis-cluster集群引入了主从模式，一个主节点对应一个或者多个从节点，当主节点宕机的时候，就会启用从节点。如果主节点和它的从节点都宕机了，那么该集群就无法再提供服务了。