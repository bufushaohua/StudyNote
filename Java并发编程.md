# Java并发编程

## 一、线程基础

### 1.实现线程的方法

> ####  实现Runnable接口

~~~java
public class RunnableThread implements Runnable {

    @Override
    public void run() {
        System.out.println('用实现Runnable接口实现线程');
    }
}
~~~

> #### 继承 Thread 类

~~~java
public class ExtendsThread extends Thread {

    @Override
    public void run() {
        System.out.println('用Thread类实现线程');
    }
}
~~~

> #### 通过线程池创建线程

~~~java
static class DefaultThreadFactory implements ThreadFactory {
 
    DefaultThreadFactory() {
        SecurityManager s = System.getSecurityManager();
        group = (s != null) ? s.getThreadGroup() :
            Thread.currentThread().getThreadGroup();
        namePrefix = "pool-" +
            poolNumber.getAndIncrement() +
            "-thread-";
    }
 

    public Thread newThread(Runnable r) {
        Thread t = new Thread(group, r,
                    namePrefix + threadNumber.getAndIncrement(),
0);

        if (t.isDaemon())
            t.setDaemon(false);
        if (t.getPriority() != Thread.NORM_PRIORITY)
            t.setPriority(Thread.NORM_PRIORITY);
        return t;
    }
}

/*
对于线程池而言，本质上是通过线程工厂创建线程的，默认采用 DefaultThreadFactory ，它会给线程池创建的线程设置一些默认值，比如：线程的名字、是否是守护线程，以及线程的优先级等。但是无论怎么设置这些属性，最终它还是通过 new Thread() 创建线程的 ，只不过这里的构造函数传入的参数要多一些，由此可以看出通过线程池创建线程并没有脱离最开始的那两种基本的创建方式，因为本质上还是通过 new Thread() 实现的。
*/
~~~

> #### 有返回值的 Callable 创建线程

~~~java
class CallableTask implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        return new Random().nextInt();
    }
}

//创建线程池
ExecutorService service = Executors.newFixedThreadPool(10);
//提交任务，并用 Future提交返回结果
Future<Integer> future = service.submit(new CallableTask());

/*
通过有返回值的 Callable 创建线程，Runnable 创建线程是无返回值的，而 Callable 和与之相关的 Future、FutureTask，它们可以把线程执行的结果作为返回值返回，如代码所示，实现了 Callable 接口，并且给它的泛型设置成 Integer，然后它会返回一个随机数。

但是，无论是 Callable 还是 FutureTask，它们首先和 Runnable 一样，都是一个任务，是需要被执行的，而不是说它们本身就是线程。它们可以放到线程池中执行，如代码所示， submit() 方法把任务放到线程池中，并由线程池创建线程，不管用什么方法，最终都是靠线程来执行的，而子线程的创建方式仍脱离不了最开始讲的两种基本方式，也就是实现 Runnable 接口和继承 Thread 类。
*/
~~~

> #### 其他创建方式

- ##### 定时器 Timer

  ~~~java
  class TimerThread extends Thread {
  //具体实现
  }
  
  /*
  定时器也可以实现线程，如果新建一个 Timer，令其每隔 10 秒或设置两个小时之后，执行一些任务，那么这时它确实也创建了线程并执行了任务，但如果我们深入分析定时器的源码会发现，本质上它还是会有一个继承自 Thread 类的 TimerThread
  */
  ~~~

- 其他方法

  ~~~java
  /**
   *描述：匿名内部类创建线程
   */
  new Thread(new Runnable() {
      @Override
      public void run() {
          System.out.println(Thread.currentThread().getName());
      }
  }).start();
  
  }
  }
  
  ~~~

> 实现线程只有一种方式	

~~~markdown
- 先认为有两种创建线程的方式
   而其他的创建方式，比如线程池或是定时器，它们仅仅是在 new Thread() 外做了一层封装，如果我们把这些都叫作一种新的方式，那么创建线程的方式便会千变万化、层出不穷，比如 JDK 更新了，它可能会多出几个类，会把 new Thread() 重新封装，表面上看又会是一种新的实现线程的方式，透过现象看本质，打开封装后，会发现它们最终都是基于 Runnable 接口或继承 Thread 类实现的。

