# redis:有哪些数据类型，zset底层咋实现

- string，list，set，sorted set，hash

# Redis key怎么设计

# Redis 是一个基于内存的高性能key-value数据库。

# * Redis相比memcached有哪些优势：

memcached所有的值均是简单的字符串，redis作为其替代者，支持更为丰富的数据类型
redis的速度比memcached快很多
redis可以持久化其数据

# * Redis是单线程
redis利用队列技术将并发访问变为串行访问，消除了传统数据库串行控制的开销

# * Reids6种淘汰策略

noeviction: 不删除策略, 达到最大内存限制时, 如果需要更多内存, 直接返回错误信息。大多数写命令都会导致占用更多的内存(有极少数会例外。
allkeys-lru:所有key通用; 优先删除最近最少使用(less recently used ,LRU) 的 key。
volatile-lru:只限于设置了 expire 的部分; 优先删除最近最少使用(less recently used ,LRU) 的 key。
allkeys-random:所有key通用; 随机删除一部分 key。
volatile-random: 只限于设置了 expire 的部分; 随机删除一部分 key。
volatile-ttl: 只限于设置了 expire 的部分; 优先删除剩余时间(time to live,TTL) 短的key。






# *Redis的并发竞争问题如何解决?
单进程单线程模式，采用队列模式将并发访问变为串行访问。Redis本身没有锁的概念，Redis对于多个客户端连接并不存在竞争，利用setnx实现锁。

# Redis是使用c语言开发的。

# Redis前端启动命令
./redis-server

# ** Redis 持久化方案：

RDB AOF

# * Redis 的主从复制
持久化保证了即使redis服务重启也不会丢失数据，因为redis服务重启后会将硬盘上持久化的数据恢复到内存中，但是当redis服务器的硬盘损坏了可能会导致数据丢失，如果通过redis的主从复制机制就可以避免这种单点故障

#  * Redis是单线程的，但Redis为什么这么快？
1、完全基于内存，绝大部分请求是纯粹的内存操作，非常快速。数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)；

2、数据结构简单，对数据操作也简单，Redis中的数据结构是专门进行设计的；

3、采用单线程，避免了不必要的上下文切换和竞争条件，也不存在多进程或者多线程导致的切换而消耗 CPU，不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗；

4、使用多路I/O复用模型，非阻塞IO；这里“多路”指的是多个网络连接，“复用”指的是复用同一个线程

5、使用底层模型不同，它们之间底层实现方式以及与客户端之间通信的应用协议不一样，Redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求；

# * 为什么Redis是单线程的？
Redis是基于内存的操作，CPU不是Redis的瓶颈，Redis的瓶颈最有可能是机器内存的大小或者网络带宽。既然单线程容易实现，而且CPU不会成为瓶颈，那就顺理成章地采用单线程的方案了（毕竟采用多线程会有很多麻烦！）。

# * Redis info查看命令：info memory

# * Redis内存模型
used_memory：Redis分配器分配的内存总量（单位是字节），包括使用的虚拟内存（即swap）；Redis分配器后面会介绍。used_memory_human只是显示更友好。

used_memory_rss：Redis进程占据操作系统的内存（单位是字节），与top及ps命令看到的值是一致的；除了分配器分配的内存之外，used_memory_rss还包括进程运行本身需要的内存、内存碎片等，但是不包括虚拟内存。

mem_fragmentation_ratio：内存碎片比率，该值是used_memory_rss / used_memory的比值。

mem_allocator：Redis使用的内存分配器，在编译时指定；可以是 libc 、jemalloc或者tcmalloc，默认是jemalloc；截图中使用的便是默认的jemalloc。

# Redis内存划分
数据
作为数据库，数据是最主要的部分；这部分占用的内存会统计在used_memory中。

进程本身运行需要的内存
Redis主进程本身运行肯定需要占用内存，如代码、常量池等等；这部分内存大约几兆，在大多数生产环境中与Redis数据占用的内存相比可以忽略。这部分内存不是由jemalloc分配，因此不会统计在used_memory中。

缓冲内存
缓冲内存包括客户端缓冲区、复制积压缓冲区、AOF缓冲区等；其中，客户端缓冲存储客户端连接的输入输出缓冲；复制积压缓冲用于部分复制功能；AOF缓冲区用于在进行AOF重写时，保存最近的写入命令。在了解相应功能之前，不需要知道这些缓冲的细节；这部分内存由jemalloc分配，因此会统计在used_memory中。

内存碎片
内存碎片是Redis在分配、回收物理内存过程中产生的。例如，如果对数据的更改频繁，而且数据之间的大小相差很大，可能导致redis释放的空间在物理内存中并没有释放，但redis又无法有效利用，这就形成了内存碎片。内存碎片不会统计在used_memory中。

# Redis对象有5种类型
无论是哪种类型，Redis都不会直接存储，而是通过redisObject对象进行存储。

# Redis没有直接使用C字符串
(即以空字符’\0’结尾的字符数组)作为默认的字符串表示，而是使用了SDS。SDS是简单动态字符串(Simple Dynamic String)的缩写。

# Reidis的SDS在C字符串的基础上加入了free和len字段

# ** Reids主从复制


# ** Redis哨兵

# * Reids持久化触发条件
RDB持久化的触发分为手动触发和自动触发两种。

# Redis 开启AOF
Redis服务器默认开启RDB，关闭AOF；要开启AOF，需要在配置文件中配置：

appendonly yes

# AOF常用配置总结
下面是AOF常用的配置项，以及默认值；前面介绍过的这里不再详细介绍。

appendonly no：是否开启AOF
appendfilename “appendonly.aof”：AOF文件名
dir ./：RDB文件和AOF文件所在目录
appendfsync everysec：fsync持久化策略
no-appendfsync-on-rewrite no：AOF重写期间是否禁止fsync；如果开启该选项，可以减轻文件重写时CPU和硬盘的负载（尤其是硬盘），但是可能会丢失AOF重写期间的数据；需要在负载和安全性之间进行平衡
auto-aof-rewrite-percentage 100：文件重写触发条件之一
auto-aof-rewrite-min-size 64mb：文件重写触发提交之一
aof-load-truncated yes：如果AOF文件结尾损坏，Redis启动时是否仍载入AOF文件
