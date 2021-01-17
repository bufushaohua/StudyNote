# 剑指Offer



## 面试题2：实现Singleton模式

**不好的解法一：只适用于单线程环境**

> 优点：将构造函数设为私有函数，禁止他人创建实例。同时定义一个静态的实例，在需要的时候创建该实例
>
> 缺点：在单线程的时候工作正常，但多线程的情况下就会有问题
>
> ​	设想如果两个线程同时运行到判断instance是否为null的if语句，并且instance的确没有创建时，那么两个线程都会创建一个实例，此时类型Singleon1就不再满足单例模式的要求了

~~~java
/**
 * 代码实现：
 *   避免了重复创建，确保只创建一个实例
 */
public  sealed class Singleton1{
    
    private Singleton1(){ }
    
    private static Singleton1 instance = null;
    public static Singleton1 Instance{
        get{
            if(instance == null){
                instance = new Singleton1();
            }
            return instance;
        }
    }
}
~~~

**不好的解法二：虽然在多线程环境下能工作，但效率不高**

> 优点：在判断时，加上一个同步锁，这样能保证在多线程环境下也只能得到一个实例
>
> 缺点：每次通过属性Instance得到Singleton的实例，都会试图加上一个同步锁，但加锁是一个非常耗时的操作，我们在没有必要的时候应该尽量避免

~~~java
//代码实现
public  sealed class Singleton2{
    private Singleton2(){ }
    
    private static readonly object syncObj = new object();
    
    private static Singleton2 instance = null;
    public static Singleton2 Instance{
        get{
            lock(syncObj){
                 if(instance == null){
              		  instance = new Singleton2();
          		  }
            }
            return instance;
        }
    }
}
~~~

**可行的解法：加同步锁前后两次判断实例是否已存在**

> 只有当instance为null即没有创建时，需要加锁操作。当instance已经创建出来之后，则无需加锁。
>
> 两个if判断来提高效率，但代码实现起来比较复杂，容易出错

~~~java
//代码实现
public  sealed class Singleton3{
    private Singleton3(){ }
    
    private static readonly object syncObj = new object();
    
    private static Singleton3 instance = null;
    public static Singleton3 Instance{
        get{
            if(instance == null){
                  lock(syncObj){
                     if(instance == null){
                          instance = new Singleton3();
                      }
             	  }
            }
            return instance;
        }
    }
}
~~~

## 面试题3：数组中重复的数字

:one: ==题目一：找出数组中重复的数字==

>   在一个长度为n的数组里所有数字都在0~n-1的范围内，数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。例如：如果输入长度为7的数组{2,3,1,0,2,5,3},那么对应的输出的重复的数字2或者3

**方法一：**

​	先把输入的数组排序，然后再从排序的数组中找出重复的数字。其中排序一个长度为n的数组需要O(nlog n)的时间

**方法二：**

​	使用哈希表来解决这个问题。从头到位扫描数组的每个数字，每扫描到一个数字的时候，都可以用O(1)的时间来判断哈希表里是否已经包含了该数字。如果哈希表中没有，则把它加入到哈希表中。如果哈希表中有，就找到一个重复的数字。算法的时间复杂度是O(n)，但空间复杂度也为O(n)

**方法三：**

​	如果数组中没有重复的数字，那么当数组排序后数字 i 将出现在下标为 i 的位置。由于数组中有重复的数字，有些位置可能存在多个数字，同时有些位置可能没有数字

>思想：
>
>​	从头到尾一次扫描这个数组的每个数字。当扫描到下标为i的数字时，首先比较这个数字（用m表示）是不是等于i。如果是，则接着扫描下一个数字。如果不是，则再拿它和第m个数字进行比较。如果它和第m个数字相等，就找到了一个重复的数字（该数字在下标为i和m的位置都出现了）；如果它和第m个数字不相等，就把第i个数字和第m个数字交换，把m放到属于它的位置。重复这个比较，交换的过程中，直到我们发现一个重复的数字

~~~java
//代码实现
bool duplicate(int numbers[],int length, int * duplication){
    if(numbers == null || length <= 0){
        return false;
    }
    for(int i = 0; i < length; ++i){
        if(numbers[i] < 0 || numbers[i] > length - 1){
            return false;
        }
    }
    for(int i = 0; i< length; ++i){
        while(numbers[i] != i){
            if(numbers[i] == numbers[numbers[i]]){
                *duplication = numbers[i];
                return true;
            }
            
            // swap numbers[i] and numbers[numbers[i]]
            int temp = numbers[i];
            numbers[i] = numbers[temp];
            numbers[temp] = temp;
        }
    }
    return false;
}
~~~

