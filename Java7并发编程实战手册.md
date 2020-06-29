[TOC]

# 第一章、线程管理

## 1.8、守护线程的使用

setDaemon的使用，应用场景：一个守护线程的任务在运行监控，其他线程的工作，保证维护的队列不变可控。

## 1.11、线程的分组

ThreadGroup的使用，线程组类存储了线程对象和关联的线程组对象，并可以访问它们的信息（例如状态），将执行的操作应用到所有成员上（例如中断）。

# 第二章、线程同步基础

## 2.1、简介

临界区（Critical Section），是一个用以访问共享资源的代码块，这个代码块在同一时间内只允许一个线程执行。

# 第三章、线程同步辅助类

Phaser，把并发任务分成多个阶段运行，在开始下一阶段前，当前阶段中的所有线程都必须执行完成。

Exchanger提供两个线程之间的数据交换点。

## 3.2、资源的并发访问控制

Semaphore，一种计数器，用来保护一个或者多个共享资源的访问。内部计数器只有0和1用来保护对唯一共享资源的访问。

遵循的三个步骤：

* acquire（）方法获取信号量

* 使用共享资源执行必要的操作

* 必须通过release（）方法释放信号量

  

acquireUninterruptibly()就是acquire（）方法，当信号量的内部计数器变成0的时候，信号量将阻塞线程直到被释放。线程在被阻塞的这段时间中，可能会被中断，抛出InterruptedException异常。而acquireUninterruptibly()会忽略线程的中断且不会抛出任何异常。

tryAcquire，这个方法试图获得信号量。如果能获得返回true，如果不能，就返回false，从而避开线程的阻塞和等待信号量的释放，我们可以根据返回值是true还是false来做出恰当的处理。

信号量也可以创建公平性和非公平性。

## 3.3、资源的多副本并发控制

可以通过int提供的传入参数来控制----Semaphore

## 3.4、等待多个并发事件的完成

CountDownLatch,在完成一组正在其他线程中执行的操作之前，它允许线程一直等待。

比如实现视频会议室系统

## 3.5、在集合点同步

CyclicBarrier,允许多个线程在某个集合点处相互等待。如何重置是通过reset（）方法完成的。预防损坏的CyclicBarrier,根据isBroken（）方法来判断。

## 3.6、并发阶段任务的运行

所有休眠线程以进行下一个阶段之前，必须执行arriveAndAwaitAdvance(),如果没有执行则可以通过arriveAndDeregister()。

Phaser对象有两种状态，活跃态（Active）,终止态（Termination

## 3.7、并发阶段任务中的阶段切换

也可以通过Phaser实现。

## 3.8、并发任务间的数据交换

只有一个生产者和一个消费者问题情境中，Exchanger可以使用。

# 第四章、线程执行器

## 4.1、简介

Java5开始提供的执行器框架：

线程池与之前相比，避免不断创建和销毁线程而导致系统性能下降。

Callable接口，类似Runnable接口，但是却提供了两方面的增强。

* 接口的主方法名称为call（）可以返回结果
* 当发送一个Callable对象给执行器时候，将获得一个实现Future接口的对象，可以使用这个对象来控制Callable对象的状态和结果。

## 4.2、创建线程执行器

ThreadPoolExecutor对象创建方式：

* 四个构造器
* Executors工厂类

执行器以及ThreadPoolExecutor类一个重要的特性，通常需要显示的结束它，否则程序不会结束。

<font color=red> newCachedThreadPool()方法创建的线程池，需要关注的使用场景：</font>

线程重用的优点是减少了创建新线程所花费的时间，然而新任务固定会依赖线程来执行，因此缓存线程池的缺点，如果发送过多的任务给执行器，那么系统的负荷则会过载。

## 4.3、创建固定大小的线程执行器

newFixedThreadPool()方法

原理：创建具有线程最大数量的执行器，如果发送超过线程数的任务给执行器，剩余的任务将被阻塞直到线程池里有空闲的线程来处理他们。

极端场景：

newSingleThreadExecutor（）方法

## 4.4、在执行器中执行任务并返回结果

Callable:这个接口声明call（）方法，可以在这个方法实现任务的具体逻辑操作。Callable接口是一个泛型接口，这意味着必须声明call（）方法的返回数据类型。

Future：这个接口声明了一些方法来获取由Callable对象产生的结果，并管理它们的状态。

原理：submit（）方法发送一个Callable对象给执行器执行， 这个su bmit()方法接受Callable对象作为参数，并返回Future对象。

Future两个目的：

* 控制任务的状态：可以取消任务和检查任务是否完成，isDone()方法。
* 通过call（）方法获取返回结果，可以使用get（）方法，这个方法是个阻塞方法，会一直等待Callable对象的call（）方法执行完成并返回结果。如果get（）方法在等待结果时候线程中断了，则将抛出InterruptedException异常，如果call（）方法抛出异常，那么get（）方法抛出Exe cu ti o n E x ce p ti o n异常。

## 4.5、运行多个任务并处理第一个结果

原理：ThreadPoolExecutor类的invokeAny()方法

## 4.6、运行多个任务并处理所有结果

原理：ExecutorService类的invokeAll()方法

想要等待任务结束，可以用2种方法：

* 如果任务执行结束，那么Future接口的isDone()方法返回true。
* 调用shutDown()方法后，ThreadPoolExecutor类的awaitTermination（）方法会将线程休眠，直到所有任务执行结束。

缺点：

* isDone()只可以控制任务完成与否
* shutDown()方法，必须关闭执行器来等待一个线程，否则调用这个方法线程将立即返回。

