# Java

- abstract修饰类：不能new对象，但可以声明引用
- abstract修饰方法：只有方法声明，没有方法实现。（需包含在抽象类中）
- 抽象类中不一定有抽象方法，但有抽象方法的类一定是抽象类
- 子类继承抽象类后，必须覆盖父类所有抽象方法，否则子类还是抽象类
### Collection体系集合

![image-20200517141143972](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200517141143972.png)

> Collection：
> 	特点： 代表一组任意类型的对象，无序、无下标
~~~java
方法：
		boolean add(Object obj) :添加一个对象
		boolean addAll(Collection c) : 将一个集合中的所有对象添加到此集合中
		void clear() :清空此集合中的所有对象
 		boolean contains(Object o) :检查此集合中是否包含o对象
 		boolean equals(Object o):比较此集合是否与指定对象相等
 		boolean isEmpty():判断此集合是否为空
 		boolean remove(Object o):此集合中移除o对象
~~~
> List子接口：称为有序的Collection
> 	特点：有序、有下标、元素可重复

~~~java
方法：
	void add(int index,Object o) :在index位置插入对象o
	boolean addAll(int index,Collectin c)：将一个集合中的元素添加到此集合中的index位置
	Object get(int index): 返回集合中指定位置的元素
	List subList(int fromIndex, int toIndex):返回fromIndex和toIndex之间的集合元素
~~~

> ArrayList 【重要】：
> 	JDK1.7之前是创建时初始化一个长度为10 的空间，但JDK7之后（包括JDK1.7）他就不分配内存空间了，只有在使用时才分配空间
> 特点：
> 	数组结构实现、查询快、增删慢
> 	JDK1.2版本，运行效率快，线程不安全

> Vector：
> 	数组结构实现、查询快、增删慢
> 	JDK1.0版本，运行效率慢、线程安全

> LinkedList:
> 	链表结构实现，增删快，查询慢

![image-20200517163759695](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200517163759695.png)

> 泛型集合：
> 	概念：参数化类型，类型安全的集合，强制集合元素的类型必须一致
> 	特点：
> 		编译时即可检查，而非运行时抛出异常
> 		访问时，不必类型转换（拆箱）
> 		不同泛型之间引用不能相互赋值，泛型不存在多态

### 多线程



## 垃圾回收

1.对象已死吗？：垃圾收集器在对堆进行回收前，第一件事就是要确定这些对象之中哪些还“活着”，哪些已经“死去”（即不可能再被任何途径使用的对象）

- 方法

  - 1.引用计数算法：给对象中添加一个引用计算器，每当有一个地方引用它时，计数器就加1，当引用失效时，计数器就减1；任何时刻计数器为0的对象就是不可能再被使用的

  

  优缺点：实现简单，判定效率也很高，但最致命的缺点是它很难解决对象之间循环引用的问题。

  比如：A对象引用B对象，B对象引用A对象，除此之外，两者对象无任何引用，但此时引用计数不为0，导致引用计数算法无法通知GC收集器回收它们

  - 2.可达性分析算法：主流的商用程序语言（Java、C#，甚至包括前面提到的古老的Lisp）的主流实现都是这个算法来判断对象是否存活的。

    基本思想：通过一系列的称为“GC Roots”的 对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链，当一个对象到GC Roots没有任何引用链相连（用图论的话来说，就是从GC Root到这个对象不可达）时，则证明此对象是不可用的。

  - 在Java语言中，可作为GC Roots的对象包括下面几种：

    - 虚拟机栈（栈帧中的本地变量表）中引用的对象
    - 方法区中类静态属性引用的对象
    - 方法区中常量引用的对象
    - 本地方法栈中JNI（即一般说的Native方法）引用的对象

![image-20200519194108726](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200519194108726.png)

#### 面向对象五大基本原则是什么

- **单一职责原则SRP**(Single Responsibility Principle)
  类的功能要单一，不能包罗万象，跟杂货铺似的
- **开放封闭原则OCP**(Open－Close Principle)
  一个模块对于拓展是开放的，对于修改是封闭的，想要增加功能热烈欢迎，想要修改，哼，一万个不乐意。
- **里式替换原则LSP**(the Liskov Substitution Principle LSP)
  子类可以替换父类出现在父类能够出现的任何地方。比如你能代表你爸去你姥姥家干活。哈哈~~
- **依赖倒置原则DIP**(the Dependency Inversion Principle DIP)
  高层次的模块不应该依赖于低层次的模块，他们都应该依赖于抽象。抽象不应该依赖于具体实现，具体实现应该依赖于抽象。就是你出国要说你是中国人，而不能说你是哪个村子的。比如说中国人是抽象的，下面有具体的xx省，xx市，xx县。你要依赖的抽象是中国人，而不是你是xx村的。
- **接口分离原则ISP**(the Interface Segregation Principle ISP)
  设计时采用多个与特定客户类有关的接口比采用一个通用的接口要好。就比如一个手机拥有打电话，看视频，玩游戏等功能，把这几个功能拆分成不同的接口，比在一个接口里要好的多。



### String

> 常用方法

- public charAt(int index) : 根据下标获取字符
- public boolean contains(String str) : 判断当前字符串中是否包含str 
- public char[] toCharArray : 将字符串转换成数组
		> public char[] toCharArray() {
   
        ​    // Cannot use Arrays.copyOf because of class initialization order issues
        
          //生成字符串数组副本给新数组，保证原数组不被修改
        
        ​    char result[] = new char[value.length];
        ​    System.arraycopy(value, 0, result, 0, value.length);
        ​    return result;
        
        }
   
