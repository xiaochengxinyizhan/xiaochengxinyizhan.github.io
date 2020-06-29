[TOC]

# prefaces(序言)

- bifurcate（v：分叉，使分叉，adj:植物成叉的）
- Evolve（v：进化，演进）
- tuning（n: 优化，微调）

## Conventions Used in This Book

本书使用的约定

在本书中使用了以下的语法约定：

**斜体**

​	暗示新的术语，URL，邮件地址，文件名称，和文件扩展信息

**宽字体**

​	通常用于变量，函数名称，数据库，数据类型，环境变量，声明和关键字

**宽字加粗体**

​	需要用户输入的命令行或者其他文本

**宽字加斜体**

​	会被替代的文本信息

狐狸：代表了技巧或者建议

乌鸦：代表了通用的笔记

蝎子：代表了警告或者小心

## 使用的代码例子

# 第一章、介绍

这是一本关于艺术和科学的Java性能书。

科学这部分的声明并不使人惊奇；关于性能的讨论包括了大量的数值和测量和分析。大多数性能工程师都有在科学领域的背景，并且应用科学的严谨是实现最大性能的一个关键环节。

关于艺术的部分是什么呢？性能优化是部分艺术与部分科学组成的这个概念是非常新的，但是他几乎给出了明确的知识在性能讨论方面。这只是一部分，因为艺术的想法有悖于我们的训练。

原因的另一部分是艺术对一些人看起来更像是从根本上是基于深度的知识和体验。据说魔术是无法辨识的从十分先进的科技里，和手机看起来是神奇圆桌的骑士。相似的，一个好的性能工程师产生的工作就看起来像艺术，但是艺术是个一个深度知识，经历和直觉的应用。

这个书不能帮助有同等的经历和直觉，但是他的目标是帮助有着深厚的知识，有着应用知识的你将时间花费在成为一个好的java性能工作师必要的开发技能上面。这个目标是给你一个Java平台性能方面的深度理解。

这个知识一共两个宽泛的类型。第一个是Java虚拟机自身的性能，jv m的配置方式影响了程序的性能许多方面。

暂时放弃，因为时间太耗费了。

# 正式内容 --看的翻译中文版

## 性能优化建议

<font color="red">

我们不应该把大量时间都浪费在那些小的性能改进上；过早考虑优化是所有噩梦的根源-高德纳</font>

性能分析表明能够获得巨大收益的时候进行性能优化

日志的输出-可以根据日志级别进行判断有效输出



CPU使用率，I/O延迟，系统的吞吐量需要根据测量和分析，采用结构化方法来判定到底是哪个组建影响了系统的性能。

借助性能分析来优化代码，重点关注性能分析中最耗时的操作。

利用奥卡姆剃刀原则解释性能问题：性能问题通常是很好解释的。新代码比机器配置更可能引入性能问题，而机器配置比JVM或者操作系统bug更容易引入性能问题。

为性能进行初步的估算，从而进行期望值的判断是高是低。

# 第二章、性能测试方法

## 2.1、原则1：测试真实应用

实际使用的真实环境进行性能测试

### 2.1.1、微基准测试

用来测量微笑代码单元的性能，包括调用同步方法的用时与非同步方法的用时比较，创建线程的代价与使用线程池的代价，执行某种算法的耗时与其替代实现的耗时。

* 原则1:必须使用被测的结果
* 不要包括无关操作
* 必须输入合理的参数

由于Java特殊性，代码执行的越多性能越好，所以测试的时候需要包括热身期。

微基准测试难写，价值有限，只是为了快速了解性能，不要依赖它们。

### 2.1.2、宏基准测试

整体应用链路的测试，一方面由于大型分布式系统应用比较多，复杂系统耦合，以及另一方面涉及到重点系统的资源分配问题。

数据库会因为缓存而消耗大量内存，网络因为发送大量数据而饱和，代码调用简单的方法不同优化，短代码路径因为CPU管线和缓存而比长代码更有效。

### 2.1.3、介基准测试



在模块或者操作级别隔离性能，是一种折中途径，比微基准测试更细。

## 2.2、原则2:理解批处理流逝时间，吞吐量和响应时间

多角度审视应用性能，应该测量哪个指标取决于对应用最最重要的因素。

### 2.2.1、批处理流逝时间

非Java应用直接将测试数据量扩大即可判断，但是针对Java应用需要考虑到的热身期。

### 2.2.2、吞吐量测试

吞吐量测试是基于一段时间内所能完成的工作量

每秒事务数TPS，每秒请求数RPS，每秒操作数OPS

吞吐量测试要在热身期之后进行，更加接近。

### 2.2.3、响应时间测试

一个常用测试指标是响应时间，从客户端发送请求至收到响应之间的流逝。

响应时间测试和吞吐量测试的区别：响应时间测试比吞吐量测试多增加了思考时间。

![image-20200209132803535](/Users/didi/Library/Application Support/typora-user-images/image-20200209132803535.png)

对于离群值的响应时间，应该考虑尽量减少其带来的影响。

![image-20200209133417630](/Users/didi/Library/Application Support/typora-user-images/image-20200209133417630.png)

![image-20200209133656485](/Users/didi/Library/Application Support/typora-user-images/image-20200209133656485.png)

【总结】

