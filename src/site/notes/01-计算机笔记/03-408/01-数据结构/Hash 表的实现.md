---
{"dg-publish":true,"permalink":"/01/03-408/01/hash/","tags":["personal/blog","algorithm/hash","algorithm/模板题"]}
---


# 题目描述
## 关系

## 内容
维护一个集合，支持如下几种操作：

1.  `I x`，插入一个整数 $x$；
2.  `Q x`，询问整数 $x$ 是否在集合中出现过；

现在要进行 $N$ 次操作，对于每个询问操作输出对应的结果。

#### 输入格式

第一行包含整数 $N$，表示操作数量。

接下来 $N$ 行，每行包含一个操作指令，操作指令为 `I x`，`Q x` 中的一种。

#### 输出格式

对于每个询问指令 `Q x`，输出一个询问结果，如果 $x$ 在集合中出现过，则输出 `Yes`，否则输出 `No`。

每个结果占一行。

#### 数据范围

$1 \le N \le 10^5$  
$-10^9 \le x \le 10^9$

#### 输入样例：

```
5
I 1
I 2
I 3
Q 2
Q 5
```

#### 输出样例：

```
Yes
No
```

# 总结

# 代码实现
## 拉链法
```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 2e5 + 10;
int n;
int h[N], e[N], ne[N], idx;

int _hash(int x) {
    return (x % N + N) % N;
}

int find(int x) {
    int t = _hash(x);
    for (int i = h[t]; ~i; i = ne[i]) {
        if (e[i] == x) 
            return true;
    }
    return false;
}

void add(int a, int b) {
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx++;
}

void insert(int x) {
    if (find(x)) return;
    int t = _hash(x);
    add(t, x);
}

int main()
{
    memset(h, -1, sizeof h);
    cin >> n;
    while (n -- ) {
        char op; 
        int x;
        cin >> op >> x;
        if (op == 'I') insert(x);
        else {
            if (find(x)) puts("Yes");
            else puts("No");
        }
    }
    
    return 0;
}
```

## 开放寻址法
```c++
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 2e5 + 10, null = 0x3f3f3f3f;
int n;
int h[N];

int find(int x) {
    int t = (x % N + N) % N;
    while (h[t] != null && h[t] != x) {
        t = (t + 1) % N;
    }
    return t;
}

int insert(int x) {
    h[find(x)] = x;   
}

int main()
{
    memset(h, 0x3f, sizeof h);
    cin >> n;
    while (n -- ) {
        char op;  
        int x;
        cin >> op >> x;
        if (op == 'I') h[find(x)] = x;
        else {
            if (h[find(x)] != null) puts("Yes");
            else puts("No");
        }
    }
    return 0;
}
```