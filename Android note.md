# Android note

## `synchronized` `volatile`用法，两者的区别和场景

`volatile`变量可以被看作是一种程度较轻的`synchronized`。与 synchronized 块相比，volatile 变量所需的编码较少，并且运行时开销也较少，但是它所能实现的功能也仅是 synchronized 的一部分

`volatile` 变量具有 `synchronized` 的可见性特性，但是不具备原子特性。这就是说线程能够自动发现 volatile 变量的最新值。

正确使用`volatile`	变量的条件

- 对变量的写操作不依赖于当前值
- 该变量没有包含在具有其他变量的不变式中

第一个条件的限制使 volatile 变量不能用作线程安全计数器。

结合使用 volatile 和 synchronized 实现 “开销较低的读－写锁”

```java
public class CheesyCounter {
    // Employs the cheap read-write lock trick
    // All mutative operations MUST be done with the 'this' lock held
    @GuardedBy("this") private volatile int value;
 
    public int getValue() { return value; }
 
    public synchronized int increment() {
        return value++;
    }
}
```

`synchronized`是`Java`中的关键字，是一种同步锁。它修饰的对象有以下几种：
1. 修饰一个代码块，被修饰的代码块称为同步语句块，其作用的范围是大括号{}括起来的代码，作用的对象是调用这个代码块的对象；
2. 修饰一个方法，被修饰的方法称为同步方法，其作用的范围是整个方法，作用的对象是调用这个方法的对象；
3. 修饰一个静态的方法，其作用的范围是整个静态方法，作用的对象是这个类的所有对象；
4. 修饰一个类，其作用的范围是`synchronized`后面括号括起来的部分，作用主的对象是这个类的所有对象。