1. Java性能测试中很少使用面向批处理的测试（或者任何没有热身期的测试），但这种测试可以产生很有价值的后果。
2. 其他可以测量吞吐量或响应时间的测试，则依赖负载是否以固定的速率加载（基于模拟的客户端思考时间）。

## 2.3、原则3：用统计方法应对性能的变化

性能测试的结果会随时间而变。比如后台的进程或者网络时不时的拥堵等。

好的基准测试不会每次都处理相同的数据集，而是会测试一些随机行为以模拟真实的世界。问题是：运行的结果差别是因为性能有变化还是因为测试的随机性。

因代码更改而进行的测试--回归测试。原先的代码称为基线，新的代码称为试样。

![image-20200209134914678](/Users/didi/Library/Application Support/typora-user-images/image-20200209134914678.png)

正确判定测试结果间的差异需要统计分析，通过统计分析才能分析这些差异是不是归因于随机因素。

## 2.4、原则4：尽早频繁测试

代码发生变化-性能也会随之变化，堆内存使用情况会改变，代码编译也会改变。

### 2.4.1、自动化测试一切：

​	所有性能测试都应该脚本化或者程序化。测试运行前必须通过自动化技术确保机器处于已知状态，以及环境配置都相同的情况下，产生置信度报告，说明统计结果。

### 2.4.2、测试一切

CPU使用率，磁盘使用率，网络使用率，内存使用率等等

应用产生的日志，以及垃圾收集器的日志，

还应该包括JFR记录的信息，或者对系统影响较小的性能分析信息

周期性线程堆栈，其他堆分析数据。数据库的系统统计数据。可以作为未来的回归分析。

### 2.4.3、在真实系统上运行

# 第三章、Java性能调优工具箱

第二章获取数据，第三章主要是理解掌握精确的运行数据。

## 3.1、操作系统的工具和分析

### 3.1.1、CPU使用率

CPU使用率可以分为2类：用户态时间和系统时间

用户态时间是CPU执行应用代码所占时间的百分比，而系统时间则是CPU执行内核代码所占时间的百分比。系统态时间与应用相关，比如应用执行I/O操作，系统就会执行内核代码从磁盘读取文件，或者将缓存数据发送到网络，等等。任何使用底层系统资源的操作，都会导致应用占用更多的系统态时间。性能调优的目的：让在尽可能短的时间内让CPU使用率尽可能的高。

![image-20200209143702399](/Users/didi/Library/Application Support/typora-user-images/image-20200209143702399.png)

CPU被占用450毫秒（42%的时间执行用户代码，3%的时间执行系统代码），相对应的CPU空闲550毫秒，CPU空闲的原因可能如下：

* 应用被同步原语阻塞，直至锁释放才能继续执行。
* 应用在等待某些东西，例如数据库调用所返回的响应。
* 应用的确是无所事事。

### 3.1.2、CPU运行队列

UNIX系统称为运行队列。

【总结】

1. 检查应用性能时候，首先应该审查CPU时间
2. 优化代码的目的是提升而不是降低（更短时间段内的）CPU使用率
3. 在试图深入优化应用前，应该先弄清楚为何CPU使用率低。

### 3.1.3、磁盘使用率

监控磁盘使用率的2个目的：

* 与应用本身相关，如果应用正在做大量的磁盘I/O操作，那么I/O就容易成为瓶颈。查看IOwai t，是否需要提高I？O操作。
* 预计应用不会很高的I/O，有助于监控系统是否存在内存交换。计算机的物理内存量是固定的，但他们可以用大的多的虚拟内存运行一系列应用，应用会保留更多超过他们实际所需的内存量，并且他们通常也只使用配给他们内存的一部分。这两种情况，操作系统可以将不用的内存保留到磁盘上，等需要的时候在换页到物理内存。

【总结】

1. 对于所有系统应用都应该关注磁盘使用率，系统交换会影响性能
2. 写入磁盘应用遇到瓶颈，写入数据太少（吞吐量过低0或者写入数据太多（吞吐量太高）

### 3.1.4、网络使用率

1000M网络带宽每秒处理125M字节

![image-20200209145925346](/Users/didi/Library/Application Support/typora-user-images/image-20200209145925346.png)

网络无法支持100%的使用率，承受40%的使用率基本上接口饱和。

【总结】

1. 基于网络应用，需要监控网络确保不是瓶颈
2. 遇到瓶颈可能是因为写入太少或者写入太多（吞吐量过低，或者过高的原因）

## 3.2、Java监控工具

![image-20200209150415789](/Users/didi/Library/Application Support/typora-user-images/image-20200209150415789.png)

![image-20200209150434255](/Users/didi/Library/Application Support/typora-user-images/image-20200209150434255.png)

![image-20200209151852988](/Users/didi/Library/Application Support/typora-user-images/image-20200209151852988.png)

![image-20200209151910968](/Users/didi/Library/Application Support/typora-user-images/image-20200209151910968.png)

【总结】jcmd可用来查找JVM的所有基本信息，命令行加上-XX:Printflagsfinal输出有标志的默认值。

### 3.2.2、线程信息

![image-20200209152504968](/Users/didi/Library/Application Support/typora-user-images/image-20200209152504968.png)

### 3.2.3、类信息

### 3.2.4、实时GC信息

### 3.2.5、事后堆转储

![image-20200209152651464](/Users/didi/Library/Application Support/typora-user-images/image-20200209152651464.png)

