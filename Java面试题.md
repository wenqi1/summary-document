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

内部类和外部类是处于同一个级别的，内部类不会因为定义在方法中就会随着方法的执行完毕而销毁。这里会产生一个问题，当外部类的方法执行结束时，局部变量就会被销毁，但此时内部类可以还要访问该变量。为了解决这个问题，将局部变量复制了一份作为内部类的成员变量，当局部变量被销毁后，依然可以访问。这样会产生一个新的问题，怎么保证两个变量的一致性，就是将局部变量设置为final，一旦初始化不让再修改。

# 5.String、StringBuilder和StringBuffer
String是final修饰的不可变，每次操作都会产生一个新的对象

StringBuilder和StringBuffer是在原对象上操作

StringBuffer是线程安全的，方法都是synchronized修饰的

# 6.重载和重写
重载：发生在同一个类中，方法名必须相同，参数列表必须不同，返回值和访问修饰符可以不同

重写：发生在父子类中，方法名和参数列表必须相同，返回值范围小于等于父类，抛出异常范围小于等于父类，访问修饰符大于等于父类

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

# 59.CAP理论
CAP理论，指的是在一个分布式系统中，Consistency(一致性)、Availability(可用性)、Partition Tolerance(分区容错性)，不能同时成立。

一致性：对于客户端的每次读操作，要么读到的是最新的数据，要么读取失败。换句话说，一致性是站在分布式系统的角度，对访问本系统的客户端的一种承诺：要么我给您返回一个错误，要么我给你返回绝对一致的最新数据，不难看出，其强调的是数据正确。

可用性：任何客户端的请求都能得到响应数据，不会出现响应错误。换句话说，可用性是站在分布式系统的角度，对访问本系统的客户的另一种承诺：我一定会给您返回数据，不会给你返回错误，但不保证数据最新，强调的是不出错。

分区容忍性：由于分布式系统通过网络进行通信，网络是不可靠的。当任意数量的消息丢失或延迟到达时，系统仍会继续提供服务，不会挂掉。换句话说，分区容忍性是站在分布式系统的角度，对访问本系统的客户端的再一种承诺：我会一直运行，不管我的内部出现何种数据同步问题，强调的是不挂掉。

对于一个分布式系统而言，P是前提，必须保证，因为只要有网络交互就一定会有延迟和数据丢失，这种状况我们必须接受，必须保证系统不能挂掉。所以只剩下C、A可以选择。要么保证数据一致性（保证数据绝对正确），要么保证可用性（保证系统不出错）。

当选择了C（一致性）时，如果由于网络分区而无法保证特定信息是最新的，则系统将返回错误或超时。

当选择了A（可用性）时，系统将始终处理客户端的查询并尝试返回最新的可用的信息版本，即使由于网络分区而无法保证其是最新的。

# 60.BASE理论
BASE理论是Basically Available(基本可用)，Soft State（软状态）和Eventually Consistent（最终一致性）三个短语的缩写。

其核心思想是：既是无法做到强一致性（Strong consistency），但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性（Eventual consistency）。

基本可用：什么是基本可用呢？假设系统，出现了不可预知的故障，但还是能用，相比较正常的系统而言：

    响应时间上的损失：正常情况下的搜索引擎0.5秒即返回给用户结果，而基本可用的搜索引擎可以在2秒作用返回结果。

    功能上的损失：在一个电商网站上，正常情况下，用户可以顺利完成每一笔订单。但是到了大促期间，为了保护购物系统的稳定性，部分消费者可能会被引导到一个降级页面。

软状态：什么是软状态呢？相对于原子性而言，要求多个节点的数据副本都是一致的，这是一种“硬状态”

    软状态指的是：允许系统中的数据存在中间状态，并认为该状态不影响系统的整体可用性，即允许系统在多个不同节点的数据副本存在数据延时。

最终一致性：上面说软状态，然后不可能一直是软状态，必须有个时间期限
    
    在期限过后，应当保证所有副本保持数据一致性，从而达到数据的最终一致性。这个时间期限取决于网络延时、系统负载、数据复制方案设计等等因素。

