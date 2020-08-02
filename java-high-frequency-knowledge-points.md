java基础

1.   [深入理解hashCode和equals方法](https://www.cnblogs.com/panxuejun/p/5866869.html)

   >- hashCode方法是调用object的hashCode方法计算出hashCode值，然后根据hashCode值计算出对应的位置是否存在元素（hashCode|length-1），如果存在则使用equals()方法进行比较，如果相等则属于同一个元素，如果不相等则属于hash冲突，需要解决hash冲突. 
   >
   >- object的equal相当于==，是比较两个对象的内存地址是否相等；string方法重写了equal方法，比较的是两个对象的值是否相等

2. [java中基本数据类型所占的内存空间](https://www.cnblogs.com/gu-bin/p/9859103.html)

   | boolean | 1  (理论上占用1bit,1/8字节，实际处理按1byte处理) |                |
   | ------- | ------------------------------------------------ | -------------- |
   | byte    | 1                                                | －128～127     |
   | short   | 2                                                | －32768～32767 |
   | char    | 2                                                |                |
   | Int     | 4                                                |                |
   | float   | 4                                                |                |
   | long    | 8                                                |                |
   | double  | 8                                                |                |

3. [jdk8的新特性](http://www.jianshu.com/p/5b800057f2d8)

   > - 函数式接口  (functionalInterface注解  要求只有一个抽象方法）lambdad表达式
   > - 接口的默认方法和静态方法
   > - 集合之流式操作
   
4.   [ Atomic类如何保证原子性](https://blog.csdn.net/weixin_44902907/article/details/104745116)

     > 使用cas操作。 java的unsafe类。cas使用版本号来解决A-B-A问题，1A-2B-3A，
     
5.   谈谈你对 Java 平台的理解？“Java 是解释执行”，这句话正确吗？

     > java特点：一次编译，处处运行的跨平台能力；提供垃圾收集功能；
     >
     > java包括：jre 包括jvm以及java类库，jdk除此之外还包括编译器以及一些诊断工具；
     >
     > java代码由源代码通过javac编译成字节码文件，然后有jvm通过内嵌的解释器（JIT）让热点的字节码编译成机器码，这种是java的编译执行。其实java的 JDK 8 实际是解释和编译混合的一种模式。

6.   Exception和error的区别

     > Exception 和 Error 都是继承了 Throwable 类，他们提现了java平台对不同异常的处理。
     >
     > - Exception是程序正常运行中可以预料到的错误，需要捕获进行处理的；Exception异常分为可检查和不检查异常，`检查型异常`必须在编译期检查，然后显示的捕获处理。`非检查型的异常`是运行时异常，比如NullPointerException、ArrayIndexOutOfBoundsException异常，可以通过编码避免的逻辑错误，需要根据需要进行捕获，编译器不需要处理
     >
     > - ERROR是在正常情况下不会出现的错误，绝大多数的ERROR会导致程序处于非正常的、不可恢复的状态（一般是虚拟机异常）。比如OutOfMemoryError
     >
     >   ![img](https://static001.geekbang.org/resource/image/ac/00/accba531a365e6ae39614ebfa3273900.png)

7.   NoClassDefFoundError 和 ClassNotFoundException 有什么区别

     > - NoClassDefFoundError是当Java虚拟机或者ClassLoader实例试图加载类时，类却找不到了，但是`在编译期是没有问题的，只是在运行期找不到`；比如通过new关键字实例化或者通过方法去加载一个类；
     > - ClassNotFoundException是一种异常，它是java的Class.forName()动态的加载类到虚拟机中，假如根据类路径没有找到，则会抛出该异常；
     >
     > **前者强调编译时没问题，运行时却无法实例化一个类；后者强调运行时无法匹配到指定参数名称的类**。

8.   谈谈 final、finally、 finalize 有什么不同？

     > - final 可以用来修饰类、方法、变量，分别有不同的意义，final 修饰的 class 代表不可以继承扩展，final 的变量是不可以修改的，而 final 的方法也是不可以重写的（override）; 可以用于保护只读数据，有利于减少额外的同步开销；
     >
     > - finally 则是 Java 保证重点代码一定要被执行的一种机制。我们可以使用 try-finally 或者 try-catch-finally 来进行类似关闭 JDBC 连接、保证 unlock 锁等动作。
     >
     >   ```java
     >   try {
     >     // do something
     >     System.exit(1);
     >   } finally{
     >     System.out.println(“Print from finally”);
     >   }
     >   // finally代码块中的代码不会被执行
     >   ```
     >
     > - finalize 是基础类 java.lang.Object 的一个方法，它的设计目的是保证对象在被垃圾收集前完成特定资源的回收。finalize 机制现在已经不推荐使用，并且在 JDK 9 开始被标记为 deprecated。
     
9.   String、StringBuffer、StringBuilder 有什么区别？

     > - String 类是final类，是不可修改的，原生的保证了基础线程安全，他的拼接和裁剪等方法都会创建新的字符串对象；
     >
     > - StringBuffer 是为解决上面提到拼接产生太多中间对象的问题而提供的一个类，我们可以用 append 或者 add 方法（使用synchronized实现了同步），把字符串添加到已有序列的末尾或者指定位置。StringBuffer 本质是一个线程安全的可修改字符序列，它保证了线程安全，也带来了额外的性能开销。
     >
     > - StringBuilder在能力上和 StringBuffer 没有本质区别，但是它去掉了线程安全的部分，有效减小了开销，是绝大部分情况下进行字符串拼接的首选；
     >
     >   `String的intern()` 方法就是扩充常量池的一个方法；当一个String实例str调用intern()方法时，Java查找常量池中是否有相同Unicode的字符串常量，如果有，则返回其的引用，如果没有，则在常量池中增加一个Unicode等于str的字符串并返回它的引用；
     >
     >   [string的intern方法](https://juejin.im/post/5d53cf70f265da03e83b6509)

10. 谈谈 Java 反射机制，动态代理是基于什么原理？

    > -  动态和静态类型的区分：简单区分就是语言类型信息是在运行时检查，还是编译期检查。
    > -  反射机制是赋予程序运行时自省的能力，通过反射可以直接操作类或者对象的定义，获取类声明的属性或者方法，调用方法或者构造对象，甚至可以修改类的定义；Class、Field、Method、Constructor 等，这些完全就是我们去操作类和对象的元数据对应。
    > -  动态代理是方便运行时动态构建代理、动态处理代理方法调用的机制。比如jdk的动态代理就是利用反射机制，当然还有字节码操作机制

11.   int 和 Integer 有什么区别？谈谈 Integer 的值缓存范围。

      > - int是我们常说的整数类型，是java的8个基本数据类型之一。Integer是int的对应的包装类，在java5中引入了自动装箱和拆箱功能。自动装箱是编译器调用valueOf将原始类型值转换成对象，同时自动拆箱时，编译器通过调用类似intValue()将对象类型转换成基本数据类型，发生在编译阶段。
      >
      > - Integer的值的缓存，创建对象一般是直接new一个对象，但是他的大部分数据都是在较小的范围内，为了改善性能，会将-128到127之间的数据缓存起来；
      >
      > - 由于integer没有执行加减乘除的操作，所以进行计算是会进行拆箱。（常见的错误：自动拆箱会也会导致空指针异常）
      >
      >   ```java
      >   java 语句 Integer i = 1; i += 1; 做了哪些事情？
      >   首先 Integer i = 1; 做了自动装箱（使用 valueOf() 方法将 int 装箱为 Integer 类型），接着 i += 1; 先将 Integer 类型的 i 自动拆箱成 int（使用 intValue() 方法将 Integer 拆箱为 int），完成加法运行之后的 i 再装箱成 Integer 类型。
      >   ```

12.   对比 Vector、ArrayList、LinkedList 有何区别？

      > - Vector 是 Java 早期提供的**线程安全的动态数组**，如果不需要线程安全，并不建议选择，毕竟同步是有额外开销的。Vector 内部是使用对象数组来保存数据，可以根据需要自动的增加容量，当数组已满时，会创建新的数组，并拷贝原有数组数据。
      >
      > - ArrayList 是应用更加广泛的**动态数组**实现，它本身不是线程安全的，所以性能要好很多。与 Vector 近似，ArrayList 也是可以根据需要调整容量，不过两者的调整逻辑有所区别，初始容量都是10，Vector 在扩容时会提高 1 倍，而 ArrayList 则是增加 50%。
      >
      > - LinkedList 顾名思义是 Java 提供的**双向链表**，所以它不需要像上面两种那样调整容量，它也不是线程安全的。
      >
      >   [arrayList和linkedList源码](https://blog.csdn.net/u012926924/article/details/47955035)

13.   对比 Hashtable、HashMap、TreeMap 有什么不同？

      > - Hashtable 是早期 Java 类库提供的一个hash表实现，扩展了 Dictionary。它本身是`同步的，不支持 null 键和值，由于同步导致的性能开销`，所以已经很少被推荐使用。
      > - HashMap 是应用更加广泛的哈希表实现，行为上大致上与 HashTable 一致，主要区别在于 `HashMap 不是同步的，支持 null 键和值等`。通常情况下，HashMap 进行 put 或者 get 操作，可以达到常数时间的性能，所以**它是绝大部分利用键值对存取场景的首选**。
      > - TreeMap 则是基于红黑树的一种`提供顺序访问的 Map`，和 HashMap 不同，它的 get、put、remove 之类操作都是 O（log(n)）的时间复杂度，具体顺序可以由指定的 Comparator 来决定，或者根据键的自然顺序来判断。

14.   hashMap为什么是线程不安全的

      >  如果有两个线程A和B，都进行插入数据，刚好这两条不同的数据经过哈希计算后得到的哈希码是一样的，且该位 置还没有其他的数据。所以这两个线程都会进入我在上面标记为1的代码中。假设一种情况，线程A通过if判断，该 位置没有哈希冲突，进入了if语句，还没有进行数据插入，这时候 CPU 就把资源让给了线程B，线程A停在了if语句 里面，线程B判断该位置没有哈希冲突(线程A的数据还没插入)，也进入了if语句，线程B执行完后，轮到线程A执 行，现在线程A直接在该位置插入而不用再判断。这时候，你会发现线程A把线程B插入的数据给覆盖了。发生了线 程不安全情况。本来在 HashMap 中，发生哈希冲突是可以用链表法或者红黑树来解决的，但是在多线程中，可能 就直接给覆盖了。  扩容的时候也会造成线程不安全。
      >
      > 

15. 如何保证容器是线程安全的？ConcurrentHashMap 如何实现高效地线程安全？

    > - Java 提供了不同层面的线程安全支持。在传统集合框架内部，除了 Hashtable 等同步容器，还提供了所谓的`同步包装器`，我们可以调用 Collections 工具类提供的包装方法，来获取一个同步的包装容器（如Collections.synchronizedMap/synchronizedList/synchronizeSet），但是它们都是利用非常粗粒度的同步方式，在高并发情况下，性能比较低下。
    >
    > 另外，更加普遍的选择是利用并发包提供的线程安全容器类，它提供了：
    >
    > - 各种并发容器，比如 ConcurrentHashMap、CopyOnWriteArrayList。
    > - 各种线程安全队列（Queue/Deque），如 ArrayBlockingQueue、SynchronousQueue。
    > - 各种有序容器的线程安全版本等。
    >
    > 具体保证线程安全的方式，包括有从简单的 synchronize 方式，到基于更加精细化的，比如基于分离锁实现的 ConcurrentHashMap 等并发实现等。具体选择要看开发的场景需求，总体来说，并发包内提供的容器通用场景，远优于早期的简单同步实现。

16. [LinkedBlockingQueue和ArrayBlockingQueue的区别](https://blog.csdn.net/Androidlushangderen/article/details/80219264)

    > - ArrayBlockingQueue是有界的，而LinkedBlockingQueue默认是无界的（可以通过指定大小来变为有界）。ArrayBlockingQueue有界就意味着我们使用ArrayBlockingQueue必须指定capacity大小。这样的话，内存空间会直接预先分配好，所以在使用LinkedBlockingQueue无界情况下时要考虑到内存实际使用问题，防止内存溢出问题的发生。
    >- 锁使用的比较。ArrayBlockingQueue内部使用1个锁来控制队列项的插入、取出操作（notEmpty、notFull 都是同一个再入锁的条件变量），而LinkedBlockingQueue则是使用了2个锁来控制，一个名为putLock，另一个是takeLock，但是锁的本质都是ReentrantLock。因为LinkedBlockingQueue使用了2个锁的情况下，所以在一定程度上LinkedBlockingQueue能更好支持高并发的场景操作，这里指的是并发性上，当然根据官方文档的描述LinkedBlockingQueue的吞吐量也比较高。
    > -  SynchronousQueue，在 Java 6 中，其实现发生了非常大的变化，利用 CAS 替换掉了原本基于锁的逻辑，同步开销比较小。它是 Executors.newCachedThreadPool() 的默认队列。

17. 为什么需要 ConcurrentHashMap？

    > -  Hashtable 本身比较低效，因为它的实现基本就是将 put、get、size 等各种方法加上“synchronized”。简单来说，这就导致了所有并发操作都要竞争同一把锁，一个线程在进行同步操作时，其他线程只能等待，大大降低了并发操作的效率。
    >
    > - 前面已经提过 HashMap 不是线程安全的，并发情况会导致类似 CPU 占用 100% 等一些问题，
    >
    > - 那么能不能利用 Collections 提供的同步包装器来解决问题呢？我们发现同步包装器只是利用输入 Map 构造了另一个同步版本，所有操作虽然不再声明成为 synchronized 方法，但是还是利用了“this”作为互斥的 mutex，没有真正意义上的改进！

18. 线程安全的CopyOnWriteArrayList的底层实现

    >  ```java
    > public class CopyOnWriteArrayList<E>
    >     implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
    >     private static final long serialVersionUID = 8673264195747942595L;
    > 
    >     /** The lock protecting all mutators */
    >     final transient ReentrantLock lock = new ReentrantLock();
    > 
    >     /** The array, accessed only via getArray/setArray. */
    >     private transient volatile Object[] array;
    >     public boolean add(E e) {
    >         final ReentrantLock lock = this.lock;
    >        //1、先加锁
    >         lock.lock();
    >         try {
    >             Object[] elements = getArray();
    >             int len = elements.length;
    >            //2、拷贝数组
    >             Object[] newElements = Arrays.copyOf(elements, len + 1);
    >           //3、将元素加入到新数组中
    >             newElements[len] = e;
    >           //4、将array引用指向到新数组
    >             setArray(newElements);
    >             return true;
    >         } finally {
    >            //5、解锁
    >             lock.unlock();
    >         }
    >     }
    > 
    >  ```
    >
    > 由于所有的写操作都是在新数组进行的，这个时候如果有线程并发的写，则通过锁来控制，如果有线程并发的读
    >
    > - 如果写操作未完成，那么直接读取原数组的数据；
    > - 如果写操作完成，但是引用还未指向新数组，那么也是读取原数组数据；
    > - 如果写操作完成，并且引用已经指向了新的数组，那么直接从新数组中读取数据。
    >
    > `缺点`：
    >
    > - 由于写操作的时候，需要拷贝数组，会消耗内存，如果原数组的内容比较多的情况下，可能导致young gc或者full gc
    >
    > - 不能用于实时读的场景，像拷贝数组、新增元素都需要时间，所以调用一个set操作后，读取到数据可能还是旧的，虽然CopyOnWriteArrayList 能做到最终一致性，但是还是没法满足实时性要求；
    >
    > - CopyOnWriteArrayList 合适读多写少的场景，不过这类慎用
    >   因为谁也没法保证CopyOnWriteArrayList 到底要放置多少数据，万一数据稍微有点多，每次add/set都要重新复制数组，这个代价实在太高昂了。在高性能的互联网应用中，这种操作分分钟引起故障。

19. Java 有几种文件拷贝方式？哪一种最高效？

    > - 利用 java.io 类库，直接为源文件构建一个 FileInputStream 读取，然后再为目标文件构建一个 FileOutputStream，完成写入工作。
    > - 利用 java.nio 类库提供的 transferTo 或 transferFrom 方法实现。
    > - 对于 Copy 的效率，这个其实与操作系统和配置等情况相关，总体上来说，NIO transferTo/From 的方式**可能更快**，因为它更能利用现代操作系统底层机制，避免不必要拷贝和上下文切换。

20. 拷贝实现机制分析

    > - 首先，你需要理解用户态空间（User Space）和内核态空间（Kernel Space），这是操作系统层面的基本概念，操作系统内核、硬件驱动等运行在内核态空间，具有相对高的特权；而用户态空间，则是给普通应用和服务使用;
    > - 当我们使用输入输出流进行读写时，实际上是进行了多次上下文切换，比如应用读取数据时，先在内核态将数据从磁盘读取到内核缓存，再切换到用户态将数据从内核缓存读取到用户缓存。
    > - 而基于 NIO transferTo 的实现方式，在 Linux 和 Unix 上，则会使用到零拷贝技术，数据传输并不需要用户态参与，省去了上下文切换的开销和不必要的内存拷贝，进而可能提高应用拷贝性能。注意，transferTo 不仅仅是可以用在文件拷贝中，与其类似的，例如读取磁盘文件，然后进行 Socket 发送，同样可以享受这种机制带来的性能和扩展性提高。
    > - 零拷贝可以理解为内核态空间与磁盘之间的数据传输，不需要再经过用户态空间
    > - `提高类似拷贝等IO操作的特性`:
    >   - 在程序中，使用缓存等机制，合理减少 IO 次数（在网络通信中，如 TCP 传输，window 大小也可以看作是类似思路）。
    >   - 使用 transferTo 等机制，减少上下文切换和额外 IO 操作。
    >   - 尽量减少不必要的转换过程，比如编解码；对象序列化和反序列化

21. 谈谈接口和抽象类有什么区别？

    > - `接口是对行为的抽象，对方法的抽象，它是抽象方法的集合`，利用接口可以达到 API 定义和实现分离的目的。接口，不能实例化；不能包含任何非常量成员，任何 field 都是隐含着 public static final 的意义；同时，没有非静态方法实现，也就是说要么是抽象方法，要么是静态方法。Java 标准类库中，定义了非常多的接口，比如 java.util.List。
    > - `抽象类是对类的抽象`， 抽象类是不能实例化的类，用 abstract 关键字修饰 class，其目的`主要是代码重用`。除了不能实例化，形式上和一般的 Java 类并没有太大区别，可以有一个或者多个抽象方法，也可以没有抽象方法。抽象类大多用于`抽取相关Java 类的共用方法实现或者是共同成员变量`，然后通过继承的方式达到代码复用的目的。Java 标准库中，比如 collection 框架，很多通用部分就被抽取成为抽象类，例如 java.util.AbstractList。

22. 面向对象的基本要素：封装、继承、多态

    > - **封装**的目的是`隐藏事务内部的实现细节，以便提高安全性和简化编程`。封装提供了合理的边界，避免外部调用者接触到内部的细节。我们在日常开发中，因为无意间暴露了细节导致的难缠 bug 太多了，比如在多线程环境暴露内部状态，导致的并发修改问题。从另外一个角度看，封装这种隐藏，也提供了简化的界面，避免太多无意义的细节浪费调用者的精力。
    >
    > - `继承是代码复用的基础机制`，类似于我们对于马、白马、黑马的归纳总结。但要注意，继承可以看作是非常紧耦合的一种关系，父类代码修改，子类行为也会变动。在实践中，过度滥用继承，可能会起到反效果。
    >
    > - **多态**，你可能立即会想到重写（override）和重载（overload）、向上转型。简单说，重写是父子类中相同名字和参数的方法，不同的实现；重载则是相同名字的方法，但是不同的参数，本质上这些方法签名是不一样的。
    >
    >   方法名称和参数一致，但是返回值不同这种不是重载，编译都会出错的。
    >

23. 什么情况下 Java 程序会产生死锁？如何定位、修复？

    > 多是聚焦在多线程场景中的死锁，指两个或多个线程之间，由于互相持有对方需要的锁，而永久处于阻塞的状态。
    >
    > `解决`：定位死锁最常见的方式就是利用 jstack 等工具获取线程栈，然后定位互相之间的依赖关系，进而找到死锁。
    >
    > ```java
    > jstack 4324
    > Found one Java-level deadlock:
    > =============================
    > "线程 2":
    >   waiting to lock monitor 0x00007f82540192a8 (object 0x000000076afc5508, a [Ljava.lang.Byte;),
    >   which is held by "线程 1"
    > "线程 1":
    >   waiting to lock monitor 0x00007f82540168b8 (object 0x000000076afc5518, a [Ljava.lang.Byte;),
    >   which is held by "线程 2"
    > 
    > Java stack information for the threads listed above:
    > ===================================================
    > "线程 2":
    > 	at com.chen.api.util.thread.deadlock.DeadLockDemo.lambda$main$1(DeadLockDemo.java:40)
    > 	- waiting to lock <0x000000076afc5508> (a [Ljava.lang.Byte;)
    > 	- locked <0x000000076afc5518> (a [Ljava.lang.Byte;)
    > 	at com.chen.api.util.thread.deadlock.DeadLockDemo$$Lambda$2/1753447031.run(Unknown Source)
    > 	at java.lang.Thread.run(Thread.java:748)
    > "线程 1":
    > 	at com.chen.api.util.thread.deadlock.DeadLockDemo.lambda$main$0(DeadLockDemo.java:21)
    > 	- waiting to lock <0x000000076afc5518> (a [Ljava.lang.Byte;)
    > 	- locked <0x000000076afc5508> (a [Ljava.lang.Byte;)
    > 	at com.chen.api.util.thread.deadlock.DeadLockDemo$$Lambda$1/1286783232.run(Unknown Source)
    > 	at java.lang.Thread.run(Thread.java:748)
    > 
    > Found 1 deadlock.
    > ```

24. java并发包提供了哪些工具类 ***

    > - 提供了比 synchronized 更加高级的各种同步结构，包括 CountDownLatch、CyclicBarrier、Semaphore 、reentrantLock、reentrantReadWriteLock等，可以实现更加丰富的多线程操作，比如利用 Semaphore 作为资源控制器，限制同时进行工作的线程数量。
    > - 各种线程安全的容器，比如最常见的 ConcurrentHashMap、有序的 ConcurrentSkipListMap，或者通过类似快照机制，实现线程安全的动态数组 CopyOnWriteArrayList 等。
    > - 各种并发队列实现，如各种 BlockedQueue 实现，比较典型的 ArrayBlockingQueue、 SynchorousQueue 或针对特定场景的 PriorityBlockingQueue 等。
    > - 强大的 Executor 框架，可以创建各种不同类型的线程池，调度任务运行等，绝大部分情况下，不再需要自己从头实现线程池和任务调度器。
    >
    > 工作中用到 线程池、阻塞队列、CountDownLatch（countDown、await）等待锁、CyclicBarrier是所有的线程都进行了await操作后进行下一步操作、Semaphore（aquire/release）计数器、CopyOnWriteArrayList、ReentrantLock；future； 

25. [线程池并发执行，批量获取返回结果](https://blog.csdn.net/u010425776/article/details/54580710)

    > - Callable  +future
    > - threadPool.invokeAll
    > - completetionService  阻塞队列获取结果

26. 并发包中的 ConcurrentLinkedQueue 和 LinkedBlockingQueue 有什么区别？

    > - Concurrent 类型基于 lock-free，在常见的多线程访问场景，一般可以提供较高吞吐量；
    > - 而 LinkedBlockingQueue 内部则是基于锁，并提供了 BlockingQueue 的等待性方法。
    > - Concurrent 类型没有类似 CopyOnWrite 之类容器相对较重的修改开销。但是，凡事都是有代价的，Concurrent 往往提供了较低的遍历一致性。你可以这样理解所谓的弱一致性，例如，当利用迭代器遍历时，如果容器发生修改，迭代器仍然可以继续进行遍历。
    >
    > ` 队列的实现`：
    >
    > - 有两个特别的[Deque](https://docs.oracle.com/javase/9/docs/api/java/util/Deque.html)实现，ConcurrentLinkedDeque 和 LinkedBlockingDeque。Deque 的侧重点是支持对队列头尾都进行插入和删除，所以提供了特定的方法。
    > - 


# NIO与IO

- [NIO与IO的区别](https://www.cnblogs.com/aspirant/p/8630283.html) ***

- [nio和io的实现](https://github.com/Snailclimb/JavaGuide/blob/master/docs/java/BIO-NIO-AIO.md)

- [bio /nio/aio](https://www.cnblogs.com/aspirant/p/6877350.html)

  >`区别`
  >
  >1. io面向的是流，nio面向的是缓冲buffer或者说块；所以nio比io快很多；
  >- java io是面向流，每次从中读取一个或者多个字节，直到读取所有的字节，它没有任何缓冲的地方；
  >- nio则是面向缓冲区的，它将数据读取到缓存区，可以在缓冲区中前后移动获取到的数据，更灵活；
  >2. io是阻塞的，nio是非阻塞的；
  >
  > - io的各种流是阻塞的。这意味着，一个请求来来后创建一个线程，当线程调用read/write方法时，该线程是被阻塞的，只能处理一个socket请求。直到一些数据被读取完，在此期间该线程不能干任何其他事；
  > - nio是非阻塞的，一个线程负责接口请求，其它多个线程请求写入一些数据到某个通道，不用等他完全写入，这个线程可以同时干别的事情，所以一个单独的线程现在可以管理多个输入和输出通道（channel）。
  > - NIO通讯是将整个任务切换成许多小任务，由一个线程负责处理所有io事件，并负责分发。它是利用事件监听机制，而不是阻塞监听机制，事件到的时候再触发。NIO线程之间通过wait，notify等方式通讯。保证了每次上下文切换都有意义，减少无谓的进程切换。 
  >
  >3. io没有选择器，nio是有selector选择器，Selector(多路复用器)用于监听多个通道的事件（比如：连接打开，数据到达）。因此，单个线程可以监听多个数据通道
  >
  > 
  >
  >bio是来一个请求开一个线程，nio是一个线程监听所有的套接字，监测到内核缓存区的数据准备好后，调用一个线程去处理，对于那些处理事件的线程来说，它多数时间都是在有效的工作，而bio，处理事件的线程亦是监听socket的线程，只要socket fd没准备好，这个线程就只能阻塞。同样1000个链接，使用nio，只需要10个线程就能搞定，使用bio得需要1000个线程。
  >
  >nio是有一个线程监听多个socketChannal对应的缓冲区是否有数据准备好，事件监听机制。AIO是真正的异步通知，AIO是数据准备好直接回调对应的线程进行处理，使用的是事件和回调机制；
  >
  >
  
- nio的组成部分

  > - Buffer，高效的数据容器，除了布尔类型，所有原始数据类型都有相应的 Buffer 实现。
  >
  > - Channel，类似在 Linux 之类操作系统上看到的文件描述符，是 NIO 中被用来支持批量式 IO 操作的一种抽象。
  >
  > - File 或者 Socket，通常被认为是比较高层次的抽象，而 Channel 则是更加操作系统底层的一种抽象，这也使得 NIO 得以充分利用现代操作系统底层机制，获得特定场景的性能优化，例如，DMA（Direct Memory Access）等。不同层次的抽象是相互关联的，我们可以通过 Socket 获取 Channel，反之亦然。 
  > - Selector，是 NIO 实现多路复用的基础，它提供了一种高效的机制，可以检测到注册在 Selector 上的多个 Channel 中，是否有 Channel 处于就绪状态，进而实现了单线程对多 Channel 的高效管理。

- 区分同步或异步  和 区分阻塞与非阻塞

  > - `同步或异步`:  两个强调的是流程上的；简单来说，同步是一种可靠的有序运行机制，当我们进行同步操作时，后续的任务是等待当前调用返回，才会进行下一步；而异步则相反，其他任务不需要等待当前调用返回，通常依靠事件、回调等机制来实现任务间次序关系。
  > - `阻塞与非阻塞` ：强调的是线程是否阻塞，在进行阻塞操作时，当前线程会处于阻塞状态，无法从事其他任务，只有当条件就绪才能继续，比如 ServerSocket 新连接建立完毕，或数据读取、写入操作完成；而非阻塞则是不管 IO 操作是否结束，直接返回，相应操作在后台继续处理。

- io的的基本介绍

  > - IO 不仅仅是对文件的操作，网络编程中，比如 Socket 通信，都是典型的 IO 操作目标。
  >
  > - 输入流、输出流（InputStream/OutputStream）是用于`读取或写入字节`的，例如操作图片文件。
  > - 而 Reader/Writer 则是`用于操作字符`，增加了字符编解码等功能，适用于类似从文件中读取或者写入文本信息。本质上计算机操作的都是字节，不管是网络通信还是文件读取，Reader/Writer 相当于构建了应用逻辑和原始数据之间的桥梁。
  > - BufferedOutputStream 等 `带缓冲区的实现，可以避免频繁的磁盘读写`，进而提高 IO 处理效率。这种设计利用了缓冲区，将批量数据进行一次操作，但在使用中千万别忘了 flush。

  


# java泛型

 1. [java泛型总结](https://www.cnblogs.com/coprince/p/8603492.html)

    > 1. 编译之后程序会采取去泛型化的措施。也就是说Java中的泛型，只在编译阶段有效。泛型信息不会进入到运行时阶段；
    >
    > 2. 泛型类、泛型接口、泛型方法（`泛型类，是在实例化类的时候指明泛型的具体类型；泛型方法，是在调用方法的时候指明泛型的具体类型`）
    >
    >     ```java
    >    /**
    >      * 泛型方法的基本介绍
    >      * @param tClass 传入的泛型实参
    >      * @return T 返回值为T类型
    >      * 说明：
    >      *     1）public 与 返回值中间<T>非常重要，可以理解为声明此方法为泛型方法。
    >      *     2）只有声明了<T的方法才是泛型方法，泛型类中的使用了泛型的成员方法并不是泛型方法。
    >      *     3）<T表明该方法将使用泛型类型T，此时才可以在方法中使用泛型类型T。
    >      *     4）与泛型类的定义一样，此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型。
    >      */
    >     public <T> T genericMethod(Class<T tClass)throws InstantiationException ,
    >       IllegalAccessException{
    >             T instance = tClass.newInstance();
    >             return instance;
    >     }
    >    ```

#  java反射机制



# 并发多线程

参考：

[多线程常见问题]( http://www.cnblogs.com/xrq730/p/5060921.html)

[java并发编程试题](https://blog.csdn.net/qq_43107323/article/details/104300200)

1. Runnable接口和Callable接口的区别

   > - runnable接口中的run方法的返回值是void
   > - callable接口中的call方法的返回值是泛型的，一般配合future和futureTask对象获取异步操作的结果；  

2. volatile关键字的作用 [volatile关键字深入解析](https://www.cnblogs.com/dolphin0520/p/3920373.html)

   > - 保证了变量在多线程间的可见性，即线程读取到的数据都是最新的数据；
   >
   > - 禁止指令重排序；正常情况下jvm为了获取最佳的性能会进行指令重排序，多线程情况下有可能出现意想不到的问题。在进行指令优化时，不能将在对volatile变量访问的语句放在其后面执行，也不能把volatile变量后面的语句放到其前面执行。
   >
   >   ```java
   >   public class Test {
   >       public volatile int inc = 0;
   >        
   >       public void increase() {
   >           inc++;
   >       }
   >        
   >       public static void main(String[] args) {
   >           final Test test = new Test();
   >           for(int i=0;i<10;i++){
   >               new Thread(){
   >                   public void run() {
   >                       for(int j=0;j<1000;j++)
   >                           test.increase();
   >                   };
   >               }.start();
   >           }
   >            
   >           while(Thread.activeCount()>1)  //保证前面的线程都执行完
   >               Thread.yield();
   >           System.out.println(test.inc);
   >       }
   >   }
   >   ```
   >
   >   也许有些朋友认为是10000。但是事实上运行它会发现每次运行结果都不一致，都是一个小于10000的数字。因为volatile关键字能保证可见性没有错，但是上面的程序错在没能保证原子性。可见性只能保证每次读取的是最新的值，但是volatile没办法保证对变量的操作的原子性。
   >
   >   `举例`
   >
   >   ```
   >   假如某个时刻变量inc的值为10，
   >   
   >   　　线程1对变量进行自增操作，线程1先读取了变量inc的原始值，然后线程1被阻塞了；
   >   
   >   　　然后线程2对变量进行自增操作，线程2也去读取变量inc的原始值，由于线程1只是对变量inc进行读取操作，而没有对变量进行修改操作，所以不会导致线程2的工作内存中缓存变量inc的缓存行无效，所以线程2会直接去主存读取inc的值，发现inc的值是10，然后进行加1操作，并把11写入工作内存，最后写入主存。
   >   
   >   　　然后线程1接着进行加1操作，由于已经读取了inc的值，注意此时在线程1的工作内存中inc的值仍然为10，所以线程1对inc进行加1操作后inc的值为11，然后将11写入工作内存，最后写入主存。
   >   
   >   　　那么两个线程分别进行了一次自增操作后，inc只增加了1。
   >   
   >   　　解释到这里，可能有朋友会有疑问，不对啊，前面不是保证一个变量在修改volatile变量时，会让缓存行无效吗？然后其他线程去读就会读到新的值，对，这个没错。这个就是上面的happens-before规则中的volatile变量规则，但是要注意，线程1对变量进行读取操作之后，被阻塞了的话，并没有对inc值进行修改。然后虽然volatile能保证线程2对变量inc的值读取是从内存中读取的，但是线程1没有进行修改，所以线程2根本就不会看到修改的值。
   >   
   >   　　根源就在这里，自增操作不是原子性操作，而且volatile也无法保证对变量的任何操作都是原子性的。
   >   ```
   >
   >   

4. volatile是如何保证内存可见性的 ，volatile是通过内存屏障和禁止指令重排序来保证内存可见性的

   > - 当写一个volatile变量时，JVM会把该线程对应的本地内存中的共享变量刷新到主内存。
   >
   > - 当读一个volatile变量时，JVM会把该线程对应的本地内存置为无效。线程接下来将从主内存中读取共享变量。

4. volatile是如何保证有序性的

   > - 观察他的汇编语言，加入volatile关键字后，会在总线加一个lock锁前缀指令。lock前缀指令实际上相当于一个`内存屏障（也成内存栅栏）`，它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面

5. sleep和wait方法的区别，join、yield

   > - 他们都会使得线程放弃cpu的执行时间 ；
   > - sleep是线程类的方法，wait是object的方法；
   > - sleep可以在任何地方使用，wait只能在同步方法或者同步代码块中使用；
   > - sleep方法不会释放对象的锁，wait方法会释放对象的锁；
   > - join是将并行运行的线程改为串行；
   > - yield 是告诉调度器，主动让出 CPU。但是有可能马上又参与抢占cpu

5.  ThreadLocal的作用 

   > 它内部维护了一个threadLocalMap对象，key是线程变量，value是保存了每个线程的的本地变量的副本。采用数据隔离、空间换时间的方式保证多线程的安全。其内部条目是弱引用。当 Key 为 null 时，该条目就变成“废弃条目”，相关“value”的回收，往往依赖于几个关键点，即 set、remove、rehash。

7. 如何在两个线程之间共享数据 

   > 采用wait方法 notify以及notifyAll进行唤醒和等待来实现数据共享。
   >
   > 如果我们持有某个对象的 Monitor 锁，调用 wait 会让当前线程处于等待状态，直到其他线程 notify 或者 notifyAll。所以，本质上是提供了 Monitor 的获取和释放的能力，是基本的线程间通信方式。
   
7. 线程池的作用

   > - 避免频繁创建和销毁线程带来的性能损耗；
   > - 提前创建线程，可以降低响应时间；
   > - 更好的规划线程数量，避免线程无节制的创建； 

9. [深入源码分析Java线程池的实现原理](https://www.cnblogs.com/rinack/p/9888717.html)

   >- 线程池底层其实是使用HashSet存储Runnable对象，多余的任务放到阻塞队列

10. [ThreadPoolExecutor线程池的参数](https://www.cnblogs.com/superfj/p/7544971.html)  ***

    > `核心参数`：
    >
    > - corePoolSize：核心线程数
    >
    > - maxPoolSize：最大线程数
    >
    > - keepAliveTime：当线程池中线程数大于核心线程数时，线程的空闲时间如果超过线程存活时间，那么这个线程就会被销毁，直到线程池中的线程数小于等于核心线程数
    >
    > - Unit：时间单位
    >
    > - workQueue：传输和保存等待执行任务的阻塞队列。
    >
    > - threadFactory：用于创建新线程的线程工厂；
    >
    > - Handler：线程饱和策略 
    >
    > `阻塞队列`: 
    >
    > - ArrayBlockingQueue：有界任务队列，基于数组的有界队列，必须指定大小；
    > - LinkedBlockingQueue：无界的链表的任务队列，创建是可以指定队列的大小；
    > - synchronousQueue：这个队列比较特殊，它不会保存提交的任务，而是将直接新建一个线程来执行新来的任务;
    >
    > `线程池为什么要使用阻塞队列而不使用非阻塞队列？`
    >
    > - 阻塞队列可以保证任务队列中没有任务时阻塞获取任务的线程，使得线程进入wait状态，释放cpu资源。
    > - 当队列中有任务时才唤醒对应线程从队列中取出消息进行执行。使得在线程不至于一直占用cpu资源。
    >
    >  `拒绝策略` 
    >
    > - AbortPolicy：丢弃任务并抛出RejectedExecutionException；
    > - CallerRunsPolicy：只要线程池未关闭，会使用调用线程池的线程来执行任务；
    > - discardOldestPolicy：丢弃队列中最老的一个任务；
    > - DiscardPolicy：丢弃任务，不做任何处理。
    >
    > `线程池流程`：①当线程数小于核心线程池没满的时候会创建线程来执行任务；②当线程数大于核心线程数且阻塞队列没满的时候会放入阻塞队列中；③阻塞队列已满，且最大线程池数，此时会创建线程来执行；
    >
    > ④当达到最大线程数后，会采用拒绝策略来处理任务；
    >
    > `线程池的关闭`: 
    >
    > - shutdown()：不会立即终止线程池，而是要等所有任务缓存队列中的任务都执行完后才终止，但再也不会接受新的任务；
    > - shutdownNow()：立即终止线程池，并尝试打断正在执行的任务，并且清空任务缓存队列，返回尚未执行的任务；
    >
    > `线程池线程数`：
    >
    > - IO密集型的（常出现于线程中：数据库数据交互、文件上传下载、网络数据传输等等）：2Ncpu;
    >
    >   我们平时开发的 Web 系统通常都有大量的 IO 操作，任务在执行 IO 操作的时候 CPU 就空闲了下来，这时如果增加执行任务的线程数而不是把任务暂存在队列中，就可以在单位时间内执行更多的任务，大大提高了任务执行的吞吐量。
    >
    > - cpu密集型（复杂算法的）：Ncpu。因为执行 CPU 密集型的任务时 CPU 比较繁忙，因此只需要创建和 CPU 核数相当的线程就好了，多了反而会造成线程上下文切换，降低任务执行效率。所以当当前线程数超过核心线程数时，线程池不会增加线程，而是放在队列里等待核心线程空闲下来。
    >
    > `线程池的类和接口介绍`
    >
    > - Executor接口是一个基础接口，是java前期将任务提交和任务执行细节解耦的方法；execute是唯一方法；
    > - ExecutorService 接口则更加完善，不仅提供 service 的管理功能，比如 shutdown 等方法，也提供了更加全面的提交任务机制，返回Future的submit方法，Callable接口；
    > - Executors类使用的静态工厂方法模式提供了很多创建线程池的方法
    >
    > `四种常用线程池`: 
    >
    > - **newSingleThreadExecutor**：单个线程线程池，只有一个线程的线程池，阻塞队列使用的是LinkedBlockingQueue，大小为整数最大值，队列会太大，有可能会耗尽系统资源。
    >
    > - **newFixedThreadPool**：固定大小的线程池，可以指定线程池的大小，该线程池corePoolSize和maximumPoolSize相等，阻塞队列使用的是LinkedBlockingQueue，大小为整数最大值，队列会太大，有可能会耗尽系统资源
    > - **newCachedThreadPool**：缓存线程池，缓存的线程默认存活60秒。线程的核心池corePoolSize大小为0，核心池最大为Integer.**MAX_VALUE**，阻塞队列使用的是SynchronousQueue。是一个直接提交的阻塞队列，  他总会迫使线程池增加新的线程去执行新的任务，也会耗尽系统资源；
    > - **newScheduledThreadPool**：周期性的执行任务的线程池；scheduleAtFixedRate:是以固定的频率去执行任务；schedultWithFixedDelay:是以固定的延时去执行任务；

11. [forkJoinPool的实现](https://juejin.im/post/5d5766eb6fb9a06ad347274b)

    >  ```java
    > static class Task extends RecursiveTask<Integer> {
    > 
    >         private int start;
    > 
    >         private int end;
    >         private int mid;
    > 
    >         public Task(int start, int end) {
    >             this.start = start;
    >             this.end = end;
    >         }
    > 
    >         @Override
    >         protected Integer compute() {
    >             int sum = 0;
    >             if (end - start < 6) {
    >                 // 当任务很小时，直接进行计算
    >                 for (int i = start; i <= end; i++) {
    >                     sum += i;
    >                 }
    >                 System.out.println(Thread.currentThread().getName() + " count sum: " + sum);
    >             } else {
    > 
    >                 mid = (end - start) / 2 + start;
    >                 Task leftTask = new Task(start, mid);
    >                 Task rightTask = new Task(mid + 1, end);
    >                 leftTask.fork();
    >                 rightTask.fork();
    >                 sum += leftTask.join();
    >                 sum += rightTask.join();
    >             }
    >             return sum;
    >         }
    >     }
    > 
    > 
    >     public static void main(String[] args) throws ExecutionException, InterruptedException {
    >         ForkJoinPool forkJoinPool = new ForkJoinPool();
    >         Task countTask = new Task(1, 100);
    >         ForkJoinTask<Integer> result = forkJoinPool.submit(countTask);
    >         System.out.println("result: "+result.get());
    >         forkJoinPool.shutdown();
    >     }
    >  ```
    >
    > 

12. 池化技术

    >  它的核心思想是空间换时间，期望使用预先创建好的对象来减少频繁创建对象的性能开销，同时还可以对对象进行统一的管理，降低了对象的使用的成本。

13. [线程和进程的区别](https://www.zhihu.com/question/21535820)

    > - 线程是cpu调度和分派的基本单位，进程是系统进行调度和资源分配的独立单位，进程可以独立运行；
    > - 一个线程只能属于一个进程，而一个进程可以有多个线程，但至少有一个线程；
    > - 资源分配给进程，同一进程的所有线程共享该进程的所有资源。 

14. 线程安全

    > - 一段代码在多线程和单线程的场景运行的结果是一致的就被称为是线程安全的；
    >
    > - 一般线程安全需要保证原子性（一般通过同步机制实现）、可见性（使用volatile保证）、有序性（保证线程内串行语义）；

15. 线程的声明周期和流转

    > - 新建（NEW），表示线程被创建出来还没真正启动的状态，可以认为它是个 Java 内部状态。
    > - 就绪（RUNNABLE），表示该线程已经在 JVM 中执行，当然由于执行需要计算资源，它可能是正在运行，也可能还在等待系统分配给它 CPU 片段，在就绪队列里面排队。
    > - 在其他一些分析中，会额外区分一种状态 RUNNING，但是从 Java API 的角度，并不能表示出来。
    > - 阻塞（BLOCKED），这个状态和我们前面两讲介绍的同步非常相关，阻塞表示线程在等待 Monitor lock。比如，线程试图通过 synchronized 去获取某个锁，但是其他线程已经独占了，那么当前线程就会处于阻塞状态。
    > - 等待（WAITING），表示正在等待其他线程采取某些操作。一个常见的场景是类似生产者消费者模式，发现任务条件尚未满足，就让当前消费者线程等待（wait），另外的生产者线程去准备任务数据，然后通过类似 notify 等动作，通知消费线程可以继续工作了。Thread.join() 也会令线程进入等待状态。
    > - 计时等待（TIMED_WAIT），其进入条件和等待状态类似，但是调用的是存在超时条件的方法，比如 wait 或 join 等方法的指定超时版本
    > - 终止（TERMINATED），不管是意外退出还是正常执行结束，线程已经完成使命，终止运行，也有人把这个状态叫作死亡。
    > - ![img](https://img2018.cnblogs.com/blog/137084/201908/137084-20190813080541362-1019213130.png)

16. [Synchronized与ReentrantLock的区别](http://www.cnblogs.com/moonandstar08/p/4973079.html) ***

    > `相同点`
    >
    > - 他们都是可重入锁；就是当一个线程试图获取它已经持有的锁的时候会自动成功；
    >
    > `不同点`
    >
    > 1. 实现机制的不同：Synchronized是java的关键字，它是有java虚拟机通过对象头中的mark word字段以及monitor对象来实现的，同步代码块是使用monitorenter和monitorexit指令实现的，同步方法（在这看不出来需要看JVM底层实现）依靠的是方法修饰符上的ACC_SYNCHRONIZED实现。他是`悲观锁机制`，是独占锁，当发生竞争阻塞的时候，获取不到锁的线程会频繁的上下文切换带来性能损耗；
    >
    >    而reentrantLock是java的类，是java的api，是底层通过调用unsafe.compareAndSetState，他是使用的`cas乐观锁`的机制，每次不加锁而是假设没有冲突而去完成某项操作，如果因为冲突失败就重试，直到成功为止；[reentrantLock实现详细解释](https://www.jianshu.com/p/b6efbdbdc6fa)
    >
    > 2. lock锁有更多的特性，支持更多的场景：支持公平锁、等待中断锁
    >
    > 3. synchronized使用Object对象本身的wait 、notify、notifyAll调度机制，ReentrantLock里面的Condition应用，能够控制notify哪个线程。[ReentrantLock详细介绍及conditon的使用](https://www.cnblogs.com/xiaoxi/p/7651360.html)

17. reentrantLock是如何实现可重入锁的

    >  ReentrantLock 内部自定义了同步器 Sync(Sync 实现了 AQS，而 AOS 提供了一种互斥锁持有的方式)，其实就是 加锁的时候通过 CAS 算法，将线程对象放到一个双向链表中，每次获 取锁的时候，看下当前维护的那个线程 ID 和当前请求的线程 ID 是否 一样，一样就可重入了。
    >
    > 

18. [线程间通信](https://zhuanlan.zhihu.com/p/47948392)

    >  
    >
    > 

19. [公平和非公平锁](https://zhuanlan.zhihu.com/p/45305463) ***

    > **非公平锁和公平锁的两处不同：**
    >
    > 1. 非公平锁在调用 lock 后，首先就会调用 CAS 进行一次抢锁，如果这个时候恰巧锁没有被占用，那么直接就获取到锁返回了。
    >
    > 2. 非公平锁在 CAS 失败后，和公平锁一样都会进入到 tryAcquire 方法，在 tryAcquire 方法中，如果发现锁这个时候被释放了（state == 0），非公平锁会直接 CAS 抢锁，但是公平锁会判断等待队列是否有线程处于等待状态，如果有则不去抢锁，乖乖排到后面。
    >
    > 公平锁和非公平锁就这两点区别，如果这两次 CAS 都不成功，那么后面非公平锁和公平锁是一样的，都要进入到阻塞队列等待唤醒。
    >
    > 相对来说，非公平锁会有更好的性能，因为它的吞吐量比较大。当然，非公平锁让获取锁的时间变得更加不确定，可能会导致在阻塞队列中的线程长期处于饥饿状态。

20. [synchronized底层实现](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247484838&amp;idx=1&amp;sn=54b33b4c76e136efac09941b2dd346b3&source=41#wechat_redirect)

    > - 对于synchronized同步代码块，会在代码开始和结束的位置有monitorEnter和minitorExit指令，当执行monitorEnter时，线程就必须获取monitor对象的持有权限（monitor对象存在于每个Java对象的对象头中，synchronized 锁便是通过这种方式获取锁的）。
    > - 对于synchronized同步方法，会有ACC_SYNCHRONIZED 标识，表示改方法是同步方法，调用方法前需要获取对象实例的锁

21. Jdk1.6后有的锁优化 ***

    > `锁优化`：引入了偏向锁、轻量级锁、自旋锁10次、自适应自旋锁、锁粗化、锁消除来减少锁操作的开销；
    >
    > `锁状态`  ：无锁状态，偏向锁状态，轻量级锁状态，重量级锁状态
    >
    > `锁优化的意义` ：偏向锁和轻量级锁都是为了减少没有多线程竞争的情况下，重量级锁使用操作系统的互斥量带来的性能损耗。
    >
    > `锁优化的流程`：① 对象的偏向锁会偏向于第一个获取它的线程，JVM 会利用 CAS 操作，在对象头上的 Mark Word 部分设置线程 ID。接下来如果没有其他线程获取该对象的锁，它一直处于偏向锁状态，持有偏向锁的线程会不执行同步操作；②当有第二个线程获取该锁的时候，偏向锁升级为轻量级锁，轻量级锁使用了cas操作 Mark Word 来试图获取锁（`主要解决的是绝大部分情况下是不存在竞争的，不需要同步操作`）③轻量级锁不加锁的进行尝试，当失败后不会挂起线程（`因为挂起线程/恢复线程的操作都需要转入内核态中完成（用户态转换到内核态会耗费时间）`）而是在线程周围进行忙循环，达到一定次数会进一步升级为重量级锁；④重量级锁：编译后的代码块中加入moniterEnter和monitorExist指令；

22. [java锁的详细介绍](https://www.cnblogs.com/jyroy/p/11365935.html)  ???

    > - lock 接口下的reentrantLock，互斥锁；
    >
    > - readWriteLock接口下的ReentrantReadWriteLock： 基于的原理是多个读操作是不需要互斥的，因为读操作并不会更改数据，所以不存在互相干扰。而写操作则会导致并发一致性的问题，所以写线程之间、读写线程之间，需要精心设计的互斥逻辑
    >
    > - StampedLock：不支持再入性的定义。JDK 在后期引入了 StampedLock，在提供类似读写锁的同时，还支持优化读模式。优化读基于假设，大多数情况下读操作并不会和写操作冲突，其逻辑是先试着读，然后通过 validate 方法确认是否进入了写模式，如果没有进入，就成功避免了开销；如果进入，则尝试获取读锁。

23. [CountDownLatch实现原理   ***](https://blog.csdn.net/u014653197/article/details/78217571)

    > `简介` ：CountDownLatch是一个同步工具类，用来协调多个线程之间的同步。这个工具通常用来控制线程等待，它可以让某一个线程等待直到倒计时结束，再开始执行 
    >
    > `原理` ：让需要的暂时阻塞的线程（await），进入一个死循环里面，得到某个条件后再退出循环(count=0)，以此实现阻塞当前线程的效果。
    >
    > `代码层面`： 使用内部类sync 继承了aqs，① 维护state状态就是countDownLatch的个数；②countDown方法调用的被重写了的tryReleaseShared 使用for(;;)死循环，来判断是否为0；③：await方法调用被重写了的tryAquireShared，判断状态是否为0；

24. countDownLatch和join()方法的区别

    >  在当前线程中，如果调用某个thread的join方法，那么当前线程就会被阻塞，直到thread线程执行完毕，当前线程才能继续执行。join的原理是，不断的检查thread是否存活，如果存活，那么让当前线程一直wait，直到thread线程终止，线程的this.notifyAll 就会被调用；
    >
    > CountDownLatch中我们主要用到两个方法一个是await()方法，调用这个方法的线程会被阻塞，另外一个是countDown()方法，调用这个方法会使计数器减一，当计数器的值为0时，因调用await()方法被阻塞的线程会被唤醒，继续执行
    >
    > 总结：调用join方法需要等待thread执行完毕才能继续向下执行,而CountDownLatch只需要检查计数器的值为零就可以继续向下执行，相比之下，CountDownLatch更加灵活一些，可以实现一些更加复杂的业务场景。
    >
    > 

25. [深入理解Semaphore](https://blog.csdn.net/qq_19431333/article/details/70212663) 

    > Semaphore(共享信号量)：允许多个线程同时访问： synchronized 和 ReentrantLock 都是一次只允许一个线程访问某个资源，Semaphore(信号量)可以指定多个线程同时访问某个资源。

26. CyclicBarrier(循环栅栏)  ***

    >  CyclicBarrier 和 CountDownLatch非常类似，它也可以实现线程间的等待，但是它的功能比 CountDownLatch 更加复杂和强大。主要应用场景和 CountDownLatch 类似。CyclicBarrier的字面意思是可循环使用（Cyclic）的屏障（Barrier）。CyclicBarrier默认的构造方法是 CyclicBarrier(int parties)，其参数表示屏障拦截的线程数量，每个线程调用await方法告诉 CyclicBarrier 我已经到达了屏障，然后当前线程被阻塞
    >
    >  它要做的事情是，让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。

27. cyclicbarrier和countdownLatch的区别

    > - CountDownLatch 是不可以重置的，所以无法重用，CyclicBarrier 没 有这种限制，可以重用。
    > -  CountDownLatch 的 基 本 操 作 组 合 是 countDown/await， 调 用 await 的线程阻塞等待 countDown 足够的次数，不管你是在一个线程还是多个线程里 countDown，只要次数足够即可。 CyclicBarrier 的基本操作组合就是 await，当所有的伙伴都调用了 await，才会继续 进行任务，并自动进行重置。
    > - CountDownLatch 目的是让一个线程等待其他 N 个线程达到某个条 件后，自己再去做某个事(通过 CyclicBarrier 的第二个构造方法 public CyclicBarrier(int parties, Runnable barrierAction)，在新线 程里做事可以达到同样的效果)。而 CyclicBarrier 的目的是让 N 多 线程互相等待直到所有的都达到某个状态，然后这 N 个线程再继续执 行各自后续(通过 CountDownLatch 在某些场合也能完成类似的效果)。

28. [Java并发包基石-AQS详解](https://www.cnblogs.com/chengxiao/archive/2017/07/24/7141160.html)   /[AQS 原理以及 AQS 同步组件总结](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247484832&amp;idx=1&amp;sn=f902febd050eac59d67fc0804d7e1ad5&source=41#wechat_redirect)  ***

    > 1. ` 核心思想`：当被请求的共享资源处于空闲状态，则当前请求资源的线程被标记为有效状态，共享资源被设置为锁定状态。如果被请求的资源被占用，则需要一套线程阻塞等待以及被唤醒时的锁分配的机制，AQS就是使用CLH队列锁来实现的，即将暂时获取不到锁的线程加入到队列中。
    >
    > 2. `资源共享的方式`：独占和共享，其中独占只有一个线程可以执行，如reentrantLock，又分为公平和非公平的；共享是指多个线程可以同时执行，如countDownLatch/ Semaphore
    >
    > 3. `AQS底层使用了模板方法模式`：使用的时候需要继承AQS类，重写如下方法
    >
    >    ```
    >    isHeldExclusively()//该线程是否正在独占资源。只有用到condition才需要去实现它。
    >    tryAcquire(int)//独占方式。尝试获取资源，成功则返回true，失败则返回false。
    >    tryRelease(int)//独占方式。尝试释放资源，成功则返回true，失败则返回false。
    >    tryAcquireShared(int)//共享方式。尝试获取资源。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。
    >    tryReleaseShared(int)//共享方式。尝试释放资源，成功则返回true，失败则返回false
    >    ```

29. synchronized和volatile的区别

    > 1. 粒度不同，前者锁对象和类，后者针对变量；
    > 2. syn保证三大特性，volatile不保证原子性；
    > 3. syn阻塞，volatile线程不阻塞；
    > 4. syn编译器优化，volatile不优化；

30. 悲观锁和乐观锁

    > - 乐观锁是假设并发冲突不会发生，总是不加锁的执行操作，如果失败，则会进行重试；
    > - 悲观锁是假设冲突会发生，执行操作的时候就加一个独占锁，共享资源每次都只给一个线程使用，其它线程阻塞，用完后再给其它线程；
    >
    > 读多写少的场景适合用乐观锁；写多的场景适合用悲观锁；

31. cas是什么

    > cas就是comare and swap，就是内存值V，旧值A，要修改的值b，只有当预期值A与内存的值V相等时才执行设置值为b，并且返回成功，否则返回失败；一般是配合volatile关键字使用，才可以保证拿到的变量主内存中的值，修改后可以将值设置到主内存中去；

32. java内存模型  ***

    > 1. java内存模型分为主内存和工作内存，共享变量保存在主内存中，每一个线程需要修改和读取共享变量的时候都要从主内存中copy一份到自己的工作内存中，修改完后会写到主内存中去；
    >
    > 2. 定义了volatile的使用规则，保证每个线程读取volatile修饰的字段，都可以读取到最新的值，写的值都可以写到主内存中去；
    >
    > 3. 定义了原子操作，用于操作主内存和工作内存中的变量；
    >
    >    ```
    >    lock（锁定主内存）
    >    unlock（解锁主内存）
    >    read（读取主内存，为load准备）
    >    load（载入主内存至工作内存）
    >    use（执行引擎使用工作内存）
    >    assign（接受执行引擎计算后的值赋值给工作内存）
    >    store（存储工作内存至主内存，为write准备）
    >    write（把工作内存写入主内存）
    >    ```
    >
    > 4. 定义了happen-before，即先行发生规则，一个unlock操作一定先行发生于对后面的同一个锁进行lock操作；同一个线程，前面的操作一定先行执行与后面的操作；
    >
    > 5. 指令重排序，只要和源代码产生的结果一样，编译器会进行操作的重排序，提升计算机性能；

33. Happens-before 关系

    > 如果线程 A 与线程 B 满足 happens-before 关系，则线程 A 执行动作的结果对于线程 B 是可见的
    >
    > - 程序次序法则：一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作
    > - 监视器锁法则：一个unLock操作先行发生于后面对同一个锁的lock操作
    > - Volatile 变量法则：对一个Volatile变量的写操作先行发生于后面对这个变量的读操作
    > - 传递性：如果 A happens-before 于 B，且 B happens-before C，则 A happens-before C。

34. Thread.sleep(0)的作用是什么

    > 由于java使用抢占式调度算法，而sleep操作可以放弃cpu的执行时间，这样可以操作系统重新进行一次操作系统重新分配时间片的操作；  

35. AtomicInteger 底层实现原理是什么？

    > - AtomicIntger 是对 int 类型的一个封装，提供原子性的访问和更新操作，其原子性操作的实现是基于 CAS。
    >
    > - 它依赖于 Unsafe 提供的一些底层能力，进行底层操作；以 volatile 的 value 字段，记录数值，以保证可见性。
    >
    >   ```java
    >   //Unsafe 会利用 value 字段的内存地址偏移，直接完成操作。
    >      public final int incrementAndGet() {
    >           return unsafe.getAndAddInt(this, valueOffset, 1) + 1;
    >       }
    >   
    >   //类似 compareAndSet 这种返回 boolean 类型的函数，因为其返回值表现的就是成功与否
    >       public final boolean compareAndSet(int expect, int update) {
    >           return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    >       }
    >   
    >   
    >   ```
    >
    > - CAS 更加底层是如何实现的？这依赖于 CPU 提供的特定指令，具体根据体系结构的不同还存在着明显区别。比如，x86 CPU 提供 cmpxchg 指令

# java的集合数据结构

# List相关

1. [arrayList和vector的区别](https://blog.csdn.net/qq_37113604/article/details/80836025?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-1)

   > - arrayList是数组来实现的，初始容量10，Arrays.copyOf 扩容为1.5倍；线程不安全
   >
   > - vector也是数组来实现的，初始容量10，默认扩容为2倍，可以制动扩容因子；synchronized修饰线程安全；

2. [arrayList](https://blog.csdn.net/ywlmsm1224811/article/details/91388048)

   > arrayList继承了abstractList，最上层是collection接口，RandmoAccess接口随机访问表示；
   >
   > 多线程选择vector和copyOnWriteArrayList

# hashMap相关

1. 如何解决hash冲突

   > 1. 开放地址法：
   >
   > 优点：开放寻址法不像链表法，需要拉很多链表，他们需要根据指针寻址，对cpu不友好。散列表中的数据都存储在数组中，可以有效地利用CPU缓存加快查询速度；
   >
   > 缺点：用开放寻址法解决冲突的散列表，删除数据的时候比较麻烦，需要特殊标记已经删除掉的数据。而且，在开放寻址法中，所有的数据都存储在一个数组中，比起链表法来说，冲突的代价更高。
   >
   > ​       当数据量比较小、装载因子小的时候，适合采用开放寻址法。这也是Java中的ThreadLocalMap使用开放寻址法解决散列冲突的原因。
   >
   > 2.链地址法：
   >
   > ​     优点：首先，链表法对内存的利用率比开放寻址法要高。因为链表结点可以在需要的时候再创建，并不需要像开放寻址法那样事先申请好。
   >
   > ​    其次  对装载因子的容忍度高，即使装很多元素只是查找效率下降而已；
   >
   > ​    最后  链表法比较灵活，可以修改实现改为红黑树；
   >
   > ​    缺点：如果存储的小对象，那么指针消耗也是很大一部分
   >
   > ​    基于链表的散列冲突处理方法比较适合存储大对象、大数据量的散列表，而且，比起开放寻址法，它更加灵活，支持更多的优化策略，比如用红黑树代替链表。
   >
   > 3. 再hash法

2. [为什么hash表的容量一定要是2的整数倍](https://www.cnblogs.com/peizhe123/p/5790252.html)

   > 为了使不同 hash 值发生碰撞的概率更小，尽可能促使元素在哈希表中均匀地散列。
   >
   > - 2的整数倍减1后，换成二进制后，最后一位肯定是1，这样与hashCode与操作，有可能是奇偶，否则只能是偶数，会浪费一般空间而且不能保证散列均匀；
   >
   > capacity 为 2 的整数次幂的话，为偶数，这样 capacity-1 为奇数，奇数的最后一位是 1，这样便保证了 h&(capacity-1) 的最后一位可能为 0，也可能为 1（这取决于h的值），即与后的结果可能为偶数，也可能为奇数，这样便可以保证散列的均匀性；
   >
   > 而如果 capacity 为奇数的话，很明显 capacity-1 为偶数，它的最后一位是 0，这样 h&(capacity-1) 的最后一位肯定为 0，即只能为偶数，这样任何 hash 值都只会被散列到数组的偶数下标位置上，这便浪费了近一半的空间。

   > - 而与操作是使用取余替代取模，提升计算效率；
   >
   >    首先，capacity 为 2的整数次幂的话，长度减一后相当于低位的掩码，计算桶的位置 h&(length-1) 就相当于对 length 取模，提升了计算效率；
   
3. [hashmap多线程操作导致死循环](https://coolshell.cn/articles/9606.html) 

   [hashmap死循环](https://blog.csdn.net/xuefeng0707/article/details/40797085)

   > hashmap在多线程的情况下进行rehash操作会导致死循环；

4. rehash算法

   > ![img](https://pic2.zhimg.com/80/a285d9b2da279a18b052fe5eed69afe9_720w.png)
   >
   > - rehash的算法中，扩容为2倍后，重新计算索引，index = 原位置+oldcap或者为原index；
   >
   > - 只需要看看原来的hash值新增的那个bit是1还是0就好了，是0的话索引没变，是1的话索引变成“原索引+oldCap”，可以看看下图为16扩充为32的resize示意图：
   >
   > 

5. hashmap的hash算法  ***

   >
   >
   >![img](https://pic2.zhimg.com/80/8e8203c1b51be6446cda4026eaaccf19_720w.png)
   >
   >int hash(Object key) {
   >int h = key.hashCode()；
   >return (h ^ (h >>> 16)) & (capitity -1); //capicity 表示散列表的大小
   >}
   >
   >先补充下老师使用的这段代码的一些问题：在JDK HashMap源码中，是分两步走的：
   >
   >1. hash值的计算，源码如下：
   >    static final int hash(Object key) {
   >       int hash;
   >       return key == null ? 0 : (hash = key.hashCode()) ^ hash >>> 16;
   >     }
   >2. 在插入或查找的时候，计算Key被映射到桶的位置：
   >    int index = hash(key) & (capacity - 1)

   >- JDK HashMap中hash函数的设计，确实很巧妙：
   >
   >首先hashcode本身是个32位整型值，获取对象的hashcode以后，先进行移位运算，然后再和自己做异或运算，即：hashcode ^ (hashcode >>> 16)，这一步甚是巧妙，是`将高16位移到低16位，这样计算出来的整型值将“具有”高位和低位的性质`
   >
   >最后，用hash表当前的容量减去一，再和刚刚计算出来的整型值做位与运算。进行位与运算，很好理解，是为了计算出数组中的位置。但这里有个问题：
   >为什么要用容量减去一？
   >因为 A % B = A & (B - 1)，所以，(h ^ (h >>> 16)) & (capitity -1) = (h ^ (h >>> 16)) % capitity，可以看出这里本质上是使用了「除留余数法」
   >
   >综上，可以看出，hashcode的随机性，加上移位异或算法，得到一个非常随机的hash值，再通过「除留余数法」，得到index，

6. HashMap如何避免低效地扩容  ***

   >   大部分情况下，动态扩容的散列表插入一个数据都很快，但是在特殊情况下，当装载因子已经到达阈值，需要先进行扩容，再插入数据。这个时候，插入数据就会变得很慢，甚至会无法接受。我举一个极端的例子，如果散列表当前大小为1GB，要想扩容为原来的两倍大小，那就需要对1GB的数据重新计算哈希值，并且从原来的散列表搬移到新的散列表，听起来就很耗时，是不是？
   >
   > ​        如果我们的业务代码直接服务于用户，尽管大部分情况下，插入一个数据的操作都很快，但是，极个别非常慢的插入操作，也会让用户崩溃。这个时候，“一次性”扩容的机制就不合适了。
   >
   > ​        为了解决一次性扩容耗时过多的情况，我们可以将扩容操作穿插在插入操作的过程中，分批完成。当装载因子触达阈值之后，我们只申请新空间，但并不将老的数据搬移到新散列表中。
   >
   > ​        当有新数据要插入时，我们将新数据插入新散列表中，并且从老的散列表中拿出一个数据放入到新散列表。每次插入一个数据到散列表，我们都重复上面的过程。经过多次插入操作之后，老的散列表中的数据就一点一点全部搬移到新散列表中了。这样没有了集中的一次性数据搬移，插入操作就都变得很快了。
   >
   > ​    对于查询操作，为了兼容了新、老散列表中的数据，我们先从新散列表中查找，如果没有找到，再去老的散列表中查找。

7. [hashMap的源码解析1.8  ***](https://zhuanlan.zhihu.com/p/21673805)

   [hashMap的源码解析1.7](https://www.cnblogs.com/peizhe123/p/5790252.html)

   > `1.8的put流程`：
   >
   > ①.判断键值对数组table[i]是否为空或为null，否则执行resize()进行扩容；
   >
   > ②.根据键值key计算hash值得到插入的数组索引i，如果table[i]==null，直接新建节点添加，转向⑥，如果table[i]不为空，转向③；
   >
   > ③.判断table[i]的首个元素是否和key一样，如果相同直接覆盖value，否则转向④，这里的相同指的是hashCode以及equals；
   >
   > ④.判断table[i] 是否为treeNode，即table[i] 是否是红黑树，如果是红黑树，则直接在树中插入键值对，否则转向⑤；
   >
   > ⑤.遍历table[i]，判断链表长度是否大于8，大于8的话把链表转换为红黑树，在红黑树中执行插入操作，否则进行链表的插入操作；遍历过程中若发现key已经存在直接覆盖value即可；
   >
   > ⑥.插入成功后，判断实际存在的键值对数量size是否超多了最大容量threshold，如果超过，进行扩容。

8. [hashMap1.8比1.7做了什么优化](https://blog.csdn.net/liSir159633/article/details/105003718) ***

   > 1. 在java 1.8中，如果链表的长度超过了8并且它的hash桶大于等于64的时候，那么链表将转换为红黑树。
   >    - 把时间复杂度从O（n）变成O（logN）提高了效率
   > 2. 发生hash碰撞时，java 1.7 会在链表的头部插入，而java 1.8会在链表的尾部插入
   >    - 因为JDK1.7是用单链表进行的纵向延伸，当采用头插法时会容易出现逆序且环形链表死循环问题。但是在JDK1.8之后是因为加入了红黑树使用尾插法，能够避免出现逆序且链表死循环的问题。
   > 3. 在java 1.8中，Entry被Node替代(换了一个马甲)。
   > 4. 获取hashCode时候的扰动函数由 9次扰动处理=4次位运算+5次异或，而JDK1.8只用了2次扰动处理=1次位运算+1次异或；
   > 5. 扩容后的元素的存储位置的计算：
   >    - 判断hash值新增的参与位运算的（高位）如果为1 则：原始位置+扩容前的旧容量
   >    - 如果为0，则放在原始位置；

9. 负载因子的作用

   >  负载因子是hash冲突和内存空间利用率的一种折中。
   >
   >  若负载因子越大，装载的元素越多，空间利用率高了，但是冲突变大，链表变长，查找效率变低；
   >
   >  若负载因子越小，状态的元素变少，空间表少了，冲突变小，查找起来更快；

10. hashMap的modCount用来的作用

   > modCount字段是用来记录hashmap内部结构发生变化的次数，主要用于迭代的快速失败。例如put新键值对，如果某个key对应的value被覆盖不属于结构变化。遍历过程中改变hashMap的内部结构则会出现ConcurrentModificationException。

11. 多线程操作hashMap会出现什么问题

    > - 在rehash时会出现死循环；rehash会重新将原数组的内容重新hash到新的扩容数组中，在多线程的环境下，由于采用的是头插法，会造成闭环。导致在get时定位到该槽位时，且元素不存在的情况下会出现死循环
    > - 在遍历的时候修改hashmap的结构，会出现concurrentModifyException，fail-fast机制

12. [linkHashMap](https://www.imooc.com/article/22931)

    > - linkHashMap是在 HashMap 基础上，通过维护一条双向链表，解决了 HashMap 不能随时保持遍历顺序和插入顺序一致的问题。
    >
    > - LinkedHashMap 对访问顺序也提供了相关支持。在一些场景下，该特性很有用，比如缓存。
    >
    > - 添加元素是将新元素放到双向链表的尾部。
    > - 实现访问控制，就是在get元素后，将节点放到双向链表的尾部；移除最近最少访问的元素在头部；需要重写removeEldestEntry方法，订制删除策略

13. [fail-fast 机制](https://baijiahao.baidu.com/s?id=1638201147057831295&wfr=spider&for=pc)

    > - Fail-fast机制是指在系统设计中，快速失效系统一种可以立即报告任何可能表明故障的情况的系统。快速失效系统通常设计用于停止正常操作，而不是试图继续可能存在缺陷的过程。
    >
    > - arrayList和hashmap就使用modCount字段来记录表机构修改的次数，来和遍历过程中获取的次数进行对比，如果不一致，则fail-fast;

14. concurrentHashMap

    [concurrentHashMap](https://www.ibm.com/developerworks/cn/java/java-lo-concurrenthashmap/index.html)

    [concurrentHashMap-1.8](https://www.cnblogs.com/yangming1996/p/8031199.html)

    > `jdk1.7与1.8的实现机制`
    >
    > - Jdk1.7采用的分段锁技术，整个hash表被分成多个段，每个段对应一个segment锁。段与段之间的可以并发访问，同一个段之间并发访问需要获取锁；
    > - Jdk1.8取消了segment分段锁机制，jdk1.8主要使用了UNsafe类的cas自旋赋值(putVal操作中桶位置添加节点)+sychronize同步(初始化Node数组以及向桶桶添加元素)+lockSupport阻塞等手段实现高效并发。使用数组加链表+红黑树取代了数组+链表。
    >
    > ConcurrentHashMap的JDK8与JDK7版本的并发实现相比，最大的区别在于JDK8的锁粒度更细，理想情况下talbe数组元素的大小就是其支持并发的最大个数，在JDK7里面最大并发个数就是Segment的个数，默认值是16。
    >
    > jdk1.8将锁的级别控制在了更细粒度的table元素级别，也就是说只需要锁住这个链表的head节点，并不会影响其他的table元素的读写。缺点是需要等扩容完之后，所有的读写操作才能进行，所以扩容的效率就成为了整个并发的一个瓶颈点，
    >
    > 
    >
    > `concurrentHashMap 1.8`:使用了多线程扩容
    >
    > `sizeCtl 属性`：代表是初始化哈希表，还是扩容 rehash 的过程
    >
    > - 未初始化：
    >
    > - - sizeCtl=0：表示没有指定初始容量。
    >   - sizeCtl>0：表示初始容量。
    >
    > - 初始化中：
    >
    > - - sizeCtl=-1,标记作用，告知其他线程，正在初始化
    >
    > - 正常状态：
    >
    > - - sizeCtl=0.75n ,扩容阈值
    >
    > - 扩容中:
    >
    > - - sizeCtl < 0 : 表示有其他线程正在执行扩容
    >   - sizeCtl = (resizeStamp(n) << RESIZE_STAMP_SHIFT) + 2 :表示此时只有一个线程在执行扩容
    >
    > `putval方法`
    >
    > 1. 如果没有初始化就先调用initTable（）方法来进行初始化过程
    >
    > 2. 如果没有hash冲突就直接CAS插入
    >
    > 3. 如果还在进行扩容操作就先进行扩容
    >
    > 4. 如果存在hash冲突，就加锁来保证线程安全，这里有两种情况，一种是链表形式就直接遍历到尾端插入，一种是红黑树就按照红黑树结构插入，
    >
    > 5. 最后一个如果该链表的数量大于阈值8，就要先转换成黑红树的结构，break再一次进入循环
    >
    > 6. 如果添加成功就调用addCount（）方法统计size，并且检查是否需要扩容
    >
    >    
    >
    > - 放置元素的时候，如果对应的位置为空，则以cas的方式在对应的位置添加节点；
    >
    > - hash表只允许一个线程进行初始化，判断sizeCtl属性，来判断U.compareAndSwapInt初始化；
    >
    > - 扩容的时候需要会使用多线程扩容；helpTransfer（）方法是调用多个工作线程一起帮助进行扩容
    >
    > - volatile`类型的变量`baseCount记录元素的个数，通过cas的方式修改baseCount
    >
    >   
    >
    > 
    >
    > `concurrentHashMap 1.7`:
    >
    > ```java
    > static final class Segment<K,V> extends ReentrantLock implements Serializable {
    >         transient volatile HashEntry<K,V>[] table;
    >         transient int count;
    >         transient int modCount;
    >         transient int threshold;
    >         final float loadFactor;
    > ```
    >
    > -	Segment的put方法：需要通过tryLock获取到该段的锁，然后才可以执行put操作；
    > -	size方法：先采用不加锁的方式，连续计算多个segment元素的个数，最多计算3次：
    >    1、如果前后两次计算结果相同，则说明计算出来的元素个数是准确的；
    >    2、如果前后两次计算结果都不同，则给每个`Segment`进行加锁，再计算一次元素的个数；
    >
    > `concurrentHashMap不允许空key和空value；hashMap允许一个空key，多个空value`

15. concurrentHashMap进行扩容（1.8）

    > 它使用多线程扩容。使用sizeCtl,
    >
    > sizeCtl ：默认为0，用来控制table的初始化和扩容操作，具体应用在后续会体现出来。 不同状态，sizeCtl所代表的含义也有所不同。
    >
    > - 未初始化：
    >
    > - - sizeCtl=0：表示没有指定初始容量。
    >   - sizeCtl>0：表示初始容量。
    >
    > - 初始化中：
    >
    > - - sizeCtl=-1,标记作用，告知其他线程，正在初始化
    >
    > - 正常状态：
    >
    > - - sizeCtl=0.75n ,扩容阈值
    >
    > - 扩容中:
    >
    > - - sizeCtl < 0 : 表示有其他线程正在执行扩容
    >   - sizeCtl = (resizeStamp(n) << RESIZE_STAMP_SHIFT) + 2 :表示此时只有一个线程在执行扩容
    >
    > 扩容：
    >
    > 扩容时候会判断sizeCtl这个值，如果超过阈值就要扩容。
    >
    > `transferIndex是扩容索引`，表示已经分配给扩容线程的table数组索引位置。主要用来协调多个线程，并发安全地获取迁移任务（hash桶）。
    >
    > ​	在扩容之前，transferIndex 在数组的最右边 。此时有一个线程发现已经到达扩容阈值，准备开始扩容。扩容线程，在迁移数据之前，首先要将transferIndex左移（以cas的方式修改**transferIndex=transferIndex-stride(要迁移hash桶的个数)**），获取迁移任务。每个扩容线程都会通过for循环+CAS的方式设置transferIndex，因此可以确保多线程扩容的并发安全。
    >
    > 
    >
    > 换个角度，我们可以将待迁移的table数组，看成一个任务队列，transferIndex看成任务队列的头指针。而扩容线程，就是这个队列的消费者。扩容线程通过CAS设置transferIndex索引的过程，就是消费者从任务队列中获取任务的过程。为了性能考虑，我们当然不会每次只获取一个任务（hash桶），因此ConcurrentHashMap规定，每次至少要获取16个迁移任务（迁移16个hash桶，MIN_TRANSFER_STRIDE = 16）
    >
    > cas设置transferIndex的源码如下：
    >
    > ```java
    > 
    >   private final void transfer(Node<K,V>[] tab, Node<K,V>[] nextTab) {
    >         //计算每次迁移的node个数
    >         if ((stride = (NCPU > 1) ? (n >>> 3) / NCPU : n) < MIN_TRANSFER_STRIDE)
    >             stride = MIN_TRANSFER_STRIDE; // 确保每次迁移的node个数不少于16个
    >         ...
    >         for (int i = 0, bound = 0;;) {
    >             ...
    >             //cas无锁算法设置 transferIndex = transferIndex - stride
    >             if (U.compareAndSwapInt
    >                          (this, TRANSFERINDEX, nextIndex,
    >                           nextBound = (nextIndex > stride ?
    >                                        nextIndex - stride : 0))) {
    >                   ...
    >                   ...
    >             }
    >             ...//省略迁移逻辑
    >         }
    >     }
    > 
    > ```
    >
    > `ForwardingNode节点 `
    >
    > 1. 标记作用，表示其他线程正在扩容，并且此节点已经扩容完毕
    > 2. 关联了nextTable,扩容期间可以通过find方法，访问已经迁移到了nextTable中的数据
    >
    > 扩容线程A 以cas的方式修改transferindex=31-16=16 ,然后按照降序迁移table[31]--table[16]这个区间的hash桶。迁移hash桶时，会将桶内的链表或者红黑树，按照一定算法，拆分成2份，将其插入nextTable[i]和nextTable[i+n]（n是table数组的长度）。 迁移完毕的hash桶,会被设置成ForwardingNode节点，以此告知访问此桶的其他线程，此节点已经迁移完毕。
    >
    > 线程2访问到了ForwardingNode节点，如果线程2执行的put或remove等写操作，那么就会先帮其扩容。如果线程2执行的是get等读方法，则会调用ForwardingNode的find方法，去nextTable里面查找相关元素。
    >
    > 如果准备加入扩容的线程，发现以下情况，放弃扩容，直接返回。
    >
    > - 发现transferIndex=0,即所有node均已分配
    > - 发现扩容线程已经达到最大扩容线程数
    >
    > 

16. [一致性hash算法](https://www.jianshu.com/p/e968c081f563)

    > `意义`：
    >
    > 普通的hash算法，可以使每台机器固定处理一部分用户请求，起到负载均衡的作用；但当一些机器失效的时候，用户id与服务器的映射关系会大量失效；此时一致性hash算法诞生；
    >
    > `总结`：
    >
    > - 将多个机器的ip进行取hash，hash值在0-2的32次方之间，即可将机器和ip值在hash环上映射；
    > - 然后对用户id进行取hash，找到hash值在hash环上的位置后，顺时针寻找最近的一个机器就是目标机器；
    > - 由于机器数量少存在一致性hash倾斜的问题，可以使用虚拟节点来减少节点失效的影响。
    >
    > 1、2、3、4、5、6、7、8、9
    >
    > A：1、4、7；B：2、5、8；C：3、6、9

​    

# 队列

1. [java的队列-queue](https://blog.csdn.net/qq_33524158/article/details/78578370)

   > `ConcurrentLinkedQueue`：是一个适用于高并发场景下的队列，通过无锁的方式，实现了高并发状态下的高性能。头是最先加入的，尾是最近加入的，该队列不允许null元素。
   >
   > `blockingQueue`:
   >
   > - ArrayBlockingQueue是基于`数组`的阻塞队列，内部维护一个`定长`数组缓冲队列，`有界`队列，内部`没有实现读写分离`，意味着生产和消费不能并行；
   > - LinkedBlockingQueue：基于链表的阻塞队列，内部也维护着缓存队列，无界的，内部实现读写分离，支持并发；
   > - SynchronousQueue：一种没有缓冲的队列，生产者产生的数据直接会被消费者获取并消费。
   > - PriorityBlockingQueue：基于优先级的阻塞队列（优先级的判断通过构造函数传入的Compator对象来决定，也就是说传入队列的对象必须实现Comparable接口），在实现PriorityBlockingQueue时，内部控制线程同步的锁采用的是公平锁，他也是一个无界的队列；
   > - DelayQueue：带有延迟时间的Queue，其中的元素只有当其指定的延迟时间到了，才能够从队列中获取到该元素。
   >
   > `Deque 双端队列`:
   >
   > -  LinkedBlockingDeque是一个线程安全的双端队列实现，可以说他是最为复杂的一种队列，在内部实现维护了前端和后端节点，但是其没有实现读写分离，因此同一时间只能有一个线程对其讲行操作。

# JVM

1. JVM运行内存的分类  [深入java虚拟机总结](https://www.cnblogs.com/wangzhongqiu/p/8908266.html) ***

   > - 程序计数器：存放的是虚拟机正在执行的字节码指令，可以看成是当前线程所执行的字节码行号，线程私有；
   >
   > - java虚拟机栈：线程私有，每个方法执行的时候都会创建一个栈帧，方法执行过程对应着栈帧入栈和出栈的过程；存储局部变量表、操作数栈、动态链接、方法返回值等；包含基本数据类型和对象的引用；
   >
   >   `Java 虚拟机栈会出现两种异常：StackOverFlowError 和 OutOfMemoryError。
   >
   >   - StackOverFlowError 若 Java 虚拟机栈的大小不允许动态扩展，那么当线程请求栈的深度超过当前 Java 虚拟机栈的最大深度时，抛出 StackOverFlowError 异常。
   >
   >   - OutOfMemoryError 若允许动态扩展，那么当线程请求栈时内存用完了，无法再动态扩展时，抛出 OutOfMemoryError 异常。`
   >
   >     
   >
   > - 本地方法栈：和虚拟机栈一样，不过是为虚拟机的native方法服务；
   >
   > - java堆：是线程共享的，是垃圾回收的主要区域；主要包括所有的对象实例、包括数组；
   >
   > - 方法区：线程共享，虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。也就是老年代；回收目标主要是常量池的回收和类型的卸载。jdk1.8使用metaSpace来代替永久代方法区；
   >
   >   `JDK 1.8 同 JDK 1.7 比，最大的差别就是：元数据区取代了永久代。元空间的本质和永久代类似，都是对 JVM 规范中方法区的实现。不过元空间与永久代之间最大的区别在于：元数据空间并不在虚拟机中，而是使用本地内存。[metaspace](https://blog.csdn.net/u012834750/article/details/70160594)
   >
   > - 运行时常量池：是方法区的一部分，运行时的常量池，比如integer的-128-127；1.8中转移到堆中；
   >
   > - 直接内存：也叫堆外内存，不是jvm的运行区域的一部分，是java1.4加入的NIO可以使用native方法直接分配堆外内存，Java堆中的DirectByteBuffer对象作为这块内存的引用进行操作。

2. HotSpot虚拟机

   - 对象的创建 （对象的初始化、分配内存、把对象指针指向内存区域）  ***

   > - 为对象分配内存，如果是规整的采用指针碰撞的方式；不规整的采用空闲列表，内存是否规则看是否代用压缩的功能；
   >
   > - serial、parNew带有压缩功能使用的是指针碰撞，cms使用的是空闲列表；

   - 对象涉及到分配内存和指针指向两个操作，不是原子性的，不是线程安全的。***

   > 1. 使用cas加上失败重试来保证原子性；
   > 2. 使用TLAB（Thread Local Allocation Buffer）策略，预先为每个线程分配一块内存，那个线程需要就各自分配，只有用完的时候才需要锁定同步；是否使用TLAB 需要配置-XX:+/- UseTLAB；

   - 设置对象头

   > 内存分配完后需要设置对象头，包括对象所属的实例，对象的hash码，对象的分代年龄、对象的元数据信息，锁状态标志、偏向线程ID、偏向时间戳；

   - 对象的访问定位

   > 1. 使用reference句柄，最大好处是当对象修改时reference本身不需要修改；
   > 2. 使用直接指针，节省了一次指针的开销，访问速度快；

3. 四种引用

   > 强引用：普通的java对象引用，只要有引用存在，java虚拟机就不会回收；
   > 软引用：可以让对象豁免一些垃圾收集，如果内存空间不足了，就会回收这些对象的内存；
   > 弱引用：与软引用相比弱引用的对象拥有更短暂的生命周期，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存；
   > 虚引用：主要用来跟踪对象被垃圾回收器回收的活动，被回收时会收到一个系统通知；

   4. GC标记对象死活

   > - 引用计数法：给对象添加一个引用计数器，被引用就加一，引用失效就减一；判断效率高，但无法解决循环引用的问题；
   > - 可达性分析算法：以一个GCRoot对象作为起点，从这个节点向下搜索，搜索走过的路径称为引用链，当一个对象没有与引用链中任何对象相连，则对象就被标记位可回收；

   5. GC Roots都有哪些

   > - 虚拟机栈中的引用的对象；
   >
   > - 本地方法栈中JNI（即一般说的Native方法）引用的对象；
   >
   > - 方法区中`静态变量`引用的对象，`常量引用`的对象

   6. 对象被回收的过程 ***

   > -  使用GCRoot标记可回收后判断是否需要执行finalize()方法（重写了或者手动调用了finalize()方法就需要执行）；
   >
   > - 如果需要执行，则放入F-queue队列中，虚拟机对应的线程会在稍后执行，如果在执行finalize方法时与GCRoot产生引用链，则可以逃脱被回收的命运；

   7. GC垃圾回收算法 

   > - 标记-清除算法：标记出没有用的对象，然后进行清除；缺点标记和清除的算法都不高，且会产生内存碎片；
   > - 复制算法：容量等大分为两份，一份满了，将存活的复制到另一份中；缺点浪费空间；
   > - 标记-整理：标记出存活的对象，让存活的对象移动到一端，直接清除端以外的对象；优点是解决了标记清除导致的内存碎片问题；
   > - 分代回收：根据对象的存活时间分为年轻代和老年代，年轻代使用复制算法，老年代使用标记整理算法，cms采用标记清除算法；

   8. 内存分配和回收策略 ***

   > - 结构： 年轻代（1/3）生命周期短，8:1:1 eden:survival ;老年代 (2/3):生命周期长
   > - 小对象先在年轻代区分配；
   > - 大对象直接进入老年代；
   > - 长期存活的对象，指多次在年轻代中复制达到阈值，进入老年代；
   > - 动态对象年龄判断。如果在Survival空间中相同年龄所有对象的大小总和超过了Survival空间的一半，年龄大于等于这个年龄的对象都会被晋升到老年代。无需等待年龄超过MaxTenuringThreShold指定的年龄；

   9. [Full GC 和 Minor GC](https://juejin.im/post/5b8d2a5551882542ba1ddcf8)

   > - yongGC只回收年轻代，
   > - oldGC只收集老年代（CMS才有的称呼）；
   > - fullGC收集整个堆，包括年轻代、老年代以及永久代（1.8后由metaspace取代了永久代）

   10. Full GC的触发条件,不同垃圾回收期的触发条件不同 ***

   > 1. 当年轻代晋升到老年代的对象大小比目前老年代剩余的空间大小还要大时，此时会触发Full GC；
   > 2. 当老年代的空间使用率超过某阈值时，此时会触发Full GC;
   > 3. 当元空间不足时（JDK1.7永久代不足），也会触发Full GC;
   > 4. 使用system.gc()方法，默认是出发full gc； 

   11. Survivor区对象晋升为老年代对象的条件

   > 1. Survivor区中的对象被来回复制的次数会被记录下来。如果一个对象被复制的次数为 15 (对应虚拟机参数 -XX:+MaxTenuringThreshold，那么该对象将被晋升为至老年代;
   > 2. 动态对象年龄判断。如果在Survival空间中相同年龄所有对象的大小总和超过了Survival空间的一半，年龄大于等于这个年龄的对象都会被晋升到老年代。无需等待年龄超过MaxTenuringThreShold指定的年龄；
   12. 垃圾收集器 ***

   > Serial 收集器是针对新生代的收集器，单线程收集器，采用的是复制算法；
   > Parnew收集器，Serial的多线程版本 新生代采用复制算法，cms的搭配使用；
   > Parallel Scavenge（并行收集器）收集器，针对新生代，采用复制收集算法，关注的是`吞吐量`；-XX:GCTimeRatio是设置吞吐量的大小，
   > Serial Old 收集器，老年代采用标记整理
   > Parallel Old（并行）收集器，针对老年代，标记整理
   > CMS收集器，基于标记清理，老年代收集器，注重的是`最短停顿时间`；
   > G1收集器(JDK)：整体上是基于标记-整理，分代收集
   > 综上：新生代基本采用复制算法，老年代采用标记整理算法。cms采用标记清除
   >
   > client vm mode（win 32）一直是Seiral GC，servermode下，9以后改为了G1，以前是Parrallel Gc
   13. CMS 收集器与G1收集器 ***

   > CMS垃圾收集器
   >
   > - 特点：cms，低延迟（最短停顿时间为目标），基于标记-清除算法；
   > - 流程：初始标记、并发标记、重新标记、并发清除；
   > - 各个阶段的作用：初始标记是标记GCRoot直连的；
   >    并发标记是通过GCRoot往下寻找；
   >   重新标记为了修正并发标记期间程序运行导致的变动的对象，时间比初始标记长，比并发标记短；
   >   耗时最长的并发标记和并发清除不会停顿，用户线程和处理线程一起工作；
   > - 缺点：
   >   1. 垃圾回收时占用线程，导致系统变慢，吞吐量降低；
   >   2. 使用标记-清除算法，容易产生内存碎片；需要开启碎片整理功能，多少次fullgc后开启一次带压缩的fullGC,使用-XX:CMSFullGCsBeforeCompaction
   >   3. 并发操作期间需要预留给用户的线程的内存，否则会出现 Concurrent Mode Failure 错误，需要使用 serial old，进行垃圾回收，导致停顿时间边长；
   >
   > G1收集器
   >
   > - `特点`：包括新生代和老年代的垃圾回收；并行并发，分代收集，标记-整理，可预测的停顿；
   >
   >   1、并行与并发：G1能够更充分利用多CPU、多核环境运行；
   >
   >   2、分代收集：G1虽然也用了分代概念，但相比其他收集器需要配合不同收集协同工作，但G1收集器能够独立管理整个堆
   >
   >   3、空间管理：与CMS的标记一清理算法不同，G1从整体上基于标记一整理算法，将整个Java堆划分为多个大小相等的独立区域（Region）,这种算法能够在运行过程中不产生内存碎片
   >
   >   4、可预测的停顿：降低停顿时间是G1和CMS共同目标，但是G1追求低停顿外，还能建立可预测的停顿时间模型，能让使用者明确指定一个长度为M毫秒的时间片段内，消耗在垃圾收集器上的时间不得超过N毫秒。
   >
   >   `流程`：
   >
   >   - 初始标记：标记GC Roots能够直接关联到的对象，这阶段需要停顿线程，时间很短
   >   - 并发标记：进行可达性分析，这阶段耗时较长，可与用户程序并发执行
   >   - 最终标记：修正发生变化的记录，需要停顿线程，但是可并行执行
   >   - 筛选回收：对各个Region的回收价值和成本进行排序，根据用户所期望的停顿时间来执行回收计划；
   14. 虚拟机性能监控工具 ***

       >1. jps，直接带有权限的hotspot虚拟机进程；
       >2. jstat -gcutil proessId interval count ,可以查看堆的各个区的使用情况，垃圾回收次数和时间等的统计；
       >3. jmap jmap -dump:live,format=b,file=dump.hprof 24971,查看内存的dump文件
       >4. jinfo -flag +PrintGCDetails 105704 查看和修改JVM运行参数
       >5. jstack  jstack pid - Java堆栈跟踪工具 。dead lock问题，占用cpu时间最多的线程，频繁GC
       >6. jconsole 可视化工具，内存，线程，垃圾回收，配合各种插件使用；
       >7. jvisual
       >8. [mat eclipse](https://blog.csdn.net/qeqeqe236/article/details/43577857)  开源的内存分析工具，将dump的文件导入后。提供可以查看对象的引用关系、最占内存的对象、疑似内存泄漏点等功能；



   15. [虚拟机参数](https://www.cnblogs.com/redcreen/archive/2011/05/04/2037057.html)

   ```
   128G内存，40核cpu
   
   CMS+ParNew
   -Xms8G
   -Xmx8G
   -Xmn3G
   -Xss512k
   -XX:MetaspaceSize=512m
   -XX:MaxMetaspaceSize=512m
   -XX:+PrintGCDetails
   -XX:+UseParNewGC
   -XX:+PrintGCDateStamps
   -XX:ParallelGCThreads=6
   -XX:+UseConcMarkSweepGC
   -XX:+CMSParallelRemarkEnabled
   -XX:SurvivorRatio=8
   -XX:NewRatio=4      //新生代：老年代=1：4
   -XX:CMSInitiatingOccupancyFraction=70  //老年代垃圾占比达到这个阈值开始CMS收集，设置过高容易导致并发收集失败，会出现SerialOld收集的情况
   -XX:CMSFullGCsBeforeCompaction=5
   -XX:+UseCMSCompactAtFullCollection
-XX:+HeapDumpOnOutOfMemoryError
   
   ```

   16. [java类加载过程](https://blog.csdn.net/shuangyue/article/details/9262791) ***

   > 触发类加载：
   >
   > - new实例或者调用类的静态变量；
   > - 初始化子类的时候触发父类初始化
   > - 使用java.lang.reflect包的方式对类进行反射调用的时候
   > - 作为程序入口的主类；
   >
   > 类加载的过程：
   >
   > 类加载被分为加载、连接、初始化过程；连接又细分为验证、准备和解析；
   >
   > 1. 装载：jvm将字节码文件以二进制的方式读入到内存中，解释器转变为机器码，转化为运行时数据结构；
   > 2. 连接：主要做加载完的准备工作，`验证被加载的文件是否符合java以及jvm规范`；`为类变量分配内存空间和设置初始值的阶段；虚拟机将符号引用替换为直接引用的过程`；
   > 3. 初始化：根据程序代码去初始化类变量和其他资源；


   17. 两种主动加载方式

   >class.forName()静态方法；
   >classLoader.loadClass()方法;

   18. 双亲委派模型

   > - 被不同类加载器加载的同名类，也认为是不同的类。
   > - 双亲委派模型。分为两种类加载器： 1 是启动类加载器 ，是虚拟机自身的一部分；2 是所有的其他类加载器，这些类加载器都由java语言实现。独立于虚拟机外部，全部继承自java.lang.ClassLoader抽象类。类加载器具体层次关系：启动类加载器->扩展类加载器->系统类加载器->自定义类加载器。每一个类的加载，会优先由父加载器来加载。这种方式就称为双亲委派，双亲委派保证了java基本类的不会被破坏和替代，避免重复加载java类；
   > - 双亲委派模型的缺点，比如我们需要一个类的不同版本，则双亲委派模型就不合适了。

   19. [什么地方违反了双亲委派模型](https://cloud.tencent.com/developer/article/1490214) ***

       >  jdk中的基础类作为用户典型的api被调用，但是也存在被api调用用户的代码的情况，典型的如SPI代码。
       >
       >  - [spi是如何破坏双亲委派模型的？](https://www.zhihu.com/question/49667892)
       >
       >  Java 在核心类库中定义了许多接口，并且还给出了针对这些接口的调用逻辑，然而并未给出实现。开发者要做的就是定制一个实现类，在 META-INF/services 中注册实现类信息，以供核心类库使用。
       >
       >  java.sql.Driver 是最为典型的 SPI 接口，java.sql.DriverManager 通过扫包的方式拿到指定的实现类，完成 DriverManager的初始化。
       >
       >  SPI Serviceloader 通过线程上下文获取能够加载实现类的classloader，一般情况下是 application classloader，绕过了这层限制，逻辑上打破了双亲委派原则。

   20. java虚拟机调优记录

   ```markdown
   1. jstat -gcutil -h 10 150355 10000 10000
   	 jstat日志详见 jstat日志
   
   日志详见catalina.out日志
   
   2. 使用jstack查找进程下的线程信息
   第一步：top          输入大写P 按照cpu使用情况排序         #### 找进程
   第二步：top -Hp pid           #### 看具体线程使用系统资源情况
   第三步：printf "%x\n"         #####线程id 10进制的线程id转十六进制的线程id
   第四步:jstack pid | grep     -> jstat.txt #####线程id,如果要看详细的就把 jstack pid  到具体文件
   
   3. 使用无界队列导致的fullGC
   
   也许你会觉得使用了线程池的无界队列后，任务就永远不会被丢弃，只要任务对实时性要求不高，反正早晚有消费完的一天。但是，大量的任务堆积会占用大量的内存空间，一旦内存空间被占满就会频繁地触发 Full GC，造成服务不可用，我之前排查过的一次 GC 引起的宕机，起因就是系统中的一个线程池使用了无界队列。
   
   ```

20. 有哪些方法在运行是生成一个java类，字节码操纵技术

    >  ```java
    > 
    > protected final Class<?> defineClass(String name, byte[] b, int off, int len,
    >                                  	ProtectionDomain protectionDomain)
    > protected final Class<?> defineClass(String name, java.nio.ByteBuffer b,
    >                                  	ProtectionDomain protectionDomain)
    >  ```

21. java的哪些区域会出现OutOfMemoryError异常。

    > - java堆：java.lang.OutOfMemoryError:Java heap space，比如存在内存泄漏问题；比如jvm对大小偏小；比如分配一个超大数组；而又无法进行垃圾回收来释放内存空间；
    > - Java 虚拟机栈和本地方法栈，递归调用的时候没有退出条件，由于超过栈的最大深度的时候， StackOverFlowError，如果由于试图去扩展栈空间的的时候失败，则会抛出 OutOfMemoryError。
    > - 永久代：它的大小是有限的，假如我们运行是大量创建动态类，而又没有及时回收。或者项目特别大，需要加载特别多的类，类似intern字符串缓存太多空间也会导致java.lang.OutOfMemoryError: PermGen space；

22. GC的调优思路

    > - 理解应用需求和问题，确定调优目标。假设，我们开发了一个应用服务，但发现偶尔会出现性能抖动，出现较长的服务停顿。评估用户可接受的响应时间和业务量，将目标简化为，希望 GC 暂停尽量控制在 200ms 以内，并且保证一定标准的吞吐量。
    > - 掌握 JVM 和 GC 的状态，定位具体的问题，确定真的有 GC 调优的必要。具体有很多方法，比如，通过 jstat 等工具查看 GC 等相关状态，可以开启 GC 日志，或者是利用操作系统提供的诊断工具等。例如，通过追踪 GC 日志，就可以查找是不是 GC 在特定时间发生了长时间的暂停，进而导致了应用响应不及时。
    > - 这里需要思考，选择的 GC 类型是否符合我们的应用特征，如果是，具体问题表现在哪里，是 Minor GC 过长，还是 Mixed GC 等出现异常停顿情况；如果不是，考虑切换到什么类型，如 CMS 和 G1 都是更侧重于低延迟的 GC 选项。
    > - 通过分析确定具体调整的参数或者软硬件配置。
    > - 验证是否达到调优目标，如果达到目标，即可以考虑结束调优；否则，重复完成分析、调整、验证这个过程。
    > - youngGC 频繁一般是短周期小对象较多，先考虑是不是 Eden 区/新生代设置的太小了，看能否通过调整-Xmn、-XX:SurvivorRatio 等参数设置来解决问题；
    > - 




# 设计模式 ?

- 单例模式（2种模式以及调优）

  > ```java
  > public class DoubleCheckedLocking {                     //1
  >     private  static  Instance instance;                   //2
  >     public  static Instance getInstance(){              //3
  >         if(instance ==null) {                           //4:第一次检查
  >             synchronized (DoubleCheckedLocking.class) { //5：加锁
  >                 if (instance == null)                   //6：第二次检查
  >                     instance = new Instance();          //7：问题的根源处在这里
  >             }                                           //8
  >         }                                               //9
  >         return instance;                                //10
  >     }                                                   //11
  > }
  > ```
  >
  > 1. 如果第一次检查instance不为null，那就不需要执行下面的加锁和初始化操作。因此，可以大幅降低synchronized带来的性能开销（原先是在getInstance()方法上加synchronized方法）。
  > 2. 这样似乎很完美，但这是一个错误的优化！在线程执行到第4行，代码读取到instance不为null时，`instance引用的对象可能还没有完成初始化`。
  >
  > - 问题的根源
  >
  >    前面的双重检查示例代码第7行创建了一个对象。这一行代码可以分解为如下的3行伪代码。
  >
  >   ```java
  >   memory=allocate();        //1:分配对象的内存空间
  >   ctorInstance(memory);     //2:初始化对象
  >   instance = memory;          //3:设置instance指向刚分配的内存地址
  >   ```
  >
  >    2和3有可能会进行重排序；DoubleCheckedLocking代码第7行（instance=new  Instance()；）如果发生重排序，拎一个并发执行的线程B就有可能在第4行判断instance不为null。线程B接下来访问instance所引用的对象，但此时这个对象可能还没有被A线程初始化！
  >
  >   - 解决方案
  >
  >   ```java
  >   private volatile static Instance instance
  >   ```

- 工厂模式 （静态工厂模式、工厂方法模式、抽象工厂模式）

- 策略模式

- 代理模式

- 模板方法模式

- 适配器模式

- [动态代理模式](https://www.cnblogs.com/gonjan-blog/p/6685611.html)

- [23种设计模式](https://blog.csdn.net/jason0539/article/details/44956775)  ***

> - 建造者模式   创建bean的时候使用建造者模式，不用使用多个构造函数，不用使用set方法
> - 模板方法模式（消费答题记录的场景，一个是做学员的统计相关的，一个是算试题的正答率。它的公共模式 获取消费者客户端、设置消费者组，解析订阅的topic列表，拉取消息、过滤消息、进行计算和业务处理、commit。 不同的是设置消费者组、过滤，具体的入库逻辑不同等抽象方法；
> - 策略模式（智能推题场景下获取试题列表的接口，提供三种策略。按照自己做错的试题，按照题库中所有的错误正答率，按照自己的收藏试题）
> - 单例模式 创建kafka生产者客户端，使用单例模式；
> - 适配器模式：新加接口的时候，原有的逻辑差不多，只是入参不同，加部分逻辑。  新建一个接口，在实际工作中，适配器模式大多用在代码后期维护，和复用代码以及改写工具类。
>   https://zhuanlan.zhihu.com/p/98718949
>
> 

# mybatis 

1. [常见问题](http://www.cnblogs.com/huajiezh/p/6415388.html)

2. #{}和${}的区别是什么？

   > ${}是properties文件中变量的占位符，可以用于java中属性注入和sql内部，属于静态文本替换；
   >
   > #{}是sql参数占位符，mybatis会将sql中#{}替换为？，在执行前使用prepareStatement设置参数值；

3. mybatis的xml中常用的标签

   > <resultMap>、<parameterMap>、<sql>、<include> trim|where|set|foreach|if|choose|when|otherwise|bind

4. 简述Mybatis的插件运行原理，以及如何编写一个插件。

   > Mybatis仅可以编写针对ParameterHandler、ResultSetHandler、StatementHandler、Executor这4种接口的插件，Mybatis使用JDK的动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行这4种接口对象的方法时，就会进入拦截方法，具体就是InvocationHandler的invoke()方法，当然，只会拦截那些你指定需要拦截的方法。
   >
   > 实现Mybatis的Interceptor接口并复写intercept()方法，然后在给插件编写注解，指定要拦截哪一个接口的哪些方法即可，记住，别忘了在配置文件中配置你编写的插件。
   
5. [mybatis流程及工作原理 ***](https://www.jianshu.com/p/7f8dace369ea) ***

   [源码层面的执行过程](https://blog.csdn.net/majinggogogo/article/details/72179560)

   > 1. mybatis配置Config.xml，它是mybatis的全局配置文件，配置了mybatis的运行环境等信息。mapper.xml文件即sql映射文件，文件中配置了操作数据库的sql语句。mapper.xml要在Config.xml中加载。
   >
   > 2. 通过mybatis的Config.xml中的配置生成SqlSessionFactory，默认实现DefaultSqlSessionFactory；
   > 3. 通过SqlSessionFactory生成sqlSesstion，即默认的defaulSqlSesstion；
   > 4. 通过sqlSesstion的getMapper方法获取对应的mapper接口，（其实是通过configuration类中的MapperProxyRegister类获取的对应的mapper接口的代理类）
   > 5. 然后执行mapper接口中的方法，通过代理类实现了invocationHandler接口，执行动态代理；把操作委托给Executor接口中对应的方法比如query、update、commit等方法来执行；
   > 6. 然后找到对应的statement或者prepareStatement来执行sql。
   > 7. Mapped Statement也是mybatis一个底层封装对象，它包装了mybatis配置信息及sql映射信息等，Mapped Statement在执行sql后将输出结果映射至java对象中；

# springboot  

1. [spring常用注解](https://blog.csdn.net/lafengwnagzi/article/details/53034369)

   > - @RestController注解代替 @Controller和@responseBody注解；
   >
   > - @EnableAutoConfiguration注解，这个注解告诉Spring Boot根据添加的jar依赖猜测你想如何配置Spring。由于 spring-boot-starter-web 添加了Tomcat和Spring MVC，所以auto-configuration将假定你正在开发一个web应用并相应地对Spring进行设置
   >
   > - @Configuration：Spring Boot提倡基于Java的配置，@Import 注解可以用来导入其他配置类。另外，你也可以使用 @ComponentScan 注解自动收集所有的Spring组件。
   >
   > - @SpringBootApplication 等价于@Configuration @EnableAutoConfiguration 和 @ComponentScan；
   >
   > - @ConfigurationProperties(prefix="connection") 注入熟悉文件的前缀约束；
   >
   > - @ResponseBody：表示该方法的返回结果直接写入HTTP response body中，一般在异步获取数据时使用，在使用@RequestMapping后，返回值通常解析为跳转路径，加上@responsebody后返回结果不会被解析为跳转路径，而是直接写入HTTP response body中。比如异步获取json数据，加上@responsebody后，会直接返回json数据。
   >
   > - @AutoWired，byType方式。把配置好的Bean拿来用，完成属性、方法的组装
   >
   > - @RequestParam：用在方法的参数前面。
   >
   >   > ```java
   >   > @RequestParam String a =request.getParameter("a")。
   >   > ```
   >
   > - @PathVariable：路径变量
   >
   >   ```java
   >   @RequestMapping("user/get/mac/{macAddress}")
   >   public String getByMacAddress(@PathVariable String macAddress)
   >   ```
   >
   > - @ControllerAdvice：全局异常处理。@ExceptionHandler（Exception.class）
   >
   >   ```java
   >   
   >   /**
   >    * 全局异常处理
   >    */
   >   @ControllerAdvice
   >   class GlobalDefaultExceptionHandler {
   >       public static final String DEFAULT_ERROR_VIEW = "error";
   >    
   >       @ExceptionHandler({TypeMismatchException.class,NumberFormatException.class})
   >       public ModelAndView formatErrorHandler(HttpServletRequest req, Exception e) throws Exception {
   >           ModelAndView mav = new ModelAndView();
   >           mav.addObject("error","参数类型错误");
   >           mav.addObject("exception", e);
   >           mav.addObject("url", RequestUtils.getCompleteRequestUrl(req));
   >           mav.addObject("timestamp", new Date());
   >           mav.setViewName(DEFAULT_ERROR_VIEW);
   >           return mav;
   >       }}
   >   
   >   ```

2. [springboot和spring的区别](https://www.jianshu.com/p/4784b4fabd0e)

   >- Spring 是一站式的轻量级的java开发框架，核心是控制反转（IOC）和面向切面（AOP），针对于开发的WEB层(springMvc)、业务层(Ioc)、持久层(jdbcTemplate)等都提供了多种配置解决方案。
   >- springMVC是spring基础之上的一个MVC框架，主要处理web开发的路径映射和视图渲染，属于spring框架中WEB层开发的一部分
   >- Spring boot，Spring Boot本身并不提供Spring框架的核心特性以及扩展功能。它是一种基于约定优于配置的理念，简化了spring的配置流程，降低了项目搭建的复杂度，更快的搭建服务。 因为 Spring 的配置非常复杂，各种XML、 JavaConfig、处理起来比较繁琐。springboot简化简化配置。
   
3. [springboot的spi机制 ***](https://juejin.im/post/5d2c80da6fb9a07ec42b8bd4)

   >   `SPI机制`已经定义好了加载服务的流程框架, 你只需要按照约定, 在META-INF/services目录下面, 以接口的全限定名称为名创建一个文件夹(com.north.spilat.service.Search), 文件夹下再放具体的实现类的全限定名称(com.north.spilat.service.impl.DatabaseSearch), 系统就能根据这些文件,加载不同的实现类.这就是SPI的大体流程.
   >
   >   [springboot的流程](https://www.jianshu.com/p/0d196ad23915)
   >
   >   - 在jar包下的META-INF/spring.factories下有org.springframework.boot.autoconfigure.EnableAutoConfigurations属性下有多个需要注入的autoConfigration配置文件；
   >   - 在main方法启动的时候在SpringApplication类下的构造方法中，会获取到springFactoryLoader的类加载器类来查找指定的接口实现类并实例化。
   >   - 其中loadFactoryNames方法是从指定位置查找指定接口的实现类的全限定名；
   >   - 使用instantiateFactory 根据类名的全限定名称实例化；
   >
   >   在springboot的自动装配过程中，最终会加载META-INF/spring.factories文件，而加载的过程是由SpringFactoriesLoader加载的。从CLASSPATH下的每个Jar包中搜寻所有META-INF/spring.factories配置文件，然后将解析properties文件，找到指定名称的配置后返回。需要注意的是，其实这里不仅仅是会去ClassPath路径下查找，会扫描所有路径下的Jar包，只不过这个文件只会在Classpath下的jar包中。
   >
   >   

# springcloud

# springMVC

1. [springmvc的原理流程](https://www.jianshu.com/p/8a20c547e245)

2. [springmvc工作原理-源码](https://www.jianshu.com/p/8a20c547e245)

   > - 客户端请求提交到DispatcherServlet
   >
   > - 由DispatcherServlet控制器查询HandlerMapping，找到并分发到指定的Controller中。
   > - Controller调用业务逻辑处理后，返回ModelAndView
   > - DispatcherServlet查询一个或多个ViewResoler视图解析器，找到ModelAndView指定的视图
   > - 视图负责将结果显示到客户端

# spring

[ioc的源码解析](https://javadoop.com/post/spring-ioc)

[Spring容器IOC初始化过程](https://juejin.im/post/5ab30714f265da237b21fbcc#heading-0)

[Spring常见问题](http://blog.csdn.net/qq1137623160/article/details/71194429) 

1. [BeanFactory和ApplicationContext有什么区别](https://www.jianshu.com/p/fd8e441b98c8) ***

   > - BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。其中ApplicationContext是BeanFactory的子接口。
   >
   > 1. BeanFactory：是Spring里面最底层的接口，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，控制bean的生命周期，维护bean之间的依赖关系。ApplicationContext接口作为BeanFactory的派生，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：
   >
   >    ①继承MessageSource，因此支持国际化；
   >
   >    ② 支持获取环境属性，Enviroment对象，包括jdk、profile等属性；
   >
   >    ③统一的资源文件访问方式，定义了resource接口；
   >
   >    ④提供在监听器中注册bean的事件；
   >
   > 2. ①BeanFactroy采用的是延迟加载形式来注入Bean的，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。这样，我们就不能发现一些存在的Spring的配置问题。springApplicationContext容器启动时会实例化非懒加载和单例的bean；
   >
   > 3. BeanFactory通常以编程的方式被创建，ApplicationContext还能以声明的方式创建，如使用ContextLoader。
   >
   > 4. BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册。
   
2. spring的好处

   > - 轻量： Spring 是轻量的，基本的版本大约2MB。
   > - 控制反转： Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
   > - 面向切面的编程(AOP)： Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开；
   > - 容器： Spring 包含并管理应用中对象的生命周期和配置。
   > - MVC框架： Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
   > - 事务管理： Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。
   > - 异常处理： Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常

3. BeanFactory 实现举例/applicationContext实现举例

   > Bean 工厂是工厂模式的一个实现，提供了控制反转功能，用来把应用的配置和依赖从正真的应用代码中分离。最常用的BeanFactory 实现是XmlBeanFactory 类。
   >
   > FileSystemXmlApplicationContext、ClassPathXmlApplicationContext、WebXmlApplicationContext

4. spring的注入方式

   >- 构造器依赖注入
   >- 接口注入
   >- setter注入
   >- 最好的解决方案是用构造器参数实现强制依赖，setter方法实现可选依赖

5. 如何给spring容器配置元数据

   > - 基于xml
   > - 基于注解的配置
   > - 基于java config的配置

6. spring支持的bean的作用域

   > Singleton、protype、request、sesstion

7. spring中单例bean是线程安全的吗

   > no

8. spring中bean生命周期的方法

   > The bean 标签有两个重要的属性（init-method和destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct和@PreDestroy）

9. 多个相同类型的bean注入

   > 将@Qualifier 注解和@Autowire 注解结合使用以消除这种混淆

10. [spring生命周期 ***](https://www.jianshu.com/p/3944792a5fff)

    > 生命周期执行的过程如下:
    >
    >  实例化、属性注入、执行初始化、销毁；
    >
    > - 实例化，实例化前后会进行BeanPostProcessor的前置后置处理；
    > - 依赖注入的过程，如果实现了beanWare接口（beanNameAware/BeanFactoryAware/ApplicationContextAware）则会注入响应的属性；
    > - 执行初始化，如果你实现了 InitializingBean接口 则会进行afterPropertiesSet注入，以及init-menthod方法，进行一些初始化操作；
    > - destroy销毁；
    >
    > 1. spring对bean进行实例化,默认bean是单例
    > 2. spring对bean进行依赖注入
    > 3. 如果bean实现了BeanNameAware接口,spring将bean的id传给setBeanName()方法
    > 4. 如果bean实现了BeanFactoryAware接口,spring将调用setBeanFactory方法,将BeanFactory实例传进来
    > 5. 如果bean实现了ApplicationContextAware()接口,spring将调用setApplicationContext()方法将应用上下文的引用传入
    > 6. 如果bean实现了[BeanPostProcessor](https://www.jianshu.com/p/dcc990d47df1)接口,spring将调用它们的postProcessBeforeInitialization接口方法  
    > 7. 如果bean实现了InitializingBean接口,spring将调用它们的afterPropertiesSet接口方法,类似的如果bean使用了init-method属性声明了初始化方法,改方法也会被调用
    > 8. 如果bean实现了BeanPostProcessor接口,spring将调用它们的postProcessAfterInitialization接口方法
    > 9. 此时bean已经准备就绪,可以被应用程序使用了,他们将一直驻留在应用上下文中,直到该应用上下文被销毁
    > 10. 若bean实现了DisposableBean接口,spring将调用它的distroy()接口方法。同样的,如果bean使用了destroy-method属性声明了销毁方法，则该方法被调用

11. [aop详解](https://www.cnblogs.com/hongwz/p/5764917.html)
    [aop实战](https://juejin.im/post/5a55af9e518825734d14813f)

    > `含义`：
    >
    > aop是相对于oop而言的，它是面向切面编程。例如日志、权限，是散在各个对象中的，而面向切面编程就是将那些与业务无关的，各个对象都用到的功能封装成一个切面，以便减少系统的重复代码，降低模块间的耦合度。
    >
    > - 切面：类是对物体特征的抽象，切面就是对横切关注点的抽象；
    > - 连接点：被拦截到的点，因为Spring只支持方法类型的连接点，所以在Spring中连接点指的就是被拦截到的方法；
    > - 通知：指的就是指拦截到连接点之后要执行的代码，通知分为前置、后置、异常、最终、环绕通知五类；
    > - 目标对象：被代理的目标对象；
    > - 织入：将切面应用到目标对象并导致代理对象创建的过程；

12. spring对aop的支持

    > Spring中AOP代理由Spring的IOC容器负责生成、管理，其依赖关系也由IOC容器负责管理。
    >
    > 1. 默认使用Java提供的jdk动态代理来创建AOP代理，这样就可以为任何接口实例创建代理了;
    > 2. 当需要代理的类不是代理接口的时候，Spring会切换为使用CGLIB代理，也可强制使用CGLIB;
    >
    > 步骤：
    >
    > 1. 定义普通业务组件
    > 2. 定义切入点，一个切入点可能横切多个业务组件
    > 3. 定义增强处理，增强处理就是在AOP框架为普通业务组件织入的处理动作

13. [jdk动态代理cglib的区别](https://www.jianshu.com/p/abb674bb418c)

    [jdk动态代理实例](https://blog.csdn.net/yaomingyang/article/details/80981004)

    [CGLIB动态代理实例 ](https://blog.csdn.net/yhl_jxy/article/details/80633194)  ***

    > - JDK动态代理只能对实现了接口的类生成代理，而不能针对类；目标对象作为代理对象的一个属性，具体接口实现中，可以在调用目标对象相应方法前后加上其他业务处理逻辑；
    >
    > 代码层面主要是通过proxy类的newProxyInstance方法生成代理类；通过实现InvocationHandler接口的invoke方法，注入被代理类，执行以前的方法；
    >
    > - CGLIB是针对类实现代理，主要是对指定的类生成一个子类，覆盖其中的方法（继承）；它使用的是`字节码生产技术`，在编译的时候生成的代理类，不能代理被final修饰的属性或者类；
    >
    > jdk创建的快，cglib运行的快；

14. [为什么jdk动态代理一定需要实现接口](https://segmentfault.com/a/1190000021821314)

    >  jdk动态代理是为接口生成代理对象，该代理对象（实现类）继承了java标准类库proxy.java并实现了目标对象。由于java遵从单继承多实现的原则，所以jdk无法利用继承为目标对象生成代理对象。

15. [spring是如何解决循环依赖2 ](https://blog.csdn.net/u010853261/article/details/77940767) ***

    [spring是如何解决循环依赖3](https://juejin.im/post/5c98a7b4f265da60ee12e9b2)     ***

    > 需要明确的是spring对循环依赖的处理有三种情况： ①构造器的循环依赖：这种依赖spring是处理不了的，直 接抛出BeanCurrentlylnCreationException异常。 ②单例模式下的setter循环依赖：通过“三级缓存”处理循环依赖。 ③非单例循环依赖：无法处理。
    >
    > `对象的初始化方法分为三步`
    >
    > 1. createBeanInstance：实例化，其实也就是调用对象的构造方法实例化对象
    > 2. populateBean：填充属性，这一步主要是多bean的依赖属性进行填充
    > 3. initializeBean：调用spring xml中的init 方法。
    >
    > `构造器循环依赖` 
    >
    > 无法解决通过构造器注入构成的循环依赖，只能抛出BeanCurrentylyInCreationException异常表示循环依赖。
    >  Spring容器将每一个正在创建的bean标识符放在一个“当前创建bean池”中，bean表示符在创建过程中将一直保持在这个池中，因此，如果在创建bean过程中发现自己已经在“当前创建bean池”中，将抛出BeanCurrentylyInCreationException异常，表示循环依赖；
    >
    > `单例模式下的set注入的循环依赖`
    >
    > spring有三级缓存，它的作用分别是：
    >
    > - singletonFactories ： 进入实例化阶段的单例对象工厂的cache （三级缓存）
    >
    > - earlySingletonObjects ：完成实例化但是尚未初始化的，提前暴光的单例对象的Cache （二级缓存）
    >
    > - singletonObjects：完成初始化的单例对象的cache（一级缓存）
    >
    > ```
    > A的某个field或者setter依赖了B的实例对象，同时B的某个field或者setter依赖了A的实例对象”这种循环依赖的情况。A首先完成了初始化的第一步，并且将自己提前曝光到singletonFactories中，此时进行初始化的第二步，发现自己依赖对象B，此时就尝试去get(B)，发现B还没有被create，所以走create流程，B在初始化第一步的时候发现自己依赖了对象A，于是尝试get(A)，尝试一级缓存singletonObjects(肯定没有，因为A还没初始化完全)，尝试二级缓存earlySingletonObjects（也没有），尝试三级缓存singletonFactories，由于A通过ObjectFactory将自己提前曝光了，所以B能够通过ObjectFactory.getObject拿到A对象(虽然A还没有初始化完全，但是总比没有好呀)，B拿到A对象后顺利完成了初始化阶段1、2、3，完全初始化之后将自己放入到一级缓存singletonObjects中。此时返回A中，A此时能拿到B的对象顺利完成自己的初始化阶段2、3，最终A也完成了初始化，进去了一级缓存singletonObjects中，而且更加幸运的是，由于B拿到了A的对象引用，所以B现在hold住的A对象完成了初始化。
    > ```
    >
    > 通过Spring容器提前暴露刚完成实例化但未完成其他步骤（如setter注入）的bean来完成的。`通过提前暴露一个单例工厂方法，从而使其他bean能够引用到该bean`

16. spring用到了哪些设计模式

    > -  [BeanFactory](https://github.com/spring-projects/spring-framework/blob/master/spring-beans/src/main/java/org/springframework/beans/factory/BeanFactory.java)和[ApplicationContext](https://github.com/spring-projects/spring-framework/blob/master/spring-context/src/main/java/org/springframework/context/ApplicationContext.java)应用了工厂模式。
    >- 在 Bean 的创建中，Spring 也为不同 scope 定义的对象，提供了单例和原型等模式实现。
    > - AOP 领域则是使用了代理模式、装饰器模式、适配器模式等。
    > - 各种事件监听器，是观察者模式的典型应用。
    > - 类似 JdbcTemplate 等则是应用了模板模式。

17. @Autowired和@Resource的区别

    > - 前者是spring的注解，默认是按照bean的Type进行注入的，可以配置require =false表示可为空，可以配合@qualifier按照beanName注入；
    > - 后者是java提供的annotation注解，但是spring支持，他是默认按照名字注入。可以指定name和type字段来注入；

18. spring事务相关  ？？？？

    > - 
    > -  事务简介

19. spring是如何实现事务 ***

    >  [源码层面](https://my.oschina.net/zhangxufeng/blog/1973493) ,
    >
    >  [事务介绍](https://zhuanlan.zhihu.com/p/54067384)
    >
    >  > - 动态代理是Spring实现AOP的默认方式，分为两种：**JDK动态代理**和**CGLIB动态代理**。JDK动态代理面向接口，通过反射生成目标代理接口的匿名实现类；CGLIB动态代理则通过继承，使用字节码增强技术（或者`objenesis`类库）为目标代理类生成代理子类。Spring默认对接口实现使用JDK动态代理，对具体类使用CGLIB，同时也支持配置全局使用CGLIB来生成代理对象。
    >  >
    >  > - 事务的抽象：
    >  >
    >  >   >  `PlatformTransactionManager`定义了事务的操作行为，获取事务，提交回滚；
    >  >   >
    >  >   > `TransactionDefinition`定了了事务的属性，事务的传播行为（默认是PROPAGATION_REQUIRED，比如在有事务的上下文中运行，如果有事务则加入事务，如果没有则创建一个事务），事务的隔离级别（默认是可重复读，只有在新事物中才有效，`PROPAGATION_REQUIRED`和`PROPAGATION_REQUIRES_NEW`传播行为中有效）；exceptionFor（特定的异常回滚）；事务超时时间等；
    >  >
    >  > - 事务的切面
    >  >
    >  >   >  代理对象生成的核心类`AbstractAutoProxyCreator，实现了`BeanPostProcessor接口，会在Bean初始化完成之后通过postProcessAfterInitialization方法生成代理对象。
    >  >   
    >  > - TransactionInterceptor 中调用invoke方法，然后通过他的支持类`TransactionAspectSupport`的`invokeWithinTransaction`方法进行事务处理，

# mongodb

1. MongoDB简介

   > `优点`：
   >
   > - MongoDB是非关系型数据库，有数据库、集合、文档的概念；
   >
   > - 使用类似于json格式的bson结构存储文档数据；支持自定义字段，动态创建字段，更适合存储数组和更复杂的结构；
   > - 数据是存储在硬盘上的，但将热数据和索引存在于内存中，使得热数据的查询非常快；但如果索引的大小超过内存则MongoDB会删除一些内存，导致性能急剧下降；
   > - MongoDB可以提供副本集的高可用性。使用mongos、当主副本发生故障时，副本集可以将辅助副本升级为主服务器；
   > - 负载均衡-MongoDB使用分片的概念，通过MongoDB实例之间拆分数据来水平扩展。
   >
   > `缺点`：
   >
   > - 不支持事务；
   > - 使用的是内置函数，不是sql；所以使用node.js操作比较方便；
   > - 和mysql比成熟度相对较低；

# mysql

1. [mysql的innodb和mylsam存储引擎实现区别](https://blog.csdn.net/qq_35642036/article/details/82820178) ***

   > 1. innodb是支持事务、支持外键等高级功能，带来性能损耗；mylsam是不支持的，所以性能高点；
   > 2. innodb支持行级锁和表锁；mylsam只支持表锁；
   > 4. innodb的主键索引和数据都放到一个数据文件中，是聚集索引；mylsam是索引和数据文件是分开存放的，是非聚集索引；
   > 5. 实现结构上的区别：
   > 
   >  - 第一个重大区别是InnoDB的数据文件本身就是索引文件。MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。而在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录。这个索引的key是数据表的主键，因此InnoDB表数据文件本身就是主索引。
   >   - 第二个InnoDB的辅助索引data域存储相应记录主键的值而不是地址。换句话说，InnoDB的所有辅助索引都引用主键作为data域，mylsam使用data域中存放的事数据行记录的地址；
   > 
   
2. [一颗b+树可以存储多少数据  ***](mysql b+树能存多少条数据)

   >  这里我们`先假设B+树高为2`，即存在一个根节点和若干个叶子节点，那么这棵B+树的存放总记录数为：根节点指针数*单个叶子节点记录行数。
   >
   > 上文我们已经说明单个叶子节点（页）中的记录数=16K/1K=16。（这里假设一行记录的数据大小为1k，`实际上`现在很多互联网业务`数据记录大小通常就是1K左右`）。
   >
   > 那么现在我们需要计算出`非叶子节点能存放多少指针`，其实这也很好算，我们假设`主键ID为bigint类型，长度为8字节`，而`指针大小`在InnoDB源码中设置为`6字节`，这样一共14字节，我们一个页中能存放多少这样的单元，其实就代表有多少指针，即`16384/14=1170`。那么可以算出一棵`高度为2的B+树`，能存放`1170*16=18720条`这样的数据记录。
   >
   > 根据同样的原理我们可以算出一个`高度为3的B+树`可以存放：`1170*1170*16=21902400条`这样的记录。所以在InnoDB中`B+树高度一般为1-3层`，它就能`满足千万级的数据存储`。在查找数据时 **`一次页的查找代表一次IO`**， 所以通过主键索引查询通常 **`只需要1-3次IO操作`** 即可查找到数据。
   >
   > 

3. [数据库事务](https://www.cnblogs.com/fjdingsd/p/5273008.html)

   > - 原子性：是指事务内的所有操作，要么全部成功，要么全部回滚；
   > - 一致性：是指数据库从一个一致性状态变为另一个一致性状态，事务执行前后都处于一致性状态,比如转账的例子转账前后两个账户的钱的总和不变；
   > - 隔离性：多个事务并发执行，各个事务之间要相互隔离，互不影响；
   > - 持久性：指一个事务一旦提交了，对数据的改变就是永久性的，即使数据库服务挂了也不会改变；

4. 事务带来的问题

   > - 脏读：一个事务读取了另一个未提交事务的数据；
   > - 不可重复读：同一个事务多次查询读取到的同一条数据的某个字段不一致，这是被另一个事务修改了，侧重点是修改；
   > - 幻读：同一个事务中，查询的数据的数量不一致，有可能是修改了或者新增，因为另一个事务进行了删除或者新增操作，侧重点是增删。这个用到的查询条件是非主键索引、非唯一索引；

5. 事务的隔离级别

   > - 读未提交：最低级别，任何情况都无法保证；
   > - 读已提交：可避免脏读；
   > - 可重复读：可避免脏读、不可重复读；
   > - 串行化: 可避免脏读、幻读、不可重复读；

6. 你所了解的mysql索引

   > 主键索引、联合索引、覆盖索引、唯一索引

7. 数据库的优化总结  ***

   >   `创建索引`
   >
   > - where、join、order by、group by、sum、count、in、distinct 字段中创建索引；
   > - 频繁变化的字段不适合作为索引，维护索引带来的性能损耗太大；
   > - 区分度低的不适合创建索引；
   >
   > `使用索引`
   >
   > - 尽量使用覆盖索引；
   > - 不要对where字段进行表达式计算；
   > - order by a,b 两个字段都是降序或者升序，否则用不到索引；
   > - 联合索引注意最左索引 。
   > - 不要使用!= <>,可以使用between或者in代替，否则用不到索引；
   > - 最左前缀匹配原则，非常重要的原则，mysql会一直向右匹配直到遇到范围查询(>、<、between、like)就停止匹配。
   > - 长字段索引使用前缀索引；
   >
   > `存储字段的选择`
   >
   > - 尽量使用not null约束，应为null会消耗额外的空间记录它是否为空；
   > - 选择合适的字段类型，比如主键尽量选择自增id，guid比较长，占空间以及太长查找起来比较慢；datetime占用8个字节，而timestamp占用4个字节，只用了一半，而timestamp表示的范围是1970—2037；
   > - 文件和图片使用文件系统存储，数据库只存储地址；
   > - 尽量避免使用子查询；
   >
   > `水平和垂直拆分`
   >
   > - 对表记录进行水平拆分分表
   > - 对表字段多的结构进行垂直拆分；
   > - 批量操作，尽量避免频繁读写
   > - 数据库尽量使用集群，读写分离；

8. [explain进行sql分析](https://blog.csdn.net/why15732625998/article/details/80388236) ***

   > [mysql慢查询 + explain使用](https://blog.csdn.net/wuhuagu_wuhuaguo/article/details/80625124)
   >
   > [order by实现原理](https://blog.csdn.net/hguisu/article/details/7161981)
   >
   > - id
   >
   >   >  相同的从上倒下执行，不同的从小到大执行；
   >
   > - Select_type
   >
   >   > - SIMPLE `简单的select查询`，查询中`不包含子查询或者UNION`
   >   > - PRIMARY 查询中若`包含任何复杂的`子部分，`最外层查询则被标记为PRIMARY`
   >   > - SUBQUERY `在SELECT或WHERE列表中包含了子查询`
   >   > - DERIVED 在FROM列表中包含的`子查询被标记为DERIVED`（衍生），MySQL会递归执行这些子查询，把`结果放在临时表`中
   >   > - UNION 若第二个SELECT出现在UNION之后，则被标记为UNION：若UNION包含在FROM子句的子查询中，外层SELECT将被标记为：DERIVED
   >
   > - Type
   >
   >   > - System：表只有一行记录；
   >   >
   >   > - Const：表示通过索引一次就找到了，const用于比较primary key 或者unique索引。因为只匹配一行数据，所以很快；
   >   >
   >   > - eq_ref: 只匹配到一行的时候，一般在连表查询的时候匹配到一行，或者等值计算的时候 =；
   >   >
   >   > - ref : 非唯一性索引扫描，根据索引查询匹配到多行或者关联查询匹配到多行；
   >   >
   >   > - index_merge：在一个查询里面很有多索引用被用到，可能会触发`index_merge`的优化机制。
   >   >
   >   > - range：只有给定范围内的行才能被检索，使用索引来查询出多行。  `key_len`列表示使用的最长的 key 部分。 这个类型的`ref`列是NULL；
   >   >
   >   > - Index：`index`类型和`ALL`类型一样，区别就是`index`类型是扫描的索引树。
   >   >
   >   >   如果索引是查询的覆盖索引，就是说索引查询的数据可以满足查询中所需的所有数据，则只扫描索引树，不需要回表查询。 在这种情况下，explain 的 `Extra` 列的结果是 `Using index`。仅索引扫描通常比ALL快，因为索引的大小通常小于表数据；
   >   >
   >   
   > - key_len
   >
   >   >  表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度，在`不损失精确性的情况下，长度越短越好`。key_len显示的值为索引字段的最大可能长度，并非实际使用长度
   >
   > - key：实际用到的索引
   >
   > - rows：返回结果所需检查的行数
   >
   > - Extra： Using filesort、Using temporary（ORDER BY）[extra详解](https://www.cnblogs.com/kerrycode/p/9909093.html)
   >
   >   > - Using where：如果不读取表的所有数据（where过滤数据），或不是仅仅通过索引就可以获取所有需要的数据；
   >   > - Using index：mysql将使用覆盖索引，以避免访问表；
   >   > -  Using filesort：当Query 中包含 ORDER BY 操作，而且无法利用索引完成排序操作的时候，MySQL Query Optimizer 不得不选择相应的排序算法来实现。
   >   > - Using temporary：当 MySQL 在某些操作中必须使用临时表时，在 Extra 信息中就会出现Using temporary 。主要常见于 GROUP BY 和 ORDER BY 等操作中。
   >   > - Using Index Condition：会先条件过滤索引，过滤完索引后找到所有符合索引条件的数据行，随后用 WHERE 子句中的其他条件去过滤这些数据行；一般是联合索引中只用了部分所以你的情况？
   >
   >   

9. [关于SQL数据库中的范式](https://blog.csdn.net/sinat_35512245/article/details/52923516)

   > - 第一范式：强调的是列的原子性，即不能再分成其它几列；
   >
   > - 第二范式：一是表必须有一个主键；二是没有包含在主键中的列必须完全依赖于主键，而不能只依赖于主键的一部分；
   >
   > - 第三范式：另外非主键列必须直接依赖于主键，不能存在传递依赖。即不能存在：非主键列 A 依赖于非主键列 B，非主键列 B 依赖于主键的情况；
   >
   >   三范式：
   >
   > - ```
   >   第一范式:数据库表的每一个字段都是不可分割的。
   >   第二范式:数据库表中的非主属性只依赖于主键。
   >   第三范式:不存在非主属性对关键字的传递函数依赖关系。
   >   ```

10. [mysql锁的分类](https://blog.csdn.net/qq_34337272/article/details/80611486) ***

   > - 表级锁粒度最大，加锁资源消耗少，最简单，触发锁的冲突高；更新大表中的大批量数据的时候用表锁效率最高；
   >
   > - 行级锁粒度最小，加锁资源消耗大，实现起来复杂，但是表的并发度最大；包括record lock、gap lock（范围锁，防止别的事务新增幻影行）、next-key lock 锁定记录本身以及间隙，可解决幻读问题；
   >
   > - 共享锁：事务T对数据A加上共享锁，其它事务只能对数据A加共享锁，不能加排它锁，获取共享锁的事务只能读取数据，不能修改数据；
   >
   > - 排它锁：事务T对数据A加上排它锁后，其它事务不能对A加任何类型的锁，获取排它锁的事务既可以修改数据也可以读数据；
   >
   > - 意向锁：innodb的意向锁主要用户多粒度的锁并存的情况。比如事务A要在一个表上加S锁，如果表中的一行已被事务B加了X锁，那么该锁的申请也应被阻塞。如果表中的数据很多，逐行检查锁标志的开销将很大，系统的性能将会受到影响。为了解决这个问题，可以在表级上引入新的锁类型来表示其所属行的加锁情况，这就引出了“意向锁”的概念。
   >
   >   举个例子，如果表中记录1亿，事务A把其中有几条记录上了行锁了，这时事务B需要给这个表加表级锁，如果没有意向锁的话，那就要去表中查找这一亿条记录是否上锁了。如果存在意向锁，那么假如事务Ａ在更新一条记录之前，先加意向锁，再加Ｘ锁，事务B先检查该表上是否存在意向锁，存在的意向锁是否与自己准备加的锁冲突，如果有冲突，则等待直到事务Ａ释放，而无须逐条记录去检测。事务Ｂ更新表时，其实无须知道到底哪一行被锁了，它只要知道反正有一行被锁了就行了。

11. 如何避免死锁

    > innodb的行锁是基于索引实现的，未命中所以则会使用表锁；
    >
    > - 使用表锁；
    > - 同一个事务一次尽量获取所有需要的锁；
    > - 多个程序以相同的顺序访问表；

12. 处理死锁的思路 [mysql锁机制](https://juejin.im/post/5b82e0196fb9a019f47d1823)

    >  ```
    > 第一种方案
    > 
    > 1. 查询是否锁表
    > show OPEN TABLES where In_use > 0;
    > 2. 查询进程（如果您有SUPER权限，您可以看到所有线程。否则，您只能看到您自己的线程）
    > show processlist
    > 3. 杀死进程id（就是上面命令的id列）
    > kill id
    > 
    > 第二种方案
    > 
    > 1. 查看当前的事务
    > SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;
    > 2. 查看当前锁定的事务
    > SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;
    > 3. 查看当前等锁的事务
    > SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;
    > 4. 杀死进程
    > kill 进程ID
    > 
    > 查看表锁加锁情况
    > show open tables where in_use > 0;  // 1表示加锁，0表示未加锁。
    > 
    > show status like 'table_locks%' 
    > // table_locks_immediate: 表示立即释放表锁数。table_locks_waited: 表示需要等待的表锁数
    > 
    >  ```

13. 行锁的优化思路

    >  mysql> show status like 'innodb_row_lock%';
    >
    > **innodb_row_lock_current_waits:** 当前正在等待锁定的数量
    >
    > **innodb_row_lock_time:** 从系统启动到现在锁定总时间长度；非常重要的参数，
    >
    > **innodb_row_lock_time_avg:** 每次等待所花平均时间；非常重要的参数，
    >
    > **innodb_row_lock_time_max:** 从系统启动到现在等待最常的一次所花的时间；
    >
    > **innodb_row_lock_waits:** 系统启动后到现在总共等待的次数；非常重要的参数。直接决定优化的方向和策略。
    >
    > 优化思路
    >
    > - 尽可能让所有数据检索都通过索引来完成，避免无索引行或索引失效导致行锁升级为表锁。
    > - 尽可能避免间隙锁带来的性能下降，减少或使用合理的检索范围。
    >
    > - 尽可能减少事务的粒度，比如控制事务大小，而从减少锁定资源量和时间长度，从而减少锁的竞争等，提供性能。
    > - 尽可能低级别事务隔离，隔离级别越高，并发的处理能力越低。
    >
    > 
    >
    > 

14. **varchar和char 的区别：**

    >  char是一种固定长度的类型，varchar则是一种可变长度的类型，
    >
    > `它们的区别是`： char(M)类型的数据列里，每个值都占用M个字节，如果某个长度小于M，MySQL就会在它的右边用空格字符补足．（在检索操作中那些填补出来的空格字符将被去掉）
    >
    > 在varchar(M)类型的数据列里，每个值只占用刚好够用的字节再加上一个用来记录其长度的字节（即总长度为L+1字节）．
    >
    > 

15. [mysql加锁详解系列](https://www.cnblogs.com/crazylqy/p/7611069.html)

16. mysql在rr隔离级别下如何解决幻读 ***

    > 使用行锁加间隙锁来解决的；
    >
    > - Repeatable Read隔离级别下，age列上有一个非唯一索引，对应SQL：delete from t1 where age = 10; 首先，通过id索引定位到第一条满足查询条件的记录，加记录上的X锁在GAP上的GAP锁，然后加主键聚簇索引上的记录X锁，然后返回；然后读取下一条，重复进行。直至进行到第一条不满足条件的记录[11,f]，此时，不需要加记录X锁，但是仍旧需要加GAP锁，最后返回结束。
    > - 什么时候会取得gap lock或nextkey lock  这和隔离级别有关,只在REPEATABLE READ或以上的隔离级别下的特定操作才会取得gap lock或nextkey lock。
    > - mysql 的重复读解决了幻读的现象，但是需要 加上 select for update/lock in share mode 变成当读避免幻读，普通读select存在幻读

17. 与mvcc相关的概念

    > - 快照读：简单的select操作，属于快照读。`读取的是记录的可见版本` (有可能是历史版本)，不用加锁。
    >- 当前读：插入/更新/删除操作，属于当前读。`读取的是记录的最新版本`，并且当前读返回的记录，都会加上锁（悲观锁、排它锁），保证其他事务不会再并发修改这条记录。
    > 
    >- 在MySQL/InnoDB中，所谓的读不加锁，并不适用于所有的情况，而是隔离级别相关的。Serializable隔离级别，读不加锁就不再成立，所有的读操作，都是当前读。

18. MVCC的优点

    > - 多版本并发控制（MVCC）是一种用来解决`读-写冲突`的**无锁并发控制**，也就是为事务分配单向增长的时间戳，为每个修改保存一个版本，版本与事务时间戳关联，`读操作只读该事务开始前的数据库的快照`；
    > - 在并发读写数据库时，可以做到在读操作时不用阻塞写操作，写操作也不用阻塞读操作，提高了数据库并发读写的性能；

19. [mysql是如何实现mvcc的](https://blog.csdn.net/sofia1217/article/details/50778906)  

     [mysql的mvcc详细实现](https://blog.csdn.net/SnailMann/article/details/94724197)

    [mvcc机制及原理](https://chenjiayang.me/2019/06/22/mysql-innodb-mvcc/#%E4%BB%80%E4%B9%88%E6%98%AF-mvcc)  ***

    > MVCC实现了mysql的事务的隔离性，该模型在MySQL中的具体实现则是由 **`3个隐式字段`**，**`undo日志`** ，**`Read View`** 等去完成的。
    >
    > 1. DATA_TRX_ID（事务id）：标识最近修改本行记录的事务标识符；
    >
    > 2. DATA_ROLL_PTR（回滚指针）：回滚的事务段，undo log record(撤销日志记录)，就是重建该行之前的内容；
    > 3. DB_ROW_ID：隐含的自增ID（隐藏主键），如果数据表没有主键，InnoDB会自动以`DB_ROW_ID`产生一个聚簇索引；
    >
    > `mvcc有以下特点`：MVCC多版本并发控制指的是 **“维持一个数据的多个版本，使得读写操作没有冲突”** 
    >
    > - 乐观锁；
    >
    > - 每行数据都存储一个版本，每行数据更新时都更新版本；
    > - 修改时copy出当前数据，各个事务之间无干扰；
    > - 保存时比较当前版本，成功则覆盖原纪录；失败则放弃（copy、rollback）；
    >
    > `innodb的实现mvcc修改记录的方式`：
    >
    > - 用排它锁锁定该行；
    > - 把该行修改前的值copy到undo Log中；
    > - 修改当前行的值，填写事务id，使回滚指针指向undo log中的修改前的行，如果失败就rollback；
    > - 记录redo日志，包括undo log中的变化；
    >
    > `插入和删除更简单`:
    >
    > insert会产生一条新纪录，将当前事务id插入到事务id字段中；
    >
    > 删除操作和更新操作类似，事务id存放当前事务id，将删除标记为设置为删除；
    >
    > select操作，可以读取事务id小于当前事务id以及未被删除的记录；
    >
    > `如何实现一致性读 —— ReadView`：Read View就是事务进行`快照读`操作的时候生成的`读视图`(Read View)。即生成数据库系统当前的一个快照，记录并维护系统当前活跃事务的ID；
    >
    > 1. 如果被访问版本的事务id小于列表中最小的事务id，则证明事务id是之前生成的，所以该版本可以被当前事务访问；
    >
    > 2.  如果当前事务id大于id列表中最大的值，则数据不可以被当前事务访问，需要根据undolog日志链表找到前一个版本，根据前一个版本判断可见性。
    >
    > 3. 如果在列表中，则证明是活跃的事务id，则需要判断undo Log链的上一个版本，然后在重新计算一次；
    > 4. 判断删除标记位，删除标记要么为空，要么大于当前事务版本号；
    >
    > `mvvc的标准实现和innodb的实现区别`
    >
    > Innodb的实现真算不上MVCC，因为并没有实现核心的多版本共存，undo log中的内容只是串行化的结果，记录了多个事务的过程，不属于多版本共存。但理想的MVCC是难以实现的，当事务仅修改一行记录使用理想的MVCC模式是没有问题的，可以通过比较版本号进行回滚；但当事务影响到多行数据时，理想的MVCC据无能为力了。理想MVCC难以实现的根本原因在于企图通过乐观锁代替二段提交。修改两行数据，但为了保证其一致性，与修改两个分布式系统中的数据并无区别，而二段提交是目前这种场景保证一致性的唯一手段。`二段提交的本质是锁定，乐观锁的本质是消除锁定`，二者矛盾。

20. RC,RR级别下的InnoDB快照读有什么不同？

    > - 在RR级别下的某个事务的对某条记录的第一次快照读会创建一个快照及Read View, 那么之后的快照读使用的都是同一个Read View；
    > - RC级别下的，事务中，每次快照读都会新生成一个快照和Read View, 这就是我们在RC级别下的事务中可以看到别的事务提交的更新的原因；

21. innodb如何实现事务的隔离机制；

    > 1. 加读写锁；
    > 2. 一致性快照读，即mvcc；

22. mysql如何实现事务的幻读问题？***

    >  使用record lock加gap锁；

23. [mysql是如何实现可重复读的 ***](https://juejin.im/post/6844904180440629262)

    > - InnoDB 的行数据有多个版本，每个版本都有 row trx_id。
    > - 事务根据 undo log 和 trx_id 构建出满足当前隔离级别的一致性视图。
    > - 可重复读的核心是一致性读，而事务更新数据的时候，只能使用当前读，如果当前记录的行锁被其他事务占用，就需要进入锁等待。

24. [数据库为什么要用B+树结构--MySQL索引结构的实现](https://blog.csdn.net/bigtree_3721/article/details/73650601)、

    [mysql的索引数的原理解析](https://blog.csdn.net/u013967628/article/details/84305511) ***

    > - 首先索引要满足支持根据某个值快速查找；其次索引要满足根据区间值来查找；
    >   1. 散列表支持O1的根据某个值查找，但是他不支持根据区间范围来查找；
    >   2. 平衡二叉查找树也是支持Ologn的时间复杂度的查找，而且根据中序遍历输出有序序列，但是也不支持按照区间进行查找；红黑树是平衡二叉树的一种，他的h太高，且它的逻辑上很近的节点物理上很远，无法使用局部性原理；
    >   3. 跳表支持，跳表是在链表之上加上多层索引构成的。它支持快速地插入、查找、删除数据，对应的时间复杂度是O(logn)。并且，跳表也支持按照区间快速地查找数据。我们只需要定位到区间起点值对应在链表中的结点，然后从这个结点开始，顺序遍历链表，直到区间终点对应的结点为止，这期间遍历得到的数据就是满足区间值的数据。
    > - 索引其实使用一个和跳表差不多的结构，b+tree，他是根据二叉树演化过来的；
    >   - 把叶子节点串在一条链表上，从小到大排序；我们只需要拿区间的起始值，在树中进行查找，当查找到某个叶子节点之后，我们再顺着链表往后遍历，直到链表中的结点数据值大于区间的终止值为止。所有遍历到的数据，就是符合区间值的所有数据；
    > - 但是数据量比较大的时候索引文件不可能都放到内存里(1亿条数据每个节点占用16字节则需要1GB空间)，只能存储到硬盘上。所以每次查询的时候需要从硬盘上读取节点的数据（索引的结构就要尽量减少磁盘I/O的次数），而树的高度越低，磁盘的io次数越低，此时就需要增大数的度。
    > - 但度也不是越大越好，由于操作系统有页的概念，一次都要读取一页的数据(一页的数据就是一次IO)。根据局部性原理（当一个数据被用到时，其附近的数据也通常会马上被使用）以及磁盘预读，预读的长度一般为页的整数倍，数据库系统巧妙的利用这两个原理。将一个节点的大小设计为一个页的大小；
    > - 而红黑树这种结构，h明显要深的多。由于逻辑上很近的节点（父子）物理上可能很远，无法利用局部性，而B+树每次新建节点时，直接申请一个页的空间，这样就保证一个节点物理上也存储在一个页里；
    >
    > `总结版本`：
    >
    > - 索引即数据，由于索引很大，不可能全部存储在内存中，所以要存储到磁盘上；
    > - 索引的结构要尽可能减少查过过程中的磁盘IO的存取次数；
    > - 根据局部性原理以及磁盘预读，预读的长度一般为页的整数倍；
    > - 数据库系统巧妙的利用磁盘预读原理，将一个节点的大小设置为一个页，这样每个节点只需要一次IO就可以完全载入，像红黑树这种结构，h明显要深很多。由于逻辑上很近的节点物理上就可能很远，无法利用局部性原理；

25. [B+树的缺点](https://blog.csdn.net/dbanote/article/details/8897599)

    > B+树最大的性能问题是会产生大量的随机IO，随着新数据的插入，叶子节点会慢慢分裂，逻辑上连续的叶子节点在物理上往往不连续，甚至分离的很远，但做范围查询时，会产生大量读随机IO。

26. [分布式id生成器](https://tech.meituan.com/2017/04/21/mt-leaf.html)

    [分布式id生成器简介2](https://mp.weixin.qq.com/s/7RQhCazoLJ-qO7CglZ6b2Q) ***

    > 1. snowflake方案：64bit（1bit不用，41bit用表示时间，10bit表示机器，12bit自增序列id），理论上snowflake方案的QPS约为409.6w/s，保证任一机器任一毫秒内生产的id都是不同的；优点是根据时间生成，自增列在低位，递增趋势，不依赖第三方系统；缺点是`强烈依赖机器时钟`；
    > 2. UUID(Universally Unique Identifier)的标准型式包含32个16进制数字，`优点:`是性能非常高，本地生成，没有网络消耗；全球唯一；`缺点:`存储空间大，不易存储；基于mac地址生产，造成mac地址泄漏；无法保证递增趋势；
    > 3. mysql自增主键：存在单点问题；
    > 4. mysql多实例自增主键：每个实例的起始值不同，步长相同；  1+step；2+step；3+step；4+step；`缺点是`:不容易扩容
    > 5. redis生产方案：使用incr原子操作。年月日时分秒+自增id；10万个请求获取id，并发执行完9s左右；`性能一般，占用带宽`
    >

27. mysql的各种日志的（这三种日志是顺序IO）***

    > - Redo log（重做日志）：事务开始之后就产生redo log，redo log的落盘并不是随着事务的提交才写入的，而是在事务的执行过程中，便开始写入redo log文件中。确保事务的持久性。`防止在发生故障的时间点，尚有脏页未写入磁盘，在重启mysql服务的时候，根据redo log进行重做`，从而达到事务的持久性这一特性。
    > - Undo log(回滚日志)：保存了事务发生之前的数据的一个版本，可以用于回滚，同时可以提供多版本并发控制下的读（MVCC），也即非锁定读。
    > - **二进制日志（binlog）**：
    >   1. 用于复制，在主从复制中，从库利用主库上的binlog进行重播，实现主从同步。
    >   2. 用于数据库的基于时间点的还原

28. mysql主从同步流程 ***

    > - master将操作语句记录到binlog日志中，然后授予slave远程连接的权限（master一定要开启binlog二进制日志功能；通常为了数据安全考虑，slave也开启binlog功能）。
    > - slave开启两个线程：IO线程和SQL线程。其中：IO线程负责读取master的binlog内容到中继日志relay log里；SQL线程负责从relay log日志里读出binlog内容，并更新到slave的数据库里。
    > - 日志格式：statement level记录操作语句, row level记录操作涉及的所有行数据, mixed level;

29. [深度分页的优化](https://blog.csdn.net/ydyang1126/article/details/72885246)

    >  核心思想是缩小limit m,n的范围。
    >
    > 1. 翻页的记录下一页的maxId， where>maxId（增加的 ）order by id  asc  limit 0,10（ASC数据需要倒置）；上一页 where<maxId order by id desc limit 0 10；
    >
    > 2. 跳页码：
    >
    >    原理还是一样，**记录住当前页id的最大值和最小值，计算跳转页面和当前页相对偏移**，由于页面相近，这个偏移量不会很大，这样的话m值相对较小，大大减少扫描的行数。

30. [mysql，未分库之后进行分库，具体的方案](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/database-shard-method.md)

    > 1. 场景，数据库的数据量单表达到2000万，db的磁盘不够用，又无法升级磁盘，所以需要使用新机器进行分400张表扩容分表；
    > 2. 首先，将库里的其它表的数据从旧db导入到新db，自己使用定时任务将数据重新rehash到新的400张表中。需要对比某个时间点，新旧两个库里的数据量是否一致。
    > 3. 同时，修改代码，将业务中对这些表的写操作的逻辑进行双写。第一次上线只是保证双写，同时用定时任务检查某个时间点后数据是否一致，如果不一致，则由旧库里插入到新库里。
    > 4. 跑一段时间，稳定后再一次上线使用切换数据源到新库和新的分表；
    > 5. 再过一点时间停止写入旧库；

# 网络

[网络学习指南](https://www.jianshu.com/p/45d27f3e1196)

1. http相关

   > - http是无状态的，就是同一个客户端第二次访问同一个服务器页面时，无法知道客户端曾经访问过；
   > - http1.0是非持久连接，每一个客户端必须为每一个请求创建一个新的连接；http1.1使用了keeplive，所谓持久连接就是服务器在发送响应后的一段时间保持这种链接，供其它请求使用；

2. [一个http请求从浏览器输入url后的过程](https://segmentfault.com/a/1190000006879700)

   > DNS域名解析 (要依次查询浏览器缓存、系统缓存、路由器缓存、dns)--> 发起TCP的三次握手 --> 建立TCP连接后发起http请求 --> 服务器响应http请求，浏览器得到html代码 --> 浏览器解析html代码，并请求html代码中的资源（如js、css、图片等） --> 浏览器对页面进行渲染呈现给用户-->断开tcp连接

3. [HTTP与HTTPS的区别](http://www.mahaixiang.cn/internet/1233.html)

   > - https协议需要申请ca证书，一般免费的较少，所以需要一定费用；
   > - http是超文本传输协议，是明文传输；https则是具有安全性的ssl加密传输协议；
   > - 端口不同 80和443；
   > - http的连接很简单是无状态的，https协议是由ssl+http协议加密传输，身份认证的网络协议比较安全；

4. [TCP和UDP的区别](https://blog.csdn.net/sifanchao/article/details/82285018) 

   [TCP和UDP的区别2](https://blog.csdn.net/Li_Ning_/article/details/52117463)

   [深入理解两者的区别](https://blog.csdn.net/striveb/article/details/84063712) ***

   > - UDP协议和TCP协议都是传输层协议。
   >
   > - 不同点：
   >
   >   1）`是否面向连接`：TCP提供面向连接的传输，通信前要先建立连接（三次握手机制）； UDP提供无连接的传输，通信前不需要建立连接。
   >   2）`是否可靠`： TCP提供全双工的可靠的传输（有序，无差错，不丢失，不重复）； UDP提供不可靠的传输。
   >   3）`传输格式`：  TCP面`向字节流`的传输，因此它能将信息分割成组（报文段），并在接收端将其重组； UDP是面向数据报的传输，没有分组开销。
   >   4） `特殊机制`： TCP提供拥塞控制和流量控制机制； UDP不提供拥塞控制和流量控制机制。
   >
   > - TCP（HTTP、HTTPS、SSH、Telnet、FTP、SMTP）
   >
   > - UDP：（NFS: 网络文件系统、DNS: 域名解析协议、TFTP: 简单文件传输协议、DHCP: 动态主机配置协议、BOOTP: 启动协议(用于无盘设备启动)）

5. UDP

   > - **UDP数据报最大长度64K（包含UDP首部），如果数据长度超过64K就需要在应用层手动分包，UDP无法保证包序，需要在应用层进行编号。**
   > - 无连接：知道对端的IP和端口号就直接进行传输，不需要建立连接。
   > - 不可靠：没有确认机制, 没有重传机制; 如果因为网络故障该段无法发到对方, UDP协议层也不会给应用层返回任何错误信息。
   > - 面向数据报：不能够灵活的控制读写数据的次数和数量，应用层交给UDP多长的报文, UDP原样发送, 既不会拆分, 也不会合并。
   > - 数据收不够灵活，但是能够明确区分两个数据包，避免粘包问题。![img](https://img-blog.csdn.net/20180901091529706?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpZmFuY2hhbw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

6. [网络分层](https://www.cnblogs.com/duanwandao/p/9941411.html)

   > - 七层模型：物理层、数据链路层、网络层、传输层、会话层、表示层、应用层；
   >
   > - 五层模型：物理层、网络接口层、网络层、传输层、应用层
   >
   > - 物理层：网卡，网线，集线器，中继器，调制解调器
   >
   >   数据链路层：网桥，交换机
   >
   >   网络层：路由器

7. [三次握手四次挥手](https://blog.csdn.net/qzcsu/article/details/72861891)  ***

   ![img](https://img-blog.csdn.net/20180901094250499?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpZmFuY2hhbw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

   > `三次握手`：
   >
   > - 第一次握手：建立连接时，客户端A发送SYN包（SYN=j）到服务器B，并进入SYN_SEND状态，等待服务器B确认
   > - 第二次握手：服务器B收到SYN包，必须确认客户A的SYN（ACK=j+1），同时自己也发送一个SYN包（SYN=k），即SYN+ACK包，此时服务器B进入SYN_RECV状态
   > - 第三次握手：客户端A收到服务器B的SYN＋ACK包，向服务器B发送确认包ACK（ACK=k+1），此包发送完毕，完成三次握手。
   >
   > `四次挥手`：由于TCP连接是全双工的，因此每个方向都必须单独进行关闭
   >
   > - 客户端A发送一个FIN，用来关闭客户A到服务器B的数据传送。
   > - 服务器B收到这个FIN，它发回一个ACK，确认序号为收到的序号加1。
   > - 服务器B关闭与客户端A的连接，发送一个FIN给客户端A。
   > - 客户端A发回ACK报文确认，并将确认序号设置为收到序号加1。
   >
   > `TIME_WAIT状态`：
   >
   > - TCP协议规定,主动关闭连接的一方要处于`TIME_ WAIT`状态**,等待两个MSL(最大报文生存周期)**的时间后才能回到CLOSED状态
   > - 服务器方要处理大量的客户端的连接（生命周期短。但是每秒很多客户端请求）；这个时候如果服务端主动关闭连接，则会出现大量的time_wait状态；
   >
   > `三次握手和四次挥手`：在TCP连接中，服务器端的SYN和ACK向客户端发送是一次性发送的，而在断开连接的过程中， B端向A端发送的ACK和FIN是分两次发送的。因为在B端接收到A端的FIN后， B端可能还有数据要传输，所以先发送ACK，等B端处理完自己的事情后就可以发送FIN断开连接了。
   
8. tcp如何保证可靠传输  ***

   >  `序列号、确认机制、超时重传去重、拥塞控制`
   >
   > - 序列号：TCP将每个字节的数据都进行了编号，即为序列号。
   > - 确认应答：每一个ACK都带有对应的确认序列号，意思是告诉发送者，我已经收到了哪些数据；
   > - 超时重传 、去重：主机A发送数据给B之后, 可能因为网络拥堵等原因, 数据无法到达主机B; 如果主机A在一个特定时间间隔内没有收到B发来的确认应答, 就会进行重发；如果收到重复的数据则会去重；
   > - 拥塞控制：
   >
   >     - 滑动窗口：建立连接时，各端分配一个缓冲区用来存储接收的数据，并将缓冲区的尺寸发送给另一端。接收方发送的确认消息中包含了自己剩余的缓冲区尺寸。剩余缓冲区空间的数量叫做窗口。其实就是建立连接的双方互相知道彼此剩余的缓冲区大小；
   >     - 流量控制：接收端处理数据的速度是有限的。 如果发送端发的太快, 导致接收端的缓冲区被打满, 这个时候如果发送端继续发送, 就会造成丢包, 继而引起丢包重传等等一系列连锁反应。
   >     - 延迟应答：如果接收数据的主机立刻返回ACK应答, 这时候返回的窗口可能比较小.
   >       窗口越大, 网络吞吐量就越大, 传输效率就越高. 我们的目标是在保证网络不拥塞的情况下尽量提高传输效率;
   >     - 捎带应答：在延迟应答的基础上, 我们发现, 很多情况下, 客户端服务器在应用层也是 “一发一收” 的；意味着客户端给服务器说了 “How are you”, 服务器也会给客户端回一个 “Fine, thank you”; 那么这个时候ACK就可以搭顺风车, 和服务器回应的 “Fine, thank you” 一起回给客户端。
   >

9. TCP粘包问题 ***

   > `描述`
   >
   > - 首先要明确, 粘包问题中的 “包” , 是指的应用层的数据包；
   > - 在TCP的协议头中, 没有如同UDP一样的 “报文长度” 这样的字段, 但是有一个序号这样的字段；
   > - 站在传输层的角度, TCP是一个一个报文过来的，按照序号排好序放在缓冲区中；
   > - 站在应用层的角度，看到的只是一串连续的字节数据。那么应用程序看到了这么一连串的字节数据， 就不知道从哪个部分开始到哪个部分是一个完整的应用层数据包；
   >
   > `解决`：
   >
   > - 对于定长的包, 保证每次都按固定大小读取即可;
   > - 对于变长的包, 可以在报头的位置, 约定一个包总长度的字段, 从而就知道了包的结束位置;
   > - 对于变长的包, 还可以在包和包之间使用明确的分隔符

10. [tcp/ip协议](https://developer.51cto.com/art/201906/597961.htm) 

11. session和cookie的区别

    > session和cookie主要是为了解决http请求无状态的问题；
    >
    > - Cookie可以让服务端跟踪每个客户端的访问，但是每次客户端的访问都必须传回这些Cookie，如果Cookie很多，则无形的增加了客户端与服务端的数据传输量，
    > - 而Session则很好地解决了这个问题，同一个客户端每次和服务端交互时，将数据存储通过Session到服务端，不需要每次都传回所有的Cookie值，而是传回一个ID，每个客户端第一次访问服务器生成的唯一的ID，客户端只要传回这个ID就行了，这个ID通常为NAME为JSESSIONID的一个Cookie。这样服务端就可以通过这个ID，来将存储到服务端的KV值取出了。

12. 长连接和短连接

    > HTTP的长连接和短连接本质上是TCP长连接和短连接。HTTP属于应用层协议，在传输层使用TCP协议，在网络层使用IP协议。 IP协议主要解决网络路由和寻址问题，TCP协议主要解决如何在IP层之上可靠地传递数据包，使得网络上接收端收到发送端所发出的所有包，并且顺序与发送顺序一致。
    >

# 数据结构

1. 数组

   > 线性结构、连续内存、O(1)查找、增删需要移动元素O(n)。扩容需要新建一个数组copy旧元素；

2. 链表

   > 线性结构、不是连续内存。通过指针相连；增删比较快O(1)，查找指定元素比较慢 O(n)。

3. 树

   > - 满二叉树：一个深度为n的树，有2的n次方-1个节点的都是满二叉树；
   >
   > - 完全二叉树：完全二叉树是由满二叉树引出来的。一个深度为h的树，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第h 层所有的结点都连续集中在最左边，这就是完全二叉树。
   >
   > - 二叉查找树：也叫二叉排序树、二叉搜索树；左子节点小于父亲节点、右子节点大于父亲节点；
   >
   > - 平衡二叉树：为了解决二叉查找树退化成线性结构，平衡二叉树规定，任意节点的左右子树的深度之差的绝对值不能超过1；
   >
   > - [红黑树](https://www.jianshu.com/p/30afcf59828f)：红黑树是一种平衡二叉树。
   >
   >      `特点`：
   >
   >   - 节点是红色或者黑色；
   >   - 根节点一定是黑色；
   >   - 每个叶子节点是黑色的；
   >   - 一个红色节点，他的儿子节点一定是黑色的；
   >   - 每个节点，从该节点到其子孙节点所有路径上包含的相同数目的黑节点；
   >
   >    `插入操作`：
   >
   >   - 由于红黑树是基于二叉排序树的，所以插入也是基于二叉排序树的，即：从根节点开始，遇键值较大者就向左，遇键值较小者就向右，一直到末端，就是插入点；
   >
   >   `修正`：
   >
   >   - 插入节点的父节点和其叔叔节点（祖父节点的另一个子节点）均为红色的；
   >
   >     将当前节点的父节点和叔叔节点涂黑，将祖父节点涂红，再将当前节点指向其祖父节点，再次从新的当前节点开始算法；
   >
   >   - 插入节点的父节点是红色，叔叔节点是黑色，且插入节点是其父节点的左子节点；
   >
   >     操作是把父节点变黑，祖父节点变红，再对祖父节点右旋
   >
   >   - 插入节点的父节点是红色，叔叔节点是黑色，且插入节点是其父节点的右子节点。

4. [b+树](https://ivanzz1001.github.io/records/post/data-structure/2018/06/16/ds-bplustree)  

   > `简介`：b+树是一颗多路查找树，N叉排序树；主要用于数据库和操作系统中，NTFS、ReiserFS、NSS、XFS、JFS、ReFS和BFS等文件系统都在使用`B+树`作为元数据索引。
   >
   > `定义`：
   >
   > 		-  关键字的个数比孩子节点个数少1；
   > 		-  B+树包括内部节点和叶子节点；
   > 		-  B+树和B树的最大不同是内部节点不保存数据，只保存索引，所有数据都保存在叶子节点中；
   > 		-  m阶B+树表示了内部结点最多有m-1个关键字（或者说内部结点最多有m个子树），阶数m同时限制了叶子结点最多存储m-1个记录；
   > 		-  内部结点中的key都按照从小到大的顺序排列；
   > 		-  叶子结点本身依关键字的大小自小而大顺序链接，有指向相邻节点的指针；
   >
   > `特性`：
   >
   > - 所有关键字都出现在叶子节点的链表中，且链表的关键字是有序的；
   > - 不可能是在非叶子节点中命中；
   > - 非叶子节点相当于稀疏索引；
   > - 更适合文件索引系统；

   

# sharding-jdbc

[sharding-jdbc结合mybatis实现分库分表功能](https://www.cnblogs.com/zwt1990/p/6762135.html)

[解读分库分表中间件Sharding-JDBC](https://www.cnblogs.com/duanxz/p/3467106.html)

 1. 分库分表

    >  - 单纯的分表虽然可以解决数据量过大导致检索变慢的问题，但无法解决过多并发请求访问同一个库，导致数据库响应变慢的问题。所以通常水平拆分都至少要采用分库的方式，用于一并解决大数据量和高并发的问题。这也是部分开源的分片数据库中间件只支持分库的原因。
    >
    > - sharding-jdbc
    >
    >  `优点`：
    >
    >  1. 支持各种orm框架：jpa、hibernate、mybatis等
    >  2. 支持各种数据库连接池，dbcp、c3p0、druid等；
    >  3. 轻量级的框架，客户端直连数据库，jar包形式、无代理；
    >  4. 分片策略灵活，支持等号、between、in；
    >  5. 支持聚合、分组、排序、limit、or等操作；
    >
    >  `缺点`：
    >
    >  1. 不支持union以及部分子查询；
    >
    >  `sql改写`：sql在分片环境中执行某些操作是不正确的
    >
    >  - 计算avg计算，以avg1 +avg2+avg3/3计算平均值并不正确，需要改写为（sum1+sum2+sum3）/（count1+count2+ count3）
    >  - 分页，取前10条。每个分片取10条，然后综合再取10条；

# redis 

- 内存淘汰机制

  > 1. volatile-lru：设置过期时间中取最近最少使用的；
  > 2. volatile-ttl：设置过期时间中取将要淘汰的；
  > 3. volatile-random：设置过期时间中任意选取淘汰；
  > 4. allkeys-lru：从全部的key中取最近做少使用；
  > 5. allkeys-random：从全部的key中随机选取；
  > 6. no-enviction：不淘汰数据，抛出异常；

- [redis的LRU过期策略的具体实现](https://zhuanlan.zhihu.com/p/34133067)

  > - 如果使用linkHashMap的实现策略，需要存储额外的next+prev指针，牺牲比较大的内存空间，redis使用的是一个近似的算法，就是随机取出若干个key，然后按照访问时间排序后，淘汰掉最不经常使用的。

- redis的雪崩如何如何解决

  > `事前`：事前采用主从+sentinel哨兵模式，保证failover，避免全盘崩溃；
  >
  > `事中`：使用限流降级措施，使用本地缓存+hystrix限流降级；
  >
  > `事后`：redis持久化，重启服务，从硬盘上回复缓存数据；

- redis的缓存穿透如何解决

  > - 如果缓存的数据是不会更新的，则可以设置为永不失效；
  >
  > - 可以使用定时任务将数据库中的数据放入缓存；
  >
  > - 回种空值，设置较短的超时时间；
  >
  > - 使用布隆过滤器：
  >
  >   ```tiki wiki
  >   加入要判断用户是否存在缓存中
  >   1. 初始化一个20亿的数组； 20亿/8/1024/2014 = 238M
  >   2. 把所有的数据库的用户id进行取hash然后对20亿取余，计算出在布隆过滤器的位置，设置为1；新增的数据插入数据库中，同时布隆过滤对应的位置也设置为1；
  >   3. 然后获取数据前，先从布隆过滤器中判断是否存在；
  >   
  >   存在hash碰撞，所以会误判。不存在的却被布隆过滤器判断为存在；可以使用多个hash值解决
  >   不支持删除。不止使用0和1 也是用2，但会浪费空间
  >   ```
  >
  
- [过期键的删除策略]([http://www.chenwj.cn/2018-01-10/redis%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0-%E6%95%B0%E6%8D%AE%E5%BA%93/](http://www.chenwj.cn/2018-01-10/redis设计与实现-数据库/))

  > 1. 定时删除:
  >
  >    `定义`：在设置键的过期时间的同时,创建一个定时器,让定时器在过期时间来临时，立即执行删除操作
  >
  >    `优缺点`：定时删除是对内存最友好的，但是是对cpu不友好的。当某一个时刻,有大量的过期键需要删除的时候会占用大量的cpu时间，而此时如果有查询操作，影响查询效率；
  >
  > 2. 惰性删除:
  >
  >    `定义`：放任过期键不管，但每次从键空间获取键时都要检查键是否过期,如果过期则删除;如果没有过期则返回；
  >
  >    `优缺点`：惰性删除对cpu是友好的，只有到了键非删除不可的时候才会被删除.但是对内存是不友好的，相当于内存泄漏；
  >
  > 3. 定期删除:
  >
  >    `定义`： 每个一段时间,程序就会进行一次减产,删除里面的过期键.至于删除多少过期键,怎么检查,由算法决定；
  >
  >    `优缺点`：定期删除是上述两种方式的折中策略，定时删除会每隔一段时间执行一次删除过期键的操作，并通过控制执行的时长和频率来减少对cpu时间的影响，除此之外定期删除还有效的减少了内存浪费。定期删除策略的难点时确定删除操作的执行时间和频率；
  >
  > 4. redis采用`定期删除`和`惰性删除`。

- 持久化（RDB AOF）

  > 1. RDB
  >
  >    - RDB文件是一个经过压缩的二进制文件；
  >
  >    - bgsave命令会派生出一个子进程来创建RDB文件。这期间父进程会处理命令请求。子进程创建完成之后，会通知父进程.。
  >
  >    - dirty属性记录距离上一次成功执行save和bgsave命令之后，修改操作数；
  >    
  >    - lastsave属性时一个时间戳记录距离上一次成功执行save和bgsave命令的时间；
  >    
  >      ```java
  >      save 900 1
  >      save 300 10
  > save 60 10000
  >       格式：
  >  redis-dbversion-selectDB-0-paris-selctDB-3-paris-EOF-check_sum
  >      ```
  >
  > 2. AOF
  >
  >    -  当AOF持久化功能打开时，服务器执行完一个写命令，会以协议格式将被执行的写命令追加到服务器状态的aof_buf缓冲区的末尾；
  >
  >    ```
  >    
  >    appendonly yes
  >    
  >    always 将aof_buf缓冲区中的所有内容写入并同步到aof文件
  >    evrysec
  >    no
  >    ```
  >
  >    - `aof重写`： AOF文件存储的是客户端的写命令，随着服务器运行时间越长,文件会越大，系统通过重写来新建一个AOF文件替换旧的文件，新旧的数据是一直的，去除了冗余数据，体积小很多。
  >
  >      通常使用子进程来执行BGREWRITEAOF进行重写；.

- [redis数据结构的编码]([http://www.chenwj.cn/2018-01-08/redis%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E5%AF%B9%E8%B1%A13/](http://www.chenwj.cn/2018-01-08/redis设计与实现-数据结构与对象3/))

  > 1. string
  >
  >    - embstr 编码的字符串：`长度小于等于 39 字节`。它是一种优化版本，将创建字符串对象所需的内存分配次数从 raw 编码的两次降低为一次，它是`只读的`，不可逆的
  >
  >    - raw：简单动态字符串。o(1)获取字符串长度；api安全，不会造成缓冲区溢出（会提前检查是否超出内存）；二进制安全；`如果长度大于39，则使用简单动态字符串存储`
  >    - Int 编码是long 类型的整数：`存储整数`
  >
  > 2. list
  >
  >    - ziplist：`字符串长度小于64byte且数量小于512`
  >
  >      1. 压缩列表是 Redis 为了节约内存而开发的， 由一系列特殊编码的连续内存块组成的顺序型结构
  >
  >      2. 组成：**zlbytes**（占用字节数）、**zltail**（到表尾还有多少距离）、**zllen**（包含节点数）、**entryX**（节点）**zlend**（节点）
  >
  >    - linkedList：双向链表
  >
  > 3. hash
  >
  >    - ziplist`键、值的字符串长度小于64byte且数量小于512`
  >    - hashtable
  >
  >    - rehash扩容
  >    - 渐进式rehash步骤   loadFactor 1，5(执行bgsave操作期间)；
  >      1. 为 ht[1] 分配空间， 让字典同时持有 ht[0] 和 ht[1] 两个哈希表。
  >      2. 在字典中维持一个索引计数器变量 rehashidx ， 并将它的值设置为 `0` ， 表示 rehash 工作正式开始。**rehashidx代表当前rehash的索引**；
  >      3. 在 rehash 进行期间， 每次对字典执行添加、删除、查找或者更新操作时， `程序除了执行指定的操作以外， 还会顺带将 ht[0] 哈希表在 rehashidx 索引上的所有键值对 rehash 到 ht[1]` ， 当 rehash 工作完成之后， 程序将 rehashidx 属性的值增一。
  >      4. 随着字典操作的不断执行， 最终在某个时间点上， ht[0] 的所有键值对都会被 rehash 至 ht[1] ， 这时程序将 rehashidx 属性的值设为`-1` ， 表示 rehash 操作已完成。
  >
  > 4. set
  >
  >    - inset `集合元素都是整数，且个数不超过512`
  >    - hashtable
  >
  > 5. zset
  >
  >    - ziplist `每个元素的大小小于64byte且元素个数小于128个`
  >    - skiplist：
  >
  >    - 跳跃表
  >      1. 跳跃表（skiplist）是一种有序数据结构， 它通过在每个节点中维持多个指向其他节点的指针， 从而达到快速访问节点的目的；
  >      2. 跳跃表支持平均 O(log N) 最坏 O(N) 复杂度的节点查；
  >      3. zset 结构中的 zsl 跳跃表按分值从小到大保存了所有集合元素， 每个跳跃表节点都保存了一个集合元素： 跳跃表节点的 object 属性保存了元素的成员， 而跳跃表节点的 score 属性则保存了元素的分值。 通过这个跳跃表， **程序可以对有序集合进行范围型操作**， 比如 ZRANK 、 ZRANGE 等命令就是基于跳跃表 API 来实现的。
  >      4. zset 结构中的 dict 字典为有序集合创建了一个从成员到分值的映射， 字典中的每个键值对都保存了一个集合元素： 字典的键保存了元素的成员， 而字典的值则保存了元素的分值。 通过这个字典， **程序可以用 O(1) 复杂度查找给定成员的分值， ZSCORE 命令就是根据这一特性实现的**



- zset的数据机构 ***

  > 当满足以下条件时使用ziplist，否则使用skipList
  >
  > - 元素的长度小于64字节；
  > - 有序集合的数量不超过128个；
  >
  > 1. ziplist的结构就是压缩的结构，包括头部节点和元素部分；
  >
  > 2. skiplist结构是dict结构和zskiplist结构
  >
  >    - dict保存key为元素，value为分值；//获取成员分值
  >
  >    - zskiplist保存有序的元素列表，每个元素包括元素和分值；// 支持o(logn)复杂度内根据分值定位
  >
  >    - ```c
  >      /*
  >       * 跳跃表
  >       */
  >      typedef struct zskiplist {
  >      
  >          // 表头节点和表尾节点
  >          struct zskiplistNode *header, *tail;
  >      
  >          // 表中节点的数量
  >          unsigned long length;
  >      
  >          // 表中层数最大的节点的层数
  >          int level;
  >      
  >      } zskiplist;
  >      
  >      /*
  >       * 跳跃表节点
  >       */
  >      typedef struct zskiplistNode {
  >      
  >          // 成员对象
  >          robj *obj;
  >      
  >          // 分值
  >          double score;
  >      
  >          // 后退指针
  >          struct zskiplistNode *backward;
  >      
  >          // 层
  >          struct zskiplistLevel {
  >      
  >              // 前进指针
  >              struct zskiplistNode *forward;
  >      
  >              // 跨度
  >              unsigned int span;
  >      
  >          } level[];
  >      
  >      } zskiplistNode;
  >      ```
  >
  >      https://zhuanlan.zhihu.com/p/26499803
  >    
  > 3. 查询逻辑
  >
  >    - zscore的查询，不是由skiplist来提供的，而是由那个dict来提供的。
  >
  >    - `根据排名的查找，时间复杂度也为O(log n)。` 为了支持排名(rank)，   Redis里对skiplist做了扩展，使得根据排名能够快速查到数据，或者根据分数查到数据之后，也同时很容易获得排名。而且，根据排名的查找，时间复杂度也为O(log n)。
  >
  >    - zrevrange的查询，是根据排名查数据，由扩展后的skiplist来提供。
  >
  >    - zrevrank是先在dict中由数据查到分数，再拿分数到skiplist中去查找，查到后也同时获得了排名。
  >
  > 4. 时间复杂度
  >
  >    - zscore只用查询一个dict，所以时间复杂度为O(1)
  >    - zrevrank, zrevrange, zrevrangebyscore由于要查询skiplist，所以zrevrank的时间复杂度为O(log n)，而zrevrange, zrevrangebyscore的时间复杂度为O(log(n)+M)，其中M是当前查询返回的元素个数。

- [为什么会用跳表来实现zset而不用红黑树]([https://syt-honey.github.io/2019/03/23/17-%E8%B7%B3%E8%A1%A8%EF%BC%9A%E4%B8%BA%E4%BB%80%E4%B9%88Redis%E4%B8%80%E5%AE%9A%E8%A6%81%E7%94%A8%E8%B7%B3%E8%A1%A8%E6%9D%A5%E5%AE%9E%E7%8E%B0%E6%9C%89%E5%BA%8F%E9%9B%86%E5%90%88%EF%BC%9F/](https://syt-honey.github.io/2019/03/23/17-跳表：为什么Redis一定要用跳表来实现有序集合))

  > - 其中，插入、删除、查找以及迭代输出有序序列，红黑树也可以完成，时间复杂度和跳表一样。`但是按照区间来查找数据这个操作，红黑树的效率没有跳表高`。
  >
  > - 对于按照区间查找数据这个操作，`跳表可以做到O(logn)的时间复杂度定位区间的起点，然后在原始链表中顺序往后遍历就可以了`。
  > - redis之所以用跳表来实现有序集合还有其它的原因。比如，`相比于红黑树，跳表的代码看起来更易于理解、可读性更好也不容易出错`。而且跳表也更加的灵活，他可以通过改变索引构建策略，有效平衡执行效率和内存消耗。
  > - 不过，跳表也不能完全代替红黑树。红黑树比跳表出现的更早一些，很多编程语言中的Map类型都是基于红黑树实现的，当我们做业务开发的时候直接拿来用就好了，但是对于跳表我们就需要手动实现了。

- [redis分布式锁](https://blog.csdn.net/yb223731/article/details/90349502)

  > redis分布式锁满足的条件：
  >
  > 1. 互斥性。在任意时刻，只有一个客户端能持有锁。
  > 2. 不会发生死锁。即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。
  > 3. 具有容错性。只要大部分的Redis节点正常运行，客户端就可以加锁和解锁。
  > 4. 解铃还须系铃人。加锁和解锁必须是同一个客户端，客户端自己不能把别人加的锁给解了。
  > 5. 如何保证锁的时间大于业务时间；
  > 6. 等待锁的时间不能超过某个时间，即超时失败；
  >
  > 可重入锁锁：使用另一个hash存储重入锁的加锁次数： key/count
  >
  > 加锁：try   set(lockKey,requestId,nx,expire,second)业务处理的最大时间，
  >
  > 解锁：  lua脚本，
  >
  > ```lua
  > if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end"
  > ```

- [redis集群环境下的redLock]([https://www.xilidou.com/2017/10/23/Redis%E5%AE%9E%E7%8E%B0%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81/](https://www.xilidou.com/2017/10/23/Redis实现分布式锁/))

  > 假设我们有N个Redis节点，N应该是一个大于2的奇数。RedLock的实现步骤:
  >
  > 1. 取得当前时间
  > 2. 使用上文提到的方法依次获取N个节点的Redis锁。
  > 3. 如果获取到的锁的数量大于 （N/2+1）个,且获取的时间小于锁的有效时间(lock validity time)就认为获取到了一个有效的锁。锁自动释放时间就是最初的锁释放时间减去之前获取锁所消耗的时间。
  > 4. 如果获取锁的数量小于 （N/2+1），或者在锁的有效时间(lock validity time)内没有获取到足够的说，就认为获取锁失败。这个时候需要向所有节点发送释放锁的消息。
  
- [redis和zookeeper的分布式锁的区别](https://cloud.tencent.com/developer/article/1476050)  ***

  > - redis获取锁的方式简单粗暴，获取不到锁需要不断尝试获取锁，比较消耗性能；
  > - 假如redis使用主从模式，发生主从切换，锁容易丢失；
  > - zk天生是分布式协调器，强一致性，模型健壮，简单易用；
  > - 如果获取不到锁直接加一个监听器就可以了，不用一直轮训；
  > - zk的缺点：如果有较多的客户端频繁的申请加锁、释放锁，对于 ZK 集群的压力会比较大。
  > - 公司已有redis集群，所以选择了redis做缓存；

- [单线程支持高并发](https://www.cnblogs.com/javazhiyin/p/10823768.html)

  > - redis是纯内存操作；
  > - redis使用的是非阻塞的io多路复用机制；
  > - 单线程避免了多线程频繁的上下文切换问题；

- redis的应用场景

  > 热点数据的缓存；
  >
  > 限时业务，expire，比如手机验证码；
  >
  > 计数器，incr；
  >
  > sorted set 做排行榜；
  >
  > 分布式锁；
  >
  > 发布、订阅；
  >
  > 存储好友关系，set，取交集共同好友； 
  >
  > setbit bicount  getbit 实现布隆顾虑器；

- [如何保障mysql和redis之间的数据一致](https://zhuanlan.zhihu.com/p/106101091?utm_source=wechat_session)  ***

  > - 首先说明，线上使用的是设置的key有超时时间，极端情况是缓存有效时间内是不一致；
  > - 其次，我们采用先更新库，然后删除缓存；
  > - 最后我们会使用canal监控数据库的binLog日志，订阅table的数据变化，如果发生变化发送mq，消费mq删除对应的缓存数据；
  
- 实现秒杀

  > - 场景：
  >
  > 秒杀前：用户不断刷新商品详情页，页面请求达到瞬时峰值。
  >
  > 秒杀开始：用户点击秒杀按钮，下单请求达到瞬时峰值。
  >
  > 秒杀后：一部分成功下单的用户不断刷新订单或者产生退单操作，大部分用户继续刷新商品详情页等待退单机会。
  >
  > - 面临的问题：①并发太高导致程序阻塞；②库存无法有效控制导致出现超卖的情况；
  >
  > - 基本解决方案：① 数据尽量缓存，阻断和数据库的交互；② 通过锁来控制超卖的情况；
  >
  > - 详细实现：
  >   - 提前预热数据，放入Redis
  >   - 商品列表放入Redis List
  >   - 商品的详情数据 Redis hash保存，设置过期时间
  >   - 用户秒杀存数据Redis sorted set保存，
  >   - 订单产生扣库存通过Redis制造分布式锁，库存同步扣除
  >   - 订单产生后发货的数据，产生Redis list，通过消息队列处理
  >   - 秒杀结束后，再把Redis数据和数据库进行同步

- redis如何实现高可用 

  > 1. 主从复制：数据备份、读写分离、分布式集群；
  >
  >    - 主从复制的全量同步，从库启动后，会向主库发送sync命令；
  >    - 主库会在后台生产rdb文件后发送给从库；从库丢弃旧数据进行载入；
  >    - 主库发送rdb文件后将写命令同时放入缓冲区中；
  >    - 从服务器载入完毕后同时接收来自主服务器的缓冲区的命令；
  >    - 新版复制采用PSYNC命令，具有完整重同步和部分重同步2种操作。主从服务器都会维护一个复制偏移量。
  >    - slaveof 10.211.55.9 6379
  >
  > 2. 哨兵模式：
  >
  >    - 使用sentinal哨兵集群管理多个redis；
  >    - 监控(Monitoring)：哨兵(sentinel) 会不断地检查你的Master和Slave是否运作正常。
  >    - 提醒(Notification)：当被监控的某个Redis出现问题时, 哨兵(sentinel) 会发出通知
  >    - 自动故障迁移(Automatic failover)：当一个Master不能正常工作时，哨兵(sentinel) 会开始一次自动故障迁移操作，它会将失效Master的其中一个Slave升级为新的Master， 并让失效Master的其他Slave改为复制新的Master；当客户端试图连接失效的Master时，集群也会向客户端返回新Master的地址,使得集群可以使用Master代替失效Master。
  >
  > 3. cluster模式：
  >    - redis cluster在设计的时候，就考虑到了去中心化，去中间件，加入一个节点就可以获取其它节点；
  >    - Redis 集群**没有使用传统的一致性哈希**来分配数据，而是采用另外一种叫做**哈希槽** (hash slot)的方式来分配的。redis cluster 默认分配了 16384个槽位 ；
  >    - 为了提高高可用，加入了主从模式；

- [redis的sentinel进行failover流程](https://www.cnblogs.com/ivictor/p/9755065.html)  ***

  > - 每隔1秒，每个Sentinel节点会向主节点、从节点、其余Sentinel节点发送一条ping命令做一次心跳检测，来确认这些节点当前是否可达。当这些节点超过down-after-milliseconds（default  30s）没有进行有效回复，Sentinel节点就会判定该节点为主观下线。
  >
  > - 如果被判定为主观下线的节点是master主节点，该Sentinel节点会通过sentinel is master-down-by-addr命令向其他Sentinel节点询问对主节点的判断，当超过<quorum>个数，Sentinel节点会判定该节点为客观下线。如果从节点、Sentinel节点被判定为主观下线，并不会进行后续的故障切换操作。
  >
  > - 对Sentinel进行领导者选举，由其来进行后续的故障切换（failover）工作。选举算法基于Raft。（
  >
  >    1.每个在线的Sentinel节点都有资格成为领导者，当它确认主节点主观下线时候，会向其他Sentinel节点发送sentinel is-master-down-by-addr命令，要求将自己设置为领导者。
  >
  >   2. 收到命令的Sentinel节点，如果没有同意过其他Sentinel节点的sentinel is-master-down-by-addr命令，将同意该请求，否则拒绝。
  >
  >     3. 如果该Sentinel节点发现自己的票数已经大于等于max（quorum，num（sentinels）/2+1），那么它将成为领导者。）；
  >
  > - 然后进行新主节点选举：删除下线的、与之前leader断开连接超过down-after-milliseconds*10毫秒的从节点；选择优先级最高、复制偏移量最大、runId最小的从节点；
  >
  > - Sentinel领导者节点对上一步选出来的从节点执行slaveof no one命令让其成为主节点。
  >
  > - 向剩余的从节点发送命令，让它们成为新主节点的从节点，复制规则和parallel-syncs参数有关。
  >
  > - 将原来的主节点更新为从节点，并将其纳入到Sentinel的管理，让其恢复后去复制新的主节点。

- [redis的cluster模式各个节点的通讯方式](https://www.cnblogs.com/leeSmall/p/8414687.html)

  > - 通讯方式：节点之间采用Gossip协议进行通信，Gossip协议就是指节点彼此之间不断通信交换信息；
  >
  >   - meet消息：用于通知新节点加入，消息发送者通知接收者加入到当前集群，meet消息通信完后，接收节点会加入到集群中，并进行周期性ping pong交换
  >
  >   - ping消息：集群内交换最频繁的消息，集群内每个节点每秒向其它节点发ping消息，用于检测节点是在在线和状态信息，ping消息发送封装自身节点和其他节点的状态数据；
  >
  >   - pong消息，当接收到ping meet消息时，作为响应消息返回给发送方，用来确认正常通信，pong消息也封闭了自身状态数据；
  >
  >   - fail消息：当节点判定集群内的另一节点下线时，会向集群内广播一个fail消息，

- [Redis cluster 集群选举](https://blog.csdn.net/yuliang_liu/article/details/102544424) ***

- https://www.jianshu.com/p/87e06d81b597

  >  **1. 判断节点宕机**
  >
  > 　　- 如果一个节点认为另外一个节点宕机，那么就是pfail，主观宕机
  > 　　-  如果多个节点都认为另外一个节点宕机了，那么就是fail，客观宕机，跟哨兵的原理几乎一样，sdown，odown
  >
  > - 在cluster-node-timeout内，某个节点一直没有返回pong，那么就被认为pfail；
  >
  > 　- 如果一个节点认为某个节点pfail了，那么会在gossip ping消息中，ping给其他节点，如果超过半数的节点都认为pfail了，那么就会变成fail；
  >
  > **2. 从节点过滤**
  >
  > - 对宕机的master node，从其所有的slave node中，选择一个切换成master node
  > - 检查每个slave node与master node断开连接的时间，如果超过了cluster-node-timeout * cluster-slave-validity-factor，那么就没有资格切换成master
  >
  > **3. 从节点选举**
  >
  > - 哨兵：对所有从节点进行排序，slave priority，offset，run id
  > - 每个从节点，都根据自己对master复制数据的offset，来设置一个选举时间，offset越大（复制数据越多）的从节点，选举时间越靠前，优先进行选举
  > - 所有的master node开始slave选举投票，给要进行选举的slave进行投票，如果大部分master node（N/2 + 1）都投票给了某个从节点，那么选举通过，那个从节点可以切换成master

  

- [redis 的线程模型](https://www.javazhiyin.com/22943.html) ***

  > - redis内部使用文件事件处理器（file event handler），这个文件事件处理器是单线程的，所以叫做单线程模型。它是使用io多路复用机制同时监听多个socket，根据socket上的事件选择对应的事件处理器。
  > - 文件事件处理器包括：多个socket、io多路复用器、文件事件分配器、文件事件处理器。
  > - 流程：多个socket可能会产生多个操作，每个操作对应的事不同的文件事件。多路复用程序会监听多个socket，将socket产生的事件放入到队列中，事件分配器每次从队列中取出一个事件，把事件交给对应的事件处理器处理。
  > -  ![Redis-single-thread-model](https://github.com/doocs/advanced-java/raw/master/docs/high-concurrency/images/redis-single-thread-model.png)
  > -  
  
- redis的主从复制 ***

  > - slave启动后，会向master发送psync命令；如果是初次复制，则进行全量复制；
  > - master收到命令后，会启动一个后台线程进行bgsave 生成一个RDB文件，同时会接收客户端命令放到缓冲区中；生成后发送给slave节点；
  > - slave收到master发来的rdb文件后，先清除本地数据；然后先写到本地磁盘，然后加载到内存，接着master会将缓冲区中的命令发送给slave；
  > - 如果是断点续传，在master和slave都会记录 replica offset 还有一个master runId， offset是记录在backLog中的；
  > - master 每次接收到写命令之后，先在内部写入数据，然后异步发送给 slave node。
  > - 主从节点互相都会发送 heartbeat 信息。master 默认每隔 10秒 发送一次 heartbeat，slave node 每隔 1秒 发送一个 heartbeat。

- redis的cluster模式 ***

  >  Redis cluster，6 台机器，3 台机器部署了 Redis 主实例，另外 3 台机器部署了 Redis 的从实例，每个主实例挂了一个从实例，3 个节点对外提供读写服务，每个节点的读写高峰qps可能可以达到每秒 1000，3 台机器最多是 10000 读写请求/s。
  >
  > 机器是什么配置？32G 内存+ 8 核 CPU + 1T 磁盘，但是分配给 Redis 进程的是20g内存，一般线上生产环境，Redis 的内存尽量不要超过 20g。
  >
  > 3 台机器对外提供读写，一共有 60g 内存。
  >
  > 因为每个主实例都挂了一个从实例，所以是高可用的，任何一个主实例宕机，都会自动故障迁移，Redis 从实例会自动变成主实例继续提供读写服务。
  >
  > 你往内存里写的是什么数据？每条数据的大小是多少？答题记录数据，每条数据是 10kb。100 条数据是 1mb，10 万条数据是 1g。常驻内存的是 100 万条答题记录数据，占用内存是 10g，仅仅不到总内存的。10万套试卷，20k一套，共计2G。目前高峰期每秒就是 3500 左右的请求量。
  >
  > 其实大型的公司，会有基础架构的 team 负责缓存集群的运维。
  
- redis实现异步队列 ***

  >  使用 list 类型保存数据信息，rpush 生产消息，lpop 消费消息，当 lpop 没有消息时，可以使用 blpop, 在没有信息的时候，会一直阻塞，直到信息的到来。
  >
  > redis 可以通过 pub/sub 主题订阅模式实现 一个生产者，多个消费者，当然也存在一定的缺点，当消费者下线时，生产的消息会丢 失。

- redis实现延时队列 ***

  >  使用sorted set。使用时间戳作为score、使用消息内容作为value、使用zadd生产消息，消费的时候使用zrangebyscore获取n秒前的数据作为轮训进行处理；
  >
  > 

- 本地缓存和分布式缓存的优缺点 ***

  >  本地缓存：指的是在应用中的缓存组件，其最大的优点是应用和cache是在同一个进程内部，`请求缓存非常快速，没有过多的网络开销等`，在单应用不需要集群支持或者集群情况下各节点无需互相通知的场景下使用本地缓存较合适；
  >
  > 同时，它的缺点也是应为`缓存跟应用程序耦合，多个应用程序无法直接的共享缓存`，各应用或集群的各节点都需要维护自己的单独缓存，`对内存是一种浪费`。
  >
  > 
  >
  > 分布式缓存：指的是与应用分离的缓存组件或服务，其最大的优点是自身就是一个独立的应用，与本地应用隔离，多个应用可直接的共享缓存。

- 本地缓存

  > [缓存讲解](https://tech.meituan.com/2017/03/17/cache-about.html)
  >
  > 1. 方法内缓存；
  > 2. 静态变量缓存（本地缓存数据的实时性问题，目前大量使用的是结合ZooKeeper的自动发现机制，实时变更本地静态变量缓存）
  > 3. ehcache缓存，
  >
  > [使用guava来实现本地缓存](https://www.cnblogs.com/rickiyang/p/11074159.html)
  >
  > - 很好的封装了get、put操作，能够集成数据源 ；
  > - 线程安全的缓存，与ConcurrentMap相似，但前者增加了更多的元素失效策略，后者只能显示的移除元素；
  > - Guava Cache提供了三种基本的缓存回收方式：基于容量回收、定时回收和基于引用回收。定时回收有两种：按照写入时间，最早写入的最先回收；按照访问时间，最早访问的最早回收；
  > - 监控缓存加载/命中情况
  >
  > ```
  > package com.chen.util;
  > 
  > import com.google.common.cache.CacheBuilder;
  > import com.google.common.cache.CacheLoader;
  > import com.google.common.cache.LoadingCache;
  > import org.junit.Test;
  > 
  > import java.text.SimpleDateFormat;
  > import java.util.Date;
  > import java.util.Random;
  > import java.util.concurrent.TimeUnit;
  > 
  > /**
  >  * @author :  chen weijie
  >  * @Date: 2020-08-01 11:19
  >  */
  > public class GuavaCacheServie {
  > 
  > 
  >     LoadingCache<Integer, String> cache = CacheBuilder.newBuilder()
  >             //设置并发级别为8，并发级别是指可以同时写缓存的线程数
  >             .concurrencyLevel(8)
  >             //设置缓存容器的初始容量为10
  >             .initialCapacity(10)
  >             //设置缓存最大容量为100，超过100之后就会按照LRU最近虽少使用算法来移除缓存项
  >             .maximumSize(100)
  >             //是否需要统计缓存情况,该操作消耗一定的性能,生产环境应该去除
  >             .recordStats()
  >             //设置写缓存后n秒钟过期
  >             .expireAfterWrite(60, TimeUnit.SECONDS)
  >             //设置读写缓存后n秒钟过期,实际很少用到,类似于expireAfterWrite
  >             //.expireAfterAccess(17, TimeUnit.SECONDS)
  >             //只阻塞当前数据加载线程，其他线程返回旧值
  >             //.refreshAfterWrite(13, TimeUnit.SECONDS)
  >             //设置缓存的移除通知
  >             .removalListener(notification -> {
  >                 System.out.println(notification.getKey() + " " + notification.getValue() + " 被移除,原因:" + notification.getCause());
  >             })
  >             //build方法中可以指定CacheLoader，在缓存不存在时通过CacheLoader的实现自动加载缓存
  >             .build(new DemoCacheLoader());
  > 
  > 
  >     public void setCache() throws InterruptedException {
  > 
  >         //模拟线程并发
  >        Thread a = new Thread(() -> {
  >             //非线程安全的时间格式化工具
  >             SimpleDateFormat simpleDateFormat = new SimpleDateFormat("HH:mm:ss");
  >             try {
  >                 for (int i = 0; i < 10; i++) {
  >                     String value = cache.get(1);
  >                     System.out.println(Thread.currentThread().getName() + " " + simpleDateFormat.format(new Date()) + " " + value);
  >                     TimeUnit.SECONDS.sleep(1);
  >                 }
  >             } catch (Exception ignored) {
  >             }
  >         });
  > 
  >        Thread b = new Thread(() -> {
  >             SimpleDateFormat simpleDateFormat = new SimpleDateFormat("HH:mm:ss");
  >             try {
  >                 for (int i = 0; i < 10; i++) {
  >                     String value = cache.get(1);
  >                     System.out.println(Thread.currentThread().getName() + " " + simpleDateFormat.format(new Date()) + " " + value);
  >                     TimeUnit.SECONDS.sleep(2);
  >                 }
  >             } catch (Exception ignored) {
  >             }
  >         });
  > 
  >        a.start();
  >        b.start();
  >        a.join();
  >        b.join();
  > 
  >         //缓存状态查看
  >         System.out.println(cache.stats().toString());
  > 
  >     }
  > 
  >     /**
  >      * 随机缓存加载,实际使用时应实现业务的缓存加载逻辑,例如从数据库获取数据
  >      */
  >     public static class DemoCacheLoader extends CacheLoader<Integer, String> {
  >         @Override
  >         public String load(Integer key) throws Exception {
  >             System.out.println(Thread.currentThread().getName() + " 加载数据开始");
  >             TimeUnit.SECONDS.sleep(1);
  >             Random random = new Random();
  >             System.out.println(Thread.currentThread().getName() + " 加载数据结束");
  >             return "value:" + random.nextInt(10000);
  >         }
  >     }
  > 
  >     @Test
  >     public void testCase(){
  >         try {
  >             setCache();
  >         } catch (InterruptedException e) {
  >             e.printStackTrace();
  >         }
  >     }
  > 
  > }
  > ```
  >
  > 
  >
  > 
  >
  > [自己实现 本地缓存策略](https://juejin.im/post/6844903939582722056)  ***
  >
  > 使用ConcurrentHashMap 来存储；
  >
  > 使用最近使用的内存淘汰策略；
  >
  > 使用定期删除和定时删除的过期删除策略；
  >
  > ①进程间缓存一致性的保证可以使用，消息队列的消费者订阅消息进行更新；②可以使用zookeeper这种协调器实现监听更新；
  >
  > ```
  > public class Cache implements Comparable<Cache>{
  >     // 键
  >     private Object key;
  >     // 缓存值
  >     private Object value;
  >     // 最后一次访问时间
  >     private long accessTime;
  >     // 创建时间
  >     private long writeTime;
  >     // 存活时间
  >     private long expireTime;
  >     // 命中次数
  >     private Integer hitCount;
  > 
  > ```
  >
  > ```
  > 
  > /**
  >  * 添加缓存
  >  *
  >  * @param key
  >  * @param value
  >  */
  > public void put(K key, V value,long expire) {
  >     checkNotNull(key);
  >     checkNotNull(value);
  >     // 当缓存存在时，更新缓存
  >     if (concurrentHashMap.containsKey(key)){
  >         Cache cache = concurrentHashMap.get(key);
  >         cache.setHitCount(cache.getHitCount()+1);
  >         cache.setWriteTime(System.currentTimeMillis());
  >         cache.setAccessTime(System.currentTimeMillis());
  >         cache.setExpireTime(expire);
  >         cache.setValue(value);
  >         return;
  >     }
  >     // 已经达到最大缓存
  >     if (isFull()) {
  >         Object kickedKey = getKickedKey();
  >         if (kickedKey !=null){
  >             // 移除最少使用的缓存
  >             concurrentHashMap.remove(kickedKey);
  >         }else {
  >             return;
  >         }
  >     }
  >     Cache cache = new Cache();
  >     cache.setKey(key);
  >     cache.setValue(value);
  >     cache.setWriteTime(System.currentTimeMillis());
  >     cache.setAccessTime(System.currentTimeMillis());
  >     cache.setHitCount(1);
  >     cache.setExpireTime(expire);
  >     concurrentHashMap.put(key, cache);
  > }
  > 
  >  /**
  >      * 获取最少使用的缓存
  >      * @return
  >      */
  >     private Object getKickedKey() {
  >         Cache min = Collections.min(concurrentHashMap.values());
  >         return min.getKey();
  >     }
  > 
  > 
  > /**
  >  * 获取缓存
  >  *
  >  * @param key
  >  * @return
  >  */
  > public Object get(K key) {
  >     checkNotNull(key);
  >     if (concurrentHashMap.isEmpty()) return null;
  >     if (!concurrentHashMap.containsKey(key)) return null;
  >     Cache cache = concurrentHashMap.get(key);
  >     if (cache == null) return null;
  >     cache.setHitCount(cache.getHitCount()+1);
  >     cache.setAccessTime(System.currentTimeMillis());
  >     return cache.getValue();
  > }
  > 
  > /**
  >  * 处理过期缓存
  >  */
  > class TimeoutTimerThread implements Runnable {
  >     public void run() {
  >         while (true) {
  >             try {
  >                 TimeUnit.SECONDS.sleep(60);
  >                 expireCache();
  >             } catch (Exception e) {
  >                 e.printStackTrace();
  >             }
  >         }
  >     }
  > 
  >     /**
  >      * 创建多久后，缓存失效
  >      *
  >      * @throws Exception
  >      */
  >     private void expireCache() throws Exception {
  >         System.out.println("检测缓存是否过期缓存");
  >         for (Object key : concurrentHashMap.keySet()) {
  >             Cache cache = concurrentHashMap.get(key);
  >             long timoutTime = TimeUnit.NANOSECONDS.toSeconds(System.nanoTime()
  >                     - cache.getWriteTime());
  >             if (cache.getExpireTime() > timoutTime) {
  >                 continue;
  >             }
  >             System.out.println(" 清除过期缓存 ： " + key);
  >             //清除过期缓存
  >             concurrentHashMap.remove(key);
  >         }
  >     }
  > }
  > 
  > ```

# kafka [入门](http://blog.csdn.net/hmsiwtv/article/details/46960053)

 [经典试题](https://www.jianshu.com/p/eaafb1581e55)

1. kafka的特性 ***

   >- kafka具有近乎实时的消息处理能力,面对海量消息的查询和存储也能高效的处理。Kafka每秒可以生产约25万消息（50 MB），每秒处理55万消息（110 MB）。
   >- kafka支持消息分区，每个分区中的消息可以保证顺序传输，而分区之间的消息可以并发操作，这样提高了kafka的并发能力；
   >- kafka支持在线增加分区，支持在线水平扩展。
   >- kafka支持为多个分区创建多个副本，其中只会有一个leader副本负责读写,其它副本负责与leader进行同步；
   >- 分布式的部署提高了数据的容灾能力;

2. kafka吞吐量大的原因 ***

   > `顺序读写、零拷贝机制、分区、批量发送、数据压缩`
   >
   > - 计算机组成（划重点）里我们学过，硬盘是机械结构，需要指针寻址找到存储数据的位置，所以，如果是随机IO，磁盘会进行频繁的寻址，导致写入速度下降。Kafka使用了顺序IO（`指的是本次 I/O 给出的初始扇区地址和上一次 I/O 的结束扇区地址是完全连续或者相隔不多的。反之，如果相差很大，则算作一次随机 I/O。`）提高了磁盘的写入速度，Kafka会将数据顺序插入到文件末尾，避免了随机读写磁盘导致的性能瓶颈。有测试证明多个分区顺序写磁盘的总效率要比随机写内存还要高。顺序结构的存储对于即使数以TB的消息存储也能够保持长时间的稳定性能。[kafka高性能的读写消息](https://www.jianshu.com/p/650c9878dee7)
   > - “零拷贝(zero-copy)”系统调用机制，就是跳过“用户缓冲区”的拷贝，建立一个磁盘空间到内存的直接映射，数据不再复制到“用户态缓冲区”，省去了一步比较耗时的工作；Memory Mapped Files
   > - kafka中的topic中的内容可以被分为多分partition存在,每个partition又分为多个segment段，所以每次操作都是针对一小部分做操作，很轻便，并且增加并行操作的能力；生产上并发写，消费上多个消费者进消费，提高并发能力；
   > - kafka允许进行批量发送消息，producter发送消息的时候，可以将消息缓存在本地，等到了固定条件发送到kafka；
   > - Kafka还支持对消息集合进行压缩，Producer可以通过GZIP或Snappy格式对消息集合进行压缩压缩的好处就是减少传输的数据量，减轻对网络传输的压力；
   
3. 应用场景

   >- 应用耦合：多应用（服务）间通过消息队列对同一消息进行处理，避免调用接口失败导致整个过程失败；
   >- 限流削峰：应对处理任务某一时刻比较大的场景，避免流量过大导致应用系统挂掉的情况；
   >- 异步处理：多应用对消息队列中同一消息进行处理，应用间并发处理消息，相比串行处理，减少处理时间；
   >- 顺序保证：一般的业务场景中，对消息的顺序的要求还是比较高的，消息队列可以做到；
   >

4. [topic数据量比较多的时候为什么性能急剧下降](http://jm.taobao.org/2016/04/07/kafka-vs-rocketmq-topic-amout/)  ***

   >因为Kafka的每个Topic、每个分区的每个segment都会对应一个物理文件。当Topic数量增加时，消息分散的落盘策略会导致 `磁盘IO竞争激烈` 成为瓶颈。而RocketMQ所有的消息是保存在同一个物理文件中的，Topic和分区数对RocketMQ也只是逻辑概念上的划分，所以Topic数的增加对RocketMQ的性能不会造成太大的影响。

5. [kafka如何保证消息不丢失  ***](https://blog.csdn.net/u010627840/article/details/76435385)

   >1. 在 producer 端设置 `retries=MAX`，失败后无限重试。由于kafka采用至少一次的机制，保证消息不丢失，有可能重复；
   >2. broker端设置ack=all，各个follower都同步完消息才算成功；
   >3. consumer端采用手动提交commit日志的机制，只有自己手动处理成功，才提交commit日志；
   >  - 落库的数据使用唯一索引的方式保证数据不重复；
   >  - 业务处理逻辑中，将唯一键存储在redis中，消费之前判断是否存在，如果存在则不处理；如果不存在则在处理然后放入redis中；
   
7. kafka是如何实现高可用的

   > - 多个broker，由zookeeper负责进行服务治理；
   > - 支持副本机制，topic支持多个partition，每个partition可以在不同的broker上有副本；支持leader选举；假如leader宕机，则可以选举follower担任主节点；
   > - 支持生产者写数据的ack机制；
   > - 消费只会从leader读取，只有所有的follower都被ack了才会被读取到；

8. 大量的消息积压了几个小时还没解决 ***

   > - 先修复consumer的消费问题，确认其没有问题，把所有消费者停了；
   > - 新建一个topic，其partition是原先数量的10倍；
   > - 然后新建一个消费者去分发数据到新建的topic里；
   > - 使用10个消费者来消费，每个消费者消费一个partition中的数据；
   > - 当积压数据消费完之后，恢复原先架构，恢复原先的消费者来消费消息；

9. mq中的消息失效

   > 丢弃旧的数据，重新按照时间点批量写入数据到mq中；

10. mq快写满了

    > 添加分区，添加消费者，加快消费速度；

11. 自己设计一个消息队列

    >- 首先这个 mq 得`支持可伸缩性`吧，就是需要的时候快速扩容，就可以增加吞吐量和容量，那怎么搞？设计个分布式的系统呗，参照一下 kafka 的设计理念，broker -> topic -> partition，每个 partition 放一个机器，就存一部分数据。如果现在资源不够了，简单啊，给 topic 增加 partition，然后做数据迁移，增加机器，不就可以存放更多数据，提供更高的吞吐量了？
    >- 其次你得考虑一下这个 mq 的数据要不要`落地磁盘`吧？那肯定要了，落磁盘才能保证别进程挂了数据就丢了。那落磁盘的时候怎么落啊？顺序写，这样就没有磁盘随机读写的寻址开销，磁盘顺序读写的性能是很高的，这就是 kafka 的思路。
    >- 其次你考虑一下你的 mq 的`可用性`啊？这个事儿，具体参考之前可用性那个环节讲解的 kafka 的高可用保障机制。多副本 -> leader & follower -> broker 挂了重新选举 leader 即可对外服务。
    >- 能不能`支持数据 0`丢失 啊？可以的，参考我们之前说的那个 kafka 数据零丢失方案。
    >- 支持消息的有序应，参考partition；
    
12. [kafka和rocketMq的区别 ***](https://www.cnblogs.com/eryun/p/12088253.html)

    > - 吞吐量：kafka是百万级别的，rocketmq是十万级别的；
    > - 服务治理：kafka使用的是zookeeper做服务发现和治理治理，broker和consumer都会向其注册自身的信息，当有broker或者consumer有宕机的时候会立刻感知（利用zookeeper的监听机制），做相应的调整；rocketMq使用自定义的nameServer做服务发现和治理，实时性差点，比如broker宕机，producer和consumer都不能立刻感知，只有下次更新broker集群的时候才能做调整（轮训机制），但数据不会丢失；
    > - 消息查询和延迟队列：rocketmq支持根据offset查询，还支持自定义的key查询；支持延迟队列，rocketmq针对每个topic都有延迟队列，当消费失败后会将消息存入延迟队列中，每个消费者启动的时候回自动订阅延迟队列；
    > - 消息的落盘机制机制：kafka的每个toic下的每个分区的每个segment对应一个物理文件，分散落盘；roketmq的消息是一个物理文件，每个topic的每个分区只是逻辑上的分区，顺序落盘；
    > - 发送方式：kafka默认使用异步批量发送的形式，有一个memory buffer暂存消息，同时会将多个消息整合成一个数据包发送，这样能提高吞吐量，但对消息的实效有些影响；rocketmq可选择使用同步或者异步发送；
    
12. 消息队列的对比

    > 单机吞吐量：kafka百万级别；rocketMq是十万级别；activeMQ是万级别的；
    >
    > topic对吞吐量影响：kafka从几十上百的时候，吞吐量下降明显；rocketmq上千的时候吞吐量无变化；
    >
    > 功能支持：kafka较为简单；rocketMq功能较为完善，java开发，利于扩展；
    >
    > 时效性：都是ms级别的，而rabbitMQ是微秒级别的（`erlang`）；
    >
    > 架构：kafka是分布式架构，多副本；rocketMq分布式架构；
    >
    > 可靠性：经过参数优化配置，可以做到 0 丢失，都这样；

13. kafka的leader选举 ***

    > 1. `kafka controller选举`：Kafka Controller的选举是依赖Zookeeper来实现的。leader在zk上创建一个临时节点，所有follower对此节点注册监听；当leader宕机后，从isr集合里的所有follower都尝试创建改节点，创建成功者即是leader。
    >
    >    > controller负责增、删topic；
    >    >
    >    > 更新分区副本数量；
    >    >
    >    > 选举分区leader；
    >    >
    >    > 集群broker增加和宕机后的调整；
    >
    > 2. `leader副本的选举`： 基本思路是按照AR集合中副本的顺序查找第一个存活的副本，并且这个副本在ISR集合中。一个分区的AR集合在分配的时候就被指定，并且只要不发生重分配的情况，集合内部副本的顺序是保持不变的，而分区的ISR集合中副本的顺序可能会改变。
    >
    > 3. isr的标准： 与zookeeper保持心跳、与leader的同步进度落后不超过指定数目
    
14. [基于消息队列的分布式事务](https://zhuanlan.zhihu.com/p/101974130)

    [常用的分布式事务解决方案](https://juejin.im/post/5aa3c7736fb9a028bb189bca#heading-15) ***
    
    > - 基于rocketmq（可靠消息服务）的分布式事务
    >
    >   > 1. 在消息队列上开启一个事务主题。
    >   >
    >   > 2. 事务中第一个执行的服务发送一条“半消息”（半消息和普通消息的唯一区别是，在事务提交之前，对于消费者来说，这个消息是不可见的）给消息队列。
    >   >
    >   > 3. 半消息发送成功后，发送半消息的服务就会开始执行本地事务，根据本地事务执行结果来决定事务消息提交或者回滚，RocketMQ提供事务反查来解决异常情况，(如果RocketMQ没有收到提交或者回滚的请求，Broker会定时到生产者上去反查本地事务的状态，然后根据生产者本地事务的状态来处理这个“半消息”是提交还是回滚。值得注意的是我们需要根据自己的业逻辑来实现反查逻辑接口，然后根据返回值Broker自己做提交或者回滚，而且这个反查接口已经做到了无状态的，请求到任意一个生产者节点都会返回正确的数据。)
    >   >
    >   > 4. 本地事务成功后会让这个“半消息”变成正常消息，供分布式事务后面的步骤执行自己的本地事务。（这里的事务消息，producer不会因为consumer消费失败而做回滚，采用事务消息的应用，其所追求的是高可用和最终一致性，消息消费失败的话，MQ自己会负责重推消息，直到消费成功。当然如果你可以根据自己业务来反向操作）。
    >   >
    >   >    ![img](https://pic3.zhimg.com/80/v2-11ea249b164b893fb9c36e86ae32577a_720w.jpg)
    >
    >   Q：如果你当前使用的消息队列不支持“半消息/预发消息”怎么做？
    >
    >   A： 可以使用关系型数据库的一行记录来记录本地事务，使用状态列来表示本地事务执行的结果，通过异步线程不断捞出本地事务执行成功的消息发生到MQ中。
    >
    >   
    >
    >   Q：为什么要增加一个消息预发送机制，增加两次发布出去消息的重试机制，为什么不在业务成功之后，发送失败的话使用一次重试机制？
    >
    >   A：如果业务执行成功，再去发消息，此时如果还没来得及发消息，业务系统就已经宕机了，系统重启后，根本没有记录之前是否发送过消息，这样就会导致业务执行成功，消息最终没发出去的情况。
    
15. [消息队列选型](https://zhuanlan.zhihu.com/p/60196818)



# [thrift简介](https://www.cnblogs.com/chenny7/p/4224720.html)
[springboot整合thrift](https://blog.csdn.net/lupengfei1009/article/details/100934794)

> `简介`： thrift是`跨语言`的服务部署框架，它通过`IDL语言`来定义RPC的接口和数据类型，然后通过`thrift编译器`生成不同语言的代码。并由生产的代码负责RPC`协议层`和`传输层`的实现。 
>
> `TProtocol 协议层`：
>
> - TBinaryProtocol:二进制格式
> - TCompactProtocol:压缩格式，一种更紧凑的二进制格式
> - TJSONProtocol: JSON格式
> - TSimpleJSONProtocol:通过json只写协议，生成的文件很容易通过脚本语言解析；
> - TDebugProtocol:使用易读的可读文本格式，一般debug

> `TTransport 传输层`：定义数据传输格式，可以为TCP/ip传输
>
> - TSocket:阻塞式socket
> - TFramedTransport:以frame为单位进行传输，非阻塞服务中使用
> - TFileTransport: 以文件形式传输
> - TMemoryTransport:将内存用于I/O，java实现时内部实际使用了简单的ByteArrayOutputStream；

> ```
> Thrift支持的服务模型
> ```
>
> - TSimpleServer：简单的单线程的线程模型
> - TThreadPoolServer：多线程服务模型，使用标准的阻塞式IO；
> - TNonblockingServer：多线程服务模型，使用非阻塞式IO（需使用TFramedTransport数据传输方式）
> - THsHaServer，YHsHa引入了线程池去处理（需要使用TFramedTransport数据传输方式），其模型把读写任务放到线程池去处理；

# [nginx反向代理](https://www.jianshu.com/p/bed000e1830b)

> 概念：指以代理服务器来接受internet上的连接请求，然后转发给内部网络上的服务器，并将从服务器上得到的结果返回给一个请求连接的客户端； 优点：
>
> 1. 保护了真实的web服务，对外不可见。外网只能看到反向代理的服务器。
> 2. 节约了有限的IP地址资源，企业内所有的网站共享一个在internet中注册的IP地址，这些服务器分配私有地址，采用虚拟主机的方式对外提供服务。
> 3. 减少WEB服务器压力，提高响应速度；

# zookeeper ***

- [zookeeper简介](https://www.cnblogs.com/wangyayun/p/6811734.html)

  > - 它是一个文件系统，具有监听通知机制；
  > - zookeeper提供了一个多层级的节点命名空间（znode），与文件系统不同的是这些节点可以存储关联的数据。为了保证高吞吐量和低延迟，在内存中维护这些节点的目录结构，但这使得zookeeper不能存放大量的数据，每个节点存储数据的上限为1M。
  > - client端会对某个znode节点建立一个watcher事件，当znode发生变化时，这些client会受到zk的通知，然后client会根据znode变化来做出业务上的改变等；

- 四种类型的节点

  > 1. 持久化目录节点；客户端与 zookeeper 断开连接后，该节点依旧存在
  > 2. 持久化顺序编号的目录节点；
  > 3. 临时目录节点；客户端与 zookeeper 断开连接后，该节点被删除
  > 4. 临时顺序编号目录节点

- zookeeper功能 ***

  > - 命名服务（文件系统）：通过制定的名字来获取资源或者服务地址，通过zk创建一个全局唯一的路径；这个路径就是一个名字，指向集群中的集群，提供服务地址；
  >
  > - 配置管理（文件系统、通知机制）：将程序的配置信息放在zk的znode下。当有配置发生变更的时候也是znode发生变化的时候，可以改变zk中某个目录节点的内容，同时通过watcher通知给各个客户端。
  >
  > - 集群管理（文件系统、通知机制）：集群管理主要有2点（机器的加入和退出，选举master）。
  >
  >   1. 在指定的父目录下创建临时目录节点，然后监听该父目录下的子节点的变化，一旦有节点在该父节点下的临时节点被删除（当客户端与server断开连接，则临时节点就会被删除），其它节点就得到通知。
  >   2. 指定的父节点下所有的临时顺序子节点，每次选取编号最小的机器作为master就好。当master挂了通知其它节点进行选举，编号最小的节点作为master；
  >
  > - 分布式锁
  >
  > - 队列管理
  >
  >   1. 同步队列，当一个队列的成员都聚齐时，这个队列才可用，否则一直等待所有成员到达。
  >
  >   2.  队列按照 FIFO 方式进行入队和出队操作。 
  >
  >      第一类，在约定目录下创建临时目录节点，监听节点数目是否是我们要求的数目。 
  >
  >      第二类，和分布式锁服务中的控制时序场景基本原理一致，入列有编号，出列按编号。在特定的目录下创建 **PERSISTENT_SEQUENTIAL** 节点，创建成功时 **Watcher** 通知等待的队列，队列删除**序列号最小的节点**用以 消费。此场景下 Zookeeper 的 znode 用于消息存储，znode 存储的数据就是消息队列中的消息内容， SEQUENTIAL 序列号就是消息的编号，按序取出即可。由于创建的节点是持久化的，所以**不必担心队列消息的 丢失问题**。

- [zookeeper集群搭建](https://www.cnblogs.com/wuxl360/p/5817489.html)

- zookeeper数据复制

  > 两种方式：
  >
  > - 写主：对数据的修改提交给指定节点，读无限制，读写分离；采用同步复制，可以保证强一致性；
  > - 写任意：对数据的修改提交给任意的节点，节点间进行同步；
  >
  > zookeeper的实现：使用写任意，通过增加机器，提高吞吐量和扩展性。响应能力取决于方式：采用延迟复制来保持最终一致性，还是立即复制快速响应；
  
- zookeeper的工作原理

  >  核心原理是`原子广播`，保证了各个server间同步。这个协议叫做zab协议。Zab 协议有两种模式，它们分别是**恢复模式(选主)**和**广播模式(同步)**。
  >
  > 当服务启动或者在领导者崩溃 后，Zab 就进入了恢复模式，当领导者被选举出来，且大多数 Server 完成了和 leader 的状态同步以后，恢复 模式就结束了。状态同步保证了 leader 和 Server 具有相同的系统状态。

- zookeeper下的server的状态

  > - LOOKING:当前 Server **不知道** **leader** **是谁**，正在搜寻
  > -  LEADING:当前 Server 即为选举出来的 leader 
  > - FOLLOWING:leader 已经选举出来，当前 Server 与之同步

- [zookeeper实现分布式锁](https://www.cnblogs.com/liuyang0/p/6800538.html) ***

  > - 在zookeeper指定节点下（lock）创建临时顺序节点node_n;
  >
  > - 获取lock下所有的子节点children；
  >
  > - 对子节点按照自增序号从小到大排序；
  >
  > - 判断本节点是不是第一个节点，若是，则获取成功；若不是，则监听比该节点小的那个节点删除事件
  >
  > - 若监听事件生效，则回到第二步重新进行判断，知道获取到锁；
  >
  >   `具体实现`：
  >
  >   - 通过实现Watcher接口，实现process(WatchedEvent event)方法来实施监控，使CountDownLatch来完成监控，在等待锁的时候使用CountDownLatch来计数，等到后进行countDown，停止等待，继续运行

- [zookeeper作为注册中心的原理 ](https://www.jianshu.com/p/68a05b5af088) ***

  > - zookeeper就是个分布式文件系统，每当一个服务提供者部署后都要将自己的服务注册到zookeeper的某一路径上: /{service}/{version}/{ip:port}。创建一个Znode节点，存储用户的ip、端口调用方协议；
  >
  > ` RPC服务注册、发现过程`
  >
  > - Provider 启动时，会将服务的名称和ip端口注册到配置中心；
  > - consumer在第一次调用服务时，从配置中心拉取相应服务的ip地址列表，并缓存到本地，供后续通过负载均衡的方式调用；同时消费者会监听对应的路径；
  > - 当provider的某个节点下线时（心跳检测），会从对应的节点下移除；同时注册中心会将新的ip列表发送给服务消费者，并让他们缓存在本地；
  >
  > ` 感知服务上下线`
  >
  > - 使用心跳检测功能，如果长时间没有相应则会被剔除可用列表；
  > - 服务消费者会监听对应的路径，一旦有提供者列表发生变化，从而进行更新；
  > - zookeeper与生俱来的服务容错容灾能力，可以确保服务注册表的高可用；

- [zookeeper实现分布式配置文件](https://www.cnblogs.com/leeSmall/p/9614601.html)

# linux服务器？

1. [linux常用命令](https://blog.52itstyle.com/archives/166/)
2. 如何查看端口是否被占用
3. 查看负载

# es简介

1. [简介]([http://www.readingnotes.site/posts/Elasticsearch-%E7%AE%80%E4%BB%8B.html](http://www.readingnotes.site/posts/Elasticsearch-简介.html))

   > es是一个分布式的搜索和分析引擎，用于全文检索、结构化检索和分析；Elasticsearch 基于 Lucene 开发。

2. 常用概念

   > - node：即一个es的运行实例，使用多播或者单播的方式发现cluster并加入；
   > - cluster：包含一个或者多个拥有相同集群名字的node，其中包括一个master node。
   > - index：类比数据库的db，一个逻辑命名空间；
   > - alias：可以给index添加0个或者多个alias，alias给我们提供了一种切换index的能力，不用修改代码；
   > - type：类比关系数据库中的table，一个index可以配置多个type，但一般配置一个；
   > - mapping：类比关系数据库中的schema的概念，mapping 定义了 index 中的 type。mapping 可以显示的定义，也可以在 document 被索引时自动生成，如果有新的 field，Elasticsearch 会自动推测出 field 的type并加到mapping中。
   > - document：类比关系数据库里的一行记录(record)，document 是 Elasticsearch 里的一个 JSON 对象，包括零个或多个field。
   > - field：类比关系数据库里的field，每个field 都有自己的字段类型。
   > - shard：是一个Lucene 实例。Elasticsearch 基于 Lucene，shard 是一个 Lucene 实例，被 Elasticsearch 自动管理。之前提到，index 是一个逻辑命名空间，shard 是具体的物理概念，建索引、查询等都是具体的shard在工作。shard 包括primary shard 和 replica shard，写数据时，先写到primary shard，然后，同步到replica shard，查询时，primary 和 replica 充当相同的作用。replica shard 可以有多份，也可以没有，replica shard的存在有两个作用，一是容灾，如果primary shard 挂了，数据也不会丢失，集群仍然能正常工作；二是提高性能，因为replica 和 primary shard 都能处理查询。另外，shard数和replica数都可以设置，但是，shard 数只能在建立index 时设置，后期不能更改，但是，replica 数可以随时更改。但是，由于 Elasticsearch 很友好的封装了这部分，在使用Elasticsearch 的过程中，我们一般仅需要关注 index 即可，不需关注shard。
   >
   > shard、node、cluster 在物理上构成了 Elasticsearch 集群，field、type、index 在逻辑上构成一个index的基本概念，在使用 Elasticsearch 过程中，我们一般关注到逻辑概念就好，就像我们在使用MySQL 时，我们一般就关注DB Name、Table和schema即可

3. 基础操作

   > - Index: 写document到es中，如果不存在则创建；如果存在则取代旧的；
   >
   > - create：和index不同的是如果存在则抛出异常；
   >
   > - get：根据id获取文档；
   >
   > - update：在Elasticsearch中，更新document时，是把旧数据取出来，然后改写要更新的部分，删除旧document，创建新document，而不是在原document上做修改。
   > - delete：删除文档，只是先标记删除。等Lucene底层进行merge时才会真正把标记删除的文档删除

4. 查询语句

   > - filter与query：官网推荐，仅在全文检索时使用query，其它都是使用filter。
   >
   > - match_all：取出所有文档；
   >
   > - match：一般全文检索时使用；
   >
   > - term/terms：精确匹配；
   >
   > - range：范围查找
   >
   > - exists/missiing：存在和不存在
   >
   > - bool：使用bool 子句来将各种子查询关联起来，组成布尔表达式，bool 子句可以随意组合、嵌套。
   >
   >   ```json
   >   {
   >       "bool" : {
   >           "must" : {
   >               "term" : { "user" : "kimchy" }
   >           },
   >           "must_not" : {
   >               "range" : {
   >                   "age" : { "from" : 10, "to" : 20 }
   >               }
   >           },
   >           "should" : [
   >               {
   >                   "term" : { "tag" : "wow" }
   >               },
   >               {
   >                   "term" : { "tag" : "elasticsearch" }
   >               }
   >           ],
   >           "minimum_should_match" : 1,
   >           "boost" : 1.0
   >       }
   >   }
   >   ```

5. 使用注意事项

   > - **深度分页问题**：Elasticsearch 作为一个分布式搜索与分析引擎，`深度分页问题会带来严重的问题`，给CPU、内存、IO、网络带来巨大压力，所以，在Elasticsearch 不建议使用深度分页，如果要遍历数据，可以采用 SCROLL的方式;
   > - **排序问题**：根据某field排序时，Elasticsearch 会将这个` field 的所有值给加载到内存`，然后，这部分数据会常驻内存，如果数据量大或排序字段多，就会给系统带来巨大压力;
   > - **terms 问题**： terms 里可以传多个值，但是，`量不能太多`，搜索引擎的基本数据结构是倒排索引，terms 里传多个值，原理上来说是查很多的倒排索引，量大了也会给系统带来很大压力。

6. [倒排索引](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-write-query-search.md)

   > 倒排索引需要先进行分词，然后做单词到文档id数组的映射；
   >
   > - 倒排索引中的所有词项对应一个或多个文档；
   > - 倒排索引中的词项**根据字典顺序升序排列**

7. es写入数据、查询数据的工作原理是什么？

   > - 写入操作
   >   - 客户端选择一个node发送请求，这个node就是cordinating node(协调节点)
   >   - 协调节点根据document取hash路由到对应的node（primary shard）；
   >   - 实际的node上的primary shard处理请求，然后数据同步的replica；
   >   - 协调节点发现primary node和所有的replica都搞定后，就响应给客户端；
   > - 读取数据
   >   - 可以通过 `doc id` 来找一个node查询，coordinate node会根据 `doc id` 进行 hash，判断出来当时把 `doc id` 分配到了哪个 shard 上面去，从那个 shard 去查询。

# [ 微服务相关]([http://www.chenwj.cn/2020-06-02/%E9%AB%98%E5%B9%B6%E5%8F%91%E8%AE%BE%E8%AE%A1%E6%80%9D%E6%83%B3/#more](http://www.chenwj.cn/2020-06-02/高并发设计思想/#more))

1. [限流算法](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/huifer-how-to-limit-current.md)

2. 微服务的优点和缺点 ***

   > 单体应用的优点：
   >
   > - 代码集中管理，排查问题简单，节省了运维成本；
   >
   > 单体应用的问题：
   >
   > - 不会像单体服务一样有大量的代码，业务耦合到一起，研发效率低下。比如代码冲突，依赖冲突，服务启动缓慢；
   > - 上线一个模块的问题影响整个服务，比如内存溢出导致整个服务不可用，一个服务上线所有模块的都上线；
   > - 服务瓶颈，比如数据库连接数最大是8000，瓶颈问题。如果使用微服务可以拆分成多个库，每个服务有单独的库，容易扩展；
   >
   > 微服务的优点：
   >
   > - 拆分后，各个服务之间通过RPC或者REST协议进行调用，各个服务的业务逻辑简单清晰，容易维护；
   > - 拆分后，各个子服务可以选择不同的技术架构，解决依赖问题；
   > - 各个子服务独立部署，快速响应，不需要协调其它模块对本功能的影响；
   > - 独立扩展，那个功能对资源的需求更大，可以进行扩展或者增加配置；
   >
   > 微服务的缺点
   >
   > - 服务调用链查找问题比较复杂，不确定是哪个服务的问题；
   > - 服务间通信问题，比如需要考虑服务的通信的可靠性问题、[超时、限流、降级、容错](https://juejin.im/post/6844903838231576589)；
   > - 服务部署问题，需要知道各个服务的依赖关系、调用链；

3. 服务拆分的原则 ***

   > - 单一服务内部功能高内聚、低耦合。每个服务只完成自己的职责的任务，对于不是自己职责的功能要交给其他模块完成；比如判断用户是否为认证用户的逻辑要放在用户服务中而不能放到内容服务中；
   > - 服务拆分的粒度，先粗略拆分、再逐渐细化。比如黑名单相关的服务要先拆到用户服务中，后期可以再细拆；
   > - 拆分的过程尽量避免日常功能的迭代
   >   - 优先剥离比较独立的边界服务，从非核心服务出发，减少对现有服务的影响。也给团队一个试错的机会；
   >   - 两个服务有依赖关系的时候，需要先拆分被依赖的服务；
   > - 服务接口的定义要具备可扩展性；比如一个微服务的接口有三个参数，一次需求开发中，组内的同学调整为4个参数，调用方没有修改，所以会报错

4. 微服务化带来的问题和解决思路 ***

   > - 引入注册中心，管理接口挂进程调用的服务地址及端口的管理；
   > - 多个服务之间的依赖，需要服务治理体系。需要熔断、限流、降级、超时机制；
   > - 快读定位调用链路的问题，需要引入分布式追踪工具，以及就监控机制。elk
   
5. zookeeper和eureka作为注册中心的区别  ***

   > - cap模型的支持：zookeeper保证的是cap定理中的cp，它的集群模式模式是主从模式，在一个时间点只有一个leader真正的对外提供服务，其它follower负责冗余备份；而eureka保证的事cap中的ap，它的分布式模式是无主模式，他所有节点都是平等的，客户端访问的任一节点都可以对应的提供服务。如果某个节点发送故障停机，其请求会交给其它节点来实现，它很难保证各个节点数据的实时一致性。通过各节点时候实时同步，保证的是最终一致性；
   > - 是否支持数据存储：szookeeper支持数据存储，可以作为配置中心；eureka不支持数据存储；
   > - 客户端的变化监听：zookeeper支持订阅监听来实现，eureka通过轮训的方式来实现；
   > - 集群监控：eureka支持metrics（运维可以手机并报警这些度量信息达到监控），zookeeper不支持；


# vertx

1. [阻塞IO、非阻塞IO、同步IO、异步IO](https://www.jianshu.com/p/2461535c38f3)

   > - 一个io其实分为了两步：发起io请求和实际的io操作；
   > - 阻塞io和非阻塞io的区别在第一步：发起io请求是否被阻塞，如果阻塞直到完成就是传统的阻塞io；
   > - 同步io和异步io的区别在于第二步是否阻塞：如果实际的io读写阻塞请求进程，那么就是同步io；如果不阻塞，而是操作系统帮你完成io操作再返回结果给你就是异步io；

2. [vertx线程实例]([https://colobu.com/2016/03/31/vertx-thread-model/#Vert-x%E7%9A%84%E7%BA%BF%E7%A8%8B%E6%A8%A1%E5%9E%8B](https://colobu.com/2016/03/31/vertx-thread-model/#Vert-x的线程模型))

3. [vertx技术内幕](https://www.sczyh30.com/posts/Vert-x/vertx-advanced-demystifying-thread-model/)

   > - Vert.x 中主要有两种线程：**Event Loop 线程** 和 **Worker 线程**。
   > - 其中，Event Loop 线程结合了 Netty 的 `EventLoop`，用于处理事件。每一个 `EventLoop` 都与唯一的线程相绑定，这个线程就叫 Event Loop 线程
   > - Worker 线程用于执行阻塞任务，这样既可以执行阻塞任务而又不阻塞 Event Loop 线程。
   > - 为了保证线程安全，防止资源争用，Vert.x 保证了某一个 `Handler` 总是被同一个 Event Loop 线程执行，这样不仅可以保证线程安全，而且还可以在底层对锁进行优化提升性能。

# 项目经验类？ 

- fhh自媒体平台
- fhh-service项目
- kafka流平台


- [advanced-java](https://github.com/doocs/advanced-java)

  

# 其它常见试题

 1. 布隆过滤器

> 布隆过滤器是一个位图，主要用于判重。优点是省空间，缺点是存在误判的情况  `10亿数据大概是100M`
>
> set  每次对输入的值进行取hash运算，然后放到对应的位置设置为1。为了解决冲突，可以使用多个hash函数，在对应的bit位设置为1。
>
> get 单个hash函数则直接判断该bit位是否为1，多个的话需要判断多个bit为的值是否为1，
>
> ```
> >    加入要判断用户是否存在缓存中
> 1. 初始化一个20亿的数组； 20亿/8/1024/2014 = 238M
> 2. 把所有的数据库的用户id进行取hash然后对20亿取余，计算出在布隆过滤器的位置，设置为1；新增的数据插入数据库中，同事布隆过滤对应的位置也设置为1；
> 3. 然后获取数据前，先从布隆过滤器中判断是否存在；
> 存在hash碰撞，所以会误判。不存在的却被布隆过滤器判断为存在；可以使用多个hash值解决
> 不支持删除。不止使用0和1 也是用2，但会浪费空间
> ```
>
> 数据的范围是1到10亿。布隆过滤器的做法是，我们仍然使用一个1亿个二进制大小的位图，然后通过哈希函数，对数字进行处理，让它落在这1到1亿范围内。比如我们把哈希函数设计成f(x)=x%n。其中，x表示数字，n表示位图的大小（1亿），也就是，对数字跟位图的大小进行取模求余。
>
> 不过，你肯定会说，哈希函数会存在冲突的问题啊，一亿零一和1两个数字，经过你刚刚那个取模求余的哈希函数处理之后，最后的结果都是1。这样我就无法区分，位图存储的是1还是一亿零一了。
>
> ​    	为了降低这种冲突概率，当然我们可以设计一个复杂点、随机点的哈希函数。除此之外，还有其他方法吗？我们来看布隆过滤器的处理方法。既然一个哈希函数可能会存在冲突，那用多个哈希函数一块儿定位一个数据，是否能降低冲突的概率呢？我来具体解释一下，布隆过滤器是怎么做的。
>
> 我们使用K个哈希函数，对同一个数字进行求哈希值，那会得到K个不同的哈希值，我们分别记作$X_{1}$，$X_{2}$，$X_{3}$，…，$X_{K}$。我们把这K个数字作为位图中的下标，将对应的BitMap[$X_{1}$]，BitMap[$X_{2}$]，BitMap[$X_{3}$]，…，BitMap[$X_{K}$]都设置成true，也就是说，我们用K个二进制位，来表示一个数字的存在。
>
> 当我们要查询某个数字是否存在的时候，我们用同样的K个哈希函数，对这个数字求哈希值，分别得到$Y_{1}$，$Y_{2}$，$Y_{3}$，…，$Y_{K}$。我们看这K个哈希值，对应位图中的数值是否都为true，如果都是true，则说明，这个数字存在，如果有其中任意一个不为true，那就说明这个数字不存在

[2. 分布式事务TCC ](http://ifeve.com/tcc/) ***

> - 所有事务的参与方都需要实现 try confirm cancle接口；
> - 事务发起方向事务协调器发送事务请求，事务协调器调用所有事务参与这的try方法完成资源的预留；
> - 事务协调器如果发现参与者的try方法失败，则调用参与者的cancle方法，cancle方法要幂等，如果失败重试；
> - 事务协调器如果发现所有的try都ok，则所有参与者都执行confirm操作，直接执行不检查资源是否合适；
> - 协调器如果发现所有的confirm都成功了，则分布式事务结束；
> - 协调器发现有confirm失败了，则协调器会重试；如果一直失败，则进行记录，后续做事务补偿机制；

3. [分布式事务-二阶段协议](http://ifeve.com/distribute-transaction-2pc/)

> 第一阶段：
>
> 分布式事务`发起方`调用协调器发起分布式事务；
>
> 协调器向所有事务参与者发起准备请求，参与者收到请求后执行本地事务，但不提交；
>
> 如果都返回ok，则进入第二个阶段。如果失败则发起回滚请求；
>
> 第二阶段
>
> 事务协调器向参与者发起commit请求，如果都成功则事务执行成功；否则执行反向操作

4. [分布式事务-三阶段协议](http://ifeve.com/3pc/)

> 第一阶段
>
> 事务发起方发起事务后，事务协调器会给所有的事务参与者发起canCommit?的请求，参与者收到后根据自己的情况判断是否可以执行提交，如果可以则回执OK，否者返回fail，并不开启本地事务并执行。
>
> 第二阶段
>
> 事务协调器向所有参与者发起准备事务请求，参与者接受到后，开启本地事务并执行，但是不提交。
>
> 第三阶段
>
> 事务协调器向参与者发起commit请求，如果都成功则事务执行成功；否则执行反向操作

5. 二阶段和三阶段协议的区别

> 三阶段与二阶段最大不同在于三阶段协议把二阶段的第一阶段拆分为了两个阶段，其中第一阶段并不锁定资源，而是询问参与者是否可以提交，等所有参与者回复OK后在具体执行第二阶段锁定资源。理论上如果第一阶段返回都OK，则第二阶段和三阶段执行成功的概率就很大，另外如果第一阶段有些参与者返回了fail，由于这时候其他参与者还没有锁定资源，所以不会造成资源的阻塞。







- 什么是CAP定理？
- 说说CAP理论和BASE理论？
- 什么是最终一致性？最终一致性实现方式？
- 什么是一致性Hash？
- 讲讲分布式事务？
- 如何实现分布式锁？
- 如何实现分布式 Session?
- 如何保证消息的一致性?
- 负载均衡的理解？
- 正向代理和反向代理？
- CDN实现原理？
- 怎么提升系统的QPS和吞吐？
- Dubbo的底层实现原理和机制？
- 描述一个服务从发布到被消费的详细过程？
- 分布式系统怎么做服务治理？
- 消息中间件如何解决消息丢失问题？
- Dubbo的服务请求失败怎么处理？

 [raft一致性协议](https://zhuanlan.zhihu.com/p/91288179)

[gossip流言协议](https://zhuanlan.zhihu.com/p/41228196)















# java

设计开发中的功能，遇到的技术难点或者有挑战性的工作；工作中的难点；

java的类加载机制；

java的并发机制；





