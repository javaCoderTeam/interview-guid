





工作中各个技术使用的经验

1. 常用linux命令总结

2. 如何解决缓存穿透  

   > - 存储空值，设置较短的超时时间；
   > - 使用布隆过滤器；
   > - 不设置超时时间；

3. 工作中常用的设计模式

   > 单例模式；
   >
   > 工厂模式；
   >
   > 策略模式；
   >
   > 模板方法模式；
   >
   > 适配器模式（重构的时候用的比较多）
   >
   > aop动态代理模式

4. 工作中如何使用kafka

   > 处理答题记录的统计数据；
   >
   > 消费订单数据的消息后进行排课处理；

5. 工作中解决的问题


	- 消息堆积的处理
	- 带宽打满的异常信息排查
	- 分库分表
	- 数据迁移

- **如何使用aop实现读写分离**

  > - 使用annotation定义标识数据源dataSource注解； write/read/other
  >
  > - 定义切面，切入点在service实现层的。 前置增强通过反射获取到对应的读注解和写注解到ThreadLocal里、后置增强从ThreadLocal清除对应的数据源；我们使用threadLocal以及linkedList持有线程的数据源副本；
  >
  > -  继承spring的 AbstractRoutingDataSource，**实现动态数源**。**determineCurrentLookupKey**方法从ThreadLocal里获取对应注解的数据源

- 如何定位cpu负载过高的问题  https://www.cnblogs.com/dennyzhangdd/p/11585971.html

  > 1. 使用top查看进程 
  >
  >    > -P按照cpu使用率排序
  >    >
  >    > -M 按照内存使用率排序
  >    >
  >    > -c 显示进程详细信息（包含了运行参数）的列表
  >
  > 2. top -H -p pid 查看进程下的异常线程id
  >
  >    >  -p 指定进程号
  >    > -H 显示该进程的所有线程
  >
  > 3. `printf "%x\n" 线程号`   将异常线程号转化为16进制
  >
  > 4. jstack 进程号|grep 16进制异常线程号   定位异常代码

- 解决的jvm上传大文件导致频繁fullGC;

  >  运营反馈群反馈教研工作台运行缓慢，操作卡顿，上传文件失败。
  >
  > https://juejin.im/post/5c8dce786fb9a049f23dabe2
  >
  > - 首先查看系统后台日志，没有明显异常，重启后问题解决，后续操作又有问题；
  >
  > - 我们在tomcat的catalina.sh下配置了打印虚拟机垃圾回收日志参数printGCDetail，
  >
  >   > ```
  >   > GC (Allocation Failure) [ParNew: 2527059K->10118K(2831168K), 0.0283851 secs] 2717025K->200087K(8074048K), 0.0285194 secs] [Times: user=0.11 sys=0.00, real=0.03 secs] 
  >   > ```
  >   >
  >   > 发现一直GC分配内存失败，而且parnew的回收基本没有变化回收前后大小基本没有变化，且fullgc很频繁，过几秒就有一次，而且重启后问题解决；（jstat -gcutil -pid -interval -count 达到同样的查看效果）；
  >
  > - 使用jmap -dump:format=b,file=heapdump.phrof pid 生成堆快照文件，
  >
  > - 导入eclipse mat，一个可以可视化展示堆的dump文件的工具。具体Leak Suspects 即可，mat 给出了内存泄漏的建议。另外也可以选择 Top Consumers 来查看最大对象报告。和线程相关的问题可以选择 thread overview 进行分析。除此之外就是选择 Histogram 类来分析
  >
  >   >  可以看到内存分析报告，看到一个org.apache.poi.xssf.usermodel 下的关于Xssf相关的对象非常多。
  >   >
  >   > 疑似的内存泄漏点是整个excel解析为List列表的位置。
  >
  > - 在数据量大和请求量过大的情况下，内存极速增长，然而这个过程中大量对象存活在年轻代，在年轻代无法被回收直接进入老年代。总体来说POI使用XMLBean处理Dom从Excel中读取对象，内存占用过大，耗费资源；并且上传速度慢，占用内存资源时间过长，导致一系列恶性循环。
  >
  > - 后使用的是阿里巴巴开源的一个excel处理框架easyExcel来解决问题的。他们的内存占用差一个数量级；限制excel的行数；