# 61.分布式系统Session共享方式
1. 基于Cookie的Session共享

   将全站用户的Session信息加密、序列化后以Cookie的方式， 统一种植在根域名下，利用浏览器访问该根域名下的所有二级域名站点时，会传递与之域名对应的所有Cookie内容的特性，从而实现用户的Cookie化Session在多服务间的共享访问。


   优点：无需额外的服务器资源；

   缺点：由于受http协议头信息长度的限制，仅能够存储小部分的用户信息；同时Cookie化的Session内容需要进行安全加解密。

2. Redis集中存储Session

# 62.分布式id生成方案
1. uuid
   优点：代码简单，性能好

   缺点：无序的，对范围查询不友好；长度过长，耗数据库性能

2. 雪花算法
雪花算法是Twitter推出的⼀个⽤于⽣成分布式ID的策略，⽣成的ID是⼀个64bit的整数

符号位：第一个bit作为符号位，因为我们生成的都是正数，所以第一个 bit 统一都是 0

时间戳：占用41bit，精确到毫秒。41位最好可以表示2^41-1毫秒，转化成单位年为69年

机器编码：占用10bit，其中高位5bit是数据中心ID，低位5bit是工作节点ID，最多可以容纳1024个节点

序列号：占用12bit，每个节点每毫秒0开始不断累加，最多可以累加到4095，一共可以产生4096个ID

优点：ID是趋势增长的，对范围访问友好。

缺点：强依赖机器时钟，如果时间回调可能产生重复ID。

3. 借助Redis的incr命令获取全局唯一ID

# 63.分布式锁
1. 获取当前时间戳
2. client尝试按照顺序使用相同的key,value获取所有redis服务的锁，在获取锁的过程中的获取时间比锁过期时间短很多，这是为了不要过长时间等待已经关闭的redis服务。比如：TTL为5s，设置获取锁最多用1s，所以如果一秒内无法获取锁，就放弃获取这个锁，从而尝试获取下个锁
3. client通过获取所有能获取的锁后的时间减去第一步的时间，这个时间差要小于TTL时间并且至少有3个redis实例成功获取锁，才算真正的获取锁成功
4. 如果成功获取锁，则锁的真正有效时间是TTL减去第三步的时间差的时间；比如：TTL 是5s,获取所有锁用了2s,则真正锁有效时间为3s(其实应该再减去时钟漂移)
5. 如果客户端由于某些原因获取锁失败，便会开始解锁所有redis实例；因为可能已经获取了小于3个锁，必须释放，否则影响其他client获取锁

# 64.分布式事务
1. 本地消息表

写本地消息和业务操作放在一个事务里，保证了业务和发消息的原子性，要么他们全都成功，要么全都失败。

容错机制：

    扣减余额事务 失败时，事务直接回滚，无后续步骤

    轮序生产消息失败， 增加余额事务失败都会进行重试

本地消息表的特点：

    长事务仅需要分拆成多个任务，使用简单

    生产者需要额外的创建消息表

    每个本地消息表都需要进行轮询

    消费者的逻辑如果无法通过重试成功，那么还需要更多的机制，来回滚操作

适用于可异步执行的业务，且后续操作无需回滚的业务

2. TCC

TCC分为3个阶段

    Try阶段：尝试执行，完成所有业务检查（一致性）, 预留必须业务资源（准隔离性）

    Confirm阶段：确认执行真正执行业务，不作任何业务检查，只使用Try阶段预留的业务资源，Confirm操作要求具备幂等设计，Confirm失败后需要进行重试

    Cancel阶段：取消执行，释放Try阶段预留的业务资源。Cancel阶段的异常和Confirm阶段异常处理方案基本上一致，要求满足幂等设计

TCC特点如下：

    并发度较高，无长期资源锁定

    开发量较大，需要提供Try/Confirm/Cancel接口

    一致性较好，不会发生SAGA已扣款最后又转账失败的情况

TCC适用于订单类业务，对中间状态有约束的业务

# 65.如何实现接口幂等性
1. 每个请求操作必须有唯一的ID，而这个ID就是用来表示此业务是否被执行过的关键凭证；

2. 每次执行业务之前必须要先判断此业务是否已经被处理过；