## 3.3、性能分析工具

### 3.3.1、采样分析器

性能分析有2种模式：数据采样或者数据探查

### 3.3.2、探查分析器

侵入性更强，适用范围 ：较小的代码或者包范围

### 3.3.3、阻塞方法和线程时间线

线程阻塞可能是性能问题也可能不是，需要具体问题具体定位。

### 3.3.4、本地分析器

## 3.4、Java任务控制

![image-20200209155129615](/Users/didi/Library/Application Support/typora-user-images/image-20200209155129615.png)

### 3.4.1、Java飞行记录器

![image-20200209155702455](/Users/didi/Library/Application Support/typora-user-images/image-20200209155702455.png)

![image-20200209155730114](/Users/didi/Library/Application Support/typora-user-images/image-20200209155730114.png)

![image-20200209155747859](/Users/didi/Library/Application Support/typora-user-images/image-20200209155747859.png)

![image-20200209155812040](/Users/didi/Library/Application Support/typora-user-images/image-20200209155812040.png)

![image-20200209155842932](/Users/didi/Library/Application Support/typora-user-images/image-20200209155842932.png)

# 第四章、JIT编译器

## 4.1、概览

just in time 即时编译器是Java虚拟机的核心。

编译型语言：C++，Fortran，依赖编译器

解释型语言：PHP，PERL，依赖解释器

平台独立的解释型语言：Java介于上面的两者中间，运行的是理想化的二进制代码。由于这个编译是在执行的时候执行的，所以称为即时编译

热点编译：Java对于执行代码时候不会立即编译，会根据需要进行编译，避免过多编译，第二个原因是为了进行编译优化，执行越多，他会越了解这段代码进行编译优化。

![image-20200209161903003](/Users/didi/Library/Application Support/typora-user-images/image-20200209161903003.png)

## 4.2、调优入门：选择编译器类型client或者server

![image-20200209162142690](/Users/didi/Library/Application Support/typora-user-images/image-20200209162142690.png)

<font color=red>Java8分层编译默认是开启的</font>

### 4.2.1、优化启动

「总结」

  		启动时间是性能的考量，client编译器就是非常有用的

 		 分层编译的启动时间可以非常接近client编译器所获的启动时间

### 4.2.2、优化批处理

![image-20200209163147689](/Users/didi/Library/Application Support/typora-user-images/image-20200209163147689.png)

![image-20200209163203663](/Users/didi/Library/Application Support/typora-user-images/image-20200209163203663.png)

![image-20200209163224558](/Users/didi/Library/Application Support/typora-user-images/image-20200209163224558.png)

### 4.2.3、优化长时间运行的应用

![image-20200209163342752](/Users/didi/Library/Application Support/typora-user-images/image-20200209163342752.png)

## 4.3、Java和JIT编译器版本

![image-20200209163603301](/Users/didi/Library/Application Support/typora-user-images/image-20200209163603301.png)

![image-20200209163617735](/Users/didi/Library/Application Support/typora-user-images/image-20200209163617735.png)

## 4.4、编译器中级调优

![image-20200209164023442](/Users/didi/Library/Application Support/typora-user-images/image-20200209164023442.png)

### 4.4.1、调优代码缓存

![image-20200209164106980](/Users/didi/Library/Application Support/typora-user-images/image-20200209164106980.png)

![image-20200209164145779](/Users/didi/Library/Application Support/typora-user-images/image-20200209164145779.png)

![image-20200209164224252](/Users/didi/Library/Application Support/typora-user-images/image-20200209164224252.png)

![image-20200209164326664](/Users/didi/Library/Application Support/typora-user-images/image-20200209164326664.png)

### 4.4.2、编译阈值

![image-20200209164646403](/Users/didi/Library/Application Support/typora-user-images/image-20200209164646403.png)

![image-20200209164716884](/Users/didi/Library/Application Support/typora-user-images/image-20200209164716884.png)



### 4.4.3、检测编译过程

![image-20200209165005466](/Users/didi/Library/Application Support/typora-user-images/image-20200209165005466.png)

![image-20200209165026901](/Users/didi/Library/Application Support/typora-user-images/image-20200209165026901.png)

## 4.5、高级编译器调优

### 4.5.1、编译线程

### ![image-20200209165147834](/Users/didi/Library/Application Support/typora-user-images/image-20200209165147834.png)

![image-20200209165247280](/Users/didi/Library/Application Support/typora-user-images/image-20200209165247280.png)

![image-20200209165307469](/Users/didi/Library/Application Support/typora-user-images/image-20200209165307469.png)

### 4.5.2、内联

内联默认是开启的，可以通过-XX：Inline关闭，从源代码编译JVM可以通过+XX：PrintInlining信息获取如何内联，由于内联性能高于关闭时候所以一般在JVM内联的条件下进行调优。

![image-20200209165744304](/Users/didi/Library/Application Support/typora-user-images/image-20200209165744304.png)

![image-20200209165426406](/Users/didi/Library/Application Support/typora-user-images/image-20200209165426406.png)

### 4.5.3、逃逸分析

默认true，+XX：+DoEscapeAnalysis

![image-20200209165949450](/Users/didi/Library/Application Support/typora-user-images/image-20200209165949450.png)

## 4.6、逆优化