- 接下来，我们进行更深层次的探讨，为什么说这两种方式本质上是一种呢？
@Override
public void run() {
    if (target != null) {
        target.run();
    }
}


	第一种方式中 run() 方法究竟是怎么实现的，可以看出 run() 方法的代码非常短小精悍，第 1 行代码 if (target != null) ，判断 target 是否等于 null，如果不等于 null，就执行第 2 行代码 target.run()，而 target 实际上就是一个 Runnable，即使用 Runnable 接口实现线程时传给Thread类的对象。
	
	然后，我们来看第二种方式，也就是继承 Thread 方式，实际上，继承 Thread 类之后，会把上述的 run() 方法重写，重写后 run() 方法里直接就是所需要执行的任务，但它最终还是需要调用 thread.start() 方法来启动线程，而 start() 方法最终也会调用这个已经被重写的 run() 方法来执行它的任务，这时我们就可以彻底明白了，事实上创建线程只有一种方式，就是构造一个 Thread 类，这是创建线程的唯一方式。
	
- 它们的不同点仅仅在于实现线程运行内容的不同
    运行内容主要来自于两个地方，要么来自于 target，要么来自于重写的 run() 方法

    本质上，实现线程只有一种方式，而要想实现线程执行的内容，却有两种方式，也就是可以通过 实现 Runnable 接口的方式，或是继承 Thread 类重写 run() 方法的方式，把我们想要执行的代码传入，让线程去执行
~~~

> #### 实现 Runnable 接口比继承 Thread 类实现线程要好

~~~markdown
- 从代码的架构考虑
    实际上，Runnable 里只有一个 run() 方法，它定义了需要执行的内容，在这种情况下，实现了 Runnable 与 Thread 类的解耦，Thread 类负责线程启动和属性设置等内容，权责分明。
    
- 从性能方面
	使用继承 Thread 类方式，每次执行一次任务，都需要新建一个独立的线程，执行完任务后线程走到生命周期的尽头被销毁，如果还想执行这个任务，就必须再新建一个继承了 Thread 类的类，如果run执行的内容比这一系列操作要小的多，那就得不偿失。
	
	如果我们使用实现 Runnable 接口的方式，就可以把任务直接传入线程池，使用一些固定的线程来完成任务，不需要每次新建销毁线程，大大降低了性能开销。
	
- Java单继承
	第三点好处在于 Java 语言不支持双继承，如果我们的类一旦继承了 Thread 类，那么它后续就没有办法再继承其他的类，这样一来，如果未来这个类需要继承其他类实现一些功能上的拓展，它就没有办法做到了，相当于限制了代码未来的可拓展性。
~~~



### 2.停止线程

> #### 为什么不强制停止？而是通知、协作

~~~markdown
# 错误的停止方法：
	stop() 会直接把线程停止，这样就没有给线程足够的时间来处理想要在停止前保存数据的逻辑，任务戛然而止，会导致出现数据完整性等问题。
	
	对于 suspend() 和 resume() 而言，它们的问题在于如果线程调用 suspend()，它并不会释放锁，就开始进入休眠，但此时有可能仍持有锁，这样就容易导致死锁问题，因为这把锁在线程被 resume() 之前，是不会被释放的。

	假设线程 A 调用了 suspend() 方法让线程 B 挂起，线程 B 进入休眠，而线程 B 又刚好持有一把锁，此时假设线程 A 想访问线程 B 持有的锁，但由于线程 B 并没有释放锁就进入休眠了，所以对于线程 A 而言，此时拿不到锁，也会陷入阻塞，那么线程 A 和线程 B 就都无法继续向下执行。


	对于 Java 而言，最正确的停止线程的方式是使用 interrupt。但 interrupt 仅仅起到通知被停止线程的作用。而对于被停止的线程而言，它拥有完全的自主权，它既可以选择立即停止，也可以选择一段时间后停止，也可以选择压根不停止。
	
	Java 希望程序间能够相互通知、相互协作地管理线程，因为如果不了解对方正在做的工作，贸然强制停止线程就可能会造成一些安全的问题，为了避免造成问题就需要给对方一定的时间来整理收尾工作。
	
	比如：线程正在写入一个文件，这时收到终止信号，它就需要根据自身业务判断，是选择立即停止，还是将整个文件写入成功后停止，而如果选择立即停止就可能造成数据不完整，不管是中断命令发起者，还是接收者都不希望数据出现问题。

