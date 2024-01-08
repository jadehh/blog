---
title: Java能力框架之JVM调优
categories: Java系列
author: _山海
tags:
  - Java
keywords: java
abbrlink: 1531152183
data: 2024-01-01 00:00:00
---

操作系统级别Java进程所占用的内存数值不能准确的反应堆内存的真实占用情况，因为GC过后这个值是不会变化的，内存调优的时候要更多地使用JDK提供的内存查看工具，比如JConsole和Java VisualVM。

## Full GC
### Full GC的几种情况
对JVM内存的系统级的调优主要的目的是减少GC的频率和Full GC的次数，过多的GC和Full GC是会占用很多的系统资源（主要是CPU），影响系统的吞吐量。特别要关注Full GC，因为它会对整个堆进行整理，导致Full GC一般由于以下几种情况：

- 旧生代空间不足
调优时尽量让对象在新生代GC时被回收、让对象在新生代多存活一段时间和不要创建过大的对象及数组避免直接在旧生代创建对象。
- Pemanet Generation空间不足
- 增大Perm Gen空间，避免太多静态对象
- 统计得到的GC后晋升到旧生代的平均大小大于旧生代剩余空间
- 控制好新生代和旧生代的比例
-  System.gc()被显示调用
- 垃圾回收不要手动触发，尽量依靠JVM自身的机制

###调优比例不良设置会导致的后果

调优手段主要是通过控制堆内存的各个部分的比例和GC策略来实现，各部分比例不良设置会导致什么后果。

#### 新生代设置过小，有以下两种情况
- 会让新生代GC次数非常频繁，增大系统消耗；
- 会导致大对象直接进入旧生代，占据了旧生代剩余空间，诱发Full GC。

#### 新生代设置过大，有以下两种情况
 - 会导致旧生代过小（堆总量一定），从而诱发Full GC；
 - 新生代GC耗时大幅度增加，一般说来新生代占整个堆1/3比较合适；

#### Survivor设置过小
导致对象从eden直接到达旧生代，降低了在新生代的存活时间。

####  Survivor设置过大
导致eden过小，增加了GC频率。

## 调优策略
由内存管理和垃圾回收可知新生代和旧生代都有多种GC策略和组合搭配，选择这些策略对于我们这些开发人员是个难题，JVM提供两种较为简单的GC策略的设置方式：

###  吞吐量优先
JVM以吞吐量为指标，自行选择相应的GC策略及控制新生代与旧生代的大小比例，来达到吞吐量指标。这个值可由```-XX:GCTimeRatio=n```来设置

###  暂停时间优先
JVM以暂停时间为指标，自行选择相应的GC策略及控制新生代与旧生代的大小比例，尽量保证每次GC造成的应用停止时间都在指定的数值范围内完成。这个值可由```-XX:MaxGCPauseRatio=n```来设置。

## JVM参数设置

### 堆内存
- -Xms:  或-XX:InitialHeapSize，最小堆内存,单位是Byte,m,g等
- -Xmx：或 -XX:MaxHeapSize，最大堆内存,单位是Byte,m,g等
比如```java -Xms128m -Xmx2g ```

### 新生代

-  -XX:NewSize  初始时年轻区内存，通常为 Xmx 的 1/3 或 1/4。  

-  -XX:MaxNewSize 最大年轻区内存  

-  -XX:MaxTenuringThreshold   
来控制新生代存活时间，尽量让对象在新生代被回收。 新生代=Eden + 2 个 Survivor空间, 但实际可用空间为 = Eden + 1 个 Survivor，即 90%。

### 新老比值变化

- -XX:NewRatio 指定老年代与新生代的堆大小比例。在使用CMS收集器时，此参数失效

-  -XX:SurvivorRatio   
新生代中 Eden 与 Survivor 的比值。默认值为 8,即 Eden 占新生代空间的 8/10，另外两个 Survivor 各占 1/10。

- -MinHeapFreeRatio  
指定jvm heap在使用率小于所设定的值，heap进行收缩，Xmx==Xms的情况下无效

- -XX:MaxHeapFreeRatio  
指定jvm heap在使用率大于n的情况下,heap进行扩张,Xmx==Xms的情况下无效

###  线程栈
- -Xss：线程栈大小

### Java heap页大小

- -XX:LargePageSizeInBytes：指定Java heap的分页页面大小

