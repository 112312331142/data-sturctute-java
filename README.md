## 一、排序

### 1.1 冒泡排序

#### 1

```java
public static void sort(int[] arr) {
    recursion(arr, arr.length);
}

public static void recursion(int[] arr, int len) {
    if (len == 1) {
        return;
    }
    int x = 0;
    for (int i = 0; i < len - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            swap(arr, i, i + 1);
            x = i;
        }
    }
    recursion(arr, x+1);
}
```

### 1.2 选择排序



### 1.3 插入排序

#### 1

```java
 public static void sort(int[] a) {
        insertion(a, 1);
    }

 private static void insertion(int[] a, int low) {
     if (low == a.length) {
         return;
     }
     int temp = a[low];
     int i = low;
     for (i = low; i > 0 && a[i - 1] > temp; i--) {
         a[i] = a[i - 1];
     }
     a[i] = temp;

     insertion(a, ++low);
 }
```



## 二、查找

### 2.1 二分查找

#### 1

二分查找普通版：

```java
public static int binarySearchNormal(int[] arr, int target) {
    int l = 0;
    int r = arr.length - 1;
    while (l <= r) {
        int mid = l + ((r - l) >> 1);
        if (target < arr[mid]) {
            r = mid - 1;
        } else if (target > arr[mid]) {
            l = mid + 1;
        } else {
            return mid;
        }
    }
    return -(l + 1);
}
```

#### 2

在数组中有重复元素，求数组的最左（右）边目标i元素时，要对代码的if条件进行更改

```java
public static int binarySearchLeftMost2(int[] arr, int target) {
    int l = 0;
    int r = arr.length - 1;
    int index = -1;
    while (l <= r) {
        int mid = l + ((r - l) >> 1);
        if (target < arr[mid]) {
            r = mid - 1;
        } else if (target > arr[mid]) {
            l = mid + 1;
        } else {
            index = mid;
            r = mid - 1;
        }
    }
    return index;
}
```

在找到指定元素时，将不能直接返回索引，需要先添加补充一个变量存储该元素的索引，将有边界减小为mid-1（与目标元素的在中点的左边所处的情况一致），只是多了一步存储操作

#### 3

在边界的值无法值无法直接不参与查找时，循环的边界条件要进一步更改

```java
public static int binarySearch(int[] a, int target) {
    int i = 0, j = a.length;
    while (i < j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {			// 在左边
            j = m;
        } else if (a[m] < target) {		// 在右边
            i = m + 1;
        } else {
            return m;
        }
    }
    return -1;
}
```

上述代码的有边界j所处的值无法取到，所以在二分时j不能在中点的基础上减一；



## 三、链表

### 3.1 反转链表

#### 1

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode newList=null;
        for(ListNode node=head;node!=null;node=node.next){
            newList=new ListNode(node.val,newList);
        }
        return newList;
    }
}
```

创建一个新链表，依次将创建新节点，将遍历到节点的值赋予它，并将它的下一个节点指针指向头节点，最后将该节点变为新的头节点，进行头插法插进newList中

#### 2

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
// 方法2
class Solution {
    public ListNode reverseList(ListNode head) {
        List list1 = new List(head);
        List list2 = new List(null);
        while (true) {
            ListNode first = list1.removeFirst();
            if (first == null) {
                break;
            }
            list2.addFirst(first);
        }
        return list2.head;

    }

    static class List {
        ListNode head;

        public List(ListNode head) {
            this.head = head;
        }

        public void addFirst(ListNode node) {
            node.next = head;
            head = node;
        }

        public ListNode removeFirst() {
            ListNode first = head;
            if (first != null) {
                head = first.next;
            }
            return first;
        }
    }
}
```

与方法1 类似，构造一个新链表，从**旧链表头部**移除节点，添加到**新链表头部**，完成后新链表即是倒序的，区别在于原题目未提供节点外层的容器类，这里提供一个，另外一个区别是并不去构造新节点

list1包装旧链表，list2包装新链表

#### 3

递归解决

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head.next==null){
            return head;//找到最后节点返回
        }
        ListNode last=reverseList(head.next);
        return last;
    }
}
```

上述代码是找到该链表的最后一个节点并返回，作为新链表的头节点

下面是伪代码的调用流程：

```java
reverseList(ListNode p = 1) {
    reverseList(ListNode p = 2) {
    	reverseList(ListNode p = 3) {
    		reverseList(ListNode p = 4) {
    			reverseList(ListNode p = 5) {
    				if (p == null || p.next == null) {
                        return p; // 返回5
                    }
				}
                // 此时p是4, p.next是5
			}
            // 此时p是3, p.next是4
		}
        // 此时p是2, p.next是3
	}
    // 此时p是1, p.next是2
}
```

根据上述伪代码的流程，在出递归的时候要让当前指针的指向的指针指向自己

```JAVA
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null||head.next == null) {
            return head;// 找到最后节点返回
        }
        ListNode last = reverseList(head.next);
        head.next.next = head;
        head.next=null;
        return last;
    }
}
```

head.next.next=head;

当前指针必须指向null，这是防止在最后一次递归时，两个指针互相指向，而不能向前几次递归那样，后面的递归可以帮前一个指针更改指向

#### 4

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode o1 = head;
        ListNode o2 = head.next;
        while (o2 != null) {
            o1.next = o2.next;//将o1指向o2的下一个节点，使o2从链表中断开，
            o2.next = head;//将o2插在头节点的前方
            head = o2;//o2所处的值变为新的头节点
            o2 = o1.next;//在每次循环结束后，o1的下一个节点一定是o2；
        }
        return head;
    }
}
```

