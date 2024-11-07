## 一、Greedy Algorithm

### 1.1 Dijkstra算法

[743. 网络延迟时间 - 力扣（LeetCode）](https://leetcode.cn/problems/network-delay-time/description/)

有 `n` 个网络节点，标记为 `1` 到 `n`。

给你一个列表 `times`，表示信号经过 **有向** 边的传递时间。 `times[i] = (ui, vi, wi)`，其中 `ui` 是源节点，`vi` 是目标节点， `wi` 是一个信号从源节点传递到目标节点的时间。

现在，从某个节点 `K` 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 `-1` 

#### 1

```cpp
算法描述：
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