## 一、Greedy Algorithm

### 1.1 Dijkstra算法

[743. 网络延迟时间 - 力扣（LeetCode）](https://leetcode.cn/problems/network-delay-time/description/)

有 `n` 个网络节点，标记为 `1` 到 `n`。

给你一个列表 `times`，表示信号经过 **有向** 边的传递时间。 `times[i] = (ui, vi, wi)`，其中 `ui` 是源节点，`vi` 是目标节点， `wi` 是一个信号从源节点传递到目标节点的时间。

现在，从某个节点 `K` 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 `-1` 

#### 1 

```cpp
算法描述：适合于稠密图
定义g[i]表示节点i到节点j这条边的边权，如果没有这条边，则g[i][j]=∞
定义dis[i]表示起点k待节点i的最短路径长度，一开始dis[k]=0，其余dis[i]=∞表示尚未算到
我们的目的是计算出最终的dis数组：
- 首先更新起点k到其邻居y的最短路，及更新dis[y]为dis[y]为g[k][y]
- 然后区除了起点k以外的dis[i]的最小值，假设最小值对应的节点是3。此时可以断言：dis[3]已经是k到3最短路径长度，不可能有其他k到3的路径更短
- 用节点3到其邻居y的边权g[3][y]更新dis[y]:如果dis[3] + dis[3][y] < dis[y]，那么更新dis[y]为dis[3] + g[3][y]，否则不更新
- 然后去除了节点k，3以外的dis[i]的最小值，重复上述过程
```

```java
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<int>> g(n, vector<int>(n, INT_MAX / 2));
        for (auto& time : times) {
            g[time[0] - 1][time[1] - 1] = time[2];
        }
        vector<int> dis(n, INT_MAX / 2), visit(n, 0);
        dis[k - 1] = 0;
        int ans = 0;

        // 准备工作完毕
        int count = n;
        while (count--) { // 循环n，将所有节点找出
            int x = -1;
            for (int i = 0; i < n; i++) { // 找出最短边所在的点
                if (!visit[i] &&
                    (x == -1 || dis[x] > dis[i])) { // 第一次循环是zhaodaok点
                    x = i;
                }
            }
            ans = max(dis[x], ans);
            visit[x] = 1;
            for (int y = 0; y < n; y++) {
                if (visit[y] == 0 && dis[x] + g[x][y] < dis[y]) {
                    dis[y] = dis[x] + g[x][y];
                }
            }
        }
        return ans == INT_MAX / 2 ? -1 : ans;
    }
};
```

#### 2 

```
寻找最小值的过程可以用一个最小堆来快速完成：
* 一开始把(dis[k],[k])二元组入堆
* 当节点x首次出堆时，dis[x]就是写法一中寻找的最小最短路
* 更新dis[y]时，把(dis[y],y)二元组入堆
注意，如果一个节点x在出对前，其最短路径长度dis[x]被多次更新，那么堆中会有多个重复的x，并且包含x的二元组中的dis[x]是互不相同的（因为我们只在找更小的最短路时才会把二元组入堆）
所以用出堆的最短路径值（记作dx）与当前的dis[x]比较，如果dx>dis[x]说明x之前出堆过，我们已经更新了x邻居的最短路，所以这次就不用更新了，继续外层循环
```

```cpp
class Solution {
public:
//结点和边的关系，用e 和 nlogn 判断，e是边，n是结点，e大是稠密，nlogn大是稀疏
// 时间复杂度：O(mlogm)，其中 m 为 times 的长度。由于 m >= n−1，分析复杂度时以 m 为主。注意堆中会有重复节点，所以至多有 O(m) 个元素，单次操作的复杂度是 O(logm)。值得注意的是，如果输入的是稠密图，写法二的时间复杂度为 O(n^2logn)，不如写法一。
// mlogm 和 n^2 谁大谁小 m是边，n是点

    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<pair<int, int>>> g(n); // 邻接表
        for (auto& t : times) {
            g[t[0] - 1].emplace_back(t[1] - 1, t[2]);
        }

        vector<int> dis(n, INT_MAX);
        dis[k - 1] = 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        pq.emplace(0, k - 1);
        while (!pq.empty()) {
            auto [dx, x] = pq.top();// dx表示当时堆中的最短距离，x表示该店
            pq.pop();
            if(dx > dis[x]) { //  x之前出堆过
                continue;// 之前已经更新过距离，不需要在更新，continue
            }
            // 更新其邻居的距离
            for(auto &[y, d] : g[x]) {// y表示邻接点，d表示离该点的距离
                int newDis = dx + d;
                if (newDis < dis[y]) {
                    dis[y] = newDis;// 更新距离
                    pq.emplace(newDis, y);//入堆（只要更新距离就会入堆）
                    // 所以只要取其中的最小值即可
                }
            }
        }
        int mx = *max_element(dis.begin(),dis.end());
        return mx < INT_MAX ? mx : -1;
    }
};
```

## 二、Dynamic Programming

### 2.1 整数拆分

给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。

返回 *你可以获得的最大乘积* 。

#### 1

对于正整数 n，当 n≥2 时，可以拆分成至少两个正整数的和。令 x 是拆分出的第一个正整数，则剩下的部分是 n−x，n−x 可以不继续拆分，或者继续拆分成至少两个正整数的和。由于每个正整数对应的最大乘积取决于比它小的正整数对应的最大乘积，因此可以使用动态规划求解。

创建数组 dp，其中 dp[i] 表示将正整数 i 拆分成至少两个正整数的和之后，这些正整数的最大乘积。特别地，0 不是正整数，1 是最小的正整数，0 和 1 都不能拆分，因此 dp[0]=dp[1]=0。

当 i≥2 时，假设对正整数 i 拆分出的第一个正整数是 j（1≤j<i），则有以下两种方案：

将 i 拆分成 j 和 i−j 的和，且 i−j 不再拆分成多个正整数，此时的乘积是 j×(i−j)；

将 i 拆分成 j 和 i−j 的和，且 i−j 继续拆分成多个正整数，此时的乘积是 j×dp[i−j]。

因此，当 j 固定时，有 dp[i]=max(j×(i−j),j×dp[i−j])。由于 j 的取值范围是 1 到 i−1，需要遍历所有的 j 得到 dp[i] 的最大值，因此可以得到状态转移方程如下：

dp[i]= max {max(j×(i−j),j×dp[i−j])}
	   1≤j<i

最终得到 dp[n] 的值即为将正整数 n 拆分成至少两个正整数的和之后，这些正整数的最大乘积

```cpp
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1, 0);
        for (int i = 2; i <= n; i++) {
            int curMax = 0;
            for (int j = 1; j < i; j++) {
                curMax = max(curMax, max(j * (i - j), j * dp[i - j]));
            }
            dp[i] = curMax;
        }
        return dp[n];
    }
};
```

### 2.2 最长公共子序列

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<int> dp(text2.size() + 1);

        for (int i = 0; i < text1.size(); i++) {
            for (int j = 0, pre = 0; j < text2.size(); j++) {
                int tmp = dp[j + 1];
                if (text1[i] == text2[j]) {
                    dp[j + 1] = pre + 1;
                } else {
                    dp[j + 1] = max(dp[j + 1], dp[j]);
                }
                pre = tmp;
            }
        }
        return dp[dp.size() - 1];
    }
};
```

```java
class Solution {
    public static int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        int[][] dp= new int[m + 1][n + 1];
        for(int i = 0; i < m;i++) {
            for(int j = 0; j < n;j++) {
                if(text1.charAt(i) == text2.charAt(j)) {
                    dp[i+1][j + 1] = dp[i][j] + 1;
                }else{
                    dp[i+1][j + 1] = Math.max(dp[i + 1][j],dp[i][j + 1]);
                }
            }
        }
        return dp[m][n];
    }
}
```



## 三、Divide and Conquer