#### 5

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev=null;
        ListNode curr=head;
        while(curr!=null){
            ListNode temp=curr.next;
            curr.next=prev;
            prev=curr;
            curr=temp;
        }

        return prev;
    }
}
```

与方法4不同的是，该方法是直接更改节点的指向，最后返回的是原链表的尾节点

## 四、数组



## 五、队列



## 六、栈



## 七、树



## 八、图



## 九、字符串



## 十、经典算法问题

### 10.1 斐波那契数列

#### 1 递归

递归解决，太简单，就不写了

#### 2 动态规划

```java
public int Fibonacci(int n) {
        int prev = 0;
        int cur = 1;

        int temp = 0;
        for (int i = 0; i < n; i++) {
            temp=cur;
            cur=cur+prev;
            prev=temp;           
        }
        return cur;
    }
```

用prev代表当前的前一个值，初始值为0，用cur代表要返回的当前值，初始值为1

由于斐波那契数列的关系为f(n)=f(n-1)+f(n-2)，在该代码中，前一个循环的cur为f(n-1)，前一个循环的prev为f(n-2)

`cur=cur+prev;`

在相加之前，用一个临时变量temp存储cur，防止cur改变，无法给prev赋予正确的值，让其正确的参与到下一次循环中，与两数交换所用的思想一致

```java
 public static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
```

#### 3 数学公式

```java
public static void main(String[] args) {
    int n=6;
    double p=Math.pow(5,0.5);
    System.out.println((1 / p) * (Math.pow((1 + p) / 2, n+1) - Math.pow((1 - p) / 2, n+1)));
}
```

#### 3 变体问题

* 兔子问题
  - 第一个月，有一对未成熟的兔子（黑色，注意图中个头较小）
  - 第二个月，它们成熟
  - 第三个月，它们能产下一对新的小兔子（蓝色）
  - 所有兔子遵循相同规律，求第 n 个月的兔子数
* 爬楼梯问题
  - 楼梯有 n 阶
  - 青蛙要爬到楼顶，可以一次跳一阶，也可以一次跳两阶
  - 只能向上跳，问有多少种跳法

#### 4 记忆递归

```java
 public static int memorization(int n) {//假设n=5
     int[] cache = new int[n + 1];
     Arrays.fill(cache, -1); //cache=[-1,-1,-1,-1,-1,-1]
     cache[0] = 0;
     cache[1] = 1;
     return f(n, cache);
 }

 public static int f(int n, int[] cache) {
     if (cache[n] != -1) {
         return cache[n];//在数组中已经存储了，不需要在做重复的运算，直接返回
     }
     int x = f(n - 1, cache);
     int y = f(n - 2, cache);
     cache[n] = x + y;
     return cache[n];
 }
```

时间复杂度：O(n)，与传统递规相比增加了空间复杂度，空间换时间

### 10.2 汉诺塔

```java
/**
 * @param n 是圆盘的歌数
 */
private static void recursion(int n, LinkedList<Integer> a,
                              LinkedList<Integer> b, LinkedList<Integer> c) {
    count++;
    if (n == 0) {
        return;
    }
    recursion(n - 1, a, c, b);
    c.addLast(a.removeLast());//中间步骤
    recursion(n - 1, b, a, c);
}
```

## 十一、递归

在函数最后一步是调用一个函数，那么称之为尾调用

### 11.1 时间复杂度计算

若有递归式 T(n) = aT(n/b) + f(n)其中

- T(n) 是问题的运行时间，n是数据规模
- a 是子问题个数
- T(nb) 是子问题运行时间，每个子问题被拆成原问题数据规模的 nb
- f(n) 是除递归外执行的计算

**例1**

T(n)=2T(n2)+n4

- 此时 x=1<4，由后者决定整个时间复杂度 Θ(n4)
- 如果觉得对数不好算，可以换为求【$b$ 的几次方能等于 a】

**例2**

T(n)=T(7n/10)+n

- a=1,b=10/7,x=0,c=1
- 此时 x=0<1，由后者决定整个时间复杂度 Θ(n)

**例3**

T(n)=16T(n/4)+n2

- a=16,b=4,x=2,c=2
- 此时 x=2=c，时间复杂度 Θ(n2log⁡n)