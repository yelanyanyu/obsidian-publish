---
{"dg-publish":true,"permalink":"/01/01/01/acwing/07/006-859-kruskal/","tags":["personal/blog","algorithm/模板题","algorithm/sorting","algorithm/数据结构/树","algorithm/图论/生成树"]}
---

[859. Kruskal算法求最小生成树 - AcWing题库](https://www.acwing.com/problem/content/description/861/)
# 题目描述
## 关系
[[01-计算机笔记/01-竞赛/01-刷题/acwing/07-考研算法辅导课/005-858-Prim算法求最小生成树\|005-858-Prim算法求最小生成树]]
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

$1 \le n \le 10^5$,  
$1 \le m \le 2*10^5$,  
图中涉及边的边权的绝对值均不超过 $1000$。

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
Kruskal 算法的贪心思路是选择边权最小的边来实现的。时间复杂度为 $\displaystyle O(m\log m)$。

借用一个并查集，按照边权从小到大枚举所有边，如果两个边不在同一个集合之内，就加入。
## 执行流程设计 ef

# 总结

# 代码实现
```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;
int n, m;
int p[N];

struct Edge {
    int a, b, c;
    bool operator< (const Edge &e) const {
        return c < e.c;
    }
} e[N];

int find(int x) {
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++) p[i] = i;
    
    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        e[i] = {a, b, c}; 
    }
    
    sort(e, e + m);
    
    int cnt = n, res = 0; // cnt 表示还未被加入生成树的节点数
    for (int i = 0; i < m; i++) {
        int a = e[i].a, b = e[i].b, c = e[i].c;
        if (find(a) != find(b)) {
            p[find(b)] = find(a);
            res += c;
            cnt--;
        }
    }   
    
    if (cnt > 1) puts("impossible"); // cnt > 1 意味有多个生成树，这是不可能的
    else printf("%d\n", res);
    return 0;
}
```