:two:==题目二：不修改数组找出重复的数字==

>   在一个长度为n+1的数组里的所有数字都在1~n的范围内，所以数组中至少有一个数字是重复的。请找出数组中任意一个重复的数字，但不能修改输入的数组。例如：如果输入长度为i8de数组{2,3,5,4,3,2,6,7}，那么对应的输出是重复的数字2或者3

**方法一**：

​	创建一个长度为n+1的辅助数组，然后逐一把原数组的每个数字复制到辅助数组中下标对应该数字的位置上，这样就很容易就能发现那个数字是重复的，不过该方案需要O(n)的辅助空间

**方法二：**

​	将1~n的数字从中间的的数字m分为两部分，前面一半1~m，后面m+1~n。如果前者的数目超过m，那么这一半的区间里一定有重复的数字；否则，另一半m+1~n的区间里一定包含重复的数字，接着可以继续把包含重复数字的区间一分为二，直到找到一个重复的数字。有点类似于二分查找算法，只是多了一步统计区间里数字的数目

~~~java
//代码实现
int getDuplication(Const int *numbers, int length){
    if(numbers == null || length <= 0){  //数组判空，长度判非正
        return -1;
    }
    int start =1;
    int end = length -1;
    while(end >= start){  //循环找出重复区间
        int middle = (end - start)>>1 + start;   //求中间数字
        int count = countRange(numbers, length, start, middle); 
        if(end == start){  //区间只为1个数时
            if(count > 1){  //重复次数 > 1,该数就为重复数 
                return start;
            }else{
                break;
            }
        }
        if(count > (middle - start + 1)){  //判断重复区间是否是前半部分
            end = middle;
        }else{
            start = middle + 1;
        }
    }
    return -1;
}

int countRange(const int *numbers, int length, int start, int end){
    if(numbers == null){  //数组判空
        return 0;
    }
    int count =0;
    for(int i=0; i<length; i++){
        if(numbers[i] >= start && numbers[i] <= end){
            ++count; //存在于[start,end]区间，次数加1
        }
    }
    return count;
}
~~~

> 如果输入长度为n的数组，那么函数countRange将被调用O(log n)次，每次需要O(n)的时间，因此总的时间复杂度是O(nlog n)，空间复杂度O(1)，相比法一，这种是==以时间换空间==

*==注意==*：该算法不能保证找出所有重复的数字。例如，该算法不能找出数组{2,3,5,4,3,2,6,7}中重复的数字2。因为在1~2的范围里有1和2两个数字，这个范围的数字也出现2次，此时，我们无法确定是每个数字个出现一次，还是某个数字出现了两次



## 面试题4：二维数组中的查找

> 题目：
>
> ​	  在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
>
> ​	【二维数组在内存中占据连续的空间。在内存中从上到下存储各行元素，在同一行中按照从左到右的顺序存储】

![image-20201220171438282](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20201220171438282.png)

如按上述数组查找数字7为例，首先选取数组中右上角的数字进行比较。若相等，则查找结束，若该数字大于要查找的数字，则去除该数所在的列；若该数字小于要查找的数字，则去除该数所在的行。重复上述操作，缩小查找的范围，直到找到要查找的数字，或者查找范围为空。

~~~java
//代码实现
bool Find(int *matrix, int rows, int columns, int number){
    bool found = false;
    if(matrix != null && rows > 0 && columns > 0){
        //起初行数为1行，列数为最大列数
        int row = 0;
        int column = columns -1 ;
        while(row < rows && column >= 0){
            if (matrix[row * columns + column] == number){  //相等，查找结束
                found = true;
                break;
            }else if(matrix[row * columns + column] > number){  //大于要查找的数字,去除列
                --column;
            }else{  //小于要查找的数字，去除行
                ++row;
            }
        }
    }
    return found;
}
~~~

*注意：*也可以选取左下角的数字，但是不能选择左上角数字或者右下角的数字。以左上角数字为例，由于最初数字1小于7，那么7应该位于1的右边或者下边。此时我们既不能从查找范围内剔除1所在的行，，也不能剔除1所在的列，这样我们就无法缩小查找范围。