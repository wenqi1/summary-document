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
1. BootstarpClassLoader是ExtClassLoader的父加载器，负责加载%JAVA_HOME%/lib下的jar包和class文件；
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

@Transactional可以通过rollbackFor属性配置，针对指定异常进行事务回滚。默认对RunnablException和Error进行回滚。

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
Spring是一个IOC容器，用来管理Bean，使用依赖注入实现控制反转，可以很方便的整合各种框架；提供AOP机制弥补OOP的代码重复问题，将公共处理封装成切面注定注入。

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
