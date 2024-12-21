---
{"dg-publish":true,"permalink":"/02-pages/n 阶矩阵相似对角化的充要条件/","tags":["personal/blog"]}
---

设有 n 阶矩阵 A，那么若 A 的每个特征值中，[[线性无关\|线性无关]]的特征向量的个数恰好等于该特征值的重数，则矩阵 A 必然可以相似对角化。例如，

```ad-example
1. 三阶矩阵 A 可相似对角化 $\displaystyle \Longleftrightarrow$ A 有三个[[线性无关]]的[[02-pages/特征向量\|特征向量]]。

```

即有：
$$
P = [\alpha_{1},\alpha_{2},\alpha_{3}],P^{-1}AP = \begin{bmatrix}
\lambda_{1} \\
 & \lambda_{2} \\
&&\lambda_{3}
\end{bmatrix}\implies A = P \begin{bmatrix}
\lambda_{1} \\
& \lambda_{2} \\
&&\lambda_{3}
\end{bmatrix}P^{-1}
$$
我们可以通过特征值，反解出 A 矩阵。

若三阶矩阵 A 有特征值 2,2,4，那么有其对应的特征向量也应该有三个，并且特征值 2 对应两个线性无关的[[特征向量]]，从而根据[[02-pages/特征值\|特征值]]的定义可知，$\displaystyle Ax=2x$ 有两个解，即 $\displaystyle (A-2E)x=0$ 有两个线性无关的解。于是就要求该方程[[02-pages/齐次方程组的通解与基础解系\|基础解系]]解向量的个数为 2，而个数 = A 的阶数 - $\displaystyle r(A-2E)$。

[[02-pages/矩阵的相似\|矩阵的相似]]。
[[02-pages/对角矩阵\|对角矩阵]]。
[[02-pages/矩阵的秩\|矩阵的秩]]。