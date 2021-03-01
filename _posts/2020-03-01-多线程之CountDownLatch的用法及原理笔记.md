多线程之CountDownLatch的用法及原理笔记

getCount：
返回当前的计数count值，
public void countDown()
调用此方法后，会减少计数count的值。
递减后如果为0，则会释放所有等待的线程
public void await()
           throws InterruptedException
调用CountDownLatch对象的await方法后。
会让当前线程阻塞，直到计数count递减至0。

如果当前线程数大于0，则当前线程在线程调度中将变得不可用，并处于休眠状态，直到发生以下两种情况之一：

1、调用countDown()方法，将计数count递减至0。

2、当前线程被其他线程打断。

public boolean await(long timeout,
            TimeUnit unit)
              throws InterruptedException
同时await还提供一个带参数和返回值的方法。

> 如果计数count正常递减，返回0后，await方法会返回true并继续执行后续逻辑。

> 或是，尚未递减到0，而到达了指定的时间间隔后，方法返回false。如果时间小于等于0，则此方法不执行等待。