### 4.6.1、代码被丢弃

### 4.6.2、逆优化僵尸代码

![image-20200209170315461](/Users/didi/Library/Application Support/typora-user-images/image-20200209170315461.png)

## 4.7、分层编译级别

![image-20200209170408651](/Users/didi/Library/Application Support/typora-user-images/image-20200209170408651.png)

![image-20200209170430747](/Users/didi/Library/Application Support/typora-user-images/image-20200209170430747.png)

![image-20200209170540999](/Users/didi/Library/Application Support/typora-user-images/image-20200209170540999.png)

# 第五章、垃圾收集入门

## 5.1、垃圾收集概述

所有应用都停止运行所产生的停顿被称为时空停顿（STOP-THE-WORLD）

### 5.1.1、分代垃圾收集器

![image-20200209230532103](/Users/didi/Library/Application Support/typora-user-images/image-20200209230532103.png)

### 5.1.2、GC算法

![image-20200209230758532](/Users/didi/Library/Application Support/typora-user-images/image-20200209230758532.png)

![image-20200209230832222](/Users/didi/Library/Application Support/typora-user-images/image-20200209230832222.png)

### 5.1.3、选择GC算法

![image-20200209231056280](/Users/didi/Library/Application Support/typora-user-images/image-20200209231056280.png)

![image-20200209231158444](/Users/didi/Library/Application Support/typora-user-images/image-20200209231158444.png)

![image-20200209231327758](/Users/didi/Library/Application Support/typora-user-images/image-20200209231327758.png)

![image-20200209231408957](/Users/didi/Library/Application Support/typora-user-images/image-20200209231408957.png)

## 5.2、GC调优基础

### 5.2.1、调整堆的大小

![image-20200209231745402](/Users/didi/Library/Application Support/typora-user-images/image-20200209231745402.png)

### 5.2.2、代空间调整

![image-20200209231917866](/Users/didi/Library/Application Support/typora-user-images/image-20200209231917866.png)

![image-20200209231947001](/Users/didi/Library/Application Support/typora-user-images/image-20200209231947001.png)

### 5.2.3、永久代和元空间的调整

![image-20200209232104710](/Users/didi/Library/Application Support/typora-user-images/image-20200209232104710.png)

### 5.2.4、控制并发

![image-20200209232449159](/Users/didi/Library/Application Support/typora-user-images/image-20200209232449159.png)

![image-20200209232409048](/Users/didi/Library/Application Support/typora-user-images/image-20200209232409048.png)

### 5.2.5、自适应调整

+XX：UseAdaptiveSizePolicy，可以加上+XX：PrintUseAdaptiveSizePolicy来知道调节的时候过程

![image-20200209232609703](/Users/didi/Library/Application Support/typora-user-images/image-20200209232609703.png)

## 5.3、垃圾回收工具

-verbose：g c,+XX:PrintGC,+XX:PrintGCDetails,-xloggc

![image-20200209233102725](/Users/didi/Library/Application Support/typora-user-images/image-20200209233102725.png)

![image-20200209233142538](/Users/didi/Library/Application Support/typora-user-images/image-20200209233142538.png)

# 第六章、垃圾收集算法

## 6.1、并行收集器

![image-20200209233334218](/Users/didi/Library/Application Support/typora-user-images/image-20200209233334218.png)

![image-20200209233428116](/Users/didi/Library/Application Support/typora-user-images/image-20200209233428116.png)

![image-20200209233447465](/Users/didi/Library/Application Support/typora-user-images/image-20200209233447465.png)

![image-20200209233604059](/Users/didi/Library/Application Support/typora-user-images/image-20200209233604059.png)

![image-20200209233734748](/Users/didi/Library/Application Support/typora-user-images/image-20200209233734748.png)

## 6.2、理解CMS收集器

![image-20200209233831146](/Users/didi/Library/Application Support/typora-user-images/image-20200209233831146.png)

![image-20200209233901110](/Users/didi/Library/Application Support/typora-user-images/image-20200209233901110.png)

![image-20200209234038388](/Users/didi/Library/Application Support/typora-user-images/image-20200209234038388.png)

### 6.2.1、针对并发模式失效的调优

![image-20200209234424615](/Users/didi/Library/Application Support/typora-user-images/image-20200209234424615.png)

![image-20200209234612678](/Users/didi/Library/Application Support/typora-user-images/image-20200209234612678.png)

### 6.2.2、CMS收集器的永久代调优

![image-20200209234731964](/Users/didi/Library/Application Support/typora-user-images/image-20200209234731964.png)

### 6.2.3、增量式CMS垃圾收集

![image-20200209234831372](/Users/didi/Library/Application Support/typora-user-images/image-20200209234831372.png)

![image-20200209234922448](/Users/didi/Library/Application Support/typora-user-images/image-20200209234922448.png)

## 6.3、理解G1垃圾收集器

![image-20200209235024485](/Users/didi/Library/Application Support/typora-user-images/image-20200209235024485.png)

![image-20200209235057502](/Users/didi/Library/Application Support/typora-user-images/image-20200209235057502.png)

![image-20200209235136011](/Users/didi/Library/Application Support/typora-user-images/image-20200209235136011.png)

![image-20200209235217809](/Users/didi/Library/Application Support/typora-user-images/image-20200209235217809.png)

6.3.1、G1垃圾收集器的调优