-  public int indexof（String str) :查找str首次出现的下标，存在，则返回该下标，不存在，则返回-1
- public int lastIndexof(String str) : 查找字符串在当前字符串中最后一次出现的下标索引
- public int length() : 返回字符串的长度
- public String trim() :去掉字符串前后的空格
- public String toUpperCase() :将小写转成大写
- public boolean endWith(String str) :判断字符串是否以str结尾
- public String replace(char oldChar, char newChar) ：将旧字符串替换成了新字符串
- public String[] split(String str) ：根据str做拆分成字符串数组
- public String subString(String str) ：返回一个新的字符串，它是此字符串的一个字序列串

#### finalize() 方法

- 当对象被判定为垃圾对象时，有JVM自动调用此方法，用以标记垃圾对象
- 垃圾对象：没有有效引用指向此对象时，为垃圾对象
- 垃圾回收：由GC销毁垃圾对象，释放数据存储空间
- 自动回收机制：JVM的内存耗尽，一次性回收所有垃圾随想
- 手动回收机制：使用System.gc()；通知JVM执行垃圾回收





**类的加载顺序**。

(1) 父类静态对象和静态代码块

(2) 子类静态对象和静态代码块

(3) 父类非静态对象和非静态代码块

(4) 父类构造函数

(5) 子类 非静态对象和非静态代码块

(6) 子类构造函数



### Java四种引用

只要引用存在，垃圾回收器永远不会回收
Object obj = new Object();
//可直接通过obj取得对应的对象 如obj.equels(new Object());
而这样 obj对象对后面new Object的一个强引用，只有当obj这个引用被释放之后，对象才会被释放掉，这也是我们经常所用到的编码形式。

#### **强引用：**

只要引用存在，垃圾回收器永远不会回收
Object obj = new Object();
//可直接通过obj取得对应的对象 如obj.equels(new Object());
而这样 obj对象对后面new Object的一个强引用，只有当obj这个引用被释放之后，对象才会被释放掉，这也是我们经常所用到的编码形式。

#### **软引用：**

非必须引用，内存溢出之前进行回收，可以通过以下代码实现
Object obj = new Object();
SoftReference<Object> sf = new SoftReference<Object>(obj);
obj = null;
sf.get();//有时候会返回null
这时候sf是对obj的一个软引用，通过sf.get()方法可以取到这个对象，当然，当这个对象被标记为需要回收的对象时，则返回null；
软引用主要用户实现类似缓存的功能，在内存足够的情况下直接通过软引用取值，无需从繁忙的真实来源查询数据，提升速度；当内存不足时，自动删除这部分缓存数据，从真正的来源查询这些数据。

#### **弱引用：**

第二次垃圾回收时回收，可以通过如下代码实现
Object obj = new Object();
WeakReference<Object> wf = new WeakReference<Object>(obj);
obj = null;
wf.get();//有时候会返回null
wf.isEnQueued();//返回是否被垃圾回收器标记为即将回收的垃圾
弱引用是在第二次垃圾回收时回收，短时间内通过弱引用取对应的数据，可以取到，当执行过第二次垃圾回收时，将返回null。
弱引用主要用于监控对象是否已经被垃圾回收器标记为即将回收的垃圾，可以通过弱引用的isEnQueued方法返回对象是否被垃圾回收器标记。

#### **虚引用：**

垃圾回收时回收，无法通过引用取到对象值，可以通过如下代码实现
Object obj = new Object();
PhantomReference<Object> pf = new PhantomReference<Object>(obj);
obj=null;
pf.get();//永远返回null
pf.isEnQueued();//返回是否从内存中已经删除
虚引用是每次垃圾回收的时候都会被回收，通过虚引用的get方法永远获取到的数据为null，因此也被成为幽灵引用。
虚引用主要用于检测对象是否已经从内存中删除。

> **对象的强、软、弱和虚引用**
> 在JDK 1.2以前的版本中，若一个对象不被任何变量引用，那么程序就无法再使用这个对象。也就是说，只有对象处于可触及（reachable）状态，程序才能使用它。从JDK 1.2版本开始，把对象的引用分为4种级别，从而使程序能更加灵活地控制对象的生命周期。这4种级别由高到低依次为：强引用、软引用、弱引用和虚引用。
>
> 
>
> **⑴强引用（StrongReference）**
> 强引用是使用最普遍的引用。==如果一个对象具有强引用，那垃圾回收器绝不会回收它。当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足的问题。== ps：强引用其实也就是我们平时A a = new A()这个意思。
>
> **⑵软引用（SoftReference）**
> ==如果一个对象只具有软引用，则内存空间足够，垃圾回收器就不会回收它；如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。==软引用可用来实现内存敏感的高速缓存（下文给出示例）。
> 软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收器回收，Java虚拟机就会把这个软引用加入到与之关联的引用队列中。
>
> **⑶弱引用（WeakReference）**
> ==弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。==
> 弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。
>
> **⑷虚引用（PhantomReference）**
> “虚引用”顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。==如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收。==
> 虚引用主要用来跟踪对象被垃圾回收器回收的活动。==虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列 （ReferenceQueue）联合使用。==当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之 关联的引用队列中