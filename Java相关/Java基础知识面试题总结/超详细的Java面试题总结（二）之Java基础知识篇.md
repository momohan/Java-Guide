
# 多线程和Java虚拟机
## <font face="楷体">创建线程有几种不同的方式？你喜欢哪一种？为什么？</font>
1.  继承Thread类
2. 实现Runnable接口
3. 应用程序可以使用Executor框架来创建线程池
4. 实现Callable接口。

我更喜欢实现Runnable接口这种方法，当然这也是现在大多程序员会选用的方法。因为一个类只能继承一个父类而可以实现多个接口。同时，线程池也是非常高效的，很容易实现和使用。

## <font face="楷体">简述线程，程序、进程的基本概念。以及他们之间关系是什么？（参考书籍：《Java程序设计基础》第五版）</font>
**线程**与进程相似，但线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。  

**程序**是含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中，也就是说程序是静态的代码。

**进程**是程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。简单来说，一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，同时，每个进程还占有某些系统资源如CPU时间，内存空间，文件，文件，输入输出设备的使用权等等。换句话说，当程序在执行时，将会被操作系统载入内存中。
线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。

## <font face="楷体">什么是多线程？为什么程序的多线程功能是必要的？</font>
 多线程就是几乎同时执行多个线程（一个处理器在某一个时间点上永远都只能是一个线程！即使这个处理器是多核的，除非有多个处理器才能实现多个线程同时运行。）。几乎同时是因为实际上多线程程序中的多个线程实际上是一个线程执行一会然后其他的线程再执行，并不是很多书籍所谓的同时执行。这样可以带来以下的好处：
1. 使用线程可以把占据长时间的程序中的任务放到后台去处理
2. 用户界面可以更加吸引人，这样比如用户点击了一个按钮去触发某些事件的处理，可以弹出一个进度条来显示处理的进度
3. 程序的运行速度可能加快
4. 在一些等待的任务实现上如用户输入、文件读写和网络收发数据等，线程就比较有用了。在这种情况下可以释放一些珍贵的资源如内存占用等等。
还有其他很多使用多线程的好处，这里就不一一说明了。

## <font face="楷体">多线程与多任务的差异是什么？（参考书籍：《Java程序设计基础》第五版）</font>
多任务与多线程是两个不同的概念，
多任务是针对操作系统而言的，表示操作系统可以同时运行多个应用程序。
而多线程是针对一个进程而言的，表示在一个进程内部可以几乎同时执行多个线程

