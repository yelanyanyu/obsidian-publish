---
{"dg-publish":true,"permalink":"/02-pages/849-Dijkstra求最短路 I/","tags":["personal/blog","algorithm/graph/单源最短路","algorithm/graph/Dijkstra","algorithm/模板题"]}
---


# 题目描述
## 关系

## 内容
给定一个 $n$ 个点 $m$ 条边的有向图，图中可能存在重边和自环，所有边权均为正值。

请你求出 $1$ 号点到 $n$ 号点的最短距离，如果无法从 $1$ 号点走到 $n$ 号点，则输出 $-1$。

#### 输入格式

第一行包含整数 $n$ 和 $m$。

接下来 $m$ 行每行包含三个整数 $x,y,z$，表示存在一条从点 $x$ 到点 $y$ 的有向边，边长为 $z$。

#### 输出格式

输出一个整数，表示 $1$ 号点到 $n$ 号点的最短距离。

如果路径不存在，则输出 $-1$。

#### 数据范围

$1 \le n \le 500$,  
$1 \le m \le 10^5$,  
图中涉及边长均不超过 10000。

#### 输入样例：

```
3 3
1 2 2
2 3 1
1 3 4
```

#### 输出样例：

```
3
```
# 问题分析
## 思路分析
我们定义节点 1 为开始点，节点 n 为结束点，我们要求的是 1 到 n 的最短路。

具体的流程可以参考：[AcWing 849. Dijkstra求最短路 I：图解 详细代码（图解） - AcWing](https://www.acwing.com/solution/content/38318/)
## 执行流程设计

# 总结

# 代码实现
```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 510, M = 1e5 + 10, INF = 0x3f3f3f3f;
int n, m;
int g[N][N], dist[N]; // dist[i] 1 到 i 节点的最短距离, g: 邻接矩阵
bool st[N]; // st[i] = true: 节点 i 已经求出

int dijkstra() {
    memset(dist, 0x3f, sizeof dist);
    
    dist[1] = 0;
    for (int i = 0; i < n; i++) {
        int t = -1;
        for (int j = 1; j <= n; j++) {
            if (!st[j] && (t == -1 || dist[t] > dist[j])) 
                t = j;
        }
        st[t] = true;
        for (int j = 1; j <= n; j++) {
            // 更新所有与 t 节点连通的节点
            dist[j] = min(dist[j], dist[t] + g[t][j]);
        }
    }
    
    return dist[n];
}

int main()
{
    cin >> n >> m;
    memset(g, 0x3f, sizeof g);
    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        g[a][b] = min(g[a][b], c);
    }
    
    int res = dijkstra();
    
    if (res == INF) puts("-1");
    else cout << res;
    return 0;
}
```