### 压缩类指针
- -XX:+UseCompressedClassPointers    
压缩类指针。对象的类指针（_klass）被压缩至32bit，使用类指针压缩空间的基地址

- -UseCompressedOops  
压缩对象指针，oops是普通对象指针，Java堆中对象的对象指针被压缩到32bit，使用堆基地址。
> 注意：64bit的服务器上设置-Xmx32g时，-XX:+UseCompressedOops和-XX:+UseCompressedClassPointers会失效，所以最大的堆设置为**31g**。

### 永久代(jdk8以前)
- -XX:PermSize 永久代初始大小
- -XX:MaxPermSize：永久代大小的最大值

###  元数据相关(jdk8及以后)
- -XX:MetaspaceSize 初始空间大小，达到该值就会触发垃圾收集进行类型卸载。

- -XX:MaxMetaspaceSize 最大空间，默认是没有限制的。 

- -XX:MinMetaspaceFreeRatio 在GC之后，最小的Metaspace剩余空间容量的百分比，减少为分配空间所导致的垃圾收集 

- -XX:MaxMetaspaceFreeRatio 在GC之后，最大的Metaspace剩余空间容量的百分比，减少为释放空间所导致的垃圾收集 

- -XX:MaxMetaspaceExpansion Metaspace增长时的最大幅度

- -XX:MinMetaspaceExpansion  Metaspace增长时的最小幅度

###  垃圾回收统计信息
- -XX:+PrintGC
- -XX:+PrintGCDetails
- -XX:+PrintGCTimeStamps
- -Xloggc:filename

### 回收器设置
- -XX:+UseSerialGC: 设置串行收集器
- -XX:+UseParallelGC: 设置并行收集器
- -XX:+UseParalledlOldGC:设置并行年老代收集器
- -XX:+UseConcMarkSweepGC:设置并发收集器

### 并行收集器设置
- -XX:ParallelGCThreads  
指定并行 GC 线程的数量。默认情况下，当 CPU 数量小于8，ParallelGCThreads 的值等于 CPU 数量，当 CPU 数量大于 8 时，则使用公式：```ParallelGCThreads = 8 + ((N - 8) * 5/8) = 3 +（（5*CPU）/ 8）```。

- -XX:MaxGCPauseMillis:设置并行收集最大暂停时间
- -XX:GCTimeRatio: 设置垃圾回收时间占程序运行时间的百分比```公式为1/(1+n)```。
- -XX:+CMSIncrementalMode: 设置为增量模式，适用于单CPU情况。

###  生成堆内存快照
- -XX:+HeapDumpOnOutOfMemoryError：让JVM在发生内存溢出时自动的生成堆内存快照。
- -XX:HeapDumpPath：快照存储路径，默认保存在JVM的启动目录下名为java_pid<pid>.hprof
- -XX:OnOutOfMemoryError: 异常发生时执行一些操作


### 代码缓存
- -XX:InitialCodeCacheSize
- -XX:ReservedCodeCacheSize  
    代码缓存确实很少引起性能问题，但是一旦发生其影响可能是毁灭性的。如果代码缓存被占满，JVM会打印出一条警告消息，并切换到interpreted-only 模式：JIT编译器被停用，字节码将不再会被编译成机器码。应用程序将继续运行，但运行速度会降低一个数量级，直到有人注意到这个问题。

- -XX:+UseCodeCacheFlushing   
  

当代码缓存被填满时让JVM放弃一些编译代码，避免当代码缓存被填满的时候JVM切换到interpreted-only 模式 。
    
<br>
    
## 后记

该类文章网上已经非常多，但是能完整的全面且清晰的写下来的，需要精力与耐力。为了JYM，笔者多写一文。

<br><br>
    
**参考**
    
http://niweiwei.iteye.com/blog/2123347

https://blog.csdn.net/huaweitman/article/details/50552960

http://www.cnblogs.com/redcreen/archive/2011/05/05/2038331.html，jvm调优参考配置

http://ifeve.com/useful-jvm-flags-part-4-heap-tuning/

https://blog.csdn.net/liubenlong007/article/details/78143285

https://www.cnblogs.com/redcreen/archive/2011/05/04/2037057.html

https://www.cnblogs.com/zhulin-jun/p/6516292.html

https://blog.csdn.net/beautygao/article/details/79083058

https://blog.csdn.net/liubenlong007/article/details/78143285