- redis获取不到连接

  > 
  >
  > 1. 晚高峰的时候运营反馈讲师反馈有学员获取试卷接口和交卷失败；
  >
  > 2. 首先查看线上日志，提示相关接口是空指针异常，获取不到试卷信息和答题记录信息，爆出的异常是 jedisConnectionException could not get a resource from pool。
  >
  > 3. 连接到redis服务器， info clients，获取服务器的当前连接数，此时的连接数是 3000多，而平时的是500左右；（线上的maxConnect 是600 ）
  >
  > 4. 使用slowLog get 查询，显示有好多慢日志（慢日志是超过20ms的），各种key的都有。毕竟是服务器压力比较大，而且看了下这些 object debug key 查看的也不是很大。所以这些key不一定是真凶；
  >
  > 5. 猜测是redis的大key导致的，然后使用 bgsave，使用redis-rdb-tool 分析成csv文件（处理了一个分片上的，且数据量太大）导入mysql  查询出来一些大key。 python写的程序
  >
  >    > - KNOWLEDGE_TREE_BY_CONDITION   。知识树的使用一个hashMap来处理，这个大key达到了50M，主要是多个知识树。最大的是1M，但是知识树有192个；`拆分、压缩`；
  >    > -  STUDY_REPORT:RANK_SCORE:70 。`对数量走限制（2万个）毕竟超过128个 会转化为跳表；`
  >    > -   T_TIKU_USER_RECORD_MAP:STU_ID2414629  
  >    > -  ORDER_LIFE_CYCLE  
  >    > -  APP_EXAM_PAPER_27388  本身很大的 使用gdk自带的gzip做了压缩；
  >    > -  tiku_server:rank:paper_id#157294
  >
  > 6. 自己写程序  跑定时任务  https://blog.csdn.net/qq_36530221/article/details/104950721 
  >
  >    scan  sleep 10s
  >
  >    object debug key   
  >
  >    ```java
  >    172.16.100.64:9736[7]> debug object LAST_DO_SYNC_FAILURE:LAST_DO_SYNC_FAILURE_NEW_TIKU_10375562_3145947
  >    Value at:0x7fa31bbb2600 refcount:1 encoding:raw serializedlength:796 lru:5484175 lru_seconds_idle:1281166
  >    ```
  >
  >    通过邮件进行通知；
  >
  > 7. 通过redis manager的一个工具，可以监听redis的客户端连接数，redis的qps；超过指定的阈值进行报警；
  >
  >    连接数的阈值是 1500 ； qps的阈值是  30000；
  >
  > 
  >
  > 突然报警，提示redis获取不到连接。 jedisConnectionException could not get a resource from pool
  >
  > ```java
  > 172.16.100.64:9736> info clients
  > # Clients
  > connected_clients:3115
  > client_longest_output_list:0
  > client_biggest_input_buf:0
  > blocked_clients:0
  >   
  > 172.16.100.64:9736> config get maxclients
  > 1) "maxclients"
  > 2) "10000"
  > 
  > ```
  >
  > 查看redis的连接池的参数设置
  >
  > ```
  > jedipool连接池配置推荐的设置（适合v2.5+版本，咨询了用户团队的开发人员）：
  > // 设置最小空闲连接数或者说初始化连接数
  > config.setMinIdle(10);
  > // 设置最大空闲连接数，（根据并发请求合理设置）
  > config.setMaxIdle(20);
  > // 设置最大连接数，（根据并发请求合理设置）。
  > config.setMaxTotal(100);
  > // 多长空闲时间之后回收空闲连接
  > setMinEvictableIdleTimeMillis(60000);
  > // 设置最大等待时间
  > config.setMaxWaitMillis(500);
  > ```
  >
  > 想到最近上线的需求是什么？
  >
  > 原来最近上线的功能需要是一个试卷的内容的存储，试卷大部分都比较大 从十几k到300多k。试卷内容中新加了一个试卷的答题人数的功能，然后他在每次交卷的时候去获取这个试卷的内容然后增加他的答题人数。这样当并发上来的时候根据redis的单线程模型，当并发量上来的时候，好多连接都在处理获取大key，计算后set大key，这样大部分时间处理器都用在处理redis的value的网络IO上。所以连接被占用，当并发量越大，连接数用的越多的时候，这种情况越明显；
  >
  > - 处理方案
  >
  >   使用jdk提供的gzip工具对value进行压缩，压缩80%。
  >
  >   对这种试卷答题人数的计算使用 incr命令，分开统计；

