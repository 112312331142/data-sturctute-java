











####  











##  











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

- 兔子问题
  - 第一个月，有一对未成熟的兔子（黑色，注意图中个头较小）
  - 第二个月，它们成熟
  - 第三个月，它们能产下一对新的小兔子（蓝色）
  - 所有兔子遵循相同规律，求第 n 个月的兔子数
- 爬楼梯问题
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

### 10.3 卡特兰数

#### 1 不同的二叉搜索树

给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数

```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1,0);
        dp[0] = 1;
        for(int i = 1;i <= n;i++) {
            for (int j = 0; j < i;j++) {
                dp[i] += dp[j] * dp[i - 1 - j];
            }
        }
        return dp[n];
    }
};
```

#### 2 出栈序列

n个元素进栈序列序列为：1，2，3，4，5，6...n，则有多少个出栈序列





- ​