3. 第一次业务处理完成之后，要把此业务处理的状态进行保存，比如存储到Redis，这样才能防止业务被重复处理。

# 66.简述RabbitMQ的架构设计
从整体上讲RabbitMQ就是一个生产者消费者的模型。我们将中间的整个Broker当作是一个消息中间件的实体，生产者发送消息到Broker，然后消费者从Broker之中获取数据，最终完成数据的通信任务。

1. Broker的结构　　

　　一个RabbitMQ可以称为是一个Broker，一般情况下就当做是一个消息中间件的实例。

　　RabbitMQ将整个Broker划分成多个Vhost，我们可以认为是一个一个的逻辑的空间，这些逻辑空间之间是相互隔离的。

　　一般情况下，在一次的生产和消费的过程之中只会使用一个Vhost。

2. Vhost的结构

　　一个Broker里可以有多个vhost，用作不同用户的权限分离。虚拟主机是共享相同的身份认证和加密环境的独立服务器域。每个Vhost本质上就是一个mini版的rabbitmq服务器，拥有自己的队列、交换器、绑定和权限机制。

3. Exchange

   生产者将消息发送到Exchange，由交换器将消息路由到一个或多个队列中。如果路由不到，或返回给生产者，或直接丢弃，或做其它处理。


4. Queue

　　RabbitMQ中消息只能存储在队列中。多个消费者可以订阅同一个队列，这时队列中的消息会被平均分摊(轮询)给多个消费者进行消费，而不是每个消费者都收到所有的消息进行消费。 
   
   注意：RabbitMQ不支持队列层面的广播消费，如果需要广播消费，可以采用一个交换器通过路由Key绑定多个队列，由多个消费者来订阅这些队列的方式。

5. RoutingKey

   生产者将消息发送给交换器的时候，一般会指定一个RoutingKey，用来指定这个消息的路由规则。这个路由Key需要与交换器类型和绑定键(BindingKey)联合使用才能最终生效。

6. Binding
   
   通过绑定将交换器和队列关联起来，在绑定的时候一般会指定一个绑定键，这样RabbitMQ就可以指定如何正确的路由到队列了。

7. Channel
   
   消息通道，在客户端的每个连接里，可建立多个channel。多路复用连接中的一条独立双向数据流通道。信道是建立在真实的TCP连接内的虚拟连接，AMQP命令都是通过信道发出去的。因为对于操作系统来说建立和销毁TCP都是非常昂贵的开销，所以引入了信道的概念以达到复用一条TCP连接的目的。

# 67.RabbitMQ如何确保消息不被丢失
1. 生产者丢失消息

第一种：开启事务

生产者在发送数据之前开启事务，然后发送消息，如果消息没有成功被rabbitmq接收到，那么生产者会受到异常报错，这时就可以回滚事务，然后尝试重新发送；如果收到了消息，那么就可以提交事物
```java
channel.txSelect();//开启事物
try{
}catch(Exection e){
      channel.txRollback()；//回滚事务
      //重新提交
}
```
rabbitmq事务一旦开启，就会变为同步阻塞操作，生产者会阻塞等待是否发送成功，太耗性能会造成吞吐量的下降。

第二种：开启confirm模式

生产者开启confirm模式之后，每次写的消息都会分配一个唯一的id，然后如何写入了rabbitmq之中，rabbitmq会给你回传一个ack消息，告诉你这个消息发送OK了；如果rabbitmq没能处理这个消息，会回调你一个nack接口，告诉你这个消息失败了。
```java
//开启confirm
channel.confirm();
//发送成功回调
public void ack(String messageId){

}
// 发送失败回调
public void nack(String messageId){
   //重发该消息
}
```
confirm机制是异步的，发送消息之后可以接着发送下一个消息，然后rabbitmq会回调告知成功与否。一般在生产者这块避免丢失，都是用confirm机制。

2. MQ丢失了消息

mq丢失消息可以开启mq的持久化，而且持久化和生产者的confirm机制配合起来，只有消息持久化到了磁盘，才会给生产者发送ack。

3. 消费者丢失消息

使用rabbitmq提供的ack机制，首先关闭rabbitmq的自动ack，然后每次在确保处理完这个消息之后，在代码里手动调用ack。这样就可以避免消息还没有处理完就ack。


