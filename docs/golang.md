# 垃圾回收

## 常见的垃圾回收算法

## 三色标记法

## STW（Stop The World）

## 写屏障(Write Barrier)

# GPM 调度 和 CSP 模型

## CSP 模型？

## GPM 分别是什么、分别有多少数量？

## Goroutine调度策略

# golang基础数据结构的底层原理（slice，channel，map）

[(1条消息) golang基础数据结构的底层原理（slice，channel，map）_golang底层原理_慢！有杀气的博客-CSDN博客](https://blog.csdn.net/qq1319713925/article/details/117276730)



一：slice
Slice又称动态数组， 依托数组实现， 可以方便的进行扩容、 传递等， 实际使用中比数组更灵活。

底层数据结构：
type slice struct {
	array unsafe.Pointer
	len int
	cap int
}

二：channel

channel是go语言协程间通信的管道。channel可用于协程同步，也可以协程间可以传递各种消息数据。

底层数据结构
type hchan struct {
	qcount uint
	dataqsiz uint
	buf unsafe.Pointer
	elemsize uint16
	closed uint32
	elemtype *_type
	sendx uint
	recvx uint
	recvq waitq
	sendq waitq
	lock mutex
}
channel底层数据结构有一个环形队列指针   buf 
以及环形队列存储的数据类型和数据结构大小。  elemsize,elemtype
，还有标识环形队列指针读写位置的标记。sendx,recvx
等待读写的两个goroutine队列。 recvq,sendq
还有互斥锁。 lock

其中环形队列是用来存储传递的消息
两个gotoutine队列是用来存储和唤醒被该channel阻塞的队列。
数据类型和数据大小是用来读写消息时用于计算便宜量。

三：map
map底层使用哈希表来实现的，哈希过程产生冲突使用的冲突解决办法是链地址法。
除此之外常见的冲突解决办法还有：开放寻址法，链地址法，再次哈希法，创建一个公共溢出区等几种方法。
python中的字典底层依靠哈希表(hash table)实现, 使用开放寻址法解决冲突,
java和go都采用链地址法来解决哈希冲突。

底层结构
type hmap struct {
	count int // 当前保存的元素个数
	...
	B uint8 // 指示bucket数组的大小
	...
	buckets unsafe.Pointer // bucket数组指针， 数组的大小为2^B
	...
}
go的哈希表实现底层数据结构中有一个count标识当前元素个数，一个B成员标识桶的个数，还有一个buckes是一个指针指向桶数组。

而桶的底层数据结构：

type bmap struct {
	tophash [8]uint8 //存储哈希值的高8位
	data byte[1] //key value数据:key/key/key/.../value/value/value...
	overflow *bmap //溢出bucket的地址
}
tophash是个数组用来存放键的哈希值的高8位，，一个data成员用来存放value，还有一个Overflow成员用来指向溢出的桶的地址。

# CHAN 原理

## 结构体

## 读写流程

### 向 channel 写数据:

### 从 channel 读数据

### 关闭 channel

## 无缓冲 Chan 的发送和接收是否同步?

# context 结构原理

## 用途

## 数据结构

# 竞态、内存逃逸

## 竞态

## 逃逸分析

# go 中除了加 Mutex 锁以外还有哪些方式安全读写共享变量？

# golang中new和make的区别？

# Go中对nil的Slice和空Slice的处理是一致的吗?

# Go中对nil的Slice和空Slice的处理是一致的吗?

# 协程和线程和进程的区别？

# Golang的内存模型中为什么小对象多了会造成GC压力？

# channel 为什么它可以做到线程安全？

# GC 的触发条件？

# 怎么查看Goroutine的数量？怎么限制Goroutine的数量？

# Channel是同步的还是异步的？

# Goroutine和线程的区别？

# Go的Struct能不能比较？

# Go主协程如何等其余协程完再操作？

# Go的Slice如何扩容？

# Go中的map如何实现顺序读取？