+XX：MaxGCPauseMills

+XX:ParallelGCThreads设置后台线程数

![image-20200209235623194](/Users/didi/Library/Application Support/typora-user-images/image-20200209235623194.png)

![image-20200209235736635](/Users/didi/Library/Application Support/typora-user-images/image-20200209235736635.png)

![image-20200209235813718](/Users/didi/Library/Application Support/typora-user-images/image-20200209235813718.png)

![image-20200209235837482](/Users/didi/Library/Application Support/typora-user-images/image-20200209235837482.png)



## 6.4、高级调优

### 6.4.1、晋升及Survivor空间



![image-20200210090605171](/Users/didi/Library/Application Support/typora-user-images/image-20200210090605171.png)

![image-20200210090747451](/Users/didi/Library/Application Support/typora-user-images/image-20200210090747451.png)

![image-20200210090948147](/Users/didi/Library/Application Support/typora-user-images/image-20200210090948147.png)

### 6.4.2、分配大对象

取决于线程本地分配缓存区（TLAB）

![image-20200210091345836](/Users/didi/Library/Application Support/typora-user-images/image-20200210091345836.png)

+XX：UseTLAB关闭线程本地分配缓存区，默认是开启的

![image-20200210091517390](/Users/didi/Library/Application Support/typora-user-images/image-20200210091517390.png)

![image-20200210092004834](/Users/didi/Library/Application Support/typora-user-images/image-20200210092004834.png)

![image-20200210092233109](/Users/didi/Library/Application Support/typora-user-images/image-20200210092233109.png)

由于开源的JVM的话，可以使用+XX：PrintTLAB输出JVM分配情况信息，使用+XX：TLABSize=N可以显式的指定其大小。为了避免每次GC调整该参数可以使用+XX：ResizeTLAB，默认true，通过调整TLAB来提升性能的方式。

![image-20200210092742042](/Users/didi/Library/Application Support/typora-user-images/image-20200210092742042.png)

G1分区大小的计算

![image-20200210092900017](/Users/didi/Library/Application Support/typora-user-images/image-20200210092900017.png)

![image-20200210092935037](/Users/didi/Library/Application Support/typora-user-images/image-20200210092935037.png)

![image-20200210093053868](/Users/didi/Library/Application Support/typora-user-images/image-20200210093053868.png)

![image-20200210093139228](/Users/didi/Library/Application Support/typora-user-images/image-20200210093139228.png)

![image-20200210093220131](/Users/didi/Library/Application Support/typora-user-images/image-20200210093220131.png)

![image-20200210093250579](/Users/didi/Library/Application Support/typora-user-images/image-20200210093250579.png)

### 6.4.4、全盘掌握堆空间大小

![image-20200210093454621](/Users/didi/Library/Application Support/typora-user-images/image-20200210093454621.png)

「总结」

![image-20200210093619414](/Users/didi/Library/Application Support/typora-user-images/image-20200210093619414.png)

# 第7章、堆内存最佳实践

有节制的创建对象并尽快丢弃。重复的利用某些频繁创建的对象。这两个原则相互冲突但是又互相都有作用，所以需要权衡。

## 7.1、堆分析

### 7.1.1、堆直方图

看到应用内的对象数目，同时不需要进行完整的堆转储（因为堆转储会需要一段时间来分析，而且会消耗大量磁盘空间）

![image-20200211164231400](/Users/didi/Library/Application Support/typora-user-images/image-20200211164231400.png)

![image-20200211164400904](/Users/didi/Library/Application Support/typora-user-images/image-20200211164400904.png)

### 7.1.2、堆转储

![image-20200211164454078](/Users/didi/Library/Application Support/typora-user-images/image-20200211164454078.png)

![image-20200211164539080](/Users/didi/Library/Application Support/typora-user-images/image-20200211164539080.png)

![image-20200211164728456](/Users/didi/Library/Application Support/typora-user-images/image-20200211164728456.png)

### 7.1.3、内存溢出错误

![image-20200211164822640](/Users/didi/Library/Application Support/typora-user-images/image-20200211164822640.png)

![image-20200211164925350](/Users/didi/Library/Application Support/typora-user-images/image-20200211164925350.png)

![image-20200211165043592](/Users/didi/Library/Application Support/typora-user-images/image-20200211165043592.png)

![image-20200211165133529](/Users/didi/Library/Application Support/typora-user-images/image-20200211165133529.png)

## 7.2、减少内存使用

### 7.2.1、减少对象大小

对象会占用一定数量的堆内存，所以要减少内存使用。可以减少对象大小的。

方式：

* 减少实例变量的个数（效果很明显）
* 减少实例变量的大小（效果不明显）

![image-20200211165654035](/Users/didi/Library/Application Support/typora-user-images/image-20200211165654035.png)

### 7.2.2、延迟初始化

![image-20200211170034632](/Users/didi/Library/Application Support/typora-user-images/image-20200211170034632.png)

### 7.2.3、不可变对象和标准化对象

![image-20200211170714104](/Users/didi/Library/Application Support/typora-user-images/image-20200211170714104.png)

比如JSR 166规范中的CustomCOncurrentHashMap

### 7.2.4、字符串的保留

![image-20200211170929174](/Users/didi/Library/Application Support/typora-user-images/image-20200211170929174.png)