# 68.RabbitMQ如何保证消息的顺序
解决消费顺序的问题，通常就是一个队列只有一个消费者。

这样一个个消息按顺序处理，缺点就是并发能力下降了，无法并发消费消息，这是个取舍问题。

如果业务又要顺序消费，又要增加并发，通常思路就是开启多个队列，业务根据规则将消息分发到不同的队列。

# 69.RabbitMQ如何保证消息不被重复消费
让每个消息携带一个全局的唯一ID，即可保证消息的幂等性，具体消费过程为：
1. 消费者获取到消息后先根据ID去查询redis/db是否存在该消息
2. 如果不存在，则正常消费，消费完毕后写入redis/db
3. 如果存在，则证明消息被消费过，直接丢弃

# 70.RabbitMQ的死信队列、延迟队列
消息变成死信有三种原因：
1. 消息被拒绝（Basic.Reject或Basic.Nack）并且设置 requeue 参数的值为 false
2. 消息过期了
3. 队列达到最大的长度

过期时间TTL：RabbitMQ设置TTL有两种方式
1. 通过队列属性x-message-ttl进行设置，队列中的所有消息都有相同的过期时间
2. 通过消息属性expiration进行单独设置，每条消息TTL可以不同

如果同时使用上述两种方法，则消息过期时间以两者之间的TTL较小数值为准。消息在队列的生存时间一旦超过了TTL值，就称为dead message并被投递到死信队列

如果不设置TTL代表此消息永不过期。如果TTL设置为0代表除非此时可以直接将消息投递到消费者，否则该消息被立

DLX(Dead-Letter-Exchange)即死信交换机。当消息在一个队列中变成dead message后，它会被重新发送到另一个交换机，这个交换机就是DLX，绑定DLX的队列称之为死信队列

# 71.Spring Cloud是什么
Spring Cloud并不是一个项目，而是一组项目的集合。在 Spring Cloud中包含了很多的子项目，每一个子项目都是一种微服务开发过程中遇到的问题的一种解决方案。
它利用 Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用 Spring Boot的开发
风格做到一键启动和部署。Spring Cloud并没有重复制造轮子，它只是将目前各家公司开发的比较成熟、经得起实际考验的服务框架组合起来，通过Spring Boot风格进行再封装
屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。

# 72.Spring Cloud Eureka
Eureka 分为 Eureka Server 和 Eureka Client 。Eureka Server作为注册中心属于服务端，而服务提供者和服务消费者相对于注册中心是客户端。同时Eureka Server在启动时默认会自己注册自己，自身作为一个服务，因此Eureka Server也是一个客户端，这是搭建Eureka集群的基础。

服务提供者向注册中心注册服务，隔30秒（默认）发送一次心跳，，如果Eureka Server在90秒（默认）后还未收到服务提供者发来的心跳时，那么它就会认定该服务已经死亡就会注销这个服务。
注意：这里注销并不是立即注销，而是会在60秒以后对在这个之间段内“死亡”的服务集中注销，原因是，集中处理可以避免Eureka出现极大的负担。
此外，Eureka还有自我保护机制，如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障，所以不会再接收心跳，也不会删除服务。

客户端消费者会向注册中心拉取服务列表，因为一个服务器的承载量是有限的，所以同一个服务会部署在多个服务器上，每个服务器上的服务都会去注册中心注册服务，他们会有相同的服务名称但有不同的实例id，所以拉取的是服务列表。最终通过负载均衡来获取一个服务，这样可以均衡各个服务器上的服务。

# 73.Spring Cloud Config
Spring Cloud Config是一个统一管理微服务配置的一个组件，具有集中管理、不同环境不同配置、运行期间动态调整配置参数、自动刷新等功能。

# 74.Spring Cloud Hystric
在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整个体系服务失败，避免级联故障，以提高分布式系统的弹性。