## <font face="楷体">线程有哪些基本状态？这些状态是如何定义的?</font>
1. **新建(new)**：新创建了一个线程对象。
2. **可运行(runnable)**：线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获 取cpu的使用权。
3. **运行(running)**：可运行状态(runnable)的线程获得了cpu时间片（timeslice），执行程序代码。
4. **阻塞(block)**：阻塞状态是指线程因为某种原因放弃了cpu使用权，也即让出了cpu timeslice，暂时停止运行。直到线程进入可运行(runnable)状态，才有 机会再次获得cpu timeslice转到运行(running)状态。阻塞的情况分三种：
(一). 等待阻塞：运行(running)的线程执行o.wait()方法，JVM会把该线程放 入等待队列(waitting queue)中。
(二). 同步阻塞：运行(running)的线程在获取对象的同步锁时，若该同步锁 被别的线程占用，则JVM会把该线程放入锁池(lock pool)中。
(三). 其他阻塞: 运行(running)的线程执行Thread.sleep(long ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入可运行(runnable)状态。
5. **死亡(dead)**：线程run()、main()方法执行结束，或者因异常退出了run()方法，则该线程结束生命周期。死亡的线程不可再次复生。
![线程的基本状态](https://user-gold-cdn.xitu.io/2017/12/15/16059cc91ee8efb3?w=876&h=492&f=png&s=-1)
备注：
可以用早起坐地铁来比喻这个过程： 
还没起床：sleeping 
起床收拾好了，随时可以坐地铁出发：Runnable 
等地铁来：Waiting 
地铁来了，但要排队上地铁：I/O阻塞 
上了地铁，发现暂时没座位：synchronized阻塞 
地铁上找到座位：Running 
到达目的地：Dead

## <font face="楷体">什么是线程的同步？程序中为什么要实现线程的同步？是如何实现同步的？</font>
当一个线程对共享的数据进行操作时，应使之成为一个”原子操作“，即在没有完成相关操作之前，不允许其他线程打断它，否则，就会破坏数据的完整性，必然会得到错误的处理结果，这就是线程的同步。
在多线程应用中，考虑不同线程之间的数据同步和防止死锁。当两个或多个线程之间同时等待对方释放资源的时候就会形成线程之间的死锁。为了防止死锁的发生，需要通过同步来实现线程安全。

## <font face="楷体">在监视器(Monitor)内部，是如何做线程同步的？程序应该做哪种级别的同步？</font>
在 java 虚拟机中, 每个对象( Object 和 class )通过某种逻辑关联监视器,每个监视器和一个对象引用相关联, 为了实现监视器的互斥功能, 每个对象都关联着一把锁.
一旦方法或者代码块被 **synchronized** 修饰, 那么这个部分就放入了监视器的监视区域, **确保一次只能有一个线程执行该部分的代码**, 线程在获取锁之前不允许执行该部分的代码
另外 java 还提供了显式监视器( Lock )和隐式监视器( synchronized )两种锁方案

## <font face="楷体">什么是死锁(deadlock)？</font>
[360百科](https://baike.so.com/doc/6950313-7172714.html)

死锁 :是指两个或两个以上的进程在执行过程中,因争夺资源而造成的一种互相等待的现象,若无外力作用,它们都将无法推进下去 。
产生原因：
- 因为系统资源不足。 
- 进程运行推进顺序不合适。 
- 资源分配不当等。 
- 占用资源的程序崩溃等。

如果系统资源充足，进程的资源请求都能够得到满足，死锁出现的可能性就很低，否则就会因争夺有限的资源而陷入死锁。其次，进程运行推进顺序与速度不同，也可能产生死锁。

下面四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要下列条件之一不满足，就不会发生死锁。 
- 互斥条件：一个资源每次只能被一个进程使用。 
- 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。 
- 不剥夺条件:进程已获得的资源，在末使用完之前，不能强行剥夺。 
- 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。 
死锁的解除与预防：

理解了死锁的原因，尤其是产生死锁的四个必要条件，就可以最大可能地避免、预防和 解除死锁。所以，在系统设计、进程调度等方面注意如何不让这四个必要条件成立，如何确 定资源的合理分配算法，避免进程永久占据系统资源。此外，也要防止进程在处于等待状态的情况下占用资源。因此，对资源的分配要给予合理的规划。

## <font face="楷体">如何确保N个线程可以访问N个资源同时又不导致死锁？</font>
上面一题我们知道了发生死锁的四个必要条件。我们只要使其中一个不成立就行了。一种非常简单的避免死锁的方式就是：指定获取锁的顺序，并强制线程按照指定的顺序获取锁。因此，如果所有的线程都是以同样的顺序加锁和释放锁，就不会出现死锁了。这也就是破坏了第四个条件循环等待条件。

## <font face="楷体">Java中垃圾回收有什么目的？什么时候进行垃圾回收？</font>
垃圾回收是在内存中存在没有引用的对象或超过作用域的对象时进行。

垃圾回收的目的是识别并且丢弃应用不再使用的对象来释放和重用资源。

## <font face="楷体">finalize()方法什么时候被调用？析构函数(finalization)的目的是什么？</font>
1）垃圾回收器（garbage colector）决定回收某对象时，就会运行该对象的finalize()方法；
finalize是Object类的一个方法，该方法在Object类中的声明protected void finalize() throws Throwable { }
在垃圾回收器执行时会调用被回收对象的finalize()方法，可以覆盖此方法来实现对其资源的回收。注意：一旦垃圾回收器准备释放对象占用的内存，将首先调用该对象的finalize()方法，并且下一次垃圾回收动作发生时，才真正回收对象占用的内存空间
    
2）GC本来就是内存回收了，应用还需要在finalization做什么呢？ 答案是大部分时候，什么都不用做(也就是不需要重载)。只有在某些很特殊的情况下，比如你调用了一些native的方法(一般是C写的)，可以要在finaliztion里去调用C的释放函数。

## <font face="楷体">如果对象的引用被置为null，垃圾收集器是否会立即释放对象占用的内存？</font>
不会，在下一个垃圾回收周期中，这个对象将是可被回收的。

## <font face="楷体">Java中堆和栈的区别</font>
[Java中堆和栈的区别](http://www.cnblogs.com/perfy/p/3820594.html)

**堆内存**用来存放由new创建的对象和数组。 在堆中分配的内存，由Java虚拟机的自动垃圾回收器来管理。 
**栈**中一般存放的是局部变量（方法中的变量或某代码段里（比如for循环））

## <font face="楷体">Java堆的结构是什么样子的？什么是堆中的永久代(Perm Gen space)?</font>
[Java堆的结构是什么样子的？什么是堆中的永久代](https://www.nowcoder.com/ta/review-java/review?page=39)

# Java集合框架
<font face="楷体">List，Set,Map三者的区别及总结</font>

<font face="楷体">ArrayList 与 Vector 区别（为什么要用Arraylist取代Vector呢？）</font>

<font face="楷体">List，Set,Map三者的区别及总结</font>

<font face="楷体">HashMap 和 Hashtable 的区别</font>

<font face="楷体">HashSet 和 HashMap 区别</font>

<font face="楷体">HashMap 和 ConcurrentHashMap 的区别</font>

<font face="楷体">HashSet如何检查重复</font>

<font face="楷体">comparable 和 comparator的区别？</font>

<font face="楷体">如何对Object的list排序？</font>

<font face="楷体">如何实现数组与List的相互转换？</font>

<font face="楷体">如何求ArrayList集合的交集 并集 差集 去重复并集?</font>

<font face="楷体">HashMap 的工作原理及代码实现</font>

<font face="楷体">ConcurrentHashMap 的工作原理及代码实现</font>

<font face="楷体">集合框架底层数据结构总结</font>

<font face="楷体">集合的选用</font>



