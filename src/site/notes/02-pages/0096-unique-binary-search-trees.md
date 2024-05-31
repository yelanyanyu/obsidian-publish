---
{"dg-publish":true,"permalink":"/02-pages/0096-unique-binary-search-trees/","tags":["personal/blog","algorithm/data-structures/有序表","algorithm/math/Catalan-number"]}
---


# 题目描述
## 关系
[96. 不同的二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-binary-search-trees/description/)
[1645. 不同的二叉搜索树 - AcWing题库](https://www.acwing.com/problem/content/description/1647/)
## 内容
给定一个整数 $n$，求以 $1 … n$ 为节点组成的二叉搜索树有多少种？

结果对 $10^9 + 7$ 取模后输出。

#### 输入格式

共一行，包含一个整数 $n$。

#### 输出格式

输出一个整数，表示对 $10^9 + 7$ 取模后的结果。

#### 数据范围

$1 \le n \le 1000$

#### 输入样例：

```
3
```

#### 输出样例：

```
5
```

#### 样例解释

当 $n = 3$ 时, 一共有 $5$ 种不同结构的二叉搜索树:

```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
# 问题分析
## 思路分析
与 [[02-pages/0415-栈\|0415-栈]]相同，我们以哪个节点为根进行分类讨论。如果 k 为根，那么 k 左边 k - 1 个数单独构成一颗二叉搜索树，k 右边 n - k 个数单独构成一颗搜索二叉树。

所以连代码都是一样的。

# 代码实现
```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

typedef long long LL;

const int N = 1010, MOD = 1e9 + 7;
int n;
LL f[N];

int main()
{
    cin >> n;
    f[0] = f[1] = 1l;
    
    for (int i = 2; i <= n; i++)
        for (int k = 1; k <= i; k++) 
            f[i] = (f[i] + f[k - 1] * f[i - k]) % MOD;
    
    cout << f[n] % MOD;
    return 0;
}
```