![image-20200211171047579](/Users/didi/Library/Application Support/typora-user-images/image-20200211171047579.png)

## 7.3、对象声明周期管理

### 7.3.1、对象重用

方式：

* 对象池
* 线程局部变量

对象在堆中时间越久，则GC的效率越差。

![image-20200211171532938](/Users/didi/Library/Application Support/typora-user-images/image-20200211171532938.png)

![image-20200211171550665](/Users/didi/Library/Application Support/typora-user-images/image-20200211171550665.png)

![image-20200211171652878](/Users/didi/Library/Application Support/typora-user-images/image-20200211171652878.png)

### 7.3.2、弱引用/软引用与其他引用

![image-20200211171752111](/Users/didi/Library/Application Support/typora-user-images/image-20200211171752111.png)

![image-20200211172032601](/Users/didi/Library/Application Support/typora-user-images/image-20200211172032601.png)

# 第八章、原生内存最佳实践

## 8.1、内存占用

JVM原生内存+堆内存的总量=应用总的内存占用

### 8.1.1、测量内存占用

使用top或者ps获取基本数据

![image-20200211174921809](/Users/didi/Library/Application Support/typora-user-images/image-20200211174921809.png)

### 8.1.2、内存占用最小化

![image-20200211175110916](/Users/didi/Library/Application Support/typora-user-images/image-20200211175110916.png)

### 8.1.3、原生NIO缓冲区

![image-20200211175330594](/Users/didi/Library/Application Support/typora-user-images/image-20200211175330594.png)

### 8.1.4、原生内存跟踪

![image-20200211175507069](/Users/didi/Library/Application Support/typora-user-images/image-20200211175507069.png)

![image-20200211175537188](/Users/didi/Library/Application Support/typora-user-images/image-20200211175537188.png)

![image-20200211175631743](/Users/didi/Library/Application Support/typora-user-images/image-20200211175631743.png)

![image-20200211175701471](/Users/didi/Library/Application Support/typora-user-images/image-20200211175701471.png)

![image-20200211175724613](/Users/didi/Library/Application Support/typora-user-images/image-20200211175724613.png)



##  8.2、针对不同操作系统优化JVM



### 8.2.1、大页

![image-20200211175934763](/Users/didi/Library/Application Support/typora-user-images/image-20200211175934763.png)

![image-20200211180034883](/Users/didi/Library/Application Support/typora-user-images/image-20200211180034883.png)

![image-20200211180112940](/Users/didi/Library/Application Support/typora-user-images/image-20200211180112940.png)



![image-20200211180151885](/Users/didi/Library/Application Support/typora-user-images/image-20200211180151885.png)

![image-20200211180222483](/Users/didi/Library/Application Support/typora-user-images/image-20200211180222483.png)

![image-20200211180251021](/Users/didi/Library/Application Support/typora-user-images/image-20200211180251021.png)

# 第九章、线程与同步的性能

## 9.1、线程池与ThreadPoolExecutor

最小线程数的意义：创建线程的成本非常高昂，可以提高任务提交时候的整体性能。另外一方面，线程需要一些系统资源，包括栈所需的原生内存，空闲线程太多，就会消耗本来可以分配给其他进程的资源。最大线程数相当于一个限流阀，来防止一次执行太多的线程。

### 9.1.1、设置最大线程数

根据CPU，以及是CPU密集型还是I/O密集型来测试线程池的最大性能。

### 9.1.2、设置最小线程数

* 设置最小值1，出发点是防止系统创建太多线程，消耗太多资源，因为每个线程都需要一定量的内存，特别是线程的栈。

* 设置线程的空闲时间，基于每个线程创建出来留存几分钟为了处理任何负载飙升的情况。

* 留存一些空闲线程

### 9.1.3、线程池任务大小

![image-20200212105642329](/Users/didi/Library/Application Support/typora-user-images/image-20200212105642329.png)

### 9.1.4、设置ThreadPoolExecutor的大小

![image-20200212105848418](/Users/didi/Library/Application Support/typora-user-images/image-20200212105848418.png)

## 9.2、ForkJoinPool

内部使用了一个无界任务列表，供构造器中所指定的数目（如果所选的是无参构造器，则为该机器上的CPU数）的线程来运行。

工作窃取算法，自动并行化

![image-20200212111048318](/Users/didi/Library/Application Support/typora-user-images/image-20200212111048318.png)

## 9.3、线程同步

![image-20200212111526932](/Users/didi/Library/Application Support/typora-user-images/image-20200212111526932.png)

![image-20200212111543256](/Users/didi/Library/Application Support/typora-user-images/image-20200212111543256.png)

### 9.3.1、同步的代价

![image-20200212111701937](/Users/didi/Library/Application Support/typora-user-images/image-20200212111701937.png)

2、锁定对象的开销

* 获取同步锁的成本

* Java内存模型（Java特有的开销）

  ![image-20200212112038376](/Users/didi/Library/Application Support/typora-user-images/image-20200212112038376.png)

  

![image-20200212112127353](/Users/didi/Library/Application Support/typora-user-images/image-20200212112127353.png)

### 9.3.2、避免同步

* 每个线程使用不同的对象。

* 基于CAS的替代方案。

![image-20200212112410443](/Users/didi/Library/Application Support/typora-user-images/image-20200212112410443.png)