- 解决主从同步延迟的问题；

  > 运营群反馈获取结果页接口异常。具体场景是这样。刚交完卷获取结果页异常，再重新退出，点进去可以获取到解结果页；这种问题尤其在晚上下课就是高峰的时候尤为明显。
  >
  > - 刚开始猜测是数据库连接池太小了
  >
  >   > 看看线上这个时间段是不是有慢查询日志，
  >   >
  >   > 数据库连接情况，
  >   >
  >   > 查看时不时有获取不到数据库连接的情况，都没有出现；
  >
  > - 我们看线上的case，日志里显示抛出的是空指针异常，然后根据参数可以查询到答题记录，从库也可以查到。根据之前的运营的反馈是业务高峰才会出现，猜想有可能是主从同步延延迟问题。现在查询也是没问题，后面又反馈的时候 我们根据线上日志查询，发现是主从同步问题。
  >
  > - 缓存解决、主从延迟监控、找DBA解决主从延迟的问题 [彻底解决主从延迟](https://juejin.im/entry/5a73d1f55188257a780d8518)
  >
  
-  使用redis解决es处理排行榜的问题

  >  本地使用分库分表、分表存储，按照学员维度分表；
  >
  > 查询排行榜数据需要使用全量数据，而且使用的es的prepareSearch函数查询；试卷维度的，以分数降序，时间升序排行；
  >
  > 排行榜的场景是答题记录完成后，立马去查询的，但是没有查询到，经过查资料发现es的刷盘过程是
  >
  > ```
  > 1. 先写到内存中，此时不可搜索
  > 2. 默认经过 1s 之后进行 refresh 会被写入 lucene 的底层文件 segment 中 ，此时可以搜索到
  > ```
  > 
  >了解到它这种保证强刷盘特性 setRefresh，是每次index数据的时候强制刷新，这种可以实现。而且压测的时候是单个接口压测的，测试环境的压800 都是在100毫秒内 没问题；
  > 
  >开始是没问题的。
  > 
  >当流量上来后，查询排行榜的接口特别慢，查询日志发现是查询es的时候特别慢，猜测是es的setRefresh影响；
  > 
  >实时更新不能缓存，数据量大；
  > 
  >后来改为redis查询，正常放到缓存中，每次入库放入到redis的zset结构中，key是paperId，score是答分数减去答题用时值；
  > 
  
- 消息堆积的处理

  >  报警，消息堆积
  >
  >  查看是什么问题导致，暂时旧消息队列的消费；
  >
  >  新建topic、多个partition，使用多个消费者实例进行消费处理；

7. 工作中主要负责的功能

   > 题库中的随堂考，智能推题，遇到的问题，怎么解决的；
   >
   > 主从同步延迟；
   >
   > 同步消息给数据分析团队，开始采用的是接口，强依赖；
   >
   > 消息堆积；
   >
   > 排行榜问题；
   >
   > redis大字段问题；

8. 项目的性能指标

   服务器的配置
   服务的qps
   tomcat的配置
   jvm的配置
   redis的内存占用
   mysql的数据量
   主app的用户数



整体流程

介绍

- 项目背景要有但不宜过多、详细介绍、项目中职责、业务指标、遇到的问题且如何排查；
- 项目是如何做服务化、有哪些服务、服务之间的协议、是如何交互的；使用了什么组件，选型做了什么考虑；

tips

- 可以有意的引导他们往自己擅长的方面，具体的事解决了什么问题，遇到了什么坑，又是怎么优化的；

- 多突出项目中的亮点，比如10万+的qps、数据量是亿级别、使用profile工具排查系统负载过高的问题等；

- 或者请求量不大，聊聊当流量上来后的改造和优化方案；

- `平时多关注系统的性能指标，多参与同事排查性能问题，解决诡异bug的过程，讨论思路`；

  

问题的思路

- 针对复杂的需求的技术难点是什么，是如何解决的；
- 项目中遇到的疑难诡异的问题，排查思路是什么；
- 项目运维过程中有哪些性能方面的问题，又是如何优化的



demo

针对我提到的这个问题，我也想给你分享一个之前面试的案例。这个案例涉及排查一次自研的RPC框架的故障，回答的思路如下：

项目中使用的RPC框架存在某个Bug，偶发地出现RPC客户端发送给RPC服务节点的所有心跳包全部超时，从而让RPC客户端认为服务节点已经发生故障，然后RPC客户端就不会将流量发送给这个服务节点。由于项目中使用了 Netty3框架，我就理了一遍Netty3的连接过程的代码，确认是Boss线程或者NioWorke「线程某个地方阻塞了。

然后，我通过stace (系统调用抓取工具）抓取那段时间的系统调用，发现Boss线程中涉及的系统调用全部完成了，却没有发现把socket fd (文件句柄）注册给NioWorke「的selectorfd (文件句柄）的系统调用，于是确
 认是NioWoker的问题。

后来我打印了 jvm thread stack trace (Java的线程堆栈)，发现所有的NioWorke「线程阻塞在 Object.wait，代码位置是 Class.for-Name，这样就导致所有的NioWorker不再处理新的Register task (注册任务），NettyClientBootstrap 中所有的 ConnectFuture 的状态自然不被标记，最终导致上层所有的连 接超时。

再往下追查后，我发现JDK 6中确实存在类初始化过程中出现死锁的Bug，在升级到JDK 7之后问题就得到解决。

你看，本来是一个偶发的RPC框架层的问题，而你在追查这个Bug的过程中涉及到了Netty网络编程、系统调用追踪，和多线程锁诊断等知识，面试官在听到这样的案例之后也会认可你排查问题的思路和能力。



**自我介绍**

​        2017年10月加入尚德机构，在集团研发后端学平台参与学员学习相关系统的开发，

包括给主app使用的C端的课程、题库服务，课程主要包括给学员提供查看直播、重播课、查看课件ppt等课程资料，学习打卡等相关功能；题库服务包括练习类型和考试类型多个场景的考试和练习，随堂考、课后作业、章节练习，智能推题等做题服务；

B端的服务主要包括讲师、教研工作台等；主要给讲师使用的配置课程直播间、上传课件、上传试题、配置试卷、查看学员出勤情况、试卷正答率、上课评价相关功能；

 在工作中主要参与的工作是需求分析、技术文档的编写、组织技术评审、编码、配合测试上线，后期的维护；

​       再往前一家是2016到2017年在凤凰网任职，主要参与的是凤凰网的自媒体平台凤凰号的开发，是给自媒体用户上传文章、视频的web平台；主要包括给新闻客户端提供文章、视频列表以及统计相关接口服务以及面向社会内容创作者的自媒体平台；

​      

​       主要使用的是java技术栈，包括常用的springmvc、springboot/ mybatis框架，数据库主要用的mysql，常用的kafka以及redis中间件，用到过thrift的rpc框架；

**1. 如何使用aop实现读写分离**

- 使用annotation定义标识数据源的注解； write/read/other

- 定义切面，切入点在service实现层的、前置增强在对进入方法前根据对应的注解设置放到ThreadLocal中、后置处理从ThreadLocal清除对应的注解；我们使用threadLocal以及linkedList持有线程的数据源副本；

-  继承spring的 AbstractRoutingDataSource，**实现动态数据源**。**determineCurrentLookupKey**方法根据注解从linkedList获取对应的数据源

**2. 最熟悉的项目介绍下吧**

 

**服务介绍：**

课程和题库服务、他是尚德为学员查看课程和练习做题的平台， 主要分为多个场景，课上的随堂考，课后作业练习、平时按照章节练习、收藏练习、错题练习、智能练习（根据你之前的错题和收藏采用一定的算法给你推题）、群作业；主要功能接口包括知识点列表、科目列表、试卷内容、试题列表、交卷接口、结果页、解析页、排行榜接口；

主app的活跃用户大概是25万，

每天大概300节直播课，大概直播和重播20万左右的观看记录。

大概800到3000万的做题需求，

整个服务的qps可以达到6000到8000左右；

**服务架构**

主题框架是 java的 springMVC+mybatis    服务节点是9-12个；

数据库采用的是一主4从，3T的ssd机器

请求服务，为了缓解主库的压力，使用springAop实现了读写分离；

使用sharding jdbc实现了横向分表

其次使用redis，实现了热点数据缓存，使用gzip压缩了json串，释放了大量的内存空间；

使用thrift调用其它服务的比如订单、学期、课程数据

使用kafka完成了给数据中心同步数据，异步操作，缓解了交卷压力；

使用es 完成了日志数据的存储；

用nginx做反向代理  lvs 做负载 

**3. 用户缓存项目**

 该项目主要是为了解决接口中用户缓存信息更新不及时的问题，后续为了解决表中数据发生变化通知业务方进行业务处理的功能。

 亮点在于，可以根据业务的增删改查进行响应的业务处理，监控的表以及字段都可以动态调整；发送的topic可以存储到库里，然后动态的新增；

发送消息队列可以做好多用处，比如最初是更新用户缓存，后比如我们的科目信息发生变化后要通知订单中心，就是采用的这种策略；

**4. 单点登录系统**

该系主要是采用cas实现的sso，一处登陆，处处可用的效果；

主要用扫码登录、短信验证码登录、密码登录；

**原理**

**第一次**

1.  浏览器获取  A系统的资源，服务cookie中没有sessionId也没有携带ticket，那就重定向到casServer   http://sso.abc.com?service=http://a.abc.com

2. casServer端 发现请求的cookie中并没有TGT返给你登录页面, 来登录吧

3. 浏览器输入账号密码，返回到casServer进行验证；

4. 验证通过后，casServer发放该请求的ticket，同时在服务端设置TGT，然后 Redirect到 http://pr.abc.com/?ticket=ST-14041-VbqemTxMkTGbDtzyI3vk-cas01.example.org；

5. A系统发现 Client1又来访问我了，这次带了ticket去CAS Server验证一下

6.  server端验证，是我发放的ticket，给他资源吧

7. A系统设置和client1的cookie；返回Client1请求的资源

**第二次**

1.  访问Client2 http://B.abc.com, B系统发现Cookie中没有sessionid，也没有携带ticket，给你重定向到CAS Server登录吧 [http://sso.abc.com?service=http://b.abc.com](http://sso.abc.com?service=http://a.abc.com)

2. 有请求来访问我了,发现请求的cookie中有我签发的TGT，已经登陆了,给你个ST,重定向到client2吧 

http://ehr.abc.com?ticket=ST-12061-VdfHfgTxMkTGbDtzyI3vk-cas01.example.org

3. Redirect:Client2

http://ehr.abc.com?ticket=ST-12061-VdfHfgTxMkTGbDtzyI3vk-cas01.example.org

4. 这次带了ticket，去CAS Server验证一下

5. server端验证，是我发放的ticket，给他资源吧

6. A系统设置和client2的cookie；返回Client2请求的资源













































