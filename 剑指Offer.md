# 剑指Offer

## 一、归类

> ## **数据结构类题目**
>
> **LinkedList**
>
> 面试题06-从尾到头打印链表
>
> 面试题22-链表中倒数第k个结点
>
> 面试题24-反转链表
>
> 面试题25-合并两个排序的链表
>
> 面试题35-复杂链表的复制
>
> 面试题52-两个链表的第一个公共节点
>
> 面试题18-删除链表的节点
>
> 
>
> **Tree**
>
> 面试题07-重建二叉树
>
> 面试题26-树的子结构
>
> 面试题27-二叉树的镜像
>
> 面试题32-1 -从上往下打印二叉树
>
> 面试题32-2 -从上往下打印二叉树 2
>
> 面试题32-3 -从上往下打印二叉树 3
>
> 面试题33-二叉搜索树的后序遍历序列
>
> 面试题34-二叉树中和为某一值的路径
>
> 面试题36-二叉搜索树与双向链表
>
> 面试题55-1-二叉树的深度
>
> 面试题55-2-平衡二叉树
>
> 面试题28-对称的二叉树
>
> 面试题37-序列化二叉树
>
> 面试题54-[二叉搜索树的第k大节点](https://link.zhihu.com/?target=https%3A//leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof)
>
> 
>
> **Stack & Queue**
>
> 面试题09-用两个栈实现队列
>
> 面试题30-[包含min函数的栈](https://link.zhihu.com/?target=https%3A//leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof)
>
> 面试题31-栈的压入、弹出序列
>
> 面试题58-1-翻转单词顺序
>
> 面试题59-1-滑动窗口的最大值
>
> 
>
> **Heap**
>
> 面试题40-最小的K个数
>
> 
>
> **Hash Table**
>
> 面试题50-第一个只出现一次的字符
>
> 
>
> **图**
>
> 面试题12-矩阵中的路径(BFS)
>
> 面试题13-机器人的运动范围(DFS)
>
> ## **具体算法类题目**
>
> **斐波那契数列**
>
> 面试题10-1-斐波拉契数列
>
> 面试题10-2-青蛙跳台阶问题
>
> 
>
> **搜索算法**
>
> 面试题04-二维数组中的查找
>
> 面试题11-旋转数组的最小数字（二分查找）
>
> 面试题56-1-数组中数字出现的次数（二分查找）
>
> 
>
> **全排列**
>
> 面试题38-字符串的排列
>
> 
>
> **动态规划**
>
> 面试题42-连续子数组的最大和
>
> 面试题19-正则表达式匹配(我用的暴力)
>
> 
>
> **回溯**
>
> 面试题12-矩阵中的路径(BFS)
>
> 面试题13-机器人的运动范围(DFS)
>
> 
>
> **排序**
>
> 面试题51-数组中的逆序对(归并排序)
>
> 面试题40-最小的K个数(堆排序)
>
> 
>
> **位运算**
>
> 面试题15-二进制中1的个数
>
> 面试题16-数值的整数次方
>
> 
>
> **其他算法**
>
> 面试题05-替换空格
>
> 面试题21-调整数组顺序使奇数位于偶数前面
>
> 面试题39-数组中出现次数超过一半的数字
>
> 面试题43- 1～n整数中1出现的次数
>
> 面试题45-把数组排成最小的数
>
> 面试题49-丑数
>
> 面试题57-2-和为S的连续正数序列(滑动窗口思想)
>
> 面试题57-和为S的两个数字(双指针思想)
>
> 面试题58-2-左旋转字符串(矩阵翻转)
>
> 面试题62-圆圈中最后剩下的数(约瑟夫环)
>
> 面试题66-构建乘积数组

## 二、数组

### 面试题2：实现Singleton模式

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

### 面试题3：数组中重复的数字

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



### 面试题4：二维数组中的查找

> 题目：
>
> ​	  在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
>
> ​	【二维数组在内存中占据连续的空间。在内存中从上到下存储各行元素，在同一行中按照从左到右的顺序存储】

![image-20201220171438282](D:\Files\Note\Typora\PNG\剑指Offer.assets\image-20201220171438282.png)

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



## 三、字符串

### 面试题5：替换空格

> 题目：请实现一个函数，吧字符串中的每个空格替换成“%20”。例如，输入“We are happy”，则输出“We%20are%20happy” 。转换规则是在'%'后面跟上ASCII码的两位十六进制的表示
>
> ①、创建新的字符串并在新的字符串上进行替换，那么我们可以分配足够多的内存
>
> ②、在原来的字符串上进行替换，并且保证输入的字符串后面有足够多的空闲内存

**方法一：时间复杂度为O(n^2)，面试的时候可能不会取得高分**

​	最直观的做法是从头到尾扫描字符串，每次碰到空格字符的时候进行替换。假设字符串的长度是n。对每个空格字符，需要移动后面O(n)个字符，因此对于含有O(n)个字符的字符串而言，总的时间效率是O(n^2)

**方法二：时间复杂度为O(n)的解法**

​	可以先遍历一次字符串，统计出字符串中空格的总数，从而计算出替换之后的字符串的总长度。从字符串的后面开始复制和替换。

​	首先准备两个指针：P1和P2。P1指向原来字符串的末尾，而P2指向替换之后的字符串的末尾。接下来向前移动指针P1，逐个把它指向的字符复制到P2指向的位置，直到碰到第一个空格为止。

​	碰到第一个空格之后，把P1向前移动1格，在P2之前插入字符串“%20”，接着P1再向前移动，P2再向前复制，直到碰到第二个空格，重复上述动作。

​	当P1和P2指向同一位置时，表明所有空格都已经替换完毕。

![image-20201222221305931](D:\Files\Note\Typora\PNG\剑指Offer.assets\image-20201222221305931.png)

~~~java
//代码实现
//length为字符数组string的总容量
void ReplaceBlank(char string[], int length){
    if(string == nul || length <= 0){
        return ;
    }
    //originalLength为字符串string的实际长度
    int originalLength = 0;
    int numberOfBlank = 0;
    int i = 0;
    while(string[i] != '\0'){
        ++originalLength; //除去'\0'的实际长度
        if(string[i] == ''){
            ++numberOfBlank; //空格个数
        }
        ++i;
    }
    //newLength为把空格替换成“%20”之后的长度
    int newLength = originalLength + numberOfBlank * 2;
    if(newLength > length) //替换后的长度大于数组总长度，则无法替换
        return;
   	int indexOfOriginal = originalLength;
    int indexOfNew = newLength;
    while(indexOfOriginal >= 0 && indexOfNew > indexOfOriginal){
        if(string[indexOfOriginal] == ''){
            string[indexOfNew--] = '0';
            string[indexOfNew--] = '2';
            string[indexOfNew--] = '%';
        }
        else{
            string[indexOfNew--] = string[indexOfOriginal];
        }
        --indexOfOriginal;
    }
}
~~~



## 四、链表

### 面试题6：从尾到头打印链表

> 题目：输入一个链表的头结点，从尾到头反过来打印出每个节点的值。链表节点定义如下：
>
> struct ListNode{
>
> ​	int value;
>
> ​	ListNode* next;
>
> }

**方法一：改变链表的方向**

​	把链表中链接节点的指针反转过来，改变链表的方向，然后就可以从头到尾输出了

**方法二：在不许修改链表结构的基础上实现**

​	可以使用栈来实现“后进先出”的顺序

~~~c++
// C++代码如下
void PrintListReverse(ListNode* pHead){
    std:stack<ListNode*> nodes;  // 从库中创建一个栈
    ListNode* pNode = pHead;  // 获取指向头结点的指针
    while(pNode != null){	// 所有数据元素入栈
        node.push(pNode);
        pNode = pNode->next;
    }
    while(!nodes.empty()){  // 依次遍历栈，直到栈空
        pNode = nodes.top();
        cout<<pNode->value;
        nodes.pop();
    }
}
~~~

​	==递归在本质上就是一个栈结构==，可以用递归来实现【每访问到一个结点的时候，先递归输出它后面的结点，在输出该节点自身，这样输出的结果就反过来了】

~~~C++
// C++代码如下
void PrintListReverse(ListNode* pHead){
    if(pHead != null){
        if(pHead->next != null){  // 先输出其后结点
            PrintListReverse(pHead->next);
        }
        cout<<pHead->value; // 最后自身
    }
}
/**
* 有一个问题：当链表非常长的时候，就会导致函数调用的层级很深，从而有可能导致函数调用栈溢出。显然用栈基于循环实现的代码的鲁棒性要好一些
/
~~~



## 五、树

### 面试题7：重建二叉树

> 题目：输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。
>
> 假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如：输入前序遍历序列{1,2,4,7,3,5,6,7}和中序遍历{4,7,2,1,5,3,8,6}，则重建下图的二叉树并输出它的头结点。二叉树结点的定义如下：
>
> struct BinaryTreeNode{
>
> ​	int value;  // 值
>
> ​	BinaryTreeNode* left;  // 左节点
>
> ​	BinaryTreeNode* right;  // 右节点
>
> }



## 七、递归和循环

### 面试题10：斐波那契数列

> 题目一：求斐波那契数列的第n项
>
> 写一个函数，输入n，求斐波那契数列的第n项。斐波那契数列的定义如下：
>
> ​	f(n) = { 0,(n=0);
>
> ​			 1,(n=1);
>
> ​			 f(n-1)+f(n-2),(n>1);			
>
> ​			}





### 快速排序

~~~java
//代码实现
//1.先在数组中选择一个数字，接下来把数组中的数字分为两部分，比选择的数字小的数字移到数组的左边，比选择的数字大的数字移到数组的右边
int Partition(int data[], int length, int start, int end){
    if(data == null ||length <= 0 || start < 0|| end >= length){
        throw new std:exception("Invalid Parameters");
    }
    //函数RandomInRange用来生成一个在start和end之间的随机数
    int index = RandomInRange(start, end);
    //函数Swap的作用是用来交换两个数字
    Swap(&data[index], &data[end]);//将选中的数字换到最后一位上
    
    int small = start - 1; //设定最小值下标
    for(index = start; index < end; ++index){ // 数组遍历到最后一位的前一位
        if(data[index] < data[end]){ //与选中的数字进行比大小
            ++small; //小于，则最小值下标加1
            if(small != index){
                Swap(&data[index], &data[small]);
            }
        }
    }
    
    ++small;
    Swap(&data[small], &data[end]);
    
    return small;
}

//2.接下来可以用递归的思路分别对每次选中的数字的左右两边排序
void QuickSort(int data[], int length, int start, int end){
    if(start == end){
        return;
    }
    int index = Partition(data, length, start, end);
    if(index > start)
        QuickSort(data, length, start, index-1);
    if(index < end)
        QuickSort(data,length, index +1, end);
}
~~~



### 实例[对公司所有员工的年龄排序]

> 要求： 排序算法，时间效率O(n),年龄数字在一个较小的范围之内，可以使用辅助空间，但只允许使用常量大小的辅助空间，不得超过O(n)

~~~java
//代码实现
void SortAges(int ages[], int length){
    if(ages == null || length <= 0)
        return;
    const int oldestAge = 99;
    int timesOfAge[oldestAge + 1];
    for(int i = 0; i <= oldestAge; ++i){
        timesOfAge[i] = 0;
    }
    for(int i = 0; i< length; ++i){
        int age = ages[i];
        if(age < 0 || age > oldestAge){
            throw new std::exception("age out of range");
        }
        ++timesOfAge[age];
    }
    int index = 0;
    for(int i = 0; i<= oldestAge; ++i){
        for(int j = 0; j < timesOfAge; ++i){
            ages[inde] = i;
            ++index;
        }
    }
}
~~~



## 八、查找和排序

### 面试题11：旋转数组的最小数字

> 题目：把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。
>
> 例如：数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1

**方法一：最直观的解法**

​	从头到尾遍历数组一次，我们就能找到最小的元素，时间复杂度是O(n)，但这种思路没有利用到输入的旋转数组的特性，可能达不到面试官的要求

**方法二：依据排序数组，可以用二分查找法实现O(log n)**

​	注意到旋转后的数组实际上可以划分为两个排序的子数组，而且前面子数组的元素都大于或者等于后面子数组的元素。最小的元素正好是这两个子数组的分界线。

思想：

​	1.用两个指针分贝指向数组的第一个元素和最后一个元素，按理说，第一个元素应该是大于或者等于最后一个元素的*（这其实不完全对，特殊情况：如果把排序数组的前面的0个元素搬到最后面，即排序数组本身，这仍然是数组的一个旋转。此时，数组中的第一个数字就是最小的数字，可以直接返回）*

​	2.找到中间的元素。若中间元素大于或者等于第一个指针指向的元素，说明该中间元素位于前面的递增子数组。此时数组中最小的元素应该位于该中间元素的后面。我们可以把第一个指针指向该中间元素，这样移动之后的第一个指针仍然位于前面的递增子数组，从而可以缩小寻找的范围

​	3.同样，如果中间元素小于或者等于第二个指针指向的元素，说明该中间元素位于后面的递增子数组。此时数组中最小的元素应该位于该中间元素的前面。我们可以把第二个指针指向该中间元素，这样移动之后的第二个指针仍然位于后面的递增子数组，从而也可以缩小寻找的范围

​	4.接下来在用更新后的两个指针重复做新一轮的查找。最终第一个指针将指向前面子数组的最后一个元素，而第二个指针会指向后面子数组的第一个元素。也就是它们最终会指向两个相邻的元素。而第二个指向的刚好是最小的元素。这就是循环结束的条件

![img](D:\Files\Note\Typora\PNG\剑指Offer.assets\985F4C12B2C5CA5EDCF0ACC0C34524DD.jpg)

~~~java
//代码如下
/**
* 
*/
int Min(int* numbers, int length ){
    if(numbers == null || length <= 0)
        throw new std::exception("Invalid parameters");
    int index1 = 0;
    int index2 = length - 1;
    /**
    * 为什么把indexMid初始化为index1 ?
    * 因为一旦出现上述说明的特例，即发现数组中第一个数字小于最后一个数字，表名该数组是排序的，就可以直接返回第一个数字
    */
    int  indexMid = index1;
    while(numbers[index1] >= numbers[index2]){
        if(index2 -index1 == 1){ //当两指针相邻，结束循环
            indexMid = index2;
            break;
        }
     	indexMid = (index1 + index2)/2; // 中间元素
    	if(numbers[indexMid] >= numbers[index1]){
            index1 = indexMid;
        }else if(numbers[indexMid] <= numbers[index2]){
            index2 = indexMid;
        }
    }
    return numbers[indexMid];
}
~~~



上述代码还是有点问题，假如有两个这样的数组{1,0,1,1,1}和数组{1,1,1,0,1}都可以看成递增排序数组{0,1,1,1,1}的旋转

==注意：==**在这两个数组中，第一个数字、最后一个数字和中间数字都是1，我们无法确定中间的数字1是属于第一个递增子数组还是属于第二个递增子数组，因此也就无法移动两个指针来缩小查找的范围。此时，不得不采用顺序查找的方法**

~~~java
// 修改后的代码如下
int Min(int* numbers, int length){
    if(numbers == null || length <= 0){
        throw new std:exception("INvalid parameters");
    }
    int index1 = 0;
    int index2 = length -1;
    int indexMid = index1;
    while(numbers[index1] >= numbers[index2]){
        if(index2 -index1 == 1){
            indexMid = index2;
            break;
        }
        indeMid = (index1 + index2)/2;
        //如果下标为index1、index2和indexMid指向的三个数字相等
        //则只能顺序查找
        if(numbers[index1] == numbers[index2] && numbers[indexMid] == numbers[index1]){
            return MinInOrder(numbers, index1, index2);
        }
        if(numbers[indexMid] >= numbers[index1]){
            index1 = indexMid;
        }else if(numbers[indexMid] <= numbers[index2]){
            index2 = indexMid;
        }
    }
    return numbers[indexMid];
}

int MinInOrder(int* numbers, int index1, int index2){
    int result = numbers[index1];
    for(int i = index1 +1; i <= index2; ++i){
        if(result > numbers[i]){
            result = numbers[i];
        }
    }
    return result;
}
~~~



## 十一、位运算

### 面试题15：二进制中1的个数

> 题目：请实现一个函数，输入一个整数，输出该数二进制表示中1的个数。例如：把9表示成二进制是1001，有2位是1.因此，如果输入9，则该函数输出2。

==**方法一：可能引起死循环的解法**==

​	基本思路：1.先判断整数二进制最右边一位是不是1。2.把输入的整数右移一位，此时原来处于从右边数起的第二位被移到最右边，再判断是不是1。3.重复1的动作，直到整个整数变为0为止。

~~~java
//代码如下
int NumberOf1(int n){
    int count = 0;
    while(n){
        if(n & 1) count++;
        n = n >> 1;
    }
    return count;
}
~~~

==注意上述存在两个问题：==

​	==1==.把整数右移一位和把整数除以2在数学上是等价的，那上面的代码中可以把右移运算换成除以2吗？

​	答案是否定的。因为除以的效率比移位运算要低得多，在实际编程中应尽可能地用移位运算代替乘除法。

​	==2==.上面的函数如果输入一个负数，比如Ox80000000，则运算的时候会发生什么情况？【负数右移情况特例】

​	当把负数Ox80000000右移一位的时候，并不是简单地把最高位的1移到低二位变成Ox40000000，而是OxC0000000。这是因为移位前是一个负数，仍然要保证移位后是一个负数，因此移位后的最高位会设为1。如果一直做右移运算，那么最终这个数字就会变成OxFFFFFFF而陷入死循环。

==**方法二：常规解法**==

​	为避免出现方法一中的死循环，这次我们不改变原整数二进制。

​	1.首先把n和1做与运算，判断n的最低位是不是为1.接着把1左移一位得到2，再和n做与运算，就能判断n的最低位是不是为1。2.接着把1左移一位得到2，再和n做与运算，就能判断n的次低位是不是1... 3.反复循环左移，每次都能判断n的其中一位是不是1。

~~~java
//代码如下
int NumberOf1(int n){
    int count = 0; //1的个数
    unsigned int flag = 1; 
    while(flag){
        if(n & flag){
            count ++;
        }
        flag = flag << 1;
    }
    return count;
}
~~~

==提示：==这种解法循环的次数等于整数二进制的位数，32位的整数需要循环32次 

==**方法三：Nice解法**==

思想：

​		1、从右到左消去（将“1”变为“0”）第一个出现“1”的位数：将该整数减去1，则该二进制最右边的“1”变为“0”，同时如果它的右边位数还有“0”，则所有的“0”都变成“1”【例：0100100 - 1 ——> 0100011】

​		2、将步骤1中得到的结果与原整数做一次“与”操作，相当于把上一步中由“0”变为“1”的位数又重新变回来【例：0100011 & 0100100 ——> 0100000】，此时计数器自增1

​		3、重复上述操作，直到整数变为“0”为止。

~~~java
//代码如下
int NumberOf1(int n){
    int count = 0;
    while(n){
        ++count;
        n = (n-1) & n;
    }
    return count;
}
~~~



## 十二、代码的完整性

### 面试题19：正则表达式匹配

~~~txt
题目：请实现一个函数用来匹配包含“.”和“*”的正则表达式。模式中的字符“.”表示任意一个字符，而“*”表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是字符串的所有字符匹配整个模式。例如，字符串“aaa”与模式“a.a”和“ab*ac*a”匹配，但与“aa.a”和“ab*a”均不匹配  
~~~

思路：

​	1、如果模式中的字符ch是'.'，那么它可以匹配字符串中的任意字符。如果模式中的字符ch不是'.'，而且字符串中的也是ch，那么它们相互匹配，当字符串中的字符和模式中的字符相匹配时，接着匹配后面的字符

​	2、当模式中的第二个字符不是'*'时，如果字符串中的第一个字符和模式中的第一个字符相匹配，那么字符串和模式都向后移动一个字符，然后在匹配剩下的字符串和模式。如果字符串中的第一个字符和模式中第一个字符不相匹配，则直接返回false。

​	3、当模式中的第二个字符是‘*’时，问题要复杂一些。如果模式中的第一个字符和字符串中的第一个字符相匹配，则在字符串上向后移动一个字符。而在模式上有两种选择：可以在模式上向后移动两个字符，也可以保持模式不变。

~~~java
//代码如下
bool match(char* str, char* pattern){
    if(str == null || pattern == null){
        return false;
    }
    return matchCore(str, pattern);
}
bool matchCore(char* str, char* pattern){
    if(*str == '\0' && *pattern == '\0'){
        return true;
    }
    if(*str != '\0' && *pattern == '\0'){
        return false;
    }
    if(*(pattern + 1) == '*'){
        if(*pattern == *str || (*pattern == '.' && *str != '\0')){
            return matchCore(str +1, pattern +2) //move on the next state
                || matchCore(str +1, pattern)   //stay on the current state
                || mathCore(str, pattern +2);  //ignore a '*' 
        }else{
            return matchCore(str, pattern +2);  //ignore a '*'
        }
    }
    if(*str == *pattern || (*pattern == '.' && *str != '\0')){
        return matchCore(str + 1, pattern +1);
    }
    return false;
}
~~~



### 面试题20：表示数值的字符串

> ​		题目：请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串“+ 100”、“5e2”、“-123”、“3.1416”及'"-1E-16"都表示数值，但“12e”、“1a3.14”、“1.2.3”、“+-5”等等都不是

==思路：==表示数值的字符串遵循模式`A[.[B]][e|EC]`或者`.B[e|EC]`，其中A为数值的整数部分，B紧跟着小数点为数值的小数部分，C紧跟着‘e’或者'E'为数值的指数部分。如果一个数没有整数部分，那么它的小数部分不能为空。

~~~java
//代码如下
/*
* 数字的格式可以用A[.[B]][e|EC]或者.B[e|EC]表示，其中A和C都是整数（可以有正负号，也可以没有），而B是一个无符号* 整数
*/
bool isNumeric(const cahr* str){
    if(str == null){
        return false;
    }
    bool numeric = scanInteger(&str);
    //如果出现‘。’，则接下来是数字的小数部分
    if(*str == '.'){
        ++str;
        /*
        * 下面代码使用||的原因
        * 1. 小数可以没有整数部分，如.123等于0.123
        * 2. 小数点后面可以没有数字，如233等于233.0
        * 3. 当然，小数点前面和后面可以都有数字，如233.334
        */
        numeric = scanUnsignedInteger(&str)||numeric;
        // 如果出现‘e’或者‘E’，则接下来是数字的指数部分
        if(*str == 'e' || *str =='E'){
            ++str;
            //下面代码用&&的原因
            // 1.当e或E前面没有数字时，整个字符串不能表示数字，如.e1、e1
            // 2.当e或E后面没有整数时，整个字符串不能表示数字，如12e、12e+5.4
            numeric = numeric && scanInteger(&str);
        }
        return numeric && *str == ‘\0’;
    }
}

//函数scanUnsignedInteger扫描字符串中0~9的数位（类似于一个无符号整数），用来匹配B部分
bool scanUnsignedInteger(const char** str){
    const char* before = *str;
    while(**str != '\0' && **str >= '0' && *str <='9'){
        ++(*str);
    }
    // 当str中存在若干0~9的数字时，返回true
    return *str > before;
}

//该函数scanInteger扫描可能以表示正负的‘+’或者'-'为其实的0~9的数值（类似于一个可能带正负符号的整数），用来匹配前面数值模式中的A和C部分
bool scanInteger(const char** str){
    if(**str == '+' || **str == '-'){
        ++(*str);
    }
    return scanUnsignedInteger(str);
}
~~~



### 面试题21：调整数组顺序是奇函数位于偶数前面

> ​		题目：输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分

**方法一：最简单思路**

​		思想：从头扫描这个数组，每碰到一个偶数，拿出这个数字，并把位于这个数字后面的所有数字往前移动一位。挪完之后在数组的末尾有一个空位，这时把该偶数放入这个空位。由于每碰到一个偶数就需要移动O(n)个数字，因此总的时间复杂度是O(n^2)。

**方法二：双指针法**

​		思想：一个从前往后移动，一个从后往前移动，前者始终位于后者之前。如果前者指向偶数并且后者指向奇数，则交换这两个数字。

~~~java
面试//代码如下
void RecorderOddEven(int *pData,unsigned int length){
    if(pData == null || length == 0){
        return;
    }
    int *pBegin = pData;
    int *pEnd = pData + length -1;
    while(pBegin < pEnd){
        //向后移动pBegin，直到它指向偶数
        while(pBegin < pEnd && (*pBegin & 0x1) != 0){ //pBegin不为偶数
            pBegin++;
        }
        //向前移动pEnd，直到它指向奇数
        while(pBegin < pEnd && (*pEnd &0x1) == 0){  //pEnd不为奇数
            pEnd--;
        }
        if(pBegin < pEnd){
            int temp = *pBegin;
            *pBegin = *pEnd;
            *pEnd = temp;
        }
    }
}
~~~



## 十三、代码的鲁棒性

### 面试题22：链表中倒数第k个节点

> ​		题目：输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头结点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的结点。

~~~java
//链表节点定义如下：
struct ListNode{
    int value;
    ListNode *next;
}
~~~

**方法一：简单思路**

​		遍历链表两次，第一次统计出链表中节点的个数，第二次就能找到倒数第k个节点。

**方法二：快慢指针**

​		为了实现只遍历链表依次一次就找到倒数第k个节点，可以定义两个指针。第一个指针从链表的头指针开始遍历向前走k-1步，第二个指针保持不动；从第k步开始，第二个指针也开始从链表的头指针开始遍历。由于两个指针的距离保持在k-1，当第一个（走在前面的）指针到达链表的尾节点时，第二个（走在后面的）指针正好指向倒数第k个节点 。

~~~java
//代码如下
ListNode* FindKthToTail(ListNode* pListHead, unsigned int k){
    ListNode *pAhead = pListHead;
    ListNode *pBehind = null;
    for(unsigned int i = 0; i < k-1; ++i){
        pAhead = pAhead -> next;
    }
    pBehind = pListHead;
    while(pAhead->next != null){
        pAhead = pAhead -> next;
        pBehind = pBehind -> next;
    }
    return pBehind;
}
~~~

==注意：==上述代码存在**鲁棒性问题**<!--所谓的鲁棒性是指程序能够判断输入是否合乎规范要求，并对不符合要求的输入予以合理的处理-->

（1）输入的pListHead为空指针 （2）输入的以PListHead为头结点的链表的结点总数少于k （3）输入的参数k为0

**问题解决方法：**

~~~java
//修改之后的代码
ListNode* FindKthToTail(ListNode *pListHead, unsigned int k){
    if(pListHead == null || k == 0){
        return null;
    }
    ListNode *pAhead = pListHead;
    ListNode *pBehind = null;
    for(unsigned int i = 0; i < k-1; ++i){
        if(pAhead->next != null){
            pAhead = pAhead->next;
        }else{
            return null;
        }
    }
    pBehind = pListHead;
    while(pAhead -> next != null){
        pAhead = pAhead -> next;
        pBehind = pBehind -> next;
    }
    return pBehind;
}
~~~



### 面试题23：链表中环的入口节点

> ​	题目：如果一个链表中包含环，如何找到环的入口节点？

**解决方法：**

​		==第一步：确定一个链表中是否包含环【使用双指针来解决】：==定义两个指针，同时从链表的头结点出发，一个指针走的慢，每次走一步；一个指针走的快，每次走两步。如果走的快的指针追上了走得慢的指针，那么链表就包含环；如果走的快的指针走到了链表的末尾（next指向null）都没有追上第一个指针，那么链表就不包含环。

​		==第二步：找到环的入口【使用双指针来解决】：==先定义两个指针P1和P2指向链表的头结点。如果链表中的环有n个节点，则指针P1先在链表上向前移动n步，然后两个指针以相同的速度向前移动。当第二个指针指向环的入口节点时，第一个指针已经围绕着环走了一圈，又回到了入口节点。

​		==最后一步：得到环中节点的数目：==前面第一步用到了一快一慢两个指针，两个指针相遇的节点一定是在环中。可以从这个节点出发，一遍继续向前移动一边技术，当再次回到这个节点时，就可以得到环中节点数

~~~java
//代码如下
//函数MeetingNode在链表中存在环的前提下找到一快一慢两个指针相遇的节点，如果链表中不存在环，则返回null
ListNode* MeetingNode(ListNode *pHead){
    if(pHead == null){
        return null;
    }
    ListNode *pSlow = pHead -> next;
    if(pSlow == null){
        return null;
    }
    ListNode *PFast = pSlow->next;
    while(pFast != null && pSlow != null){
        if(pFast == pSlow){
            return pFast;
        }
        //慢指针一次走一步
        pSlow = pSlow -> next;
        //快指针一次走两步
        pFast = pFast->next;
        if(pFast != null){
            pFast = pFast -> next;
        }
    }
    return null;
}

//函数EntryNodeOfLoopde得出环中的节点数目，并找到环的入口节点
ListNode* EntryNodeOfLoop(ListNode *pHead){
    ListNode *meetingNode = MeetingNode(pHead);
    if(meetingNode == null){
        return null;
    }
    //得到环中节点的数目
    int nodesInLoop = 1;
    ListNode *pNode1 = meetingNode;
    while(pNode->next != meetingNode){
        pNode1 =pNode1->next;
        ++nodeInLoop;
    }
    //先移动pNode1，次数为环中节点的数目
    pNode1 = pHead;
    for(int i = 0; i < nodesInLoop; ++i){
        pNode1 = pNode1 -> next;
    }
    //再移动pNode1和pNode2
    ListNode* pNode2 = pHead;
    while(pNode1 != pNode2){
        pNode1 = pNode1 ->next;
        pNode2 = pNode2 ->next;
    }
    return pNode1;
}
~~~



### 面试题24：反转链表

> ​		题目：定义一个函数，输入一个链表的头结点，反转该链表并输出反转后链表的头结点。

~~~java
//链表结点定义如下：
struct ListNode {
    int key;
    ListNode *next;
}
~~~

**问题思路分析：**我们需要定义三个指针，分别指向当前遍历到的节点，它的前一个节点及后一个节点。另外反转后链表的头结点，即原始链表的尾节点，自然是next为null的结点。

~~~java
//代码如下：
ListNode* ReverseList(ListNode* pHead){
    ListNode *pNode = pHead; //当前遍历的结点
    ListNode *pReversedHead = null; //反转后链表的头结点
    ListNode *pPrev = null; //当前遍历结点的前一节点
    
    while(pNode != null){
        ListNode *pNext = pNode -> next;
        if(pNext == null){
            pReversedHead = pNode;
        }
        pNode->next = pPrev;
        pPrev = pNode;
        pNode = pNext;
    }
    return pReversedHead;
}
~~~



### 面试题25：合并两个排序的链表

> ​		题目：输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

~~~java
//链表节点定义如下：
struct ListNode{
    int value;
    ListNode *next;
}
~~~

~~~java
//代码如下：
ListNode* Merge(ListNode *pHead1, ListNode *pHead2){
    //鲁棒性
    //当链表1为null，合并后的链表为链表2
    if(pHead1 == null){
        return pHead2;
    }else if(pHead2 == null){  //当链表2为null，合并后的链表为链表1
        return pHead1;
    }
    ListNode *pMergeHead = null;
    if(pHead1 -> value < pHead2 -> value){
        PMergedHead = pHead1;
        pMergedHead->next = Merge(pHead1->next, pHead2); //递归
    }else{
        pMergedHead = pHead2;
        pMergedHead->next = Merge(pHead1, pHead2->next); //递归
    }
    return pMergedHead;
}
~~~



### 面试题26：树的子结构

> ​		题目：输入两棵二叉树A和B，判断B是不是A的子结构。

~~~java
//二叉树结点的定义如下：
struct BinaryTreeNode{
    double value;
    BinaryTreeNode *left;
    BinaryTreeNode *right;
}
~~~

**问题思路分析：**第一步，在树A中找到和树B的根节点的值一样的节点R；第二步，判断树A中以R为根节点的子树是不是包含和树B一样的结构

~~~java
//代码如下：
//第一步在树A中查找与根节点的值一样的节点，可以用递归的方法或者用循环的方法去遍历
bool HashSubtree(BinaryTreeNode *pRoot1, BinaryTreeNode *pRoot2){
    bool result = false;
    if(pRoot1 != null && pRoot2 != null){
        if(Equal(pRoot1->value, pRoot2->value)){ //根节点匹配
            result = DoesTree1HaveTree2(pRoot1, pRoot2);
        }
        if(!result){  //遍历左节点是否匹配
            result = HashSubtree(pRoot1->left, pRoot2);
        }
        if(!result){ //遍历右节点是否匹配
            result = HashSubtree(pRoot->pRight,pRoot2);
        }
    }
    return result;
}

//第二步判断树A中以R为根节点的子树是不是和树B具有相同的结构，同样可以使用递归的思路来考虑
bool DoesTree1HaveTree2(BinaryTreeNode *pRoot1, BinaryTreeNode *pRoot2){
    //为了避免试图访问空指针而造成程序崩溃，同时也设置了递归调用的退出条件
    if(pRoot2 == null){
        return true;
    }
    if(pRoot1 == null){
        return false;
    }
    //判断根节点是否相同
    if(!Equal(pRoot1->value, pRoot2->value)){
        return false;
    }
    //递归判断他们各自的左右结点的值是不是相同，终止条件是到达了树A或者树B的叶结点
    return DoesTree1HaveTree2(pRoot1-> left, pRoot2->left)&&DoesTree1HaveTree2(pRoot1->right, pRoot2->right);
}
~~~

==注意：==与二叉树相关的代码有大量的指针操作，在每次使用指针的时候，都应该思考这个指针是否可能是null，如果是，那么该如何处理。==另：==有一个细节值得我们注意：本节点的值的类型是double。在判断两个结点的值是否相等时，不能直接“==”判断，这是因为计算机内表示小数时（包括float和double型小数）都有误差。所以用Equal进行判断



## 十四、画图让抽象问题形象化

### 面试题27：二叉树的镜像

> ​		题目：请完成一个函数，输入一棵二叉树，该函数输出它的镜像。

~~~java
//二叉树节点的定义如下：
struct BinaryTreeNode{
    int value;
    BinaryTreeNode *left;
    BinaryTreeNode *right;
}
~~~

**问题解决思路：**递归遍历（前序、中序、后序都行）树的同时交换非叶节点的左、右子节点

~~~java
//代码如下：
void MirrorRecursively(BinaryTreeNode *pNode){
    if(pNode == null){
        return ;
    }
    if(pNode->left == null && pNode->right == null){
        return ;
    }
    //交换左、右子节点
    BinaryTreeNode *pTemp = pNode->left;
    pNode->left = pNode->right;
    pNode->right = pTemp;
    
    if(pNode->left){
        MirrorRecursively(pNode->left);
    }
    if(pNode->right){
        MirrorRecursively(pNode->right);
    }
}
~~~



### 面试题28：对称的二叉树

> ​		题目：请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

![image-20210126221258628](D:\Files\Note\Typora\PNG\剑指Offer.assets\image-20210126221258628.png)

==注意：==三种可能性：对称的二叉树；因结构而不对称的二叉树；结构对称但节点的值不对称的二叉树

~~~java
//代码如下：
bool isSymmetrical(BinaryTreeNode *pRoot){
    return isSymmetrical(pRoot, pRoot);
}

bool isSymmetrical(BinaryTreeNode *pRoot1, BinaryTreeNode *pRoot2){
        if(pRoot1 == null && pRoot2 == null){
            return true;
        }
        if(pRoot1 == null || pRoot2 == null){
            return false;
        }
        if(pRoot1->value != pRoot2->value){
            return false;
        }
        return isSymmetrical(pRoot1->left, pRoot2->right) && isSymmetrical(pRoot1->right, pRoot2->left);
}
~~~



### 面试题29：顺时针打印矩阵

> ​		题目：输入一个矩阵，按照从外向里以顺时针的顺序一次打印出每一个数字

**问题解决思路：**

1. 打印方式？可以用一个循环来打印矩阵，每次打印矩阵中的一个圈
2. 打印继续条件？循环继续的条件是columns > startX * 2 并且 rows > startY * 2
3. 如何打印？打印一圈分四步：第一步，从左到右打印一行；第二步，从上到下打印一列；第三步，从右到左打印一行；第四步，从下到上打印一列。

==注意：==最后一圈有可能退化成只有一行、只有一列，甚至只有一个数字，因此打印这样的一圈就不再需要四步

![image-20210126221454209](D:\Files\Note\Typora\PNG\剑指Offer.assets\image-20210126221454209.png)

~~~java
//代码如下：
void PrintMatrixClockwisely(int **numbers, int columns, int rows){
    if(numbers == null || colums <= 0 || rows <= 0){
        return;
    }
    int start = 0;
    while(colums > start * 2 && rows > start * 2){
        PrintMatrixInCircle(numbers, columns, rows, start);
        ++start;
    }
}
//如何打印一圈的功能
void PrintMatrixInCircle(int **numbers, int columns, int rows, int start){
    int endX = columns - 1 - start;
    int endY = rows -1 -start;
    
    //从左到右打印一行
    for(int i = start; i<=endX; ++i){
        int number = numbers[start][i];
        printNumber(number);
    }
    
    //从上到下打印一列
    if(start < endY){
        for(int i = start + 1; i <= endY; ++i){
            int number = numbers[i][endY];
            printNumber(number);
        }
    }
    
    //从右到左打印一行
    if(start < endX && start < endY){
        for(int i = endX - 1; i >= start; --i){
            int number = numbers[endY][i];
            printNumber(number);
        }
    }
    
    //从下到上打印一列
    if(start <endX && start < endY -1){
        for(int i = endY -1; i>= start + 1; --i){
            int number = numbers[i][start];
            printNumber(number);
        }
    }
}
~~~



## 十五、举例让抽象问题具体化

### 面试题30：包含min函数的栈

> ​		题目：定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的min函数。在该栈中，调用min、push及pop的时间复杂度都是O(1)。 

**问题解决思路：**

1. 第一反应是每次压入一个新元素进栈时，将栈里的所有元素排序，让最小的元素位于栈顶，这样就能在O(1)时间内得到最小元素了【==缺点：==不能保证后进先出，此时这个数据结构就已经不是栈了】
2. 在栈里添加一个成员变量存放最下的元素，每次压入新元素时，与当前最小元素比较，若小，则更新最小元素【==漏洞：==如果当前最小的元素被弹出栈了，那么如何得到下一个最小的元素呢？】

优化：可以在压入最小元素之前，把次小元素保存起来。即把每次的最小元素（之前的最小元素和新压入栈的元素两者的较小者）都保存起来放到另外一个辅助栈里

![image-20210130113509584](D:\Files\Note\Typora\PNG\剑指Offer.assets\image-20210130113509584.png)

~~~java
//代码如下
template<typename T> void StackWithMin<T>:push(const T & value){
    m_data.push(value);
    if(m_min.size() == 0 || value < m_min.top()){
        m_min.push(value);
    }else{
        m_min.push(m_min.top());
    }
}
template<typename T> void StackWithMin<T>:pop(){
    assert(m_data.size() > 0 && m_min.size() >0);
    m_data.pop();
    m_min.pop();
}

template<typename T> const T & StackWithMin<T>::min() const{
    assert(m_data.size() > 0 && m_min.size() > 0);
    return m_min.top();
}
~~~



### 面试题31：栈的压入、弹出序列

> ​	题目：输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。假设压入栈的所有数字均不相等。例如，序列{1,2,3,4,5}是某栈的压栈序列，序列{4,5,3,2,1}是该压栈序列对应的一个弹出序列，但{4,3,5,1,2}就不可能是该压栈序列的弹出序列。

**问题解决思路：**建立一个辅助栈，把输入的第一个序列中的数字依次压入该辅助栈，并按照第二个序列的顺序依次从该栈中弹出数字。

==总结：==【判断一个序列是不是栈的弹出序列的规律】如果下一个弹出的数字刚好是栈顶数字，那么直接弹出，如果下一个弹出的数字不在栈顶，则把压栈序列中还没有入栈的数字压入辅助栈，直到把下一个需要弹出的数字压入栈顶为止，如果所有数字都压完，还是没有找到下一个弹出的数字，那么该序列不可能是一个弹出序列。

~~~java
//代码如下：
bool IsPopOrder(const int *pPush, const int * pPop, int nLength){
    bool bPossible = false;
    if(pPush != null && pPop != null && nLength > 0){
        const int *pNextPush = pPush;
        const int *pNextPop = pPop;
        //新建一个数据栈
        std:stack<int> stackData;
        //遍历第二个序列
        while(pNextPop - pPop < nLength){ 
            //数据栈没数据，或者栈顶数据不是下一个要弹出的元素，遍历第一个序列
            while(stackData.empty() || stackData.top() != *pNextPop){
                if(pNextPush - pPush == nLength){
                    break;
                }
                stackData.push(*pNextPush);
                pNextPush++;
            }
            
            if(stackData.top() != *pNextPop){
                break;
            }
            stackData.pop();
            pNextPop++;
        }
        if(stackData.empty() && pNextPop - pPop == nLength){
            bPossible = true;
        }
    }
    return bPossible;
}
~~~



面试32：从上到下打印二叉树

> ​	题目一：不分行从上到下打印二叉树【二叉树的层序遍历】
>
> ​	从上到下打印出二叉树的每个节点，同一层的结点按照从左到右的顺序打印。

~~~java
//二叉树节点的定义如下：
struct BinaryTreeNode{
    int value;
    BinaryTreeNode *left;
    BinaryTreeNode *right;
}
~~~

==问题解决思路：==每次打印一个节点的时候，如果该节点有子节点，则把该节点的子节点放到一个队列的末尾，接下来取出队列头部最早进入队列的结点，重复前面的打印操作，直到队列中所有的节点都被打印出来。

~~~java
//代码如下：
void PrintFromTopBottom(BinaryTreeNode *pTreeRoot){
    if(!pTreeRoot){
        return;
    }
    //新建一个队列
    std:deque<BinaryTreeNode *>dequeTreeNode;
    //将根节点放入队列中
    dequeTreeNode.push_back(pTreeRoot);
    while(dequeTreeNode.size()){
        BinaryTreeNode *pNode = dequeTreeNode.front();
        dequeTreeNode.pop_front(); //弹出最早进入队列节点
        printf("%d", pNode->value);
        if(pNode->left){ //判断该节点是否有左子节点，放入队列中
            dequeTreeNode.push_back(pNode->left);
        }
        if(pNode->right){ //判断该节点是否有右子节点，放入队列中
            dequeTreeNode.push_back(pNode->right);
        }
    }
}
~~~



> ​	题目二：分行从上到下打印二叉树。
>
> ​	从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

==问题解决思路：==与前一题类似，不同的是为了把二叉树的每一行单独打印到一行里，需要两个变量：一个变量表示在当前层中还没有打印的节点数；另一个变量表示下一层节点的数目。

~~~java
//代码如下：
void Print(BinaryTreeNode *pRoot){
    if(pRoot == null){
        return;
    }
    std:queue<BinaryTreeNode *>nodes;
    nodes.push(pRoot);
    int nextLevel = 0;  //下一层的节点数目
    int toBePrinted = 1;  //当层未打印的节点数
    while(!nodes.empty()){
        BinaryTreeNode *pNode = nodes.front();
        printf("%d", pNode->value);
        if(pNode->left != null){
            nodes.push(pNode->left);
            ++nextLevel;
        }
        if(pNode->right != null){
            nodes.push(pNode->right);
            ++nextLevel;
        }
        nodes.pop();
        --toBePrinted;
        //当层没有需要打印的节点，获取下一层打印的节点
        if(toBePrinted == 0){
            printf("\n");
            toBePrinted = nextLevel;
            nextLevel = 0;
        }
    }
}
~~~



> ​	题目三：之字形打印二叉树
>
> ​	请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推

==问题解决思路：==按之字形顺序打印二叉树需要两个栈。如果当前打印的是奇数层（第一层、第二层等），则先保存左子节点再保存右子节点到第一个栈里（实际上打印的时候就是从右到左打印）；如果当前打印的是偶数层（第二层、第四层等），则先保存右子节点再保存左子节点到第二个栈里。

~~~java
//代码如下：
void Print(BinaryTreeNode *pRoot){
    if(pRoot == null){
        return;
    }
    std:stack<BinaryTreeNode *> levels[2];
    int current = 0;
    int next = 1;
    levels[current].push(pRoot);
    while(!levels[0].empty() || !levels[1].empty()){
        BinaryTreeNode *pNode = levels[current].top();
        levels[current].pop();
        printf("%d", pNode->value);
        
        if(current == 0){
            if(pNode->left != null){
                levels[next].push(pNode->left);
            }
            if(pNode->right != null){
                levels[next].push(pNode->right);
            }
        }
        else{
            if(pNode->right != null){
                levels[next].push(pNode->right);
            }
            if(pNode->left != null){
                levels[next].push(pNode->left);
            }
        }
        //当前层数遍历完成，换方向
        if(levels[current].empty()){
            printf("\n");
            current = 1- current;
            next = 1- next;
        }
    }
}
~~~



### 面试题33：二叉搜索树的后序遍历序列

> ​	题目：输入一个整数数组，判断该数组是不是某**二叉搜索树**的后序遍历结果。如果是则返回true，否则返回false。假设输入的数组的任意两个数字都互不相同。【注：判断前序遍历同理】
>
> *二叉查找树（Binary Search Tree）*，（又：*二叉搜索树*，二叉排序树）它或者是一棵空树，或者是具有下列性质的*二叉树*： 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值； 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；

~~~java
//代码如下：
bool VerifySquenceOfBST(int sequence[], int length){
    if(sequence == null || length <= 0){
        return false;
    }
    int root = sequence[length -1];
    //在二叉搜索树中左子树节点的值小于根节点的值
    int i = 0;
    for(; i<length -1; ++i){
        if(sequence[i] > root){
            break;
        }
    }
    //在二叉搜索树中右子树节点的值大于根节点的值
    int j = i;
    for(; j <length -1; ++j){
        if(sequence[j] < root){
            return false;
        }
    }
    //判断左子树是不是二叉搜索树
    bool left = true;
    if(i > 0){
        left = VerifySquenceOfBST(sequence, i);
    }
    //判断右子树是不是二叉搜索树
    bool right = true;
    if(i < length -1){
        right = VerifySquenceOfBST(sequence +i, length - i - 1);
    }
    return (left && right);
}
~~~



### 面试题34：二叉树中和为某一值的路径

> ​	题目：输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶结点所经过的结点形成一条路径。

~~~java
//二叉树节点的定义如下：
struct BinaryTreeNode{
    int value;
    BinaryTreeNode *left;
    BinaryTreeNode *right;
}
~~~

==问题解决思路：==

​		因为是从根节点到叶结点的路径，所以采用前序遍历最好，每访问到某一节点时，即把该节点添加到路径上，并累加该节点的值。

​		如果该节点为叶结点，并且路径中节点值的和刚好等于输入的整数，则表示当前路径符合要求，随即打印出来。如果该节点不是叶结点，则继续访问它的子节点。

​		当前节点访问结束后，递归函数将自动回到它的父节点。因此在函数退出之前要在路径上删除当前节点并减去当前节点的值，从而确保返回父节点时路径刚好是从根节点到父节点。

​		保存路径的数据结构实际是一个栈，因为路径要与递归调用状态一致，而递归调用的本质实际上就是一个压栈和出栈的过程。

~~~java
//代码如下:
void FindPath(BinaryTreeNode *pRoot, int expectedSum){
    //代码鲁棒性的体现
    if(pRoot == null){
        return;
    }
    //用标准模板库中的vector实现了一个栈来保存路径
    //没有直接用STL中的stack的原因是，在stack中只能得到栈顶元素，而我们打印路径的时候需要得到路径上所有的节点
    std::vector<int> path; 
    int currentSum = 0;//已走过路径的节点值之和
    FindPath(pRoot, expectedSum, path, currentSum);
}
void FindPath(BinaryTreeNode *pRoot, int expectedSum, std::vector<int> &path, int currentSum){
    currentSum += pRoot->value;
    path.push_back(pRoot->value);
    //如果是叶结点，并且路径上节点值的和等于输入的值,则打印出这条路径
    bool isLeaf = pRoot->left = null && pRoot->right == null;
    if(currentSum == expectedSum && isLeaf){
        printf("A path is found：");
        std::vector<int>::iterator iter = path.begin();
        for(;iter != path.end(); ++iter){
            printf("%d\t", *iter);
        }
        printf("\n");
    }
    //如果不是叶结点，则遍历它的子节点
    if(pRoot->left != null){
        FindPath(pRoot->left, expectedSum, path, currentSum);
    }
    if(pRoot->right != null){
        FindPath(pRoot->right, expectedSum, path, currentSum);
    }
    //在返回父节点之前，在路径上删除当前节点
    path.pop_back();
}
~~~



## 十六、分解让复杂问题简单化

### 面试题35：复杂链表的复制

> ​	题目：请实现函数ComplexListNode* Clone(ComplexListNode *pHead)，复制一个复杂链表。在复杂链表中，每个节点除了有一个next指针指向下一个节点，还有一个Sibling指针指向链表中的任意节点或者null

~~~C++
//节点的C++定义如下：
struct ComplexListNode{
    int value;
    ComplexListNode *next;
    ComplexListNode *Sibling;
}
~~~

==问题解决思路：==

**方法一：**