![image-20200212112500865](/Users/didi/Library/Application Support/typora-user-images/image-20200212112500865.png)

![image-20200212112527091](/Users/didi/Library/Application Support/typora-user-images/image-20200212112527091.png)

### 9.3.3、伪共享

加载邻接值是有意义的，大多数情况下，如果这些实例变量呗加载到当前核的告诉内存中，内存访问非常快，这是很大的性能优势。

缺点：当程序更新本地缓存中的某个值，当前的核必须通知其他核，这个内存被修改了，其他核必须作废其缓存行，并重新从内存中加载。

如何优化：

* 检查缓存未命中事件来检测伪共享（特定的原生分析器可能会提供给定代码行的每指令周期数（Cycles Per Instruction,CPI）的相关信息，如果某个循环内的一条简单指令的CPI非常高，可能预示着代码正在等待将目标内存的信息重新加载到CPU缓存中。）
* * 也可以凭借直觉和实验：比如某个循环耗时非常长。

解决方案：

1. 避免伪共享的主要手段就是代码检查。

2. 填充相关变量

![image-20200212114133759](/Users/didi/Library/Application Support/typora-user-images/image-20200212114133759.png)



![image-20200212114156051](/Users/didi/Library/Application Support/typora-user-images/image-20200212114156051.png)



![image-20200212114226507](/Users/didi/Library/Application Support/typora-user-images/image-20200212114226507.png)

## 9.4、JVM线程调优

### 9.4.1、调节线程栈大小

前提是空间非常珍贵，物理内存非常有限，去调节该线程栈大小。

![image-20200212115523949](/Users/didi/Library/Application Support/typora-user-images/image-20200212115523949.png)

### 9.4.2、偏向锁

![image-20200212115621281](/Users/didi/Library/Application Support/typora-user-images/image-20200212115621281.png)

### 9.4.3、自旋锁

![image-20200212115640153](/Users/didi/Library/Application Support/typora-user-images/image-20200212115640153.png)

### 9.4.4、线程优先级

## 9.5、监控线程与锁

### 9.5.1、查看线程

查看线程状态-根据jconsole。

### 9.5.2、查看阻塞线程

![image-20200212121656212](/Users/didi/Library/Application Support/typora-user-images/image-20200212121656212.png)

![image-20200212121920592](/Users/didi/Library/Application Support/typora-user-images/image-20200212121920592.png)

![image-20200212122131374](/Users/didi/Library/Application Support/typora-user-images/image-20200212122131374.png)

![image-20200212122223067](/Users/didi/Library/Application Support/typora-user-images/image-20200212122223067.png)



# 第十章、Java.EE性能调优

## 10.1、Web容器的基本性能

![image-20200212124401257](/Users/didi/Library/Application Support/typora-user-images/image-20200212124401257.png)

![image-20200212125154003](/Users/didi/Library/Application Support/typora-user-images/image-20200212125154003.png)

![image-20200212125228861](/Users/didi/Library/Application Support/typora-user-images/image-20200212125228861.png)

![image-20200212125315160](/Users/didi/Library/Application Support/typora-user-images/image-20200212125315160.png)

![image-20200212125349783](/Users/didi/Library/Application Support/typora-user-images/image-20200212125349783.png)

![image-20200212125411447](/Users/didi/Library/Application Support/typora-user-images/image-20200212125411447.png)

## 10.2、线程池

![image-20200212125621363](/Users/didi/Library/Application Support/typora-user-images/image-20200212125621363.png)

## 10.3、EJB会话Bean

### 10.3.1、调优EJB对象池

![image-20200212125724143](/Users/didi/Library/Application Support/typora-user-images/image-20200212125724143.png)

![image-20200212125747390](/Users/didi/Library/Application Support/typora-user-images/image-20200212125747390.png)

![image-20200212125832922](/Users/didi/Library/Application Support/typora-user-images/image-20200212125832922.png)

### 10.3.2、调优EJB缓存

### 10.3.3、本地和远程实例

![image-20200212130001539](/Users/didi/Library/Application Support/typora-user-images/image-20200212130001539.png)

## 10.4、XML和JSON处理

### 10.4.1、数据大小（数据压缩）

![image-20200212130112918](/Users/didi/Library/Application Support/typora-user-images/image-20200212130112918.png)

### 10.4.2、解析和编组概述

![image-20200212130218637](/Users/didi/Library/Application Support/typora-user-images/image-20200212130218637.png)

### 10.4.3、选择解析器

![image-20200212130313522](/Users/didi/Library/Application Support/typora-user-images/image-20200212130313522.png)

![image-20200212130352154](/Users/didi/Library/Application Support/typora-user-images/image-20200212130352154.png)

![image-20200212130421780](/Users/didi/Library/Application Support/typora-user-images/image-20200212130421780.png)

![image-20200212130504056](/Users/didi/Library/Application Support/typora-user-images/image-20200212130504056.png)

### 10.4.4、XML验证

![image-20200212130549785](/Users/didi/Library/Application Support/typora-user-images/image-20200212130549785.png)

### 10.4.5、文档模型

![image-20200212130638491](/Users/didi/Library/Application Support/typora-user-images/image-20200212130638491.png)

![image-20200212130703126](/Users/didi/Library/Application Support/typora-user-images/image-20200212130703126.png)

### 10.4.6、Java对象模型

