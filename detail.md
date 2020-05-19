tcp如何保证可靠传输。https://blog.csdn.net/striveb/article/details/84063712  https://blog.csdn.net/Li_Ning_/article/details/52117463

kafka如何进行leader选举？

了解redis的zset结构吗

thrift的注册机制

如何慢查询sql explain





工作中各个技术使用的经验

1. 常用linux命令总结

2. 如何使用redis存储数据

3. 如何解决缓存穿透

4. 工作中常用的设计模式

5. 工作中如何使用kafka

6. 工作中解决的问题
	
	- 解决的jvm上传大文件导致频繁fullGC;
	- 使用redis解决es处理排行榜的问题；
	- 缓解主库的压力，使用aop实现的读写分离；
	- 解决主从同步延迟的问题； Es默认支持1秒刷新索引，现在对数据写入es加refresh操作，具体对性能的影响需要观察
	- 解决redis的大key问题；压缩
	- 消息堆积的处理
	
	  

7. 工作中主要负责的功能

8. 项目的性能指标

   服务器的配置
   服务的qps
   tomcat的配置
   jvm的配置
   redis的内存占用
   mysql的数据量
   主app的用户数