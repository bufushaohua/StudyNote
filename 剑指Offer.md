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

![image-20201222221305931](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20201222221305931.png)

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

![img](file:///D:\Files\Tencent Files\3557251540\Image\C2C\985F4C12B2C5CA5EDCF0ACC0C34524DD.jpg)

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

**方法一：可能引起死循环的解法**

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

**方法二：常规解法**

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