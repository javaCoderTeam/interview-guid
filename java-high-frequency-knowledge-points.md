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

    > - 对于synchronized同步代码块，会在代码开始和结束的位置有monitorEnter和minitorExit指令，当执行monitorEnter时，线程就必须获取monitor对象的持有权限（monitor对象存在于每个Java对象的对象头中，获取monitor后，将minitor的进入数设置为1。synchronized 锁便是通过这种方式获取锁的）。
    > - 对于synchronized同步方法，会有ACC_SYNCHRONIZED 标识，表示改方法是同步方法，调用方法前需要获取对象实例的锁

21. Jdk1.6后有的锁优化 ***

    > `锁优化`：引入了偏向锁、轻量级锁、自旋锁10次、自适应自旋锁、锁粗化、锁消除来减少锁操作的开销；
    >
    > `锁状态`  ：无锁状态，偏向锁状态，轻量级锁状态，重量级锁状态
    >
    > `锁优化的意义` ：synchronized是通过对象内部的监视器对象锁monitor来实现的，对象监视器依赖操作系统的mutex lock来实现的。操作系统实现线程切换需要从用户态切换到内核态，成本非常高。依赖于操作系统Mutex Lock所实现的锁我们称之为“重量级锁”
    >
    > - **轻量级**是相对于使用操作系统互斥量来实现的传统锁而言的。但是，首先需要强调一点的是，轻量级锁并不是用来代替重量级锁的，它的本意是在没有多线程竞争的前提下，减少传统的重量级锁使用产生的性能消耗
    >
    > [锁膨胀的流程](https://tech.meituan.com/2018/11/15/java-lock.html)
    >
    > - 无锁状态，cas操作。
    >
    > -  对象的偏向锁会偏向于第一个获取它的线程，会在对象头上的 Mark Word 部分设置偏向的线程 ID。接下来如果没有其他线程获取该对象的锁，它一直处于偏向锁状态。通过检测Mark Word里是否存储着指向当前线程的偏向锁，持有偏向锁的线程会不执行同步操作；
    >
    > - [轻量级锁](https://www.cnblogs.com/paddix/p/5405678.html)是指当锁是偏向锁的时候，被另外的线程所访问，偏向锁就会升级为轻量级锁，其他线程会通过自旋的形式尝试获取锁，不会阻塞，从而提高性能。
    >
    >   > - 在代码进入同步块的时候，如果同步对象锁状态为无锁状态（锁标志位为“01”状态，是否为偏向锁为“0”），虚拟机首先将在当前线程的栈帧中建立一个名为锁记录（Lock Record）的空间，用于存储锁对象目前的Mark Word的拷贝，然后拷贝对象头中的Mark Word复制到锁记录中。
    >   > - 拷贝成功后，虚拟机将使用CAS操作尝试将对象的Mark Word更新为指向Lock Record的指针，并将Lock Record里的owner指针指向对象的Mark Word。
    >   > - 成功了，那么这个线程就拥有了该对象的锁，并且对象Mark Word的锁标志位设置为“00”，表示此对象处于轻量级锁定状态。
    >   > - 失败了，虚拟机首先会检查对象的Mark Word是否指向当前线程的栈帧，如果是就说明当前线程已经拥有了这个对象的锁，那就可以直接进入同步块继续执行，否则说明多个线程竞争锁。
    >   > - 若当前只有一个等待线程，则该线程通过自旋进行等待。但是当自旋超过一定的次数，或者一个线程在持有锁，一个在自旋，又有第三个来访时，轻量级锁升级为重量级锁。
    >
    > - 重量级锁（10）：编译后的代码块中加入moniterEnter和monitorExist指令，使用monitor对象锁来实现同步阻塞；

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

30. cas是什么

    > cas就是comare and swap，就是内存值V，旧值A，要修改的值b，只有当预期值A与内存的值V相等时才执行设置值为b，并且返回成功，否则返回失败；一般是配合volatile关键字使用，才可以保证拿到的变量主内存中的值，修改后可以将值设置到主内存中去；
    >
    > > - **ABA问题**
    > > - **循环时间长开销大**,自旋

31. java内存模型  ***

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

32. Happens-before 关系

    > 如果线程 A 与线程 B 满足 happens-before 关系，则线程 A 执行动作的结果对于线程 B 是可见的
    >
    > - 程序次序法则：一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作
    > - 监视器锁法则：一个unLock操作先行发生于后面对同一个锁的lock操作
    > - Volatile 变量法则：对一个Volatile变量的写操作先行发生于后面对这个变量的读操作
    > - 传递性：如果 A happens-before 于 B，且 B happens-before C，则 A happens-before C。

33. Thread.sleep(0)的作用是什么

    > 由于java使用抢占式调度算法，而sleep操作可以放弃cpu的执行时间，这样可以操作系统重新进行一次操作系统重新分配时间片的操作；  

34. AtomicInteger 底层实现原理是什么？

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

   > `为了使不同 hash 值发生碰撞的概率更小，尽可能促使元素在哈希表中均匀地散列。`
   >
   > - 2的整数倍减1后，换成二进制后，最后一位肯定是1，这样与hashCode与操作，有可能是奇偶，否则只能是偶数，会浪费一般空间而且不能保证散列均匀；
   >
   > capacity 为 2 的整数次幂的话，为偶数，这样 capacity-1 为奇数，奇数的最后一位是 1，这样便保证了 h&(capacity-1) 的最后一位可能为 0，也可能为 1（这取决于h的值），即与后的结果可能为偶数，也可能为奇数，这样便可以保证散列的均匀性；
   >
   > 而如果 capacity 为奇数的话，很明显 capacity-1 为偶数，它的最后一位是 0，这样 h&(capacity-1) 的最后一位肯定为 0，即只能为偶数，这样任何 hash 值都只会被散列到数组的偶数下标位置上，这便浪费了近一半的空间。
   >
   > - 而与操作是使用取余替代取模，提升计算效率；
   >
   >   首先，capacity 为 2的整数次幂的话，长度减一后相当于低位的掩码，计算桶的位置 h&(length-1) 就相当于对 length 取模，提升了计算效率；

3. [hashmap多线程操作导致死循环](https://coolshell.cn/articles/9606.html) 

   [hashmap死循环](https://blog.csdn.net/xuefeng0707/article/details/40797085)

   > hashmap在多线程的情况下进行rehash操作会导致死循环；
   >
   > **例子**
   >
   > 当线程A执行到链表元素a时，得出它的next元素是b，刚好时间片切换到线程B，线程B将a，b两个元素都倒置存入到新的tab中，注意，现在b的next就是a了！正当此时，线程A醒来，在线程A中，a的next是b。。。。
   > 也就是说，对于线程a来说，a的next是b，而b的next是a，完美，环链形成。
   >
   >  
   >
   > 1.7因为采用了头插法，头插法首尾颠倒。
   >
   > 1.8中 一条链表要做数据迁移到新的tab下，那么这个散列地址的算法跟hashmap7已经有了区别，它首先需要经过e.hash & oldCap这行代码来决定链表元素的高低位置，等于是分为了两条链表，低位元素仍然在老的位置，而高位元素却到了当前位置+oldCap的位置，一条链表被拆成了两条。这样做的好处如下：
   > ①省却了再次计算hash值的开销，避免每个key都进行rehash
   > ②采用尾插法，避免了环链的形成
   >
   > 

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
   >    - 因为JDK1.7是用单链表进行的纵向延伸，当采用头插法时会容易出现逆序且环形链表死循环问题。但是在JDK1.8之后是因为加入了红黑树使用尾插法，能够避免出现逆序且链表死循环的问题。采用`复制整个链表而不是操作原链的方式` ??
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

    >  modCount字段是用来记录hashmap内部结构发生变化的次数，主要用于迭代的快速失败。例如put新键值对，如果某个key对应的value被覆盖不属于结构变化。遍历过程中改变hashMap的内部结构则会出现ConcurrentModificationException。

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

14. concurrentHashMap ***

    [concurrentHashMap](https://www.ibm.com/developerworks/cn/java/java-lo-concurrenthashmap/index.html)

    [concurrentHashMap-1.8](https://www.cnblogs.com/yangming1996/p/8031199.html) 

    > `jdk1.7与1.8的实现机制`
    >
    > - Jdk1.7采用的分段锁技术，整个hash表被分成多个段，每个段对应一个segment锁。段与段之间的可以并发访问，同一个段之间并发访问需要获取锁；
    >
    >   >  ```java
    >   > public class ConcurrentHashMap<K, V> extends AbstractMap<K, V>
    >   >         implements ConcurrentMap<K, V>, Serializable {
    >   > 
    >   >     final Segment<K,V>[] segments;
    >   > 
    >   >     static final class Segment<K,V> extends ReentrantLock implements Serializable {
    >   >         transient volatile HashEntry<K,V>[] table;
    >   >     }
    >   >     
    >   >     static final class HashEntry<K,V> {
    >   >         final int hash;
    >   >         final K key;
    >   >         volatile V value;
    >   >         volatile HashEntry<K,V> next;
    >   >         // ... 省略 ...
    >   >     }
    >   > }
    >   > 
    >   >  ```
    >   >
    >   > 1.7 中ConcurrentHashMap的扩容只针对每个segment中的HashEntry数组进行扩容。
    >   >
    >   > ConcurrentHashMap在rehash的时候是有锁的，所以在rehash的过程中，其他线程无法对segment的hash表做操作，这就保证了线程安全。
    >   >
    >   > 
    >
    > - JDK1.8中的ConcurrentHashMap不再使用Segment分段锁，而是以table数组的头结点作为synchronized的锁。 **transient** **volatile** Node<K,V>[] table;
    >
    >   > - 使用volatile保证当Node中的值变化时对于其他线程是可见的
    >   >
    >   >   >  ```java
    >   >   > static class Node<K,V> implements Map.Entry<K,V> {
    >   >   >         final int hash;
    >   >   >         final K key;
    >   >   >         volatile V val;
    >   >   >         volatile Node<K,V> next;
    >   >   >   }
    >   >   >  ```
    >   >   >
    >   >   > Node中的val和next都被volatile关键。也就是我们改动val的值或者next的值对于其他线程是可见的。`因为volatile关键字，会在读指令前插入读屏障，可以让高速缓存中的数据失效，重新从主内存加载数据`
    >   >   >
    >   >   > 
    >   >
    >   > - 使用table数组的头结点作为synchronized的锁来保证写操作的安全
    >   >
    >   >   >  ```java
    >   >   >                 //头结点不为null，使用synchronized加锁
    >   >   >          synchronized (f) {
    >   >   >               if (tabAt(tab, i) == f) {
    >   >   >                 if (fh >= 0) {
    >   >   >                 //此时hash桶是链表结构
    >   >   >                 binCount = 1;
    >   >   >  ```
    >   >
    >   > - 当头结点为null时，使用CAS操作来保证数据能正确的写入。
    >   >
    >   >   tabAt方法通过Unsafe.getObjectVolatile()的方式获取数组对应index上的元素，getObjectVolatile作用于对应的内存偏移量上，是具备volatile内存语义的
    >   >
    >   >   >  ```java
    >   >   > //当头结点为null,则通过casTabAt方式写入
    >   >   >  else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
    >   >   >         if (casTabAt(tab, i, null,
    >   >   >                    new Node<K,V>(hash, key, value, null)))
    >   >   >                     break;      
    >   >   >             }
    >   >   > 
    >   >   >  ```
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
    > - **未初始化**：
    >
    >   - sizeCtl=0：表示没有指定初始容量。
    >
    >   - sizeCtl>0：表示初始容量。
    >
    > - **初始化中**：
    >
    >   - sizeCtl=-1,标记作用，告知其他线程，正在初始化
    >
    > - **正常状态**：
    >
    >   - sizeCtl=0.75n ,扩容阈值
    >
    > - **扩容中**:
    >
    >   - sizeCtl < 0 : 表示有其他线程正在执行扩容
    >
    >   - sizeCtl = (resizeStamp(n) << RESIZE_STAMP_SHIFT) + 2 :表示此时只有一个线程在执行扩容
    >
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

   18. [new一个对象后执行的操作](https://www.cnblogs.com/JackPn/p/9386182.html)  ***

       >  new一个对象就可以分为两个过程：**加载并初始化类**和**创建对象**
       >
       > 
       >
       > 

   19. 双亲委派模型

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
       >
       >  2. 可覆盖的loadClass方法。双亲委派的实现细节都在loadClass方法中，而该方法是一个**protected**的，意味着子类可以覆盖该方法，从而可绕过双亲委派逻辑
       >  3. 程序动态性的需求，比如热部署。OSGi已经成为业界事实上的Java模块化标准

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
   
4. [springboot的启动流程](https://juejin.im/post/6844903669998026759)

   > **springApplication对象的初始化**
   >
   > > - 使用 `SpringFactoriesLoader`查找并加载 classpath下 `META-INF/spring.factories`文件中所有可用的 `ApplicationContextInitializer`
   > > - 使用 `SpringFactoriesLoader`查找并加载 classpath下 `META-INF/spring.factories`文件中的所有可用的 `ApplicationListener`
   >
   > **Run方法的运行**
   >
   > - 通过 `SpringFactoriesLoader` 加载 `META-INF/spring.factories` 文件，获取并创建 `SpringApplicationRunListener` 对象
   > - 然后由 `SpringApplicationRunListener` 来发出 **starting 消息**
   > - 创建参数，并配置当前 SpringBoot 应用将要使用的 Environment
   > - 完成之后，依然由 `SpringApplicationRunListener` 来发出 **environmentPrepared 消息**
   > - 创建 `ApplicationContext`,初始化 `ApplicationContext`，并设置 Environment，加载相关配置等
   > - 由 `SpringApplicationRunListener` 来发出 **contextPrepared 消息**，告知SpringBoot 应用使用的 `ApplicationContext` 已准备OK
   > - 将各种 beans 装载入 `ApplicationContext`，继续由 `SpringApplicationRunListener` 来发出 **contextLoaded 消息**，告知 SpringBoot 应用使用的 `ApplicationContext` 已装填OK
   > - refresh ApplicationContext，完成IoC容器可用的最后一步
   > - 由 `SpringApplicationRunListener` 来发出 **started 消息**
   > - 完成最终的程序的启动
   > - 由 `SpringApplicationRunListener` 来发出 **running 消息**，告知程序已运行起来了

5. [自定义的springboot stater ***](https://objcoding.com/2018/02/02/Costom-SpringBoot-Starter/)

   >  SpringBoot 项目就是由一个一个 Starter 组成的，一个 Starter 代表该项目的 SpringBoot 起步依赖
   >
   >  自定义stater类步骤
   >
   > 1. 创建一个项目引入springboot的autoconfigure包，pom中项目的artifact定位为spring-boot-stater-helloworld
   >
   > 2. 创建业务类，比如redis基本方法类；
   >
   > 3. 创建属性类，就是需要注入的配置；
   >
   > 4. 创建配置类，就是HelloAutoConfigration类。
   >
   >    ```java
   >    @Configuration
   >    // 当 HelloworldService 在类路径的条件下
   >    @ConditionalOnClass({HelloworldService.class})
   >    // 将 application.properties 的相关的属性字段与该类一一对应，并生成 Bean
   >    @EnableConfigurationProperties(HelloworldProperties.class)
   >    ```
   >
   > 5. 在resouce的META-INF/spring.factories下创建一个spring.factories文件，配置一个
   >
   >    ```properties
   >    # Auto Configure
   >    org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.objcoding.starters.helloworld.HelloworldAutoConfiguration
   >    ```

6. [springboot自动配置](https://mp.weixin.qq.com/s/Be7vAudvvvuCI2EdwPX80Q) ***

   >  @SpringBootApplication ->@EnableAutoConfiguration ->@Import
   >
   > 其父类**实现 ImportSelectors 接口下的selectImports() 方法返回的数组（类的全类名）都会被纳入到 Spring 容器中。**
   >
   > 由springFactoryLoader加载器加载所有jar包的**META-INF/spring.factories** 文件

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

12. [spring实现aop源码](https://juejin.im/post/6844904015843557389)

    > 1. 根据documentReader对象 的parseBeanDefinitions方法解析各种标签解析为beanDefinition。
    >
    > 2. parseAop标签 ：包括pointCut(中有过滤器classFilter、methodMacher)、parseAdvice（before/after处理）、**parseAspect**的定义
    >
    > 3. 然后向容器中注册AspectJAwareAdvisorAutoProxyCreator对象，用于创建代理类；
    >
    > 4. AspectJAwareAdvisorAutoProxyCreator 实现了BeanPostProcessor接口,可以对目标对象实例化之后（postProcessAfterInitialization），创建对应的代理对象。
    >
    >    >  **wrapIfNecessary**,
    >    >
    >    > 根据pointCut方法进行判断是否需要创建代理对象；查找代理类相关的advisor对象集合；
    >    >
    >    > 通过动态代理的方式（2种）生成代理对象；
    >    >
    >    > 放入到缓存中
    >
    > 

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
    >  >   >  代理对象生成的核心类`AbstractAutoProxyCreator，实现了`BeanPostProcessor接口，会在Bean初始化完成之后通过postProcessAfterInitialization方法生成代理对象。通过`ProxyFactory`的父类构造器实例化了`DefaultAopProxyFactory`类，从其源代码我们可以看到Spring动态代理方式选择策略的实现
    >  >   
    >  > - TransactionInterceptor 中调用invoke方法，然后通过他的支持类`TransactionAspectSupport`的`invokeWithinTransaction`方法进行事务处理，
    
20. spring事务和mysql事务的关系

    >  **spring事务**
    >
    > - 编程式事务：事务管理代码嵌入到业务方法中来控制事务的提交和回滚。代码冗余，灵活；
    > - 声明式事务：利用aop原理从代码中将事务管理分离出来。spring不管理事务，他提供了很多事务管理器，接口是（PlatformTransactionManager）。各个平台有自己的实现。
    >
    > **事务的传播特性**
    >
    >  **REQUIRED 、REQUIRES_NEW。**
    >
    > **其它属性**
    >
    > 回滚属性、超时和只读属性；
    >
    >  
    >
    > **关系**
    >
    > spring事务本质上使用数据库事务，而数据库事务本质上使用数据库锁，所以spring事务本质上使用数据库锁，开启spring事务意味着使用数据库锁；

21. spring 事务失效的场景

    > 1. Transactional注解标注方法修饰符为非public时，@Transactional注解将会不起作用。(非public方法不能被生成代理类)
    > 2. 在类内部调用调用类内部@Transactional标注的方法 。Spring AOP技术使用的是动态代理而自己调用自己的过程，并不存在代理对象的调。
    > 3. 事务方法内部捕捉了异常，没有抛出新的异常，导致事务操作不会进行回滚。
    > 4. 数据库引擎是否支持事务，MySql的引擎MyIsam不支持事务







