# JAVA
## web
- [从StringBuffer开始的面试问题](https://www.hollischuang.com/archives/3912)
## JVM内存结构
- 堆:用于存储对象,被所有线程共享,只存放对象本身,不存放引用和基本类型
- 栈:每个线程包含一个,用于存储基本对象,操作指令和执行环境上下文,每个栈内的数据各自独立,其他栈无法访问
- 方法区: 又称静态区,用于存放整个程序中唯一的元素,class和静态变量.所有线程共享.
- 直接内存: nio的byteBuffer通过调用向系统申请的一块内存,用于堆内存交互,不占用虚拟机内存.
## Java 内存模型
- 内存模型:
  - 程序计数器,每个线程拥有自己的程序计数器,存储当前方法的JVM地址,本地方法为undefined
  - 栈,记录一次次方法调用,只会有一个当前帧,JVM直接对栈的操作只有压栈和出栈
  - 堆,被所有线程共享,放置java对象实例
  - 方法区,所有线程共享,存储元数据,类结构,运行时常量池,变量等
  - 运行时常量池, 存储常量
  - 本地方法栈,每个线程创建一个,支持对本地方法调用
- 内存可见性: 
  - 一个线程堆共享变量的修改能够及时被其它线程看到
  - 线程修改共享变量后,能够及时刷新到主内存
  - 其它线程能够及时将最新值从主内存更新到工作内存
  - java实现方式:synchronize和volatile
- 重排序:
  - 当高速缓存出现堵塞时,CPU可能会重新排列指令顺序,让后面的指令先运行.保证效率.
  - 在保证执行结果一致的情况下,为了提升效率,虚拟机按照自己的规则对程序编写顺序进行打乱
  - 会影响多线程的语义
- 顺序一致性:
  - 一个线程中的所有操作都必须按照程序的顺序执行
  - 所有线程都只能看到一个单一的操作顺序,每个操作都必须是原子的且内存可见的
- volatile:
  - 保证变量可见性,不能保证变量的操作原子性.
  - volatile变量每次被线程访问前,都强迫从主线程中重读该变量的值.当该变量值发生变化时,又会强迫线程把最新值刷新到主内存中
  - volatile 比锁轻量,不会堵塞程序
- 锁: 
  - 能够实现代码的原子性和内存的可见性
  - 线程解锁前,必须把共享变量的值刷新到主内存中
  - 线程加锁前,必须把工作内存中的共享变量值清空,从而使在需要时重新从主内存中拉取
  - 线程加锁前对共享变量值得修改要对其他线程可见
- final:
  - final 可以用来修饰类、方法、变量，分别有不同的意义，
  - final 修饰class代表不能被继承
  - final 修饰变量不可被修改
  - final 修饰方法不可被重写
## 垃圾回收
- 内存分配策略
  - 对象优先在Eden分配
  - 大对象直接进入老年代
  - 长期存活的对象进入老年代
  - 如果相同年龄所有对象大小的总和大于Survivor的一半,大于等于这个年龄的对象进入老年代
  - 空间分配担保,检查老年代最大连续使用空间是否大于新生代对象总空间.否者考虑FUll GC 
- 垃圾回收器
  - 新生代: Serial, ParNew, Parallel Scavenge
  - 老年代: Serial Old, Parallel Old, CMS
  - 整堆: G1
- GC算法
  - 标记清除算法: 检查对象是否可达
  - 复制算法: 将内存分为2块,每次将存活对象复制到另一半,然后整体回收
  - 标记-整理算法: 将存活数据标记,然后移动,整理出连续空间
- GC参数
- 对象存活的判定
  - 引用计数法
  - 引用链法
## Java对象模型
- oop-klass
- 对象头
## 类加载机制
- 加载过程
  - 加载, 链接, 初始化
- 启动类加载器
  - 加载jre/lib下的jar包
- 扩展类加载器
  - 加载jre/lob/ext下的jar包
- 应用类加载器
  - 负责加载classpath下的内容
- classloader
  - 是一个负责加载class的对象,是一个抽象类
- 类加载过程
  - 查看该类是否已加载
  - 查询该类父类加载器进行加载
  - 查找该类进行加载
- 双亲委派
  - 类加载时会请示其parent使用其搜索路径帮忙载入,如果父类找不到,才由自己的搜索路径搜索类
- 模块化
    - jboss module jboss的模块化框架
    - osgi 一个接口编程实现的模块化框架
    - jigsaw jdk9的模块化,改写了classloader
## 虚拟机性能监控与故障处理工具
- jps: 类似于linux ps命令,但只查出所有的java程序,可查看java启动类,传入参数,虚拟机参数
- jstack: jstack pid 查看java程序的线程情况
- jmap: jmap pid 查看运行java程序内存分布情况
- jstat: 监控classloader, compiler, gc相关信息,可以监控资源和性能
- jconsole: 可视化java性能监控
- jinfo: 查看并动态修改jvm的一些参数
- jhat: 分析使用jmap导出的堆信息.使用html展示,支持对象查询语言
- javap: 对java class文件进行反汇编
- btrace: java运行时追踪工具
- TProfiler: 性能分析采样工具
## 编译与反编译
- javac: 本质是一个java程序,将java源码编译成class文件,并进行基本的语法检查  
- javap: 将class文件反汇编
- jad: 将class源码反编译成源码
- CRF: 
# Java基础知识
## 阅读源代码
- String
  - final, serializable,comparable,CharSequence
  - char[] value
- Integer
  - Integercache -128 127
- Long
  - Longcache -128 127 
- Enum
  - 实质是final calss extends Enum<Class> 的语法糖
- BigDecimal
- ThreadLocal
- ClassLoader & URLClassLoader
- ArrayList & LinkedList
- HashMap & LinkedHashMap & TreeMap & ConcurrentHashMap
- HashSet & LinkedHashSet & TreeSet
## String
- 字面量 运行时常量池
- intern 如果常量池没有该字母量创建,并将地址指向该字母量
## 熟悉Java中各种关键字
- transient 保证成员变量不被序列化
- instanceof 判断对象是否是类的子类,实例
- volatile 修饰变量内存可见
## 枚举
- 枚举实现单例模式, 优点:线程安全,代码简介 缺点:内存占用多
## IO
- BIO 服务端启用server socket,客户端启用socket对服务端进行通信, 一个连接一个线程
- NIO 基于reactor 多个连接公用一个线程,一个请求一个线程
- AIO/NIO2 一个有效请求一个线程
- BIO NIO AIO 只对网络IO有意义
## 反射
- javassist 创建动态代理的辅助jar包
- IOC 反射与工厂模式结合
## 注解
- @interface 定义注解
- 类反射isannotation 获取注解
- spring ApplicationContextAware, ApplicationListener<ContextRefreshEvent>
- applicationContext.getBeansWithAnnotation(annotation.class);

## JAVA并发包
- [concurrent包源码阅读](https://www.cnblogs.com/wanly3643/category/437878.html)
### 创建线程执行方式
- Thread
- Runable
- Callable
  - 和Runable区别,方法可以有返回值,抛出异常
  - 需要使用FutureTask支持,new FutureTask(runable)
  - FuctureTask会接收runable线程执行完毕的结果
- Executor
### atomic
- CAS
  - 一种无锁机制
  - 操作包含三个操作数:内存位置(V),预期原值(A),新值(B)
  - 如果内存位置的值与预期原值相匹配,处理器会自动将该位置值更新为新值
  - 优点:操作系统级别支持,效率更高,无锁机制,降低线程等待.
- 四种类型
  - AtomicBoolean
  - AtomicInteger
  - AtomicLong
  - AtomicReferrence
    - AtomicStampeReference 利用AtomicReference实现的一个存储引用和Integer数组的扩展类
### locks
#### CountDownlatch
- 闭锁: 只有所有线程运算全部完成,当前运算才完成
#### ReadWriteLock
#### ReentrantLock
### collections
#### concurrentHashMap
#### concurrentLinkedQueue
#### concurrentSkipListSet
#### concurrentSkipListMap
- 相当于同步的treemap
#### CopyOnWriteArrayList
#### CopyOnWriteArraySet
### tools
### executor
#### ExecutorService
- ThreadPoolExecutor
- ScheduleExeccutorService
  - ScheduleThreadPoolExecutor: 继承了threadPoolExecutor,实现了ScheduleExecutorService
#### Executors
- newFixedThreadPool():创建固定大小的线程池
- newCachedThreadPool():可以根据需求自动更改线程数量
- newSingleThreadExecutor():创建单个线程池
- newScheduledThreadPool():固定大小的线程池,可以延迟或者定时执行任务
### 阻塞队列
#### 无界队列和有界队列
  - 有界队列: 有固定大小的队列,如设定了固定大小的LinkedBlockingQueue,或者长度为0,只做中转的SynchronousQueue
  - 无界队列: 没有设置固定大小的队列,可以直接入列,知道溢出,现实中不会超过Integer.MAX_VALUE
- 通过使用Condition接口实现
#### ArrayBlockingQueue: 数组结构组成的有界阻塞队列,按先进先出顺序
#### LinkedBlockQueue: 链表结构的有界阻塞队列,按先进先出排序
#### PriorityBlockingQueue: 支持优先级排序的无界阻塞队列,靠元素自定义compareTo排序
#### 使用队列优先级实现的无界阻塞队列,使用priorityQueue实现,元素必须实现Delayed接口
#### SynchronousQueue: 一个不存储元素的阻塞队列,每一个put操作必须等待一个take操作,本身不存储元素,只负责将把生产者线程数据直接传递给消费者线程.
#### LinkedTransferQueue: 链表结构的无界队列,多了transfer和tryTransfer方法
  - transfer会将元素立刻传给消费者,如果没有消费者,会将元素放入tail节点,等待元素被消费才返
  - trytransfer会检测是否有消费者等待消费,该方法会立即返回,不会等待元素被消费再返回
#### LinkedBlockingDeque: 链表结构的双向阻塞队列
### ForkAndJoin
- extends RecusiveTask


## Java序列化
- 
## 锁
- 线程安全
  - 多线程下共享的,可修改的状态的正确性.状态一般指数据.
  - 封装 隐藏对象内部状态
  - 不可变 final immutable
- synchronized, reentrantLock去呗
  - reentrantLock可以保证公平性
    - 公平性
    - 倾向将锁赋予等待最久的线程,防止线程饥饿.
  - reentrantLock 因为可以向对象一样使用,可以做到细粒度的同步操作. ArrayBlockingQueue
  - reentrantLock必须手动释放锁
- synchronized, reentrantLock机制基本使用
  - 
- synchronized, reentrantLock底层实现
- 锁膨胀,降级
  - JDK对synchronized的优化
  - 当有线程试图锁定偏斜锁对象,会膨胀为轻量级锁,并重新获取锁,获取失败会进一步膨胀到重量级锁.
- 偏斜锁,自旋锁
  - 在无竞争时,默认会使用偏斜锁,并不是真正的互斥,保证性能
  - 自旋锁:竞争锁的失败的线程，并不会真实的在操作系统层面挂起等待，而是JVM会让线程做几个空循环(基于预测在不久的将来就能获得)，在经过若干次循环后，如果可以获得锁，那么进入临界区，如果还不能获得锁，才会真实的将线程在操作系统层面进行挂起。
- 轻量级锁,重量级锁
  - 内置锁在Java中被抽象为监视器锁（monitor）。在JDK 1.6之前，监视器锁可以认为直接对应底层操作系统中的互斥量（mutex）。这种同步方式的成本非常高，包括系统调用引起的内核态与用户态切换、线程阻塞造成的线程切换等。因此，后来称这种锁为“重量级锁”。
  - 使用轻量级锁时，不需要申请互斥量，仅仅将Mark Word中的部分字节CAS更新指向线程栈中的Lock Record，如果更新成功，则轻量级锁获取成功，记录锁状态为轻量级锁；否则，说明已经有线程获得了轻量级锁，目前发生了锁竞争（不适合继续使用轻量级锁），接下来膨胀为重量级锁。

## 线程
- 线程的定义
  - 系统调度的最小单元,任务的真正执行者,一个进程包含多个线程
  - 有自己的状态
  - 会和其他线程共享文件描述符,虚拟地址空间等
- 线程的生命周期和状态转移
  - 状态
  - 新建,就绪,堵塞,等待,计时等待,终止
- 一个线程多次调用start()
  - 会抛出运行时异常,
- java使用
  - Thread()
  - Runnable()
- ThreadLocal
  - java提供保存线程私有信息的机制
  - 在整个线程的生命周期有效
- 守护线程
  - 必须在线程启动前创建
  - JVM发现只有守护线程存在时,会关闭进程
## 
