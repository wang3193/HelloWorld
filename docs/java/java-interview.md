# JAVA
## JVM内存结构
- 堆:用于存储对象,被所有线程共享,只存放对象本身,不存放引用和基本类型
- 栈:每个线程包含一个,用于存储基本对象,操作指令和执行环境上下文,每个栈内的数据各自独立,其他栈无法访问
- 方法区: 又称静态区,用于存放整个程序中唯一的元素,class和静态变量.所有线程共享.
- 直接内存: nio的byteBuffer通过调用向系统申请的一块内存,用于堆内存交互,不占用虚拟机内存.
## Java 内存模型
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
- String、
  - final, serializable,comparable,CharSequence
  - char[] value
- Integer、
  - Integercache -128 127
- Long、
  - Longcache -128 127 
- Enum、
  - 实质是final calss extends Enum<Class> 的语法糖
- BigDecimal、
- ThreadLocal、
- ClassLoader & URLClassLoader、
- ArrayList & LinkedList、 
- HashMap & LinkedHashMap & TreeMap & ConcurrentHashMap、
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
-  