### reference
[正确使用 Volatile 变量](https://www.ibm.com/developerworks/cn/java/j-jtp06197.html)


## jvm相关和GC回收算法区别

垃圾回收常用算法

- 引用计数法(Reference Counting)

引用计数器的实现很简单，对于一个对象 A，只要有任何一个对象引用了 A，则 A 的引用计数器就加 1，当引用失效时，引用计数器就减 1。只要对象 A 的引用计数器的值为 0，则对象 A 就不可能再被使用。

引用计数器的实现也非常简单，只需要为每个对象配置一个整形的计数器即可。但是引用计数器有一个严重的问题，即无法处理循环引用的情况。因此，在 Java 的垃圾回收器中没有使用这种算法。

- 标记-清除算法(Mark-Sweep)

标记-清除算法将垃圾回收分为两个阶段：标记阶段和清除阶段。一种可行的实现是，在标记阶段首先通过根节点，标记所有从根节点开始的较大对象。因此，未被标记的对象就是未被引用的垃圾对象。然后，在清除阶段，清除所有未被标记的对象。该算法最大的问题是存在大量的空间碎片，因为回收后的空间是不连续的。在对象的堆空间分配过程中，尤其是大对象的内存分配，不连续的内存空间的工作效率要低于连续的空间。

- 复制算法(Coping)

将现有的内存空间分为两快，每次只使用其中一块，在垃圾回收时将正在使用的内存中的存活对象复制到未被使用的内存块中，之后，清除正在使用的内存块中的所有对象，交换两个内存的角色，完成垃圾回收。

- 标记-压缩算法(Mark-Compact)

- 增量算法(Incremental Colleting)

- 分代(Generational Collecting)



### reference

[JVM 垃圾回收器工作原理及使用实例介绍](https://www.ibm.com/developerworks/cn/java/j-lo-JVMGarbageCollection/)

## EventBus 实现原理和观察者模式在开发中的运用

### 观察者模式
- 观察者模式定义对象一对多关系，当对象的状态发生改变时，其相关依赖对象得到通知。观察者模式又叫发布-订阅模式、模型-视图模式、源-监听器模式或者从属模式。
- 观察者模式定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个目标对象，当这个目标对象的状态发生变化时，会通知所有观察者对象，使它们能够自动更新。
- 观察者模式的主要优点在于可以实现表示层和数据逻辑层的分离，并在观察目标和观察者之间建立一个抽象的耦合，支持广播通信；其主要缺点在于如果一个观察目标对象有很多直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间，而且如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。
- 观察者模式适用情况包括：一个抽象模型有两个方面，其中一个方面依赖于另一个方面；一个对象的改变将导致其他一个或多个对象也发生改变，而不知道具体有多少对象将发生改变；一个对象必须通知其他对象，而并不知道这些对象是谁；需要在系统中创建一个触发链。
- 在JDK的java.util包中，提供了Observable类以及Observer接口，它们构成了Java语言对观察者模式的支持。

### reference

[观察者模式](http://design-patterns.readthedocs.io/zh_CN/latest/behavioral_patterns/observer.html)

## https相关，如何验证证书的合法性，https中哪里用了对称加密，哪里用了非对称加密，两者的区别

### reference

- [如何验证证书的合法性](https://www.zhihu.com/question/37370216)

## 进程保活


### reference 
- [Android 进程保活招式大全]{https://segmentfault.com/a/1190000006251859}

## 进程共享和线程安全问题

## 动态代理模式如何运用？

### reference
- [Java 动态代理机制分析及扩展，第 1 部分](https://www.ibm.com/developerworks/cn/java/j-lo-proxy1/)

## App 是如何沙箱化，为什么要这么做



## TheadLocal 原理

ThreadLocal并不是用来并发控制访问一个共同对象，而是为了给每个线程分配一个只属于该线程的变量，顾名思义它是local variable（线程局部变量）。它的功用非常简单，就是为每一个使用该变量的线程都提供一个变量值的副本，是每一个线程都可以独立地改变自己的副本，而不会和其它线程的副本冲突，实现线程间的数据隔离。从线程的角度看，就好像每一个线程都完全拥有该变量

### reference

[ThreadLocal 内部实现、应用场景和内存泄漏](http://blog.csdn.net/u012834750/article/details/71646700)

[不共享有时是最好的](https://www.ibm.com/developerworks/cn/java/j-threads/index3.html)
## classloader

## 泛型


## 内部类、静态内部类、匿名内部类

在需要一个与定义它的类紧密耦合的类时，应该使用嵌套类

内部类实例化例子

```java
class Out {
 
  class Inner {
  
  }
 
  public static void mian(String[] args) {
    Out out = new Out();
    Out.Inner inner = out.new Inner();// 注意内部类只能用对象new
  }
}
```

### reference 

[嵌套类](https://www.ibm.com/developerworks/cn/java/j-perry-nested-classes/index.html)

## ArrayList和LinkedList区别

1. ArrayList是实现了基于动态数组的数据结构，`LinkedList`基于链表的数据结构。
2. 对于随机访问`get`和`set`，`ArrayList`优于`LinkedList`，因为`LinkedList`要移动指针。
3. 对于新增和删除操作`add`和`remove`，`LinedList`比较占优势，因为`ArrayList`要移动数据

**若只对单条数据插入或删除，ArrayList的速度反而优于LinkedList。但若是批量随机的插入删除数据，LinkedList的速度大大优于ArrayList. 因为ArrayList每插入一条数据，要移动插入点及之后的所有数据。  在分别有200000条“记录”的ArrayList和LinkedList的首位插入20000条数据，LinkedList耗时约是ArrayList的20分之1。**

`LinkedList` 推荐使用`foreach`或者`iterator`遍历，避免使用`get`方式遍历

## HashMap 原理，ConcurrentHashMap的实现原理

ConcurrentHashMap和Hashtable的区别
Hashtable和ConcurrentHashMap有什么分别呢？它们都可以用于多线程的环境，但是当Hashtable的大小增加到一定的时候，性能会急剧下降，因为迭代时需要被锁定很长的时间。因为ConcurrentHashMap引入了分割(segmentation)，不论它变得多么大，仅仅需要锁定map的某个部分，而其它的线程不需要等到迭代完成才能访问map。简而言之，在迭代的过程中，ConcurrentHashMap仅仅锁定map的某个部分，而Hashtable则会锁定整个map。

### reference 

- [Java-HashMap工作原理及实现](https://yikun.github.io/2015/04/01/Java-HashMap%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86%E5%8F%8A%E5%AE%9E%E7%8E%B0/)
- [ConurrentHashMap和Hashtable的区别](http://www.importnew.com/7166.html)

- [探索 ConcurrentHashMap 高并发性的实现机制](https://www.ibm.com/developerworks/cn/java/java-lo-concurrenthashmap/index.html)

## Java 内存管理

Java 虚拟机所管理的内存包括方法区、虚拟机栈、本地方法栈、堆、程序计数器。

**程序计数器**是一块较小的内存空间，作用是当前线程所执行的字节码行号指示器。字节码解释器工作就是通过改变这个计数器的值来
选取下一条需要执行的指令。

**Java 虚拟机栈**用于存储局部变量表、操作栈、动态链接、方法出口等信息。每一个方法被调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程

**本地方法栈**虚拟机栈所发挥的作用是非常相似的，其区别不过是虚拟机栈为虚拟机执行Java方法（也就是字节码）服务，而本地方法栈则是为虚拟机使用到的Native方法服务

**Java 堆**Java堆是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例

**方法区**与Java堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

### 对象访问

```Object obj = new Object()```



Java 的内存管理就是对象的分配和释放

分配：通过`new`关键字为每个对象申请内存空间，所有对象都在jvm堆（Heap）中分配空间。
释放：对象的释放是由垃圾回收机制决定和执行的

### 内存泄漏
简单理解就是无用的对象存在引用。对象是可达的，即在有向图中，存在通路可以与其相连（也就是说仍存在该内存对象的引用）
对象是无用的，即程序以后不会再使用这些对象。这些对象不会被GC回收，然而却占用内存。


### reference

- [Java 内存管理](http://wiki.jikexueyuan.com/project/java-special-topic/platorm-memory.html)
- [深入Java内存区域](https://www.cnblogs.com/gw811/archive/2012/10/18/2730117.html)
- [Android gc](https://mp.weixin.qq.com/s?__biz=MzI1MTA1MzM2Nw==&mid=400021278&idx=1&sn=0e971807eb0e9dcc1a81853189a092f3&scene=0&key=b410d3164f5f798eafd870697d352ac86e0e54b9605b5fcd2c6a62268c16080ee291069627f13ed906cc2f39706b6a54&ascene=0&uin=NzY0MTg2ODU%3D&devicetype=iMac+MacBookPro11%2C1+OSX+OSX+10.10.5+build(14F27)&version=11000003&pass_ticket=nhSGhYD4LC9FWvUPv26Y7AdIzqEDu8FTImf2AKlyrCk%3D)
- [Java中的内存泄漏](http://www.importnew.com/12961.html)


## Android 事件

### reference

- [Android事件传递](https://www.jianshu.com/p/7ff768a77410)

## Android 高性能

### 思想
1. 了解编程语言的编译原理，使用高效编码方式从语法上提高程序性能； 
2. 采用合理的数据结构和算法提高程序性能，这往往是决定程序性能的关键； 
3. 重视界面布局优化； 
4. 采用多线程、缓存数据、延迟加载、提前加载等手段，解决严重的性能瓶颈； 
5. 合理配置虚拟机堆内存使用上限和使用率，减少垃圾回收频率； 
6. 合理使用native代码； 
7. 合理配置数据库缓存类型和优化SQL语句加快读取速度，使用事务加快写入速度； 
7. 使用工具分析性能问题，找出性能瓶颈； 

### 技巧
1. 避免创建不必要的对象
2. 合理使用static成员
3. 避免内部getter／setter
4. 使用增强for循环
5. 使用public代替private以便私有内部类高效访问外部类成员 
6. 合理使用浮点类型 
7. 采用<merge>优化布局层数。 采用<include＞来共享布局。 
8. 延时加载View. 采用ViewStub 避免一些不经常的视图长期被引用，占用内存
9. 移除Activity默认背景，提升activity加载速度

### reference

- [Android开发性能优化总结](https://blog.csdn.net/gs12software/article/details/51173392)