![image-20200212130746426](/Users/didi/Library/Application Support/typora-user-images/image-20200212130746426.png)

## 10.5、对象序列化

![image-20200212130846801](/Users/didi/Library/Application Support/typora-user-images/image-20200212130846801.png)

![image-20200212130926698](/Users/didi/Library/Application Support/typora-user-images/image-20200212130926698.png)

![image-20200212131004644](/Users/didi/Library/Application Support/typora-user-images/image-20200212131004644.png)

![image-20200212131044417](/Users/didi/Library/Application Support/typora-user-images/image-20200212131044417.png)

## 10.6、JavaEE网络API

![image-20200212131153508](/Users/didi/Library/Application Support/typora-user-images/image-20200212131153508.png)

# 第十一章、数据性能最佳实践

## 11.1、JDBC

### 11.1.1、JDBC驱动程序

![image-20200212131442667](/Users/didi/Library/Application Support/typora-user-images/image-20200212131442667.png)

### 11.1.2、预处理语句和语句池

![image-20200212131555287](/Users/didi/Library/Application Support/typora-user-images/image-20200212131555287.png)

### 11.1.3、数据库连接池

![image-20200212131657459](/Users/didi/Library/Application Support/typora-user-images/image-20200212131657459.png)

### 11.1.4、事务

![image-20200212131758012](/Users/didi/Library/Application Support/typora-user-images/image-20200212131758012.png)

### 11.1.5、结果集处理

![image-20200212131901434](/Users/didi/Library/Application Support/typora-user-images/image-20200212131901434.png)



# 第十二章、API的技巧

## 12.1、缓冲式I/O

![image-20200212132108167](/Users/didi/Library/Application Support/typora-user-images/image-20200212132108167.png)

## 12.2、类加载

![image-20200212132301716](/Users/didi/Library/Application Support/typora-user-images/image-20200212132301716.png)

## 12.3、随机数

![image-20200212132441891](/Users/didi/Library/Application Support/typora-user-images/image-20200212132441891.png)

![image-20200212132506999](/Users/didi/Library/Application Support/typora-user-images/image-20200212132506999.png)

## 12.4、Java原生接口

![image-20200212132648498](/Users/didi/Library/Application Support/typora-user-images/image-20200212132648498.png)

## 12.5、异常

![image-20200212132741530](/Users/didi/Library/Application Support/typora-user-images/image-20200212132741530.png)

![image-20200212132821788](/Users/didi/Library/Application Support/typora-user-images/image-20200212132821788.png)

## 12.6、字符串的性能

![image-20200212132920010](/Users/didi/Library/Application Support/typora-user-images/image-20200212132920010.png)

![image-20200212132942506](/Users/didi/Library/Application Support/typora-user-images/image-20200212132942506.png)



## 12.7、日志

3个原则：

* 协调好要打日志的数据和级别之间的关系
* 使用细粒度的Logger实例
* 引入日志时候，记得使用isLoggable()方法



![image-20200212133043869](/Users/didi/Library/Application Support/typora-user-images/image-20200212133043869.png)

## 12.8、Java集合类API

### 12.8.1、同步还是非同步

![image-20200212133350854](/Users/didi/Library/Application Support/typora-user-images/image-20200212133350854.png)



### 12.8.2、设定集合的大小

### 12.8.3、集合与内存使用效率

![image-20200212133504992](/Users/didi/Library/Application Support/typora-user-images/image-20200212133504992.png)



## 12.9、AggressiveOpts标志

### 12.9.1、替代实现

![image-20200212133615026](/Users/didi/Library/Application Support/typora-user-images/image-20200212133615026.png)

### 12.9.2、其他标志

![image-20200212133653518](/Users/didi/Library/Application Support/typora-user-images/image-20200212133653518.png)

## 12.10、Lambda表达式和匿名类

![image-20200212133746365](/Users/didi/Library/Application Support/typora-user-images/image-20200212133746365.png)



## 12.11、流和过滤器的性能

延迟遍历

![image-20200212134039367](/Users/didi/Library/Application Support/typora-user-images/image-20200212134039367.png)



# 性能调优标志概要

![image-20200212135052087](/Users/didi/Library/Application Support/typora-user-images/image-20200212135052087.png)

![image-20200212135113702](/Users/didi/Library/Application Support/typora-user-images/image-20200212135113702.png)

![image-20200212135301696](/Users/didi/Library/Application Support/typora-user-images/image-20200212135301696.png)

![image-20200212135325354](/Users/didi/Library/Application Support/typora-user-images/image-20200212135325354.png)

![image-20200212135348445](/Users/didi/Library/Application Support/typora-user-images/image-20200212135348445.png)

![image-20200212135413785](/Users/didi/Library/Application Support/typora-user-images/image-20200212135413785.png)



![image-20200212135837095](/Users/didi/Library/Application Support/typora-user-images/image-20200212135837095.png)

![image-20200212135857369](/Users/didi/Library/Application Support/typora-user-images/image-20200212135857369.png)

![image-20200212135917763](/Users/didi/Library/Application Support/typora-user-images/image-20200212135917763.png)

![image-20200212135940401](/Users/didi/Library/Application Support/typora-user-images/image-20200212135940401.png)

![image-20200212135958535](/Users/didi/Library/Application Support/typora-user-images/image-20200212135958535.png)