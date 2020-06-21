# 前言

谷歌之路之算法基础

[http://alg4.cs.princeton.edu/](http://alg4.cs.princeton.edu/)

参考资料：https://algs4.cs.princeton.edu/home/

https://github.com/aistrate/AlgorithmsSedgewick

https://github.com/reneargento/algorithms-sedgewick-wayne

https://github.com/jimmysuncpt/Algorithms

动态可视化数据结构演示：

https://visualgo.net/zh/hashtable

学习策略：

https://zhangjia.io/628.html



# 一、基础

## 1.1、基础编程模型

基础编程模型的定义：语言特性+软件库+操作系统特性

### 1.1.1、Java程序的基本结构

* 原始数据类型
* 语句(声明，赋值，条件，循环，调用和返回)
* 数组
* 静态方法
* 字符串
* 标准输入、输出
* 数据抽象

### 1.1.2、原始数据类型与表达式

* 整型，及其算术运算符
* 双精度实数类型，及其算术运算符
* 布尔型及其逻辑操作符
* 字符型

#### 1.1.2.1、表达式

#### 1.1.2.2、类型转换

#### 1.1.2.3、比较

#### 1.1.2.4、其他原始类型

* 64位整数，及其算术运算符(long)
* 16位整数，及其算术运算符(short)
* 16位整数，及其算术运算符(char)
* 8位整数，及其算术运算符(byte)
* 32位单精度实数，及其算术运算符(float)

### 1.1.3、语句

#### 1.1.3.1、声明语句

Java是强类型的语言，因为编译器会检查类型的一致性。变量的作用域就是定义它的地方，声明语句将一个变量名和一个类型在编译时候关联起来。

#### 1.1.3.2、赋值语句

将某个数据类型的值和一个变量关联起来。

#### 1.1.3.3、条件语句

模板的形式记法去简化表达，eg:if (<boolean expression>){<block statements>}

#### 1.1.3.4、循环语句

#### 1.1.3.5、break与continue语句

* break语句从循环中立即退出
* continue语句立即开始下一轮循环

### 1.1.4、简便记法

#### 1.1.4.1、声明并初始化

#### 1.1.4.2、隐式赋值

++i=》i=i+1 ，i/2=》i=i/2

#### 1.1.4.3、单语句代码段

if (<BOOLEAN expression>) return;省略大括号

#### 1.1.4.4、for语句

### 1.1.5、数组

#### 1.1.5.1、创建并初始化数组

* 声明数组的名字和类型
* 创建数组
* 初始化数组元素

完整写法：

```java
			double [] a;

​			a = new double[N];

​			a[0]=1.0;
```



简化写法

```java
double [] a = new double[N];
```

声明初始化

```java
double [] a = {0.0,1.0,2.0}
```



#### 1.1.5.2、简化写法

#### 1.1.5.3、使用数组

#### 1.1.5.4、起别名

实际上是变量的引用赋值问题

#### 1.1.5.5、二维数组

### 1.1.6、静态方法

静态方法被称为函数。

#### 1.1.6.1、静态方法

每个静态方法都是由签名和函数体组成

#### 1.1.6.2、调用静态方法

#### 1.1.6.3、方法的性质

* 方法的参数按值传递
* 方法名可以被重载
* 方法只能返回一个值
* 方法可以产生副作用

#### 1.1.6.4、递归

* 递归总有一个最简单的情况退出条件——方法的第一条语句总是一个包含return的条件语句
* 递归调用总是尝试解决一个规模更小的子问题
* 递归调用的父问题和尝试解决的子问题不应该有交集

#### 1.1.6.5、基础编程模型

静态方法库是定义在一个Java类中的一组静态方法。

#### 1.1.6.6、模块化编程

* 程序的整体代码量很大，每次处理的模块大小适中
* 共享和重用代码无需重新实现
* 改进的实现替换老的实现
* 解决编程问题建立合适的抽象模型
* 缩小调试范围

#### 1.1.6.7、单元测试

Java提供的main方法

#### 1.1.6.8、外部库

Eg:java.lang.*

### 1.1.7、API

模块化编程，我们应该将自己编写的每一个程序都当做一个日后可以重用的库。

* 编写用例，在实现中将计算过程分解成可控的部分
* 明确静态方法库和与之对应的API
* 实现API和一个能够对方法进行独立测试的main()函数

API的目的是将调用和实现分离。除了API给出的信息，调用者不需要知道实现的其他细节。

### 1.1.8字符串

#### 1.1.8.1、字符串拼接

+ +拼接

#### 1.1.8.2、类型转换

String的转化API

#### 1.1.8.3、自动转换

拼接字符串的时候自动转化

#### 1.1.8.4、命令行参数

### 1.1.9、输入输出

#### 1.1.9.1、命令和参数

javac 编译

java 执行

more 其他

#### 1.1.9.2、标准输出

StdOut.println();

#### 1.1.9.3、格式化输出

d:十进制数

f:浮点数

s:字符串

#### 1.1.9.4、标准输入

StdIn

#### 1.1.9.5、重定向与管道（终端适用）

输出信息到某个文件

Java clazz 1 2 3 > data.txt

从某个文件输入信息

Java clazz < data.txt

将一个程序的输出重定向为另一个程序的输入叫做管道。| 

#### 1.1.9.6、基于文件的输入输出

#### 1.1.9.7~1.1.9.9、绘图相关

### 1.1.10、二分查找

#### 1.1.10.1~2、二分法举例和开发用例

![image-20200425151633940](/Users/xiaochengliu/Library/Application Support/typora-user-images/image-20200425151633940.png)

#### 1.1.10.3~4、白名单过滤举例和性能的引申

上百万的白名单或者上千万的白名单过滤，需要归并排序和二分法排序提升性能。

### 1.1、课后题：

Q1： 什么是Java的字节码？

A1： 它是程序的一种低级表示，可以运行在Java的虚拟机。将程序抽象为字节码可以保证Java程序员的代码能够运行在各种设备上。

Q2：Java允许整型溢出并返回错误值的做法是错误的。难道Java不应该自动检查溢出么？

A2：这个问题在程序员中一直有争议，简单的回答是他们之所以被称为<font color=red>原始数据类型就是因为缺乏此类检查</font>。避免此类问题并不需要很高深的知识。我们会使用int类型表示较小的数（小于10个十进制位）而使用long表示10亿以上的数。

Q3:  Math.abs(-2147483648)的返回值是什么？

A3：-2147483648，因为整数溢出了。

Q4： 如何才能将一个double的初始值为无穷大

A4:    Double.POSITIVE_INFINITY和Double.NEGATIVE_INFINITY

Q5:   double和int类型相互比较可以么？

A5：原始数据类型是不行的，所以需要类型转化。

Q6：如果使用一个变量前没有将它初始化，会发生什么？

A6：编译异常抛出

Q7： 表达式1/0和1.0/0.0的值是什么？

A7：第一个表达式会产生一个运行时除以0异常，第二个表达式的值无穷大。

Q8：能够使用< 和 >比较String变量么？

A8： 不行，只有原始数据类型定义了这些运算符。

Q9：负数的除法和余数的结果是什么？

A9：表达式a/b的商会向0取整，-14/3 =14/-3=4, 然而-14%3= -2，14%-3=2

Q10：&&和&的区别？

A10：&&从左向右断路，如果左边不满足 就不会进行到右边。&是逻辑操作与

Q11：if的二义性

A11：可以嵌套使用最好是加上大括号

Q12：for循环和while循环区别

A12：什么时候使用while，比如递增变量在循环结束之后仍然可以使用。其余时候相同。

Q13：int a[]和int [] a的区别？

A13：第一种是Java支持C语言中数组的声明方式，第二种是Java提倡的方式。

Q14:  为什么数组的起始索引是0而不是1？

A14：习惯起源于机器语言是从0开始的索引地址。

Q15：在Java中，一个静态方法能够将另一个静态方法作为参数么？

A15：不可以

## 1.2、数据抽象

数据类型指的是一组值和一组对这些值操作的集合。

Java编程的基础主要是使用class关键字构造被称为引用类型的数据类型。这种编程风格也称为面向对象编程。因为核心概念是对象，即保存了某个数据类型的值的实体。如果只有Java的原始数据类型，我么的程序会在很大程度上被限制，但有了引用类型，我们就能编写操作字符串，图像，声音以及Java的标准库中或者本书的网站上的数百种抽象类型的程序。比其他库相对而言，Java编程的数据类型的种类是无限的，因为你能够定义自己的数据类型来抽象任意对象。<font color=red>java编程的一大特色</font>

​	抽象数据类型（ADT）是一种能够对使用者隐藏数据表示的数据类型。抽象数据类型之所以重要是因为在程序设计上它们支持封装。可以在不修改任何用例代码的情况下用一种算法替换另一种算法并改进所有用例的性能。

### 1.2.1、使用抽象数据类型

要使用一种数据类型并不一定非得知道它是如何实现的。

#### 1.2.1.1、抽象数据类型的API

使用应用程序编程接口（API）来说明抽象数据类型的行为。它将列出所有构造函数和实例方法（即操作）并简要描述它们的功用。

#### 1.2.1.2、继承的方法

任意数据类型都能通过在API中包含特定的方法从Java内在机制中获取比如toString(),equals(),compareTo(),hashCode()等。

#### 1.2.1.3、用例代码

和基于静态方法的模块化编程类型相似，API封装内部实现，让上游不感知。

#### 1.2.1.4、对象

对象是能够承载数据类型的值的实体。所有对象都有三大重要特性：状态，标识和行为。

* 状态：数据类型的值
* 标识：将一个对象区别于另一个对象，实际上就是内存的地址引用
* 行为：数据类型的操作。

引用是访问对象的一种实现方法，不同的对象的区别，实际上是不同的内存地址引用。

#### 1.2.1.5、创建对象

new（）构建对象都做了什么？

* 为新的对象分配内存空间
* 调用构造函数初始化对象中的值
* 返回该对象的一个引用

#### 1.2.1.6、调用实例方法

方法的每次触发都是和一个对象相关的。

但是静态方法的主要作用是实现函数；非静态方法的主要作用是实现数据类型的操作。

#### 1.2.1.7、使用对象

* 声明该类型的变量，以用来引用对象；比如long userId;
* 使用关键字new触发能够创建该类型的对象的一个构造函数；long userId = new long();
* 使用变量名在语句或者表达式中调用实例方法。long userId = getUserIdFromDb();

#### 1.2.1.8、赋值语句

使用引用类型的赋值语句将会创建该引用的一个副本。赋值语句不会创建新的对象，而只是创建另一个指向某个已经存在的对象的引用。这种情况被称为别名。两个变量同时指向同一个对象。

long userId = 1L;

long userIdBackUp = userId;(这里并没创建新的对象，而是userId的引用地址赋值给了userIdBackUp，所以userIdBackUp和userId都指向了同一个内存地址)

#### 1.2.1.9、将对象作为参数

* 值传递：Java将参数值的一个副本从调用端传给了方法，这种方式称为值传递。math（1，2）；
* 引用传递：使用引用类型作为参数传递对象的引用。modifyMath(Integer a){ a = 2;}

#### 1.2.1.10、将对象作为返回值

因为Java只能返回一个返回值，所以如果有多个返回值需要返回，我们需要包装成一个返回对象。而这个也比较被人诟病，所以go语言可以支持多个返回值。

#### 1.2.1.11、数组也是对象

#### 1.2.1.12、对象的数组

数组元素可以是任意类型的数据。创建一个对象的数组需要以下2个步骤：

- 使用方括号语法调用数组的构造函数创建数组
- 对于每个数组元素调用它的构造函数创建相应的对象

在Java中，对象数组即是一个由对象的引用组成的数组，而非所有对象本身组成的数组。

如果对象非常大，那么操作引用是提升效率的，如果对象非常小，那么每次获取信息通过引用的方式反而是降低效率的。

面向对象编程：运用数据抽象的思想编写代码（定义和使用数据类型，将数据类型的值封装在对象中）的方式。

### 1.2.2、抽象数据类型举例

#### 1.2.2.1、几何对象

Java数据类型：

* Java.awt.Color颜色
* Java.awt.Font字体
* Java.net.URL
* Java.io.File

标准IO类型

* In
* Out
* Draw

面向数据的数据类型

* Point2D   平面上的点

* Interval1D 一维间隔

* Interval2D 二维间隔
* Date 日期
* Transaction 事务

算法分析的数据类型

* Counter 计数器
* Accumulator 累加器
* VisualAccumulator 可视累加器
* StopWatch  计时器

集合数据类型

* Stack 下压栈
* Queue FIFO先进先出队列
* Bag  包
* MinPQ，MaxPQ 优先队列
* IndexMinPQ，IndexMaxPQ 索引优先队列
* ST 符号表
* SET 集合
* StringST 符号表

面向数据的图数据类型

* Graph 无向图
* Digraph 有向图
* Edge 边
* EdgeWeightedGraph 无向图
* DirectedEdge 边
* EdgeWeightedDigraph 图

面向操作的图数据类型

* UF 动态连通性
* DepthFirstPaths 路径的深度优先搜索
* CC  连通分量
* BreadFirstPaths  广度优先搜索
* DirectedDFS  有向图深度优先搜索
* DirectedBFS 有向图广度优先搜索
* TransitiveClosure 所有路径
* Topological 拓扑排序
* DepthFirstOrder 深度优先搜索顶点被访问的顺序
* DirectedCycle 环的搜索
* SCC 强连通分量
* MST 最小生成树
* SP 最短路径

#### 1.2.2.2、信息处理

遇到逻辑上相关的不同类型的数据时，都可以定义一个抽象数据类型。

#### 1.2.2.3、字符串

非常丰富的API

### 1.2.3、抽象数据类型的实现

如何编写Java代码，构造对象，封装函数，编写API，断言和异常的使用。

## 1.3、背包、队列和栈

许多基础数据类型都和对象的集合有关。数据类型的值就是一组对象的集合，所有操作都是关于添加，删除或者是访问集合中的对象。

目标导向：

* 目标1：对集合中的对象的表示方式将直接影响各种操作的效率。
* 目标2：泛型和迭代如何协助我们写出更加清晰，简洁和优美的用例。
* 目标3：链式数据结构的重要性，理解链表是学习各种算法和数据结构中的最关键的一步。

### 1.3.1、API

#### 1.3.1.1、泛型

集合类的抽象数据类型的一个关键特性是我们应该可以用它们存储任意类型的数据。Java是通过泛型，也叫做参数化类型来定义的。相当于一个象征性的占位符，表示的用例会使用的某种具体数据类型。通过泛型我们可以只提供一份API就能够处理所有类型的数据，甚至是未来定义的未知类型。

#### 1.3.1.2、自动装箱

类型参数必须被实例化为引用类型。但是对于Java的基本数据类型的包装类型，可以在基本数据类型和包装类之间自动转化。

自动装箱：自动将一个原始数据类型转换为一个封装类型被称为自动装箱。

自动拆箱：自动将一个封装类型转为一个原始数据类型被称为自动拆箱。

#### 1.3.1.3、可迭代的集合类型

迭代访问集合，类型一致的放入背包中，可以通过类似foreach语句来编列元素。

#### 1.3.1.4、背包🎒

背包是一种不支持从中删除元素的集合数据类型-目的就是帮助用例收集元素并迭代便利所有收集到的元素（用例也可以检查背包是否为空或者获取背包中元素的数量）。

```java
/**
 * 
 *  The {@code Bag} class represents a bag (or multiset) of
 *  generic items. It supports insertion and iterating over the
 *  items in arbitrary order.
 *
 *  PS： 中文翻译： Bag类代表了一个通用元素的背包或者多个集合。它提供以任意顺序方式插入和遍历元素
 *  <p>
 *  This implementation uses a singly linked list with a static nested class Node.
 *  See {@link LinkedBag} for the version from the
 *  textbook that uses a non-static nested class.
 *  See {@link ResizingArrayBag} for a version that uses a resizing array.
 *  The <em>add</em>, <em>isEmpty</em>, and <em>size</em> operations
 *  take constant time. Iteration takes time proportional to the number of items.
 *
 *  使用一个单个链表和一个静态的嵌套类Node实现。
 *  LinkedBag为使用非静态嵌套类实现
 *  ResizingArrayBag为使用可重定义数组实现
 *  这添加 <em>add</em>, 判断是否为空<em>isEmpty</em>, 和大小的<em>size</em> 操作花费常量时间。迭代花费元素数量时间O(N)
 *  <p>
 *  For additional documentation, see <a href="https://algs4.cs.princeton.edu/13stacks">Section 1.3</a> of
 *  <i>Algorithms, 4th Edition</i> by Robert Sedgewick and Kevin Wayne.
 *
 *  另外的文档说明，可以看算法第四版的<a href="https://algs4.cs.princeton.edu/13stacks">Section 1.3</a>章节
 *
 *  @author Robert Sedgewick
 *  @author Kevin Wayne
 *
 *  @param <Item> the generic type of an item in this bag
 */
public class Bag<Item>  implements Iterable<Item>{
    private Node<Item> first;    // 背包开始节点
    private int n;               // 背包中的元素数

    // 单个静态内部链表类
    private static class Node<Item> {
        private Item item;
        private Node<Item> next;
    }
    /**
     * 空构造函数
     */
    Bag(){
        first = null;
        n = 0;
    }


    /**
     * 添加元素到背包
     *
     * @param  item the item to add to this bag
     */
    public void add(Item item) {
        Node<Item> oldfirst = first;
        first = new Node<Item>();
        first.item = item;
        first.next = oldfirst;
        n++;
    }

    /**
     * 返回背包元素是否为空
     *
     * @return {@code true} if this bag is empty;
     *         {@code false} otherwise
     */
    boolean isEmpty(){
        return first == null;
    }

    /**
     * 返回背包元素集合大小
     *
     * @return the number of items in this bag
     */
    int size(){
        return n;
    }
    /**
     * Returns an iterator that iterates over the items in this bag in arbitrary order.
     * 以任意顺序返回背包中的迭代元素
     * @return an iterator that iterates over the items in this bag in arbitrary order
     */
    public Iterator<Item> iterator()  {
        return new LinkedIterator(first);
    }

    /**
     * 链表迭代器，因为remove为可选操作，实现迭代器背包的时候不支持这种操作
     */
    private class LinkedIterator implements Iterator<Item> {
        private Node<Item> current;

        public LinkedIterator(Node<Item> first) {
            current = first;
        }

        public boolean hasNext()  { return current != null;                     }
        public void remove()      { throw new UnsupportedOperationException();  }

        public Item next() {
            if (!hasNext()) throw new NoSuchElementException();
            Item item = current.item;
            current = current.next;
            return item;
        }
    }
    /**
     * Unit tests the {@code Bag} data type.
     *
     * @param args the command-line arguments
     */
    public static void main(String[] args) {
        Bag<String> bag = new Bag<String>();
        while (!StdIn.isEmpty()) {
            String item = StdIn.readString();
            bag.add(item);
        }

        StdOut.println("size of bag = " + bag.size());
        for (String s : bag) {
            StdOut.println(s);
        }
    }
}
```

#### 1.3.1.5、先进先出队列🚄

先进先出队列是一种基于先进先出FIFO策略的集合类型。使用场景：服务性策略基本原则是公平，优先服务等待最久的。

![image-20200620170903461](/Users/xiaochengliu/Library/Application Support/typora-user-images/image-20200620170903461.png)



排队0号的顾客已经可以进去吃饭或者看电影了，那么0既可以不用排队了，但是其他人还是保持原来顺序，只是被叫号提前了一个位置。

![image-20200620170936543](/Users/xiaochengliu/Library/Application Support/typora-user-images/image-20200620170936543.png)



#### 1.3.1.6、下压栈📃

下压栈简称为栈，比如邮件或者文件。后进先出

![image-20200620172809769](/Users/xiaochengliu/Library/Application Support/typora-user-images/image-20200620172809769.png)

现在我正好有时间空闲处理文件或者查看邮件则我处理55最后进来的文件。

![image-20200620172918685](/Users/xiaochengliu/Library/Application Support/typora-user-images/image-20200620172918685.png)

应用场景，如果让你实现一个浏览器页面可以自动回退以及Java线程调用的方式。都是可以通过栈来实现。

小疑问：如何实现算术运算表达式？

A： 可以使用操作数栈和运算符栈一起使用。

比如（ 1 + ( ( 2+3 ) * (4 * 5) ) ）

执行顺序：该算法名称为Dijkstra(迪杰斯特拉)双栈算术表达式求值算法的轨迹。也可以通过一个栈来实现，比如之前写的博客中的一个<a href ="https://blog.csdn.net/wolf_love666/article/details/90760734">栈运算求值。</a>

![image-20200621100514449](/Users/xiaochengliu/Library/Application Support/typora-user-images/image-20200621100514449.png)

### 1.3.2、集合类数据类型的实现

#### 1.3.2.1、定容栈

容量固定的字符串栈的抽象数据类型。要求：只能处理String类型，并且指定一个容量且不支持迭代。参考代码：com.xiaochengxinyizhan.datastructure.stack.FixedCapacityStackOfStrings

#### 1.3.2.2、泛型

由于数组必须保证类型一致，如果想抽象出泛型来提供不同类型支持可以使用如下方式：

a = (Item []) new Object[cap];

泛型类型使用Item表示

#### 1.3.2.3、调整数组大小

选择用数组表示栈内容意味着用例必须预先估计栈的最大容量。在Java中，数组一旦创建其大小是无法改变的，因此栈使用的空间只能是这个最大容量的一部分。所以如何不浪费，就需要动态扩容。比如之后Java实现的ArrayList，HashMap都是该原理。

使用率保证不会低于1/4,栈永远不会溢出，使用率高于1/2扩容。

这里有个优化的问题就是：不断的扩容缩容实际上也是在增加系统开销。增加GC垃圾回收。

#### 1.3.2.4、对象游离

Java的垃圾回收策略是回收所有无法被访问的对象的内存。在我们对POP()的实现后，被弹出的元素引用仍然存在数组中。虽然已经不会被访问，但是GC不知道这个事情。这种情况（保存一个不需要的对象的引用）称为游离。

那么如何避免这种对象游离呢，我们需要将该元素设置为null。这将会覆盖无用的引用并使系统可以在用例使用完被弹出的元素后回收它的内存。

所有集合类型抽象数据类型实现的模板都可参考：com.xiaochengxinyizhan.datastructure.stack.ResizingArrayStack

### 1.3.3、链表

链表定义：

<font color =red>链表是一种递归的数据结构，它或者为空（null），或者是指向一个结点（node）的引用，该结点含有一个泛型的元素和一个指向另一条链表的引用。</font>

<font color = green>结点是一个可能含有任意类型数据的抽象实体，它所包含的指向结点的应用显示了它在构造链表中的作用。</font>

#### 1.3.3.1、结点记录

面向对象中，实现链表可以用嵌套类来定义结点的抽象数据类型：

```java
private class Node{
Item item;
Node next;
}
```

一个Node对象含有两个实例变量，类型分为Item(参数类型)和Node。申明的时候会用private来修饰类，因为是不对外API暴露的，使用我们可以通过无参构造函数new Node（）创建。

#### 1.3.3.2、构造链表

#### 1.3.3.3、在表头插入结点

#### 1.3.3.4、从表头删除结点

#### 1.3.3.5、在表尾插入结点

上述内容1.3.3.2~1.3.3.5可以参考<a href="https://blog.csdn.net/wolf_love666/article/details/94616818">博客了解链表的构成操作</a>

#### 1.3.3.6、在其他位置的插入和删除操作

一种方案是我们通过遍历链表找到元素的位置然后进行插入和删除操作。但是很明显所需的时间复杂度和链表的长度成正比。<font color=red>实现任意的插入和删除操作的标准解决方案是使用双向链表。</font>

#### 1.3.3.7、遍历

所有的数据集合都需要有遍历的能力。

#### 1.3.3.8、栈的实现

栈的实现可以基于单向链表实现。push则添加表头，pop则从表头取出。

链表的使用达到了我们最优的设计目标：

* 可以处理任意类型的数据
* 所需的空间总是和集合大小成正比
* 操作所需的时间总是和集合大小无关

#### 1.3.3.9、队列的实现

队列的实现依然是链表实现，先进先出，单向队列。链表的使用达到了我们最优的设计目标：

* 可以处理任意类型的数据
* 所需的空间总是和集合大小成正比
* 操作所需的时间总是和集合大小无关

#### 1.3.3.10、背包的实现

对于栈，链表的访问顺序是后进先出，对于队列，链表的访问顺序是先进先出。对于背包，它的顺序也是后进先出的顺序。

### 1.3.4、综述

顺序存储：数组

链式存储：链表

![image-20200621113421088](/Users/xiaochengliu/Library/Application Support/typora-user-images/image-20200621113421088.png)

关于数据结构的入门了解可以参考之前<a href="https://xiaochengxinyizhan.blog.csdn.net/article/details/90760734">笔记博客</a>

新的应用领域研究步骤：

* 定义API
* 根据特定的应用场景开发用例代码
* 描述一种数据结构（一组值的表示），并在API所对应的抽象数据类型的实现中根据它定义类的实例变量。
* 描述算法（实现一组操作的方式），并根据它实现类的实例方法
* 分析算法的性能特点

最后一步，分析算法的性能特点常常能够决定哪种算法和实现才是解决现实应用问题的最佳选择。

比如数据结构举例：

|       数据结构       |   章节   |      抽象数据类型      |      数据表示      |
| :------------------: | :------: | :--------------------: | :----------------: |
|       父链接树       |   1.5    |       UnionFind        |      整型数组      |
|      二分查找树      | 3.2，3.3 |          BST           | 含有两个链接的结点 |
|        字符串        |   5.1    |         String         | 数组，偏移量和长度 |
|        二叉堆        |   2.4    |           PQ           |      对象数组      |
|   散列表（拉链法）   |   3.4    | SeparateChainingHashST |      链表数组      |
| 散列表（线性探测法） |   3.4    |  LinearProbingHashST   |    两个对象数组    |
|     图的邻接链表     | 4.1，4.2 |         Graph          |    Bag对象数组     |
|      单词查找树      |   5.2    |         TrieST         | 含有链接数组的结点 |
|    三向单词查找树    |   5.3    |          TST           | 含有三个链接的结点 |

### 答疑：

Q：并不是所有编程语言都支持泛型，甚至早期Java也不支持，替代方案是什么？

A：每种类型的数据都实现一个不同的集合数据类型。或者使用Object对象的栈，并在用例中使用pop（）时候将得到的对象转为所需要的数据类型。这种方式会有类型不匹配错误只能在运行时候发现。

Q：为什么Java不允许泛型数组？

A：目前是不支持的，可以先理解共变数组和类型擦除

Q：如何能创建一个字符串栈的数组？

A：使用类型转换，比如

```java
Stack<String>[] a = (Stack<String>[])new Stack[N];
```

Q:为什么将Node声明为嵌套类？为什么使用private？

A：将Node声明为私有的嵌套类之后，我们可以将Node方法和实例变量的访问范围限制在包含他的类中。私有嵌套类的一个特点是只有包含它的类能够直接访问它的实例变量，因此无需将它的实际变量声明为public或者private。非静态的嵌套类也被称为内部类。

Q：我们能够用foreach循环访问字符串么？

A: 不行，String没有实现Iterable接口。

课后题：

页码：page104



## 1.4、算法分析

我的程序会运行多长时间，为什么我的程序耗尽所有的内存。

### 1.4.1、科学方法

* 细致的观察真实世界的特点，通常还要有精确的测量
* 根据观察结果提出假设模型
* 根据模型预测未来的事件
* 继续观察并核实预测的准确性
* 如此反复直到确认预测和观察一致

### 1.4.2、观察

* 如何定量测量程序的运行时间

#### 1.4.2.1、比如求三数之和为0的元组的数量

```java
/******************************************************************************
 *  Compilation:  javac ThreeSum.java
 *  Execution:    java ThreeSum input.txt
 *  Dependencies: In.java StdOut.java Stopwatch.java
 *  Data files:   https://algs4.cs.princeton.edu/14analysis/1Kints.txt
 *                https://algs4.cs.princeton.edu/14analysis/2Kints.txt
 *                https://algs4.cs.princeton.edu/14analysis/4Kints.txt
 *                https://algs4.cs.princeton.edu/14analysis/8Kints.txt
 *                https://algs4.cs.princeton.edu/14analysis/16Kints.txt
 *                https://algs4.cs.princeton.edu/14analysis/32Kints.txt
 *                https://algs4.cs.princeton.edu/14analysis/1Mints.txt
 *
 *  A program with cubic running time. Reads n integers
 *  and counts the number of triples that sum to exactly 0
 *  (ignoring integer overflow).
 *
 *  % java ThreeSum 1Kints.txt 
 *  70
 *
 *  % java ThreeSum 2Kints.txt 
 *  528
 *
 *  % java ThreeSum 4Kints.txt 
 *  4039
 *
 ******************************************************************************/

/**
 *  The {@code ThreeSum} class provides static methods for counting
 *  and printing the number of triples in an array of integers that sum to 0
 *  (ignoring integer overflow).
 *  <p>
 *  This implementation uses a triply nested loop and takes proportional to n^3,
 *  where n is the number of integers.
 *  <p>
 *  For additional documentation, see <a href="https://algs4.cs.princeton.edu/14analysis">Section 1.4</a> of
 *  <i>Algorithms, 4th Edition</i> by Robert Sedgewick and Kevin Wayne.
 *
 *  @author Robert Sedgewick
 *  @author Kevin Wayne
 */
public class ThreeSum {

    // Do not instantiate.
    private ThreeSum() { }

    /**
     * Prints to standard output the (i, j, k) with {@code i < j < k}
     * such that {@code a[i] + a[j] + a[k] == 0}.
     *
     * @param a the array of integers
     */
    public static void printAll(int[] a) {
        int n = a.length;
        for (int i = 0; i < n; i++) {
            for (int j = i+1; j < n; j++) {
                for (int k = j+1; k < n; k++) {
                    if (a[i] + a[j] + a[k] == 0) {
                        StdOut.println(a[i] + " " + a[j] + " " + a[k]);
                    }
                }
            }
        }
    } 

    /**
     * Returns the number of triples (i, j, k) with {@code i < j < k}
     * such that {@code a[i] + a[j] + a[k] == 0}.
     *
     * @param  a the array of integers
     * @return the number of triples (i, j, k) with {@code i < j < k}
     *         such that {@code a[i] + a[j] + a[k] == 0}
     */
    public static int count(int[] a) {
        int n = a.length;
        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i+1; j < n; j++) {
                for (int k = j+1; k < n; k++) {
                    if (a[i] + a[j] + a[k] == 0) {
                        count++;
                    }
                }
            }
        }
        return count;
    } 

    /**
     * Reads in a sequence of integers from a file, specified as a command-line argument;
     * counts the number of triples sum to exactly zero; prints out the time to perform
     * the computation.
     *
     * @param args the command-line arguments
     */
    public static void main(String[] args)  { 
        In in = new In(args[0]);
        int[] a = in.readAllInts();

        Stopwatch timer = new Stopwatch();
        int count = count(a);
        StdOut.println("elapsed time = " + timer.elapsedTime());
        StdOut.println(count);
    } 
} 

```

#### 1.4.2.2、计时器

#### 1.4.2.3、实验数据的分析

![image-20200621211733792](/Users/xiaochengliu/Library/Application Support/typora-user-images/image-20200621211733792.png)



### 1.4.3、数学模型

一个程序运行的总时间主要和2点有关：

* 执行每条语句的耗时（取决于计算机，Java编译器和操作系统）
* 执行每条语句的频率（程序本身和输入）

#### 1.4.3.1、近似

定义：我们用~f(N)表示所有随着N的增大除以f(N)的结果趋近于1的函数。我们用g(N)~f(N)表示g(N)/f(N)随着N的增大趋近于1.

#### 1.4.3.2、近似运行时间

![image-20200621212510254](/Users/xiaochengliu/Library/Application Support/typora-user-images/image-20200621212510254.png)

#### 1.4.3.3、对增长数量级的猜想

程序运行的时间分析，将语句块拆分，运行时间以s来标记，根据执行的频率，来计算总时间。

#### 1.4.3.4、算法的分析

经典算法的性能理论大部分都发表数十年前，但是仍然适用于今天的计算机。

所以我们需要抽象世界中的一个Java程序和真实世界中的运行联系起来。

#### 1.4.3.5、成本模型

可以通过数学的成本模型来评估算法的性质。这个模型定义了我们所研究的算法中的基本操作。

比如3-sum问题的成本模型是我们访问数组元素的次数。

#### 1.4.3.6、总结

对于大多数程序，得到其运行时间的数学模型所需的步骤如下：

* 确定输入模型，定位问题的规模
* 识别内循环
* 根据内循环中的操作确定成本模型
* 对于给定的输入，判断这些操作的执行频率，这可能需要用数学分析。

比如二分查找，输入模型是是大小为N的数组a[],内循环是一个while循环中的所有语句，成本模型是比较操作（比较2个数组元素的值），所以我们能判断是比较次数最多是logN+1。

比如白名单，输入模型是白名单的大小N和由标准输入得到的M个整数，且我们假设M>>N内循环是一个while循环中的所有语句，成本模型是比较操作（承自二分查找），所以我们比较次数最多为M（logN+1） 

### 1.4.4、增长数量级

#### 1.4.4.1、常数级别

完成任务所需的操作次数一定，因此运行时间不依赖于N。大多数的java操作所需的时间均为常数。

#### 1.4.4.2、对数级别

运行时间的增长数量级为对数的程序仅为常数时间的程序稍慢。经典例子就是二分查找。对数的底数和增长的数量无关（因为不同的底数仅相当于一个常数因子），所以我们说的对数级别一般使用logN



# 二、排序



# 三、查找



# 四、图



# 五、字符串