# 75.Spring Cloud openFegin
Feign是一个Http客户端的模板，目标是减少HTTP API的复杂性，希望能将HTTP远程服务调用做到像RPC一样易用。Feign集成RestTemplate、Ribbon实现了客户端的负载均衡
的Http调用，并对原调用方式进行了封装，使得开发者不必手动使用RestTemplate调用服务，而是声明一个接口，并在这个接口中标注一个注解即可完成服务调用，这样更加符合面向
接口编程的宗旨。
        
openFeign在Feign的基础上支持了SpringMVC的注解，如@RequestMapping等。OpenFeign的@FeignClient可以解析SpringMVC的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。

总的来说，openFeign作为微服务架构下服务间调用的解决方案，是一种声明式、模板化的HTTP的模板，使HTTP请求就像调用本地方法一样，通过openFeign可以替代基于RestTemplate的远程服务调用，并且默认集成了Ribbon进行负载均衡。

# 76.Spring Cloud Gateway
网关作为系统的唯一流量入口，封装内部系统的架构，所有请求都先经过网关，由网关将请求路由到合适的微服务，所以，使用网关的好处在于：
1. 简化客户端的工作。网关将微服务封装起来后，客户端只需同网关交互，而不必调用各个不同服务；
2. 降低函数间的耦合度。 一旦服务接口修改，只需修改网关的路由策略，不必修改每个调用该函数的客户端，从而减少了程序间的耦合性；
3. 解放开发人员把精力专注于业务逻辑的实现。由网关统一实现服务路由、负载均衡、访问控制、流控熔断降级等非业务相关功能。

断言（Predicate）：参照Java8的新特性Predicate，允许开发人员匹配HTTP请求中的任何内容，比如请求头或请求参数，最后根据匹配结果返回一个布尔值。

路由（route）：由ID、目标URI、断言集合和过滤器集合组成。如果聚合断言结果为真，则转发到该路由。

过滤器（filter）：可以在返回请求之前或之后修改请求和响应的内容。

# 77.HTTP协议
HTTP是基于TCP处于应用层的一种协议，属于文本格式的协议。

## 请求的格式
HTTP的请求分成四个部分：1、请求行；2、请求报头；3、空行；4、请求正文
1. 请求行：请求行包括三部分，每一部分之间用空格隔开
* HTTP方法：大概，描述了这个请求想要干什么，例如get意思就是想从服务器获取到什么
* URL：描述了要访问的网络上的资源具体是在哪
* 版本号：表示当前使用的 HTTP 的版本是什么，目前常用的版本是1.1

2. 请求头：一般有很多行，每一行都是一个键值对（key: value）
3. 空行：请求头的结束标志
4. 请求正文 ：这一部分可有可无，有时候会存在有时候没有。

## 响应的格式
1. 首行
* 版本号：代表当前HTTP协议的版本
* 状态码：描述了这个响应是表示成功还是失败的，以及不同的状态码也描述了失败的原因，常见的如404
* 状态码的描述：通过一个或是一组简短的单词描述了当前状态码的含义

2. 响应头：键值对结构，每个键值对占一行
3. 空行：响应头的结束标志
4. 响应正文：服务器返回客户端的具体数据，可能会有各种不同的格式

## HTTPS协议
HTTPS 与 HTTP 几乎没有区别，唯一的区别就是 HTTPS 在 HTTP 的基础之上引入了一个加密层。

# 78.Servlet工作机制
1. 加载和实例化

当容器启动或首次请求某个 Servlet 时，容器会读取 web.xml （配置load-on-startup=1，默认为0）或 @WebServlet 中的配置信息，对指定的 Servlet 进行加载。加载成功后，容器会通过反射对 Servlet 进行实例化。

2. 调用 init() 进行初始化

加载和实例化完成后，Servlet 容器会调用init方法（在servlet生命周期内只能调用一次init方法)去初始化 Servlet 实例。

3. 请求响应阶段

Servlet 容器调用 service() 方法来处理来自客户端的请求，并把格式化的响应写回给客户端。

每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务。service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法。
 
4. 服务终止阶段

当服务器关闭，重启或移除 Servlet 实例时Servlet调取destroy()方法进行销毁。

# 类加载过程
加载：把class文件载入内存中
验证：文件格式，程序语义的合法性
准备：为变量分配内存
解析：将常量池内的符号引用替换为直接引用的过程
初始化：对类变量的初始化