## 4.7、在执行器中延时执行任务

ScheduledThreadPoolExecutor执行器。

## 4.8、在执行器中周期性执行任务

同4.7

## 4.9、在执行器中取消任务

Future接口的cancel（）方法

## 4.10、在执行器中控制任务的完成

重写FutureTask类的done方法

## 4.11、在执行器中分离任务的启动与结果的处理

CompletionService类可以执行Callable或者Runnable类型任务。提供了2种方法获取数据

* poll()检查队列是否有Future对象，如果队列为空，则立即返回null。否则返回队列的第一个元素，并移除这个元素。
* take（）检查队列是否有Future对象，如果队列为空，阻塞线程直到队列有可用的原属。如果有，返回队列中第一个元素，并移除这个元素。

## 4.12、处理执行器中被拒绝的任务

实现RejectedExecutionHandler接口。

# 第五章、Fork/Join框架

## 5.1、简介

Java7在ExecutorService接口的另一种实现，用来解决特殊类型的问题，Fork/Join框架，分解合并框架。

解决的问题：

能够通过分治技术将问题拆分成小任务的问题。在一个任务中，先检查要解决的问题大小，如果大于一个设定的大小，那就将问题拆解。拆解的原则依赖于任务中处理的元素以及任务执行所需要的时间。

ForkJoinPool，一个特殊的Executor执行器类型：

* Fork操作：当需要将一个任务拆分更小的多个任务，在框架中执行这些小任务。
* Join操作：当一个主任务等待其创建的多个子任务的完成执行。

⚠️注意事项：

* 任务不能执行I/O操作，比如文件数据的读取与写入。
* 任务不能抛出非运行时异常（Checked Exception），必须在代码中处理掉这些异常。

Fork/Join框架的核心：

* ForkJoinPool：这个类实现ExecutorService接口和工作窃取算法。他管理工作者线程，并提供任务的状态信息，以及任务的执行信息。
* ForkJoinTask：ForkJoinPool中执行任务的基类。

如果需要实现Fork/Join任务，需要实现以下两个子类的其中一个：

* RecursiveAction:用于任务没有返回结果的场景。
* RecursiveTask:用于任务有返回结果的场景。

# 第六章、并发集合

## 6.1、简介

Java提供了2种用于并发应用的集合：

* 阻塞式集合，这类集合包括添加和移除数据的方法。当集合已满或者为空，被调用的添加或者移除方法就不能立即被执行，那么调用这个方法的线程将被阻塞，一直到该方法可以被成功执行。
* 非阻塞式集合，这类集合也包括添加和移除数据的方法，如果方法不能立即被执行，则返回null或者抛出异常，但是调用这个方法的线程不会被阻塞。

Java集合列表的使用场景：

* 非阻塞式列表对应的实现类：ConcurrentLinkedDeque类；
* 阻塞式列表对应的实现类：LinkedBlockingDeque类；
* 用于数据生成或消费的阻塞式列表对应的实现类：LinkedTransferQueue类；
* 按优先级排序列表元素的阻塞式列表对应的实现类：PriorityBlockingQueue类；
* 带有延迟列表元素的阻塞式列表对应的实现类：DelayQueue类；
* 非阻塞式可遍历映射对应的实现类：ConcurrentSkipListMap类；
* 随机数字对应的实现类：ThreadLocalRandom类；
* 原子变量对应的实现类：AtomicLong和AtomicIntegerArray类。

## 6.2、使用非阻塞式线程安全列表

## 6.3、使用阻塞式线程安全列表

## 6.4、使用按优先级排序的阻塞式线程安全列表

要求：所有添加进PriorityBlockingQueue的元素必须实现Comparable接口，这个接口提供了CompareTo()方法，他的传入参数时一个同类型的对象。这样可以使用2个同类型的对象进行比较，其中一个是执行这个方法的对象，另外一个是参数传入的对象。这个方法必须返回一个数字值，如果当前对象小于参数传入的对象，那么返回一个小于0的值；如果当前对象大于参数传入的对象，那么返回一个大于0的值，如果2个对象相等就返回0；

## 6.5、使用带有延迟元素的线程安全列表

要求：存放到DelayQueue类中的元素必须继承Delayed接口，该接口使对象成为延迟对象，使存放的对象有了激活日期，到激活日期的时间。

该接口强制执行下列2个方法：

* compareTo（Delayed o）
* getDelay(TimeUnit unit)

## 6.6、使用线程安全可遍历映射

跳表与二叉树效率相近

## 6.7、生成并发随机数

## 6.8、使用原子变量

## 6.9、使用原子数组

## 第七章、定制并发类

## 7.2、定制ThreadPoolExecutor类

## 7.3、实现基于优先级的Executor类

## 7.4、实现ThreadFactory接口生成定制线程

## 7.5、在Executor对象中使用ThreadFactory

## 7.6、定制运行在定时线程池中的任务

## 7.7、通过实现ThreadFactory接口为Fork/Join框架生成定制线程

## 7.8、定制运行Fork/Join框架中的任务

## 7.9、实现定制Lock类

## 7.10、实现基于优先级的传输队列

## 7.11、实现自己的原子对象

# 第八章、测试并发应用程序

## 8.2、监控Lock接口

## 8.3、监控Phaser类

## 8.4、监控执行器框架

## 8.5、监控Fork/Join池

## 8.6、输出高效日志

## 8.7、使用FindBugs分析并发代码

Checkstyle,PMD或者FindBugs

