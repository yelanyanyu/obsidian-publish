---
{"dg-publish":true,"permalink":"/02-pages/二分查找-c  /","tags":["personal/blog","algorithm/bineary-search"]}
---


# 整数二分
## 模板
```c++
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

## 思想
二分就是划分成两个满足性质的区域，比如在查找值为 x 的任务中，我们可以将性质规定为 `a[mid] >= x` 或者 `a[mid] <= x`。
所以，二分查找一定不会无解，但是这个解不一定是答案 x，而是性质区间的边界。
我们可以通过二分的边界是否等于 x 来判断这个题目是否有解。

我们要时时刻刻的保证答案在区间（l，r）内部。

我们要记住什么情况下应该 `int mid = l + r + 1 >> 1;`，什么时候 `int mid = l + r >> 1;`。

为什么要+1？带入实例 `l = 3, r = 4` 就可以发现这种情况会发生死循环。

对于 mid + 1 与否我觉得是为了让区间平分。
mid = left + right >> 1; 这里 mid 是上中位数。
mid = left + right + 1 >> 1; 这里 mid 是下中位数。
如果取 left = mid, 即 `[mid, right]`, 则 mid 取下中位数才能平分区间。
如果取 right = mid, 即 `[left, mid]`, 则 mid 取上中位数才能平分区间。
以上讨论是基于区间总长度是偶数的情况下的，长度为奇数时无法平分区间。

## 例题
[789. 数的范围 - AcWing题库](https://www.acwing.com/problem/content/791/)


# 浮点数二分
由于浮点数我们每一次求 mid，都可以保证均分 l 到 r 这个区间，所以省去了很多边界条件，于是代码就变得简单了一点。

当我们找到了一个很小的区间时，就可以认为找到了答案。
## 模板
```c++
bool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps)
    {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```

## 例题
[790. 数的三次方根 - AcWing题库](https://www.acwing.com/problem/content/description/792/)
