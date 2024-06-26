---
{"dg-publish":true,"permalink":"/02-pages/858-Prim算法求最小生成树/","tags":["personal/blog","algorithm/模板题","algorithm/data-structures/树","algorithm/graph/生成树"]}
---


# 题目描述
## 关系
[[02-pages/859-Kruskal算法求最小生成树\|859-Kruskal算法求最小生成树]]
## 内容
给定一个 $n$ 个点 $m$ 条边的无向图，图中可能存在重边和自环，边权可能为负数。

求最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

给定一张边带权的无向图 $G=(V, E)$，其中 $V$ 表示图中点的集合，$E$ 表示图中边的集合，$n=|V|$，$m=|E|$。

由 $V$ 中的全部 $n$ 个顶点和 $E$ 中 $n-1$ 条边构成的无向连通子图被称为 $G$ 的一棵生成树，其中边的权值之和最小的生成树被称为无向图 $G$ 的最小生成树。

#### 输入格式

第一行包含两个整数 $n$ 和 $m$。

接下来 $m$ 行，每行包含三个整数 $u,v,w$，表示点 $u$ 和点 $v$ 之间存在一条权值为 $w$ 的边。

#### 输出格式

共一行，若存在最小生成树，则输出一个整数，表示最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

#### 数据范围

$1 \le n \le 500$,  
$1 \le m \le 10^5$,  
图中涉及边的边权的绝对值均不超过 $10000$。

#### 输入样例：

```
4 5
1 2 1
1 3 2
1 4 3
2 3 2
3 4 4
```

#### 输出样例：

```
6
```
# 问题分析
## 思路分析
设节点数量为 n，边数为 m。

有两种 prim 算法，一种是朴素的实现，时间复杂度为 $\displaystyle O(n^2)$。

一种是堆优化的实现，时间复杂度为 $\displaystyle O(m\log n)$。可见若边数为 $\displaystyle m=n^2$，那么实际上是优化了一个寂寞。

所以朴素实现适合稠密图，而优化版本适合稀疏图。

***

Prim 算法的贪心思路是选择尽可能小的边，用点去扩充边。

朴素的 Prim 算法用邻接矩阵存储图。

## 执行流程设计

# 总结

# 代码实现
```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 510, M = 1e5 + 10, INF = 0x3f3f3f3f;
int n, m;
int g[N][N], dist[N];
bool st[N];

int prim()
{
    memset(dist, 0x3f, sizeof dist);
    
    dist[1] = 0;
    int res = 0;
    for (int i = 0; i < n; i++) {
        int t = -1;
        for (int j = 1; j <= n; j++) {
            if (!st[j] && (t == -1 || dist[t] > dist[j])) t = j;
        }
        // 如果最小边权对应的节点都不可达，说明没有生成树
        if (dist[t] == INF) return INF;
        // 加入生成树集合
        st[t] = true;
        
        res += dist[t];
        // 将所有生成树外的节点更新距离
        for (int j = 1; j <= n; j++) {
            dist[j] = min(dist[j], g[t][j]);
        }
    }
    return res;
}

int main()
{
    cin >> n >> m;
    memset(g, 0x3f, sizeof g);
    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        g[a][b] = g[b][a] = min(g[b][a], c);
    }
    
    int res = prim();
    if (res == INF) puts("impossible");
    else printf("%d\n", res);
    return 0;
}
```
```ad-note
title: 解读
### 为什么要更新dist距离？
因为 dist 数组记录的是节点到生成树的距离。当有一个新的节点纳入生成树集合的时候，节点到生成树的距离就会改变，所以需要重新更新一下。

### 为什么距离更新为 `min(dist[j], g[t][j])`
当是 t 节点距离更新的时候，显然 g[t][t] = 0。
当不是 t 节点更新且 t 与 j 相邻的时候，g[t][j] 代表的就是 j 节点距离 t 节点的距离。这就是 prim 算法的贪心之处了。我们可以证明 j 节点距离生成树集合最近的距离就是 j 与 t 距离。
当 j 节点不与 t 相邻。那么最后更新的结果仍然是 INF。
```

