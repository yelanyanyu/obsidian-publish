---
{"dg-publish":true,"permalink":"/02-pages/854-Floyd求最短路/","tags":["personal/blog","algorithm/graph/多源汇最短路","algorithm/graph/Floyd","algorithm/模板题","algorithm/dp"]}
---


# 题目描述
## 关系

## 内容
给定一个 $n$ 个点 $m$ 条边的有向图，图中可能存在重边和自环，边权可能为负数。

再给定 $k$ 个询问，每个询问包含两个整数 $x$ 和 $y$，表示查询从点 $x$ 到点 $y$ 的最短距离，如果路径不存在，则输出 `impossible`。

数据保证图中不存在负权回路。

#### 输入格式

第一行包含三个整数 $n,m,k$。

接下来 $m$ 行，每行包含三个整数 $x,y,z$，表示存在一条从点 $x$ 到点 $y$ 的有向边，边长为 $z$。

接下来 $k$ 行，每行包含两个整数 $x,y$，表示询问点 $x$ 到点 $y$ 的最短距离。

#### 输出格式

共 $k$ 行，每行输出一个整数，表示询问的结果，若询问两点间不存在路径，则输出 `impossible`。

#### 数据范围

$1 \le n \le 200$,  
$1 \le k \le n^2$  
$1 \le m \le 20000$,  
图中涉及边长绝对值均不超过 $10000$。

#### 输入样例：

```
3 3 2
1 2 1
2 3 2
1 3 1
2 1
1 3
```

#### 输出样例：

```
impossible
1
```
# 问题分析
## 最初思路

## 思路分析
求出任何两点之间的距离。$\displaystyle d[k][i][j]$ 表示经过节点 1 到 k，点 i 到 j 的最短距离。

若 i 到 j 的最短路径经过 k 点，那么状态转移方程可以写为：$\displaystyle f[k][i][j] = f[k-1][i][k]+f[k-1][k][j]$.

如果 i 到 j 的最短路径不经过 k，那么状态转移方程为：$\displaystyle f[k][i][j]=f[k-1][i][j]$。

初始化也很显然：
 1. 当 k 为 0，即什么节点都不经过，那么最短距离当然是无穷大的；
 2. 当然，节点到自身的最短距离为 0.


## 执行流程设计

# 总结

# 代码实现
```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 210, M = 20010, INF = 0x3f3f3f3f;
int n, m, Q;
int f[N][N];

int main()
{
    cin >> n >> m >> Q;
    memset(f, 0x3f, sizeof f);
    for (int i = 1; i <= n; i++) {
        f[i][i] = 0;
    }
    
    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        f[a][b] = min(f[a][b], c);
    }
    
    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                f[i][j] = min(f[i][j], f[i][k] + f[k][j]);
            }
        }
    }
    
    while (Q--) {
        int a, b;
        cin >> a >> b;
        int res = f[a][b];
        if (res > INF / 2) puts("impossible");
        else cout << res << endl;
    }
    return 0;
}
```