---
{"dg-publish":true,"permalink":"/02-pages/二分查找法-java/","tags":["personal/blog","algorithm/bineary-search"]}
---


# 概述
二分查找是一种思想，不是一种特定的算法。所以不要局限于在一个有序的数组中查找，二分查找的本质是将一个数组一分为 2，利用“排他性”（另外一半决定没有，或者不影响我查找的行为）来缩小查找范围，从而搜寻数据。
所以，我们可以利用二分查找的思想来查找一个**无序数组的局部最小值**（这个问题将在文章末尾给出解答）等其他问题。
# 经典实现
## 非递归方式
```java
public static int binearSearch(int[] arr, int Key) {  
    if (arr.length < 2 ||  //数据校验，排除不合理输入
            Key > arr[arr.length - 1] || Key < arr[0]) {  
        return -1;  
    }  
    int L = 0;  
    int R = arr.length - 1;  
    int M = -1;  
    while (L <= R) {  
        M = L + ((R - L) >> 1);  
        if (arr[M] == Key) {  
            return M;  
        } else if (arr[M] < Key) {  
            L = M + 1;  
        } else {  
            R = M - 1;  
        }  
    }  
    return -1;  
}
```
[[Drawing 2022-07-14 12.54.18.excalidraw.png\|Drawing 2022-07-14 12.54.18.excalidraw.png]] 流程演示
### Tip：
**为什么用** `L + ((R - L) >> 1)` 而不是 `(L + R) / 2` 代表中值？
1. 位运算更偏向于硬件层面，效率更高；
2. L+R 可能会溢出，而 R-L 则更不可能溢出；
## 递归方式
```java
public static int binearSearch_recursion(int[] arr, int L, int R, int Key) {  
    if (L > R || arr.length < 2 ||  
            Key > arr[R] || Key < arr[L]) {  
        return -1;  
    }  
    int M = L + ((R - L) >> 1);  
    int index = -1;  
    if (arr[M] == Key) {  
        return M;  
    } else if (arr[M] < Key) {  
        index = binearSearch_recursion(arr, M + 1, R, Key);  
    } else {  
        index = binearSearch_recursion(arr, L, M - 1, Key);  
    }  
    return index;  
}
```
**注意：** 这里 index 的作用是方便保存递归返回的下标的，为了使找到的下标顺利回到主栈。
## 总结
相信代码并不难看懂，无论是递归版的还是非递归版的，本质都是将数据一分为二，舍弃一部分数据。
# 二分查找变式
既然都是排除一些无用的数值，那么有没有其他“更高效”的排除方式呢？显然一种思路就是改二分法为其他的“分法”。
但不管是什么分法，其都是表现在 mid （中值）上的——以 mid 为分界，一边为有用数据，一边为无用数据。
## 插值查找法
下图是 mid 的修改逻辑：
![[Pasted image 20220714131906.png\|Pasted image 20220714131906.png]]
具体的数学原理涉及到*数值分析*的内容，所以这里就不加赘述。
右式大概表示了 key 值在数组中的大致位置，这使得一开始 mid 就与 key 比较接近，从而所需要比较的次数就少了。但这也是不稳定的，当这个数组越是接近等差数列，mid 就与 key 的实际位置越接近。**所以，当数组越接近于线性，插值查找法的效率越高。**
```java
public static int binearSearch_insert(int[] arr, int Key) {  
    if (arr.length < 2 ||  
            Key > arr[arr.length - 1] || Key < arr[0]) {  
        return -1;  
    }  
    int L = 0;  
    int R = arr.length - 1;  
    int M = -1;  
    while (L < R) {  
        M = L + (R - L) *(Key-arr[L])/(arr[R]-arr[L]);//插值法核心  
        if (arr[M] == Key) {  
            return M;  
        } else if (arr[M] < Key) {  
            L = M + 1;  
        } else {  
            R = M - 1;  
        }  
    }  
    return -1;  
}
```
**疑问：** 为什么这里的 while 循环中不是 L<=R，而是 L<R ？
## 斐波那契查找法
同理，斐波那契查找法只是将“二分”变为“黄金分割”。
```java
public static final int MAXSIZE = 20;
public static int fibonacci_Search(int[] arr, int Key) {  
    int max = arr.length - 1;  
    int[] fib = fib();  
    int k = 0;  
    while (max > fib[k] - 1) {  //获取合适大小的斐波那契数列
        k++;  
    }  
    int[] temp = Arrays.copyOf(arr, fib[k]);  //使arr的长度等于某一斐波那契数列的元素
    for (int i = arr.length; i < temp.length; i++) {  //将多余的位用最大值填满，以保证 
        temp[i] = arr[max];                           //有序
    }  
    int L = 0;  
    int R = max;  
    int M = -1;  
    while (L < R) {  
        M = L + fib[k - 1] - 1;  
        if (arr[M] == Key) {  
            return M;  
        } else if (arr[M] < Key) {  
            L = M + 1;  
            k -= 2;  
        } else {  
            R = M - 1;  
            k -= 1;  
        }  
    }  
    return L;  
}
public static int[] fib() {    //生成斐波那契数列
    int[] f = new int[MAXSIZE];  
    f[0] = 1;  
    f[1] = 1;  
    for (int i = 2; i < f.length; i++) {  
        f[i] = f[i - 1] + f[i - 2];  
    }  
    return f;  
}
```
**解读：** 上述代码中的斐波那契数组的作用是为了每次都让 mid 取到合适的值。比如说，第一次分割就让其取到 temp 数组长度的黄金分割比（0.618）的数据。第二次则从这 0.618 或 0.382 中继续分割 0.618 。
# 局部最值问题
最后，我们来完成开头提到的局部最值问题：
假设有一串**无序数组**，里面的**数据相邻各不相同**，求其中的一个局部最小值（局部最小值指这个值比其周围的数据（i-1 与 i+1）都要小）。
以下是代码实现：
```java
public static int min_search(int[] arr) {  
    if (arr[arr.length - 1] < arr[arr.length - 2]) {  //第一部分
        return arr.length - 1;  
    }  
    if (arr[0] < arr[1]) {  
        return 0;  
    }  
    int L = 1;  
    int R = arr.length - 2;  
    int M = -1;  
    int index = -1;  
    while (L < R) {  //第二部分
        M = L + ((R - L) >> 1);  
        if (arr[M - 1] < arr[M]) {  
            R = M - 1;  
        } else if (arr[M] > arr[M + 1]) {  
            L = M + 1;  
        } else {  
            return M;  
        }  
    }  
    return L;  
}
``` 
我们可以看到除了 while 循环内的 if 的判断条件略有不同，其余的部分几乎与二分查找一模一样。
1. 第一部分代码是为了判断左右端点是不是局部最小值，如果不是再进行进一步判断；
2. 当左右端点不是局部最小值时，那么就走向中点，判断中点满不满足条件；
3. 如果中点不满足且中点比其左邻接的元素大，那么就意味着左侧必定有一个局部最小值（同理，右边也有），于是**右边有没有局部最小值于我们而言是没有意义的**，于是就可以舍弃右边的数据，继续在左侧查找；
4. 重复取中点，直到找到局部最小值为止；