~~~

> #### 如何用 interrupt 停止线程

~~~java
while (!Thread.currentThread().islnterrupted() && more work to do) {
    do more work
}

/*
在 while 循环体判断语句中，首先通过 Thread.currentThread().isInterrupt() 判断线程是否被中断，随后检查是否还有工作要做。&& 逻辑表示只有当两个判断条件同时满足的情况下，才会去执行下面的工作。
*/
~~~

> #### sleep期间是否感受到中断

~~~java
// 如果线程在执行任务期间有休眠需求，也就是每打印一个数字，就进入一次 sleep ，而此时将 Thread.sleep() 的休眠时间设置为 1000 秒钟。
public class StopDuringSleep {
 
    public static void main(String[] args) throws InterruptedException {
        Runnable runnable = () -> {
            int num = 0;
            try {
                while (!Thread.currentThread().isInterrupted() && num <= 1000) {
                    System.out.println(num);
                    num++;
                    Thread.sleep(1000000);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        };
        Thread thread = new Thread(runnable);
        thread.start();
        Thread.sleep(5);
        thread.interrupt();
    }
}

/* 
如果 sleep、wait 等可以让线程进入阻塞的方法使线程休眠了，而处于休眠中的线程被中断，那么线程是可以感受到中断信号的，并且会抛出一个 InterruptedException 异常，同时清除中断信号，将中断标记位设置成 false。这样一来就不用担心长时间休眠中线程感受不到中断了，因为即便线程还在休眠，仍然能够响应中断通知，并抛出异常。
*/
~~~

> #### 为什么用 volatile 标记位的停止方法是错误的

~~~markdown
因为线程在处于阻塞状态，不能感受到中断信号，并做响应处理。		
~~~



### 3.线程状态

> #### 线程的6种状态

![image-20200910193726736](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200910193726736.png)

~~~markdown
- New（新创建）：
		New 表示线程被创建但尚未启动的状态

- Runnable（可运行）：
		Runable 状态对应操作系统线程状态中的两种状态，分别是 Running 和 Ready （可能在运行，也可能没有运行）

阻塞状态包括三种状态，分别是 Blocked(被阻塞）、Waiting(等待）、Timed Waiting(计时等待），这三种状态统称为阻塞状态

- Blocked（被阻塞）：
		从 Runnable 状态进入 Blocked 状态只有一种可能，就是进入 synchronized 保护的代码时没有抢到 monitor 锁，无论是进入 synchronized 代码块，还是 synchronized 方法，都是一样。
		当处于 Blocked 的线程抢到 monitor 锁，就会从 Blocked 状态回到Runnable 状态。

- Waiting（等待）：
	线程进入 Waiting 状态有三种可能性。
        1.没有设置 Timeout 参数的 Object.wait() 方法。

        2.没有设置 Timeout 参数的 Thread.join() 方法。

        3.LockSupport.park() 方法。
      
Blocked 与 Waiting 的区别是 Blocked 在等待其他线程释放 monitor 锁，而 Waiting 则是在等待某个条件，比如 join 的线程执行完毕，或者是 notify()/notifyAll() 。

- Timed Waiting（计时等待）：
	线程进入Timed Waiting 状态有四种可能性。
        1.设置了时间参数的 Thread.sleep(long millis) 方法；

        2. 设置了时间参数的 Object.wait(long timeout) 方法；

        3. 设置了时间参数的 Thread.join(long millis) 方法；

        4. 设置了时间参数的 LockSupport.parkNanos(long nanos) 方法和 LockSupport.parkUntil(long deadline) 方法。
- Terminated（被终止）：
	线程进入Terminated 状态有二种可能性。
		1.run() 方法执行完毕，线程正常退出。

		2.出现一个没有捕获的异常，终止了 run() 方法，最终导致意外终止。


# 线程转换注意事项
	线程的状态是需要按照箭头方向来走的，比如线程从 New 状态是不可以直接进入 Blocked 状态的，它需要先经历 Runnable 状态。

	线程生命周期不可逆：一旦进入 Runnable 状态就不能回到 New 状态；一旦被终止就不可能再有任何状态的变化。所以一个线程只能有一次 New 和 Terminated 状态，只有处于中间状态才可以相互转换。
~~~



### 4.wait/notify/notifyAll 方法的使用

> #### 为什么 wait 必须在 synchronized 保护的同步代码中使用？

[不要求 wait 方法放在 synchronized 保护的同步代码中使用的情况]()

~~~java
class BlockingQueue {
    Queue<String> buffer = new LinkedList<String>();
    public void give(String data) {
        buffer.add(data);
        notify();  // Since someone may be waiting in take
    }
    public String take() throws InterruptedException {
        while (buffer.isEmpty()) {
            wait();
        }
        return buffer.remove;
    }
}


/*
可能出现以下场景：
	首先，消费者线程调用 take 方法并判断 buffer.isEmpty 方法是否返回 true，若为 true 代表buffer是空的，则线程希望进入等待，但是在线程调用 wait 方法之前，就被调度器暂停了，所以此时还没来得及执行 wait 方法。
	此时生产者开始运行，执行了整个 give 方法，它往 buffer 中添加了数据，并执行了 notify 方法，但 notify 并没有任何效果，因为消费者线程的 wait 方法没来得及执行，所以没有线程在等待被唤醒。
	此时，刚才被调度器暂停的消费者线程回来继续执行 wait 方法并进入了等待。
*/

//这是因为 wait 方法所在的 take 方法没有被 synchronized 保护，所以它的 while 判断和 wait 方法无法构成原子操作，那么此时整个程序就很容易出错。
~~~

> #### 为什么 wait/notify/notifyAll 被定义在 Object 类中，而 sleep 定义在 Thread 类中？

~~~markdown
**主要两点原因：**
	1. 因为 Java 中每个对象都有一把称之为 monitor 监视器的锁，由于每个对象都可以上锁，这就要求在对象头中有一个用来保存锁信息的位置。这个锁是对象级别的，而非线程级别的，wait/notify/notifyAll 也都是锁级别的操作，它们的锁属于对象，所以把它们定义在 Object 类中是最合适，因为 Object 类是所有对象的父类。
	2.因为如果把 wait/notify/notifyAll 方法定义在 Thread 类中，会带来很大的局限性，比如一个线程可能持有多把锁，以便实现相互配合的复杂逻辑，假设此时 wait 方法定义在 Thread 类中，如何实现让一个线程持有多把锁呢？又如何明确线程等待的是哪把锁呢？既然我们是让当前线程去等待某个对象的锁，自然应该通过操作对象来实现，而不是操作线程。
~~~

> #### wait/notify 和 sleep 方法的异同？

~~~markdown
**相同点**
	1.它们都可以让线程阻塞。
	2.它们都可以响应 interrupt 中断：在等待的过程中如果收到中断信号，都可以进行响应，并抛出 InterruptedException 异常。
	
**不同点**
	1.wait 方法必须在 synchronized 保护的代码中使用，而 sleep 方法并没有这个要求。
	2.在同步代码中执行 sleep 方法时，并不会释放 monitor 锁，但执行 wait 方法时会主动释放 monitor 锁。
	3.sleep 方法中会要求必须定义一个时间，时间到期后会主动恢复，而对于没有参数的 wait 方法而言，意味着永久等待，直到被中断或被唤醒才能恢复，它并不会主动恢复。
	4.wait/notify 是 Object 类的方法，而 sleep 是 Thread 类的方法。
~~~

### 5.实现生产者消费者模式的方法

> #### 方法一：用 BlockingQueue 实现生产者消费者模式

~~~java
/*
虽然代码非常简单，但实际上 ArrayBlockingQueue 已经在背后完成了很多工作，比如队列满了就去阻塞生产者线程，队列有空就去唤醒生产者线程等。
*/
public static void main(String[] args) {
 
  BlockingQueue<Object> queue = new ArrayBlockingQueue<>(10);
 
 Runnable producer = () -> {
    while (true) {
          queue.put(new Object());
  }
   };
 
new Thread(producer).start();
new Thread(producer).start();
 
Runnable consumer = () -> {
      while (true) {
           queue.take();
}
   };
new Thread(consumer).start();
new Thread(consumer).start();
}


~~~

> #### 方法二：用 Condition 实现生产者消费者模式，背后的实现原理非常相似，相当于我们自己实现一个简易版的 BlockingQueue

~~~java
public class MyBlockingQueueForCondition {
 
    //定义队列
   private Queue queue;
    //设置队列最大容量16
   private int max = 16;
    //定义了一个 ReentrantLock 类型的 Lock 锁
   private ReentrantLock lock = new ReentrantLock();
   //创建了两个 Condition，一个是 notEmpty，另一个是 notFull，分别代表队列没有空和没有满的条件
   private Condition notEmpty = lock.newCondition();
   private Condition notFull = lock.newCondition();
 
 //构造函数
   public MyBlockingQueueForCondition(int size) {
       this.max = size;
       queue = new LinkedList();
   }
 
    //生产者生产
   public void put(Object o) throws InterruptedException {
       //获取锁
       lock.lock();
       try {
           while (queue.size() == max) {
               notFull.await();
           }
           queue.add(o);
           notEmpty.signalAll();
       } finally {
           //释放锁
           lock.unlock();
       }
   }
 
    //消费者消费
   public Object take() throws InterruptedException {
       //获取锁
       lock.lock();
       try {
           /*
           为什么使用 while( queue.size() == 0 ) 检查队列状态，而不能用 if( queue.size() == 0 )，见下方解析
           */
           while (queue.size() == 0) {
               notEmpty.await();
           }
           Object item = queue.remove();
           notFull.signalAll();
           return item;
       } finally {
           //释放锁
           lock.unlock();
       }
   }
}

/*
假设有两个消费者，当第一个消费者获取队列数据时，发现为空，遂进入等待状态，同时释放锁，第二个消费者便可获取锁，进入并执行 if( queue.size() == 0 )，也发现队列为空，于是也进入等待状态。此时，生产者生产了一条数据，同时也唤醒两位消费者，在第一个线程执行完操作后会在 finally 中通过 unlock 解锁，而此时第二个线程便可以拿到被第一个线程释放的锁，继续执行操作，也会去调用 queue.remove 操作，然而这个时候队列已经为空了，所以会抛出 NoSuchElementException 异常，这不符合我们的逻辑。而如果用 while 做检查，当第一个消费者被唤醒得到锁并移除数据之后，第二个线程在执行 remove 前仍会进行 while 检查，发现此时依然满足 queue.size() == 0 的条件，就会继续执行 await 方法，避免了获取的数据为 null 或抛出异常的情况。
*/

~~~

> #### 方法三：用 wait/notify 实现生产者消费者模式：实现原理和Condition 是非常类似的，它们是兄弟关系

~~~java
class MyBlockingQueue {
 
   private int maxSize;
   private LinkedList<Object> storage;
 
   public MyBlockingQueue(int size) {
       this.maxSize = size;
       storage = new LinkedList<>();
   }
 
   public synchronized void put() throws InterruptedException {
       while (storage.size() == maxSize) {
           wait();
       }
       storage.add(new Object());
       notifyAll();
   }
 
   public synchronized void take() throws InterruptedException {
       while (storage.size() == 0) {
           wait();
       }
       System.out.println(storage.remove());
       notifyAll();
   }
}
/*
	先来看 put 方法，put 方法被 synchronized 保护，while 检查队列是否为满，如果不满就往里放入数据并通过 notifyAll() 唤醒其他线程。
	同样，take 方法也被 synchronized 修饰，while 检查队列是否为空，如果不为空就获取数据并唤醒其他线程。
*/

//使用这个 MyBlockingQueue 实现的生产者消费者代码如下：
/**
* 描述：     wait形式实现生产者消费者模式
*/
public class WaitStyle {
 
   public static void main(String[] args) {
       MyBlockingQueue myBlockingQueue = new MyBlockingQueue(10);
       Producer producer = new Producer(myBlockingQueue);
       Consumer consumer = new Consumer(myBlockingQueue);
       new Thread(producer).start();
       new Thread(consumer).start();
   }
}
 
class Producer implements Runnable {
 
   private MyBlockingQueue storage;
 
   public Producer(MyBlockingQueue storage) {
       this.storage = storage;
   }
 
   @Override
   public void run() {
       for (int i = 0; i < 100; i++) {
           try {
               storage.put();
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
       }
   }
}
 
class Consumer implements Runnable {
 
   private MyBlockingQueue storage;
 
   public Consumer(MyBlockingQueue storage) {
       this.storage = storage;
   }
 
   @Override
   public void run() {
       for (int i = 0; i < 100; i++) {
           try {
               storage.take();
           } catch (InterruptedException e) {
               e.printStackTrace();
           }
       }
   }
}

~~~



## 二、线程安全

### 1.三类线程安全问题

> #### 3 种典型的线程安全问题

~~~markdown
1. 运行结果错误:
		通常是因为并发读写导致的
2. 发布和初始化导致线程安全问题:
		对象没有在正确的时间、地点被发布或初始化
3. 活跃性问题:

活跃性问题有三种：死锁、活锁和饥饿
	死锁：最常见的活跃性问题是死锁，死锁是指两个线程之间相互等待对方资源，但同时又互不相让，都想自己先执行，
	
	活锁：第二种活跃性问题是活锁，活锁与死锁非常相似，也是程序一直等不到结果，但对比于死锁，活锁是活的，什么意思呢？因为正在运行的线程并没有阻塞，它始终在运行中，却一直得不到结果。
	举个例子：
		假设有一个消息队列，队列里放着各种各样需要被处理的消息，而某个消息由于自身被写错了导致不能被正确处理，执行时会报错，可是队列的重试机制会重新把它放在队列头进行优先重试处理，但这个消息本身无论被执行多少次，都无法被正确处理，每次报错后又会被放到队列头进行重试，周而复始，最终导致线程一直处于忙碌状态，但程序始终得不到结果，便发生了活锁问题。
		
	饥饿：饥饿是指线程由于优先级较低，使得需要某些资源时始终得不到，尤其是CPU 资源，就会导致线程一直不能运行而产生的问题。
~~~

### 2.哪些场景会存在线程安全问题（四种）

> 访问共享变量或资源

~~~markdown
因为这些信息不仅会被一个线程访问到，还有可能被多个线程同时访问，那么就有可能在并发读写的情况下发生线程安全问题
~~~

> #### 依赖时序的操作

~~~markdown
如果我们操作的正确性是依赖时序的，而在多线程的情况下又不能保障执行的顺序和我们预想的一致，这个时候就会发生线程安全问题
~~~

> #### 不同数据之间存在绑定关系

~~~markdown
有时候，我们的不同数据之间是成组出现的，存在着相互对应或绑定的关系，最典型的就是 IP 和端口号。
~~~

> #### 对方没有声明自己是线程安全的

~~~markdown
如果对方没有声明自己是线程安全的，那么这种情况下对其他类进行多线程的并发操作，就有可能会发生线程安全问题。

如果我们把 ArrayList 用在了多线程的场景，需要在外部手动用 synchronized 等方式保证并发安全。
~~~



## 三、线程池

### 1.线程池相比手动创建线程的优点

> #### 为什么要使用线程池

- 如果每个任务都创建一个线程会带来哪些问题：
  - 第一点，反复创建线程系统开销比较大，每个线程创建和销毁都需要时间，如果任务比较简单，那么就有可能导致创建和销毁线程消耗的资源比线程执行任务本身消耗的资源还要大。
  - 第二点，过多的线程会占用过多的内存等资源，还会带来过多的上下文切换，同时还会导致系统不稳定。
- 线程池解决问题思路
  - 首先，针对反复创建线程开销大的问题，线程池用一些固定的线程一直保持工作状态并反复执行任务。
  - 其次，针对过多线程占用太多内存资源的问题，解决思路更直接，线程池会根据需要创建线程，控制线程的总数量，避免占用过多内存资源。

> #### 使用线程池的好处

- 三点好处：
  - 第一点，线程池可以解决线程生命周期的系统开销问题，同时还可以加快响应速度。
  - 第二点，线程池可以统筹内存和 CPU 的使用，避免资源使用不当。
  - 第三点，线程池可以统一管理资源。

### 2.线程池的各个参数

> #### 线程池的参数

![image-20200914185258103](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200914185258103.png)

> #### 线程创建的时机

![image-20200914185448809](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200914185448809.png)

### 3.线程池的四种拒绝策略

> #### 拒绝时机

- 第一种情况是当我们调用 shutdown 等方法关闭线程池后，即便此时可能线程池内部依然有没执行完的任务正在执行，但是由于线程池已经关闭，此时如果再向线程池内提交任务，就会遭到拒绝。
- 第二种情况是线程池没有能力继续处理新提交的任务，也就是工作已经非常饱和的时候。

> #### 拒绝策略

- 第一种拒绝策略是 AbortPolicy，这种拒绝策略在拒绝任务时，会直接抛出一个类型为 RejectedExecutionException 的 RuntimeException，让你感知到任务被拒绝了，于是你便可以根据业务逻辑选择重试或者放弃提交等策略。
- 第二种拒绝策略是 DiscardPolicy，这种拒绝策略正如它的名字所描述的一样，当新任务被提交后直接被丢弃掉，也不会给你任何的通知，相对而言存在一定的风险，因为我们提交的时候根本不知道这个任务会被丢弃，可能造成数据丢失。
- 第三种拒绝策略是 DiscardOldestPolicy，如果线程池没被关闭且没有能力执行，则会丢弃任务队列中的头结点，通常是存活时间最长的任务，这种策略与第二种不同之处在于它丢弃的不是最新提交的，而是队列中存活时间最长的，这样就可以腾出空间给新提交的任务，但同理它也存在一定的数据丢失风险。
- 第四种拒绝策略是 CallerRunsPolicy，相对而言它就比较完善了，当有新任务提交后，如果线程池没被关闭且没有能力执行，则把这个任务交于提交任务的线程执行，也就是谁提交任务，谁就负责执行任务。这样做主要有两点好处。
  - 第一点新提交的任务不会被丢弃，这样也就不会造成业务损失。
  - 第二点好处是，由于谁提交任务谁就要负责执行任务，这样提交任务的线程就得负责执行任务，而执行任务又是比较耗时的，在这段期间，提交任务的线程被占用，也就不会再提交新的任务，减缓了任务提交的速度，相当于是一个负反馈。在此期间，线程池中的线程也可以充分利用这段时间来执行掉一部分任务，腾出一定的空间，相当于是给了线程池一定的缓冲期。

### 4.6种常见的线程池

- FixedThreadPool

- CachedThreadPool

- ScheduledThreadPool

- SingleThreadExecutor

- SingleThreadScheduledExecutor

- ForkJoinPool（Java 8 新增）

> #### FixedThreadPool

​	 	它的核心线程数和最大线程数是一样的，所以可以把它看作是固定线程数的线程池，它的特点是线程池中的线程数除了初始阶段需要从 0 开始增加外，之后的线程数量就是固定的，就算任务数超过线程数，线程池也不会再创建更多的线程来处理任务，而是会把超出线程处理能力的任务放到任务队列中进行等待。

> #### CachedThreadPool

​			该线程池的线程数量不是固定不变的，当然它也有一个用于存储提交任务的队列，但这个队列是 SynchronousQueue，队列的容量为0，实际不存储任何任务，它只负责对任务进行中转和传递，所以效率比较高。