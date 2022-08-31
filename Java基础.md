# 1. Java修饰符

## 1.1 访问修饰符

Java中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。Java 支持 4 种不同的访问权限。

- **default** (默认）: 同一包内可见，不使用任何修饰符。
- **private** : 同一类内可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
- **protected** : 同一包内的类和所有子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
- **public** : 对所有类可见。

## 1.2  非访问修饰符

### 1.2.1 final 修饰符

修饰变量：变量一旦赋值后，不能被重新赋值。被 final 修饰的实例变量必须显式指定初始值。

修饰方法：父类中的 final 方法可以被子类继承，但是不能被子类重写。 

修饰类：final 类不能被继承。 

### 1.2.2 abstract修饰符

抽象类：

1. 抽象类不能被实例化，声明抽象类的唯一目的是为了将来对该类进行扩充。
2. 一个类不能同时被 abstract 和 final 修饰。
3. 如果一个类包含抽象方法，那么该类一定要声明为抽象类。
4. 抽象类可以包含抽象方法和非抽象方法。

抽象方法：

1. 抽象方法是一种没有任何实现的方法，该方法的的具体实现由子类提供。

2. 抽象方法不能被声明成 final 和 static。

3. 抽象方法的声明以分号结尾，例如：**public abstract sample();**。

### 1.2.3 synchronized 修饰符

 synchronized 关键字声明的方法同一时间只能被一个线程访问。synchronized 修饰符可以应用于四个访问修饰符。 

### 1.2.4 transient修饰符

被 transient 修饰的实例变量，不会被序列化。

### 1.2.5 volatile修饰符

1. volatile 修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该成员变量的值。

2. 当成员变量发生变化时，会强制线程将变化值回写到共享内存。

### 1.2.6 default修饰符

JDK1.8新增的，实现接口的默认方法。

# 2. Java的抽象类和接口

## 2.1 相同点

1. 接口和抽象类都不能被实例化

2. 抽象类和接口都是用来抽象具体对象的，要面向抽象编程，而不要面向具体编程。虽然都进行了不同程度的抽象，但是接口的抽象级别最高。

## 2.2 不同点

1. 抽象类要被子类继承，而接口要被子类实现的。

2. 接口只能做方法声明，不能有方法的实现（JDK1.8后有默认实现），抽象类可以做方法实现。

3. 实现接口的关键字为implements，继承抽象类的关键字为extends。一个类可以实现多个接口，但一个类只能继承一个抽象类。接口一方面解决Java的单继承问题，另一方面，实现了强大的可插拔性（多态机制）。

4. 抽象类可以有抽象方法、具体的方法和属性，而接口只能有抽象方法和不可变常量。接口里定义的变量只能是公共的静态的常量(public static final)。

5. 接口是设计的结果，抽象类是重构的结果。抽象类主要用来抽象类别，强调的是所属关系，接口主要用来抽象功能，强调的是特定功能的实现。

# 3. Java的方法重载和覆盖

## 3.1 重载（Overload）

项目中，有时候为了使代码更加的优雅，方法名为了统一好记，可以使用Java的重载方法。

方法重载的条件：

1. 方法重载只出现在同一个类中
2. 方法名称相同
3. 方法参数列表不同（参数类型、个数、顺序至少有一个不同）

## 3.2 覆盖（Override）

方法的覆盖的条件：

1. 必须要有继承关系，即覆盖指的是子类对超类的某个方法进行重写
2. 覆盖只能出现在子类中，如果没有继承关系，不存在覆盖
3. 在子类中被覆盖的方法，必须和父类中的方法完全一样（方法名，返回类型、参数列表）
4. 子类方法的访问权限不能小于父类方法的访问权限
5. 子类方法不能抛出比父类方法更大的异常，但可以抛出子异常

# 4. Java的反射

## 4.1 反射的概念

Java的反射（reflection）机制是指在程序的运行状态中，可以构造任意一个类的对象，可以了解任意一个对象所属的类，可以了解任意一个类的成员变量和方法，可以调用任意一个对象的属性和方法。这种动态获取程序信息以及动态调用对象的功能称为Java语言的反射机制。反射被视为动态语言的关键。

## 4.2 反射的机制

程序运行时，Java系统会一直对所有对象进行运行时类型识别，这项信息记录了每个对象所属的类。通过专门的类可以访问这些信息。用来保存这些信息的类是Class类，Class类为编写可动态操纵的Java代码程序提供了强大功能。 

## 4.3 构造Class类的三种方法

```java
// Class的forName方法
Class clazz = Class.forName("类的全路径");

// 通过Class类的静态属性，类名.class
Class clazz = 类名.class;

// Object.getClass()
Class clazz = 实例名.getClass();
```

## 4.4 反射的基本操作

### 4.4.1 类反射

```java
Class clazz = Class.forName("类的全路径");
// 获取包名
String packageName = clazz.getPackage().getName();

// 获取全类名
String fullName = clazz.getName();
// 获取简单类名
String simpleName = clazz.getSimpleName();

// 获取类的访问修饰符
int modifiers = clazz.getModifiers();
String modifierName = Modifier.toString(modifiers);

// 获取父类Class
Class superClass = clazz.getSuperclass();

// 获取实现的接口Class
Class[] interfaces = clazz.getInterfaces();

```

### 4.4.2 构造函数反射

```java
Class clazz = Class.forName("类的全路径");
// 获取所有的构造函数
Constructor[] constructors = clazz.getDeclaredConstructors();
for (Constructor constructor : constructors) {
    // 构造函数的名称
    String name = constructor.getName();

    // 构造函数的访问修饰符
    int modifiers = constructor.getModifiers();
    String modifierName = Modifier.toString(modifiers);

    // 构造函数的所有参数类型的Class
    Class[] parameterTypes = constructor.getParameterTypes();

    // 构造函数抛出的所有异常类型的Class
    Class[] exceptionTypes = constructor.getExceptionTypes();
    
    // 创建对象
    constructor.newInstance(params);
}
```

### 4.4.3 方法反射

```java
// 获取所有的方法
Method[] methods = aClass.getDeclaredMethods();
for (Method method : methods) {
    // 方法的名称
    String name = method.getName();

    // 方法的访问修饰符
    int modifiers = method.getModifiers();
    String modifierName = Modifier.toString(modifiers);

    // 方法返回的类型的Class
    Class<?> returnType = method.getReturnType();

    // 方法的所有参数类型的Class
    Class<?>[] parameterTypes = method.getParameterTypes();

    // 方法的所有抛出异常类型的Class
    Class<?>[] exceptionTypes = method.getExceptionTypes();
    
    // 调用方法
    method.invoke(object, params)
}
```

### 4.4.4 属性反射

```java
// 获取指定属性
Field field = aClass.getDeclaredField("属性名");
field.setAccessible(true);

// 设置属性的值
field.set(object, value);

// 获取属性的值
field.get(object);
```

## 4.5. 反射的缺点

1. 性能问题

   使用反射基本上是一种解释操作，告诉JVM希望做什么并且让它满足我们的要求。用于字段和方法接入时反射要远慢于直接代码。

2. 使用反射会模糊程序内部实际要发生的事情

   程序员希望在源代码中看到程序的逻辑，反射技术绕过了源代码会带来维护问题。反射代码比相应的直接代码更复杂，更难看懂逻辑。
   

# 5. Java的数据类型

| 数据类型 | 内存  | 默认值 |
| :------: | :---: | :----: |
|   byte   | 1字节 |   0    |
|  short   | 2字节 |   0    |
|   int    | 4字节 |   0    |
|   long   | 8字节 |   0L   |
|   char   | 2字节 | \u0000 |
|  float   | 4字节 |  0.0F  |
|  double  | 8字节 |  0.0   |
| boolean  | 1字节 | false  |

# 6. Java的集合

![Java集合继承关系图](.\image\Java集合继承关系图.png)

## 6.1 List、Set和Map特点总结

### 6.1.1 List

1. ArrayList：由数组实现的List，允许对元素进行快速随机访问，但是向List中间插入与移除元素的速度很慢

2. LinkedList：对顺序访问进行了优化，向List中间插入与删除的开销并不大，随机访问则行对较慢。

   下列方法：addFirst(),addLast(),getFirst(),getLast(),removeFirst(),romoveLast()，使得LinkedList可以当作堆栈，队列

   和双向队列使用

### 6.1.2 Set

1. HashSet：按照哈希算法来存储集合中的对象，速度较快。
2. LinkedHashSet：不仅实现了哈希算法，还实现了链表的数据结构，提供了插入和删除的功能。
3. TreeSet：实现了SortedSet接口。
4. EnumSet：专门为枚举类设计的有序集合类，EnumSet中所有元素都必须是指定枚举类型的枚举值。

### 6.1.3 Map

1. HashMap：按照哈希算法来存取key，有很好的存取性能，和HashSet一样。
2. LinkedHashMap：使用双向链表来维护key-value对的次序。
3. TreeMap：一个红黑树数据结构，每个key-value对即作为红黑树的一个节点。实现了SortedMap接口。

## 6.2 HashMap的实现原理

### 6.2.1 HashMap的构造函数

```Java
public HashMap(int initialCapacity, float loadFactor)   // 指定初始容量和负载因子

public HashMap(int initialCapacity)   // 指定负载因子

public HashMap()  // 无参构造，默认的初始容量16和默认的负载因子0.75

public HashMap(Map<? extends K, ? extends V> m)   // 指定集合转成HashMap
```



### 6.2.2 HashMap的存储结构

HashMap是一个数组 + 链表的结构，这种结构能够保证在遍历与增删的过程中，如果不产生hash碰撞，仅需一次定位就可完成。当一个hash值上的链表长度大于8时，该节点上的数据就不再以链表进行存储，而是转成了一个红黑树。 

# 7. 枚举

## 7.1 枚举的特性

1. 枚举类是一种特殊形式的java类；
2. 每一个枚举值代表枚举类的一个实例对象；
3. 枚举类可以声明属性、方法和构造函数，但枚举类中的构造函数必须为私有的；
4. 枚举类可以实现接口或者继承抽象类 。

## 7.2 枚举的使用场景

### 7.2.1 switch中使用

```java
enum Signal {
   GREEN, YELLOW, RED
}

public class TrafficLight {
	Signal color = Signal.RED;

    public void change() {
        switch (color) {
            case RED:
            	color = Signal.GREEN;
            	break;
            case YELLOW:
            	color = Signal.RED;
            	break;
            case GREEN:
            	color = Signal.YELLOW;
            	break;
        }
    }
}
```

### 7.2.2 返回码与返回信息

```java
public enum ServerCode {
    UNKNOWN(-1, "未知错误"),
    SUCCESS(0, "成功"),
    FAILED(1, "失败");

    private int code;
    private String message;

    ServerCode(int code, String message) {
        this.code = code;
        this.message = message;
    }

    public static ServerCode valueOf(int code) {
        for (ServerCode serverCode : values()) {
            if (serverCode.code == code) {
                return serverCode;
            }
        }
        return UNKNOWN;
    }

    @Override
    public String toString() {
        return message;
    }
}
```

### 7.2.3 重构switch

```java
String payType = "weixin";
switch (payType){
    case "weixin":
        System.out.println("微信支付");
        break;
    case "zhifubao":
        System.out.println("支付宝支付");
        break;
    case "wangyin":
        System.out.println("网银支付");
        break;
}
```



```java
public enum PayType {
    WEIXIN{
        @Override
        boolean pay() {
            System.out.println("微信支付");
            return true;
        }
    }, 
    ZHIFUBAO{
        @Override
        boolean pay() {
            System.out.println("支付宝支付");
            return true;
        }
    }, 
    WANGYIN{
        @Override
        boolean pay() {
            System.out.println("网银支付");
            return true;
        }
    };

    abstract boolean pay();
}

PayType weixin = PayType.valueOf("WEIXIN");
weixin.pay();
```



# 8. IO流

![IO类图](.\image\Java的IO类图.png)

## 8.1 流的概念和作用

流是一组有顺序的，有起点和终点的字节集合，是对数据传输的总称或抽象。即数据在设备间传输称之为流。

## 8.2 流的分类

根据数据处理的不同分为：字节流和字符流

根据数据流向不同分为：输入流和输出流

### 8.2.3 字符流和字节流区别

1. 读写单位的不同：字节流以字节（8bit）为单位。字符流以字符为单位，根据码表映射字符，一次可能读多个字节。

2. 处理对象不同：字节流可以处理任何类型的数据，如图片、avi等，而字符流只能处理字符类型的数据。

## 8.3 类的详细介绍

### 8.3.1 InputStream 子类

FileInputStream：创建一个读取文件的字节流

ByteArrayInputStream： 将字节数组转化为输入流  

```java
public ByteArrayInputStream(byte buf[])
public ByteArrayInputStream(byte buf[], int offset, int length) 
```

PipedInputStream： 管道输入流，用于多线程间的通信

```java
// 管道输入与输出实际上使用的是一个循环缓冲数组来实现，这个数组默认大小为1024字节。输入流PipedInputStream从这个循环缓冲数组中读数据，输出流PipedOutputStream往这个循环缓冲数组中写入数据。当这个缓冲数组已满的时候，输出流PipedOutputStream所在的线程将阻塞；当这个缓冲数组首次为空的时候，输入流PipedInputStream所在的线程将阻塞
```

SequenceInputStream：把多个 InputStream 合并为一个 InputStream

```java
public SequenceInputStream(InputStream s1, InputStream s2)
public SequenceInputStream(Enumeration<? extends InputStream> e)
```

FilterOutputStream： 封装其它的输入流，并为它们提供额外的功能 

ObjectInputStream： 将之前使用 ObjectOutputStream 序列化的原始数据恢复为对象，以流的方式读取对象

### 8.3.2 OutputStream 子类

FileOutputStream： 创建一个将字节写入文件的输入流

ByteArrayOutputStream： 可以捕获内存缓冲区的数据，转换成字节数组

```java
ByteArrayOutputStream bOutput = new ByteArrayOutputStream(12);
while( bOutput.size()!= 10 ) {
    // 获取用户输入值
    bOutput.write(System.in.read());
}
byte b [] = bOutput.toByteArray();
```

PipedOutputStream：管道输出流，用于多线程间的通信

FilterOutputStream： 封装其它的输出流，并为它们提供额外的功能 

ObjectOutputStream：将对象序列化后写入指定文件

### 8.3.3 Reader 子类

FileReader： 创建一个字符输入流

CharArrayReader： 将字符数组转化为输入流 

BufferedReader： 反冲字符输入流

PipedReader： 主要用途也是在线程间通讯，不过这个可以用来传输字符

### 8.3.4 Writer子类

FileWriter： 创建一个字符输出流

CharArrayWriter： 可以捕获内存缓冲区的数据，转换成字符数组

BufferedWriter： 反冲字符输出流

PipedWriter： 主要用途也是在线程间通讯，不过这个可以用来传输字符

# 9. 多线程

## 9.1 多线程的创建

```java
// 继承Thread的方式
public class ThreadA extends Thread {
    @Override
    public void run() {
        System.out.println("执行任务");
    }

    public static void main(String[] args) {
        ThreadA threadA = new ThreadA();
        // 启动线程
        threadA.start();
    }
}
```

```java
// 实现Runable的方式
public class ThreadB implements Runnable {
    @Override
    public void run() {
        System.out.println("执行任务");
    }

    public static void main(String[] args) {
        ThreadB threadB = new ThreadB();
        Thread thread = new Thread(threadB);
        thread.start();
    }
}
```

```java
// 实现Callable的方式
public class ThreadC implements Callable<Boolean> {
    @Override
    public Boolean call() throws Exception {
        System.out.println("执行任务");
        return true;
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ThreadC threadC = new ThreadC();
        // 启动方式一
        ExecutorService executorService = Executors.newFixedThreadPool(5);
        Future<Boolean> future = executorService.submit(threadC);
        executorService.shutdown();
        System.out.println(future.get());

        // 启动方式二
        FutureTask<Boolean> futureTask = new FutureTask<>(threadC);
        Thread thread = new Thread(futureTask);
        thread.start();
        System.out.println(futureTask.get());
    }
}
```

## 9.2 线程的状态

![线程的状态图](C:\Users\Leo\Desktop\面试资料\image\线程的状态图.png)

## 9.3 线程的通信

wait()、notify()、notifayAll()：定义在Object类中，因为这三个方法需要用到锁，而锁是任意对象都能充当的，所以这三个方法定义在Object类中。

```java
public class Producer implements Runnable {
    private final List<String> books;
    public Producer(List<String> books) {
        this.books = books;
    }

    @Override
    public void run() {
        try {
            while (true) {
                synchronized (books) {
                    if (books.size() >= 10) {
                        books.notifyAll();
                        books.wait();
                    } else {
                        books.add("书");
                        System.out.println("添加一本书，书店剩余" + books.size() + "本");
                        books.notifyAll();
                        Thread.sleep(1000);
                    }
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class Consumer implements Runnable {
    private final List<String> books;
    public Consumer(List<String> books) {
        this.books = books;
    }

    @Override
    public void run() {
        try {
            while (true) {
                synchronized (books) {
                    if (books.size() == 0) {
                        books.notifyAll();
                        books.wait();
                    } else {
                        books.remove(0);
                        System.out.println("卖出一本书，书店剩余" + books.size() + "本");
                        books.notifyAll();
                        Thread.sleep(1000);
                    }
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class Main {
    public static void main(String[] args) throws IOException {
        ArrayList<String> books = new ArrayList<>();
        Producer producer = new Producer(books);
        Thread p1 = new Thread(producer);
        p1.start();
        Consumer consumer = new Consumer(books);
        Thread c1 = new Thread(consumer);
        c1.start();
    }
}
```



# 10. concurrent 包

## 10.1 原子类

### 10.1.1 原子更新基本类型

```java
// AtomicBoolean:  原子更新布尔类型 
// AtomicInteger:  原子更新整型
// AtomicLong:     原子更新长整型 

// 3个类提供的方法几乎一模一样，以AtomicInteger为例进行详解，AtomicIngeter的常用方法如下： 
// int addAndGet(int delta): 将传入的值与实例中的值相加，并返回结果。 
// boolean compareAndSet(int expect, int update): 如果实例中的值等于预期值，则修改实例的值
// int getAndIncrement(): 返回实例的值，并给实例的值自增长1
// void lazySet(int newValue): 最终会设置成newValue,使用lazySet可能导致其他线程读到旧的值 
// int getAndSet(int newValue): 返回实例的值，并给实例的值设置新值

AtomicInteger atomicInteger = new AtomicInteger();
atomicInteger.compareAndSet(0, 1);
```



### 10.1.2 原子更新数组类型

```java
// AtomicIntegerArray:  原子更新整型数组里的元素
// AtomicLongArray:  原子更新长整型数组里的元素
// AtomicReferenceArray:  原子更新引用类型数组里的元素

AtomicIntegerArray atomicIntegerArray = new AtomicIntegerArray(new int[]{1, 2, 3});
int value = atomicIntegerArray.get(0);
atomicIntegerArray.getAndSet(1, 3);
```

### 10.1.3 原子更新引用类型

```java
// AtomicReference:  原子更新引用类型
```

### 10.1.4 原子更新引用类型字段

```java
// AtomicReferenceFieldUpdater:  原子更新引用类型的字段  
// AtomicIntegerFieldUpdater:  原子更新整型的字段
// AtomicLongFieldUpdater:  原子更新长整型字段
```

## 10.2 并发容器

### 10.2.1 ConcurrentHashMap

对应非并发容器：HashMap

原理：JDK6中采用一种更加细粒度的加锁机制Segment“分段锁”，JDK8中采用CAS无锁算法。

### 10.2.2 CopyOnWriteArrayList

对应非并发容器：ArrayList

原理：利用高并发往往是读多写少的特性，对读操作不加锁，对写操作，先复制一份新的集合，在新的集合上面修改，然后将新集合赋值给旧的引用，并通过volatile 保证其可见性。

### 10.2.3 CopyOnWriteArraySet

对应非并发容器：HashSet

原理：基于CopyOnWriteArrayList实现，其唯一的不同是在add时调用的是CopyOnWriteArrayList的addIfAbsent方法，其遍历当前Object数组，如Object数组中已有了当前元素，则直接返回，如果没有则放入Object数组的尾部，并返回。

### 10.2.4 ConcurrentSkipListMap

对应非并发容器：TreeMap

原理：Skip list（跳表）是一种可以代替平衡树的数据结构，默认是按照Key值升序的。

### 10.2.5 ConcurrentSkipListSet

对应非并发容器：TreeSet

原理：内部基于ConcurrentSkipListMap实现

### 10.2.6 ConcurrentLinkedQueue

对应非并发容器：Queue

原理：基于链表实现的FIFO队列（LinkedList的并发版本）

### 10.2.7 LinkedBlockingQueue

对应非并发容器：BlockingQueue

原理：通过ReentrantLock实现线程安全，通过Condition实现阻塞和唤醒

### 10.2.8 ArrayBlockingQueue

对应非并发容器：BlockingQueue

原理：通过ReentrantLock实现线程安全，通过Condition实现阻塞和唤醒

### 10.2.9 PriorityBlockingQueue

对应非并发容器：BlockingQueue

原理：通过ReentrantLock实现线程安全，通过Condition实现阻塞和唤醒

## 10.3 任务执行器

任务执行器(Executor)是一个接口，它的作用主要是为我们提供任务与执行机制之间的解耦。

### 10.3.1 ExecutorService

ExecutorService继承了Executor表示一个异步执行机制

```java
// 创建corePoolSize为0，最大线程数为整型的最大数，线程keepAliveTime为1min，缓存任务的队列为
// SynchronousQueue的线程池
ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
// 创建固定大小的线程池，缓冲任务的队列为LinkedBlockingQueue,大小为整型的最大数
ExecutorService fixedThreadPool = Executors.newFixedThreadPool(10);
// 创建单例的线程池
ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
// 创建corePoolSize大小的线程池,线程keepAliveTime为0，缓存任务的队列为DelayedWorkQueue
        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(10);
```



```java
/*
corePoolSize（核心线程数）：当提交一个任务到线程池时，线程池会创建一个线程来执行任务，即使其他空闲的基本线程能够执行新任务也会创建线程，等到需要执行的任务数大于线程池核心线程数时就不再创建。如果调用了线程池的prestartAllCoreThreads方法，线程池会提前创建并启动所有基本线程

maximumPoolSize（最大线程数）：线程池允许创建的最大线程数。如果队列满了，并且已创建的线程数小于最大线程数，则线程池会再创建新的线程执行任务。

keepAliveTime（线程空闲时长）：线程池的工作线程空闲后，保持存活的时间

TimeUnit（空闲时长的时间单位）：keepAliveTime的单位

workQueue(任务队列)：用于保存等待执行的任务的阻塞队列，可以选择以下几个阻塞队列
	ArrayBlockingQueue：是一个基于数组结构的有界阻塞队列，此队列按 FIFO（先进先出）原则对元素进行排序
    LinkedBlockingQueue：一个基于链表结构的阻塞队列，此队列按FIFO （先进先出） 排序元素，吞吐量通常要	 高于ArrayBlockingQueue
    SynchronousQueue：一个不存储元素的阻塞队列。每个插入操作必须等到另一个线程调用移除操作，否则插入操	 作一直处于阻塞状态，吞吐量通常要高于LinkedBlockingQueue
    PriorityBlockingQueue：一个具有优先级的无限阻塞队列

threadFactory（线程工厂）：设置创建线程的工厂

handler（拒绝策略）：当队列和线程池都满了，采取一种策略处理提交的新任务
	CallerRunsPolicy ：重试添加当前的任务，自动重复调用 execute() 方法，直到成功。
    AbortPolicy ：对拒绝任务抛弃处理，并且抛出异常。（默认）
    DiscardPolicy ：对拒绝任务直接无声抛弃，没有异常信息
    DiscardOldestPolicy ：对拒绝任务不抛弃，而是抛弃队列里面等待最久的一个线程，然后把拒绝任务加到队列
*/
ExecutorService threadPoolExecutor = new ThreadPoolExecutor(corePoolSize, maximumPoolSize, keepAliveTime, timeUnit, workQueue, threadFactory, handler);
```

### 10.3.2 CompletionService

 CompletionService会根据线程池中Task的执行结果按执行完成的先后顺序排序，任务先完成的可优先获取。

 CompletionService的实现类是ExecutorCompletionService 

```java
// 构造函数
public ExecutorCompletionService(Executor executor)
public ExecutorCompletionService(Executor executor, BlockingQueue<Future<V>> queue)
    
// submit(),提交一个Callable或Runnable类型的任务，并返回Future
public Future<V> submit(Callable<V> callable)
public Future<V> submit(Runnable runnable, V v) 
    
// take(),从结果队列中获取并移除一个已经执行完成的任务的结果，如果没有就会阻塞，直到有任务完成返回结果
public Future<V> take() throws InterruptedException
    
// poll(),从结果队列中获取并移除一个已经执行完成的任务的结果，如果没有就会返回null，该方法不会阻塞
public Future<V> poll()
public Future<V> poll(long timeout, TimeUnit timeUnit)
```

### 10.3.3 ScheduleExecutorService

ScheduledExecutorService继承自ExecutorService，可以延时调度任务，也可以周期调度任务。 

```java
// 在一定延时(delay)之后，运行Runnable任务
schedule(Runnable command,long delay, TimeUnit unit)

// 在一定延时(delay)之后，运行Callable任务
schedule(Callable callable,long delay, TimeUnit unit)

// 在一定延时(initialDelay)之后，开始周期性的运行Runnable任务
scheduleWithFixedDelay(Runnable command,long initialDelay,long period,TimeUnit unit)

// 在一定延时(initialDelay)之后，开始周期性的运行Runnable任务，如果任务的执行时间大于等待周期(period)
// ，上一次任务执行完成之后，立即开始下一次任务
scheduleAtFixedRate(Runnable command,long initialDelay,long period,TimeUnit unit)
```

## 10.4 并发锁

![Java主流锁](.\image\Java主流锁.png)

### 10.4.1 ReentrantLock

重入锁

### 10.4.2 ReentrantReadWriteLock

读写锁

### 10.4.3 LockSupport

一个线程工具类，所有的方法都是静态方法，可以让线程在任意位置阻塞，也可以在任意位置唤醒。 

它的内部其实两类主要的方法：park（停车阻塞线程）和unpark（启动唤醒线程）。 

```java
// blocker是用来记录线程阻塞时被谁阻塞的
// 阻塞当前线程
public static void park(Object blocker)
// 阻塞当前线程指定时长
public static void parkNanos(Object blocker, long nanos)
// 阻塞当前线程到指点时间点
public static void parkUntil(Object blocker, long deadline)
// 阻塞当前线程
public static void park()
// 阻塞当前线程指定时长
public static void parkNanos(long nanos)
// 阻塞当前线程到指点时间点
public static void parkUntil(long deadline)

// 恢复线程
public static void unpark(Thread var0)
```

```java
public class LockSupportTest {
    public static void main(String[] args) {
        Thread thread = new Thread("T"){
            @Override
            public void run() {
                System.out.println("开启线程T");
                LockSupport.park();
                System.out.println("线程T运行结束");
            }
        };
        thread.start();
        LockSupport.unpark(thread);
    }
}
```

wait和notify都是Object中的方法，在调用这两个方法前必须先获得锁对象，但是park不需要。

notify只能随机选择一个线程唤醒，unpark可以唤醒一个指定的线程。

## 10.5 线程同步

### 10.5.1 CountDownLatch

```java
public CountDownLatch(int count)
```

CountDownLatch是通过一个计数器来实现的，计数器的初始值为线程的数量；调用await()的线程会被阻塞，调用countDown()计数器减1，直到计数器减到 0，才能继续往下执行。

### 10.5.2 CyclicBarrier

```java
public CyclicBarrier(int parties)
// parties 是参与线程的个数
// barrierAction 是最后一个到达线程要做的任务
public CyclicBarrier(int parties, Runnable barrierAction)
```

CountDownLatch 是一次性的，CyclicBarrier 是可循环利用的

### 10.5.3 Semaphore

```java
public Semaphore(int permits)

// permits 表示许可线程的数量
// fair 表示公平性
public Semaphore(int permits, boolean fair) 
    
acquire(n) // 表示获取个n许可，否则进去阻塞状态
release(n) // 表示释放n个许可
int availablePermits()  // 返回此信号量中当前可用的许可数
int drainPermits() // 获取所有的许可
boolean hasQueuedThreads()   // 查询是否有线程正在等待获取
int getQueueLength() // 获取等待的许可的线程个数
```

 可用于流量控制，限制最大的并发访问数。 

### 10.5.4 Phaser

Phaser又称“阶段器”，用来解决控制多个线程分阶段共同完成任务的情景问题 。与CountDownLatch和CyclicBarrier类似，都是等待一组线程完成工作后再执行下一步。但CountDownLatch和CyclicBarrier不可以动态配置parties，而Phaser可以动态注册需要协调的线程，相比CountDownLatch和CyclicBarrier更加灵活。

```java
int register()  // 动态添加一个parties

int bulkRegister()  // 动态添加多个parties

int getRegisteredParties() // 获取当前的parties数

int arriveAndAwaitAdvance()  // 到达并等待其他线程到达

int arriveAndDeregister()  // 到达并注销该parties，这个方法不会使线程阻塞

int arrive()   // 到达，但不会使线程阻塞

// phase：阶段数值
// 等待前行，可阻塞也可不阻塞，判断条件为传入的phase是否为当前phaser的phase。如果相等则阻塞，反之不阻塞
int awaitAdvance(int phase)  

// phase：阶段数值
// timeout：超时时间
// unit：时间单位
// 与awaitAdvance类似，唯一不一样的就是它可以进行打断
int awaitAdvanceInterruptibly(int phase)
int awaitAdvanceInterruptibly(int phase, long timeout, TimeUnit unit)

int getArrivedParties()  // 获取当前到达的parties数

int getUnarrivedParties()  // 获取当前未到达的parties数

int getPhase()  // 获取当前属于第几阶段，默认从0开始，最大为integer的最大值

boolean isTerminated()  // 判断当前phaser是否关闭

void forceTermination()  // 强制关闭当前phaser
```

### 10.5.5 Exchanger

 用于两个工作线程之间交换数据的封装工具类。

```java
// 无参构造
Exchanger()

// 等待另一个线程到达此交换点，然后将给定的对象传送给该线程，并接收该线程的对象
V exchange(V v) 

// 在指定时长，等待另一个线程到达此交换点，然后将给定的对象传送给该线程，并接收该线程的对象
V exchange(V v, long timeout, TimeUnit unit)

```

