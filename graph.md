## 八、图

### 8.1 连接所有点的最小费用

#### 1 krustal，并查集

设置Edge存储每一条边，

记录Djset存储每个节点

```java
class Djset {
public:
    vector<int> parent; // 记录节点的根
    vector<int> rank;   // 记录根节点的深度（用于优化）
    vector<int> size;   // 记录每个连通分量的节点个数
    vector<int> len;    // 记录每个联通分量里的所有边长度
    int num;            // 记录节点个数

    // 初始化并查集，节点的个数为n，parent[i] = i
    Djset(int n) : parent(n), rank(n), len(n, 0), size(n, 1), num(n) {
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }

    int find(int x) {
        if (parent[x] == x) {
            return x;
        }
        return parent[x] = find(parent[x]);
    }

    // vector<int> parent; // 记录节点的根，当前节点的大哥
    // vector<int> rank;   // 记录根节点的深度（用于优化），即大哥是n的个数
    // vector<int> size;   // 记录每个连通分量的节点个数，已加入并查集的个数
    // vector<int> len;    // 记录每个联通分量里的所有边长度，最终返回的长度
    // int num;            // 记录节点个数，总个数
    int merge(int x, int y, int lengh) {
        // 分别找到并查集两个端点中的老大
        int rootx = find(x);
        int rooty = find(y);
        if (rootx != rooty) {
            // 保证rootx的深度小于rooty，则将rootx与rooty互换
            if (rank[rootx] < rank[rooty]) {
                swap(rootx, rooty);
            }
            parent[rooty] = rootx;
            if (rank[rootx] == rank[rooty]) {
                rank[rootx] += 1;
            }
            // rooty节点的根是rootx，同时将rooty的节点树和边长度累加到rootx
            size[rootx] += size[rooty];
            len[rootx] += len[rooty] + lengh;
            // 如果某个连通分量的节点数，包含了所有节点，直接返回边长度
            if (size[rootx] == num) {
                return len[rootx];
            }
        }
        return -1;
    }
};

struct Edge {
    int start;
    int end;
    int len;
};

class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        int res = 0;
        int n = points.size();
        Djset ds(n);
        vector<Edge> edges;

        // 建立点边数据结构
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                Edge edge = {i, j,
                             abs(points[i][0] - points[j][0]) +
                                 abs(points[i][1] - points[j][1])};
                edges.emplace_back(edge);
            }
        }

        // 按边长度排序
        sort(edges.begin(), edges.end(),
             [](const auto& a, const auto& b) { return a.len < b.len; });

        // 连通分量合并
        for (auto& e : edges) {
            // 将边按距离从小到大一次放入并查集中
            res = ds.merge(e.start, e.end, e.len);
            if (res != -1) {
                return res;
            }
        }
        return 0;
    }
};
```

