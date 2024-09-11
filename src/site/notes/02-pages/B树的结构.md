---
{"dg-publish":true,"permalink":"/02-pages/B树的结构/","tags":["personal/blog","algorithm/data-structures/有序表","algorithm/data-structures/有序表/平衡树/B树"]}
---

B 树就是多叉有序树。故而 B 树的定义为：
```ad-note
title: Def
1. 树中的每个节点至多有 m 个孩子节点（最多有 m - 1 个关键字）；
2. 每个节点的平衡因子都为 0；
3. 每个节点的结构为：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/05/20240510215703.png)
	其中 p 表示指针，每个非叶子节点的指针都对应一颗子树，k 表示关键字；
4. 每个节点中的关键字都是有序的；
5. 非根节点至少有 $\displaystyle \left\lceil  \frac{m}{2}  \right\rceil-1$ 个 关键字；
6. 根节点的关键字最少有 1 个；
```


一颗合法的 B 树可以是（这里省略的指针域）：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/05/20240510220043.png)

可以注意到，由于非根非叶节点至少有 $\displaystyle \left\lfloor  \frac{m}{2}  \right\rfloor-1$ 个关键字，所以至少有 2 个子树。